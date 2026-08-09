# Changelog

All notable changes to the **Vallenta Studio** extension will be documented in this file.

## [1.1.23] - 2026-08-09

### Added
- **Own color for attributes** — attribute names (`[TestFixture]`, `[Setup]`) are now a semantic token type of their own, `attribute`, so they no longer borrow the color of whatever else shares the spelling. It joins the list in **Edit symbol colors…** on the Settings page (*Source Highlight Enhancements*), and can also be targeted as `attribute:pascal` in `editor.semanticTokenColorCustomizations`. The color applies to every attribute, including one whose class is not in your search path.
- **Debugger, show a string list as text** — a `TStrings` or `TStringList` row in **Variables** or **Watch** now offers a button (hover the row, or right-click it) that opens the list's full text in a read-only editor beside your code, with search and word wrap. Any descendant is covered, including the lists behind a `TMemo`, `TListBox` or `TComboBox`. The text is complete, where copying the row's value stops after about 256 characters. A row that offers it shows its type as `TStringList : TStrings`.

### Fixed
- **LSP, an attribute on a type or a member** — an attribute such as `[Setup]` or `[TestFixture]` now has a color of its own, and hover, Go to Definition and Ctrl+Click reach the attribute class it names. 
- **Rename Symbol on an attribute class** — renaming a class whose name ends with `Attribute` is now refused with an explanation, instead of silently producing source that no longer compiles. Because an attribute may be written without that suffix (`[Setup]` for `SetupAttribute`), the declaration and its uses are spelled differently, and only the declaration was being renamed. 
- **LSP, loop variable of a `for … in` loop over a derived container** — a container declared as its own class (`TMyList = class(TObjectList<TMyItem>)`) now gives the loop variable its type; it stayed unresolved before, because the element type is named in the base clause rather than on the variable. Covers any class derived from a generic container, at any depth. A container spelled with its type arguments was unaffected.
- **LSP, loop variable of a `for … in` loop over a container from another unit** — iterating a property or field whose type is an alias declared in another unit (`TMyList = TList<TMyItem>`) now types the loop variable. It came out as `Pointer`, or as no type at all, depending on which units the iterating unit imported.
- **LSP, a type name that also exists in a unit you do not use** — hover, Go to Definition and code completion now resolve a type name to the declaration that is actually in scope. A same-named type from an unrelated unit could win before, so hover could show the wrong kind of type and Go to Definition jump into a unit you never imported.
- **LSP, members inherited from a generic base class** — a class derived from a generic instantiation written with a space before the type arguments (`TDerived = class(TBase <Cardinal>)`) now sees the fields, methods and properties it inherits. `Self.Field` reported *Unknown member* and a call to an inherited method reported *Unknown identifier*. The spelling without the space was unaffected.
- **LSP, a type written with a space before its type arguments** — `TArray <Byte>` and the same spelling on any generic type no longer report *Unknown type*.
- **LSP, a generic type named together with its unit** — a type such as `System.Generics.Collections.TList<Integer>` no longer reports *Unknown type*, as a base class, a variable type or a field type. Only the spelling without the unit prefix resolved before; the prefix worked on non-generic types.
- **LSP, a mistyped member on a variable of a generic or unit-qualified type** — a member that does not exist now reports *Unknown member* on a variable declared with type arguments (`TList<Integer>`) or with its unit (`System.Classes.TStringList`). The typo went unreported before; only a variable of a plain, unqualified type was checked. Valid members always resolved and are unaffected.
- **LSP, false *never used* hint on an inline variable in a `try` block** — an inline variable declared inside a `try` block is no longer reported as unused when another `try` block in the same routine declares one of the same name. Rename and occurrence highlighting on such a variable now stay within their own block as well; `begin … end` blocks and `case` branches were unaffected.
- **Debugger, an interface property returning a number, `Boolean`, enum or set** — a property such as `FContext.RetryCount` or `FContext.IsActive` reached through an interface now shows its value in a **Watch** or on hover, instead of reporting *identifier "Integer" is undefined*. A `Boolean` reads as `True` / `False` and an enum as its enumerator. Covers a property reached through a chain (`FContext.Options.RetryCount`) on Win32 and Win64; interface properties returning a string or another interface were unaffected.
- **Debugger, a property on an interface declared `safecall`, `stdcall` or `cdecl`** — these now evaluate in a **Watch** and on hover. A `safecall` property showed its value as `nil`, and `stdcall` or `cdecl` ended the debug session, after which every further watch reported *Unable to read memory* — including variables that had just been readable. Interfaces without a convention directive, and 64-bit targets, were unaffected. 
- **Debugger, interface variable expands to its implementing object** — an interface variable whose methods declare a calling convention now shows the object behind it, with its class name and fields, instead of an empty `IInterface` node.

## [1.1.22] - 2026-08-07

### Added
- **New Delphi Project** — create a project without the Delphi IDE: VCL Forms Application, FMX Application, Console Application, Empty Program, DLL or Package. The wizard asks for the name, the location, the main form unit and its form class, and the platforms (Win32 / Win64), and lists the files it will write; a file that already exists is flagged there and never overwritten. The finished project is added to the Projects list, made active, and its main unit opened. Reach it from the new-project button in the Projects sidebar, the command palette, or the Explorer context menu.
- **Save modified files before a build** — a build now saves every modified file before it starts, so the **Ctrl+F9** / **Shift+F9** shortcuts and the palette commands compile what is on screen; on by default, switchable under **Build** on the settings page.
- **Go to Interface Section / Go to Implementation Section** — two commands that jump the caret to the unit's `interface` or `implementation` keyword. They ship without a key; assign one in the **Keyboard Mappings** editor on the settings page.
- **Open File in Delphi IDE** — a new command opens the current file in the Delphi IDE, for editing a form in the visual designer. A `.dfm` / `.fmx` opens its unit, so the form is one **F12** away inside Delphi. A running IDE takes the file over and comes to the front, instead of a second instance starting. Assign the key shortcut in the **Keyboard Mappings** editor on the settings page.

