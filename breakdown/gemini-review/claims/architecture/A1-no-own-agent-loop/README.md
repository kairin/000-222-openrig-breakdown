# A1: OpenRig has no agent loop of its own; it supervises Claude Code and Codex CLI sessions.

**Verdict:** part — external inference is supported; the quoted account understates OpenRig's launch responsibilities and runtime set.

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> OpenRig is engineered not as an autonomous coding agent, but as a local meta-harness—a control plane that wraps, monitors, and connects pre-existing, third-party terminal coding agents such as Anthropic’s Claude Code and OpenAI’s Codex CLI7.

> OpenRig was architected to solve a specialized engineering challenge: supervising fleets of third-party command-line coding agents operating simultaneously over extended time horizons, without providing its own internal inference loop7.

> Primary System Role: Local meta-harness control plane supervising third-party CLI agents7

> Core Execution Engine: External agent harnesses (Claude Code, Codex CLI) hosted inside tmux \[cite: 8, 13\]

## What the infographic says

Verbatim from `sources/agent_fleet_architectures_infographic.html`.

> Not a coding agent itself, but a supervisor daemon that wraps host tmux sessions running Claude Code or Codex CLI with telco-style routing.

> │ ↓ Executes third-party CLI agents

> It doesn't run agents itself; it hijacks host tmux sessions running Claude Code or Codex. It replaces standard developer terms with telecom abstractions (Rigs, Pods, Seats) and expects AI agents to execute its CLI commands.

## Sources Gemini cited

- [7] [GitHub - mvschwarz/openrig: Multi-agent harness that runs Claude](https://github.com/mvschwarz/openrig)
- [8] [OpenRig - Esoteric Labs — AI Studio](https://esoteric.run/work/openrig)
- [13] [Getting started - OpenRig](https://www.openrig.dev/docs/getting-started)

## How to check

Look for any model/API client in `packages/`. If the only way work gets done is by launching `claude` or `codex` binaries, the claim holds.

## Evidence from this repo

Reviewed against source baseline `9db3ed6c406be5c3d9a84720383fcf6b543169e6`.

- **Source observation:** Claude launch constructs a `claude` command and submits it to tmux; Codex launch constructs a `codex` command and sends it through the tmux shell-command adapter: `/home/kkk/.cline/worktrees/36ea1/openrig-breakdown/packages/daemon/src/adapters/claude-code-adapter.ts:281–296`; `/home/kkk/.cline/worktrees/36ea1/openrig-breakdown/packages/daemon/src/adapters/codex-runtime-adapter.ts:383–412`. Thus “doesn't run agents itself” is misleading if it means OpenRig never launches them.
- **Source observation:** Registration includes `claude-code`, `codex`, `pi`, `stub`, and `terminal`, not only Claude/Codex: `/home/kkk/.cline/worktrees/36ea1/openrig-breakdown/packages/daemon/src/startup.ts:844–845`. Pi's repository-owned runner spawns the external `pi` process and writes RPC commands to its stdin: `/home/kkk/.cline/worktrees/36ea1/openrig-breakdown/packages/daemon/src/adapters/pi-runner.ts:568–580`.
- **Stated intent:** The runner describes itself as a terminal-to-RPC bridge, not a model engine: `/home/kkk/.cline/worktrees/36ea1/openrig-breakdown/packages/daemon/src/adapters/pi-runner.ts:1–19`.
- **Inference:** These paths support external ownership of model inference, not an absence of all OpenRig loops. OpenRig's watchdog itself loops over jobs and invokes policy evaluation: `/home/kkk/.cline/worktrees/36ea1/openrig-breakdown/packages/daemon/src/domain/watchdog-scheduler.ts:130–159`. Supervision is distinct from an LLM/tool inference loop.

## Notes

**Runtime-unverified / limitations:** No harness was launched. A bounded search of TypeScript and package manifests under `packages` for `Anthropic`, `@anthropic`, `@ai-sdk`, `chat.completions`, `responses.create`, `messages.create`, and `generativelanguage` found installer/test references but no model-client implementation. This is supporting negative evidence, not proof against dynamically loaded or differently named integrations. A universal “no internal inference anywhere” assertion remains unresolved without a complete dependency/plugin and execution-path audit. “Hijacks” and “telco-style” are descriptions, not demonstrated execution semantics; topology terminology is examined in A4, not classified here.

