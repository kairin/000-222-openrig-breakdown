# F4: Operators must tell a logical seat apart from the live process behind it.

**Verdict:** yes

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> If an underlying process crashes, the seat continues to exist within the topology, forcing the operator to distinguish between a logical address and an active execution thread7.

## What the infographic says

Verbatim from `sources/agent_fleet_architectures_infographic.html`.

> If an agent process crashes, its "Seat" still exists in the SQLite graph, forcing the human to distinguish between a "Logical Seat" and an "OS Process".

## Sources Gemini cited

- [7] [GitHub - mvschwarz/openrig: Multi-agent harness that runs Claude](https://github.com/mvschwarz/openrig)

## How to check

See `docs/reference/agent-state-taxonomy.md` and the status output. How many states can a seat be in, and can a human tell at a glance whether it is actually running?

## Evidence from this repo

### Linked result: A5 / F4 / S2

Durable seat identity and queue records are separate from the running process. Losing a terminal does not itself delete the seat or its work records. F4 is correct about this distinction; A5/S2 are only partial because retained identity is not complete context preservation, unchanged execution ownership, or liveness. This does not establish the amount of operator confusion.

Reviewed source revision: `9db3ed6c406be5c3d9a84720383fcf6b543169e6`.

- **Source observation:** `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/src/db/migrations/001_core_schema.ts:14–27` persists logical nodes. `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/src/domain/reconciler.ts:44–96` marks absent terminal sessions detached without deleting the node or its queue. The status/event transaction precedes subscriber notification; probe errors remain errors.
- **Source observation:** `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/src/domain/activity-taxonomy.ts:22–73` defines three activity values, four session-presence values and three resumability values. Needs-input is a count/reason with a derived display value, not another persisted activity state. These are independent axes, not one list of mutually exclusive seat states.
- **Source observation:** `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/cli/src/commands/ps.ts:1236–1251` renders served activity/needs-input and has a legacy fallback, including `unknown`. The code provides status information rather than leaving the logical/process distinction entirely implicit.
- **Source observation:** `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/src/domain/queue-repository.ts:3424–3458` releases retired-generation claims back to pending while preserving the work destination. `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/src/domain/restore-orchestrator.ts:980–1005` can leave a seat awaiting a restore decision. Neither a named seat nor retained work means an active agent.
- **Stated intent:** `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/docs/reference/agent-state-taxonomy.md:1–20` says surfaces share one state oracle and keep reachability separate from resumability. This is documentation, not a usability measurement.
- **Test assertions only:** `/home/kkk/.cline/worktrees/a1fa6/openrig-breakdown/packages/daemon/test/reconciler.test.ts:23–44` uses a mocked terminal; `:95–120` checks missing-session detachment. Tests were inspected, not run.

## Notes

- **Limits/contradictions:** the logical/process distinction is supported. The report does not measure confusion; the implemented state display is a counterweight to inferring a large operator burden from the distinction alone. A tmux-presence probe is not proof of the foreground AI process's health; activity may be unknown or stale. A full count of all lifecycle/attention diagnoses cannot be inferred by adding the three axes.
- **Inference:** a user must not read “seat exists” as “agent is executing.” Whether the shipped display makes that clear at a glance is unresolved without observation of users and the running display.
- **Evidence still needed / runtime-unverified:** compare live CLI/TUI output before and after an agent-only exit, terminal loss and reboot, then measure operator interpretation. No live experiment was run, and dependencies are absent in this worktree. Source and test assertions do not demonstrate runtime usability.
- This is the same mechanism evaluated in A5/S2; the verdict does not endorse claims of guaranteed context recovery or measured administrative burden.

