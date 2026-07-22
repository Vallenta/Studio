# Vallenta Studio — Roadmap

A forward-looking list of features planned for the Vallenta Studio extension. It's the single place to see what's planned and in progress; for features that have already shipped, see the [Changelog](CHANGELOG.md) and the [Feature Matrix](docs/feature-matrix.md).

This roadmap is **subject to change** and intentionally carries **no committed delivery dates** — priorities shift as the extension evolves and in response to user feedback.

**Status:** 📋 Planned · ✅ Recently shipped

## 📋 Planned

- **Delphi Breakpoints view** — A dedicated breakpoints panel that groups breakpoints by project and file, with custom labels and bulk enable / disable / delete — a clearer alternative to the generic Breakpoints list.
- **Symbols for source-less units (DCU)** — Hover, completion, and go-to-definition for units that ship only as compiled `.dcu` — RTL/VCL/FMX without source, and closed-source third-party libraries.
- **Parameter hints** — While filling in a function, method, or constructor call, see the callable's parameter list with the argument you are currently entering highlighted. All overloads are listed, with the one that best matches the arguments you have typed preselected.
- **macOS (Apple Silicon)** — Editing and IntelliSense — hover, completion, go-to-definition, Find All References, diagnostics, and semantic highlighting — on Apple Silicon Macs. Building and debugging remain Windows-only.
- **Attach to a running process** — Attach the debugger to an already-running Windows process by its process ID, with breakpoints, stepping, and Watch.
- **MCP interface for AI assistants** — Expose Vallenta Studio's Delphi code intelligence to AI tools that speak the Model Context Protocol — symbol resolution, Find All References, and go-to-definition, plus reading the active project and its configuration, and starting a build and reading back its errors and warnings — so an AI assistant works from the IDE's real understanding of your code instead of re-parsing it.
- **Code formatter** — Format your Pascal source with a choice of engine: a built-in formatter (the default — works offline, with no extra install, and the only one that can format just the selected lines) or an external command-line formatter you already use (pasfmt, Jedi Code Format or Free Pascal's ptop). Format the whole document or a selection, with optional format-on-save. A later update will add extensive style options — per-block indentation, one-line `begin`/`end` handling, casing, spacing, and alignment.
- **Unused units: inline-function awareness** — The unused-units hint learns the Delphi compiler's inline expansion rule. A unit that is only needed so an `inline` method can be expanded — for example `Contnrs` when calling `TObjectList.Add` on a field typed in another unit — is currently reported as unused, even though removing it triggers compiler hint H2443 and costs you the inlining. Such units will no longer be flagged, including the ancestor units an inline method's body relies on.
- **Breakpoint conditions in Delphi syntax** — Write conditional breakpoints the way you write Delphi: `Value = 10`, `Name = 'hello'`, `and`/`or`/`not`. Today conditions must use the underlying debugger's C++ syntax; this feature translates your Delphi expression automatically, including string comparisons for `string`, `AnsiString`, `UTF8String` and `WideString` variables. Conditions that cannot be translated are reported when the breakpoint is set, not when it is hit.
## ✅ Recently shipped

A few highlights — see the [Changelog](CHANGELOG.md) for the full history.

- **Class completion (Ctrl+Shift+C)** — Declare a method on a class or record — or a routine in the unit's interface — then press Ctrl+Shift+C (or right-click → Complete Class at Cursor) to generate its empty implementation body and jump straight to it. It completes every unimplemented method of the type at the cursor at once — class methods, constructors, destructors, operators, overloads and generics included — and works for global routines too, leaving already-implemented methods untouched.
- **Unused variables** — An optional hint that fades local variables and constants that are declared but never used within a routine, so leftover declarations are easy to spot as you type — without waiting for a compile. Can also flag variables that are only ever assigned but never read, and unused parameters (which the Delphi compiler never reports). On by default at information severity, with configurable severity.
- **EurekaLog integration** — Post-process your builds with EurekaLog automatically after every successful build, with no per-project post-build events to maintain. Vallenta Studio detects the EurekaLog compiler matching your Delphi version and runs it as part of the build, with per-version configuration and live detection status on the extension's settings page. Only projects that already have EurekaLog enabled are processed; everything else is left untouched.
- **Unused units** — An optional hint that fades `uses`-clause units never referenced in a file — across both the interface and implementation sections — so dead unit references are easy to spot and remove. On by default at information severity, with configurable severity and an editable ignore list for side-effect units.
- **Rename a unit** — Rename a unit and have everything update in one step: the `.pas` file and its companion `.dfm`/`.fmx` form, the `unit` header, every `uses` reference across your `.pas`, `.dpr` and `.dpk` files, and the project file — from the editor (`F2` on the unit name) or by renaming the file itself. Each reference keeps its existing style: a short `Forms` stays short, a qualified `Vcl.Forms` stays qualified.
- **Locals in optimized builds** — Local variables, parameters and `Self` now show while debugging optimization-compiled builds, where many previously failed to display.
- **LSP cache folder** — The offline symbol cache for your project moves into a single hidden `.vsdcache` folder, instead of placing several cache files next to each `.dproj`. 
- **Rename Symbol** — Rename a symbol and have every reference updated across the project in a single step, using the native VS Code Rename (`F2`) command, including references inside `.dfm`/`.fmx` form files.
- **Semantic highlighting** — Resolver-driven coloring of types, methods, properties, parameters, and enum members, plus same-identifier occurrence highlighting and structural begin/end coloring.
- **Find All References** — Scope-aware reference search across the project, including matches inside `.dfm`/`.fmx` form files.
- **Find unit references** — Locate every place a unit is referenced across `uses` clauses (in `.pas`, `.dpr`/`.dpk`, and the unit header), matched by physical unit identity rather than name.
- **Semantic color themes editor** — A visual editor for recoloring semantic tokens without hand-editing JSON.

## Have a request?

Found something missing? [Open a feature request](../../issues/new/choose) — community input helps shape what gets prioritized next.
