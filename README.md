# .RBGI Format SDK

**.RBGI** means **Render Background Image**.

This repository contains the open-source source files for the **.RBGI Format SDK / SharedRBGIformat DLL**. It is the public format layer used to load, save, validate, and inspect `.RBGI` files.

The `.RBGI` format was originally created for **Source BSP Explorer**, also known as **SBSPE**. SBSPE needed its own custom render-background / skybox-background image format instead of directly using normal image extensions like `.png`, `.jpg`, or `.jpeg` for its default renderer background files.

> **Important:**  
> This repo is only for the `.RBGI` format SDK / format DLL source files.  
> **MozAluz RBGI Viewer** and **MozAluz RBGI Editor** are official closed-source applications and are not included in this repository.

---

## Project status

Current public SDK status:

```txt
Format version: RBGI v1
SDK language:   C++17
Framework:      Qt 6
Main output:    SharedRBGIformat.dll
Main header:    RbgiFormat.h
Main source:    RbgiFormat.cpp
```

Current `.RBGI` v1 files are not just renamed PNG files. A real v1 `.RBGI` file has its own file header, metadata block, and embedded PNG image data.

The loader also supports older legacy `.RBGI` files that were only PNG files saved with the `.RBGI` extension.

---

## What is `.RBGI`?

`.RBGI` stands for:

```txt
Render Background Image
```

The format is meant for images that are used as render backgrounds, viewport backgrounds, skybox-style images, editor backgrounds, and other tool-specific image assets.

Examples:

```txt
Render_BG.RBGI
DefaultBackground.RBGI
ViewportBackground.RBGI
SkyBoxPreview.RBGI
```

A real `.RBGI` v1 file stores:

```txt
RBGI magic header
format version
header size
image width
image height
image mode
reserved flags
metadata size
embedded PNG size
JSON metadata
PNG image bytes
```

So the short version is:

```txt
.RBGI v1 = RBGI header + JSON metadata + embedded PNG image data
```

---

## Why was `.RBGI` created?

The format was originally created for **Source BSP Explorer**.

Source BSP Explorer needed a custom background image format for its renderer. The purpose was to have a recognizable project-specific file type for background and skybox-style images instead of using normal image files directly.

Instead of using:

```txt
Render_BG.png
Render_BG.jpg
Render_BG.jpeg
```

SBSPE could use:

```txt
Render_BG.RBGI
```

That makes the asset's purpose clearer. The file is not just a random image. It is a **Render Background Image** meant for renderer/editor usage.

---

## Relationship to MozAluz RBGI Tools

**MozAluz RBGI Tools** are the official tools for the format.

The official apps are:

```txt
MozAluz RBGI Viewer
MozAluz RBGI Editor
```

They are used to view, create, and edit `.RBGI` files.

However, the Viewer and Editor are **closed-source official applications**. This repository only contains the open-source format SDK / shared format DLL source files.

This gives developers a way to support `.RBGI` files without requiring the full Viewer or Editor source code.

---

## What is open-source in this repo?

This repository is for:

```txt
RbgiFormat.h
RbgiFormat.cpp
SharedRBGIformat.vcxproj
format documentation
example usage files
```

The open-source part is the SDK / DLL that handles the format itself.

Other developers can use it to:

- detect real `.RBGI` files
- load `.RBGI` files
- save `.RBGI` files
- read `.RBGI` metadata
- write `.RBGI` metadata
- convert images into real `.RBGI` v1 files
- load older legacy renamed-PNG `.RBGI` files
- add `.RBGI` support to their own tools

---

## What is not open-source?

The following official applications are not open-source:

```txt
MozAluz RBGI Viewer
MozAluz RBGI Editor
```

They may use this SDK, but their application source code is not included here.

Do not add Viewer or Editor project files to this repository.

---

## Real `.RBGI` v1 file structure

The current real `.RBGI` format starts with the magic bytes:

```txt
RBGI
```

After that, the file uses a little-endian binary header.

| Offset | Type | Name | Description |
|---:|---|---|---|
| 0 | char[4] | magic | Must be `RBGI` |
| 4 | quint16 | version | Current version is `1` |
| 6 | quint32 | headerSize | Current header size is `38` bytes |
| 10 | quint32 | width | Image width stored in the header |
| 14 | quint32 | height | Image height stored in the header |
| 18 | quint32 | mode | Image mode enum value |
| 22 | quint32 | flags | Reserved for future versions; currently saved as `0` |
| 26 | quint64 | pngSize | Size of the embedded PNG data |
| 34 | quint32 | metadataSize | Size of the JSON metadata block |
| 38 | byte[] | metadata | Compact JSON metadata bytes |
| 38 + metadataSize | byte[] | pngBytes | Embedded PNG image bytes |

The format is intentionally simple:

```txt
[Header][JSON Metadata][PNG Data]
```

The SDK constant for the current header size is:

```cpp
constexpr quint32 kHeaderSize = 38;
```

---

