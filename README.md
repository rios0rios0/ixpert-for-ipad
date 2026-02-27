<h1 align="center">iXpert for iPad</h1>
<p align="center">
    <a href="https://github.com/rios0rios0/ixpert-for-ipad/releases/latest">
        <img src="https://img.shields.io/github/release/rios0rios0/ixpert-for-ipad.svg?style=for-the-badge&logo=github" alt="Latest Release"/></a>
    <a href="https://github.com/rios0rios0/ixpert-for-ipad/blob/main/LICENSE">
        <img src="https://img.shields.io/github/license/rios0rios0/ixpert-for-ipad.svg?style=for-the-badge&logo=github" alt="License"/></a>
</p>

An iPad-optimized version of the iXpert computational unit converter, rebuilt with a single-screen layout that places the conversion mode selector, input field, and output field all on one view. Unlike the iPhone version which uses flip animations between separate Option and Detail views, the iPad version leverages the larger screen to display everything simultaneously, with a checkbox-style mode selection and `UIPopoverController` for the how-to guide. Development discontinued on 2012-03-11.

## Differences from the iPhone Version

| Aspect | iXpert (iPhone) | iXpert for iPad |
|--------|----------------|-----------------|
| **Layout** | Separate Option and Detail views with flip animation | Single unified view with all controls visible |
| **View switching** | `MasterViewController` swaps subviews with `UIViewAnimationTransitionFlip` | `OptionViewController` handles everything; `MasterViewController` only loads it |
| **Mode selection** | Tapping a button flips to a detail screen | Tapping a button highlights it (checkbox effect) and changes the active mode in-place |
| **How-to guide** | Curl-down animation replaces the current view | `UIPopoverController` presents a floating popover |
| **About dialog** | Action sheet with 2 options | Action sheet with 3 options (includes "How to Use This App") |
| **Splash screen** | Portrait iPhone Default.png | Portrait iPad Default-Portrait~ipad.png |
| **Input validation** | In `DetailViewController` | In `OptionViewController` (same logic, different location) |
| **Pre-built binary** | `.app` bundle only | `.app` bundle + `.ipa` file |

## Features

### Conversion Modes

| Mode | Encode | Decode | Description |
|------|--------|--------|-------------|
| **Hex** (ASCII) | ASCII to Hex | Hex to ASCII | Character-by-character hexadecimal encoding |
| **Hex2** (Decimal) | Decimal to Hex | Hex to Decimal | Numeric base conversion |
| **Bin** (ASCII) | ASCII to Binary | Binary to ASCII | Character-by-character binary encoding |
| **Bin2** (Decimal) | Decimal to Binary | Binary to Decimal | Numeric base conversion |
| **Oct** (Decimal) | Decimal to Octal | Octal to Decimal | Numeric base conversion |
| **Rev** (Reverse) | Reverse string | -- | Reverses the input string |
| **Loo** (Leet/Loco) | ASCII to Leet | Leet to ASCII | Leet-speak transformation |
| **B64** (Base64) | Base64 encode | Base64 decode | Standard Base64 encoding/decoding |
| **MD5** | MD5 hash | -- | One-way MD5 hash (no decode) |

### User Interface
- **Unified single-view layout** -- mode buttons, Encode/Decode buttons, and input/output text views all on one screen
- **Checkbox-style button selection** -- tapping a mode button deselects all others and highlights the selected one
- **`UIPopoverController`** for the how-to guide, presented as a floating panel
- **Input validation** -- character set filtering based on the selected mode (hex digits, numeric only, etc.)
- **Action sheet** with "More Apps", "How to Use This App", and "About This App" options

### Shared Components
- **`RatingSystem`** -- numeral system conversions (Hex, Binary, Octal, Reverse, Leet) -- same as the iPhone version
- **`Hashes`** -- Base64 encoding/decoding and MD5 hashing -- same as the iPhone version
- **`HowtoViewController`** -- usage instructions, displayed in a popover rather than a full-screen view
- **`HowtoViewController2`** -- additional how-to content page

## Technologies

- **Objective-C** (manual reference counting)
- **Xcode** with Interface Builder (XIB/NIB files)
- **UIKit** -- `UITextView`, `UIButton`, `UIPopoverController`, `UIActionSheet`, `UIAlertView`
- **CommonCrypto** -- MD5 hash generation
- **Foundation** -- `NSCharacterSet` for input filtering
- **iOS SDK 5.x** (iPad deployment target)

## Project Structure

```
ixpert-for-ipad/
├── Build/
│   ├── iXpert for iPad.app/            # Pre-built application bundle
│   └── iXpert for iPad.ipa             # Packaged iOS application
├── Imgs/
│   ├── Backgrounds/                    # iPad-specific background and splash screen
│   ├── Buttons/                        # Normal/selected states for all mode buttons
│   └── Icons/                          # App icons (72x72, Small, Small-50, iTunesArtwork)
├── Project/
│   ├── iXpert for iPad.xcodeproj/
│   └── iXpert for iPad/
│       ├── AppDelegate.{h,m}
│       ├── MasterViewController.{h,m}      # Root controller (loads OptionViewController)
│       ├── OptionViewController.{h,m}      # All-in-one: mode selection, input/output, encode/decode
│       ├── HowtoViewController.{h,m}       # Usage instructions (shown in popover)
│       ├── HowtoViewController2.m          # Additional how-to page
│       ├── RatingSystem.{h,m}              # Numeral system conversions
│       ├── Hashes.{h,m}                    # Base64 and MD5 implementations
│       ├── main.m
│       └── *.png                           # Runtime image assets
└── LICENSE
```

## Installation

1. Install **Xcode** (originally developed with Xcode 4.x)
2. Open `Project/iXpert for iPad.xcodeproj`
3. Select an iPad Simulator target and run (Cmd+R)

> **Note:** This project uses manual reference counting (MRC). Modern Xcode may require `-fno-objc-arc` compiler flags.

## Usage

1. Launch the app on an iPad -- the single-screen interface appears with all mode buttons on one side and input/output fields on the other
2. Tap a mode button (e.g., **Hex**, **B64**, **MD5**) -- the selected button is highlighted
3. Enter text in the input field
4. Tap **Encode** to convert, or **Decode** to reverse (where applicable)
5. Tap the **"i"** button for "More Apps", "How to Use", and "About" options

## Contributing

This project is no longer actively maintained. See [CONTRIBUTING.md](CONTRIBUTING.md) for historical build information.

## License

This project is licensed under the terms specified in the [LICENSE](LICENSE) file.
