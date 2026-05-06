# .RBGI Format SDK

**.RBGI**, also known as **Render Background Image**, is a custom image format created for render background images, skybox-style backgrounds, and other project-specific background image use cases.

This repository contains the **open-source .RBGI Format SDK / Format DLL source files**.  
The goal of this SDK is to make the `.RBGI` format documented, usable, and easy to support in other programs.

> **Important:**  
> This repository is only for the `.RBGI` format source files, SDK code, headers, examples, and documentation.  
> **MozAluz RBGI Viewer** and **MozAluz RBGI Editor** are official closed-source applications and are not part of this open-source SDK.

---

## What is `.RBGI`?

`.RBGI` stands for:

```txt
Render Background Image
```

The format was originally created for **Source BSP Explorer**, also known as **SBSPE**.

Source BSP Explorer needed a custom background/skybox image system for its renderer. Instead of using regular image formats like `.PNG`, `.JPG`, or `.JPEG` directly for that purpose, `.RBGI` was created as a dedicated format for render background images.

In simple terms:

```txt
.RBGI = a custom render-background image file format
```

It is designed for tools and engines that need a special image type for things like:

- custom render backgrounds
- skybox-style background images
- editor background images
- viewport background textures
- project-specific image assets
- metadata-supported background files

---

## Why was `.RBGI` created?

The `.RBGI` format was created because Source BSP Explorer needed a dedicated background image format.

The original reason was:

- Source BSP Explorer needed a custom background skybox image.
- The background image was meant to be part of SBSPE's own asset/tool system.
- Using `.PNG`, `.JPEG`, or another normal image format directly did not feel right for this specific render-background use case.
- A custom extension made the file's purpose clear.
- Metadata support could be added for future render/background settings.

So instead of having something like:

```txt
Render_BG.png
Render_BG.jpg
Render_BG.jpeg
```

Source BSP Explorer could use:

```txt
Render_BG.RBGI
```

That makes the file immediately recognizable as a **Render Background Image** used by the tool.

---

## Format concept

`.RBGI` files are image files made for render-background usage.

The image data itself is similar to PNG-style image data, but `.RBGI` files are intended to also support metadata for extra information.

A `.RBGI` file can contain:

- image data
- format/version information
- optional metadata
- tool-specific information
- render/background-related settings
- future extension data

The exact structure may change as the SDK develops, but the main idea is:

```txt
Image data + RBGI metadata + render-background purpose
```

---

## Current format notes

In the current format design, `.RBGI` files are very close to PNG-style image files in how the actual image data is stored.

That means:

- the image data can be treated similarly to PNG image data
- `.RBGI` keeps its own extension and purpose
- metadata can be used to store extra information
- future versions can expand the format without changing the basic purpose

This makes `.RBGI` simple enough to support, while still allowing it to grow into a more complete render-background format later.

---

## Official tools

The official tools for working with `.RBGI` files are:

```txt
MozAluz RBGI Viewer
MozAluz RBGI Editor
```

These tools are used to:

- view `.RBGI` files
- create `.RBGI` files
- edit `.RBGI` files
- preview `.RBGI` backgrounds
- manage `.RBGI` metadata

However, these applications are **not open-source**.

This SDK exists so the format itself can still be public and usable by other developers without making the full Viewer or Editor source code public.

---

## What is open-source?

This repository may include:

```txt
.RBGI Format DLL source files
.RBGI SDK headers
.RBGI format documentation
example code
test files
metadata documentation
basic read/write helpers
```

The open-source part is the format support layer.

That means other developers can use this repository to:

- load `.RBGI` files
- save `.RBGI` files
- validate `.RBGI` files
- read image data from `.RBGI` files
- write image data to `.RBGI` files
- read or write supported metadata
- add `.RBGI` support to their own tools

---

## What is not open-source?

The following projects are **closed-source official applications**:

```txt
MozAluz RBGI Viewer
MozAluz RBGI Editor
```

These are not included in this SDK.

The closed-source applications may use the open-source Format DLL / SDK, but their full source code is not part of this repository.

---

## Recommended repository layout

A good layout for this SDK is:

```txt
RBGI-Format-SDK/
│
├─ README.md
├─ LICENSE
├─ CHANGELOG.md
│
├─ docs/
│  ├─ RBGI_FORMAT.md
│  ├─ METADATA.md
│  └─ VERSIONING.md
│
├─ include/
│  └─ RBGIFormat.h
│
├─ src/
│  └─ RBGIFormat.cpp
│
├─ examples/
│  ├─ ReadRBGI/
│  ├─ WriteRBGI/
│  └─ MetadataExample/
│
├─ tests/
│  └─ test_files/
│
└─ samples/
   └─ Render_BG.RBGI
```

Not every folder is required immediately, but this structure keeps the SDK organized as it grows.

---

## Suggested SDK goals

The `.RBGI Format SDK` should aim to provide:

- a clean public C/C++ API
- simple read/write functions
- file validation
- metadata support
- version checking
- safe error handling
- example projects
- clear documentation
- compatibility notes for future `.RBGI` versions

