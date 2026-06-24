# Vallenta Studio — Roadmap

A forward-looking list of features planned for the Vallenta Studio extension. It's the single place to see what's planned and in progress; for features that have already shipped, see the [Changelog](CHANGELOG.md) and the [Feature Matrix](docs/feature-matrix.md).

This roadmap is **subject to change** and intentionally carries **no committed delivery dates** — priorities shift as the extension evolves and in response to user feedback.

**Status:** 📋 Planned · ✅ Recently shipped

## 📋 Planned

- **Delphi Breakpoints view** — A dedicated breakpoints panel that groups breakpoints by project and file, with custom labels and bulk enable / disable / delete — a clearer alternative to the generic Breakpoints list.
- **Find unit references** — Locate every place a unit is referenced across `uses` clauses (in `.pas`, `.dpr`/`.dpk`, and the unit header), matched by physical unit identity rather than name. A first step toward renaming units.
- **Unused units diagnostic** — An optional hint that fades `uses`-clause units never referenced in a file, making dead unit references easy to spot and remove. Off by default, with configurable severity and an ignore list for side-effect units.
- **Symbols for source-less units (DCU)** — Hover, completion, and go-to-definition for units that ship only as compiled `.dcu` — RTL/VCL/FMX without source, and closed-source third-party libraries.
- **Locals in optimized builds** — Show local variables, parameters, and `Self` while debugging optimization-compiled builds, where they currently fail to display.
- **macOS (Apple Silicon)** — Editing and IntelliSense — hover, completion, go-to-definition, Find All References, diagnostics, and semantic highlighting — on Apple Silicon Macs. Building and debugging remain Windows-only.
- **Attach to a running process** — Attach the debugger to an already-running Windows process by its process ID, with breakpoints, stepping, and Watch.
- **MCP interface for AI assistants** — Expose Vallenta Studio's Delphi code intelligence to AI tools that speak the Model Context Protocol — symbol resolution, Find All References, and go-to-definition, plus reading the active project and its configuration, and starting a build and reading back its errors and warnings — so an AI assistant works from the IDE's real understanding of your code instead of re-parsing it.

## ✅ Recently shipped

A few highlights — see the [Changelog](CHANGELOG.md) for the full history.

- **Rename Symbol** — Rename a symbol and have every reference updated across the project in a single step, using the native VS Code Rename (`F2`) command, including references inside `.dfm`/`.fmx` form files.
- **Semantic highlighting** — Resolver-driven coloring of types, methods, properties, parameters, and enum members, plus same-identifier occurrence highlighting and structural begin/end coloring.
- **Find All References** — Scope-aware reference search across the project, including matches inside `.dfm`/`.fmx` form files.
- **Semantic color themes editor** — A visual editor for recoloring semantic tokens without hand-editing JSON.

## Have a request?

Found something missing? [Open a feature request](../../issues/new/choose) — community input helps shape what gets prioritized next.
