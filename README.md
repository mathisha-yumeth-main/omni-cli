# Omni CLI

One terminal app for **Ollama, Claude, Gemini, OpenAI and xAI (Grok)**, on Windows, Linux and macOS.
Zero dependencies. Just Node.js 18+.

- Chat with any provider and switch with one command (`/provider claude:opus`)
- **Agent mode** with 35 tools: read/write/edit files, run tests, lint, build, git, web search and more
- **463 skills** that are routed automatically to each message
- Works with **every model you have installed in Ollama**

## Install

```bash
git clone https://github.com/<your-username>/omni-cli.git
cd omni-cli
```

| OS | Install | Or run without installing |
|----|---------|---------------------------|
| Linux / macOS | `sh install.sh` | `node bin/omni.js` |
| Windows | double-click `install.bat` | `node bin\omni.js` |

Single binary (optional): `bun build --compile bin/omni.js --outfile omni`

## Quick start

```text
omni
/provider claude            # switch provider
/key claude sk-ant-...      # saved to ~/.omni/config.json
/model sonnet               # aliases: sonnet, opus, haiku, fable, or any part of a model name
/provider claude:opus       # provider and model in one go
```

Environment variables: `ANTHROPIC_API_KEY`, `GEMINI_API_KEY`, `OPENAI_API_KEY`, `XAI_API_KEY`, `OLLAMA_HOST`.
Ollama needs no key. Just have it running.

## Features

### Ollama, all installed models
Each model is probed so Omni sends `think` only to models that support it, limits the context window to the model's real maximum, hides embedding-only models, and falls back to an installed model if the default isn't pulled. `/models` shows size, context and `think` / `tools` / `vision` tags.

### Agent mode
`/agent on` lets the model plan and use tools. Risky actions ask for approval (`y / n / a`); `/auto` or `-y` skips prompts. Tool calls use a text protocol, so small local models work the same as cloud ones.

Tools: `read_file`, `read_many`, `write_file`, `edit_file`, `symbols`, `tree`, `glob`, `grep`, `run_command`, `run_tests`, `lint`, `build`, `git`, `web_search`, `web_fetch`, `http_request`, `file_ops`, `project_summary`, `deps`, `loc`, `scan_todos`, `scan_secrets`, `json_query`, `diff_files`, `hash`, `encode`, `regex_test`, `ports`, `todo`, `remember`, `recall`, `calc`, `sysinfo`, `ask_user`.

### Skills
Every message is scored against all skills and the best few (default 4, `/skills max 6`) are injected automatically. Pin skills with `/skill code-review`; turn routing off with `/skills auto off`.

### Commands

| Area | Commands |
|------|----------|
| Models | `/provider` `/model` `/models` `/each <prompt>` `/bench` `/effort` `/ctx` `/host` |
| Git and review | `/review` `/commit` `/pr` `/changelog` `/diff` `/status` `/log` `/standup` |
| Code | `/refactor <file> <change>` `/explain` `/fix` `/tests` `/docs` `/optimize` `/security` `/find` `/tree` |
| Run | `/test` `/lint` `/build` `/serve [port]` `/plan` `/agent` `/auto` |
| Project scans | `/todos` `/loc` `/secrets` `/deps` `/ports` |
| Utilities | `/uuid` `/hash` `/b64` `/urlenc` `/jwt` `/ts` `/regex` `/calc` |
| Sessions | `/save` `/load` `/export` `/compact` `/undo` `/retry` `/copy` `/usage` `/info -l` `/doctor` |
| Knowledge | `/kb add <file\|folder\|text>` `/init` (writes `OMNI.md`) `/readme` |
| Custom | `/macro add <name> <prompt with {{args}}>` makes your own slash commands |

Shortcuts: `!cmd` runs a shell command, `@file` attaches a file, a trailing `\` continues a line, Tab completes, Ctrl+C interrupts.

## One-shot and scripting

```bash
omni -p "explain this error" -P openai -m gpt-5 -e high
omni -p "review my changes" -P claude:opus --plan
cat app.log | omni -p "find the root cause" -P claude
omni -p "add a --verbose flag to cli.js" -a -y -P ollama -m qwen3
```

## Troubleshooting (Windows + Ollama)

- **`ollama serve` says the port is already in use**: Ollama is already running. Check with `curl http://127.0.0.1:11434`.
- **"Cannot reach Ollama at http://0.0.0.0"**: update to v1.3.1 or newer, or set `OLLAMA_HOST=127.0.0.1:11434`.
- Run `/doctor` inside Omni to check configuration and connectivity.

## Configuration

Config and history live in `~/.omni` (`%USERPROFILE%\.omni` on Windows). Set `OMNI_HOME` to move it. API keys are stored in plain text in `config.json`, so never commit that file.

## License

MIT
