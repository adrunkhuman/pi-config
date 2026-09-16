# pi-config

## Workflow

One main agent owns decisions and delegates bounded tasks to a generic `worker`, rather than routing work through fixed specialist roles. Workers handle related investigation, implementation, and validation together; persistent sessions are used only when retained context helps. Delegation is selective, not a required stage.

[Global instructions](agent/AGENTS.md) favor small changes, evidence-backed conclusions, proportionate validation, and concise plain-language reporting. Reviews are reserved for explicit requests or changes whose risk justifies them. Past sessions provide searchable context; long-running commands and parallel workflows use Herdr panes.

## Models

| Use | Model | Thinking |
| --- | --- | --- |
| Default interactive session | OpenAI Codex `gpt-6-astra` | Low |
| Routine search and mechanical work | OpenAI Codex `gpt-5.6-luna` | High for workers |
| Localized coding and debugging; worker default | OpenAI Codex `gpt-5.6-terra` | High for workers |
| Difficult reasoning and high-stakes review | OpenAI Codex `gpt-5.6-sol` | High for workers; medium interactive override |
| Additional enabled model | OpenRouter `z-ai/glm-5.3-flash` | Session-dependent |
| Automatic session naming | OpenAI Codex `gpt-5.6-luna` | Extension-controlled |

## Extensions

Package versions are pinned in [settings.json](agent/settings.json); the custom subagent fork is pinned to a commit.

| Area | Packages | Role |
| --- | --- | --- |
| Delegation | `adrunkhuman/pi-subagent` | Generic workers with optional persistent sessions |
| Terminal orchestration | `@weshipwork/pi-herdr` | Workspaces, panes, commands, and output monitoring |
| Session continuity | `@k3_2o/pi-chrollo`, `@ogulcancelik/pi-codex-compaction`, `pi-session-auto-rename` | Past-session search, compaction, and session names |
| Code navigation | `@ff-labs/pi-fff`, `@narumitw/pi-lsp` | File/content search, diagnostics, and source fixes |
| Web research | `@narumitw/pi-web-search`, `@pi-lab/webfetch` | Search and page retrieval |
| Interaction | `pi-question-tool`, `@fradser/pi-btw` | Structured questions and side conversations |
| Terminal UI | `@vanillagreen/pi-tool-renderer`, `@shvax/pi-statusline`, `pi-zentui` | Tool output, status information, and editor/message presentation |

The local [Herdr integration](agent/extensions/herdr-agent-state.ts) reports agent state to the terminal. FFF runs in override mode with its home-directory scan warning disabled.

### Language servers

[Explicit LSP routes](agent/pi-lsp.json) cover:

| Language | Servers |
| --- | --- |
| Python | Ruff and ty |
| Go | gopls |
| C and C++ | clangd |
| JavaScript and TypeScript, including JSX/TSX | Biome |

## Skills

| Skill | Purpose |
| --- | --- |
| `diagnose-crash` | Diagnose local crashes from systemd core dumps and report confirmed Omarchy bugs |
| `omarchy` | Desktop, Hyprland, terminal, and theme customization |
| `python-project-bootstrap` | New Python projects using uv, Ruff, ty, pytest, prek, and CI templates |
| `pr-writing` | Concise PR descriptions centered on the aggregate change and supporting evidence |
| `issue-writing` | Resumable problem briefs with decisions, evidence, and next actions |
| `review` | Material defects and useful improvements across code, tests, and documentation |

The Omarchy skills are copied from the system-provided versions. Python and CI templates live in [agent/templates](agent/templates).

## Interface

The `omarchy-system` theme is paired with Zentui's minimalist editor, labeled user messages, tree-style thinking steps, and animated working line. Thinking remains visible; terminal image rendering is disabled. The statusline emphasizes project, model, effort, context, and session information, with provider usage display disabled.

| Shortcut | Action |
| --- | --- |
| `Ctrl+L` | Resume a session |
| `Alt+M` | Select a model |
| `Ctrl+K` | Cycle thinking level |

Project trust defaults to asking. Steering mode is `all`, and Mermaid rendering is configured for streaming.