---

## Possible SDK features

Depending on how far the SDK is developed, it may support:

### File loading

Load an existing `.RBGI` file from disk.

```txt
Input:  file path
Output: decoded image data + metadata
```

### File saving

Save image data into a `.RBGI` file.

```txt
Input:  width, height, pixel/image data, metadata
Output: .RBGI file
```

### Validation

Check whether a file is a valid `.RBGI` file.

Possible validation checks:

- file exists
- file extension is `.RBGI`
- image data can be read
- metadata block is valid
- format version is supported
- file is not empty or corrupted

### Metadata reading

Read metadata from a `.RBGI` file.

Possible metadata fields:

```txt
Title
Author
Description
CreatedWith
CreatedDate
FormatVersion
SourceProject
RenderMode
BackgroundType
Notes
```

### Metadata writing

Write or update metadata in a `.RBGI` file.

### Format versioning

Allow `.RBGI` files to store a format version, such as:

```txt
RBGI_FORMAT_VERSION=1
```

This makes it easier to support older and newer versions later.

---

## Suggested metadata fields

The SDK can support metadata such as:

```ini
[RBGI]
FormatVersion=1
FileType=Render Background Image
CreatedWith=MozAluz RBGI Editor
SourceProject=Source BSP Explorer

[Image]
Width=256
Height=256
ColorDepth=32
HasAlpha=true

[Render]
BackgroundType=Skybox
RenderMode=Flat
UseAsViewportBackground=true

[Info]
Title=Render_BG
Author=sonic Fan Tech
Description=Default render background image for Source BSP Explorer.
```

This is only an example. The real metadata structure may be different depending on how the SDK stores metadata internally.

---

## Example use cases

`.RBGI` can be used for:

- Source BSP Explorer background images
- custom render viewport backgrounds
- map editor backgrounds
- skybox preview textures
- game development tools
- level editor visual assets
- launcher or tool background images
- custom image pipelines
- asset formats for closed or open-source tools

---

## Example file names

Common `.RBGI` file names could include:

```txt
Render_BG.RBGI
DefaultBackground.RBGI
SkyboxPreview.RBGI
ViewportBackground.RBGI
EditorBackground.RBGI
```

---

## Example usage

The actual API may change depending on how the SDK is structured, but a simple C++ usage style could look like this:

```cpp
#include "RBGIFormat.h"

int main()
{
    RBGI_Image image;

    if (!RBGI_LoadFromFile("Render_BG.RBGI", &image))
    {
        // Failed to load the file
        return 1;
    }

    // Use image data here

    RBGI_FreeImage(&image);
    return 0;
}
```

A save example could look like this:

```cpp
#include "RBGIFormat.h"

int main()
{
    RBGI_Image image = {};

    image.width = 256;
    image.height = 256;
    image.channels = 4;
    image.data = /* image pixel data */;

    RBGI_Metadata metadata = {};
    metadata.title = "Render_BG";
    metadata.author = "sonic Fan Tech";
    metadata.description = "Default Source BSP Explorer render background image.";

    if (!RBGI_SaveToFile("Render_BG.RBGI", &image, &metadata))
    {
        // Failed to save the file
        return 1;
    }

    return 0;
}
```

These examples are meant to show the intended style of the SDK. The actual function names may differ depending on the current implementation.

---

## Building the SDK

The build process depends on how the SDK project files are included.

### Visual Studio / MSVC

If this repository includes a Visual Studio solution:

```txt
1. Open the .sln file in Visual Studio.
2. Select Debug or Release.
3. Select x64.
4. Build the solution.
5. The output DLL/static library should appear in the build output folder.
```

Recommended output names:

```txt
RBGIFormat.dll
RBGIFormat.lib
RBGIFormat.pdb
```

### Manual C++ build

If the SDK is simple and only has a few source files, it can also be compiled manually.

Example:

```bat
cl /EHsc /LD src\RBGIFormat.cpp /Iinclude /Fe:RBGIFormat.dll
```

This command is only an example and may need to be changed depending on dependencies and source file names.

---

## Using the DLL in another project

A program that wants to use the `.RBGI Format DLL` would usually need:

```txt
RBGIFormat.dll
RBGIFormat.lib
RBGIFormat.h
```

Typical setup:

```txt
1. Add include/ to your compiler include paths.
2. Link against RBGIFormat.lib.
3. Place RBGIFormat.dll beside your executable.
4. Include RBGIFormat.h in your project.
5. Call the SDK loading/saving functions.
```

Example folder layout:

```txt
MyProgram/
│
├─ MyProgram.exe
├─ RBGIFormat.dll
│
└─ assets/
   └─ Render_BG.RBGI
```

---

## Compatibility

The SDK should aim to support:

```txt
Windows 10
Windows 11
Visual Studio 2022 or newer
x64 builds
```

Other platforms may work if the code avoids Windows-only APIs, but official support depends on the SDK implementation.

---

## Versioning

The `.RBGI` format should use version numbers so future changes do not break older files.

Recommended version naming:

```txt
RBGI Format v1
RBGI Format v1.1
RBGI Format v2
```

Recommended SDK version naming:

```txt
RBGI Format SDK v1.0.0
RBGI Format SDK v1.1.0
RBGI Format SDK v2.0.0
```

A good rule:

- patch version changes fix bugs
- minor version changes add compatible features
- major version changes may change the format or API

Example:

```txt
1.0.0 = first public SDK release
1.0.1 = bug fix
1.1.0 = new metadata field support
2.0.0 = major format/API change
```

---

## Suggested file extension rules

The standard extension should be:

```txt
.RBGI
```

Recommended behavior:

- treat `.RBGI` and `.rbgi` as the same extension
- save using uppercase `.RBGI` for official files
- do not overwrite files without confirmation in GUI tools
- validate the file contents, not only the extension

---

## Error handling

The SDK should avoid crashing when a file is invalid.

Recommended error cases:

```txt
RBGI_OK
RBGI_ERROR_FILE_NOT_FOUND
RBGI_ERROR_INVALID_FILE
RBGI_ERROR_UNSUPPORTED_VERSION
RBGI_ERROR_READ_FAILED
RBGI_ERROR_WRITE_FAILED
RBGI_ERROR_INVALID_METADATA
RBGI_ERROR_OUT_OF_MEMORY
```

A good SDK should return useful error codes so apps can show helpful messages.

---

## Security notes

Programs using the SDK should not blindly trust `.RBGI` files.

Recommended safety checks:

- check file size before loading
- validate width and height
- reject impossible image sizes
- check metadata length
- avoid unsafe memory copying
- handle corrupted files safely
- never execute data from a `.RBGI` file

`.RBGI` files are image/metadata files. They should not contain executable code.

---

## Relationship to Source BSP Explorer

`.RBGI` was originally created for **Source BSP Explorer**.

Its first major purpose was to provide a custom background image format for SBSPE's renderer, especially for background/skybox-style rendering.

Source BSP Explorer may use `.RBGI` files for:

- renderer background images
- default viewport backgrounds
- skybox-style preview images
- tool-specific visual assets

---

## Relationship to MozAluz RBGI Tools

**MozAluz RBGI Tools** are the official applications for `.RBGI` files.

The tool suite includes:

```txt
MozAluz RBGI Viewer
MozAluz RBGI Editor
```

The Viewer is used for viewing `.RBGI` files.  
The Editor is used for creating and editing `.RBGI` files.

The Format SDK is open-source so the format can be supported by other programs, but the Viewer and Editor remain closed-source official tools.

---

## Why open-source only the Format SDK?

Open-sourcing the format layer gives developers the ability to support `.RBGI` without exposing the full official apps.

This has several benefits:

- developers can add `.RBGI` support to their own tools
- the format can be documented publicly
- compatibility is easier to maintain
- bugs in the format code can be found and fixed
- the official Viewer and Editor can remain controlled official apps
- the format can grow beyond one program

This keeps the format open while keeping the official applications protected.

---

## Possible future additions

Possible future features for the `.RBGI` format or SDK:

- stronger metadata support
- preview thumbnails
- compression options
- render mode hints
- cube-map metadata
- skybox face support
- background animation metadata
- HDR or high-bit-depth support
- alpha channel rules
- color profile metadata
- command-line converter
- `.RBGI` validation tool
- sample `.RBGI` test files
- official format specification document

---

## Contributing

Contributions are welcome for the open-source SDK.

Good contributions include:

- bug fixes
- safer file parsing
- better metadata handling
- documentation improvements
- example projects
- format validation improvements
- compatibility improvements
- build fixes

Please keep contributions focused on the `.RBGI` Format SDK.

This repository is not for the source code of MozAluz RBGI Viewer or MozAluz RBGI Editor.

---

## Development rules

Recommended development rules for this SDK:

```txt
1. Keep the format simple.
2. Keep the API easy to use.
3. Do not break existing .RBGI files without a major version change.
4. Validate files safely.
5. Keep metadata optional.
6. Keep the SDK usable outside of MozAluz tools.
7. Document format changes.
```

---

## License

This SDK is intended to be open-source.

A permissive license such as the **MIT License** is recommended if the goal is to let other developers freely use `.RBGI` support in their own programs.

Make sure the repository includes a `LICENSE` file.

Example license choice:

```txt
MIT License
```

The official Viewer and Editor are separate closed-source applications and are not covered by this SDK repository unless stated otherwise.

---

## Credits

`.RBGI` / **Render Background Image** was created by **sonic Fan Tech**.

Originally created for:

```txt
Source BSP Explorer
```

Official tools:

```txt
MozAluz RBGI Viewer
MozAluz RBGI Editor
```

Open-source component:

```txt
.RBGI Format SDK / Format DLL
```

---

## Project status

This SDK is the public/open-source format layer for `.RBGI`.

The format may continue to grow as Source BSP Explorer, MozAluz RBGI Tools, and other related projects improve.

The goal is to keep `.RBGI` easy to understand, easy to support, and useful for render-background images.