### Fixed
- **Toggle Form / Source on a file with an upper-case extension** — **F12** and the *Open …* link at the top of the file now find the companion of a unit written as `MainForm.PAS` or a form written as `MainForm.DFM`; neither found anything before.
- **LSP, loop variable of a `for … in` loop** — the variable now gets its type in three cases that were still missing: an element that is a simple type (`list: TList<Integer>`), a container reached through a type alias (`TInt64Set = THashSet<Int64>`), and a container held in a field. It could also come out as the *wrong* type — with `System.SysUtils` in the uses clause the variable was typed `Char`, and an enumerator of the same name declared in another unit won over the container's own. Covers `TList<T>`, `TObjectList<T>`, `THashSet<T>`, `TStack<T>`, `TQueue<T>`, `TDictionary<K,V>` and `TOrderedDictionary<K,V>`.
- **LSP, class helper on a descendant class** — a method added by a `class helper for TBase` now resolves on a variable of a class derived from `TBase`, instead of reporting *Unknown member*. Hover, Go to Definition and code completion are covered; a variable of the helped class itself was unaffected.
- **LSP, member reached through a `strict private` field** — hover and Go to Definition work again on a member accessed through a `strict private` or `strict protected` field of the enclosing class or record (`FMyClass.Execute`). Non-strict `private`, `protected` and `public` fields were unaffected.
- **LSP, a variable or constant named after a compiler directive** — a `var` or `const` called `local`, `static`, `virtual`, `platform`, `deprecated` or one of the other directive names no longer shows a false syntax error when another declaration precedes it in the same block. A procedural-type variable keeps its calling convention (`var F: function(...): Integer; stdcall;`).
- **Generated project files, targeted platforms** — the platform list written into a `.dproj` generated for a `.dpr` / `.dpk` was wrong beyond Win32, Win64 and Android: macOS ARM64 and the iOS ARM simulator were swapped, and Linux, macOS Intel, Win64 Modern and Windows ARM64EC were not recorded at all. Windows-only projects were unaffected.
- **Convert to UTF-8 with BOM, applied twice** — the ANSI warning and its popup now disappear the moment the file is converted. The action stayed clickable before, and a second click converted the already converted file again and garbled every non-English character in it. Converting a file that is not ANSI is refused now, and unsaved changes are written before the conversion instead of being discarded.
- **Debugger, a local reported as optimized away on the line that uses it** — in a build with optimization on, a variable the compiler keeps in a register was hidden on the last line reading it (*"Variable is optimized away and not available"*, Win64) — the very line you stop on to inspect it. A function's `Result` was hidden throughout for the same reason.
- **Debugger, RTL entries in the Variables panel** — stepping over a line no longer adds `… returned` rows for the runtime routines it called (`Writeln`, `ClassName`), which showed a mangled name and a meaningless value next to your own locals.
- **Debugger, class methods in a Watch** — `obj.ClassName` and the other `TObject` class methods (`QualifiedClassName`, `ClassParent`, `InstanceSize`, …) now evaluate on an object of any class instead of reporting the member as unavailable. Class methods a base class declares are found through the inheritance chain, and they are called on the class the object really is. 

## [1.1.21] - 2026-08-04

### Changed
- **LSP, crash notification** — the notification shown when the language server stops unexpectedly has a new **Show LSP Log** button that opens the Output panel on the *Vallenta Studio LSP* channel.

### Fixed
- **LSP, class completion in a nested class** — **Ctrl+Shift+C** on a method of a class nested inside another type now writes the full path (`function TIntList.TTest.GetCurrent`). It named only the inner class before, so the generated body did not compile. A nested method that is already implemented is recognised again as well, so a second press no longer adds a duplicate body. **Apply Signature** writes the same header. Covers types nested in classes, records and generics, at any depth.
- **LSP, labels reported as a syntax error** — a label inside a `case` branch (`tt1:` followed by `myLabel:`) no longer reports *Syntax error*. Numeric labels (`label 10;` … `10:` … `goto 10`) are recognised as well; they were rejected everywhere before.
- **LSP, values of a `set of (…)` declared in a class** — a nested type such as `test = set of (test1, test2, test3)` no longer reports *Unknown identifier* on its values, in the declaring unit and in any unit that uses it. The same declaration outside a class was unaffected.
- **LSP, names in a file that is not saved as UTF-8** — a class or type whose name contains non-English characters now resolves in a unit stored in a legacy encoding (GBK, Shift-JIS, a Windows ANSI codepage). Go to Definition, hover and code completion found nothing for those names before, and hover spelled them garbled. A file without a byte order mark is now read the way the Delphi compiler reads it, in the system codepage for non-Unicode programs; symbol caches are rebuilt once on the first start after updating.
- **LSP, names written with their full unit prefix** — hover, Go to Definition, code completion and parameter hints work again on a name qualified with its unit (`System.Classes.TStringList`, `System.Types.TDuplicates`). They found nothing whenever the leading part was a unit in its own right, which covers everything under `System`.
- **LSP, an enum value of another unit reported as an unknown type** — a declaration naming an enum value through its type and unit (`System.Types.TDuplicates.dupIgnore`, the spelling System.Classes itself uses) no longer reports *Unknown type*.
- **Missing units, quick fix on a unit-qualified name** — **Ctrl+.** on an *Unknown type* that already names its unit no longer offers unrelated units to add. `System.Types.TDuplicates.dupIgnore` suggested `Word2000` and `WordXP`, which happen to declare a type called `System`.
- **Build, editor focus** — the build output panel no longer takes the keyboard focus when a build ends, so the caret stays in the editor after **Ctrl+F9** / **Shift+F9**. The panel is still revealed.
- **Debugger, one very large class made variables undefined across the project** — a single class with a few thousand fields or methods (a large form, a big generated class) corrupted the debug symbols from that class on, so the damage was not limited to that class: breakpoints and stepping kept working, but **Watch**, hover and **Variables** reported *identifier is undefined* for locals, `Self` and object fields, also in units unrelated to the class — while plain numeric variables still resolved. Classes of any size are written correctly now; regenerate the symbols once (delete the `.pdb` next to your executable or rebuild).
- **Debugger, fields missing on very large classes** — a class with several thousand fields showed only the first part of them in the debugger; all fields are visible again. Fields located beyond 32 KB inside a large object and enum values of 32768 and above (`…_FORCE_DWORD` style) also showed wrong values and cut off the members after them.
- **Debugger, symbol files of very large projects** — the generated `.pdb` of a project with far over a million types could make symbol tools crash when looking up types; the affected lookup table is now written correctly.

## [1.1.20] - 2026-08-02