## Image modes

The SDK defines the following image modes:

```cpp
enum class ImageMode : quint32
{
    FlatImage = 0,
    RenderBackground = 1,
    SkyBoxSingleTexture = 2,
    SkyBoxCubeTexture = 3
};
```

Mode meanings:

| Value | Name | Meaning |
|---:|---|---|
| 0 | `FlatImage` | A normal flat image |
| 1 | `RenderBackground` | A render/editor background image |
| 2 | `SkyBoxSingleTexture` | A single texture used as a skybox-style background |
| 3 | `SkyBoxCubeTexture` | A cube-style skybox image mode |

The default mode used by the SDK is usually:

```txt
RenderBackground
```

---

## Metadata

Real `.RBGI` v1 files can store metadata as a JSON object.

The SDK uses Qt's `QJsonObject` for metadata.

Example metadata:

```json
{
  "format": "RBGI",
  "name": "Render_BG",
  "type": "RenderBackgroundImage",
  "createdWith": "MozAluz RBGI Editor",
  "sourceProject": "Source BSP Explorer",
  "description": "Default render background image for SBSPE."
}
```

The SDK saves metadata using compact JSON:

```cpp
QJsonDocument(metadata).toJson(QJsonDocument::Compact)
```

Metadata is optional, but real `.RBGI` v1 files still include a metadata size field in the header.

---

## Legacy renamed-PNG `.RBGI` files

The loader supports two kinds of `.RBGI` files:

```txt
1. Real RBGI v1 files
2. Legacy renamed PNG files
```

A real `.RBGI` v1 file begins with:

```txt
RBGI
```

A legacy `.RBGI` file does not have the `RBGI` header. Instead, it is just PNG image data saved with the `.RBGI` extension.

When loading a file, the SDK does this:

```txt
1. Read all file bytes.
2. Check if the file starts with RBGI.
3. If yes, load it as real RBGI v1.
4. If no, try loading the bytes as PNG data.
5. If the PNG load works, mark it as legacyRenamedPng.
6. If both fail, the file is invalid.
```

Legacy files are still supported so older `.RBGI` files do not become useless.

---

## Public API

The current public API is in:

```txt
RbgiFormat.h
```

Namespace:

```cpp
namespace RBGI
```

### `RBGI::Document`

The main loaded file result is:

```cpp
struct Document
{
    bool valid = false;
    bool legacyRenamedPng = false;
    quint16 version = 1;
    quint32 width = 0;
    quint32 height = 0;
    ImageMode mode = ImageMode::RenderBackground;
    quint32 flags = 0;
    QJsonObject metadata;
    QByteArray pngBytes;
    QImage image;
    QString error;
};
```

Field meanings:

| Field | Meaning |
|---|---|
| `valid` | `true` if the file loaded successfully |
| `legacyRenamedPng` | `true` if the file was an older renamed PNG `.RBGI` |
| `version` | Real `.RBGI` format version |
| `width` | Width from the file/header |
| `height` | Height from the file/header |
| `mode` | Image mode |
| `flags` | Reserved flags field |
| `metadata` | JSON metadata object |
| `pngBytes` | Embedded PNG data |
| `image` | Decoded `QImage`, converted to `QImage::Format_RGBA8888` |
| `error` | Error message if loading failed |

### Functions

```cpp
RBGI_API QString modeToString(ImageMode mode);
RBGI_API ImageMode modeFromIndex(int index);
RBGI_API bool looksLikeRealRbgi(const QByteArray& bytes);

RBGI_API Document loadFile(const QString& filePath);
RBGI_API bool saveFile(
    const QString& filePath,
    const QImage& image,
    ImageMode mode,
    const QJsonObject& metadata,
    QString* errorOut = nullptr
);
RBGI_API bool saveLegacyPngAsRbgi(
    const QString& filePath,
    const QImage& image,
    QString* errorOut = nullptr
);
```

---

## Loading a `.RBGI` file

Example:

```cpp
#include "RbgiFormat.h"

int main()
{
    RBGI::Document doc = RBGI::loadFile("Render_BG.RBGI");

    if (!doc.valid)
    {
        qWarning() << "Failed to load RBGI:" << doc.error;
        return 1;
    }

    qDebug() << "Loaded:" << doc.width << "x" << doc.height;
    qDebug() << "Mode:" << RBGI::modeToString(doc.mode);
    qDebug() << "Legacy renamed PNG:" << doc.legacyRenamedPng;

    return 0;
}
```

---

## Saving a real `.RBGI` v1 file

Example:

```cpp
#include "RbgiFormat.h"

int main()
{
    QImage image("input.png");
    if (image.isNull())
        return 1;

    QJsonObject metadata;
    metadata["format"] = "RBGI";
    metadata["name"] = "Render_BG";
    metadata["type"] = "RenderBackgroundImage";
    metadata["sourceProject"] = "Source BSP Explorer";

    QString error;
    const bool ok = RBGI::saveFile(
        "Render_BG.RBGI",
        image,
        RBGI::ImageMode::RenderBackground,
        metadata,
        &error
    );

    if (!ok)
    {
        qWarning() << "Failed to save RBGI:" << error;
        return 1;
    }

    return 0;
}
```

