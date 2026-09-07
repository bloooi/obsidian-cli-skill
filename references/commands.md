# Obsidian CLI — Full Command Reference

> Requirements: Obsidian 1.12+ (installed version; App Store sandbox version not supported)

## Table of Contents
- [General](#general)
- [Bases](#bases)
- [Bookmarks](#bookmarks)
- [Command Palette](#command-palette)
- [Daily Notes](#daily-notes)
- [File History](#file-history)
- [Files & Folders](#files--folders)
- [Links](#links)
- [Outline](#outline)
- [Plugins](#plugins)
- [Properties](#properties)
- [Publish](#publish)
- [Random Notes](#random-notes)
- [Search](#search)
- [Sync](#sync)
- [Tags](#tags)
- [Tasks](#tasks)
- [Templates](#templates)
- [Themes & Snippets](#themes--snippets)
- [Unique Notes](#unique-notes)
- [Vault](#vault)
- [Web Viewer](#web-viewer)
- [Wordcount](#wordcount)
- [Workspace](#workspace)
- [Developer Commands](#developer-commands)
- [TUI Keyboard Shortcuts](#tui-keyboard-shortcuts)

---

## General

| Command | Description |
|---------|-------------|
| `help` | Show help |
| `version` | Print Obsidian version |
| `reload` | Reload vault |
| `restart` | Restart Obsidian |

---

## Bases

| Command | Description |
|---------|-------------|
| `bases` | List all Bases in vault |
| `base:views` | List all views in a Base |
| `base:create` | Create a new Base |
| `base:query` | Run a Base query |

```bash
obsidian bases
obsidian base:views file="MyBase"
obsidian base:create title="Project Tracker"
obsidian base:query file="MyBase" view="Default View" query="status=complete"
```

---

## Bookmarks

| Command | Description |
|---------|-------------|
| `bookmarks` | List all bookmarks |
| `bookmark` | Open a specific bookmark |

```bash
obsidian bookmarks
obsidian bookmark title="Important Note"
```

---

## Command Palette

| Command | Description |
|---------|-------------|
| `commands` | List all available commands |
| `command` | Execute a specific command |
| `hotkeys` | List all hotkeys |
| `hotkey` | Show info for a specific hotkey |

```bash
obsidian commands
obsidian command id="editor:toggle-bold"
obsidian hotkeys
obsidian hotkey id="editor:toggle-bold"
```

---

## Daily Notes

| Command | Description |
|---------|-------------|
| `daily` | Open today's daily note |
| `daily:path` | Print daily note path |
| `daily:read` | Read daily note content |
| `daily:append` | Append content to daily note |
| `daily:prepend` | Prepend content to daily note |

```bash
obsidian daily
obsidian daily:path
obsidian daily:read
obsidian daily:read --copy
obsidian daily:append content="- Task done"
obsidian daily:prepend content="## Goals"
obsidian daily:read date="2024-01-15"
obsidian daily:append date="yesterday" content="Retrospective"
```

---

## File History

Requires Obsidian Sync.

| Command | Description |
|---------|-------------|
| `diff` | Compare current and previous version |
| `history` | List file history |
| `history:list` | List history entries |
| `history:read` | Read a specific history entry |
| `history:restore` | Restore to a specific version |
| `history:open` | Open history viewer |

```bash
obsidian diff file="Project Plan"
obsidian history file="Note"
obsidian history:read file="Note" version=3
obsidian history:restore file="Note" version=2
```

---

## Files & Folders

### File Commands

| Command | Description |
|---------|-------------|
| `file` | Show file info |
| `files` | List all files in vault |
| `open` | Open a file |
| `create` | Create a new file |
| `read` | Read file content |
| `append` | Append content to file |
| `prepend` | Prepend content to file |
| `move` | Move a file |
| `rename` | Rename a file (`name=`, not `title=`) |
| `delete` | Delete a file |

```bash
obsidian file file="Note Name"
obsidian files
obsidian files folder="Projects"
obsidian open file="Note Name"
obsidian open path="folder/note.md"
obsidian create name="New Note" content="content"
obsidian create path="FolderName/New Note.md" content="content"
obsidian read file="Note Name"
obsidian read file="Note Name" --copy
obsidian append file="Note" content="Content"
obsidian prepend file="Note" content="Header"
obsidian move file="Note" folder="New Folder"
obsidian rename file="Old Name" name="New Name"
obsidian delete file="Note to Delete"
```

Prefer path= for an exact vault-relative location. The current CLI may ignore title= and folder=, so verify the resulting file before cleanup.

### Folder Commands

| Command | Description |
|---------|-------------|
| `folder` | Show folder info |
| `folders` | List all folders in vault |

```bash
obsidian folder path="Projects"
obsidian folders
```

---

## Links

| Command | Description |
|---------|-------------|
| `backlinks` | List backlinks to a file |
| `links` | List outgoing links from a file |
| `unresolved` | List unresolved links |
| `orphans` | List files with no links |
| `deadends` | List links to non-existent files |

```bash
obsidian backlinks file="Note Name"
obsidian links file="Note Name"
obsidian unresolved
obsidian orphans
obsidian deadends
```

---

## Outline

| Command | Description |
|---------|-------------|
| `outline` | Show heading structure of a file |

```bash
obsidian outline file="Note Name"
obsidian outline file="Note Name" --copy
```

---

## Plugins

| Command | Description |
|---------|-------------|
| `plugins` | List all installed plugins |
| `plugins:enabled` | List enabled plugins |
| `plugins:restrict` | Toggle restricted mode |
| `plugin` | Show info for a specific plugin |
| `plugin:enable` | Enable a plugin |
| `plugin:disable` | Disable a plugin |
| `plugin:install` | Install a plugin |
| `plugin:uninstall` | Uninstall a plugin |
| `plugin:reload` | Reload a plugin |

```bash
obsidian plugins
obsidian plugin id="dataview"
obsidian plugin:enable id="dataview"
obsidian plugin:disable id="dataview"
obsidian plugin:install id="dataview"
obsidian plugin:reload id="dataview"
```

---

## Properties

| Command | Description |
|---------|-------------|
| `aliases` | List aliases for a file |
| `properties` | List all properties of a file |
| `property:set` | Set a property value |
| `property:remove` | Remove a property |
| `property:read` | Read a specific property value |

```bash
obsidian aliases file="Note Name"
obsidian properties file="Note Name"
obsidian property:set file="Note" key="status" value="complete"
obsidian property:set file="Note" key="tags" value='["tag1", "tag2"]'
obsidian property:remove file="Note" key="draft"
obsidian property:read file="Note" key="status"
```

---

## Publish

| Command | Description |
|---------|-------------|
| `publish:site` | Show Publish site info |
| `publish:list` | List published files |
| `publish:status` | Check publish status |
| `publish:add` | Add file to publish |
| `publish:remove` | Remove file from publish |
| `publish:open` | Open Publish site |

---

## Random Notes

| Command | Description |
|---------|-------------|
| `random` | Open a random note |
| `random:read` | Read a random note's content |

```bash
obsidian random
obsidian random:read --copy
```

---

## Search

| Command | Description |
|---------|-------------|
| `search` | Search within vault |
| `search:context` | Search results with surrounding context |
| `search:open` | Open search panel |

```bash
obsidian search query="keyword"
obsidian search query="tag:#project"
obsidian search query="path:folder keyword"
obsidian search:context query="keyword"
obsidian search:context query="keyword" --copy
```

**Search syntax:**
- `"exact phrase"` — exact phrase
- `tag:#tagname` — tag search
- `path:foldername` — path search
- `file:filename` — filename search

---

## Sync

| Command | Description |
|---------|-------------|
| `sync` | Check sync status |
| `sync:status` | Detailed sync status |
| `sync:history` | Sync history |
| `sync:read` | Read synced file content |
| `sync:restore` | Restore to synced version |
| `sync:open` | Open Sync settings |
| `sync:deleted` | List deleted files |

---

## Tags

| Command | Description |
|---------|-------------|
| `tags` | List all tags in vault |
| `tag` | List files with a specific tag |

```bash
obsidian tags
obsidian tag tag="project"
obsidian tag tag="#task"
```

---

## Tasks

| Command | Description |
|---------|-------------|
| `tasks` | List all tasks in vault |
| `task` | List tasks in a specific file |

```bash
obsidian tasks
obsidian tasks status=incomplete
obsidian tasks status=complete
obsidian task file="Todo List" status=incomplete
```

---

## Templates

| Command | Description |
|---------|-------------|
| `templates` | List all available templates |
| `template:read` | Read template content |
| `template:insert` | Insert template into current file |

```bash
obsidian templates
obsidian template:read template="Meeting Notes Template"
obsidian template:insert template="Meeting Notes Template" file="Today's Meeting"
```

---

## Themes & Snippets

### Themes

| Command | Description |
|---------|-------------|
| `themes` | List installed themes |
| `theme` | Show current theme info |
| `theme:set` | Change theme |
| `theme:install` | Install a theme |
| `theme:uninstall` | Uninstall a theme |

### CSS Snippets

| Command | Description |
|---------|-------------|
| `snippets` | List all CSS snippets |
| `snippets:enabled` | List enabled snippets |
| `snippet:enable` | Enable a snippet |
| `snippet:disable` | Disable a snippet |

---

## Unique Notes

| Command | Description |
|---------|-------------|
| `unique` | Create a new Zettelkasten unique note |

```bash
obsidian unique title="Idea Title"
```

---

## Vault

| Command | Description |
|---------|-------------|
| `vault` | Show current vault info |
| `vaults` | List all vaults |
| `vault:open` | Open another vault |

```bash
obsidian vault
obsidian vaults
obsidian vault:open vault="Personal Vault"
# Multi-vault: specify vault as first param
obsidian vault=MyVault <command>
```

---

## Web Viewer

```bash
obsidian web url="https://obsidian.md"
```

---

## Wordcount

```bash
obsidian wordcount file="Note Name"
obsidian wordcount file="Note Name" --copy
```

---

## Workspace

| Command | Description |
|---------|-------------|
| `workspace` | Show current workspace info |
| `workspaces` | List all workspaces |
| `workspace:save` | Save workspace |
| `workspace:load` | Load workspace |
| `workspace:delete` | Delete workspace |
| `tabs` | List currently open tabs |
| `tab:open` | Open file in new tab |
| `recents` | List recently opened files |

```bash
obsidian workspace:save name="Work Layout"
obsidian workspace:load name="Work Layout"
obsidian tabs
obsidian tab:open file="Note Name"
obsidian recents
```

---

## Developer Commands

| Command | Description |
|---------|-------------|
| `devtools` | Open Chrome DevTools |
| `dev:debug` | Print debug info |
| `dev:cdp` | Enable Chrome DevTools Protocol |
| `dev:errors` | List console errors |
| `dev:screenshot` | Capture screenshot |
| `dev:console` | Print console logs |
| `dev:css` | Print CSS variables |
| `dev:dom` | Print DOM structure |
| `dev:mobile` | Toggle mobile mode |
| `eval` | Execute JavaScript code |

```bash
obsidian eval code="app.vault.getName()"
obsidian eval code="app.workspace.getActiveFile()?.basename"
obsidian dev:screenshot path="screenshot.png"
```

---

## TUI Keyboard Shortcuts

Enter TUI mode with `obsidian` (no arguments).

### Cursor Movement
| Shortcut | Action |
|----------|--------|
| `←` / `Ctrl+B` | Move one character left |
| `→` / `Ctrl+F` | Move one character right |
| `Alt+B` | Move one word left |
| `Alt+F` | Move one word right |
| `Ctrl+A` | Move to beginning of line |
| `Ctrl+E` | Move to end of line |

### Editing
| Shortcut | Action |
|----------|--------|
| `Ctrl+U` | Delete everything before cursor |
| `Ctrl+K` | Delete everything after cursor |
| `Ctrl+W` / `Alt+Backspace` | Delete previous word |

### Autocomplete & History
| Shortcut | Action |
|----------|--------|
| `Tab` | Next autocomplete item |
| `Shift+Tab` | Previous autocomplete item |
| `↑` / `Ctrl+P` | Previous command |
| `↓` / `Ctrl+N` | Next command |
| `Ctrl+R` | Reverse history search |

### Other
| Shortcut | Action |
|----------|--------|
| `Enter` | Execute command |
| `Escape` | Cancel / Exit TUI |
| `Ctrl+L` | Clear screen |
| `Ctrl+C` / `Ctrl+D` | Exit TUI |
