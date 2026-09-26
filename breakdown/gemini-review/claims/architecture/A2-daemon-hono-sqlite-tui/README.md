# A2: OpenRig runs as a local daemon: a Hono HTTP server with SQLite state, plus a TUI.

**Verdict:** ? (unverified)

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> OpenRig operates as a local background daemon powered by a Hono HTTP server, supported by an SQLite state database and a Terminal User Interface7.

> Primary User Interfaces: Command-line interface and terminal user interface (rig tui)7

> State and History Management: SQLite database tracking topology graphs, chatrooms, and task queues8

## What the infographic says

Verbatim from `sources/agent_fleet_architectures_infographic.html`.

> tmux / Hono / SQLite

> OpenRig Daemon (Hono/SQLite)

> Host CLI ➔ SQLite Daemon ➔ OS tmux pane ➔ Claude Code CLI process.

## Sources Gemini cited

- [7] [GitHub - mvschwarz/openrig: Multi-agent harness that runs Claude](https://github.com/mvschwarz/openrig)
- [8] [OpenRig - Esoteric Labs — AI Studio](https://esoteric.run/work/openrig)

## How to check

Check `packages/daemon` dependencies and entry point, and whether `packages/tui` and `packages/ui` both exist as user interfaces (Gemini does not mention the web UI).

## Evidence from this repo

_Not checked yet. Add file:line references here._

## Notes

