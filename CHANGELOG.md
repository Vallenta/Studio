# Changelog

All notable changes to the **Vallenta Studio** extension will be documented in this file.

## [1.1.6] - 2026-07-05

### Added
- **EurekaLog post-processing** — Vallenta Studio can now run EurekaLog's `ecc32` on the built executable after a successful build, injecting its exception tracking and compressed debug info without editing any project file. Enable it under **EurekaLog** on the Settings page, where `ecc32.exe` is auto-detected per installed Delphi version (with a manual override); only projects that have EurekaLog enabled are processed. A **Post-process now** button runs it against the current build on demand, and the `ecc32` command and output appear in the Build Output panel.
- **Unused unit detection** — an optional diagnostic flags units in a `uses` clause (interface or implementation) that are never referenced in the file — shown faded in the editor like an unused import, and listed in the Problems panel. On by default at Information severity; change the severity, switch it off, or manage the ignore list for side-effect-only units (e.g. `FastMM4`) under **Unused Units Detection** on the Settings page.
- **Build Output panel, auto-scroll** — the **Output**, **Warnings** and **Errors** tabs now have an **Auto-scroll** checkbox that keeps the view pinned to the newest entry as a build runs; each tab remembers its own setting.

### Changed
- **LSP, faster diagnostics on large units** — semantic diagnostics for a big unit now finish in less than half the time.

### Fixed
- **LSP, sporadic wrong diagnostics with several files open** — a file could briefly get diagnostics computed from another open file's content.
- **LSP, `{$IFDEF}` regions used the wrong platform on auto-start** — a language server that started automatically (e.g. after switching to a project) evaluated conditional regions against the project's first-listed platform instead of the one selected in the Build toolbar, so platform-specific code could be grayed out the wrong way. It now follows the selected platform and configuration.
- **LSP, types in the `implementation` section** — Ctrl+Click / F12 and hover now resolve a class declared in a unit's `implementation` section — on its name in a method implementation header (`procedure TFoo.Bar;`) and on members inherited from an implementation-section base class; previously these were not found.
- **LSP, `{$IF}` with source constants** — a condition that references a constant declared in the unit or an include file (e.g. `{$IF TEST_VERSION >= 5}`) now evaluates correctly.

## [1.1.5] - 2026-07-02

### Added
- **Rename Unit** — renaming a unit now updates everything in one step: the `.pas` file and its companion `.dfm`/`.fmx`, the `unit X;` header, every `uses` reference across your `.pas`/`.dpr`/`.dpk` (including the `in '…pas'` path). Trigger it three ways: **F2** on a unit name in the editor, from the **Source Files** view (right-click → **Rename Unit…**), or by renaming the `.pas` in the Explorer. Each reference keeps its own spelling — a short `Logging` stays short, a qualified `Acme.Core.Logging` stays qualified.
- **Dproj Editor, unit scope names** — a project's **Unit Scope Names** (namespaces) are now editable per configuration and platform in the Project Options editor, with the same list editor and parent-inheritance as Conditional Defines.
- **Code completion, identifiers and members** — completion now suggests in-scope identifiers as you type — global routines, variables, constants, types and enum values, plus the current routine's locals and parameters and the enclosing class's members — and member suggestions open automatically after you type `.`, like the Delphi IDE. Previously only member completion after a `.`, triggered with `Ctrl+Space`, was available.

### Fixed
- **Dproj Editor, Runtime Packages dialog** — editing **Runtime Packages** now opens a correctly titled dialog instead of one labelled "Conditional Defines".
- **New Delphi-File** — a newly created file now shows in the **Source Files** panel and is picked up by Find All References straight away, instead of only after reopening the project.
- **LSP, anonymous methods** — a parameter of an anonymous method (an inline `procedure`/`function`) used in its body is no longer flagged as an unknown identifier, and hover and Go to Definition now resolve it.
- **LSP, Go to Definition on a unit** — Ctrl+Click / F12 on a unit in a `uses` clause now opens its file with the correct on-disk capitalization in the editor tab, instead of an all-lowercase filename.
- **LSP** — a few valid Delphi constructs no longer trigger a false *Syntax error*: a procedure/function-type field with a calling convention (`procedure(); cdecl;`), a variable with a hint directive and a value (`Integer platform = 2`), a `/` inside an `asm` block, and a label on an `if`/`else` branch.

