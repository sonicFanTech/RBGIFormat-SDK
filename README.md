# MozAluz .RBGI Format SDK

**MozAluz .RBGI Format SDK** is the official open-source format library / SDK for working with `.RBGI` files.

`.RBGI` stands for **Render Background Image**. It is a simple image format used by the MozAluz RBGI Tools project for render backgrounds, preview backgrounds, skybox-style images, and other image-based rendering resources.

This repository contains the source files for the **.RBGI Format DLL / SDK only**.

The official **MozAluz RBGI Viewer** and **MozAluz RBGI Editor** are closed-source applications and are not included in this repository.

---

## What is `.RBGI`?

`.RBGI` means:

```txt
Render Background Image
```

The format was originally created for the MozAluz RBGI Tools suite as a custom image type for render backgrounds.

In the current public version, `.RBGI` is designed to be simple, portable, and easy to support. Version 1 of the format stores image data using PNG-compatible image encoding while using the `.RBGI` extension to identify the file as a MozAluz render background image.

That means `.RBGI` files are intentionally easy to load, save, convert, and preview.

Future versions of the format may add extra metadata, render mode settings, background environment information, cube-map data, compression options, or other MozAluz-specific features.

---

## Why is only the Format SDK open-source?

The `.RBGI` format library is open-source so other developers can:

- add `.RBGI` support to their own programs
- inspect how `.RBGI` files are loaded and saved
- build compatible tools
- improve format support
- make converters, previewers, plugins, or command-line tools
- use `.RBGI` files in other rendering projects

The official MozAluz RBGI Viewer and MozAluz RBGI Editor are closed-source official tools. They are separate from this SDK.

This allows the format itself to stay open and developer-friendly while keeping the official Viewer and Editor under the MozAluz project.

---

## Repository Contents

The exact layout may change over time, but this SDK is intended to include files such as:

```txt
/RBGIFormatSDK
    /include
        RBGIFormat.h
    /src
        RBGIFormat.cpp
    /docs
        RBGI_FORMAT.md
    /examples
        ReadRBGIExample.cpp
        WriteRBGIExample.cpp
    LICENSE
    README.md
```

Recommended file purposes:

| File / Folder | Purpose |
| --- | --- |
| `include/` | Public headers for using the SDK |
| `src/` | Source files for the format DLL / static library |
| `docs/` | Extra format documentation |
| `examples/` | Example programs showing how to use the SDK |
| `LICENSE` | License for the SDK |
| `README.md` | Main documentation for the GitHub page |

---

## Features

The SDK is intended to provide:

- `.RBGI` file loading
- `.RBGI` file saving
- PNG-compatible image data support
- simple C/C++ API
- optional DLL export support
- easy integration into other software
- basic validation for `.RBGI` files
- future support for `.RBGI` metadata
- future support for render-specific format extensions

---

## Current Format Version

Current public format version:

```txt
RBGI Format Version: 1
```

Version 1 behavior:

```txt
.RBGI files use PNG-compatible encoded image data with the .RBGI extension.
```

This keeps the format simple and easy to support while still allowing MozAluz tools to recognize `.RBGI` as a special render-background image type.

---

## Planned Format Ideas

Possible future additions:

- embedded `.RBGI` metadata
- render mode hints
- background type information
- cube-map image support
- 3D preview settings
- image author / project metadata
- version tags
- thumbnail data
- optional compression settings
- editor history information
- color-space metadata
- skybox/environment settings

These features are not guaranteed yet, but the SDK is designed so the format can grow over time.

---

## Official Tools

The official MozAluz applications are:

```txt
MozAluz RBGI Viewer
MozAluz RBGI Editor
```

These are the official programs for opening, viewing, previewing, and editing `.RBGI` files.

The Viewer and Editor are closed-source and are not part of this repository.

This repository is only for the `.RBGI` Format SDK / DLL source files.

---

## Example Usage

The actual API may change depending on the version of the SDK, but a basic use case may look like this:

```cpp
#include "RBGIFormat.h"

int main()
{
    RBGI_Image image;

    if (!RBGI_Load("background.rbgi", &image))
    {
        return 1;
    }

    // Use image data here.

    RBGI_Free(&image);
    return 0;
}
```

Saving an image may look like this:

```cpp
#include "RBGIFormat.h"

int main()
{
    RBGI_Image image = {};

    image.width = 256;
    image.height = 256;
    image.channels = 4;
    image.data = /* your RGBA image data */ nullptr;

    if (!RBGI_Save("new_background.rbgi", &image))
    {
        return 1;
    }

    return 0;
}
```

