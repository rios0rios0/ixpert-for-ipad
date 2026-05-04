# Copilot Instructions for iXpert for iPad

## Project Overview

iXpert for iPad is a discontinued (since 2012-03-11) iOS app — an iPad-optimized computational unit converter. It is built in **Objective-C** using manual reference counting (MRC) and targets **iOS SDK 5.x** on the iPad form factor.

Unlike the iPhone version which used separate Option and Detail views with flip animations, the iPad version uses a **single unified view** (`OptionViewController`) that simultaneously shows the mode selector, input field, and output field on one screen. The how-to guide is presented via `UIPopoverController`.

> **Note:** This project is archived for historical reference only. No new features or bug fixes are planned.

## Repository Structure

```
ixpert-for-ipad/
├── .github/
│   └── copilot-instructions.md       # This file
├── Build/
│   ├── iXpert for iPad.app/          # Pre-built application bundle
│   └── iXpert for iPad.ipa           # Packaged iOS application archive
├── Imgs/
│   ├── Backgrounds/                  # iPad background and splash screen images
│   ├── Buttons/                      # Normal/selected state button images for each mode
│   └── Icons/                        # App icons (72x72, Small, Small-50, iTunesArtwork)
├── Project/
│   ├── iXpert for iPad.xcodeproj/    # Xcode project file
│   └── iXpert for iPad/              # Objective-C source files
│       ├── AppDelegate.{h,m}         # App entry point
│       ├── MasterViewController.{h,m}    # Root controller; only loads OptionViewController
│       ├── OptionViewController.{h,m}   # All-in-one: mode selection, input/output, encode/decode
│       ├── HowtoViewController.{h,m}    # Usage instructions shown in a UIPopoverController
│       ├── HowtoViewController2.m       # Second how-to page
│       ├── RatingSystem.{h,m}           # Numeral system conversions (Hex, Bin, Oct, Rev, Leet)
│       ├── Hashes.{h,m}                 # Base64 and MD5 implementations
│       ├── main.m                       # Application entry point
│       ├── MainWindow.xib               # Main window NIB
│       ├── OptionViewController.xib     # Main UI layout NIB
│       ├── HowtoViewController.xib      # How-to view NIB
│       ├── iXpert for iPad-Info.plist   # App bundle info
│       └── iXpert for iPad-Prefix.pch  # Precompiled header
├── CHANGELOG.md
├── CONTRIBUTING.md
├── LICENSE
└── README.md
```

## Technology Stack

| Component | Details |
|-----------|---------|
| **Language** | Objective-C (manual reference counting / MRC — no ARC) |
| **Platform** | iOS (iPad), iOS SDK 5.x |
| **IDE** | Xcode 4.x with Interface Builder (XIB/NIB files) |
| **UI Framework** | UIKit — `UITextView`, `UIButton`, `UIPopoverController`, `UIActionSheet`, `UIAlertView` |
| **Crypto** | CommonCrypto — MD5 hash generation |
| **Encoding** | Foundation — `NSCharacterSet` input filtering, custom Base64 in `Hashes` |

## Architecture and Design Patterns

- **MVC (Model-View-Controller):** Standard UIKit MVC pattern.
- **Root controller pattern:** `MasterViewController` acts as the root and loads `OptionViewController` directly — no navigation stack, no tab bar.
- **Unified single-view layout:** All mode buttons, Encode/Decode buttons, and I/O text views live in `OptionViewController` (versus the iPhone version which used view flipping between separate controllers).
- **Checkbox-style button selection:** Tapping a mode button deselects all others and visually highlights the selected one in-place.
- **Popover guide:** The how-to guide uses `UIPopoverController` (iPad-only API) instead of a full-screen view transition.
- **Manual Reference Counting (MRC):** All memory management is done explicitly with `retain`/`release`/`autorelease`. No `@autoreleasepool` blocks or ARC annotations.

## Conversion Modes

| Mode | Encode | Decode |
|------|--------|--------|
| **Hex** (ASCII↔Hex) | ASCII → Hex | Hex → ASCII |
| **Hex2** (Decimal↔Hex) | Decimal → Hex | Hex → Decimal |
| **Bin** (ASCII↔Binary) | ASCII → Binary | Binary → ASCII |
| **Bin2** (Decimal↔Binary) | Decimal → Binary | Binary → Decimal |
| **Oct** (Decimal↔Octal) | Decimal → Octal | Octal → Decimal |
| **Rev** (Reverse) | Reverse string | — |
| **Loo** (Leet/Loco) | ASCII → Leet | Leet → ASCII |
| **B64** (Base64) | Base64 encode | Base64 decode |
| **MD5** | MD5 hash | — (one-way) |

## Build Instructions (Historical)

> ⚠️ This project was originally built with Xcode 4.x targeting iOS SDK 5.x. Modern Xcode versions may require additional configuration.

1. Install **Xcode** (originally Xcode 4.x; modern Xcode should work with caveats)
2. Open `Project/iXpert for iPad.xcodeproj` in Xcode
3. Select an **iPad Simulator** target
4. Build and run (`Cmd+R`)

**Important:** The project uses **manual reference counting (MRC)**. When building with modern Xcode, you may need to add the `-fno-objc-arc` compiler flag to all `.m` source files under:
> Build Settings → Other C Flags, or per-file in Build Phases → Compile Sources

Pre-built binaries are also available in the `Build/` directory:
- `Build/iXpert for iPad.app/` — expanded application bundle
- `Build/iXpert for iPad.ipa` — installable package

## Testing

There is no automated test suite for this project (no XCTest targets). Testing was done manually:

1. Launch in iPad Simulator
2. Tap a mode button and verify it is highlighted
3. Enter text and tap **Encode** / **Decode** and verify output
4. Tap the **"i"** button and verify the action sheet options appear
5. Tap **"How to Use This App"** and verify the popover opens

## CI/CD

This repository has no CI/CD pipeline. There are no GitHub Actions workflows.

## Coding Conventions

- **Language:** Objective-C with manual memory management. No ARC, no Swift.
- **File structure:** `.h` header + `.m` implementation pairs for each class.
- **Naming:** `camelCase` for methods and variables; `PascalCase` for classes and types.
- **XIB/NIB files:** UI layout is defined in Interface Builder XIB files alongside their corresponding view controller `.h`/`.m` files.
- **No external dependencies:** The project uses only Apple system frameworks (UIKit, Foundation, CommonCrypto). There is no package manager (no CocoaPods, Carthage, or Swift Package Manager).
- **Image naming:** Button images follow the pattern `Button <Mode> Norm.png` / `Button <Mode> Sele.png` for normal and selected states.

## Common Tasks

### Viewing the Source Code

All source files are under `Project/iXpert for iPad/`. The main logic lives in:
- `OptionViewController.m` — handles mode selection, input validation, and encode/decode dispatch
- `RatingSystem.m` — numeral system conversion implementations
- `Hashes.m` — Base64 and MD5 implementations

### Inspecting the Pre-built Binary

The pre-built app bundle is at `Build/iXpert for iPad.app/`. You can inspect the `.ipa` by renaming it to `.zip` and extracting it.

### Understanding Memory Management

Since MRC is used throughout, always pair every `alloc`/`retain`/`copy` with a corresponding `release` or `autorelease`. There are no `weak` references or `__strong`/`__weak` qualifiers — those are ARC-only features.
