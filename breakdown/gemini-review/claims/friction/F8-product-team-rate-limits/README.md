# F8: `product-team` starts four Claude Code instances at once and can hit account rate limits.

**Verdict:** ? (unverified)

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> If an operator attempts to boot multi-seat topologies like product-team, which simultaneously launches four Claude Code instances, single-account tier throttling can immediately stall the fleet13.

## What the infographic says

Verbatim from `sources/agent_fleet_architectures_infographic.html`.

> Starter rigs boot 4 Claude instances at once, triggering API tier rate-limits instantly.

## Sources Gemini cited

- [13] [Getting started - OpenRig](https://www.openrig.dev/docs/getting-started)

## How to check

Find the `product-team` spec and count its seats. Is there any staggered or limited launch?

## Evidence from this repo

_Not checked yet. Add file:line references here._

## Notes

