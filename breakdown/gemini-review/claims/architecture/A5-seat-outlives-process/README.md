# A5: A seat keeps its identity, context and queue ownership when its process dies or resets.

**Verdict:** part

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

### Linked result: A5 / F4 / S2

Durable seat identity and queue records are separate from the running process. Losing a terminal does not itself delete the seat or its work records. This supports the logical-address distinction (F4), but does not guarantee that the agent is alive, that all context survives, or that an old occupant's active claim stays unchanged (A5/S2).

Reviewed source revision: `9db3ed6c406be5c3d9a84720383fcf6b543169e6`. Evidence below is source observation unless labelled otherwise; no live crash/restart was demonstrated in this review.

- `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/src/db/migrations/001_core_schema.ts:14–27` defines persistent nodes with a logical identity within a rig. `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/src/db/migrations/024_queue_items.ts:22–51` stores queue destinations, state and claim metadata independently; the destination is text, not a foreign key to a process.
- `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/src/domain/reconciler.ts:44–96` probes terminal presence and marks missing sessions detached. The status change and event commit together, then subscribers are notified. Probe errors are returned, not converted into proof of death. This path does not delete nodes or queue rows. `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/src/domain/session-registry.ts:437–446` distinguishes changing session status from clearing a binding.
- `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/src/domain/queue-repository.ts:2038–2117` checks destination/state and stamps the claimant generation. `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/src/domain/queue-repository.ts:3424–3458` returns a retiring generation's in-progress items to pending, without changing their destination. `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/src/domain/occupant-invalidator.ts:55–85` invokes this release, but skips generation-scoped invalidation when no generation is supplied. Durable role ownership is not identical to an unchanged execution claim.
- `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/src/domain/snapshot-capture.ts:60–114` reads resume metadata, checkpoints and startup context. `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/src/domain/restore-orchestrator.ts:980–1005` can stop at `awaiting-decision` when a previous session has no resume token. Saved metadata is not a guarantee of recoverable conversation history.
- Test assertions, not a runtime demonstration: `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/test/reconciler.test.ts:95–120` checks detachment; `:239–259` checks transaction rollback. Its terminal adapter is mocked at `:23–44`. These tests were inspected, not executed.

## Notes

- **Limits/contradictions:** the report joins identity, accumulated context and queue ownership into one unconditional guarantee. The source supports different persistence boundaries for each. Explicit rig deletion is different from process death: `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/src/domain/rig-teardown.ts:147–192` has a separate deletion path. Generation retirement can release a claim rather than preserve it in-progress.
- **Inference:** records surviving a missing terminal make the seat address durable; they do not make the destination reachable or productive. Terminal presence alone does not prove an AI process is still running inside it.
- **Evidence still needed / runtime-unverified:** a controlled crash and restart with before/after seat, generation, queue and conversation records; native-harness resume evidence; and a missing/stale-token case. No dependency installation, daemon startup or lifecycle experiment was performed. The worktree has no installed dependencies, so cited tests have no new passing-run claim.
- Read this result with F4/S2 and the A8/F9/S5 restore boundary. No implementation action or strength/weakness classification is proposed.

