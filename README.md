<p align="center"><img src="media/VSD_Logo128x128.png" width="128" alt="Vallenta Studio"></p>

# Vallenta Studio

A Visual Studio Code extension that provides comprehensive Delphi development support, enabling you to edit, build, and debug Delphi projects directly within VS Code.

## Features

The extension is available in two tiers. See the [full feature matrix](docs/feature-matrix.md) for a tier-by-tier comparison.

### Free (with registered account)

- **Project Explorer** - Manage multiple Delphi projects (.dproj) and packages (.dpk) in a dedicated sidebar with recent project support
- **Build System** - Build, clean, and rebuild projects using MSBuild with configuration/platform selection and build toolbar. Compiler errors, warnings, and hints are parsed into the Problems panel
- **Syntax Highlighting** - Full Object Pascal TextMate grammar including keywords, types, comments, strings, and compiler directives
- **Code Intelligence (LSP)** - Go to Definition, hover information, code completion, and document symbols/outline powered by a built-in Language Server
- **Semantic Highlighting** - Context-aware syntax coloring via the LSP server
- **Preprocessor Support** - Full `{$IFDEF}`, `{$DEFINE}`, `{$INCLUDE}` evaluation with inactive region visualization (grayed-out code)
- **File Encoding** - Detects ANSI-encoded Pascal files and offers one-click conversion to UTF-8 with BOM
- **Session Persistence** - Saves and restores open tabs when switching between projects

### Pro (with paid subscription)

All Free features, plus:

- **Source-Level Debugging** - Full source-level debugging with breakpoints, stepping, variable inspection, and call stacks. Press `F5` to start — no configuration needed.
- **Natvis Type Visualization** - Version-specific natvis files for displaying Delphi types (strings, arrays, variants, objects) in the debugger
- **Go to Declaration / Implementation** - Jump between interface declarations and implementation bodies (`Shift+Ctrl+Up` / `Shift+Ctrl+Down`)
- **Semantic Validation** - Compiler-style diagnostics that detect undefined types, methods, and variables with configurable severity
- **Breakpoint Persistence** - Breakpoints are saved and restored per project
- **Project Options Editor** - Edit project configuration directly within VS Code
- **Group Builds** - Build all projects in a project group at once

## Requirements

- **Delphi** - A valid Delphi installation (Delphi 10.x, 11.x, 12.x or 13.x)
- **C/C++ Extension** - Required for debugging ([ms-vscode.cpptools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)). VS Code installs this automatically when you install the Vallenta Studio extension.

## Installation