### Added
- **Attach to a running process** — debug a Delphi program that is already running instead of starting it from the editor. Reach it from the arrow beside the **Debug** button on the build toolbar, from the command palette (**Attach to Process**), or with an `attach` entry in `launch.json`. The process list puts your projects' running executables first and shows each one's platform and whether its debug symbols are ready; system processes stay hidden until you switch them on in the list's title bar. Once attached you get the same session as a normal launch — breakpoints, stepping, call stacks, locals, and the Watch evaluator with property getters and interface chains. Stopping the session detaches and leaves the program running. You can attach with no project open, as long as the `.pdb` sits beside the executable.
- **Attach, targets that cannot be debugged are flagged before you pick them** — debug symbols cannot be added to a program that is already running, so a program that was never converted is marked in the list and warned about before attaching, with the option to attach anyway. A program running elevated is marked as well, together with the note that VS Code has to run as administrator to attach to it.
- **Keyboard mappings editor** — a new **Keyboard Mappings** card on the settings page opens an editor for the extension's shortcuts, grouped by function. **Set to Delphi defaults** applies the Delphi IDE keys — F9 Run, F5 Toggle Breakpoint, F8 Step Over, F7 Trace Into, Ctrl+F9 Compile, Shift+F9 Build — covering the IDE's Run and Project menus. Click a shortcut to record a new one; conflicts are flagged as you type. **Apply only in Delphi context** keeps the keys to Pascal files, forms, the Vallenta Studio sidebar and Delphi debug sessions, so your other projects keep the standard VS Code behaviour. A fresh installation gets the Delphi keymap automatically; updating never changes keys you already have.
- **Rename Unit and Remove Unit from Project in the command palette** — both were reachable only from the Source Files menu, so neither could be given a keyboard shortcut. They now act on the open unit when invoked without one.

### Changed
- **Build toolbar shows your actual shortcuts** — the Run, Debug, Build, Rebuild and Clean tooltips read the keys you have mapped instead of fixed text; Build, Rebuild and Clean showed none at all before.
- **Convert Debug Symbols, running target** — converting the symbols of a program that is currently running now stops with a note to close it first, instead of leaving the program and its symbols out of step.

### Fixed
- **LSP, server stopped shortly after a project finished indexing** — the language server could shut down without notice and did not restart, leaving code completion, hover, Go to Definition and diagnostics dead until the window was reloaded. Most likely when a file was edited while a large project was still indexing.
- **LSP, missing editor colors while indexing** — semantic highlighting could silently fail for a file that was open while a project was still indexing, leaving it in the plain editor colors until it was reopened.
- **LSP, hover on an overloaded routine** — hovering the declaration of a second or later overload showed the first overload instead of its own signature. Affected routine overloads in an open file; class method overloads were unaffected.
- **LSP, comment inside a declaration shown in hover** — a comment written inside a declaration (`TEnum = (Enum1, // note`) no longer appears in the hover popup. Covers `//`, `{…}` and `(*…*)` in types, routines, properties, fields, variables and constants; symbol caches are rebuilt once on the first start after updating.
- **LSP, loop variable of a `for … in` loop** — a variable declared in the loop itself (`for var item in dic.Values do`) had no type, so code completion, hover and Go to Definition did nothing on it. Covers generic containers (`TList<T>`, `TObjectList<T>`, `TDictionary<K,V>.Values`), dynamic arrays, sets, strings and any type with a `GetEnumerator`. A collection put into a variable first (`var vc := dic.Values;`) is not covered yet.
- **Editor colors, comment inside brackets** — a comment written inside an array index (`LArray[0 {note}]`) is colored as a comment again, instead of taking the attribute color.

## [1.1.19] - 2026-07-30

### Added
- **Missing units, choose where the quick fix adds them** — a new **Add Missing Unit To** setting (settings page → *Editor Options*, or `vallenta.studio.lsp.semanticValidation.addMissingUnitTarget`) can put a unit used in the implementation part into the `interface` uses clause instead. The implementation variant stays available as a second **Ctrl+.** action. Default is unchanged: the usage location decides.
- **Debugger, switch off local variables** — a new **Evaluate local variables** checkbox in the **Delphi Debug** view (Run and Debug sidebar, formerly *Delphi Exception Filters*) lets you switch off the evaluation of locals: the **Variables** view stays empty and nothing is evaluated while stepping — useful when large object graphs make stepping slow. Hover and Watch still evaluate on demand, and toggling takes effect immediately, even mid-session. Also available as the `vallenta.studio.debug.evaluateLocals` setting.
- **Debugger, Watch ignores identifier case** — **Watch** and hover now resolve identifiers case-insensitively, the way Pascal reads them: a Watch on `C_Lage` finds a variable declared as `c_lage` instead of reporting *identifier is undefined*. Covers locals, parameters, fields and unit-level globals; symbols need to be regenerated once (delete the `.pdb` next to your executable or rebuild). Can be switched off with `vallenta.studio.debug.caseInsensitiveIdentifiers`.

### Fixed
- **Debugger, Win64 variables reported as optimized away** — in an optimized Win64 build, a local or parameter the compiler keeps in a register showed *Variable is optimized away and not available.* in **Variables** and **Watch** instead of its value. Affected `Integer`, `Boolean`, `Char`, enums and other values smaller than a pointer; object references, strings and `Int64` were unaffected, as was Win32 throughout.
- **Debugger, Watch ignores identifier case** — **Watch** and hover now resolve identifiers case-insensitively, the way Pascal reads them: a Watch on `C_Lage` finds a variable declared as `c_lage` instead of reporting *identifier is undefined*. Covers locals, parameters, fields and unit-level globals.
- **LSP, array of an inline record** — a declaration such as `array [0..2] of record … end` no longer breaks the unit it appears in. Everything below it was lost, leaving the rest of the unit without code completion, hover, Go to Definition and document outline. `array of record … end` and `file of record … end` are covered too.
- **LSP, `AnsiString` with a named codepage** — a codepage written as a constant, `type TAscii = type AnsiString(CP_ASCII)`, no longer breaks the unit it appears in the same way; only a numeric codepage (`AnsiString(20127)`) parsed before. A qualified name such as `System.CP_UTF8` is covered as well.
- **LSP, parameter hints with a long routine name** — the hint popup no longer scrolls sideways when the class and method name are long; the signature wraps onto the next line instead.
- **LSP, types declared inside a routine** — a type declared in a procedure's local `type` section now shadows same-named types elsewhere, the way the compiler scopes it. FMX.Layouts' local `TState` reported *Unknown member* on every value because a same-named type nested in another class won the lookup. Fields, globals and parameters keep the type their own declaring scope sees.
- **LSP, `&`-escaped members** — `TState.&End` now resolves like its unescaped spelling; the `&` is ignored when matching the member name.
- **Editor, reserved words as member names** — a reserved word after a dot (`TState.End`) now parses without the `&` escape, as the compiler reads it; FMX.Layouts and several other RTL units no longer show syntax errors. A dangling `obj.` before `end` or `else` while typing is still reported as incomplete member access.

