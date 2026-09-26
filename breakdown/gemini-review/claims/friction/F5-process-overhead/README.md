# F5: Claim transactions, handoff protocols, verification contracts and CULTURE.md add heavy process overhead.

**Verdict:** part

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> Furthermore, OpenRig deliberately introduces procedural bureaucracy as a safety mechanism5.

> Drawing from real-world observations where uncontrolled swarms hallucinate or deviate from system prompts, OpenRig mandates explicit task claim transactions, handoff protocols, verification contracts, and cultural policy documents5.

> In large long-running setups this structure prevents systemic degradation, but for an individual developer seeking to automate code generation, managing formal ticket queues and multi-stage handoffs introduces substantial operational resistance5.

> Governance and Control: Behavioral markdown norms (CULTURE.md) and watchdog schedules7

## What the infographic says

Verbatim from `sources/agent_fleet_architectures_infographic.html`.

> Deliberate Bureaucratic Overhead

> OpenRig was built to stop rogue AI swarms from hallucinating. To solve this, it mandates strict task claim transactions, verification contracts, watchdog checks, and markdown CULTURE.md laws.

> The Cost: While this prevents systemic drift in long-running research experiments, it imposes suffocating administrative drag on a developer who just wants to build software.

## Sources Gemini cited

- [5] [My friend gave Claude Code and Codex agents a way to talk to](https://www.reddit.com/r/AI\_Agents/comments/1wqij2i/my\_friend\_gave\_claude\_code\_and\_codex\_agents\_a\_way/)
- [7] [GitHub - mvschwarz/openrig: Multi-agent harness that runs Claude](https://github.com/mvschwarz/openrig)

## How to check

Map which of these are enforced by code and which are only conventions in markdown. Enforced ones are the real cost.

## Evidence from this repo

### Linked result: F5 / S3

Ownership and handoff rules implement durable, auditable coordination with specific runtime checks. Behavioral guidance adds further obligations, but neither universal heavy overhead (F5) nor prevention of long-running cognitive drift (S3) is demonstrated by those mechanisms. Both verdicts are partial: real coordination controls exist; their claimed cost and effectiveness remain unmeasured.

Reviewed source revision: `9db3ed6c406be5c3d9a84720383fcf6b543169e6`.

- **Source observation — claim:** `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/src/domain/queue-repository.ts:2038–2117` checks destination and claimable state, then commits claim state, generation stamp, transition and event together; subscriber notification follows commit. The checks precede the transaction, and the update is keyed by item ID. This supports a local operation, not a demonstrated multi-writer compare-and-swap guarantee.
- **Source observation — local handoff:** `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/src/domain/queue-repository.ts:1507–1560` rejects terminal sources/invalid destinations, validates human-route metadata and begins a close/create transaction. `:1618–1669` stages wake intent and events in that transaction, checks intent presence before commit, then notifies subscribers and delivers the wake. A committed handoff is not proof of receipt or action by the next owner. `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/src/domain/event-bus.ts:161–177` isolates subscriber exceptions from the committed state.
- **Source observation — recovery:** `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/src/startup.ts:351–352` attaches the outbox; `:2179–2200` reconciles abandoned sending intents and drains pending ones at startup. `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/src/domain/queue-repository.ts:996–1069` claims and records delivery outcomes; `:1113–1160` retries pending intents, not failed/indeterminate ones. Abandoned sending is made indeterminate rather than blindly sent again. This is bounded recovery, not guaranteed eventual delivery.
- **Source observation — cross-host boundary:** `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/src/routes/queue.ts:309–380` derives a stable successor ID, checks conflicting re-drives, forwards successor creation first and closes the local source second. This spans two databases without one transaction. An interrupted close can leave both records open until a re-drive; local atomicity must not be generalized to the network.
- **Source observation — rules are conditional, not all prose:** `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/src/domain/queue-repository.ts:1534–1546` checks human-route summary/evidence; `:2501–2515` checks human-park evidence. `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/src/domain/proof/judgments.ts:182–200` checks evidence hashes and exposes configured/legacy/unknown proof states. These are real checks, not proof that every agent task requires the same formal contract.
- **Stated intent/convention:** `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/assets/plugins/openrig-core/skills/queue-handoff/SKILL.md:38–85` instructs agents how to end active work, but also says not to turn tiny handoffs into bureaucracy. `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/assets/plugins/openrig-core/skills/forming-an-openrig-mental-model/SKILL.md:231–238` says to read `CULTURE.md` “if it has one.” This contradicts a universal mandatory-culture-file reading. Instructions alone do not prove compliance.
- **Test assertions only:** `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/test/queue-transactional-closure.test.ts:155–237` checks intent atomicity/rollback; `:280–347` checks delivery outcomes and repeated drain; `:443–463` checks that failed intents are not retried. Its transport is mocked at `:41–73`. `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/test/queue-cross-host-handoff.test.ts:202–253` asserts forwarding failure/re-drive behavior using the injected-fetch harness at `:52–64`. Tests were read, not run.

## Notes

- **Contradictions:** `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/src/startup.ts:2184–2185` broadly says transient failures retry next start, but the executable drain selects only pending intents (`queue-repository.ts:1143–1160`); terminal failed/indeterminate rows do not retry there. The handoff comment at `queue-repository.ts:1502–1505` says `done`, while its SQL at `:1551–1559` writes `handed-off`. Executable behavior governs this review.
- **Limits:** the no-outbox fallback in `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/src/domain/queue-repository.ts:1096–1110` is a delivery-helper path, not permission for a wake-intended terminal handoff to omit durable intent. The seam guard enforces this at `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/src/domain/queue-repository.ts:921–947`, with test assertions at `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/test/queue-transactional-closure.test.ts:115–153`. Explicit `nudge:false` is a different case.
- **Inference:** these controls create procedure and reduce some lost/ambiguous ownership states. They cannot establish that agent reasoning is correct, all work uses the queue, or the overhead is heavy for a particular developer. Markdown obligations can also cost effort; the original “only enforced ones are the real cost” check instruction is not a measured conclusion.
- **Evidence still needed / runtime-unverified:** observed end-to-end handoff/crash recovery, recipient action and ambiguity handling; workload-specific time/interaction measurements; and longitudinal drift evidence. No live daemon, network handoff or new passing test run was demonstrated. Dependencies are absent in this worktree. No implementation action or classification is proposed.