1. Install the extension from the [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=Vallenta.vallenta-studio)
2. Open a folder in VS Code to use as your workspace (see [Workspace Setup](#workspace-setup) below)
3. The extension auto-detects your Delphi installation and configures paths automatically

## Getting Started

### Workspace Setup

The extension requires an open **workspace folder** in VS Code. It stores project references and settings in a `.vallenta` folder inside your workspace root.

The workspace folder does **not** have to be the actual Delphi project folder. The recommended practice is to open a **parent directory** that covers multiple Delphi project folders. For example:

```
C:\MyWork\                    <-- open this folder as your VS Code workspace
+-- .vallenta\                <-- extension stores project list and settings here
+-- ProjectA\
¦   +-- ProjectA.dproj
¦   +-- ...
+-- ProjectB\
¦   +-- ProjectB.dproj
¦   +-- ...
+-- SharedLibrary\
    +-- ...
```

This way you can manage multiple Delphi projects within a single workspace.

### Delphi Detection

On first startup, it scans the Windows Registry for installed Delphi versions and selects the latest one. To switch to a different Delphi version, open the settings dialog (top right on the Project Explorer) and select the Delphi version from the dropdown list.

### Adding Projects

1. Open the **Vallenta Studio** view in the Activity Bar (sidebar)
2. Click **Add Project** to browse and select a `.dproj` or `.dpk` file
3. Or use **Add Project Group** to load all projects from a `.groupproj` file

### Building

1. Set the active project by clicking on it in the Project Explorer
2. Select the build configuration (Debug/Release) and target platform (Win32/Win64) in the Build toolbar
3. Click the **Build** button in the Build toolbar — one click, no VS Code `tasks.json` configuration required
4. Build output streams in real-time to the Build Output panel; errors and warnings appear in the Problems panel

### Debugging (Pro)

1. Build the active project with **Debug** configuration to include debug information
2. Press `F5` (or click **Debug** in the Build toolbar) to start debugging

That's it — no `launch.json` or `tasks.json` needed. The extension automatically:
- Generates the debug symbols required by the debugger
- Generates version-specific natvis visualizers for Delphi types
- Launches the debugger with the correct paths and settings

Use `Ctrl+F5` to run without debugging.

**Debugging capabilities:**
- Source-level breakpoints
- Step over / into / out
- Local variable and parameter inspection
- Call stacks with function names and line numbers
- Natvis type visualization for Delphi strings, arrays, variants, and objects

### Code Intelligence

The built-in LSP server starts automatically when a project is activated and provides:

- **Go to Definition** (`F12`) - Navigate to symbol declarations
- **Go to Declaration / Implementation** (`Shift+Ctrl+Up` / `Shift+Ctrl+Down`) - Jump between interface and implementation (Pro)
- **Hover** - View type information and symbol details
- **Code Completion** - Type-aware member suggestions after the dot operator
- **Document Symbols** - Outline view and breadcrumb navigation
- **Semantic Highlighting** - Context-aware syntax coloring
- **Diagnostics** - Errors, warnings, and hints for undefined types, methods, and variables (Pro)
- **Inactive Regions** - Preprocessor-excluded code shown with reduced opacity

## Extension Settings

### Vallenta Studio Environment

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `vallenta.studio.installationPath` | string | `""` | Path to the Delphi installation directory |
| `vallenta.studio.selectedVersion` | string | `""` | Selected Delphi version identifier (e.g., '23.0') |
| `vallenta.studio.msbuildPath` | string | `""` | Path to MSBuild.exe |
| `vallenta.studio.rsvarsPath` | string | `""` | Path to rsvars.bat |
| `vallenta.studio.libraryPath` | array | `[]` | Delphi library search paths |
| `vallenta.studio.bplOutputPath` | string | `""` | Output directory for BPL files |
| `vallenta.studio.dcpOutputPath` | string | `""` | Output directory for DCP files |
| `vallenta.studio.compilerPath` | string | `""` | Path to the Delphi compiler (DCC32/DCC64) |

### Debugging

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `vallenta.studio.debug.enableNatvis` | boolean | `true` | Generate natvis visualizer for Delphi types during debugging |

### Editor

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `vallenta.studio.encodingCheck` | boolean | `true` | Warn about ANSI-encoded Pascal files and offer conversion |
| `vallenta.studio.rememberOpenFiles` | boolean | `true` | Save/restore open tabs when switching projects |
| `vallenta.studio.inactiveRegions.enabled` | boolean | `true` | Show inactive preprocessor regions with reduced opacity |

### LSP Server

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `vallenta.studio.lsp.serverPath` | string | `""` | Path to Vallenta_LSP.exe (empty = bundled server) |
| `vallenta.studio.lsp.autostart` | string | `"exclusive"` | Auto-start mode: `disabled`, `exclusive`, or `enabled` |
| `vallenta.studio.lsp.trace.server` | string | `"off"` | LSP communication trace level: `off`, `messages`, `verbose` |
| `vallenta.studio.lsp.logLevel` | string | `"info"` | Server log verbosity: `trace`, `debug`, `info`, `warning`, `error` |
| `vallenta.studio.lsp.idleTimeout` | number | `10` | Minutes before stopping idle background LSP servers (0 = disabled) |
| `vallenta.studio.lsp.preprocessorAwareParsing` | boolean | `true` | Ignore inactive `{$IFDEF}` regions during syntax checking |
| `vallenta.studio.lsp.semanticValidation.enabled` | boolean | `true` | Detect undefined types, methods, and variables |
| `vallenta.studio.lsp.semanticValidation.severity` | string | `"hint"` | Severity for semantic diagnostics: `error`, `warning`, `hint`, `information` |
| `vallenta.studio.lsp.indexing.workerThreads` | number | `0` | Background indexing threads (0 = auto) |
| `vallenta.studio.lsp.indexing.batchSize` | number | `8` | Files per batch per worker thread |

## Commands

| Command | Description |
|---------|-------------|
| `Vallenta Studio: Auto-Detect Installation` | Scan registry for Delphi installations |
| `Vallenta Studio: Open Vallenta Studio Settings` | Open the extension settings UI |
| `Vallenta Studio: Add Project File...` | Add a .dproj or .dpk file to the project list |
| `Vallenta Studio: Add Projects from Folder...` | Add all projects from a folder |
| `Vallenta Studio: Add Project Group...` | Add all projects from a .groupproj file |
| `Vallenta Studio: Open Recent Project...` | Open a recently used project |
| `Vallenta Studio: New Delphi-File...` | Create a new Pascal unit file |
| `Vallenta Studio: Project Options...` | Open project configuration (Pro) |
| `Vallenta Studio: Build` | Build the active project |
| `Vallenta Studio: Clean` | Clean the active project |
| `Vallenta Studio: Rebuild` | Rebuild the active project |
| `Vallenta Studio: Build All Projects in Group` | Build all projects in a group (Pro) |
| `Vallenta Studio: Cancel Build` | Cancel the active build |
| `Vallenta Studio: Run Without Debugging` | Run the application without debugger |
| `Vallenta Studio: Start LSP Server` | Start the language server for the active project |
| `Vallenta Studio: Stop LSP Server` | Stop the language server |
| `Vallenta Studio: Restart LSP Server` | Restart the language server |
| `Vallenta Studio: Go to Declaration` | Navigate to interface declaration (Pro) |
| `Vallenta Studio: Go to Implementation` | Navigate to implementation body (Pro) |
| `Vallenta Studio: Convert to UTF-8 with BOM` | Convert ANSI file to UTF-8 BOM encoding |

## Keyboard Shortcuts

| Shortcut | Action | Context |
|----------|--------|---------|
| `F5` | Start debugging | Active Delphi project (Pro) |
| `Ctrl+F5` | Run without debugging | Active Delphi project |
| `F12` | Go to Definition | Pascal editor |
| `Shift+Ctrl+Up` | Go to Declaration | Pascal editor (Pro) |
| `Shift+Ctrl+Down` | Go to Implementation | Pascal editor (Pro) |
| `Ctrl+Shift+B` | Build | Standard VS Code build |

## Supported File Types

| Extension | Description |
|-----------|-------------|
| `.pas` | Pascal unit |
| `.dpr` | Delphi project source |
| `.dpk` | Delphi package source |
| `.inc` | Include file |
| `.dproj` | Delphi project file (XML) |
| `.groupproj` | Delphi project group file (XML) |
| `.dfm` | Delphi form file (text format) |
| `.fmx` | FireMonkey form file (text format) |

## Known Limitations

- Form Designer is not supported — .dfm and .fmx files open as text
- Windows only — the extension requires a local Delphi installation on Windows. Cross-compilation to other platforms (e.g., Linux) is possible, but deployment to target systems is not handled by the extension
- LSP does not yet support: Find References, Rename, Code Actions, Code Formatting, Signature Help

## Feedback and Issues

Found a bug or have a feature request? Please [open an issue](../../issues/new/choose) on this repository.

## Trademark Notice

Delphi is a registered trademark of Embarcadero Technologies, Inc. 
Vallenta Studio is an independent product and is not affiliated with, authorized, or endorsed by Embarcadero Technologies.

## License

See [LICENSE](LICENSE) for details.
