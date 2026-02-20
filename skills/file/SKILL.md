---
name: file
description: Open a file or folder in the system file manager. Use when the user wants to view, locate, or reveal a file in Explorer, Finder, or their Linux file browser.
disable-model-invocation: true
argument-hint: "[file or instructions]"
allowed-tools: Bash, Glob, Grep, Read
---

# Open in File Manager

Open a file's containing folder in the OS file manager with the file selected (or open a directory directly).

## Resolve the target

Determine the target file or directory from `$ARGUMENTS`:

1. **Direct path** — If the argument is an existing absolute or relative file path, use it as-is
2. **Filename** — If it looks like a filename (has an extension or matches a known file), use Glob to find it in the current working directory tree. If multiple matches, ask the user which one
3. **Natural language** — If it's a description (e.g. "the main config", "test helpers"), interpret the intent and use Glob/Grep to find the most likely file
4. **Empty** — If no arguments, infer from conversation context: use the most recently discussed, edited, or relevant file

If the target cannot be found, report what you searched for and suggest alternatives.

If the target is a directory, open it directly (skip the "select file" behavior).

## Open it

Detect the platform and run the appropriate command:

**Windows (win32):**
```bash
explorer.exe /select,"$(cygpath -w "<absolute-path>")"
```

**macOS (darwin):**
```bash
open -R "<absolute-path>"
```

**Linux:**
```bash
xdg-open "$(dirname "<absolute-path>")"
```

Use the platform value from your environment context. Convert to absolute path before running the command. On Windows/MINGW, use `cygpath -w` to convert to Windows-native paths for explorer.exe.

After opening, confirm what you opened: "Opened `<folder>` with `<file>` selected."
