# Font.dat Structure Overview

This structure is used for packing and unpacking font data within a custom font file. It stores essential information about the font, including character mapping and texture coordinates.

```c
struct FontData {
    char magic[6];            // File identifier, always set to "<FONT>"
    uint32_t charCount;       // Total number of characters in the font
    uint32_t minChar;         // Lowest character code (always 0)
    uint32_t fontID;          // Font identifier:
                              //   0 = t16plus (debug)
                              //   1 = font2 (everything except lowercase menu)
                              //   2 = font1 (lowercase menu)
    uint32_t matrixCount;     // Number of character matrices
    uint32_t origFontHeight;  // Original font height in pixels (not used by the game)
    float fontHeight;         // Normalized font height
                              // To normalize:   fontHeight = (maxCharHeight / 2) / textureHeight
                              // To denormalize: maxCharHeight = fontHeight * textureHeight * 2

    int32_t *charMap;         // Maps character codes to corresponding indices (size: charCount + 1)
                              // Value -1 (0xFFFFFFFF) means the character is not present in the texture

    struct CharMatrix {       // Stores texture coordinates and size for each character
        uint32_t index;       // Character index in the texture atlas
        float width;          // Normalized width of the character
                              // To normalize:   width = (charWidth / 2) / textureWidth
                              // To denormalize: charWidth = width * textureWidth * 2

        // Normalized texture coordinates
        // x1, y1 represent the top-left corner, and x2, y2 represent the bottom-right corner.
        // To normalize:
        //   x = x / textureWidth, y = y / textureHeight
        // To denormalize:
        //   x = x * textureWidth, y = y * textureHeight
        float x1;             // Normalized X coordinate of the left edge
        float y1;             // Normalized Y coordinate of the top edge
        float x2;             // Normalized X coordinate of the right edge
        float y2;             // Normalized Y coordinate of the bottom edge
    } *charMatrices;          // Array of character matrices (size: matrixCount)
} Font[3];
```

## Notes

The format is big-endian for the Wii and little-endian for other platforms (PC, PSP, PS2).

## Additional Resources

For more context and implementation details, refer to the following links:
- https://github.com/Sor3nt/manhunt-toolkit/issues/31
- [Sor3nt/manhunt-toolkit/Application/App/Service/Archive/Font.php](https://github.com/Sor3nt/manhunt-toolkit/blob/master/Application/App/Service/Archive/Font.php)
