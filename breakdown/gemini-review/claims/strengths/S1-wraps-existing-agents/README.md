# S1: OpenRig works with the CLI agents people already use instead of replacing them.

**Verdict:** ? (unverified)

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> OpenRig is engineered not as an autonomous coding agent, but as a local meta-harness—a control plane that wraps, monitors, and connects pre-existing, third-party terminal coding agents such as Anthropic’s Claude Code and OpenAI’s Codex CLI7.

> Originating from the private Agent Focus framework and licensed under Apache 2.0, OpenRig addresses the sprawl that emerges when developers run multiple uncoordinated terminal sessions simultaneously7.

## What the infographic says

Verbatim from `sources/agent_fleet_architectures_infographic.html`.

> You have a deliberate need to coordinate pre-existing Claude Code / Codex terminal processes through tmux sessions with persistent telecom-style seats. Be prepared for high setup friction.

## Sources Gemini cited

- [7] [GitHub - mvschwarz/openrig: Multi-agent harness that runs Claude](https://github.com/mvschwarz/openrig)

## How to check

Confirm how many agent runtimes are supported and how much code each adapter takes. This is likely the core worth keeping.

## Evidence from this repo

_Not checked yet. Add file:line references here._

## Notes

