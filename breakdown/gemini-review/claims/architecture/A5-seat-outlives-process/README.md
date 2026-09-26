# A5: A seat keeps its identity, context and queue ownership when its process dies or resets.

**Verdict:** ? (unverified)

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> A seat retains its durable identity, accumulated context, and queue ownership even if the underlying AI process terminates or resets7.

## What the infographic says

Verbatim from `sources/agent_fleet_architectures_infographic.html`.

> SEAT / Addressable durable role

## Sources Gemini cited

- [7] [GitHub - mvschwarz/openrig: Multi-agent harness that runs Claude](https://github.com/mvschwarz/openrig)

## How to check

Find what state belongs to a seat versus a session in the SQLite schema, and what happens to a seat's queue when its tmux session disappears.

## Evidence from this repo

_Not checked yet. Add file:line references here._

## Notes

