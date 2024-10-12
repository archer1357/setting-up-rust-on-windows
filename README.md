# Setting up Rust On Windows

Quick guide to setting up Rust on Windows.

## Installing

* **rust up** - download `x86_64-pc-windows-msvc` from [Other Rust Installation Methods](https://forge.rust-lang.org/infra/other-installation-methods.html)

* **vs build tools** - *rust up* will automatically download the 2019 version for you, select and install `Desktop Development with C++`, the link is also [available here](https://aka.ms/vs/16/release/vs_buildtools.exe).

* **vs code** - [download link](https://code.visualstudio.com/Download)

* **vs code extensions** - [Better Toml](https://marketplace.visualstudio.com/items?itemName=bungcip.better-toml), [CodeLLDB](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb) and [Rust Analyzer](https://marketplace.visualstudio.com/items?itemName=matklad.rust-analyzer)

## Project Config

### workspace

In vs code, click `file -> save workspace as`, and save to your project's directory. eg `c:\Projects\hello\workspace.code-workspace`.

### compile using tasks (optional)

Create a ```tasks.json``` for your project (eg `c:\Projects\hello\.vscode\tasks.json`).


```json
{
	"version": "2.0.0",
	"tasks": [
		{
			"type": "cargo",
			"command": "build",
			"problemMatcher": [
				"$rustc"
			],
			"group": {
				"kind": "build",
				"isDefault": true
			},
			"label": "rust: cargo build",
			"presentation": {
				"reveal": "silent",
				"revealProblems": "onProblem",
				"close": true,
				"clear": true 
			}
		}
	]
}
```

Run with shortcut ```Ctrl + Shift + B```.

### Using launch (optional)

Create `launch.json`. eg `c:\Projects\hello\.vscode\launch.json`.

```json

{
    "version": "0.2.0",
    "configurations": [

        {
            "name": "Debug",
            "type": "lldb",
            "request": "launch",
            "program": "cargo",
            "args": ["run"],
            "stopAtEntry": false,
            "cwd": "${workspaceRoot}",
            "environment": [],
            "externalConsole": true,
            "env" : {
            }
        },
        
        {
            "name": "Release",
            "type": "lldb",
            "request": "launch",
            "program": "cargo",
            "args": [
                "run", "--release"
            ],
            "stopAtEntry": false,
            "cwd": "${workspaceRoot}",
            "environment": [],
            "externalConsole": true
        }
    ]
}
```
Run by pressing the `F5` key.

You can also choose between debug and release modes by clicking on the `Run and Debug` sidebar menu or using the shortcut `Ctrl+Shift+D`.

## Global Config (optional)

Instead of storing launch in the project, you store it in the global settings file. eg `%APPDATA%\code\user\settings.json` or if you're using the portable version `c:\vscode\data\user-data\User\settings.json`.

```json
{

    "launch": {
        "configurations": [

            {
                "name": "Debug",
                "type": "lldb",
                "request": "launch",
                "program": "cargo",
                "args": ["run"],
                "stopAtEntry": false,
                "cwd": "${workspaceRoot}",
                "environment": [],
                "externalConsole": true,
                "env" : {
                }
            },
            
            {
                "name": "Release",
                "type": "lldb",
                "request": "launch",
                "program": "cargo",
                "args": [
                    "run", "--release"
                ],
                "stopAtEntry": false,
                "cwd": "${workspaceRoot}",
                "environment": [],
                "externalConsole": true
            }
        ]
    },
    
    "security.workspace.trust.untrustedFiles": "open",
    "editor.accessibilitySupport": "off",
    "debug.allowBreakpointsEverywhere": true,
    "task.quickOpen.detail": false,
    "task.quickOpen.skip": true,
    "task.quickOpen.history": 1,
    "terminal.integrated.showExitAlert": false,
    "debug.terminal.clearBeforeReusing": true,
    "terminal.integrated.confirmOnKill": "never"
}
```

## Misc

### VS Build Tools 2022

Haven't tried it with rustup, but the link is [available here](https://aka.ms/vs/17/release/vs_buildtools.exe).

### Moving RustUp and Cargo to a different directory

By default it is installed in `%USERPROFILE%\.cargo` and `%USERPROFILE%\.rustup`. You can move it to a more convient location by adding/modifying the windows environment variables:

* `CARGO_HOME` (eg set to `c:\programs\cargo`)
* `RUSTUP_HOME` (eg set to `c:\programs\rustup`)
* `PATH` (eg add `c:\programs\cargo\bin`)

### Portable VS Code

By downloading the `Zip` version, it can be made portable by creating a folder called `data` inside its directory.
 
### Creating a VS Build Tools Offline Installer

```vs_buildtools.exe --layout d:\localVScache --includeRecommended --lang en-US --add Microsoft.VisualStudio.Workload.VCTools```

More information:

* [Create an offline installation of Visual Studio (2019)](https://docs.microsoft.com/en-us/visualstudio/install/create-an-offline-installation-of-visual-studio?view=vs-2019)
* [Visual Studio Build Tools component directory (2019)](https://docs.microsoft.com/en-us/visualstudio/install/workload-component-id-vs-build-tools?view=vs-2019)

### Breakpoints

I could only get breakpoints working by going to the ```main``` function in my project and clicking the "Debug" text button above it.

### Modifying external crates

If you have an issue with an external crate library, you can find its source from somewhere in `.cargo\library\` and copy it else where and modify it.

Then in your `cargo.toml` file add something like:

```toml
[patch.crates-io]
gilrs-core = { path = "../crates/gilrs-core-0.3.1" }
```
### Updating Rust and Cargo

```rustup update```
