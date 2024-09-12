# Manhunt 2 GXT Files Overview

The Manhunt 2 GXT file format is almost the same as for Manhunt (2003) and Grand Theft Auto III (2001). The differences are a text display duration field addition to TKEY Entry, and a 12 bytes long TDAT Entry Name instead of 8 for PC and Wii platforms.

```c
#ifdef PLATFORM_PC_WII
    #define TKEY_NAME_LENGTH 12  // For PC and Wii.
#else
    #define TKEY_NAME_LENGTH 8   // For PSP and PS2.
#endif

typedef struct {
    uint32_t tdatOffset;                   // Offset to the corresponding TDAT entry (relative to the start of the TDAT block).
    char tdatEntryName[TKEY_NAME_LENGTH];  // Name of the TDAT entry (8 or 12 bytes depending on platform).
    uint32_t displayDurationMs;            // Time the text is displayed in milliseconds.
} TKEYEntry;

// The entire GXT file structure in Manhunt 2
typedef struct {
    // TKEY block: Contains metadata for accessing the text strings in TDAT
    char tkeyIdentifier[4];  // Should be "TKEY" (FourCC identifier for the TKEY block).
    uint32_t tkeyBlockSize;  // Size of the TKEY block (excluding the FourCC and this field).
    TKEYEntry *tkeyEntries;  // Array of TKEY entries. Number of entries = tkeyBlockSize / sizeof(TKEYEntry).
    
    // TDAT block: Contains the actual text strings
    char tdatIdentifier[4];  // Should be "TDAT" (FourCC identifier for the TDAT block).
    uint32_t tdatBlockSize;  // Size of the TDAT block (excluding the FourCC and this field).
    uint16_t *tdatStrings;   // Array of wide characters (16 bits). Each string is null-terminated (0x0000).
} Manhunt2GXTFile;
```

## Endianness
The format is big-endian for the Wii and little-endian for other platforms (PC, PSP, PS2).

## String Encoding
Manhunt 2 uses a custom charset encoding defined in FONT.DAT file. 