## [1.1.4] - 2026-06-27

### Added
- **Debugger, local variables in optimized builds** — local variables, parameters and `Self` now show in the **Variables** and **Watch** views when debugging optimization-compiled builds; previously many reported "An unspecified error has occurred".

### Changed
- **Project symbol cache, vsdcache folder** — the per-project cache files now sit in a single hidden `.vsdcache/` folder beside the `.dproj` instead of scattering several files into the project folder; an existing cache moves there automatically on the next start. If you track a Delphi project in git or svn, ignore `.vsdcache/`.

### Removed
- **Settings, three unused entries** — *Compiler Path*, *BPL Output Path* and *DCP Output Path* had no effect on builds and are no longer shown on the Settings page.

### Fixed
- **Source Files panel, scrollbar** — the panel's scrollbar now follows the color theme instead of staying white on dark themes, matching the other sidebar scrollbars.

## [1.1.3] - 2026-06-24

### Added
- **Find All References, units** — `Shift+F12` (or the right-click menu) on a unit name now finds every place that unit is used across the project — whether the cursor is on a `uses`-clause entry, the `unit`/`program`/`library`/`package` header, or a `.dpr`/`.dpk` `uses`/`contains`/`requires` entry. All spellings of the same unit match (e.g. `Forms` and `Vcl.Forms`), and a same-named unit on a different search path is not included.

### Fixed
- **Semantic highlighting, `uses` clause** — the final segment of a dotted unit name (e.g. `SysUtils` in `System.SysUtils`) is now colored as a unit rather than as a namespace.
- **Indexing scope, multi-program folders** — a project no longer indexes sibling programs' `.dpr`s or third-party `.dpk` packages that happen to sit on its search path; Find All References stays scoped to the active project and indexing is leaner.
- **LSP, legacy `begin` initialization** — a unit using a `begin … end.` block in place of an `initialization` section (the old Turbo Pascal form) now parses correctly; previously the whole unit failed to parse, breaking hover, navigation and highlighting.

## [1.1.2] - 2026-06-23

### Fixed
- **LSP, hover shows textDocument failure** — while editing and the mouse pointer is on a symbol, an error could occure.

## [1.1.1] - 2026-06-21

### Fixed
- **VSIX Package** — wrong deployment option triggered a LSP startup failure.

## [1.1.0] - 2026-06-21

### Added
- **Rename Symbol (key "F2")** — rename a Delphi symbol and every use of it across the project — its declaration, implementation, and all references. Covers locals and inline `var`s, unit-level types, routines, variables and constants, and type members (methods, fields, properties, constructors, destructors, helper members).
- **Rename, clear refusals** — a rename that would clash with an existing name, or that can't be applied safely (interface methods, RTL/library overrides, or symbols declared outside your project), is blocked with a short explanation and a button that jumps to the conflicting symbol.
- **Rename, one-time backup reminder** — the first rename shows a one-time reminder to keep your source backed up or under version control.
- **Project explorer, add a project from its `.dpr`/`.dpk`** — you can now add a project by selecting its `.dpr`/`.dpk` source even when it has no `.dproj` yet; a default `.dproj` is created next to the source.

### Fixed
- **Debugger, object properties** — expanding the `[Properties]` node on a form or component no longer pops a “… has not been registered as a COM class” error or briefly freezes; a few properties that act on the running program when read (`ComObject`, `Handle`, `Canvas`) are no longer listed.
- **LSP, members inside a `with` block** — hover and member resolution now work for a member reached through a `with` whose target type is declared in the unit's `implementation` section (e.g. a local type alias); previously the member couldn't be resolved.
- **Dproj Editor, build events** — build events now run reliably for every configuration and platform, with Delphi macros such as `$(PROJECTPATH)` resolved (some previously ran empty).

## [1.0.10] - 2026-06-16

### Added
- **Add Existing File** — Right-click a project (**Projects** sidebar) or a unit folder (**Source Files**) to add an existing `.pas` to the `.dpr`/`.dpk`, with a project-relative path and a `{FormName}` comment when a matching `.dfm`/`.fmx` exists.
- **Remove from Project** — Right-click a unit in the **Source Files** view to remove it from the `.dpr`/`.dpk`, fixing the surrounding comma or semicolon.

