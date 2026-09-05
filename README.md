# Vallenta Studio

[![Latest release](https://badgen.net/github/release/Vallenta/Studio/stable)](https://github.com/Vallenta/Studio/releases/latest)

**[Features](#features) · [Roadmap](ROADMAP.md) · [Changelog](CHANGELOG.md) · [Report an issue](../../issues/new/choose)**

A Visual Studio Code extension that provides comprehensive Delphi development support, enabling you to edit, build, and debug Delphi projects directly within VS Code.

## Features

The extension is available in two tiers. See the [full feature matrix](docs/feature-matrix.md) for a tier-by-tier comparison.

### Free (with registered account)

- **Project Explorer** - Manage multiple Delphi projects (.dproj) and packages (.dpk) in a dedicated sidebar with recent project support
- **Build System** - Build, clean, and rebuild projects using MSBuild with configuration/platform selection and build toolbar. Compiler errors, warnings, and hints are parsed into the Problems panel
- **Syntax Highlighting** - Full Object Pascal TextMate grammar including keywords, types, comments, strings, and compiler directives
- **Code Intelligence (LSP)** - Go to Definition, hover information, code completion, and document symbols/outline powered by a built-in Language Server
- **Find Symbol** - Workspace symbol picker (`Ctrl+T` or editor right-click "Find Symbol…") for classes, records, interfaces, methods, properties, and unit-level symbols
- **Find All References** - Semantic, scope-aware reference search (`Shift+F12`) across the project, including matches inside `.dfm`/`.fmx` form files
- **Toggle Form / Source** - Press `F12` (or use the CodeLens at the top of the file) to swap between a `.pas` unit and its sibling `.dfm`/`.fmx` form. The Source Files view shows an inline `[DFM]`/`[FMX]` tag for units with a form
- **Semantic Highlighting** - Resolver-driven coloring that distinguishes identifiers by meaning — types, classes, interfaces, methods, properties, fields, parameters, and enum members — plus same-identifier occurrence highlighting
- **Structural Highlighting** - Block keywords (begin/end, if, for, case, try…) colored by nesting level; the whole construct highlights when your cursor is on one of its keywords, and control-flow keywords (exit, break, continue, raise) are emphasized
- **Preprocessor Support** - Full `{$IFDEF}`, `{$DEFINE}`, `{$INCLUDE}` evaluation with inactive region visualization (grayed-out code)
- **File Encoding** - Detects ANSI-encoded Pascal files and offers one-click conversion to UTF-8 with BOM
- **Session Persistence** - Saves and restores open tabs when switching between projects

### Pro (with paid subscription)

All Free features, plus:

- **Source-Level Debugging** - Full source-level debugging with breakpoints, stepping, variable inspection, and call stacks. Press `F5` to start — no configuration needed.
- **Watch & Hover Evaluation** - Call methods on your objects, read properties (including inherited ones), drill into interface and class references, and see sets, enums and Booleans in Delphi form — right from the Watch panel and on hover; a `[Properties]` node lists an object's getter-backed properties
- **Delphi Exception Handling** - Debugger breaks on raised Delphi exceptions and unwinds the Delphi exception stack, showing the full call path back to the `raise` site merged with the native frames, and a **Delphi Exception Filters** view lets you skip chosen exception types by name or pattern
- **Natvis Type Visualization** - Version-specific natvis files for displaying Delphi types (strings, arrays, variants, objects) in the debugger
- **Go to Declaration / Implementation** - Jump between interface declarations and implementation bodies (`Shift+Ctrl+Up` / `Shift+Ctrl+Down`)
- **Semantic Validation** - Compiler-style diagnostics that detect undefined types, methods, and variables with configurable severity
- **Rename Symbol** - Rename a symbol with `F2` and every use updates across the project in one step — declaration, implementation, and all references, including inside `.dfm`/`.fmx` forms. Unsafe or conflicting renames are refused with a clear explanation
- **Highlight Color Editor** - Recolor semantic and structural highlighting with visual color-picker dialogs and a live preview, instead of hand-editing JSON
- **Breakpoint Persistence** - Breakpoints are saved and restored per project
- **Project Options Editor** - Edit project configuration directly within VS Code, including per-configuration build settings, per-configuration/platform debugger environment variables, and a visual editor for referenced option sets (`.optset`)
- **Copy Variable as Tree** - Right-click in the debugger **Variables** or **Watch** view to copy the expanded variable hierarchy to the clipboard (up to 2 levels deep, 2000 nodes)
- **Group Builds** - Build all projects in a project group at once
- **Linux Remote Debugging** - Build `Linux64` on Windows, deploy to a Linux machine and debug it there with breakpoints, stepping and watch evaluation. Machines are paired once and referenced by name; per build configuration you choose the target, the working directory and the files deployed alongside the program

## Requirements

- **Delphi** - A valid Delphi installation (Delphi 10.x, 11.x, 12.x or 13.x)
- **C/C++ Extension** - Required for debugging ([ms-vscode.cpptools](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)). VS Code installs this automatically when you install the Vallenta Studio extension.

## Installation

1. Install the extension from the [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=VallentaStudio.vallenta-studio)
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

#### Information regarding Delphi 2005 - XE8

Vallenta Studio detects Delphi installations from Delphi 2005 onwards. However, only versions 10 - 13 Florence have been officially tested. If you use Delphi 2005 - XE8, the extension may work. Please report any issues you encounter so we can improve support.

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
- Copy expanded variable trees to the clipboard via right-click in Variables / Watch
- **Delphi exception handling** — the debugger automatically breaks on raised Delphi exceptions (identified by the `0x0EEDFADE` exception code), reads the exception class name and message, and unwinds the Delphi exception stack so the call stack shows the full path back to the `raise` site merged with the native C++ frames. Enabled by default via the *All Delphi Exceptions* breakpoint filter in the **Breakpoints** view; toggle *All Exceptions* there to also break on non-Delphi (SEH) exceptions

### Linux Development (Pro)

A project with the `Linux64` platform is compiled on Windows by Delphi's Linux compiler, then
deployed to and debugged on a Linux machine. Three things are set up once; after that `F5`
behaves as it does on Windows.

**1. Run the agent on the Linux machine.** Remote debugging is driven by **VallentaAgent**, one
static binary that needs no root and installs nothing outside the user's home directory:

```
curl -fsSL https://github.com/vallenta/VallentaAgent/releases/latest/download/install.sh | sh
```

*Vallenta Studio: Copy Linux Agent Install Command* puts that line on the clipboard. The agent
prints a pairing token when it starts. `lldb-server` is not bundled — the distribution supplies
it, and the agent names the package to install for the distribution it detects.

**2. Register the machine.** *Vallenta Studio: Add Linux Target* takes the host, port and printed
token and pairs once. Registered machines are listed in the **Linux Targets** panel with what each
one reported: agent version, `lldb-server` version, distribution and glibc. Targets belong to the
workspace and are referenced by name, so no host names or ports are written into project files.

**3. Provide a sysroot.** A `Linux64` link needs the target distribution's libraries. The **Linux
Distributions** panel downloads and prepares them per distribution, and the target's distribution
decides which one a build links against. A project that carries its own `DCC_SysLibRoot`, or a
machine with a registered Delphi Linux SDK, uses that instead.

The debug engine is fetched on demand: the extension matches `lldb-dap` to the `lldb-server`
version the target reports, so both ends speak the same protocol.

#### Deployment Options

Open **Deployment Options** from a project's context menu in the Projects view. The editor is
scoped by build configuration: pick a configuration on the left, and the settings on the right
apply to it. *All configurations* holds the values every configuration inherits, and a
configuration that sets nothing of its own shows the inherited value greyed out.

Per configuration you set:

- **Linux target** — the machine to deploy to. It also supplies the sysroot the `Linux64` build
  links against, so changing it changes what the project compiles against. The indicator beside
  it shows whether the target answered; the check runs when you open the editor, switch
  configuration or pick a target.
- **Working directory** — where the program is placed and run, relative to the agent's scratch
  root. It defaults to the project name.
- **Deployed files** — files copied to the target before the program starts. Add single files, a
  folder, or a pattern such as `data/**`; a folder or pattern keeps its structure relative to the
  project directory. Per entry you can set a subfolder, a different remote name, whether an
  existing file is overwritten, and whether the file is made executable.

The **Live Target View** at the bottom shows the resulting layout on the target, including the
program itself, which the debugger uploads without an entry. A file that cannot be deployed — a
path matching nothing, or one outside the project directory — is reported there and stops the
session before the target is contacted.

Projects that already carry Delphi's own deployment entries can take them over with **Import from
Delphi**. The import is one-time and additive, covers the `Linux64` entries only, and reports
anything it skipped.

#### Debugging on Linux

`F5` builds the project if needed, deploys it, and starts the session. Breakpoints, stepping,
call stacks and variable inspection work as they do on Windows, including Delphi strings, dynamic
arrays, sets, records, classes and interfaces. Delphi exceptions break at the raise site with the
class name and message. Program output goes to an integrated terminal, so a program that reads
from standard input can be driven from it.

Source files stay on Windows — nothing but the program and the files you list is copied to the
target.

### Code Intelligence

The built-in LSP server starts automatically when a project is activated and provides:

- **Go to Definition** (`Ctrl+Click`) - Navigate to symbol declarations
- **Go to Declaration / Implementation** (`Shift+Ctrl+Up` / `Shift+Ctrl+Down`) - Jump between interface and implementation (Pro)
- **Find Symbol** (`Ctrl+T`) - Workspace-wide symbol search
- **Find All References** (`Shift+F12`) - Semantic reference search across the project, including matches inside `.dfm`/`.fmx` form files
- **Overload Resolution** - Hover and `Ctrl+Click` on an overloaded routine narrow to the specific overload by matching argument types
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

### Debugging

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| `vallenta.studio.debug.enableNatvis` | boolean | `true` | Generate natvis visualizer for Delphi types during debugging |

### Linux (Pro)

Targets and distributions are managed by their panels rather than edited by hand.

| Setting | Type | Scope | Description |
|---------|------|-------|-------------|
| `vallenta.studio.linux.targets` | array | workspace | Paired Linux machines, referenced by name from a project |
| `vallenta.studio.linux.distros` | array | machine | Sysroots a `Linux64` build can link against |
| `vallenta.studio.linux.lldbDapPath` | string | — | Use a specific `lldb-dap` instead of the one matched to the target |
| `vallenta.studio.linux.trace` | boolean | — | Log the Linux debug adapter's traffic to the *Delphi Linux Debug* output channel |

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
| `Vallenta Studio: Deployment Options` | Open the Linux deployment editor for the active project (Pro) |
| `Vallenta Studio: Add Linux Target` | Pair a Linux machine running VallentaAgent (Pro) |
| `Vallenta Studio: Linux Targets` | Manage the paired Linux machines (Pro) |
| `Vallenta Studio: Linux Distributions` | Manage the sysroots a `Linux64` build links against (Pro) |
| `Vallenta Studio: Copy Linux Agent Install Command` | Copy the agent install command to the clipboard (Pro) |
| `Vallenta Studio: Download Linux Debug Engine` | Fetch the `lldb-dap` matching a target's `lldb-server` (Pro) |
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
| `Vallenta Studio: Find Symbol...` | Open workspace-wide symbol picker |
| `Vallenta Studio: Toggle Form / Source` | Swap between `.pas` and matching `.dfm`/`.fmx` form file |
| `Vallenta Studio: Copy Variable as Tree` | Copy expanded debugger variable to clipboard (Pro, debug context only) |
| `Vallenta Studio: Convert to UTF-8 with BOM` | Convert ANSI file to UTF-8 BOM encoding |

## Keyboard Shortcuts

| Shortcut | Action | Context |
|----------|--------|---------|
| `F5` | Start debugging | Active Delphi project (Pro) |
| `Ctrl+F5` | Run without debugging | Active Delphi project |
| `F12` | Toggle Form / Source | Pascal / DFM / FMX editor |
| `Ctrl+Click` | Go to Definition | Pascal editor |
| `Ctrl+T` | Find Symbol | Workspace |
| `Shift+F12` | Find All References | Pascal editor |
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
- Windows only — the extension requires a local Delphi installation on Windows. `Linux64` projects are deployed to and debugged on a Linux machine running VallentaAgent; other target platforms can be cross-compiled, but deployment to them is not handled by the extension
- LSP does not yet support: Code Actions, Code Formatting, Signature Help

## Feedback and Issues

Found a bug or have a feature request? Please [open an issue](../../issues/new/choose) on this repository.

## Trademark Notice

Delphi is a registered trademark of Embarcadero Technologies, Inc. 
Vallenta Studio is an independent product and is not affiliated with, authorized, or endorsed by Embarcadero Technologies.

## License

See [LICENSE](LICENSE) for details.