## [1.1.18] - 2026-07-27

### Added
- **Parameter hints** — typing a call's `(` or `,` now shows the routine's parameters, with the one you are filling in in bold. Every overload is listed — step through them with ↑/↓, and the one matching the arguments you have typed so far is preselected. Methods, constructors, `inherited` and calls inside a `with` block are covered; **Ctrl+Shift+Space** brings the hint back at any time.

### Added
- **Go to Definition switches between declaration and implementation** — Ctrl+Click or F12 on a routine's own name now jumps to its counterpart: from the declaration — in the `interface` section or inside a class — to the body, and from the body header back to the declaration. Overloads land on the matching signature. Clicking the class part of `TFoo.Bar` still goes to the class, and every other position — call sites, parameter types, variables — keeps following the **Go to Definition target** setting.

### Changed
- **LSP, faster diagnostics on large units** — the unused-variable analysis is many times faster on large `with`-heavy units (Vcl.Forms: 36 s → ~1 s), and errors and warnings now appear as soon as they are found instead of waiting for the slower hint analyzers to finish.

### Fixed
- **Missing units, quick fix on a qualified type** — **Ctrl+.** on an *Unknown type* written as `TOuter.TNested` now offers the unit declaring `TOuter`, instead of no fix at all. A unit-qualified name (`Vcl.Forms.TForm`) is still not covered.
- **Editor colors, escaped quote at the end of a line** — a line ending in an escaped quote (`quotechar := ''''`) no longer colors the rest of the unit as one long string. 
- **LSP, `Destroy` on a class from another unit** — `LVar.Destroy` no longer reports *Unknown member* when the class is declared in a different unit; it resolves, hovers and appears in code completion like any other inherited member. Symbol caches are rebuilt once on the first start after updating.
- **LSP, members through a unit-qualified type alias** — an alias whose target names its unit (`PExceptionRecord = System.PExceptionRecord`) now resolves through to the type it names, so members reached through it no longer report *Unknown member*.
- **LSP, `Create` on a dynamic array** — a dynamic array's implicit constructor (`TBytes.Create($EF, $BB, $BF)`) no longer reports *Unknown member*. Covers `array of T`, `packed array of T` and `TArray<T>`; a fixed-length array still has none.
- **LSP, signature mismatch on aliased parameter types** — a routine declared with one type name and implemented with an alias of it (`PAnsiChar` against `MarshaledAString`, or your own `TMyChar = PAnsiChar`) no longer reports *Signature differs from the declaration*.
- **LSP, signature mismatch on class constructors and destructors** — a `class destructor Destroy` declared beside an instance `destructor Destroy` no longer reports *Signature differs from the declaration*, and **Apply Signature** no longer offers to rewrite the one into the other.
- **LSP, unused-parameter hint on procedural types** — the parameters of a procedural type (`TCompareProc = function(const S1, S2: string): Boolean`) are no longer flagged as *never used*; they name the type's call shape and have no body to be used in.

## [1.1.17] - 2026-07-26

### Added
- **Insert GUID (Ctrl+Shift+G)** — press **Ctrl+Shift+G** in a Delphi file (or right-click → **Insert GUID**) to drop a fresh interface GUID — `['{…}']` — at the cursor.
- **Apply Signature (Ctrl+Shift+Alt+C)** — press **Ctrl+Shift+Alt+C** (or **Ctrl+.** → **Apply signature to declaration** / **to implementation**) to apply the routine signature at the cursor to its counterpart: the declaration when you are in the body, the implementation header when you are on the declaration. Parameter defaults, visibility and `virtual`/`override` are preserved; overloads are paired rather than guessed, and you are asked when more than one counterpart fits. A counterpart that does not exist yet is created.
- **Signature mismatch, hint** — a routine whose implementation header no longer matches its declaration is flagged as you type, instead of at the next build. On by default at information severity, configurable on the Settings page.

### Changed
- **Debugger, faster symbol conversion** — converting debug symbols is now several times faster on large executables. The generated PDB is unchanged, and the accompanying `.meta.json` is about a third smaller.

### Fixed
- **Debugger, macros in the Host Application path** — a Host Application or working directory written with a macro — an environment variable such as `$(FINE_ROOT)`, a Delphi *Environment Variables* override, or `$(BDS)` — now resolves when you debug a library or package, instead of failing with *Host application not found*. Output and search paths follow the same rules; previously only `$(Platform)` and `$(Config)` were expanded.
- **Debugger, garbled function results in Variables** — stepping over a line that calls a function returning a `string`, `Variant`, interface or dynamic array listed those functions in the **Variables** view as *&lt;name&gt; returned*, showing nonsense text or *Error reading characters of string* — three such rows appeared for a single line like `IncludeTrailingPathDelimiter(ExtractFilePath(ParamStr(0)))`. Delphi hands a result of that kind back through a hidden pointer rather than the register the debugger reads, which the generated debug symbols now describe correctly. Results that really are register-returned — numbers, Booleans, enums, object references — are unaffected.
- **Editor colors, multiline strings** — Delphi keywords inside a Delphi 12+ multiline string (`record`, `begin`, `end`) are no longer highlighted as code by the block-nesting colors. A string whose content starts a line with a run of quotes, or whose delimiter uses an even number of quotes (`''''`), is no longer cut short either.
- **Exception handler variables** — the variable an `on E: Exception do` handler declares is now a local like any other: code completion offers it and its members, and hover and Go to Definition work on it.
- **Editor colors, exception handlers** — `on` is now highlighted as a keyword in `on E: Exception do`. A property named `On` stays an identifier.
- **Editor colors, members named after a directive** — `E.Message` is colored as the property it is, instead of in the keyword color; `.Register`, `.Read`, `.Write` and `.Default` are likewise colored as the members they are.
- **Structural highlight, exception handlers** — putting the cursor on an `on` highlights it together with its `do`, and the other way round, the way `for`, `while` and `with` already pair with theirs. A variable or property named `On` is left alone.
- **Hover, units on the Delphi library path** — a third-party unit reached through the Delphi *Library Path* (e.g. `SuperObject`) no longer hovers as *Delphi RTL/VCL library*; it now reads *third-party library*, while Delphi's own RTL/VCL units keep the RTL label.
- **LSP, conditional inside a type alias** — a type alias whose target is switched by a directive inside the declaration (`TFoo = {$IFDEF X} TThreadList {$ELSE} TList {$ENDIF};`) now resolves to the active branch from other units; previously the first branch always won unless the declaring unit was open in the editor.