### Fixed
- **Debugger, Watch — interface properties** — a property whose value is an interface now shows the object behind it instead of `nil`, including through multi-step property chains (`a.b.c`). On 64-bit and 32-bit.
- **Debugger, Watch/hover (32-bit) — properties of your own classes** — properties backed by a getter method now resolve on 32-bit (in the `[Properties]` node, Watch, and on hover); several previously showed no value.
- **Debugger, Watch/hover — fields of by-reference record parameters** — reading a field of a record passed as a `var`, `out` or `const` parameter (e.g. `Test.SomeField`, including nested records) now resolves instead of reporting the field as not available. On 64-bit and 32-bit.
- **LSP, inline `var` in a program body** — hover and member resolution now work for an inline `var` declared in a `.dpr`/`.dpk` main `begin…end` block; previously this only worked inside a unit's routines.
- **LSP, empty record constant** — a typed constant with an empty `()` value (a record with no fields, e.g. one that only has methods) no longer shows a false “missing token” error.
- **LSP, member access through a typecast** — hover and Go to Definition now resolve an identifier inside a typecast or parenthesized expression.
- **Dproj Editor, build events** — build events added or edited in the project editor now run reliably, including multi-command events; a command was previously truncated or dropped. 
- **Dproj Editor, “inherit parent build events”** — turning this off now sticks: the unchecked state persists when you reopen the editor, and the configuration grid shows the level's own value instead of the inherited one.

## [1.0.9] - 2026-06-13

### Added
- **Dproj Editor, Delphi default values** — options not stored in the `.dproj` now show Delphi's built-in default (e.g. *Optimization* checked on Base), marked as `default`.

### Changed
- **Sidebar redesign** — the Vallenta Studio sidebar now has a permanent header showing the logo, version, subscription status and quick access to Settings and Account. The Projects, Build and Source Files sections below look and work as before, with individually adjustable heights.

### Fixed
- **Debugger, Delphi array indices** — static arrays with a non-zero lower bound (e.g. `array[1..8] of Char`) now show their declared Delphi indices in the **Variables** and **Watch** views — `[1]…[8]` instead of zero-based `[0]…[7]`.
- **Build Toolbar, Cancel Build** — cancelling during the "Converting debug symbols…" phase now also stops the symbol converter; previously only the MSBuild process was cancelled.
- **LSP, pointer member access** — member resolution (hover, completion, navigation) now works through pointer dereferences, e.g. `P[0].Field` on a pointer-to-array, `P^[0].Field`, and `PRec^.Field`.
- **Structural highlighting, nesting-level colors** — an `else if` chain without `begin`/`end` now stays one flat color instead of stepping a color deeper at each `else if`; an `if` nested inside a `begin`/`end` block still steps to the next color.

## [1.0.8] - 2026-06-09

### Added
- **Source Highlight Enhancements** — symbol-type and nesting-level colors are now edited with visual color-picker dialogs (with a live code preview) instead of JSON.
- **Source highlighting, unit names** — dotted unit names in a `uses` clause are now fully colored, with a separate **unit** color (new picker entry) for the unit and its namespace prefix.
- **Source highlighting, constants** — `const` values now get their own **constant** color (new picker entry), distinct from regular variables.

### Fixed
- **Semantic coloring** — fixed a language-server error when a Copilot inline suggestion is inserted while typing, which could briefly drop the semantic colors.
- **Structural highlighting, nesting-level colors** — `begin`/`end` in an `if`/`else` branch now take the color of that `if`, and a nested or `else if` `if` steps to the next color instead of repeating its parent's.
- **Debugger, Call Stack on an exception (32-bit)** — when the debugger stops on an exception and unwinds the Delphi stack, sometimes a frame resolved to a wrong source unit.

## [1.0.7] - 2026-06-08

### Added
- **Semantic source coloring** — the language server now colors identifiers by meaning: types, classes, interfaces, enum members, methods, properties, fields and parameters each get a distinct color.
- **Structural keyword highlighting** — block keywords (begin/end, if, for, case, try, …) are tinted by nesting level, the whole construct lights up when the cursor is on one of its keywords, and control-flow keywords (exit, break, continue, raise, …) can be shown in bold.
- **Source Highlight Enhancements settings** — a new section on the extension's settings page switches each highlight on or off and adjusts its colors. A visual color picker and further refinements are on the way.

