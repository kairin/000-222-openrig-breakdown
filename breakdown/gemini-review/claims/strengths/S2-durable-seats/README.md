# S2: Seats stay addressable across crashes and restarts (same mechanism as A5).

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

Decide together with A5 and F4: the same feature is a strength and a source of confusion.

## Evidence from this repo

### Linked result: A5 / F4 / S2

Durable seat identity and queue records are separate from the running process. This supports a retained logical destination across process loss, and explains F4. It does not prove uninterrupted reachability, full accumulated-context recovery, or preservation of an old occupant's active claim. S2 and A5 therefore receive partial verdicts for the full quoted claim, not a denial that durable seats exist.

Reviewed source revision: `9db3ed6c406be5c3d9a84720383fcf6b543169e6`.

- **Source observation:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/db/migrations/001_core_schema.ts:14–27` persists logical nodes; `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/db/migrations/024_queue_items.ts:22–51` independently persists queue destinations and claim state.
- **Source observation:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/reconciler.ts:44–96` changes missing sessions to detached, retaining node and queue records. Status and event persistence share a transaction; notification follows commit. A probe error does not prove the process is gone.
- **Source observation:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/queue-repository.ts:3424–3458` releases retired-generation claims to pending. `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/occupant-invalidator.ts:55–85` calls the release only with generation information. Work remains assigned to the role; its execution claim need not survive unchanged.
- **Source observation:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/snapshot-capture.ts:60–114` captures resume metadata, checkpoints and startup context; `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/restore-orchestrator.ts:980–1005` stops and asks when a prior session has no resume token. `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/restore-orchestrator.ts:1175–1237` handles failed resume, attention-required and missing-adapter cases explicitly.
- **Test assertions only:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/test/reconciler.test.ts:95–120` asserts detachment using the mocked terminal at `:23–44`; `/home/kkk/Apps/openrig-breakdown/packages/daemon/test/restore-orchestrator.test.ts:413–439` asserts a post-crash restore path, with mocked terminal/resume helpers at `:42–69`. These were read, not executed.

## Notes

- **Limits/contradictions:** “addressable” is supported as persistent logical identity, not guaranteed successful live delivery. The quotation also promises accumulated context and queue ownership without separating role assignment from occupant claims. Explicit deletion is outside the crash guarantee; `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/rig-teardown.ts:147–192` can delete the rig.
- **Inference:** durable records make recovery possible, but recovery still depends on available state, native-harness data and valid identity. This is the A8/F9/S5 preservation boundary, not an additional guarantee from S2.
- **Evidence still needed / runtime-unverified:** end-to-end process crash, reboot, delivery and native resume with persistent database and transcript evidence. No live recovery was demonstrated and no tests were run; this worktree lacks installed dependencies. The claimed operational benefit remains unmeasured.

