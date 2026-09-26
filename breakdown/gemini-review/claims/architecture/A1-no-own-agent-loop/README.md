# A1: OpenRig has no agent loop of its own; it supervises Claude Code and Codex CLI sessions.

**Verdict:** ? (unverified)

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

_Not checked yet. Add file:line references here._

## Notes