## [1.1.16] - 2026-07-23

### Added
- **Missing units, quick fix** — press **Ctrl+.** on an *Unknown type* or *Unknown identifier* to add the unit declaring it to the `uses` clause: **Add 'Vcl.Forms' to interface uses** or **Add 'Vcl.Dialogs' to implementation uses**, depending on where the symbol is used. When several units declare a symbol of that name (`TBitmap` in `Vcl.Graphics` and `FMX.Graphics`), one action per unit is offered, project units first and ordered by the project's unit scope names; `TList<Integer>` finds the generic `TList<T>`'s unit, a bare `TList` the non-generic one. Constants, enum values and global variables are covered too (`mrOk` → `System.UITypes`), and a section without a `uses` clause gets one created. Where a comment or a compiler directive sits at the end of the clause, no fix is offered and the clause is left for you to edit by hand.
- **Settings, write launch.json** — a new **Debugger** section on the Settings page toggles whether Vallenta Studio writes or updates a launch configuration in `.vscode/launch.json` when you start debugging (on by default). Turn it off to leave your `launch.json` untouched; debugging still works.
- **Debugger, symbol conversion timeout is now configurable** — the timeout for converting debug symbols now defaults to 5 minutes (up from a fixed 60 seconds) and can be changed with the new `vallenta.studio.symbolConverterTimeout` setting, giving large executables time to finish.

### Fixed
- **LSP, a bare type name shared with a generic** — a type reference with no type arguments (e.g. `TList` in `type MyList = TList`) no longer resolves to a same-named generic such as `TList<T>` when no non-generic `TList` is in scope; the compiler treats the two as distinct names, so hover, Go to Definition and completion no longer point at the wrong type. A generic type's own bare name used inside its own declaration (`function Clone: TMyStack` within `TMyStack<T>`) still resolves to itself.
- **LSP, `MANAGED_RECORD` and `WEAKREF` conditionals** — `{$IFDEF MANAGED_RECORD}` and `{$IFDEF WEAKREF}` now follow the Delphi version's predefined compiler symbols.
- **Debugger, timed-out symbol conversion** — a conversion that hit the timeout could leave a partial PDB behind, causing the next debug start to skip re-converting and run without symbols; the incomplete file is now discarded so conversion runs again.

## [1.1.15] - 2026-07-22

### Added
- **Dproj Editor, package & DCP output paths** — the Paths section now has **Package Output Directory** and **DCP Output Directory** fields, editable per configuration and platform.
- **Dproj Editor, Search Paths filter** — a filter box with `*`/`?` wildcards at the top of the Search Paths dialog narrows long path lists; clear it with the ✕ button.

### Changed
- **Dproj Editor, editable output paths** — the output-directory fields can now be typed directly — so you can enter a macro path such as `..\..\$(Platform)` — with a folder button beside each for the pick-a-folder dialog.

### Fixed
- **Build Toolbar, Cancel Build** — dismissing the symbol-conversion failure notification with its close (✕) button now cancels the debug launch and returns the toolbar to Ready, instead of leaving it stuck on "Converting debug symbols…" with a Cancel Build button that did nothing. Choose **Continue** on that notification to debug without symbols.
- **LSP, member of a type from an indirectly used unit** — a member reached through a function result, property or field whose type is declared in a unit the calling unit doesn't itself use (e.g. `ExceptionLog7.CurrentEurekaLogOptions.ActivateEurekaLog`, where the options class lives in another EurekaLog unit) now resolves on hover, Go to Definition and code completion; previously nothing was offered and completion showed an *unresolved* placeholder.
- **LSP, `uses` entries split by `{$IFDEF}`** — a `uses` clause with compiler directives between or inside its entries (as in EurekaLog's `ExceptionLog7.pas`) no longer loses or garbles unit names during indexing; the units of every branch are recorded, so the types they provide resolve. Symbol caches rebuild once on the next start.
- **LSP, same-named types from different units** — a member accessed through a function result or property now resolves against the type copy visible to the unit declaring that function or property — as the compiler does — instead of whichever same-named type the calling unit could see. This also removes false *Unknown member* errors on `TMonitor` members inside `Vcl.Forms`, and a nonexistent member on an inline `var` typed via `Default(T)` is flagged again.
- **LSP, inline `var` from an inherited member** — a variable initialized from an unqualified member of the enclosing class (e.g. `var o2 := Owner;`, an inherited `property Owner: TObject`) now infers that member's type, even when a unit in the `uses` clause declares an unrelated member of the same name (e.g. `TComponent.Owner` from `Classes`); previously the unrelated one could win and the variable took the wrong type.
- **LSP, republished properties** — a property republished without a type (`property Style;`) on a class from another unit now finds its type through the ancestor, so hover, Go to Definition and completion work on members reached through it — including a protected ancestor property republished public.

## [1.1.14] - 2026-07-20

### Added
- **LSP, a unit opened from outside the project** — opening a `.pas` that isn't part of the active project — one belonging to another project, or anywhere on disk — now gives it the full language features (hover, Go to Definition, code completion, outline, jumping between a routine's declaration and its body, diagnostics) under the active project's settings, where before it offered none. Other units it uses from the same folder resolve as well, and opening such a unit resolves the types it provides in the ones that use it; closing a file removes it again, leaving the project's own symbol index untouched.

### Fixed
- **LSP, members of an element of a generic collection** — a member accessed on an indexed element of a generic collection (e.g. `FGroups[I].Active`, where `FGroups` is a `TObjectList<TRegGroup>`) now resolves on hover, Go to Definition and code completion; previously the element's type was not worked out, so nothing was offered and a misspelled member went unreported as well. Collections that take their index from a base such as `TList<T>` are covered, as is the explicit form (`FGroups.Items[I].Active`).
- **LSP, indexed properties** — a property whose index takes more than one parameter (e.g. `Cells[Row: Integer; Col: Integer]`) now resolves the members of its element type, and a class declaring several indexed properties uses the one marked `default` instead of whichever was declared first.
- **Code completion, enum variables and implementation-section types** — typing a dot after a variable of an enum type now offers the enum's values and the methods a `record helper` adds to it; previously the list showed only an *unresolved* placeholder. A variable whose type is declared in the unit's implementation section resolves for completion as well.

