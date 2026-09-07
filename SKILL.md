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

## macOS 샌드박스 PATH 우회

Aside의 Bash 실행 환경에서 사용자의 터미널에 있는 obsidian 명령이 보이지 않거나, 사용자 Mac에서 실행 중인 Obsidian GUI를 CLI가 찾지 못할 때 사용한다.

핵심은 사용자의 셸 PATH에서 obsidian 명령을 억지로 찾는 것이 아니라, Obsidian 앱 번들에 포함된 공식 obsidian-cli 실행 파일을 절대 경로로 호출하는 것이다.

### 먼저 확인할 것

1. **Obsidian 버전**: 공식 CLI는 Obsidian 1.12 이상 인스톨러 버전이 필요하다. App Store 샌드박스 버전은 지원하지 않는다. 사용자의 일반 터미널에서 다음을 실행한다.

~~~bash
obsidian version
~~~

정상 예시는 다음과 같다.

~~~text
1.13.7 (installer 1.13.7)
~~~

최신 인스톨러는 [Obsidian 공식 다운로드 페이지](https://obsidian.md/download)에서 받는다.

2. **CLI 활성화와 등록**: Obsidian에서 Settings -> General -> Command line interface를 열고 CLI를 켠 뒤 Register CLI를 승인한다. 사용자가 터미널을 새로 열어 PATH를 다시 읽도록 안내한다.

3. **Obsidian GUI 실행**: CLI는 실행 중인 Obsidian 앱에 연결하는 원격 제어 방식이다. 사용자가 Finder 또는 Spotlight에서 Obsidian을 직접 실행하고 창을 열어 둔 상태인지 확인한다. 샌드박스 Bash에서 open -a Obsidian 또는 앱 직접 실행이 실패할 수 있으므로, Bash에서 앱을 실행하려고 반복하지 않는다.

### 샌드박스에서 PATH를 우회하는 기본 명령

~~~bash
CLI=/Applications/Obsidian.app/Contents/MacOS/obsidian-cli
"$CLI" version
~~~

사용자 셸 PATH에 /usr/local/bin이 없거나 /usr/local/bin/obsidian 심볼릭 링크 접근이 제한되어도 앱 번들 내부의 절대 경로로 실행할 수 있다.

### GUI 연결에 실패할 때

다음 오류가 나오면 Obsidian GUI가 현재 샌드박스에서 보이지 않는 상태다.

~~~text
The CLI is unable to find Obsidian. Please make sure Obsidian is running and try again.
~~~

다음 순서로 대응한다.

1. 사용자가 Finder 또는 Spotlight로 Obsidian을 직접 실행한다.
2. Obsidian 창을 닫지 않은 상태로 둔다.
3. Bash에서 위의 절대 경로 CLI를 다시 실행한다.
4. 그래도 실패하면 샌드박스와 GUI 세션 사이의 IPC 연결이 허용되지 않는 것이다. 해당 환경에서 직접 실행을 중단하고 사용자의 터미널에서 명령을 실행하는 방식으로 전환한다.

### vault와 폴더 확인

정확한 vault 및 폴더 이름을 먼저 확인한다.

~~~bash
"$CLI" vaults
"$CLI" vault
"$CLI" vault=Documents folders
~~~

사람이 부르는 이름과 실제 vault 또는 폴더 이름은 대소문자가 다를 수 있다. 예를 들어 Personal space와 Personal Space를 혼동하지 않도록 vaults와 folders 결과를 먼저 사용한다.

### 샌드박스에서 노트 생성

현재 CLI의 create 명령은 name, path, content 인자를 사용한다. title이나 folder 인자가 무시될 수 있으므로, 정확한 vault 상대 경로를 포함한 path를 사용하는 것이 안전하다. vault 파일을 직접 쓰거나 수정하지 말고 CLI 명령으로 처리한다.

긴 문서는 heredoc으로 CONTENT를 만든 뒤 content="$CONTENT"로 전달한다.

~~~bash
CLI=/Applications/Obsidian.app/Contents/MacOS/obsidian-cli
CONTENT=$(cat <<'NOTE'
---
title: 테스트 노트
---

# 테스트 노트

Obsidian CLI로 생성한 문서입니다.
NOTE
)

"$CLI" vault=Documents create \
  path="Personal space/테스트/테스트 노트.md" \
  content="$CONTENT"
~~~

### 생성 결과 검증

파일 경로, 파일 크기, 내용이 모두 확인되어야 완료로 본다.

~~~bash
"$CLI" vault=Documents file \
  path="Personal space/테스트/테스트 노트.md"

"$CLI" vault=Documents read \
  path="Personal space/테스트/테스트 노트.md"
~~~

### 잘못 생성된 Untitled.md 정리

title 또는 folder를 사용한 create가 해당 인자를 무시하고 vault 루트에 Untitled.md를 만들었을 수 있다. 먼저 정확한 path로 다시 생성한 뒤, file path로 임시 파일이 방금 생성한 파일인지 확인한다. 사용자 파일일 가능성이 있으면 삭제하지 않는다.

~~~bash
"$CLI" vault=Documents file path="Untitled.md"
"$CLI" vault=Documents delete path="Untitled.md"
~~~

delete는 확인된 임시 파일에만 실행한다.

### 일반 셸과 샌드박스의 판단 순서

1. 사용자의 일반 터미널에서 obsidian version을 확인한다.
2. Aside Bash에서는 앱 번들의 절대 경로인 /Applications/Obsidian.app/Contents/MacOS/obsidian-cli를 실행한다.
3. GUI 연결 오류가 나면 사용자가 Obsidian을 직접 실행한 뒤 다시 시도한다.
4. 그래도 연결되지 않으면 사용자 터미널에서 CLI 명령을 실행하는 방식으로 전환한다.

### 참고

- [Obsidian CLI 공식 도움말](https://obsidian.md/help/cli)
- [Obsidian 공식 다운로드](https://obsidian.md/download)

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

Use name= for a note name or, preferably, path= for an exact vault-relative path. The current CLI uses name, path, and content; title= and folder= may be ignored.

~~~bash
obsidian create path="folder/My Note.md" content="Initial content"
~~~

When a create call using title= or folder= produces Untitled.md, verify the file with file path= before considering any cleanup.
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
| create ignores title= or folder= | Prefer name= or an exact path= and verify the resulting file |
| Unexpected Untitled.md | Run file path= first; delete it only when it is confirmed to be the temporary file just created |
| rename parameter is name=, not title= | obsidian rename file="X" name="Y" |
| Content too long for one create call | Use create for the first chunk, then multiple append calls |
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
- `\` → literal backslash
- `"` → no escaping needed

## Long Documents (Multi-chunk Pattern)

For long content, prefer an exact path and split the document into create plus sequential appends:
~~~bash
obsidian create path="Long Doc.md" content=$'# Title\n\nFirst section...'
obsidian append path="Long Doc.md" content=$'\n## Section 2\n\nContent...'
obsidian append path="Long Doc.md" content=$'\n## Section 3\n\nContent...'
~~~
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
