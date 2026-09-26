# S5: Snapshots allow a whole fleet to be brought back after a reboot (same mechanism as A8).

**Verdict:** part

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> State persistence is handled through declarative snapshots that capture running topologies and attempt to restore them across machine reboots7.

## What the infographic says

Not mentioned.

## Sources Gemini cited

- [7] [GitHub - mvschwarz/openrig: Multi-agent harness that runs Claude](https://github.com/mvschwarz/openrig)

## How to check

Decide together with A8 and F9.

## Evidence from this repo

### Linked result: A8 / F9 / S5

Snapshots implement an attempt to restore intended topology and selected continuity state (A8: yes), not guaranteed recovery of every agent and all work (S5: part). Teardown continues after capture failure, but that alone does not prove saved uncommitted-file loss (F9: part). These three claims describe one conditional recovery mechanism.

Reviewed source revision: `9db3ed6c406be5c3d9a84720383fcf6b543169e6`.

- **Source observation:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/snapshot-capture.ts:60–114` selects an intended roster and active occupants and reads resume/checkpoint/startup state. `:125–166` persists that payload and its event transactionally. It is more than topology alone, but not a file, volume or full conversation archive; queue state is not embedded in the snapshot payload.
- **Source observation:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/routes/rigs.ts:643–694` filters stale-occupant snapshots and either rehydrates eligible current DB state or returns `no_snapshot`. Current durable state can aid recovery even without a usable old snapshot; it cannot conjure missing native history.
- **Source observation:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/restore-orchestrator.ts:75–85` reports full, partial and failed outcomes. `:980–1005` can refuse a prior session without a token; `:1175–1250` distinguishes resumed history, attention-required, rollback, missing adapter and checkpoint rebuilding. `:1263–1279` distinguishes contained exact resume from startup replay and checks referenced files. “Fleet restored” is not synonymous with “all histories resumed exactly.”
- **Source observation:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/rig-teardown.ts:103–143` continues after snapshot failure. The separate volume-deleting policy is implemented at `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/adapters/compose-services-adapter.ts:81–98`; metadata receipts do not contain those volume bytes.
- **Test assertions only:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/test/restore-orchestrator.test.ts:186–228` checks intended-roster restore and rejection of unusable snapshots; `:413–439` checks a post-crash path; `:568–602` checks partial launch failure reporting. Terminal and resume helpers are mocked at `:42–69`. These were inspected, not executed or treated as a real reboot demonstration.

## Notes

- **Limits/contradictions:** the original quotation says “attempt,” which A8 reflects; S5's whole-fleet formulation overstates the guarantee. A full rollup accepts checkpoint rebuilding as well as resume. A live process awaiting operator attention is not fully restored. A preserved seat address (A5/F4/S2) is not proof of successful recovery.
- **Inference:** durable topology plus valid native resume sources can restore a fleet, but this depends on usable database state, identities, files, adapters and harness state. Partial failure and deliberate fresh starts are explicit outcomes, not evidence of preserved conversation.
- **Evidence still needed / runtime-unverified:** an actual multi-seat reboot recovery with per-seat lineage, conversation continuity, workspace/service data and final usability receipts. No runtime success rate, automatic reboot restoration guarantee or complete-work-preservation result is established. Dependencies are absent in this worktree; no test suite or live lifecycle operation was run.

