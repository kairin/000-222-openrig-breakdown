# F11: Agents share one filesystem; there is no per-agent git worktree isolation.

**Verdict:** ? (unverified)

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> Workspace Isolation Mechanism: Multiplexed terminal panes sharing local filesystems or repository paths7

## What the infographic says

Verbatim from `sources/agent_fleet_architectures_infographic.html`.

> Multiplexed Panes (Shared)

> tmux windows on host OS (shared disk)

> Multiplexed Terminal Panes

## Sources Gemini cited

- [7] [GitHub - mvschwarz/openrig: Multi-agent harness that runs Claude](https://github.com/mvschwarz/openrig)

## How to check

Read `docs/reference/worktree-builds.md`, which suggests worktree support exists. Is it automatic per seat, or opt-in?

## Evidence from this repo

_Not checked yet. Add file:line references here._

## Notes