### Fixed
- **Find All References** — references in units that use the symbol's unit from their `implementation` section (typically forms) are now found; many were previously missed.
- **Run/Debug, working directory** — a launched program now starts in the build output folder (e.g. `Win32\Debug`), instead of the project root.

## [1.0.6] - 2026-06-05

### Added
- **Find All References, form-file values** — searching a component or an enum value now also finds where it's used as a property value in a `.dfm`/`.fmx` form (e.g. `Action = MyAction`, `Speed = spSlow`, including set and scoped values).

### Fixed
- **Find All References, form files** — symbol references inside `.dfm`/`.fmx` forms are now found reliably; some were previously missed.
- **LSP** — false *Syntax error* on Delphi 13 inline `if` (conditional/ternary) expressions, e.g. `X := if Cond then A else B`.

## [1.0.5] - 2026-06-04

### Added
- **Debugger, Variables/Watch — object properties in the tree** — expanding a Delphi object now shows a `[Properties]` node listing its properties backed by a getter method (numbers, strings, enums, sets, Booleans, object references and `Variant`)

### Fixed
- **Debugger, hover on a property getter (64-bit)** — hovering a method-backed property such as `widget.Count` now shows its value
- **Project explorer, active project from a group** — a project activated from inside a project group is now restored as the active project, and revealed in its group, when you reopen the workspace.
- **LSP** — false *Syntax error* on identifiers containing non-Latin Unicode letters (e.g. Greek `α`, Cyrillic, CJK); only Latin letters were recognized before.
- **LSP, code completion on inline constants** — autocomplete now works after an inline `const c = …;` (typing `c.` lists the type's members), matching inline `var`.
- **LSP, overloaded method return types** — code completion and hover now infer the return type from the overload matching the call's arguments (e.g. `obj.Make(b)` picks the overload that takes `b`'s type)

## [1.0.4] - 2026-06-03

### Hotfix

## [1.0.3] - 2026-06-03

### Added
- **Debugger, readable exception popup** — stopping on a Delphi exception now shows the exception class and message (e.g. `EConvertError: '12x' is not a valid integer value`) instead of the raw `0x0EEDFADE` code and parameter dump. Works on 64-bit and 32-bit.
- **Debugger, exception ignore list** — a new **Delphi Exception Filters** view in the Run and Debug sidebar selects which exception types the debugger skips, by name or `*` pattern (e.g. `EAbort`, `E*`); when stopped, an **Ignore this type** button adds the current one and continues. `EAbort` is skipped by default.

### Fixed
- **Search paths, environment-variable overrides** — Delphi *Environment Variables* overrides (e.g. `$(WEATHERLIBS)`) used in library and project search paths are now resolved, so units in those folders are found by the LSP and included in debug symbols.
- **Find All References, overloaded methods** — searching one overload of a method now returns only that overload's references and its descendant overrides, instead of also listing the other overloads' sites.
- **Find All References, enums and interface delegation** — a reference to an enum value is no longer confused with a same-named value in a different enum, and method-resolution clauses (`procedure IFoo.Bar = MyBar;`) are now found from both the interface method and its implementation.
- **Debugger, exception at the end of a procedure** — an exception raised by a procedure's last statement was shown on the procedure's closing `end`; it now points to the raising statement.
- **Debugger, Watch panel — overridden property getters** — inspecting a property whose getter is overridden in a derived class now shows the derived class's value instead of the base class's, on 64-bit and 32-bit.
- **Debugger, Watch panel — property chains through class getters** — a multi-step property chain whose links are class getters now resolves at every step (previously only interface chains did), and a plain field hop inside such a chain that used to report "not available" now reads correctly, on 64-bit and 32-bit.

## [1.0.2] - 2026-06-02

### Fixed
- **Build** — the `NoDefaultCurrentDirectoryInExePath` environment variable is no longer passed to spawned Delphi build, run, and debug processes, so it no longer reaches build events.

## [1.0.1] - 2026-06-01

### Changed
- **Project explorer** — the active project and recent projects list now save to a separate `projects.local.json`, so a shared `projects.json` no longer carries per-machine state; existing files are read and migrated automatically.

