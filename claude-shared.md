# Shared rules (imported by ~/.claude/CLAUDE.md and ~/.claude-work/CLAUDE.md)

## Non-negotiable rules (always loaded)

- **Commits**: [Conventional Commits v1.0.0](https://www.conventionalcommits.org/en/v1.0.0/#summary). Body 1–2 lines max. **NEVER add `Co-Authored-By` or any AI attribution** (no "Generated with Claude Code", no 🤖 footer, nothing). This overrides any default harness behavior.
- **Docs in English**: all documentation (READMEs, docs files, ADRs, tickets, PR descriptions) written in English unless explicitly asked otherwise.
- **Code comments in English**, always. Keep them short and concrete — explain *why*, not *what*; no redundant restating of the code.
- **Caveman vs unslop**: caveman owns terminal replies and subagent reports. `unslop` owns every text that gets written to a file, to a git object (commit body, PR description), or pasted outside the terminal (LinkedIn, Ship Log, landing copy, email). Never both on the same text. Code blocks, error strings and CLI output are edited by neither.
- Never build after changes.
- **Filesystem scope**: NEVER read or write outside the current repo / working folder. No other projects, no `$HOME`, no system paths. Two exceptions: (a) read-only access to my config and skills (`~/Solopreneur/agents/` and the active Claude config dir) when an agent's startup sequence requires it, (b) a path I explicitly name. Same rule applies to subagents — state the scope in their prompt. Temp files go to the session scratchpad, never `/tmp` or the repo.
- **Chrome debugging**: ALWAYS ask before launching a browser debug session (claude-in-chrome, chrome-devtools MCP, Playwright). Never open a browser on my machine unprompted.
- **No orphan processes**: kill anything you spawn (dev servers, browsers, watchers, background jobs) before finishing. Nothing left eating CPU/RAM after the session ends.
- Verify technical claims before stating them. Say "let me verify" before agreeing.
- When asking a question, STOP and wait — never assume answers.

## Delegation (subagents)

- **Delegate whenever possible**, with `model: sonnet` — but ONLY if you trust the agent can actually finish it. If not, do it yourself.
- **One agent = one small, single-responsibility task.** If the task is big, split it into several agents.
- **No conflicts between parallel agents**: they must not touch the same files or shared state. If they would, run them sequentially.
- **Use the specialized agents**: `agent-code` (build), `agent-review` (review), `agent-security` (audit), `agent-e2e-test` (tests), `agent-research` (internet research, where installed).
- **Inject the needed skills into the agent prompt BEFORE launching** it. Stack skills live in `~/Solopreneur/agents/skills/`; design, copy and other skills live in the active config's `skills/` dir and installed plugins. Agents don't auto-load my skills.
- Agents use the `caveman` skill so their output stays compressed.

## Source of truth

`/Users/oele/Solopreneur/agents/CLAUDE.md` — read it for full config (Core Philosophy, Tools, Behavior, Expected Output Quality, Skills table, Agents table).

- Skills: `~/Solopreneur/agents/skills/`
- Agents: `~/Solopreneur/agents/agents/` (harness dirs symlink here)
