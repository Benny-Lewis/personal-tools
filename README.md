# personal-tools

Personal Claude Code plugin for everyday workflow automations.

## Skills

### cleanup-notepad

Triage unsaved Notepad++ tabs. Reads backup files, cross-references against existing project files, and categorizes each as save or delete.

```
/cleanup-notepad
```

- Reads `session.xml` to identify only currently open tabs
- Checks for duplicates, completed work, temporary pastes, and AI-generated scratch
- Proposes save locations using existing directory structure
- Waits for approval before saving anything

## Install

Add the marketplace, then browse and install:

```
/plugin marketplace add Benny-Lewis/Benny-Lewis-plugins
/plugin
```
