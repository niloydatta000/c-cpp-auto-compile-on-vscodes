
# 🛠️ C and C++ Smart Build Setup for VS Code (Windows)

This repository provides a ready-to-use **automated build system** for beginners and intermediate users of **C and C++** in **Visual Studio Code** on Windows. It smartly detects file types, applies appropriate compilers, and offers **debug/release build modes**, all configurable through PowerShell and VS Code's `tasks.json`.

---

## 📌 Purpose

Many C/C++ learners struggle to configure compilers or toggle between C and C++ projects in VS Code. This repo simplifies that process by:

- Automatically selecting `gcc` for `.c` files and `g++` for `.cpp` files.
- Supporting both debug and release modes.
- Enabling smooth debugging via GDB.
- Helping users focus on coding rather than configuration.

---

## 📂 What's Included?

- ✅ `smart_build.ps1`: A PowerShell script that intelligently compiles based on file type and build mode.
- ✅ `.vscode/tasks.json`: Pre-configured tasks for building in **debug** or **release**.
- ✅ `.vscode/launch.json`: A GDB-based debugging setup for MSYS2 users.
- ✅ `main.c` and `main.cpp`: Classic **"Hello, World"** examples for both languages.

All files are bundled for convenience so programmers can start writing and running C/C++ code immediately without setup headaches.

---

## 🚀 Quick Start

1. ✅ Make sure `gcc`, `g++`, and `gdb` are installed (e.g., via [MSYS2](https://www.msys2.org/)).
2. ✅ Clone this repository.
3. ✅ Open the folder in VS Code.
4. ✅ Hit `Ctrl+Shift+B` to compile in debug mode or run **Smart Build (Release)** from the task list.
5. ✅ Press `F5` to launch the debugger.

---

## 📋 Compiler Check

Run these in terminal to confirm compiler availability:
```sh
gcc --version
g++ --version
gdb --version
```

---

## 🧰 Customize or Reuse

Want to use this setup in all future projects?

1. Move `smart_build.ps1` to a global location like `C:\scripts\smart_build.ps1`.
2. Update the path in `.vscode/tasks.json`.
3. Copy the `.vscode` folder into new projects.

---

## 📎 Reference

Installation guide: [freeCodeCamp - How to Install C and C++ Compiler on Windows](https://www.freecodecamp.org/news/how-to-install-c-and-cpp-compiler-on-windows/)
