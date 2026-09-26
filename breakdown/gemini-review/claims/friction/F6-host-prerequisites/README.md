# F6: It needs Node 20+, tmux and Claude Code or Codex CLIs that are already logged in.

**Verdict:** ? (unverified)

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> The system depends on pre-existing, fully authenticated command-line harnesses13.

> OpenRig does not manage model credentials directly; instead, it executes host binaries10.

> System Prerequisites: Node.js 20+, tmux, active logins for Claude Code and/or Codex CLI13

## What the infographic says

Verbatim from `sources/agent_fleet_architectures_infographic.html`.

> tmux + Claude/Codex logins

> Requires pre-authenticated Claude Code / Codex CLIs already logged into your machine.

> Requires host Claude / Codex CLI logins

## Sources Gemini cited

- [10] [What is OpenRig](https://www.openrig.dev/what-is)
- [13] [Getting started - OpenRig](https://www.openrig.dev/docs/getting-started)

## How to check

Check `engines` in `package.json` and the doctor/preflight checks in the CLI.

## Evidence from this repo

_Not checked yet. Add file:line references here._

## Notes

