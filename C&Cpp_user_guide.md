# C and C++ Compiler Switch Using PowerShell in VS Code

C and C++ programming use different compilers. They use `gcc.exe` and `g++.exe` compilers respectively on Windows. See [Installation Guide](https://www.freecodecamp.org/news/how-to-install-c-and-cpp-compiler-on-windows/). Use the following commands to verify compilers are installed successfully:
```shell
gcc --version
g++ --version
gdb --version
```
If any of these commands do not fail, Your compilers have been set up successfully.

Here's a fully automated, future-proof build setup for C and C++ in VS Code, with smart compiler detection, debug/release modes, and room to grow.
Let's assume compilers downloaded from `MinGW` and `x86_64` version is being used.

## **Create the Smart PowerShell Script**

Create a file called `smart_build.ps1` anywhere (e.g., in `C:\scripts\ ` or inside your project folder).

**`smart_build.ps1`**:
```powershell
param (
    [string]$filePath,
    [string]$buildType = "debug"
)

$ext = [System.IO.Path]::GetExtension($filePath)
$outputPath = [System.IO.Path]::ChangeExtension($filePath, '.exe')

# Choose compiler based on file extension
switch ($ext) {
    ".c"   { $compiler = "C:\msys64\mingw64\bin\gcc.exe" }
    ".cpp" { $compiler = "C:\msys64\mingw64\bin\g++.exe" }
    default {
        Write-Host "Unsupported file type: $ext"
        exit 1
    }
}

# Set compiler flags based on build type
switch ($buildType.ToLower()) {
    "debug"   { $flags = "-g" }
    "release" { $flags = "-O2" }
    default   { $flags = "-g" } # For anything else safely switched to `debug` mode
}

Write-Host "`n>>> Compiling $filePath as $buildType..."
& $compiler $flags "$filePath" -o "$outputPath"
```

## **Add It to VS Code: `tasks.json`**

Let the script file located inside your working directory or project folder.
Go to **`.vscode/tasks.json`** and use this:
```JSON
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "Smart Build (Debug)",
      "type": "shell",
      "command": "powershell",
      "args": [
        "-ExecutionPolicy", "Bypass",
        "-File", "${workspaceFolder}\\smart_build.ps1",
        "${file}",
        "debug"
      ],
      "problemMatcher": ["$gcc"],
      "group": {
        "kind": "build",
        "isDefault": true
      }
    },
    {
      "label": "Smart Build (Release)",
      "type": "shell",
      "command": "powershell",
      "args": [
        "-ExecutionPolicy", "Bypass",
        "-File", "${workspaceFolder}\\smart_build.ps1",
        "${file}",
        "release"
      ],
      "problemMatcher": ["$gcc"]
    }
  ]
}
```

## **Bonus: `launch.json` for Debugging**

If you're using `gdb` via **MSYS2**, create or update **`.vscode/launch.json`** like this:
```JSON
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Debug (C/C++)",
      "type": "cppdbg",
      "request": "launch",
      "program": "${fileDirname}/${fileBasenameNoExtension}.exe",
      "args": [],
      "stopAtEntry": true,
      "cwd": "${fileDirname}",
      "environment": [],
      "externalConsole": true,
      "MIMode": "gdb",
      "miDebuggerPath": "C:/msys64/mingw64/bin/gdb.exe",
      "setupCommands": [
        {
          "description": "Enable pretty-printing for gdb",
          "text": "-enable-pretty-printing",
          "ignoreFailures": true
        }
      ]
    }
  ]
}
```

## **How You Use It**

- Debug build:
Ctrl + Shift + B (default)

- Release build:
Press Ctrl + Shift + P → Run Task → Smart Build (Release)

- Debug your app:
Press F5

### Optional: Make It Global for All Projects

1. Move `smart_build.ps1` to a global location like `C:\scripts\smart_build.ps1`

2. Update `tasks.json` path accordingly.

3. Copy the `.vscode` folder as a template to all your new projects.


