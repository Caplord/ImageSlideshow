# Repository Guidelines

## Project Overview

This is the `Caplord/ImageSlideshow` fork, an iOS 15+ UIKit image-slideshow library. It ships a core Swift package product plus separate AlamofireImage, SDWebImage, and Kingfisher integration products. It is distributed exclusively via Swift Package Manager (no CocoaPods). Keep the public API source-compatible unless the change is intentionally versioned as breaking.

## Project Structure

- Core sources are in `ImageSlideshow/Classes/Core`; provider adapters are in `ImageSlideshow/Classes/InputSources`.
- `Package.swift` is the source of truth for Swift Package Manager products, dependencies, source inclusion, and bundled close-button assets.
- `Example/project.yml` generates the gitignored example `.xcodeproj` and declares `ImageSlideshow` as a local Swift package dependency (`path: ../`).
- Example tests are in `Example/Tests`. GitHub Actions in `.github/workflows/ci.yml` defines the supported build and test matrix.

## Build and Test Commands

- Build all SPM products for iOS as CI does: `xcodebuild -scheme ImageSlideshow-Package -destination 'generic/platform=iOS Simulator' build`.
- Prepare the example: `cd Example && xcodegen generate`.
- Test the example from `Example/`: `xcodebuild test -project ImageSlideshow.xcodeproj -scheme ImageSlideshow-Example -destination 'platform=iOS Simulator,name=<available iPhone>' CODE_SIGNING_ALLOWED=NO`.
- After package changes, build the package products and the example because the example additionally exercises the provider adapters and their bundled resources.

## Required Consumer Validation

- The OpenFleet iOS app consumes a tagged `ImageSlideshow` SPM version from its root `project.yml`; local changes here are not exercised by that app until a tag/version is intentionally integrated.
- When integrating a new version into `openfleet_ios`, update the package pin in the app's `project.yml`, run `make generate`, then run `make build SCHEME=SearchFramework CONFIGURATION=Staging`, `make test SCHEME=SearchFramework CONFIGURATION=Staging`, `make build SCHEME=OpenFleet CONFIGURATION=Staging`, and `make test SCHEME=OpenFleet CONFIGURATION=Staging` from the app root.
- Report standalone package/example results separately from consumer-app results; do not claim main-project validation before the new package version is actually resolved there.

## Coding and Generated-File Rules

- Use four-space Swift indentation, PascalCase types, camelCase members, and UIKit conventions already present in the library.
- Keep optional image-provider integrations isolated from the core product; do not add a provider dependency to `ImageSlideshow` itself.
- Preserve iOS 15 compatibility and the declared Swift tools/language compatibility unless a release explicitly raises them.
- Never edit `Example/ImageSlideshow.xcodeproj/project.pbxproj` directly. Change `Example/project.yml`, then regenerate with `xcodegen generate`; likewise, never edit the consuming app's generated `project.pbxproj`.
- Treat `.build/` and DerivedData as generated. Do not commit them.

## Tests, Commits, and Releases

- Add deterministic XCTest coverage under `Example/Tests` for navigation, page state, inputs, and regressions. Avoid network-dependent tests for provider adapters.
- Use concise imperative commits and keep upstream-sync, dependency, API, and generated-project changes separate.
- Update `CHANGELOG.md`, `README.md`, and `Package.swift` together when a release changes supported integrations or requirements. Do not tag or publish without explicit authorization.
