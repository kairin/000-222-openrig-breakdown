# F1: The CLI is built for agents and humans at once, so its output and commands favour machines.

**Verdict:** ? (unverified)

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> OpenRig is explicitly engineered around a dual-user model where the primary command-line interface is designed to be operated simultaneously by human practitioners and autonomous AI agents13.

> Because the interface anticipates autonomous agent execution, commands heavily emphasize machine-readable outputs, transactional status flags, and low-level diagnostic parameters13.

> This creates an operational paradox.

> When a human attempts to understand or troubleshoot the system, they are forced to interact with abstractions designed for automated execution rather than human intuitive workflows13.

> The developer must simultaneously conceptualize the state of the local daemon, the SQLite metadata, the state of the multiplexed terminal panes, and the conversational context of the underlying models8.

## What the infographic says

Verbatim from `sources/agent_fleet_architectures_infographic.html`.

> The "Dual-User" Abstraction Paradox

> OpenRig’s CLI is explicitly designed to be driven by both human developers AND autonomous AI agents. Because commands are built for automated agent consumption, documentation frequently tells humans: "Instruct your agent to run the rig CLI commands for you."

> A human wanting to configure a fleet must learn machine-targeted transaction syntax instead of intuitive UI buttons or natural declarative YAML.

> tmux TUI & machine JSON logs

## Sources Gemini cited

- [8] [OpenRig - Esoteric Labs — AI Studio](https://esoteric.run/work/openrig)
- [13] [Getting started - OpenRig](https://www.openrig.dev/docs/getting-started)

## How to check

Count the `rig` subcommands and flags (`docs/as-built/cli-reference.md`). Mark which ones a human would ever type versus ones only agents use.

## Evidence from this repo

_Not checked yet. Add file:line references here._

## Notes

