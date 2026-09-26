# F9: Teardown carries on even if the snapshot fails, which can lose uncommitted work.

**Verdict:** ? (unverified)

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> Moreover, session lifecycle management introduces data preservation risks13.

> During rig teardown, snapshot generation is non-blocking: if snapshot capture fails, the daemon reports the failure but proceeds with process termination, potentially destroying uncommitted agent work13.

## What the infographic says

Verbatim from `sources/agent_fleet_architectures_infographic.html`.

> Non-blocking snapshots (risk of loss)

## Sources Gemini cited

- [13] [Getting started - OpenRig](https://www.openrig.dev/docs/getting-started)

## How to check

Read the down/teardown command. If the snapshot step throws, does it abort, prompt, or continue killing sessions? This is the most serious claim if true.

## Evidence from this repo

_Not checked yet. Add file:line references here._

## Notes

