# Vallenta Studio Feature Matrix

Overview of features available at each subscription tier.

## Subscription Tiers

| Tier | Who | Description |
|------|-----|-------------|
| **Not Registered** | Extension installed, no account | Project management, full build support, syntax and structural highlighting |
| **Free** | Registered account, no active subscription | Adds the language server: code completion, hover, navigation and Find All References |
| **Pro** | Active subscription (Trial, Pro, or Beta) | Adds debugging, refactoring, diagnostics, the Linux platform and the MCP server |

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
| Reorder projects and project groups | yes | yes | yes |
| Add existing file / remove from project | yes | yes | yes |
| Add a project from its `.dpr` / `.dpk` | yes | yes | yes |
| Open the current file in the Delphi IDE | yes | yes | yes |
| New Delphi project | — | yes | yes |

### Build System

| Feature | Not Registered | Free | Pro |
|---------|:--------------:|:----:|:---:|
| Build / Clean / Rebuild | yes | yes | yes |
| Build Toolbar (config & platform) | yes | yes | yes |
| Build Output panel (Errors, Warnings, Hints tabs) | yes | yes | yes |
| Jump to the first error | yes | yes | yes |
| Save modified files before a build | yes | yes | yes |
| Build task integration | yes | yes | yes |
| Run without debugging (Ctrl+F5) | yes | yes | yes |
| Group builds | — | — | yes |
| EurekaLog post-processing | — | — | yes |
| Linux64 build (links against a distribution sysroot) | — | — | yes |

### Editor & Highlighting

| Feature | Not Registered | Free | Pro |
|---------|:--------------:|:----:|:---:|
| Syntax highlighting | yes | yes | yes |
| Structural highlighting (nesting-level colors, matching keywords, flow-control) | yes | yes | yes |
| Semantic highlighting (colors identifiers by meaning) | — | yes | yes |
| Same-identifier occurrence highlighting | — | yes | yes |
| Inactive region decoration (grayed-out ifdef regions) | — | yes | yes |
| Hover information (signature, visibility, declaring unit, overloads) | — | yes | yes |
| Document Symbols (Outline & breadcrumbs) | — | yes | yes |
| Highlight color editor (custom semantic & structural colors) | — | — | yes |

### Code Completion & Documentation

| Feature | Not Registered | Free | Pro |
|---------|:--------------:|:----:|:---:|
| Code completion (identifiers, members, context-aware) | — | yes | yes |
| Completion for Delphi's intrinsic routines | — | yes | yes |
| Completion details pane (signature, visibility, overloads, documentation) | — | yes | yes |
| Parameter hints with overload selection | — | yes | yes |
| XML documentation (`///`) in hover, completion and parameter hints | — | yes | yes |

### Navigation & Search

| Feature | Not Registered | Free | Pro |
|---------|:--------------:|:----:|:---:|
| Toggle Form / Source (F12) | yes | yes | yes |
| Go to Interface Section / Go to Implementation Section | yes | yes | yes |
| Go to Definition, with a configurable interface / implementation target | — | yes | yes |
| Overload resolution (hover & Go-to-Definition) | — | yes | yes |
| Find Symbol (workspace picker, Ctrl+T) | — | yes | yes |
| Find All References (Shift+F12, includes DFM/FMX) | — | yes | yes |
| Find All References — base declaration | — | yes | yes |
| Find unit references (`uses` clauses, unit header, `.dpr`/`.dpk`) | — | yes | yes |
| Find All Implementations (Ctrl+F12) | — | yes | yes |
| Go to Declaration (Shift+Ctrl+Up) | — | — | yes |
| Go to Implementation (Shift+Ctrl+Down) | — | — | yes |

### Code Generation & Refactoring

| Feature | Not Registered | Free | Pro |
|---------|:--------------:|:----:|:---:|
| Insert GUID (Ctrl+Shift+G) | yes | yes | yes |
| Class completion — generate method bodies (Ctrl+Shift+C) | — | yes | yes |
| Apply Signature between declaration and implementation (Ctrl+Shift+Alt+C) | — | yes | yes |
| Rename Symbol (project-wide rename, F2, includes DFM/FMX) | — | — | yes |
| Rename Unit (file, header, every `uses` reference, `.dproj`) | — | — | yes |