These examples are only general examples. Check the actual SDK headers for the current API.

---

## Building from Source

### Requirements

Recommended build tools:

- Windows 10 or newer
- Visual Studio 2022 or newer
- C++17 or newer
- A C++ compiler such as MSVC
- Optional: CMake, if the project later adds CMake support

The SDK is intended to stay lightweight and should not require the full MozAluz Viewer or Editor source code.

---

### Build with Visual Studio

1. Clone or download this repository.
2. Open the Visual Studio solution file, if one is included.
3. Select the build configuration:
   - `Debug x64`
   - `Release x64`
4. Build the project.
5. The output should produce one or more of the following:
   - `RBGIFormat.dll`
   - `RBGIFormat.lib`
   - `RBGIFormat.pdb`
   - static library output, if supported

Example output layout:

```txt
/bin
    RBGIFormat.dll
    RBGIFormat.lib
```

---

### Build as a DLL

When building as a DLL, the public API should use export/import macros similar to:

```cpp
#ifdef RBGI_FORMAT_EXPORTS
#define RBGI_API __declspec(dllexport)
#else
#define RBGI_API __declspec(dllimport)
#endif
```

This allows the same header to be used when building the DLL and when using the DLL from another project.

---

### Build as a Static Library

If static library support is added, the SDK may also be built as:

```txt
RBGIFormat.lib
```

Static builds are useful when you want to include `.RBGI` support directly inside your program without shipping a separate DLL.

---

## Integrating the SDK into Your Project

To use the SDK in another C++ project:

1. Add the SDK `include/` folder to your compiler include directories.
2. Link against `RBGIFormat.lib`, if using the DLL/import library.
3. Place `RBGIFormat.dll` next to your program executable.
4. Include the SDK header:

```cpp
#include "RBGIFormat.h"
```

5. Use the API to load or save `.RBGI` files.

Example program layout:

```txt
MyProgram.exe
RBGIFormat.dll
```

---

## File Extension

The official file extension is:

```txt
.rbgi
```

Recommended display name:

```txt
Render Background Image
```

Recommended MIME-style name, if needed:

```txt
image/x-rbgi
```

---

## Format Compatibility

Current `.RBGI` files are intended to be compatible with the MozAluz RBGI Tools project.

Because version 1 uses PNG-compatible image data, developers may be able to inspect or recover image data using PNG-compatible tools. However, programs should not assume that all future `.RBGI` versions will behave exactly like normal PNG files.

For best compatibility, use the official SDK when possible.

---

## Recommended Use Cases

`.RBGI` files can be used for:

- render backgrounds
- skybox-style preview backgrounds
- OpenGL preview backgrounds
- custom editor backgrounds
- project thumbnails
- game tool resources
- Source BSP Explorer render backgrounds
- MozAluz Viewer and Editor files
- other image-based tool resources

---

## What This SDK Is Not

This SDK is not:

- the full MozAluz RBGI Viewer source code
- the full MozAluz RBGI Editor source code
- a full paint program
- a full image editor
- a replacement for PNG
- a general-purpose image standard

It is a small format SDK for supporting `.RBGI` files.

---

## Versioning

The SDK and format may use separate version numbers.

Example:

```txt
SDK Version: 1.0.0
RBGI Format Version: 1
```

The SDK version refers to the code release.

The format version refers to the file format itself.

This allows the SDK to receive updates without always changing the actual file format.

---

## Recommended Version Rules

Patch updates:

```txt
1.0.1
1.0.2
1.0.3
```

Use patch updates for:

- bug fixes
- build fixes
- small API fixes
- documentation updates

Minor updates:

```txt
1.1.0
1.2.0
```

Use minor updates for:

- new helper functions
- new examples
- better validation
- optional metadata helpers

Major updates:

```txt
2.0.0
```

Use major updates for:

- breaking API changes
- major format changes
- incompatible file structure changes

---

## Suggested API Goals

The public SDK API should try to stay:

- simple
- stable
- easy to call from C or C++
- safe with memory allocation
- easy to document
- easy to use from a DLL
- future-proof for metadata support

Possible API functions:

```cpp
RBGI_Load(...)
RBGI_Save(...)
RBGI_Free(...)
RBGI_GetVersion(...)
RBGI_IsValid(...)
RBGI_GetLastError(...)
```

Optional future functions:

