# F4: Operators must tell a logical seat apart from the live process behind it.

**Verdict:** ? (unverified)

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> If an underlying process crashes, the seat continues to exist within the topology, forcing the operator to distinguish between a logical address and an active execution thread7.

## What the infographic says

Verbatim from `sources/agent_fleet_architectures_infographic.html`.

> If an agent process crashes, its "Seat" still exists in the SQLite graph, forcing the human to distinguish between a "Logical Seat" and an "OS Process".

## Sources Gemini cited

- [7] [GitHub - mvschwarz/openrig: Multi-agent harness that runs Claude](https://github.com/mvschwarz/openrig)

## How to check

See `docs/reference/agent-state-taxonomy.md` and the status output. How many states can a seat be in, and can a human tell at a glance whether it is actually running?

## Evidence from this repo

_Not checked yet. Add file:line references here._

## Notes

