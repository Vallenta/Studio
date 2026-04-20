# Changelog

All notable changes to the **Vallenta Studio** extension will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [0.9.3] - 2026-04-20

### Added
- **Copy Variable as Tree** — new right-click action in the debugger **Variables** and **Watch** views. Copies the full expanded variable hierarchy to the clipboard (up to 2 levels deep, 2000 nodes). Useful for inspecting complex records and objects without the 256-character truncation of "Copy Value".

### Fixed
- **Debug builds on older Delphi versions (XE2–XE4)** — the `DCC_DebugInformation` MSBuild parameter is now passed in the correct form per Delphi version (boolean for BDS < 12.0, integer enum for BDS ≥ 12.0). Previously, debug builds on XE2/XE3/XE4 could fail or produce no debug symbols.
- **Go to Definition no longer jumps to unrelated local variables** — when a symbol at the cursor shared its name with a local variable in a different procedure, navigation could land on the wrong declaration. Resolution now uses the proper scoped lookup.

## [0.9.2] - 2026-04-18

### Notes

First public beta release.

## [0.9.1] - 2026-04-15

### Notes

Internal release.

## [0.9.0] - 2026-04-06

### Added

#### Project Management
- Project Explorer sidebar with add/remove project, project groups, and recent projects
- Multi-project workspace support (`.vallenta` folder for settings)

#### Build System
- MSBuild integration: build, clean, rebuild
- Build toolbar with configuration (Debug/Release) and platform (Win32/Win64) selection
- Real-time build output streaming
- Compiler errors, warnings, and hints parsed into Problems panel
- Build all projects in a group (Pro)

#### Code Intelligence
- Built-in LSP server with Go to Definition, hover, code completion, and document symbols
- Semantic syntax highlighting
- Preprocessor support (`{$IFDEF}`, `{$DEFINE}`, `{$INCLUDE}`) with inactive region visualization
- Go to Declaration / Implementation navigation (Pro)
- Semantic validation detecting undefined types, methods, and variables (Pro)

#### Debugging (Pro)
- Source-level debugging with breakpoints, stepping, variable inspection, and call stacks
- Automatic debug symbol generation
- Version-specific natvis visualization for Delphi types (strings, arrays, variants, objects)
- Breakpoint persistence per project

#### Editor
- Full Object Pascal syntax highlighting (TextMate grammar)
- ANSI file encoding detection with one-click UTF-8 conversion
- Session persistence for open files when switching projects

#### Environment
- Auto-detection of Delphi installations from Windows Registry (10.x, 11.x, 12.x or 13.x)