```cpp
RBGI_ReadMetadata(...)
RBGI_WriteMetadata(...)
RBGI_GetWidth(...)
RBGI_GetHeight(...)
RBGI_GetChannels(...)
RBGI_ConvertToPNG(...)
RBGI_ConvertFromPNG(...)
```

---

## Error Handling

The SDK should return clear errors when possible.

Possible error cases:

- file not found
- invalid `.RBGI` file
- unsupported format version
- unsupported color format
- failed memory allocation
- failed read operation
- failed write operation
- corrupted image data

Recommended behavior:

```cpp
if (!RBGI_Load("file.rbgi", &image))
{
    const char* error = RBGI_GetLastError();
}
```

---

## Memory Management

If the SDK allocates image memory, the same SDK should free it.

Recommended rule:

```txt
If RBGI_Load allocates it, RBGI_Free should free it.
```

This helps avoid bugs caused by freeing memory across different runtimes or DLL boundaries.

---

## Naming Rules

Recommended names:

```txt
RBGI
RBGIFormat
RBGIFormatSDK
MozAluz RBGI Format SDK
```

Recommended DLL name:

```txt
RBGIFormat.dll
```

Recommended header name:

```txt
RBGIFormat.h
```

Recommended namespace, if using C++:

```cpp
namespace RBGI
{
}
```

Recommended C-style prefix:

```txt
RBGI_
```

---

## Legal Notice

`.RBGI` and MozAluz RBGI Tools are part of the MozAluz project by sonic Fan Tech.

This SDK is provided so developers can work with `.RBGI` files and add support for the format in their own software.

The official MozAluz RBGI Viewer and MozAluz RBGI Editor are closed-source applications. This repository does not grant permission to redistribute modified versions of those applications.

---

## License

This SDK should include a license file.

Recommended license:

```txt
MIT License
```

The MIT License is recommended because it allows developers to freely use the SDK in personal, open-source, or commercial projects as long as the license notice is kept.

If the repository does not currently include a license, add one before public use.

---

## Contributing

Contributions are welcome for the open-source SDK.

Good contribution areas:

- bug fixes
- documentation improvements
- build fixes
- example programs
- validation improvements
- cross-platform improvements
- safer memory handling
- metadata support
- tests

Please keep contributions focused on the `.RBGI` Format SDK only.

Do not submit code for the closed-source Viewer or Editor.

---

## Possible Future Tools

The SDK may later include small open-source helper tools such as:

```txt
rbgi-info.exe
rbgi-to-png.exe
png-to-rbgi.exe
rbgi-validator.exe
```

These tools would be separate from the official closed-source MozAluz RBGI Viewer and Editor.

---

## FAQ

### Is `.RBGI` just PNG?

In version 1, `.RBGI` uses PNG-compatible image data with a custom `.RBGI` extension.

The reason for the custom extension is to identify the file as a Render Background Image used by MozAluz tools and future render-background workflows.

Future versions may add metadata or render-specific features.

---

### Can I use `.RBGI` in my own program?

Yes. That is the purpose of this SDK.

You can use the open-source format SDK to add `.RBGI` loading and saving support to your own programs.

---

### Are the Viewer and Editor open-source?

No.

The official MozAluz RBGI Viewer and MozAluz RBGI Editor are closed-source and are not included in this repository.

Only the `.RBGI` Format SDK / DLL source files are open-source.

---

### Can I make my own `.RBGI` viewer?

Yes.

You can use this SDK to make your own `.RBGI` viewer, converter, plugin, or tool.

However, do not claim that your program is the official MozAluz RBGI Viewer unless it is actually made and released by the MozAluz project.

---

### Can I make my own `.RBGI` editor?

Yes.

You can use the SDK to make your own editor or tool that supports `.RBGI`.

The official MozAluz RBGI Editor is still closed-source and separate from this SDK.

---

### Can I use this in a commercial program?

That depends on the license included with this repository.

If the project uses the MIT License, then yes, commercial use is allowed as long as the license terms are followed.

---

## Credits

Created by:

```txt
sonic Fan Tech
```

Project:

```txt
MozAluz RBGI Tools
```

Format:

```txt
.RBGI - Render Background Image
```

---

## Final Note

The goal of this repository is to keep the `.RBGI` format open, documented, and easy to support while keeping the official MozAluz RBGI Viewer and Editor as closed-source official applications.

If you want to add `.RBGI` support to your own software, this SDK is the recommended starting point.
