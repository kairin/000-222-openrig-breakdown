# F9: Teardown carries on even if the snapshot fails, which can lose uncommitted work.

**Verdict:** part

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

### Linked result: A8 / F9 / S5

Snapshots support attempted topology/continuity recovery, not a backup of all agent work. Teardown does continue after snapshot failure. The additional claim that this destroys uncommitted work needs a narrower boundary: volatile or unfinished work can be interrupted, but ordinary saved uncommitted files are not shown to be deleted by snapshot failure. Thus A8 is yes for an implemented attempt, F9 is part for its combined claim, and S5 is part for conditional fleet recovery.

Reviewed source revision: `9db3ed6c406be5c3d9a84720383fcf6b543169e6`.

- **Source observation:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/rig-teardown.ts:103–133` awaits resume-metadata refresh, performs synchronous capture, catches failure into `result.errors`, then kills each selected live session. “Non-blocking” means failure does not veto teardown; it is not a background snapshot racing the kill. Capture is automatic on this live-session path, not conditional on the legacy snapshot option.
- **Source observation:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/rig-teardown.ts:85–100` skips capture if no live sessions are selected. `:147–192` blocks requested rig deletion on kill failures, not snapshot failure; ordinary stop marks sessions exited and clears bindings. `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/routes/down.ts:34–60` returns the result, using deletion failure to choose a non-success HTTP status. A snapshot warning need not make the request fail.
- **Source observation:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/snapshot-capture.ts:125–166` stores topology, session metadata, checkpoints, continuity/startup state and an environment receipt. It does not archive the worktree, full native conversation or service-volume contents. Snapshot/event persistence is atomic, but does not make the external teardown atomic or preserve process memory.
- **Source observation — filesystem boundary:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/rig-teardown.ts:202–231` explicitly cleans managed guidance in the selected Claude file or `AGENTS.md`. This is a real workspace write path, not a Git reset/clean or general worktree removal. The preservation/deletion tests at `/home/kkk/Apps/openrig-breakdown/packages/daemon/test/rig-teardown.test.ts:259–302` cover managed blocks and guidance files, not all unfinished agent work.
- **Source observation — service boundary:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/rig-teardown.ts:137–143` also tears down services. `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/service-orchestrator.ts:140–161` applies the persisted down policy; `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/adapters/compose-services-adapter.ts:81–98` executes `docker compose down --volumes` for `down_and_volumes`. Volume deletion is a distinct configured risk, not caused by snapshot failure and not prevented by this metadata snapshot.
- **Test assertions only:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/test/rig-teardown.test.ts:17–37` mocks termination and snapshot capture. Its tests at `:87–150` assert kill, record retention/deletion and capture invocation. They do not demonstrate real-process work preservation or actual loss after snapshot failure. No tests were executed.

## Notes

- **Limits/contradictions:** the continuation-after-failure clause is directly supported. The implied equation “uncommitted = lost when process killed” is not. Saved Git-uncommitted files, unsaved buffers, native conversation storage, queue rows and Docker volumes have different lifetimes. Successful snapshot capture is not a backup guarantee for any arbitrary filesystem state.
- **Inference:** immediate termination can discard process-local state or interrupt writes. Whether it actually loses a particular agent's work depends on flush behavior, harness storage and service policy. This review does not claim a measured loss incident or universal safety for on-disk work.
- **Evidence still needed / runtime-unverified:** fault-injected capture failure with real processes and before/after hashes for saved files, managed guidance, native history and service data; explicit observation of error reporting and restore outcomes. These are missing evidence, not proposed implementation changes. No live teardown was run; dependencies are absent in this worktree.
- Read with A8/S5: partial or failed restore is distinct from loss of persisted work.

