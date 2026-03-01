# obsidian-cli — Claude Code Skill

A Claude Code skill for interacting with your Obsidian vault using the `obsidian` CLI.

## What it does

Enables Claude to read, create, edit, search, append, and manage notes in your Obsidian vault — all via the `obsidian` CLI command through the Bash tool.

**Trigger phrases:**
- "Obsidian에 저장해줘"
- "볼트에서 찾아줘"
- "노트 만들어줘"
- "데일리 노트에 추가해줘"
- Any request to read, create, or manage Obsidian notes

## Requirements

- [obsidian-cli](https://github.com/the-codesmith/obsidian-cli) installed and configured
- Obsidian vault set up locally

## Installation

```bash
# Using oh-my-claudecode
/oh-my-claudecode:skill add bloooi/obsidian-cli-skill
```

Or manually copy the skill folder into your `~/.claude/skills/` directory:

```bash
git clone https://github.com/bloooi/obsidian-cli-skill.git
cp -r obsidian-cli-skill ~/.claude/skills/obsidian-cli
```

## Usage Examples

| Task | Example phrase |
|------|---------------|
| Read a note | "볼트에서 'Meeting Notes' 읽어줘" |
| Create a note | "'프로젝트 계획' 노트 만들어줘" |
| Append to daily note | "오늘 데일리 노트에 완료 항목 추가해줘" |
| Search vault | "볼트에서 'API' 관련 노트 찾아줘" |
| Move a note | "'Draft' 노트를 'Archive' 폴더로 이동해줘" |

## Skill Structure

```
obsidian-cli/
├── SKILL.md                    # Main skill instructions for Claude
└── references/
    └── commands.md             # Full obsidian CLI command reference
```

## Key Behaviors

- Always uses `obsidian` CLI via Bash — never MCP obsidian tools
- Handles the `create` → `rename` pattern (CLI quirk: create always makes "Untitled.md")
- Uses `$'...'` bash quoting for content with newlines or special characters
- Splits long documents into create + sequential append chunks

## License

MIT
