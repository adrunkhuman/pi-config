# pi-config

## Workflow

One main agent owns decisions and delegates bounded tasks to a generic `worker`, rather than routing work through fixed specialist roles. Workers handle related investigation, implementation, and validation together; persistent sessions are used only when retained context helps. Delegation is selective, not a required stage.

[Global instructions](agent/AGENTS.md) favor small changes, evidence-backed conclusions, proportionate validation, and concise plain-language reporting. Reviews are reserved for explicit requests or changes whose risk justifies them. Past sessions provide searchable context; long-running commands and parallel workflows use Herdr panes.

## Codemode and MCP

Pi 1.0's built-in codemode is enabled alongside direct tools (`mode: on`), including in workers. Use scripts to batch, chain, filter, and aggregate tool results. Prefer structured data, retain small reusable state with `store`/`load`, and keep large artifacts in files. Long-running external commands use Herdr; open-ended investigation uses subagents. Herdr, delegation, and question tools remain direct-only.

[Native MCP configuration](agent/mcp.json) enables two hosted servers; neither needs a local process or extra package. No manual API keys, shell exports, or separate Pi keyring entries are needed.

| Server | Use | Authentication |
| --- | --- | --- |
| Context7 | Third-party, version-specific documentation after local docs | Native Pi OAuth through `/mcp` |
| GitHub | Repositories, issues, pull requests, and Actions reads and authorized writes | Existing `gh` authentication for github.com |

Restart Pi or run `/reload`, then open `/mcp`, select Context7, and sign in. Alternatively, run this in your terminal:

```bash
pi mcp login context7
```

Pi stores and refreshes Context7's OAuth credentials in `~/.pi/agent/mcp-auth.json`, which is excluded from Git. They are not stored in Linux Secret Service. Context7 will show sign-in required until the browser flow is completed.

GitHub MCP obtains its Authorization token internally from `gh auth token --hostname github.com`. It uses the same account and permissions as that credential, rather than a separately scoped PAT. No GitHub `/mcp` sign-in is needed. If github.com is not authenticated, run `gh auth login --hostname github.com`, then restart Pi or reconnect the server. Never print tokens into chat or logs.

GitHub MCP is preferred for supported reads and authorized writes; `gh` remains the fallback for missing capabilities or MCP failures, and Git handles local worktrees. Making write tools available does not authorize unrequested mutations. After Context7 sign-in, `pi mcp list` checks both connections. Scripts discover tools and inspect schemas rather than assume tool names or arguments.

GitHub hosted-server details: [official documentation](https://github.com/github/github-mcp-server/blob/main/docs/remote-server.md). Context7 OAuth setup: [official Pi guide](https://context7.com/docs/clients/pi#oauth).

Bitwarden, browser automation, and replacement filesystem/shell MCPs are not installed.

### Local exposure patches

Four tool registrations have `exposure: "model-only"`: `herdr`, `subagent`, `question`, and `questionnaire`. Package updates can overwrite these small patches; after an update, inspect the registrations and reapply the relevant [patch](patches) if still needed:

```bash
patch --forward -p1 -d ~/.pi/agent/npm/node_modules/@weshipwork/pi-herdr < patches/pi-herdr-model-only.patch
patch --forward -p1 -d ~/.pi/agent/npm/node_modules/pi-question-tool < patches/pi-question-tool-model-only.patch
patch --forward -p1 -d ~/.pi/agent/git/github.com/mjakl/pi-subagent < patches/pi-subagent-model-only.patch
```

Run these from this repository, then restart Pi. Do not apply an already-present or conflicting patch blindly. Other existing local extension patches are unchanged.

## Models

| Use | Model | Thinking |
| --- | --- | --- |
| Default interactive session; worker default | OpenAI (ChatGPT) `gpt-6.1-sol` | High |
| Routine search and mechanical work | OpenAI (ChatGPT) `gpt-6-luna` | High for workers |
| Difficult reasoning and high-stakes review | OpenAI (ChatGPT) `gpt-6-astra` | High for workers |
| Additional enabled model | OpenRouter `z-ai/glm-5.3-flash` | Session-dependent |
| Automatic session naming | OpenAI (ChatGPT) `gpt-5.6-luna` | Extension-controlled |

## Extensions

Package versions are pinned in [settings.json](agent/settings.json); `mjakl/pi-subagent` is pinned to a commit. Codex compaction loads from the frozen local snapshot described below, not its development checkout.

| Area | Packages | Role |
| --- | --- | --- |
| Delegation | `mjakl/pi-subagent` | Generic workers with optional persistent sessions |
| Terminal orchestration | `@weshipwork/pi-herdr` | Workspaces, panes, commands, and output monitoring |
| Session continuity | `@k3_2o/pi-chrollo`, `@ogulcancelik/pi-codex-compaction`, `pi-session-auto-rename` | Past-session search, compaction, and session names |
| Code navigation | `@ff-labs/pi-fff`, `@narumitw/pi-lsp` | File/content search, diagnostics, and source fixes |
| Web research | `@narumitw/pi-web-search`, `@pi-lab/webfetch` | Search and page retrieval |
| Interaction | `pi-question-tool`, `@fradser/pi-btw` | Structured questions and side conversations |
| Terminal UI | `@vanillagreen/pi-tool-renderer`, `pi-zentui` | Tool output, editor, footer, and message presentation |

The local [Herdr integration](agent/extensions/herdr-agent-state.ts) reports agent state to the terminal. FFF runs in override mode with its home-directory scan warning disabled.

### Native compaction

The local compaction package is pinned to **0.3.0**, fingerprint `462b510325ff`, at `~/.local/share/pi-pinned/pi-codex-compaction/0.3.0-462b510325ff`. It is a copied, read-only snapshot, not a symlink. Pi package updates leave it unchanged; Pi host updates can still introduce incompatibilities.

| Operation | Provider / API |
| --- | --- |
| Normal responses and checkpoint replay | `openai` / `openai-responses` |
| Immediate native compaction | `openai-codex` / `openai-codex-responses`, with the same model ID |

Both routes need their own subscription OAuth credentials. Missing authentication or failed compaction fails explicitly; there is no paid API-key or plaintext-summary fallback. `/compact` starts immediately when Pi's local history eligibility permits. `/native-compact` is a redundant alias, not a way around those checks.

The snapshot's validation reports record 73 passing unit tests, offline and authenticated Pi 1.0 checks with codemode `on` and `only`, repeated compaction, saved-session resume, and migration of 0.2.0 inline checkpoints. These are prior installation checks, not tests rerun by this config export. Clean typechecking remains blocked by pre-existing dependency-version conflicts.

This repository exports configuration only; it does not include the frozen package or its development source. To restore this configuration elsewhere, copy the snapshot to the configured path (or adjust that path), verify its runtime hashes against `SNAPSHOT.json`, and authenticate both routes. The snapshot's `PINNED.md` documents deliberate updates and rollback. Install future changes as a new snapshot, then update the package path and reload while idle. Older snapshots cannot replay new hybrid checkpoints; keep a compatible 0.3.0 implementation for those sessions, or branch before the checkpoint/start fresh before rolling back.

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

The `omarchy-system` theme is paired with Zentui's minimalist editor and footer, labeled user messages, tree-style thinking steps, and animated working line. Thinking remains visible; terminal image rendering is disabled. The separate `pi-statusline` extension has been removed.

| Shortcut | Action |
| --- | --- |
| `Ctrl+L` | Resume a session |
| `Alt+M` | Select a model |
| `Ctrl+K` | Cycle thinking level |

Project trust defaults to asking. Steering mode is `all`, and Mermaid rendering is configured for streaming.