### Diagnostics & Quick Fixes

| Feature | Not Registered | Free | Pro |
|---------|:--------------:|:----:|:---:|
| Semantic validation & diagnostics | — | — | yes |
| Unused units hint, with an editable ignore list | — | — | yes |
| Unused variables, constants & parameters hint | — | — | yes |
| Signature mismatch hint | — | — | yes |
| Quick fixes (add a missing unit, remove an unused unit) | — | — | yes |

### Debugging

| Feature | Not Registered | Free | Pro |
|---------|:--------------:|:----:|:---:|
| Debug launch (cppvsdbg) | — | — | yes |
| Attach to a running process | — | — | yes |
| Symbol conversion (TDS to PDB), on demand or after every build | — | — | yes |
| Natvis type visualizers | — | — | yes |
| Delphi-syntax expressions in Watch, hover, the Debug Console and breakpoint conditions | — | — | yes |
| Watch / hover evaluation (call methods, read properties incl. inherited, inspect interfaces, helper members) | — | — | yes |
| Variables view in Delphi form, with `[Fields]` and `[Properties]` nodes | — | — | yes |
| Interface variables resolved to the implementing class | — | — | yes |
| Locals in optimization-compiled builds | — | — | yes |
| String list viewer, value as date/time, Copy Address, Copy Variable as Tree | — | — | yes |
| Breakpoint persistence (per project) | — | — | yes |
| Delphi exception handling (break on raise, unwind Delphi stack) | — | — | yes |
| Delphi Exception Filters (skip exception types by name/pattern) | — | — | yes |
| Per-configuration debugger environment variables | — | — | yes |

### Linux Platform

| Feature | Not Registered | Free | Pro |
|---------|:--------------:|:----:|:---:|
| Linux64 remote debugging (breakpoints, stepping, call stack, locals, Watch) | — | — | yes |
| Linux Targets panel (pair a machine with VallentaAgent) | — | — | yes |
| Linux Distributions panel (prepare a sysroot, or import one from a target) | — | — | yes |
| Debug engine download | — | — | yes |
| Per-configuration deployment settings | — | — | yes |

### Form Designer

| Feature | Not Registered | Free | Pro |
|---------|:--------------:|:----:|:---:|
| Visual `.dfm` editing (component palette, object inspector, event handlers) | — | yes¹ | yes |
| Third-party component packages from the Delphi installation | — | yes¹ | yes |

¹ Free while the public beta runs; a Pro feature once the beta ends.

### MCP Server for AI Agents

| Feature | Not Registered | Free | Pro |
|---------|:--------------:|:----:|:---:|
| Code intelligence and build tools (17) | — | — | yes |
| Debugging tools (23) | — | — | yes |
| VS Code auto-discovery and one-click Claude Code registration | — | — | yes |

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
| Keyboard Mappings editor (with the Delphi keymap) | yes | yes | yes |
| Project Options editor (.dproj properties) | — | — | yes |
| Option set (`.optset`) editor | — | — | yes |
| Custom Delphi settings (own library and search paths) | — | — | yes |

### Session & Workflow

| Feature | Not Registered | Free | Pro |
|---------|:--------------:|:----:|:---:|
| Remember open tabs and breakpoints (per project, per group, or off) | — | yes | yes |
| LSP server control (start / stop / restart) | — | yes | yes |
| LSP auto-start | — | yes | yes |
| LSP Debug panel | — | yes | yes |

## Summary

- **Not Registered** — Syntax and structural highlighting, project management, and full build support. Enough to evaluate the extension with your projects.
- **Free** — Register for a free account to unlock the language server: code completion with parameter hints and `///` documentation, hover, Outline, semantic highlighting, inactive region visualization, Find Symbol, Find All References and Find All Implementations, class completion and Apply Signature, file creation, encoding conversion, and session persistence.
- **Pro** — Subscribe to unlock the full IDE experience: debugging on Windows and Linux64 with Delphi-syntax expressions, attach to a running process, project-wide rename of symbols and units, semantic validation with unused-code hints and quick fixes, Go to Declaration and Go to Implementation, the highlight color editor, the project options and option-set editors, custom Delphi settings, EurekaLog post-processing, group builds, and the MCP server for AI agents.
