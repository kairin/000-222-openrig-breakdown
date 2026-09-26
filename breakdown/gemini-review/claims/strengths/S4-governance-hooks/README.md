# S4: Watchdog schedules and CULTURE.md give some governance over agents.

**Verdict:** part — scheduling, policy actions and guidance provide bounded governance mechanisms, not demonstrated hallucination prevention or universal compliance.

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> Governance and Control: Behavioral markdown norms (CULTURE.md) and watchdog schedules7

## What the infographic says

Verbatim from `sources/agent_fleet_architectures_infographic.html`.

> OpenRig was built to stop rogue AI swarms from hallucinating. To solve this, it mandates strict task claim transactions, verification contracts, watchdog checks, and markdown CULTURE.md laws.

## Sources Gemini cited

- [7] [GitHub - mvschwarz/openrig: Multi-agent harness that runs Claude](https://github.com/mvschwarz/openrig)

## How to check

Find the watchdog implementation. Is it a scheduler that nudges agents, or does it enforce anything?

## Evidence from this repo

Reviewed against source baseline `9db3ed6c406be5c3d9a84720383fcf6b543169e6`.

- **Source observation:** Scheduler scans active due jobs and invokes the policy engine; cadence uses scan interval or job interval: `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/watchdog-scheduler.ts:130–159`. The engine handles skip/terminal outcomes, delivers selected messages/actions and records delivery status: `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/watchdog-policy-engine.ts:333–394,495–557`. This is more than an unconditional timer, but delivery is not proof of agent obedience.
- **Stated intent:** `CULTURE.md` is a rig-wide operating manual covering communication, coordination, quality, commits and escalation: `/home/kkk/Apps/openrig-breakdown/docs/reference/agent-startup-guide.md:79–88`.
- **Source observation:** Instantiation includes the default culture file and conditionally adds the rig's culture overlay; `auto` Markdown delivery resolves to guidance merging: `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/rigspec-instantiator.ts:2371–2381`; `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/runtime-adapter.ts:95–101`. A custom file literally named `CULTURE.md` is not mandatory: `culture_file` is optional and accepts a safe relative path (`/home/kkk/Apps/openrig-breakdown/docs/reference/rig-spec.md:176–180`; `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/rigspec-schema.ts:117–125`).
- **Source observation:** There are real bounded queue checks: claims validate destination/state and write state/transition/event in a transaction (`/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/queue-repository.ts:2038–2057,2069–2117`); human-parking validation can reject missing evidence/summary requirements (same file, lines 2501–2515). These establish specific state-transition contracts, not universal verification of all agent work.

## Notes

**Inference:** Behavioral Markdown is instruction content, not a truth checker. Scheduler/policy control and database validation enforce particular machine rules; they cannot by themselves establish the infographic's claim that rogue swarms stop hallucinating. The “terminal” policy outcome marks a watchdog job terminal in the cited code; it must not be misread as terminating an agent.

**Runtime-unverified / limitations:** No watchdog fired and no guidance ingestion or compliance was observed. The claim that OpenRig “was built to stop” hallucinations is unresolved historical intent without an attributable design source. Actual prevention requires a defined hallucination metric, adversarial tasks, measured outcomes and a baseline; universal verification-contract enforcement requires an audit of all work/dispatch paths, not extrapolation from queue validation. Successful scheduling needs isolated timing/delivery/failure tests. This page makes no linked F5/S3 ownership conclusion and proposes no classification or implementation.