## [1.1.13] - 2026-07-19

### Fixed
- **Code completion, right after a routine header** — on the implementation side, with the caret just after a routine header whose body isn't written yet (`procedure Foo;` on the line above the caret, or right after its `;`), the suggestion list now leads with `begin` and the other routine declaration-area keywords (`var`, `const`, `type`, `asm`, nested routines and directives) instead of the unit's section-level keywords. A blank line directly above `implementation` no longer shifts the interface/implementation boundary by a line.
- **LSP, compiler switch conditionals (`{$IFOPT}`)** — `{$IFOPT}` now follows the project's compiler switch settings — Optimization, Overflow and Range checking, I/O checking, and the rest — for the active configuration and platform, so the right branch is active and the other greyed out. Local switch directives (`{$Q+}`, `{$OPTIMIZATION ON}`, `{$B-,R+,Q+}`, including inside `{$I}` includes) are tracked as they change through a unit. `{$IFDEF Q+}` is now read the way the compiler reads it — the symbol `Q`, trailing `+` ignored — so `{$DEFINE Q}` makes it active; and an invalid `{$IFOPT OVERFLOWCHECKS+}` is greyed out with a hint instead of being treated as always active.
- **LSP, RTL symbols after a nested `{$IF}`-split routine header** — a routine header split across nested `{$IF}`/`{$ELSEIF}`/`{$ELSE}` branches (`_DelphiPersonalityRoutine` in `System.pas`) silently ended the unit's indexing, so every symbol declared after it — `DynArraySize` and several hundred others in the tail of the `System` interface — was flagged with a false *Unknown identifier* and offered no hover or Go to Definition from other units. Such declarations are now indexed in every branch, each tagged with its condition.
- **LSP, multiline string literals** — a Delphi 12+ multiline string (`'''…'''`), including one that embeds a shorter run of quotes by using a longer delimiter (e.g. `'''''`), no longer triggers a false *Syntax error*, and now colors as a single string in the editor rather than highlighting its content as code.
- **LSP, types reached through a `{$I}` include in a `uses` clause** — a unit named inside an include file that sits in a `uses` clause (e.g. `uses {$I units.inc} Classes;`) is now recognized, so the types it provides resolve on hover, Go to Definition and code completion instead of showing a false *Unknown type*. Renaming such a unit is refused with a note naming the include file, since the reference there can't be rewritten automatically.
- **LSP, unit search path changes** — adding or removing a project unit search path now takes effect without restarting the language server; previously the change was accepted but indexing kept using the old paths until a restart.
- **LSP, a unit dropped from the project** — a unit removed from the project (or deleted on disk) while the editor was closed is now dropped from the symbol index on the next start, instead of lingering until a manual reindex.
- **LSP, a type indexed after the file that uses it** — a type whose defining unit is indexed after the file referencing it now resolves as soon as that unit becomes available, instead of needing a language-server restart.
- **LSP, `SizeOf` of a record in `{$IF}` conditions** — a conditional such as `{$IF SizeOf(Extended) <> SizeOf(TExtended80Rec)}` now resolves the record's size and shades the correct branch. Built-in and well-known RTL type sizes are evaluated for the active target platform.
- **LSP, member of a function's result** — a member accessed directly on a function's return value (e.g. `GetObj.Value`, where `GetObj` returns a class) now resolves on hover, Go to Definition and code completion; previously the member wasn't recognized and couldn't be clicked, even when the function and its type were reachable through the `uses` clause.
- **LSP, `goto` labels** — a label name — the `label` declaration, each `goto`, and the `name:` marker — is no longer flagged with a false *Unknown identifier*.
- **Go to Definition and hover, `goto` labels** — Ctrl+Click / F12 on a `goto` now jumps to the `name:` marker, the marker jumps back to the `label` declaration, and hover shows the label; previously a label name was neither clickable nor hoverable.

## [1.1.12] - 2026-07-17

### Added
- **Unused units, quick fix** — press **Ctrl+.** on a unit flagged as unused in a `uses` clause to remove just that one, or every unused unit in the file at once; the leftover comma or line goes with it, and a clause left empty disappears. Where a comment or a compiler directive sits between the entries so that removing one would disturb it, no fix is offered and the entry is left for you to edit by hand.

### Changed
- **Class completion, placement (Ctrl+Shift+C)** — a generated method body now appears next to the bodies of its neighbours in the class, mirroring the order the methods are declared in, instead of at the end of the unit. A method whose neighbours have no bodies yet is still appended at the end of the implementation section.

