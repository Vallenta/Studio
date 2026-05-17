# Changelog

All notable changes to the **Vallenta Studio** extension will be documented in this file.

## [0.9.12] - 2026-05-17

### Added
- **LSP, redesigned hover popup** — kind badges, colored title with inheritance/return-type suffix, modifier badges, clickable `file.pas:line` link, inline **Other overloads** / **Inherited** sections, and a Go-to-Def / Find References / Peek action row. New setting `vallenta.studio.hover.colorize` (`auto` / `off`).

### Fixed
- **LSP, Find All References on a type** — clicking a parameter type identifier (e.g. `const A: TMyType`) returned only the clicked position; all uses across the project are now found.
- **LSP, code completion inside a `uses` clause** — typing a unit prefix like `System.` or `Generics.` now lists matching units (`Classes`, `Collections`, `Defaults`, …) 

## [0.9.11] - 2026-05-12

### Added
- **Find Symbol** — workspace symbol picker via `Ctrl+T` and editor right-click "Find Symbol…". Lists classes, records, interfaces, methods, properties, and unit-level symbols across the indexed project.
- **Find All References, DFM/FMX form files** — `Shift+F12` now includes matches inside `.dfm`/`.fmx` form files (component type names, property values, event handler bindings) with cross-form indexing.

### Fixed
- **LSP, race condition when switching projects during server startup** — clicking another project while the active project's LSP was still in its `Starting` phase could spawn a phantom replacement process and leave the new project unable to initialize. Switches now abort the in-flight startup cleanly.
- **Build, race condition on quick project switch** — pressing Build right after selecting a new project while the editor was still restoring the previous session's files would build the previously-active project. Project state and build-toolbar update now happen before file restoration.

## [0.9.10] - 2026-05-08

### Added
- **Project-specific debugger environment variables** — `Debugger_EnvVars` and `Debugger_IncludeSystemVars` are now editable per build configuration and platform in the project editor's new **Environment Variables** row (two-column Name/Value editor with parent-inheritance)

### Fixed
- **Debugger, dynamic-array visualization for primitive and pointer element types** — `TArray<Int64>`, `<UInt64>`, `<Word>`, `<SmallInt>`, `<ShortInt>`, `<Boolean>`, `<Char>`, `<AnsiChar>`, `<AnsiString>`, `<Pointer>`, `<Extended>`, typed pointers (`PChar`, `PInteger`, …), procedural types, and nested `TArray<TArray<…>>` now render with length and elements instead of just a pointer address.
- **Debugger, named static-array elements** — `TArray<TByteBuf>` style arrays now render via `DynArrayRec<…>`; static-array element types are emitted as PDB UDTs so the natvis cast can resolve them.
- **Debugger, field-backed properties in watch and hover** — `property Name: string read FName write FName;` style properties now resolve to their backing field's value in watch expressions and hover tooltips.

## [0.9.9] - 2026-05-05

### Added
- **Update notification** — on activation after an extension update, a toast announces the new version and offers a **View Changelog** button that opens the Marketplace changelog page.

### Changed
- **Subscription gate notifications** — UI-triggered locked actions (Debug, Open Project Options, Build Group, LSP start, New File, …) now show a reason-specific toast (not signed in / Trial expired / status not retrieved / offline-stale) and open the Account page automatically.

## [0.9.8] - 2026-05-04

### Added
- **LSP, overload resolution for hover and Go-to-Definition** — when calling an overloaded routine, hover and `Ctrl-Click` now narrow to the specific overload by matching argument types (typed identifiers, literals, `nil`, implicit conversions, `var`/`out`/`const` modifiers, default values, open arrays, `class of`, set constructors). Falls back to showing all overloads when arguments cannot be inferred.
- **LSP, declaration hovers narrow to the single declared symbol** — hovering on a class-body declaration or implementation header (`constructor TFoo.Create;`) shows only that one method, not every same-named overload.
- **Find All References (`Shift+F12` or via Popupmenu)** — semantic, scope-aware reference search for local variables/parameters, unit-level types/functions/vars/consts/enum members, and type members (methods, fields, properties, constructors, destructors, helpers, interface methods) with polymorphism-aware override matching and per-overload precision. While a search runs, a progress notification with a **Cancel** button lets you abort long lookups;

### Fixed
- **LSP, duplicate signatures from overridden virtual methods** — `Create(AOwner: TComponent)` etc. inherited through multiple ancestors no longer appear as N identical entries; deduplication runs after `{$IFDEF}` filtering so condition-mismatched overrides collapse correctly.
- **LSP, method references via `@Foo`** are no longer narrowed (the address-of operator yields a procedural value, not a call).
- **LSP, duplicate server process during sign-in** — for Premium users, two feature-availability events fired in quick succession (`None→Free`, then `Free→Premium` once subscription data arrived) could spawn two LSP processes for the active project.

## [0.9.7] - 2026-04-28

### Added
- **ProjectExplorer, DFM/FMX form indicator in Source Files** — `.pas` units with a sibling `.dfm`/`.fmx` form file now show an inline `[DFM]`/`[FMX]` tag on the same row; click the tag to open the form, or right-click the `.pas` for a new "Open Form" entry
- **Toggle Form / Source** — press **F12** inside a `.pas` editor to open the matching `.dfm`/`.fmx` (and vice versa); also available as a CodeLens link at the top of the file

## [0.9.6] - 2026-04-25

### Fixed
- LSP, hover and type inference for generic method calls in expressions (e.g., `obj.method<T>`) — hover/CTRL-Click on the type argument now resolves, and inline `var x := obj.method<T>` infers the method's return type
- LSP, hover on a top-level function/procedure declaration no longer shows duplicate signatures when the same name exists in an imported unit 
- LSP, hover on a Delphi parameter-omission implementation header (`function Foo;` paired with a forward declaration) now shows the full signature from the interface declaration

## [0.9.5] - 2026-04-23

### Fixed
- LSP, resolve hover on type arguments inside generic type references 
- LSP, diagnostics race condition could occure resulting in undefined symbol errors
- missing updates on inactive regions when file is updated outside of VSCode
- F5 shortcut (start Debugging) only on Pascal files/projects

### Added
- **Build Configuration editable** - Project editor, build configuration can be added, renamed and deleted 

## [0.9.4] - 2026-04-21

### Fixed
- VallentaStudio Account RefreshToken failure

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
