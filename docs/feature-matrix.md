# Vallenta Studio Feature Matrix

Overview of features available at each subscription tier.

## Subscription Tiers

| Tier | Who | Description |
|------|-----|-------------|
| **Not Registered** | Extension installed, no account | Basic project management and build |
| **Free** | Registered account, no active subscription | Core editing features and LSP basics |
| **Pro** | Active subscription (Trial, Pro, or Beta) | Full IDE experience with debugging |

## Feature Comparison

### Project Management

| Feature | Not Registered | Free | Pro |
|---------|:--------------:|:----:|:---:|
| Add / remove projects | yes | yes | yes |
| Set active project | yes | yes | yes |
| Project groups (.groupproj) | yes | yes | yes |
| Project Explorer sidebar | yes | yes | yes |
| Source Files view | yes | yes | yes |
| DFM/FMX form indicator in Source Files | yes | yes | yes |
| Recent projects | yes | yes | yes |

### Build System

| Feature | Not Registered | Free | Pro |
|---------|:--------------:|:----:|:---:|
| Build / Clean / Rebuild | yes | yes | yes |
| Build Toolbar (config & platform) | yes | yes | yes |
| Build Output panel | yes | yes | yes |
| Build task integration | yes | yes | yes |
| Run without debugging (Ctrl+F5) | yes | yes | yes |
| Group builds | — | — | yes |

### Editor & Language Features

| Feature | Not Registered | Free | Pro |
|---------|:--------------:|:----:|:---:|
| Syntax highlighting | yes | yes | yes |
| Semantic highlighting (colors identifiers by meaning) | — | yes | yes |
| Structural highlighting (nesting-level colors, matching keywords, flow-control) | yes | yes | yes |
| Highlight color editor (custom semantic & structural colors) | — | — | yes |
| Hover information (types, symbols) | — | yes | yes |
| Document Symbols (Outline & breadcrumbs) | — | yes | yes |
| Inactive region decoration (grayed-out ifdef regions) | — | yes | yes |
| Find Symbol (workspace picker, Ctrl+T) | — | yes | yes |
| Find All References (Shift+F12, includes DFM/FMX) | — | yes | yes |
| Toggle Form / Source (F12) | yes | yes | yes |
| Overload resolution (hover & Go-to-Definition) | — | yes | yes |
| Go to Declaration (Shift+Ctrl+Up) | — | — | yes |
| Go to Implementation (Shift+Ctrl+Down) | — | — | yes |
| Semantic validation & diagnostics | — | — | yes |
| Rename Symbol (project-wide rename, F2) | — | — | yes |

### Debugging

| Feature | Not Registered | Free | Pro |
|---------|:--------------:|:----:|:---:|
| Debug launch (cppvsdbg) | — | — | yes |
| Symbol conversion (TDS to PDB) | — | — | yes |
| Natvis type visualizers | — | — | yes |
| Watch / Hover expression evaluation (call methods, read properties incl. inherited, inspect interfaces) | — | — | yes |
| Breakpoint persistence (per project) | — | — | yes |
| Delphi exception handling (break on raise, unwind Delphi stack) | — | — | yes |
| Delphi Exception Filters (skip exception types by name/pattern) | — | — | yes |
| Copy Variable as Tree (Variables / Watch) | — | — | yes |
| Per-configuration debugger environment variables | — | — | yes |

### File Operations

| Feature | Not Registered | Free | Pro |
|---------|:--------------:|:----:|:---:|
| Encoding detection & warnings | yes | yes | yes |
| Convert to UTF-8 with BOM | — | yes | yes |
| New file creation (unit, VCL form, FMX form) | — | yes | yes |

### Project Configuration

| Feature | Not Registered | Free | Pro |
|---------|:--------------:|:----:|:---:|
| Settings UI | yes | yes | yes |
| Auto-detect Delphi installation | yes | yes | yes |
| Project Options editor (.dproj properties) | — | — | yes |

### Session & Workflow

| Feature | Not Registered | Free | Pro |
|---------|:--------------:|:----:|:---:|
| Remember open tabs per project | — | yes | yes |
| LSP server control (start / stop / restart) | — | yes | yes |
| LSP auto-start | — | yes | yes |
| LSP Debug panel | — | yes | yes |

## Summary

- **Not Registered** — Syntax and structural highlighting, project management, and full build support. Enough to evaluate the extension with your projects.
- **Free** — Register for a free account to unlock LSP-powered hover, outline, semantic highlighting, inactive region visualization, Find Symbol, Find All References, Toggle Form / Source, file creation, encoding conversion, and session persistence.
- **Pro** — Subscribe to unlock the full IDE experience: code navigation, project-wide rename, semantic validation, the highlight color editor, debugging with Watch/hover evaluation and PDB conversion, breakpoint persistence, Copy Variable as Tree, per-configuration debugger environment variables, project options editor, and group builds.