### Fixed
- **Debugger, optimized Win64 local variables** — in a 64-bit build compiled with optimization, a register-held local variable (such as a `TList` or other object or pointer) could show as `nil` or a wrong value at a breakpoint; these now show their correct value. 32-bit builds were unaffected.
- **LSP, types after an `{$IFDEF}`-split variant record** — a variant record whose fields are split across `{$IFDEF}`/`{$ELSE}` branches (e.g. `TTypeData` in `System.TypInfo`) broke the unit's indexing, so every type declared after it was missing: `PPropInfo` and friends were flagged with a false *Unknown type* error and their members did not resolve.
- **LSP, inherited members of an implementation-section class** — a class declared in a unit's implementation section that inherits from a type imported in the implementation `uses` (e.g. `TCustomStrList = class(TStringList)`) now resolves its inherited members on hover and in code completion; previously every member (`Create`, `Add`, `Free`, …) was flagged with a false *Unknown member* error.
- **Go to Definition and hover, `inherited` calls** — `inherited Test;` now navigates to the base class's method instead of the overriding method it is written in, and the `inherited` keyword itself — bare (`inherited;`) or qualified — now supports Go to Definition and hover for the method it calls; previously the keyword wasn't clickable at all.
- **LSP, multiple generic constraints** — a generic type parameter with more than one constraint (e.g. `TFoo<T: class, constructor>`) no longer triggers a false *Syntax error*.
- **LSP, generic vs non-generic type resolution** — with both `System.Classes` and `System.Generics.Collections` in the `uses` clause, a bare `TList` (e.g. `TCustomList = class(TList)`) wrongly resolved to the generic `TList<T>`. Hover, Go to Definition, code completion and inherited members now match a type reference by its type arguments, in any `uses` order: bare `TList` finds `Classes.TList`, `TList<Integer>` the generic class, and `TFunc<Integer, string>` the two-parameter `TFunc<T, TResult>`.
- **LSP, inline `var` from a literal** — a variable declared with inline `var` and initialized from a literal (e.g. `var LInteger := 1;`, `var LText := 'abc';`, `var LFlag := True;`) now infers its type, so its members — including type-helper methods such as `Integer.ToString` — are recognized on hover and in code completion; previously the list came up empty. A `for var I := 1 to 10` loop variable infers the same way, and a single-character literal (`var LChar := 'a';`) infers `Char`, as the compiler does.
- **Code completion, `Char` and `Currency` members** — a `Char` or `Currency` variable now offers its type-helper members (`ToUpper`, `ToString`, …); previously nothing was suggested. `TCharHelper` lives in `System.Character`, so that unit has to be in the `uses` clause.
- **LSP, hexadecimal literals with an `E` digit** — a hex literal such as `$FE` or `$1E5` was read as a floating-point value, which could match the wrong overload of a routine on hover and Go to Definition; it is now integral.
- **Code completion, built-in types** — a type position (e.g. `var LValue: |`) now offers Delphi's built-in simple types — `Integer`, `Double`, `Boolean`, `Char`, `Variant` and the rest. They are compiler intrinsics with no declaration in any source, so they could never be suggested before; 
- **Unused variable detection, same-named inline variables in sibling blocks** — an uninitialized inline `var` (e.g. `var nValue: Integer;`) declared under the same name in two sibling blocks, such as separate `case` branches, is no longer flagged as *declared but never used* when each block uses its own. One that genuinely is unused is also no longer missed when a later block reuses the name. Inline `const` behaves the same.
- **Find All References, uninitialized inline `var`** — a search from an uninitialized inline `var` now finds the uses in its own block. Previously, when the same name was declared in more than one block of a routine, every reference was attributed to the first declaration and a search from any later one found nothing.
- **Source Files panel, Unicode filenames** — a unit whose filename is made up entirely of non-ASCII characters now appears in the **Source Files** panel, and **Remove from Project** and **Rename Unit…** work on it; previously it was missing from the panel altogether. 
- **Outline and breadcrumbs, Unicode declarations** — a unit, class, record, interface, method, property or routine named with non-ASCII letters now appears in the Outline view and breadcrumbs; previously such declarations were missing.
- **Add Existing File, Unicode form classes** — adding a `.pas` whose companion `.dfm`/`.fmx` declares a non-ASCII class name now keeps its `{Name: TDataModule}` comment in the `.dpr`; a `.dproj` generated for a `library` or `program` with a non-ASCII name is also no longer typed as a plain application.
- **Class completion, shadowed routines** — a routine declared in the interface no longer misses out on its stub when another routine contains a local routine of the same name; Ctrl+Shift+C now generates its body as well.
- **Class completion, caret in an interface** — with the caret inside an `interface` type declaration, Ctrl+Shift+C generated the missing bodies of every other type in the unit. It now does nothing there, since an interface's methods are implemented by the classes that support it.
- **LSP, syntax errors while typing** — an unfinished routine (e.g. `begin` just typed, so the unit's closing `end.` is momentarily taken as the routine's own `end`) marked the entire file as one syntax error from the first line to the last. The errors now stay at the typing point — `';' expected but '.' found` and `'end.' expected` — hover, outline and code completion keep working for the rest of the unit.

## [1.1.11] - 2026-07-14

### Added
- **Code completion, context-aware** — the suggestion list now fits the cursor's position: it offers the Delphi keywords valid there and, in places like a type reference or a declaration, narrows the symbols to those that belong. Entries are grouped and ordered like the Delphi IDE — by scope (locals, `Self` members, the current unit, other units, then the implicit `System` symbols) — and each is labelled with its kind (`var`, `const`, `type`, `keyword`, …) as text, not by icon alone. New **Completion** settings toggle the keyword suggestions and the context filtering, and set whether keywords lead the list or sit among the members.

### Changed
- **Unused variable detection, loop variables** — an inline loop variable (`for var i := 0 to 10 do`, or `for var x in …`) whose loop body never uses it is no longer flagged as unused by default, matching the Delphi compiler. A new **Report unused loop variables** option in the detection **Options** dialog turns it back on.

### Fixed
- **LSP, crash on inline `var` named like a routine** — a file declaring an inline `var` whose initializer refers back to its own name — typically because it's named like the function it calls (e.g. `var signData := SignData('');`), or a plain self-reference (`var x := x.Trim;`) — crashed the language server on open; it now opens normally.
- **LSP, inline `var` scope** — an inline `var` now follows Delphi scope rules: it is visible only after its declaration and within its enclosing block, and a name redeclared in a nested block resolves to the innermost declaration. Previously, hovering the call in `var signData := SignData('');` showed the variable instead of the function, and members of a name redeclared in sibling blocks could be flagged with false *Unknown identifier* errors.
- **LSP, inline `var` from a routine call** — a variable initialized from a routine call (e.g. `var Data := SignData('');`, with or without parentheses) or from an unqualified method or property of the enclosing class (e.g. `var R := GetClientRect;`) now infers its type, so its members are recognized on hover and in code completion.
- **LSP, inline `var` from `Result`** — a variable declared with inline `var` and initialized from a function's `Result` (e.g. `var Rec := Result;` or `var X := Result.Field;`) now infers its type, so its members are recognized on hover and in code completion; previously member access on such a variable was flagged as a false *Unknown member* error.
- **Find All Implementations, results panel** — when the results list was long enough to scroll, the summary header overlapped the top entries; it now stays cleanly above the list.

## [1.1.10] - 2026-07-13

### Added
- **Class completion (Ctrl+Shift+C)** — declare a method on a class or record — or a routine in the unit's interface — then press **Ctrl+Shift+C** (or right-click → **Complete Class at Cursor**) to generate its empty implementation body and jump straight to it. It completes every unimplemented method of the type at the cursor at once — class methods, constructors, destructors, operators, overloads and generic types included — and works for global routines too, while leaving already-implemented methods untouched.

