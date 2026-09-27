# F1: The CLI is built for agents and humans at once, so its output and commands favour machines.

**Verdict:** part — explicit agent-oriented output exists alongside human-readable commands and an interactive TUI. Machine preference and compulsory human difficulty are not established.

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

Reviewed at source commit `9db3ed6c406be5c3d9a84720383fcf6b543169e6` (2026-09-27).

- **Observed in source:** `/home/kkk/Apps/openrig-breakdown/packages/cli/src/commands/up.ts:68–88` accepts a local YAML spec, offers plan mode, and explicitly labels `--json` “JSON output for agents.” Lines 103–129 branch between JSON and readable errors, although the remote success branch also prints JSON without the flag. This is a mixed interface, not a uniformly human-text default.
- **Observed in source:** `/home/kkk/Apps/openrig-breakdown/packages/cli/src/front-door.ts:276–283` opens mission control for bare `rig` when both streams are TTYs; scripts fall through to the ordinary CLI.
- **Observed in source:** `/home/kkk/Apps/openrig-breakdown/packages/cli/src/commands/status.ts:74–125` renders readable daemon, kernel, workspace, rig and next-step information, rather than requiring direct SQLite inspection.
- **Stated intent:** `/home/kkk/Apps/openrig-breakdown/docs/reference/getting-started.md:22–51` documents keyboard-driven startup, help, local source reading and native-terminal recovery. Lines 66–80 give declarative-spec preview/launch commands.

## Notes

- **Scope:** representative startup/status paths, not a numerical census of every flag. Command count alone cannot establish which commands humans “would ever type”; the older as-built reference is not a current exhaustive inventory.
- **Contradiction:** Gemini's claim that users must use transaction syntax instead of YAML/UI conflicts with the current YAML input and TUI paths. Supporting agents does not imply excluding humans.
- **Inference / evidence still needed:** relative machine preference and troubleshooting burden require a defined command inventory and observed human tasks; no usability measurements or comparative study were obtained.
- **Runtime-unverified:** no CLI/TUI session, setup, tests or agent runtime was executed. Static implementation evidence is not proof of usability or successful operation on this host.

