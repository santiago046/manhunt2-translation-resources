While creating:

```
struct FontDat {
    char[6] magic;           // '<FONT>'
    uint32_t charCount;      // Highest character code + 1
    uint32_t minimumChar;    // Always 0
    uint32_t fontIndex;      // 0, 1, or 2
    uint32_t matrixCount;    // Actual number of characters in the font
    uint32_t fontHeightInt;  // Max character height (unused by the game)
    float fontHeight;        // (maxCharHeight / 2) / textureHeight

    uint32_t charMap[charCount];  // Maps character codes to matrix indices

    struct Char {
        uint32_t keyIndex;  // Character code in hex (e.g., 0x61000000 for 'a')
        float width;        // (charWidth / 2) / textureWidth
        float coords[4];    // Normalized texture coordinates:
                            // [0] = x1 = x / textureWidth
                            // [1] = y1 = y / textureHeight
                            // [2] = x2 = (x + charWidth) / textureWidth
                            // [3] = y2 = (y + charHeight) / textureHeight
    } chars[matrixCount];
};
```

## More info:
* https://github.com/Sor3nt/manhunt-toolkit/issues/31
* [manhunt-toolkit/Application/App/Service/Archive/Font.php](https://github.com/Sor3nt/manhunt-toolkit/blob/master/Application/App/Service/Archive/Font.php)