### Fixed
- **LSP, helper members** — a method or constant added to a type by a `record`/`class helper` is now recognized on hover and in code completion, including helpers declared in a unit's implementation section and constants on built-in types such as `Integer.MaxValue`; previously these produced a false *Unknown member*.
- **LSP, inline `var` from `Default(T)`** — a variable declared with inline `var` and initialized from `Default(T)` (e.g. `var LArticle := Default(TArticle);`) now infers its type, so its members are recognized on hover, in code completion, and inside a `with` block; previously they were flagged as false *Unknown member* / *Unknown identifier* errors.
- **LSP, interface GUID from a constant** — an interface whose GUID is a named constant (e.g. `[SID_IIdentityName]`) rather than a string literal no longer triggers a false *Syntax error*.
- **LSP, `dispinterface` properties** — a property with a `readonly` or `writeonly` directive (e.g. `property nodeName: WideString readonly dispid 2;`) no longer triggers a false *Syntax error*.
- **LSP, record alignment directive** — a record with an `align(N)` alignment directive (e.g. `end align(16);`) no longer triggers a false *Syntax error*.
- **LSP, comment between a method name and its parameters** — a method declaration with a comment between the name and its parameter list (e.g. `procedure Test { note } (ANumber: Integer);`, split across lines) no longer triggers a false *Syntax error*.

## [1.1.9] - 2026-07-12

### Added
- **Find All Implementations (Ctrl+F12)** — place the cursor on an interface, class, interface method or virtual method and press **Ctrl+F12** (or right-click → **Find All Implementations**) to see every concrete implementer in a dedicated, keyboard-operable panel: the classes that implement an interface, a class's descendants, the methods implementing an interface method, and the overrides of a virtual or abstract method. Results are grouped by implementing type and show the unit each one lives in; a symbol with nothing to implement shows a short message.
- **Find All References, keyboard navigation** — the results view now takes focus when a search starts, so you can drive it from the keyboard: arrow keys move through the entries, Space collapses or expands a group, and Enter jumps to the selected match.

### Fixed
- **LSP, false syntax errors in different units** — opening an unit such as `System.Rtti.pas` could show hundreds of phantom syntax errors caused by a stale cache.
- **LSP, member resolution after a reference search** — Find All References or Rename could subtly degrade hover/completion for types declared in files the search scanned but that were not open in the editor.
- **LSP, multi-level pointer dereference** — member access now resolves through a double pointer dereference (e.g. `AInfo^.PropType^^.Kind`); previously a pointer-to-pointer flagged a false *Unknown member* error.
- **LSP, members of nested types** — a field, method or property of a type nested inside a class — or of a pointer to such a type — now resolves while the unit is open, clearing many false *Unknown member* / *Unknown identifier* errors in units such as `System.Rtti.pas`.
- **LSP, members through a generic alias** — a member reached through an alias of a generic specialization (e.g. `TInts = TList<Integer>`) now resolves instead of a false *Unknown member* error.
- **LSP, generic-qualified nested types** — a nested type referenced through a generic instantiation (e.g. `TList<THeapItem>.ParrayofT`) is no longer flagged as a false *Unknown type* error.

### Changed
- **LSP, occurrence highlighting** — placing the cursor on an identifier no longer triggers a project-wide reference scan; the gray occurrence highlights are computed from the current file only.
- **Find All References / Rename, conditional code** — matches are now consistently found in active `{$IFDEF}` branches only; previously occurrences in inactive branches were sometimes included for files not open in the editor.
- **Sidebar, section header buttons** — the header actions in the Projects and Source Files sections (add project, add from folder, add group, refresh) are now always visible, instead of appearing only while hovering the section header.

## [1.1.8] - 2026-07-10

### Added
- **Unused variable, constant & parameter detection** — a new diagnostic flags local variables and constants that are declared but never used within a routine — shown faded in the editor and listed in the Problems panel. It can additionally report symbols that are only ever assigned and never read (flagged as *unnecessary*), and unused parameters — parameters of overrides, interface implementations and published event handlers are never flagged.
- **Settings, Unused Variables, Constants, Parameter Detection** — a dedicated section on the Settings page controls it: on by default at Information severity, with a dropdown to change the severity or switch it off, and an **Options** dialog for the two opt-ins (report assigned-but-never-read, and check parameters).
- **Settings, Find All References scope** — a new **Find All References Scope** dropdown in the **LSP Server** section lets you set a default scope: keep *Always ask* (the default) to show the scope picker on every search, or choose **Project** or **All indexed sources** to skip the picker and always search that scope.

### Fixed
- **LSP, Go to Definition on a qualified type alias** — Ctrl+Click / F12 on the qualified right-hand side of a type alias (e.g. `System.TTypeKind` in `TTypeKind = System.TTypeKind;`) now navigates to the referenced type; previously the unit qualifier was ignored, so it jumped to the same-named local alias instead of the real type.

## [1.1.7] - 2026-07-09

### Fixed
- **Build Toolbar, Cancel Build** — when converting debug symbols fails after a build, the Cancel button now aborts the build immediately; previously it did nothing and the build stayed stuck until you opened the error notification and answered it.
- **Rename, Unicode names in ANSI files** — renaming a symbol or unit to a name containing non-ASCII characters (e.g. `Prüfung`) now converts the affected ANSI-encoded files to UTF-8 with BOM, so the new name is stored intact everywhere; previously it was saved mangled (`Pr?fung`). A notification lists the converted files.
- **Add Existing File / Remove from Project, ANSI project sources** — adding or removing a unit in an ANSI `.dpr`/`.dpk` that contains umlauts or other special characters no longer corrupts those characters.
- **New Delphi-File, Unicode unit names** — the dialog now accepts unit names with non-ASCII letters (e.g. `единица1`, `Prüfung`); previously it rejected them as invalid. When such a unit is added to an ANSI-encoded `.dpr`/`.dpk`, that file is converted to UTF-8 with BOM so the name is stored intact.
- **Convert to UTF-8 with BOM, special characters** — the `€` sign, typographic quotes, dashes and similar characters are now preserved during conversion; previously they were silently corrupted.
- **Encoding warning, convert link** — the *Convert to UTF-8 with BOM* link in the hover on line 1 of an ANSI file works again.
- **LSP, `strict private` nested types** — a type declared in a class's `strict private type` (or `strict protected type`) section is no longer flagged as an unknown type when referenced from a sibling nested type; previously it was recognized only without `strict`.
- **LSP, conditionally-compiled routines in System.pas** — the `Set8087CW` / `Get8087CW` family and other FPU/assembler routines guarded by compiler conditionals now resolve on Ctrl+Click / F12 and hover, and System.pas is highlighted correctly; previously they couldn't be found and a large part of the unit was flagged as errors.
- **LSP, very large units** — computing the grayed-out inactive regions of a huge unit such as System.pas is now near-instant, instead of taking many seconds.
- **LSP, chained directives on a procedure type** — a procedural type that stacks two directives without a semicolon between them (e.g. `procedure(p: Pointer); cdecl varargs;`) no longer triggers a false *Syntax error*.

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