---

## Saving a legacy renamed-PNG `.RBGI` file

The SDK also has a helper for saving a `.RBGI` file as PNG data with a `.RBGI` extension:

```cpp
RBGI::saveLegacyPngAsRbgi("Legacy.RBGI", image, &error);
```

This should only be used when legacy compatibility is needed.

For new files, use:

```cpp
RBGI::saveFile(...)
```

---

## Safety limits

The loader has basic safety limits:

```txt
Maximum metadata size: 8 MiB
Maximum embedded PNG size: 512 MiB
```

If a file exceeds these limits, loading fails.

The SDK also rejects:

- empty files
- invalid magic headers
- unsupported format versions
- invalid metadata JSON
- invalid embedded PNG data
- files that are neither real `.RBGI` nor legacy PNG data

---

## Dependencies

The current SDK uses **Qt 6**.

Required Qt modules:

```txt
QtCore
QtGui
```

The SDK uses:

```txt
QByteArray
QBuffer
QDataStream
QFile
QImage
QJsonDocument
QJsonObject
QString
```

Recommended build environment:

```txt
Windows 10 or Windows 11
Visual Studio 2022 or newer
MSVC v143 toolset
Qt 6 msvc2022_64 kit
C++17
x64 build
```

The included Visual Studio project is set up for x64 Debug and Release builds.

---

## Building with Visual Studio

This SDK includes a standalone Visual Studio project:

```txt
SharedRBGIformat.vcxproj
```

And this cleaned package also includes a standalone solution:

```txt
RBGIFormatSDK.sln
```

Open the solution or project in Visual Studio, select `Debug x64` or `Release x64`, and build.

Expected output:

```txt
bin\Debug\SharedRBGIformat.dll
bin\Release\SharedRBGIformat.dll
```

The project does **not** reference MozAluz RBGI Viewer or MozAluz RBGI Editor.

---

## Qt path setup

The project tries to use a `QtDir` property.

The default Qt path can be changed in the `.vcxproj` file if needed.

Example Qt path:

```txt
C:\Qt\6.10.2\msvc2022_64
```

If your Qt version is installed somewhere else, update `QtDir` or set it in Visual Studio/MSBuild.

---

## Recommended repository contents

For the public SDK repo, use a clean layout like this:

```txt
RBGI-Format-SDK/
│
├─ README.md
├─ LICENSE
├─ .gitignore
├─ RBGIFormatSDK.sln
├─ SharedRBGIformat.vcxproj
├─ SharedRBGIformat.vcxproj.filters
├─ resource.h
├─ Resource.rc
│
├─ include/
│  └─ RbgiFormat.h
│
├─ src/
│  └─ RbgiFormat.cpp
│
├─ docs/
│  ├─ RBGI_FORMAT.md
│  └─ API.md
│
└─ examples/
   ├─ simple_load.cpp
   └─ simple_save.cpp
```

Avoid uploading user-specific and generated Visual Studio files, such as:

```txt
*.vcxproj.user
*.aps
.vs/
x64/
Debug/
Release/
bin/
obj/
```

---

## Keeping Viewer and Editor closed-source

This SDK is safe to upload publicly because it only contains the format code.

For the public GitHub repository, do not include:

```txt
MozAluz RBGI Viewer.vcxproj
MozAluz RBGI Editor.vcxproj
Viewer source files
Editor source files
Viewer UI files
Editor UI files
private app assets
private app resources
```

The SDK can still mention that the official Viewer and Editor exist, but their source code should not be uploaded.

---

## Versioning

Recommended version rules:

```txt
1.0.0 = first public SDK release
1.0.1 = bug fixes only
1.1.0 = compatible SDK/API additions
2.0.0 = major format/API change
```

The file format version and SDK version do not have to be the same number.

Example:

```txt
SDK version:     1.0.0
Format version:  1
DLL version:     1.0.0.0
```

---

## Future ideas

Possible future improvements:

- stronger validation
- command-line converter
- `.RBGI` to `.PNG` export tool
- `.PNG` to `.RBGI` converter
- metadata editor example
- thumbnail support
- cubemap face metadata
- animated background metadata
- color profile metadata
- API for reading header info without decoding the full image
- optional non-Qt version of the SDK

---

## License

Add a `LICENSE` file before publishing the repo.

A permissive license like **MIT** is a good choice if you want other developers to freely use the `.RBGI` Format SDK in their own tools.

The license should apply only to this SDK repository unless you clearly say otherwise.

The official MozAluz RBGI Viewer and MozAluz RBGI Editor are separate closed-source applications.

---

## Credits

Created by **sonic Fan Tech**.

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
.RBGI Format SDK / SharedRBGIformat DLL
```
