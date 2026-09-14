# Times New Roman

A SwiftUI trivia game about the Times New Roman typeface and the history of
typography, with an iOS 26 "Liquid Glass" UI (and a graceful fallback for
iOS 17–25).

## What's in here

- `Sources/` — SwiftUI app (SwiftUI-only, no storyboards)
  - `GlassCard.swift` — reusable Liquid Glass container/button style, with an
    `.ultraThinMaterial` fallback on iOS < 26
  - `Models.swift` — `Question` model + `QuizViewModel`
  - `StartView.swift`, `QuizView.swift`, `ResultView.swift` — the three screens
- `Resources/questions.json` — 500 quiz questions
- `generate_questions.py` — the script that generated `questions.json`
- `project.yml` — [XcodeGen](https://github.com/yonaskolb/XcodeGen) spec; CI
  generates the actual `.xcodeproj` from this file rather than committing a
  hand-edited project file
- `.github/workflows/build.yml` — CI that builds the app for the iOS
  Simulator on every push/PR

## About the 500 questions

Real, fact-checkable trivia about a single typeface tops out well under 500
unique items. Rather than pad the set with invented "facts," the question
bank was broadened (as you asked) to cover:

- **Times New Roman specifically** (~20 hand-written questions: Morison,
  Lardent, The Times, Monotype, etc.)
- **Printing/type history** (Gutenberg, Linotype, Monotype, letterpress, font
  formats)
- **Other well-known typefaces** (designer, release year, foundry,
  serif/sans-serif classification) — generated from a hand-verified table of
  ~30 real typefaces, so every fact is checkable
- **Typography terminology** (kerning, x-height, ligature, etc.)

Every question is generated from a small, curated, fact-checked data table in
`generate_questions.py` rather than written one-by-one from memory, so you can
audit or extend the source facts directly instead of trusting 500 free-form
claims. Re-run `python3 generate_questions.py` any time to regenerate
`Resources/questions.json` (e.g. after editing the fact tables).

## Running locally

You'll need Xcode 26+ (for the real Liquid Glass APIs) and
[XcodeGen](https://github.com/yonaskolb/XcodeGen) (`brew install xcodegen`).

```bash
xcodegen generate
open TimesNewRoman.xcodeproj
```

Then run on an iOS 26 simulator to see full Liquid Glass, or an iOS 17+
simulator to see the fallback styling.

## CI

`.github/workflows/build.yml` runs on `macos-26` GitHub-hosted runners
(Xcode 26.6 by default as of mid-2026), regenerates the Xcode project with
XcodeGen, and does an unsigned build for the iOS Simulator — no Apple
Developer account or signing certificate needed. If you later want to ship to
TestFlight/App Store, you'll need to add your Team ID, a signing certificate,
and a provisioning profile as repo secrets, and switch
`CODE_SIGNING_ALLOWED=NO` back to automatic/manual signing.
