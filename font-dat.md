# Font.Dat Structure Overview

This structure is used for packing and unpacking font data within a custom font file. It stores essential information about the font, including character mapping and texture coordinates.

```cpp
struct FontDat {
    char magic[6];           // Magic string identifying the font data, always "<FONT>"
    uint32_t charCount;      // Total number of characters (highest character code + 1)
    uint32_t minimumChar;    // Always 0, used as a base offset
    uint32_t fontIndex;      // Font identifier, can be 0, 1, or 2
    uint32_t matrixCount;    // Number of actual characters used in the font
    uint32_t fontHeightInt;  // Unused by the game, but represents (maxCharHeight / 2) / textureHeight as an integer
    float fontHeight;        // (maxCharHeight / 2) / textureHeight, the normalized height of the font

    uint32_t charMap[charCount];  // Maps character codes to indices within the 'chars' array

    struct Char {
        uint32_t keyIndex;  // Character code in hexadecimal (e.g., 0x61000000 for 'a')
        float width;        // Normalized character width: (charWidth / 2) / textureWidth
        float coords[4];    // Normalized texture coordinates:
                            // coords[0] = x1 = x / textureWidth
                            // coords[1] = y1 = y / textureHeight
                            // coords[2] = x2 = (x + charWidth) / textureWidth
                            // coords[3] = y2 = (y + charHeight) / textureHeight
    } chars[matrixCount];  // Array of characters defined in the font, size determined by matrixCount
};
```

## Notes for Unpacking

When unpacking this data, instead of dividing to normalize values (as shown in the structure), multiply the normalized values by the texture dimensions to retrieve the original values.

## Additional Resources

For more context and implementation details, refer to the following links:
- https://github.com/Sor3nt/manhunt-toolkit/issues/31
- [Sor3nt/manhunt-toolkit/Application/App/Service/Archive/Font.php](https://github.com/Sor3nt/manhunt-toolkit/blob/master/Application/App/Service/Archive/Font.php)
