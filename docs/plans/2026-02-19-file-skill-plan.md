# /file Skill Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Add a `/file` slash command to personal-tools that opens a file's containing folder in the OS file manager with the file selected.

**Architecture:** Pure markdown skill — Claude resolves the target file using Glob/Grep/context, then runs one OS-specific shell command. No helper scripts.

**Tech Stack:** Markdown (SKILL.md), Bash (explorer.exe / open -R / xdg-open)

---

### Task 1: Create SKILL.md

**Files:**
- Create: `skills/file/SKILL.md`

**Step 1: Write the skill file**

Create `skills/file/SKILL.md` with this exact content:

```markdown
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
```

**Step 2: Commit**

```bash
cd ~/dev/personal-tools
git add skills/file/SKILL.md
git commit -m "feat: add /file skill to open files in system file manager"
```

---

### Task 2: Update README.md

**Files:**
- Modify: `README.md`

**Step 1: Add the /file skill to the README**

Add a new section after the `cleanup-notepad` section in `README.md`:

```markdown
### file

Open a file's containing folder in the system file manager with the file selected.

\```
/file package.json
/file the main config
/file src/components/Header.tsx
/file
\```

- Resolves filenames, paths, natural language descriptions, or infers from context
- Cross-platform: Windows Explorer, macOS Finder, Linux xdg-open
```

**Step 2: Commit**

```bash
cd ~/dev/personal-tools
git add README.md
git commit -m "docs: add /file skill to README"
```

---

### Task 3: Bump plugin version

**Files:**
- Modify: `plugin.json`

**Step 1: Bump version from 1.0.0 to 1.1.0**

Change `"version": "1.0.0"` to `"version": "1.1.0"` in `plugin.json`.

**Step 2: Commit**

```bash
cd ~/dev/personal-tools
git add plugin.json
git commit -m "chore: bump version to 1.1.0 for /file skill"
```

---

### Task 4: Test the skill

**Step 1: Load the plugin locally and test**

```bash
claude --plugin-dir ~/dev/personal-tools
```

Then test these scenarios:
- `/file plugin.json` — should open the personal-tools root with plugin.json selected
- `/file SKILL.md` — should find skills/file/SKILL.md or skills/cleanup-notepad/SKILL.md and ask which one
- `/file` (no args, after discussing a file) — should infer from context

**Step 2: Verify each test opens the file manager correctly**

If any issues are found, fix the SKILL.md and amend the commit from Task 1.

---

### Task 5: Push to remote

**Step 1: Push all commits**

```bash
cd ~/dev/personal-tools
git push origin main
```