### Fixed
- **LSP** — false *Unknown member* error on `TArray` static generic methods (e.g. `TArray.Contains<Int64>`, `TArray.IndexOf<…>`).
- **LSP** — false *Syntax error* on binary integer literals (`%`), including with `_` digit separators (e.g. `%0000_0001`).
- **Build panel** — failures from post-build steps (MSBuild `MSBxxxx` errors, e.g. a failing build event) now appear in the **Errors** tab instead of leaving it empty on a failed build.

## [1.0.0] - 2026-05-31
- **Release**

## [0.9.17] - 2026-05-30

### Added
- **Dproj Editor, visual option-set editor** — referenced option sets are now editable directly inside the Project Options editor. Click one in the tree and its settings open in the same grid — and the same Conditional Defines / Search Path / Environment Variables / build-event dialogs — used for build configurations. Because an option set is a flat bundle of values with no platform dimension (mirroring the Delphi IDE, where the option-set **Target** dropdown is empty), it's shown as a single **Property / Value** column. A banner names the file, lists the configurations that reference it, and offers **Open raw XML**. Properties the editor doesn't surface — and any hand-authored platform-specific groups — are preserved untouched on save.
- **LSP, Go to Definition target (interface or implementation)** — Ctrl+Click / F12 on a procedure or method can now jump straight to its **implementation body** instead of the interface declaration. Choose the behavior with the new Vallenta Studio settings page under **LSP Server**. Symbols with no implementation (types, fields, constants, abstract/external methods) always go to the declaration.
- **Debugger, show a value as a date/time** — `TDate`, `TTime` and `TDateTime` are stored as plain floating-point numbers, and **the debugger currently can't tell them apart from an ordinary `double`**, so they show as a raw number (e.g. `37622`). When you know a number is really a date, right-click it in the **Variables** or **Watch** view and choose **Show as Date/Time** ▸ **Date** / **Time** / **Date + Time** — the row reformats in place using your Windows regional format and stays converted as you step; **Show as Raw** switches it back. Works on both 64-bit and 32-bit. ⚠️ This is a manual, temporary solution until the type can be detected automatically.

### Changed
- **Dproj Editor, clicking an option set** — now opens the inline visual editor instead of the raw `.optset` XML; the raw file is still one click away via the banner's **Open raw XML** link.

## [0.9.16] - 2026-05-28

### Added
- **Debugger, Watch panel — helper members & string length** — Helper members on records, classes and simple types (e.g. `IntVal.Doubled` on an `Integer`), plus `.Length` on any string kind (`string`, `AnsiString`, `WideString`, `ShortString`), now resolve from the Watch panel and on hover, on both 64-bit and 32-bit. A member the linker optimized out reports a clear "not available" message instead of a cryptic type error.

### Changed
- **New Delphi-File dialog, redesigned** — the **New Delphi-File** dialog now matches the Account page, with a logo header and card-based sections. The file-type choice is shown as two clearly selectable tiles (**Unit** / **Form**) with an unmistakable selected state, and a live preview shows the file(s) that will be created.

### Fixed
- **New Delphi-File, file extension typed into the unit name** — entering a name that already ends in `.pas`, `.dfm` or `.fmx` (e.g. `Unit1.pas`) no longer creates a file with a doubled extension (`Unit1.pas.pas`); the extension is removed and the correct one is applied. Namespaced names such as `QuickLib.Forms.Types` are preserved.
- **Debugger, inline `for` loop variables missing from Watch/Locals** — inline loop variables (`for var i := …`) and inline variables whose value is inferred from them were generated without a usable type, so they didn't appear while debugging. They now resolve to their real type (e.g. `Integer`).
- **Debugger, inline variables sometimes missing / shown out of scope** — local variables declared with inline `var` weren't always resolved while debugging, especially ones declared after a nested `begin … end` block. They now show reliably, and the debugger takes their scope into account — a variable appears only while execution is inside the block where it's declared.

## [0.9.15] - 2026-05-26

**A milestone release for the debugger.** You can now inspect far more right from the **Watch panel and on hover**, call methods on your objects, read their properties (including inherited ones), drill into interface or class references, and see sets, enums and Booleans.

