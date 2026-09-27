# A8: Snapshots capture a running topology and try to restore it after a reboot.

**Verdict:** yes

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> State persistence is handled through declarative snapshots that capture running topologies and attempt to restore them across machine reboots7.

## What the infographic says

Not mentioned.

## Sources Gemini cited

- [7] [GitHub - mvschwarz/openrig: Multi-agent harness that runs Claude](https://github.com/mvschwarz/openrig)

## How to check

Find the snapshot and restore code paths. What is captured (topology only, or agent conversation too)? How reliable is restore?

## Evidence from this repo

### Linked result: A8 / F9 / S5

Snapshots support an attempt to restore the intended topology and selected continuity state (A8: yes). They are not a full backup of files, conversation history, process memory or service volumes. Teardown continues after capture failure (F9: part for its combined behavior/data-loss claim), while complete fleet recovery is conditional (S5: part). These are boundaries of one mechanism, not independent guarantees.

Reviewed source revision: `9db3ed6c406be5c3d9a84720383fcf6b543169e6`.

- **Source observation:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/snapshot-capture.ts:60–114` reads topology, resume metadata, active occupants, intended roster, checkpoints and startup context. `:125–166` assembles the payload and commits snapshot plus event atomically before notification. The payload contains neither a Git worktree archive nor process memory nor a full native conversation archive; queue records are also not embedded in this payload.
- **Source observation:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/rig-teardown.ts:103–143` refreshes resume metadata and attempts capture before killing sessions; capture failure is recorded but does not abort teardown. Snapshot persistence is transactional, but snapshot plus all external termination effects are not one transaction.
- **Source observation:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/routes/rigs.ts:643–694` rejects a selected snapshot naming old occupants, checks current-state rehydrate eligibility, can return `no_snapshot`, or captures eligible current state before restore. This is an explicit power-on/recovery route, not evidence that every reboot automatically restores every rig.
- **Source observation:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/restore-orchestrator.ts:75–85` distinguishes failed, partial and full results. `:980–1005` can stop before launch without a resume token; `:1175–1250` distinguishes native resume, operator attention, rollback of a blank launch, missing adapter and checkpoint rebuilding. `:1263–1279` avoids startup replay into an exact resumed history and checks referenced projection files for fresh replay.
- **Test assertions only:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/test/snapshot-capture.test.ts:104–140` asserts metadata/roster capture; `:271–290` exercises rollback on failed event insertion. `/home/kkk/Apps/openrig-breakdown/packages/daemon/test/restore-orchestrator.test.ts:413–439` asserts post-crash recovery with mocked terminal/resume helpers at `:42–69`. Tests were inspected, not run.

## Notes

- **Limits/contradictions:** capture reads precede the transaction that writes snapshot/event; the transaction does not prove an instantaneous, quiesced capture of all external state. A resume token is a reference, not an embedded transcript or proof that the harness can use it. A `fully_restored` rollup can include checkpoint rebuilding; it is not proof of exact conversation continuity.
- **Inference:** this implements the report's cautious “attempt to restore,” not guaranteed recovery. Loss of unflushed/in-memory work is possible when a process is killed, but a failed metadata snapshot does not itself establish deletion of saved uncommitted files. F9 records the separate managed-guidance and service-volume boundaries.
- **Evidence still needed / runtime-unverified:** a real reboot and native Claude/Codex restore, with the intended roster, per-seat outcomes, missing-token cases, persisted files and conversation continuity checked. No such experiment or new test run occurred; this worktree has no installed dependencies. Reliability rates and whole-fleet success remain unresolved.

