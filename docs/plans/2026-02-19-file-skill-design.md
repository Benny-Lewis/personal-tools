# /file Skill Design

## Summary

A user-invokable skill (`/file`) that opens a file's containing folder in the OS file manager with the file selected. Accepts optional arguments: a file path, filename, natural language instructions, or nothing (infers from context).

## Location

`personal-tools/skills/file/SKILL.md` — added to the existing personal-tools plugin.

## Behavior

1. **Direct path** (`/file src/index.ts`) — use as-is
2. **Filename** (`/file package.json`) — Glob to find it
3. **Natural language** (`/file the main config`) — interpret and search
4. **Empty** (`/file`) — infer from conversation context (most recently discussed file)

## OS Commands

- **Windows:** `explorer.exe /select,"C:\path\to\file"`
- **macOS:** `open -R "/path/to/file"`
- **Linux:** `xdg-open "$(dirname "/path/to/file")"`

## Edge Cases

- Multiple matches: ask user which one
- Not found: report and suggest alternatives
- Directory argument: open the directory directly

## Frontmatter

- `disable-model-invocation: true` (user-triggered only)
- `argument-hint: [file or instructions]`
- `allowed-tools: Bash, Glob, Grep, Read`