### Added
- **Debugger, Watch panel — call Delphi methods** — Calling methods directly from the Watch panel, including ones that return a `string` or a `Variant`, on both 64-bit and 32-bit targets. Calls resolve against the actual type of the variable you're inspecting, so an unrelated class that declares a same-named method can't misroute them.
- **Debugger, Watch panel — read properties** — Properties backed by a getter method now resolve and call their getter automatically, including properties **inherited** from a base class or interface — for the common property types (numbers, strings, enums, sets, object references, `Variant`) on 64-bit and 32-bit.
- **Debugger, Watch panel — inspect interfaces** — Interface references now show the actual object behind them (with their real fields), and you can follow a property chain through interfaces such as `WeatherReport.ShortName.Value`.
- **Debugger — values shown in Delphi form** — Sets, enums and Booleans appear the way they look in code (`[fsBold, fsItalic]`, `fsBold`, `True` / `False`), whether read directly or returned by a getter.

### Changed
- **Dproj Editor, option sets** — option sets are now listed above their build configurations in the tree and rendered in a smaller font.
- **Build Toolbar, configuration dropdown** — build configurations are now listed in hierarchical (treeview) order with child configurations indented; referenced option sets are no longer listed inside the open dropdown, but are still shown in brackets on the collapsed field (e.g. `Release (OptionSet_Lib)`).

### Fixed
- **Option Sets, base option set ignored** — search paths from a base-level option set (`'$(Base)'`, applied to all configurations) were dropped and never reached the LSP; they are now resolved and shown in the Dproj Editor tree and Configuration dropdown.
- **Dproj Editor** — clicking an option set now opens its `.optset` file as XML; previously nothing happened.

## [0.9.14] - 2026-05-22

### Added
- **Settings, Custom Delphi Settings mode** — pick "Custom Delphi Settings" in the version dropdown to manually enter installation and library paths the LSP will use, with no Delphi installation required.
- **Option Set (`.optset`) support** — referenced option sets are shown in the Configuration dropdown and the Dproj Editor tree, and their values are merged into resolved properties used by the build, debug, and symbol-converter paths.
- **Debugger, Watch panel for Delphi method calls and procedure locals** (preview) — you can now call most Delphi methods directly from the Watch panel while debugging, including methods that take `string`, `AnsiString`, `WideString` or `Variant` parameters, using either `"…"` or Pascal `'…'` quotes. Local variables inside plain procedures (not just class methods) are also visible in the Watch panel now. ⚠️ **Pre-release:** calling a method that returns a complex type (a `Variant`, an interface, a dynamic array, or a record with managed fields) will most likely crash the debugged program - stick to methods returning simple types (numbers, strings, class references) for now.

### Changed
- **Build Toolbar** — Run/Debug/Build/Rebuild/Clean buttons (and the F5 / Ctrl+F5 keybindings) are disabled when no Delphi installation is configured; the Configuration and Platform dropdowns stay active so .dproj selections can still be edited.

### Fixed
- **Debug** — pressing Debug after editing a source file no longer skips the build with a wrong "Up-to-date" status.
- **LSP** — guard at the spawn site blocks a second LSP child for a project that already has one (fixes doubled hover/completion responses).

## [0.9.13] - 2026-05-19

### Added
- **`with` statement support** — Hover, Goto Definition, diagnostics, code completion, and Find All References now understand unqualified identifiers inside `with` blocks, including multi-target, nested, inherited-member, and Delphi shadowing semantics (with-target shadows routine locals; block-locals shadow the with-target).
- **Find All References, scope picker** — `Shift+F12` now asks whether to search the project closure (recommended; only files your project transitively uses) or every indexed source, so symbols like `TSearchRec` no longer surface hits inside library units (e.g. FireDAC) the project never references.
- **Find All References, redesigned sidebar** — results are now grouped in three levels (directory path → source file → match), with the code-line preview on the left and the line number rendered as a right-aligned pill. Path groups show file and match counts; expand/collapse state is preserved across streaming updates and re-runs.
- **Build panel, project defaults** — a new save button next to the Platform dropdown writes the current selection back to the `.dproj` as the project default.

### Fixed
- **Build, command-line too long on large projects (MSB6002 / MSB6003)** — passes `/p:DCC_ForceExecute=true` so the Delphi compiler reads search paths from a `.cmds` file instead of the 32K-limited command line.

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
