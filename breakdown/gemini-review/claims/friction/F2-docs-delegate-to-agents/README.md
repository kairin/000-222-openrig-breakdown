# F2: The docs tell humans to hand setup and operation over to their agents.

**Verdict:** ? (unverified)

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> The documentation frequently instructs human developers to delegate configuration and execution directly to their agents16.

## What the infographic says

Verbatim from `sources/agent_fleet_architectures_infographic.html`.

> OpenRig’s CLI is explicitly designed to be driven by both human developers AND autonomous AI agents. Because commands are built for automated agent consumption, documentation frequently tells humans: "Instruct your agent to run the rig CLI commands for you."

> It doesn't run agents itself; it hijacks host tmux sessions running Claude Code or Codex. It replaces standard developer terms with telecom abstractions (Rigs, Pods, Seats) and expects AI agents to execute its CLI commands.

## Sources Gemini cited

- [16] [OpenRig is the Terraform for Coding Agents (Launch Demo)](https://www.openrig.dev/blog/orchestrator)

## How to check

Read `docs/reference/getting-started.md` and the README. Can a human finish setup without asking an agent to do it?

## Evidence from this repo

_Not checked yet. Add file:line references here._

## Notes

