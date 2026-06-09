# Diffre

A fast, local file and folder diff tool — like Beyond Compare, built for the desktop.

Powered by VS Code's Monaco editor for pixel-perfect syntax-highlighted diffs, with a clean dark UI and no telemetry.

---

## Features

**File Diff**
- Side-by-side diff with full syntax highlighting (auto-detected language)
- Navigate changes chunk by chunk (↑ Prev / ↓ Next)
- Edit both sides directly in the editor
- Copy to Left / Copy to Right — push individual chunks between sides
- Save left or right independently (⌘S / Ctrl+S respects which side is focused)
- Word Wrap and Ignore Whitespace toggles
- Unsaved changes warning when switching files or closing

**Folder Compare**
- Recursive directory diff powered by blake3 hashing — fast even on large trees
- Tree view grouped by subdirectory, collapsible
- Status filter tabs: Modified / Left-only / Right-only / Same / All
- Filename search with auto-expand of matching parent nodes
- Drag-and-drop to open files or folders
- Refresh without losing the currently open file
- Copy path from right-click context menu
- Copy folder tree as ASCII text

**Configuration**
- Global settings + per-session overrides
- Font size, word wrap, whitespace ignore, show/hide same files and binary files
- Ignore rules: auto-reads `.gitignore` + custom glob patterns
- Settings changes trigger automatic re-scan

**Sessions**
- Automatically restores the last session on launch
- Recent sessions list on the Welcome screen

---

## Download

Go to the [Releases](../../releases) page and download the installer for your platform:

| Platform | File |
|----------|------|
| macOS (Apple Silicon / Intel) | `.dmg` |
| Windows | `.msi` or `.exe` |
| Linux | `.AppImage` or `.deb` |

---

## Screenshots

_Coming soon._

---

## Feedback & Bug Reports

Found a bug, have a feature idea, or just want to share how you use Diffre?

**[Open an Issue](../../issues/new/choose)** — all feedback is welcome and helps shape the next release.

---

## System Requirements

- macOS 11+, Windows 10+, or a modern Linux distribution
- No runtime dependencies — fully self-contained

---

## License

Proprietary. Binaries are provided for personal use. Source code is not publicly available yet.
