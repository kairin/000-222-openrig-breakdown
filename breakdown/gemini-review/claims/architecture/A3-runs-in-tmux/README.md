# A3: Agents run inside tmux (or cmux) sessions on the host, not in containers.

**Verdict:** ? (unverified)

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> The underlying agents run inside standard tmux terminal multiplexer sessions on the host operating system7.

> Unlike modern platforms that package agent execution inside self-contained runtimes or lightweight container sandboxes, OpenRig binds directly to host multiplexers like tmux and cmux7.

> This architectural choice creates significant friction during initial setup and routine operation13.

## What the infographic says

Verbatim from `sources/agent_fleet_architectures_infographic.html`.

> Host tmux Multiplexer / Session Sprawl

> Fragile OS Terminal Multiplexing (`tmux`)

> Instead of running containers (Docker) or pure Node scripts, OpenRig relies on hijacking host terminal sessions using tmux or cmux.

> tmux Meta-Harness

## Sources Gemini cited

- [7] [GitHub - mvschwarz/openrig: Multi-agent harness that runs Claude](https://github.com/mvschwarz/openrig)
- [13] [Getting started - OpenRig](https://www.openrig.dev/docs/getting-started)

## How to check

Find where the daemon spawns agent sessions. Is tmux required, optional, or one of several adapters? Note the repo also has a `docker/` folder.

## Evidence from this repo

_Not checked yet. Add file:line references here._

## Notes

