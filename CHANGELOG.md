# Changelog

## v0.1.27

### Bug Fixes
- **Auto-updater now works.** Previous versions (0.1.23–0.1.26) never checked for updates due to a combination of a broken deploy pipeline, a Tauri v2 API change, and missing permissions. If you're on 0.1.26 or earlier, you'll need to manually download this update — after that, automatic updates will work as expected.

---

## v0.1.26

### New Features
- **Ruleset system**: save reusable sets of file-pairing rules globally and apply them across any session, with per-rule inline editing
- **Equivalent term matching**: pair files with differently-named counterparts via term substitution (e.g. `about-authors` ≡ `关于作者`), works in either direction and with partial filename matches
- **Image comparison**: side-by-side view for PNG/JPG/GIF/WebP and other image files, with an overlay mode to highlight pixel differences
- **Lazy-loaded directory tree**: large directories now expand on demand instead of being scanned all at once, improving initial load time
- **Session ignore inheritance**: session-level ignore patterns can now extend the global ignore list instead of fully replacing it
- **More correct .gitignore handling**: nested `.gitignore` files, `~/.config/git/ignore`, and `.git/info/exclude` are now all respected

### Bug Fixes
- **Fixed the app getting stuck on a loading screen after a fresh install.** The bundled code editor's assets were being blocked by the app's content security policy; the editor is now bundled locally instead of loaded from a CDN
- Fixed confirmation dialogs being unreliable on Linux; replaced with a consistent in-app dialog
- Fixed directory paths with a trailing slash producing malformed file paths
- Fixed several directory-scanning edge cases around hidden files and nested ignore rules

### Other
- Added optional, anonymized usage analytics (which features get used) to help prioritize future development

---

## v0.1.25

### New Features
- **VS Code drag-drop**: files and folders dragged from VS Code Explorer now open correctly
- **Manual pair indicator**: manual pairings (Pick Counterpart) are visually distinct from rule-matched pairs
- **Error display**: errors moved to the bottom status bar with a Dismiss button

### Bug Fixes
- Fixed session settings (e.g. Respect .gitignore) being silently overwritten on re-scan

---

## v0.1.22 → current

### New Features

#### ⇄ Swap Left / Right
- New Swap button in the toolbar
- File mode: swaps paths, content, and dirty state for both sides
- Dir mode: re-scans with left and right directories swapped

#### Custom File Mapping
Automatically pairs files with different names across directories (e.g. `ch01.md` ↔ `第01章.md`).

**Rule-based pairing (Extracting Tools + Mapping Rules)**
- Global Extracting Tools: define named extractors (Regex or JS script) that extract a canonical key from a filename
- Session Mapping Rules: configure a left and right extractor per rule; files whose extracted keys match exactly are paired
- JS scripts run in a Web Worker sandbox with a 200 ms timeout; no DOM or Tauri IPC access
- Multiple rules are applied in order; the first matching rule wins

**Manual pairing (Pick Counterpart)**
- Click a `left_only` file → "Pick counterpart →" button appears in the DiffView toolbar
- Enter pick mode: the tree switches to a flat list of right-side files with a search box
- Select a file to pair it; a "Save pairing" button persists the pair to the session

**Match priority**: manual pairs → rule-based pairs → same relative path (default)

**Paired file indicator**: paired files show a ↔ blue icon in the directory tree

#### CLI Support
- Pass two paths on the command line to open a diff directly; file vs. dir mode is detected automatically
- Invalid paths show an error prompt

#### Drag-and-Drop Robustness
- Same-path deduplication: dropping the same file twice (2-path or pending + repeat) shows an error instead of silently replacing
- Tauri double-fire deduplication: a second drag event for the same path within 500 ms is silently discarded

---

### Settings Improvements

#### Live Save (removed Save button)
- All setting changes are saved immediately with no manual Save step
- Text inputs debounce at 400 ms; adding or removing a tool/rule saves instantly

#### Mapping Configuration UI
- Extracting Tools support inline editing (✎ button per row)
- Inline extractor tester: type a filename and see the extracted keys in real time
- Inline rule match tester: enter left and right filenames to see whether the rule would pair them
- Settings panel re-fetches tools and rules from disk on every open, preventing stale data

#### Mapping Settings UX
- Shared Test filename input at the top of the Mapping tab, used by all extractors
- "← current file" button fills the test input with the filename currently selected in the directory tree
- Existing tool rows show live test results inline when a test filename is entered
- Clone button (⎘) next to each tool's edit button — copies name + content into the Add form
- Switching to JS script type pre-fills a working example instead of leaving an empty textarea
- Regex placeholder fix: use JSX expression `{"e.g. ch(\\d+)"}` to avoid double-backslash display

#### Settings UX Polish
- Pressing Enter in the tool name or pattern input saves the tool (add and edit forms)
- Last selected tab is remembered in `localStorage` and restored on reopen
- Unsaved form drafts (tool name/content, rule extractors, test inputs) are persisted to `localStorage` and restored on reopen
- Fix: releasing the mouse outside the Settings panel after dragging from inside no longer closes it

---

### Bug Fixes

| Fix | Details |
|-----|---------|
| Right-click text selection in dir tree | WebKit doesn't reliably inherit `user-select: none`; set it directly on `.dir-entry` and `.tree-dir-row` |
| Global tools disappear on Settings reopen | Panel now fetches from disk on mount instead of relying on potentially stale store state |
| Session rules disappear on Settings reopen | Panel fetches `getSessionRecord` on mount to correctly initialize rules |
| `window.confirm` throws in Tauri | Replaced with an inline React confirmation UI |
| `saveGlobal` silently swallowed errors | Now returns `boolean`, wraps IPC call in try/catch, and surfaces errors in the UI |
| Empty rule could be added | Add Rule button is disabled when either extractor has no content |
| JS extractor silently returns nothing without `return` | Auto-detect: if the script contains no `return` keyword, wrap it as `return (\n...\n)` |
| "TextModel disposed before reset" error | Call `setModel(null)` before `dispose()` on unmount to release model refs in the correct order |
| Settings values not taking effect | All config fields now wired: `word_wrap`, `font_size`, `ignore_whitespace`, `show_same_files`, `show_binary_files`, `context_lines`, `tab_size`, `insert_spaces` |
| Debug log panel always visible | Now off by default; toggle via Settings → Display or force with `?debug=1` |
| No way to remove recent sessions | Hovering a session entry reveals a × button to delete it |
| Browser autocomplete / autocorrect on inputs | Added `autoComplete="off"`, `autoCapitalize="none"`, `autoCorrect="off"` to all text inputs |
