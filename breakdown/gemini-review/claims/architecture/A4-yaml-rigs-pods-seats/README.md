# A4: Topology is declared in YAML as Rigs, Pods and Seats.

**Verdict:** ? (unverified)

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> OpenRig defines multi-agent systems declaratively through YAML specifications7.

> System topologies are structured into Rigs (the overarching project team), Pods (bounded context groups sharing common guidance and domain knowledge), and Seats (stable, addressable network roles, such as an engineering lead or quality assurance reviewer)7.

## What the infographic says

Verbatim from `sources/agent_fleet_architectures_infographic.html`.

> RIG / Team project scope / POD / Bounded context group / SEAT / Addressable durable role

## Sources Gemini cited

- [7] [GitHub - mvschwarz/openrig: Multi-agent harness that runs Claude](https://github.com/mvschwarz/openrig)

## How to check

Read `docs/reference/rig-spec.md` and the spec parser in the daemon. Are pods and seats both required, or can a rig be a flat list of agents?

## Evidence from this repo

_Not checked yet. Add file:line references here._

## Notes

