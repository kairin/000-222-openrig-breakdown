# F8: `product-team` starts four Claude Code instances at once and can hit account rate limits.

**Verdict:** part — product-team declares four Claude Code seats (and three Codex seats), but startup is sequential and actual account throttling or an instant fleet stall is unresolved.

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

Reviewed at source commit `9db3ed6c406be5c3d9a84720383fcf6b543169e6` (2026-09-27).

- **Observed configuration:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/specs/rigs/preview/product-team/rig.yaml:9–60` declares Claude at orch1.lead, dev1.impl, dev1.design and rev1.r1, and Codex at orch1.peer, dev1.qa and rev1.r2. Lines 4–7 describe an advanced, human-operated squad, not every starter.
- **Observed behavior trace:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/bootstrap-orchestrator.ts:635–637` calls pod instantiation. `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/rigspec-instantiator.ts:1247–1258,1348–1350,1365–1366,1457–1483` computes topological order, sorts members and awaits each member's launch in a loop. This is not four simultaneous launch calls or a parallel Promise batch.
- **Observed sequencing:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/startup-orchestrator.ts:226–237,311–324` launches the harness and waits for readiness. The inspected loop has no explicit account-budget limiter or fixed inter-seat rate-limit delay. Sequential initialization can still leave multiple agents alive and generating requests concurrently afterward.
- **Stated intent / counterexample to generalization:** `/home/kkk/Apps/openrig-breakdown/docs/reference/getting-started.md:97–99` calls the seven-seat showcase optional and capacity-consuming; `/home/kkk/Apps/openrig-breakdown/packages/daemon/specs/rigs/launch/first-project/rig.yaml:10–25` is instead two Codex seats.

## Notes

- **Scope:** the authored product-team seats, excluding separately booted kernel agents, user overrides, failures and later topology changes. Seat count is neither successful process count nor API-request concurrency.
- **Contradiction:** “four Claude” is supported; literal simultaneous launch is contradicted by awaited startup. The infographic's universal “starter rigs” and “triggering ... instantly” exceed the evidence.
- **Inference:** overlapping sessions can compete for shared account capacity if configured that way. Sequential startup is not proof of protection from throttling; absence of a limiter in this path is not an audit of all native-provider throttling.
- **Unresolved / evidence still needed:** actual account sharing, provider plan/limits, model, native versions, other account traffic, timestamped requests/errors and the resulting per-seat states. No source inspected establishes a specific rate-limit incident or whole-fleet stall.
- **Runtime-unverified:** no fleet launch, account inspection, load experiment, benchmark or rate-limit reproduction was performed.

