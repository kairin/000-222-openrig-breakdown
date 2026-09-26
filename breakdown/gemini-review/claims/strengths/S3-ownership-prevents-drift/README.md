# S3: Explicit ownership and handoffs stop long-running swarms from drifting.

**Verdict:** part

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> Drawing from real-world observations where uncontrolled swarms hallucinate or deviate from system prompts, OpenRig mandates explicit task claim transactions, handoff protocols, verification contracts, and cultural policy documents5.

> In large long-running setups this structure prevents systemic degradation, but for an individual developer seeking to automate code generation, managing formal ticket queues and multi-stage handoffs introduces substantial operational resistance5.

> To prevent drift, it introduces rigorous distributed-systems protocols, terminal multiplexing, and administrative bureaucracy5.

## What the infographic says

Verbatim from `sources/agent_fleet_architectures_infographic.html`.

> OpenRig was built to stop rogue AI swarms from hallucinating. To solve this, it mandates strict task claim transactions, verification contracts, watchdog checks, and markdown CULTURE.md laws.

## Sources Gemini cited

- [5] [My friend gave Claude Code and Codex agents a way to talk to](https://www.reddit.com/r/AI\_Agents/comments/1wqij2i/my\_friend\_gave\_claude\_code\_and\_codex\_agents\_a\_way/)

## How to check

Decide together with F5: which parts of the process actually prevent drift, and which could go?

## Evidence from this repo

### Linked result: F5 / S3

Ownership and handoff rules implement durable, auditable coordination with specific runtime checks. This is a narrower result than “stop long-running swarms from drifting.” F5/S3 are both partial: the controls exist, but universal heavy overhead and demonstrated prevention of cognitive/systemic drift do not follow from queue consistency. The claimed drift-prevention outcome remains unresolved, not disproved.

Reviewed source revision: `9db3ed6c406be5c3d9a84720383fcf6b543169e6`.

- **Source observation:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/queue-repository.ts:2038–2117` checks destination/state and commits claim, claimant generation, transition and event before notification. Precondition reads occur before the transaction; this is not by itself evidence of arbitrary concurrent-writer exclusion. `:3424–3458` returns retiring-generation work to pending rather than dropping it, linking ownership recovery to A5/F4/S2.
- **Source observation:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/queue-repository.ts:1507–1560` validates local handoff inputs and starts the transaction; `:1618–1669` commits wake intent and events with the handoff, then notifies and sends. `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/event-bus.ts:161–177` catches subscriber errors. Durable transfer of responsibility is distinct from delivered notification, recipient acknowledgement and correct work.
- **Source observation:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/startup.ts:351–352` attaches the outbox; `:2179–2200` wires startup recovery. `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/queue-repository.ts:996–1069` records delivery outcomes; `:1113–1160` drains pending intents only and reconciles abandoned sending to indeterminate. Failed or ambiguous deliveries require further reconciliation rather than automatic repeated sends.
- **Source observation:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/routes/queue.ts:309–380` implements remote successor-first/local-close-second handoff with deterministic identity and conflict checks. There is no cross-host transaction; a failure between those steps leaves a recoverable but incomplete transfer. A re-drive can converge, but that is not proof of automatic completion or exactly-once recipient action.
- **Source observation versus convention:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/queue-repository.ts:1534–1546` and `:2501–2515` enforce conditional human-route/park evidence checks. `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/proof/judgments.ts:182–200` validates referenced evidence hashes and distinguishes configured from legacy proof state. In contrast, `/home/kkk/Apps/openrig-breakdown/packages/daemon/assets/plugins/openrig-core/skills/queue-handoff/SKILL.md:38–85` gives behavioral instructions, and `/home/kkk/Apps/openrig-breakdown/packages/daemon/assets/plugins/openrig-core/skills/forming-an-openrig-mental-model/SKILL.md:231–238` conditions culture reading on a file existing. Neither instructions nor valid evidence references prove absence of hallucination.
- **Test assertions only:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/test/queue-transactional-closure.test.ts:155–237` asserts transactional behavior; `:280–347` covers delivery failure and repeated drain; `:443–463` asserts failed-intent non-retry; the transport is mocked at `:41–73`. `/home/kkk/Apps/openrig-breakdown/packages/daemon/test/queue-cross-host-handoff.test.ts:202–253` asserts failure and re-drive behavior, not long-running cognitive performance. These tests were not executed.

## Notes

- **Contradictions/limits:** the startup comment at `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/startup.ts:2184–2185` overgeneralizes retry; the pending-only drain at `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/queue-repository.ts:1143–1160` is narrower. The seam-guard comment at `:907–919` describes an “unwoken” close as unwritable, but the actual guarantee is durable wake intent, not successful wake: delivery occurs after commit at `:1662–1669` and can fail. This distinction is central to both F5 and S3.
- **Inference:** atomic records, generation-scoped release and traceable handoffs reduce specific coordination failure modes. They do not force an agent to obey instructions, preserve all context, or make correct decisions. Evidence-hash checks establish reference integrity, not the semantic truth of the referenced work.
- **Evidence still needed / runtime-unverified:** real handoff and daemon-crash recovery with receipt/action evidence, plus a defined drift measure and longitudinal runs that distinguish protocol effects from model, workload and operator effects. No such runtime or causal demonstration was made. Dependencies are absent in this worktree, so no new passing-test claim is made. Heavy administrative cost in F5 is likewise unresolved without measurement.
- The original “which could go” check instruction is outside this evidence review. No removal, simplification, implementation action or strength/weakness classification is proposed.

