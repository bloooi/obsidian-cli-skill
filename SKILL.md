---
name: obsidian-cli
description: >
  Interact with the Obsidian vault using only the `obsidian` CLI command via Bash.
  Use this skill when the user wants to read, create, edit, search, append, or manage notes
  in their Obsidian vault.
  PRIORITY ACTIVATION: When the request contains any of "문서", "vault", "obsidian", "볼트", "노트",
  prioritize this skill for all CRUD operations.
  Korean triggers — document CRUD: "문서를 수정", "문서를 봤는데", "문서의 내용을", "문서를 만들어", "문서를 삭제".
  File reference triggers: "*.md 문서를 보니", "*.md 파일을".
  Vault triggers: "vault를 찾겠다", "vault에서", "볼트에서".
  Also triggers on: "Obsidian에 저장해줘", "볼트에서 찾아줘", "노트 만들어줘", "데일리 노트에 추가해줘",
  "볼트에 있는 노트 읽어줘", or any request to interact with Obsidian notes.
  IMPORTANT: Always use the `obsidian` CLI via Bash tool — never use MCP obsidian tools.
---

# Obsidian CLI

**Core rule**: All Obsidian vault operations use the `obsidian` CLI via Bash tool exclusively. Never use MCP obsidian tools.

Run `obsidian help` to see all available commands. This is always up to date.

## Syntax

**Parameters** take a value with `=`. Quote values with spaces:

```bash
obsidian create name="My Note" content="Hello world"
```

**Flags** are boolean switches with no value:

```bash
obsidian create name="My Note" silent overwrite
```

For multiline content use `\n` for newline and `\t` for tab.

## File targeting

Many commands accept `file` or `path` to target a file. Without either, the active file is used.

- `file=<name>` — resolves like a wikilink (name only, no path or extension needed)
- `path=<path>` — exact path from vault root, e.g. `folder/note.md`

## Vault targeting

Commands target the most recently focused vault by default. Use `vault=<name>` as the first parameter to target a specific vault:

```bash
obsidian vault="My Vault" search query="test"
```

## Global flags

- `--copy` — on any command, copies output to clipboard
- `silent` — prevents files from opening in Obsidian
- `total` — on list commands, returns only a count

## Common Operations

### Read a note
```bash
obsidian read file="Note Name"         # wiki-link style (name only)
obsidian read path="folder/note.md"    # exact path
```

### Create a note
`obsidian create` often creates "Untitled.md" regardless of the `title` parameter — always rename afterward:
```bash
obsidian create title="My Note" content="Initial content"
obsidian rename file="Untitled" name="My Note"
```

### Append / Prepend content
```bash
obsidian append file="Note Name" content="New content"
obsidian prepend file="Note Name" content="Header content"
```

### Search
```bash
obsidian search query="keyword"
obsidian search query="tag:#project"
obsidian search query="path:folder keyword"
obsidian search:context query="keyword"   # includes surrounding context
```

### Daily note
```bash
obsidian daily:read
obsidian daily:append content="- Task done"
obsidian daily:prepend content="## Goals"
obsidian daily:read date="2024-01-15"
```

### File management
```bash
obsidian files                            # list all files
obsidian files folder="Projects"
obsidian move file="Note" folder="Archive"
obsidian rename file="Old Name" name="New Name"   # use name=, not title=
obsidian delete file="Note Name"
```

## Known Gotchas

| Issue | Fix |
|-------|-----|
| `create` ignores `title=`, creates "Untitled.md" | Always follow `create` with `rename file="Untitled" name="..."` |
| `rename` parameter is `name=`, not `title=` | `obsidian rename file="X" name="Y"` |
| Content too long for one `create` call | Use `create` for first chunk, then multiple `append` calls |

## Bash Quoting for Content

Use `$'...'` quoting when content has newlines, single quotes, or special characters:

```bash
# Newlines with $'...'
obsidian append file="Note" content=$'## Section\n\nParagraph here.'

# Single quotes inside $'...' must be escaped as \'
obsidian append file="Note" content=$'Add to ~/.zprofile or ~/.bash_profile'

# For simple content without special chars, regular quotes work
obsidian append file="Note" content="Simple line"
```

In `$'...'` strings:
- `\n` → newline
- `\t` → tab
- `\'` → literal single quote
- `\\` → literal backslash
- `"` → no escaping needed

## Long Documents (Multi-chunk Pattern)

For long content, split into create + sequential appends:
```bash
obsidian create title="Long Doc" content=$'# Title\n\nFirst section...'
obsidian rename file="Untitled" name="Long Doc"
obsidian append file="Long Doc" content=$'\n## Section 2\n\nContent...'
obsidian append file="Long Doc" content=$'\n## Section 3\n\nContent...'
```

## Plugin development

### Develop/test cycle

After making code changes to a plugin or theme, follow this workflow:

1. **Reload** the plugin to pick up changes:
   ```bash
   obsidian plugin:reload id=my-plugin
   ```
2. **Check for errors** — if errors appear, fix and repeat from step 1:
   ```bash
   obsidian dev:errors
   ```
3. **Verify visually** with a screenshot or DOM inspection:
   ```bash
   obsidian dev:screenshot path=screenshot.png
   obsidian dev:dom selector=".workspace-leaf" text
   ```
4. **Check console output** for warnings or unexpected logs:
   ```bash
   obsidian dev:console level=error
   ```

### Additional developer commands

Run JavaScript in the app context:

```bash
obsidian eval code="app.vault.getFiles().length"
```

Inspect CSS values:

```bash
obsidian dev:css selector=".workspace-leaf" prop=background-color
```

Toggle mobile emulation:

```bash
obsidian dev:mobile on
```

Run `obsidian help` to see additional developer commands including CDP and debugger controls.

## Full Command Reference

For the complete list of all available commands (Bases, Bookmarks, Command Palette, File History, Links, Plugins, Properties, Publish, Sync, Tags, Tasks, Templates, Themes, Workspace, Developer Commands, TUI keyboard shortcuts), see `references/commands.md`.
