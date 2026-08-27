# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Status

Discontinued since 2012-03-11. Archived for historical reference — no new features or fixes planned. Expect to read code more than write it.

## What this is

iPad-only computational/text converter. Objective-C, **manual reference counting (MRC) — no ARC**, iOS SDK 5.x, Xcode 4.x with Interface Builder XIB/NIB files. No package manager and no third-party dependencies: only Apple frameworks (UIKit, Foundation, CommonCrypto).

## Build

No build automation and no automated tests (no XCTest target); the only CI is the Claude review and `@claude` mention workflows. Build is manual:

1. Open `Project/iXpert for iPad.xcodeproj` in Xcode.
2. Select an iPad Simulator target, run with `Cmd+R`.
3. Under modern Xcode, the MRC sources need `-fno-objc-arc` (Build Settings → Other C Flags, or per-file in Build Phases → Compile Sources).

Pre-built artifacts live in `Build/` (`.app` bundle and `.ipa`). Verification is by hand: pick a mode, enter text, tap Encode/Decode, check output; tap the `i` button for the action sheet and the how-to popover.

## Architecture

- `MasterViewController` is the root but does almost nothing — its only job is to `initWithNibName:@"OptionViewController"` and `insertSubview:` it. No navigation stack, no tab bar.
- `OptionViewController` is the whole app: mode buttons, Encode/Decode buttons, and input/output `UITextView`s on one screen. This single-view design is the key divergence from the iPhone version, which flipped between separate Option and Detail controllers. Don't reintroduce a detail/flip flow.
- Mode state is a plain `NSString` (`NSStringOption`, e.g. `@"Hex"`, `@"B64"`, `@"MD5"`). `ChangeProcessor:` sets it; `Process:` dispatches on it via long `if/else` chains; `CheckBoxEfect:` clears every sibling `UIButton`'s selected state and selects the tapped one. Adding a mode means touching all three plus the XIB outlets.
- Conversions are static class methods, not instances: numeral/text systems in `RatingSystem` (Hex, Bin, Oct, Reverse, Leet, and decimal variants), Base64 + MD5 in `Hashes` (`CommonCrypto` `CC_MD5`). MD5 and Rev are encode-only — there is no decode branch for them in `Process:`.
- Input validation is in `OptionViewController`'s `textView:shouldChangeTextInRange:` using inverted `NSCharacterSet`s (`forHex`/`forBin`/`forOct`), only for the numeric modes (`Hex2`/`Bin2`/`Oct`).
- The how-to guide uses `UIPopoverController` (iPad-only API), not a full-screen transition.

## Conventions

- MRC throughout: pair every `alloc`/`retain`/`copy` with `release`/`autorelease`; `dealloc` releases outlets, `viewDidUnload` nils them. No `weak`/`__strong`/`@autoreleasepool`.
- `.h`/`.m` pairs per class; UI in matching XIB files wired through IBOutlet/IBAction.
- Button image assets follow `Button <Mode> Norm.png` / `Button <Mode> Sele.png`.

See `.github/copilot-instructions.md` for the full mode table and repository-structure breakdown, and `README.md` for the iPhone-vs-iPad feature comparison.

<!-- chlog:start -->
## Changelog (chlog) — MANDATORY

If the repository you are working in uses chlog (a `.chlog.yaml` or `.chlog.yml`
config file, or a `.changes/` directory, exists at the project root), the
following is binding and ALWAYS applies: whenever you make ANY change, you MUST
create a changelog fragment as part of the same change — automatically, without
being asked, before committing.

- Do NOT edit CHANGELOG.md directly; it is generated from fragments.
- Create the fragment with:
  `chlog new --kind <Kind> --body "<imperative description>"`
- Valid kinds: Added, Changed, Deprecated, Removed, Fixed, Security
- Choose the kind that best matches the change (e.g., new feature → Added,
  bug fix → Fixed, behavior change → Changed, removal → Removed, security fix → Security).
- If the change is backward-INCOMPATIBLE with the public API (a breaking
  change), you MUST add the `--breaking` flag:
  `chlog new --kind <Kind> --breaking --body "<description>"`.
  This is the ONLY thing that triggers a major version bump — the kind alone
  never does (per SemVer, major = incompatible change). When unsure whether a
  change breaks compatibility, ask the user instead of guessing.
- Fragments are YAML files in `.changes/unreleased/`; stage them with your commit.
- `chlog check` fails the build when a fragment is missing — never skip it.
<!-- chlog:end -->
