# Vallenta Studio — Roadmap

A forward-looking list of features planned for the Vallenta Studio extension. It's the single place to see what's planned and in progress; for features that have already shipped, see the [Changelog](CHANGELOG.md) and the [Feature Matrix](docs/feature-matrix.md).

This roadmap is **subject to change** and intentionally carries **no committed delivery dates** — priorities shift as the extension evolves and in response to user feedback.

**Status:** 📋 Planned · ✅ Recently shipped

## 📋 Planned

- **Delphi Breakpoints view** — A dedicated breakpoints panel that groups breakpoints by project and file, with custom labels and bulk enable / disable / delete — a clearer alternative to the generic Breakpoints list.
- **Unused units diagnostic** — An optional hint that fades `uses`-clause units never referenced in a file, making dead unit references easy to spot and remove. Off by default, with configurable severity and an ignore list for side-effect units.
- **Symbols for source-less units (DCU)** — Hover, completion, and go-to-definition for units that ship only as compiled `.dcu` — RTL/VCL/FMX without source, and closed-source third-party libraries.
- **macOS (Apple Silicon)** — Editing and IntelliSense — hover, completion, go-to-definition, Find All References, diagnostics, and semantic highlighting — on Apple Silicon Macs. Building and debugging remain Windows-only.
- **Attach to a running process** — Attach the debugger to an already-running Windows process by its process ID, with breakpoints, stepping, and Watch.
- **MCP interface for AI assistants** — Expose Vallenta Studio's Delphi code intelligence to AI tools that speak the Model Context Protocol — symbol resolution, Find All References, and go-to-definition, plus reading the active project and its configuration, and starting a build and reading back its errors and warnings — so an AI assistant works from the IDE's real understanding of your code instead of re-parsing it.
- **Code formatter** — Format your Pascal source with a choice of engine: a built-in formatter (the default — works offline, with no extra install, and the only one that can format just the selected lines) or an external command-line formatter you already use (pasfmt, Jedi Code Format or Free Pascal's ptop). Format the whole document or a selection, with optional format-on-save. A later update will add extensive style options — per-block indentation, one-line `begin`/`end` handling, casing, spacing, and alignment.
- **Rename a unit** — Rename a unit and have everything update in one step: the `.pas` file and its companion `.dfm`/`.fmx` form file, the `unit` header, every `uses` reference across your `.pas`, `.dpr` and `.dpk` files, and the project file. Works whether you rename from the editor (`F2` on the unit name) or by renaming the file itself, and it keeps each reference's existing style — a short `Forms` stays short, a qualified `Vcl.Forms` stays qualified. Builds on Find unit references.

## ✅ Recently shipped

A few highlights — see the [Changelog](CHANGELOG.md) for the full history.

- **Locals in optimized builds** — Local variables, parameters and `Self` now show while debugging optimization-compiled builds, where many previously failed to display.
- **LSP cache folder** — The offline symbol cache for your project moves into a single hidden `.vsdcache` folder, instead of placing several cache files next to each `.dproj`. 
- **Rename Symbol** — Rename a symbol and have every reference updated across the project in a single step, using the native VS Code Rename (`F2`) command, including references inside `.dfm`/`.fmx` form files.
- **Semantic highlighting** — Resolver-driven coloring of types, methods, properties, parameters, and enum members, plus same-identifier occurrence highlighting and structural begin/end coloring.
- **Find All References** — Scope-aware reference search across the project, including matches inside `.dfm`/`.fmx` form files.
- **Find unit references** — Locate every place a unit is referenced across `uses` clauses (in `.pas`, `.dpr`/`.dpk`, and the unit header), matched by physical unit identity rather than name. A first step toward renaming units.
- **Semantic color themes editor** — A visual editor for recoloring semantic tokens without hand-editing JSON.

## Have a request?

Found something missing? [Open a feature request](../../issues/new/choose) — community input helps shape what gets prioritized next.
