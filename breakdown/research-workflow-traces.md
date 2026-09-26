# Workflow traces: setup, work, inspection and recovery

## Scope and evidence rules

This source-research note extends the provisional operator journey in
`09-adversarial-review-and-research-charter.md`. It is not an owner decision,
implementation recommendation, runtime demonstration, test result, or Gate A–D
result. All source citations in this note are pinned to inspected source commit
`9db3ed6c406be5c3d9a84720383fcf6b543169e6`; line ranges name paths at that
commit. **Reading tests is not passing tests.** No installation, daemon, agent,
queue, client-reconnect, teardown, or restore workflow was run.

Evidence labels are used precisely: **Observed in source** means the cited
implementation contains the mechanism; **Stated intent** means documentation
or a test expresses a goal/contract; **Test assertion** means a test specifies
an expected case but was not run here; **Inferred** connects evidence without
proving the resulting runtime behavior; **Unresolved** names the missing link
and the evidence still needed.

## Trace 1 — install prerequisites and start a project

| Link | Normal path | Failure path |
|---|---|---|
| Input | **Stated intent:** inspect setup, preview `first-project`, plan, then launch from a project directory (`docs/reference/getting-started.md:54-80`). | The named cases are missing tools/auth, malformed configuration, an existing/occupied daemon endpoint, and interrupted startup. The charter names these as research cases, not observed outcomes (`09-adversarial-review-and-research-charter.md:46-54`). |
| Command/client | `rig setup --dry-run`; `rig specs preview first-project`; `rig up first-project --cwd . --plan`; `rig up first-project --cwd .` (**Stated intent:** `docs/reference/getting-started.md:54-80`). | **Observed in source:** setup probes/installs tmux and Codex and checks `codex login status` (`packages/cli/src/commands/setup.ts:357-380,541-584`). `rig up` rejects an ambiguous name that matches both a spec and an existing rig and requires an explicit path or `--existing` (`packages/cli/src/commands/up.ts:287-315`). Exact malformed-config and occupied-endpoint command/output linkage is **Unresolved**; inspect the selected config parser and launch error formatter or run an authorized fault-injection case. |
| Runtime/domain owner | **Observed in source:** CLI setup owns prerequisite probes and the managed tmux configuration write (`packages/cli/src/commands/setup.ts:357-380,586-624`); CLI daemon lifecycle starts/probes the daemon (`packages/cli/src/daemon-lifecycle.ts:215-220,651-660`); daemon startup constructs the service and opens/migrates SQLite (`packages/daemon/src/index.ts:236-298`; `packages/daemon/src/startup.ts:240-249`). | **Observed in source:** name ambiguity is rejected in CLI resolution (`packages/cli/src/commands/up.ts:287-315`); daemon status guards distinguish stopped/stale from unverified/unhealthy and print separate diagnostics (`packages/cli/src/daemon-lifecycle.ts:102-134`). The source link from an interrupted startup to final cleanup is **Unresolved**; inspect startup failure compensation and an authorized interrupted-start run. |
| Persistence/process effect | **Observed in source:** successful setup can install/check host tools and replace, append, or create the OpenRig-managed `~/.tmux.conf` block (`packages/cli/src/commands/setup.ts:586-615`); daemon startup opens the SQLite database and migrations (`packages/daemon/src/startup.ts:240-249`). | **Observed in source:** setup records per-step `fail`, `skipped`, `warn`, and a final verification step; a failed setup step does not itself establish rollback (`packages/cli/src/commands/setup.ts:357-380,541-584,613-624`). Exact partial state after install/start interruption is **Unresolved**; no source citation here proves transactional rollback of host edits or a daemon-start cleanup. |
| Operator-visible output | **Stated intent:** plan/preflight and then `rig status`/`rig ps --nodes --rig first-project` are used to inspect daemon and seat readiness (`docs/reference/getting-started.md:66-80`). **Observed in source:** setup emits step messages and `Core setup verified.` or `Some setup steps failed. Run \`rig doctor\` for detailed diagnostics.` (`packages/cli/src/commands/setup.ts:617-624`). | **Observed in source:** daemon guard prints `Error: ...`, consequence, action, and optionally a wrong-home note (`packages/cli/src/daemon-lifecycle.ts:120-134`); ambiguity has an explicit selection instruction (`packages/cli/src/commands/up.ts:287-315`). Exact malformed-config, occupied-endpoint, and interrupted-start output is **Unresolved** because the full formatter path was not traced or executed. |

**WT-2 disposition:** missing tmux, failed Codex installation, failed Codex
authentication, setup-step failure, name ambiguity, and daemon non-response now
have source owners, effects, and output paths. Malformed configuration,
occupied endpoint, and interrupted-start cleanup remain explicitly unresolved;
source inspection or an authorized fault-injection run is needed rather than a
runtime claim.

## Trace 2 — launch agents into the project

| Link | Normal path | Failure path |
|---|---|---|
| Input | **Stated intent:** launch the selected starter in the chosen working directory (`docs/reference/getting-started.md:66-80`). | The relevant cases are process exit before readiness, partial multi-seat launch, existing local changes, workspace isolation, and owned-hook cleanup (`09-adversarial-review-and-research-charter.md:46-54`). These are not asserted runtime outcomes. |
| Command/client | `rig up first-project --cwd .` with separate `--plan` preview (`docs/reference/getting-started.md:66-80`; `packages/cli/src/commands/up.ts:61-80`). | Existing-rig restore uses the per-seat result path; missing resume metadata yields a decision-required result instead of silently starting a fresh conversation (`packages/cli/src/commands/up.ts:530-570`; `packages/daemon/src/domain/restore-orchestrator.ts:976-1006`). A source link for every new-rig pre-readiness failure from CLI response through cleanup is **Unresolved**. |
| Runtime/domain owner | **Observed in source:** the runtime-adapter contract projects startup content and launches the external harness in tmux (`packages/daemon/src/domain/runtime-adapter.ts:21-51,121-150`); Claude and Codex adapters construct the provider command/session path (`packages/daemon/src/adapters/claude-code-adapter.ts:281-296`; `packages/daemon/src/adapters/codex-runtime-adapter.ts:383-412`). | **Observed in source:** restore probes tmux and blocks on live or uninspectable sessions rather than treating them as absent (`packages/daemon/src/domain/restore-orchestrator.ts:239-250,782-812`); missing resume metadata produces `awaiting-decision` without launch (`:976-1006`). Adapter process-exit-to-session-registry compensation and partial-launch cleanup are **Unresolved** unless the exact launcher/registry failure path is cited; no live process was inspected. |
| Persistence/process effect | **Observed in source:** launch projects workspace files/startup content, creates or updates topology/session state, and starts an external process in tmux; Claude reconciles only its owned hook material while retaining user hooks (`packages/daemon/src/domain/runtime-adapter.ts:21-51,121-150`; `packages/daemon/src/adapters/claude-code-adapter.ts:730-818`). | **Inferred:** stored seat/session identity and process liveness can diverge because restore independently classifies tmux observations (`08-current-state-evidence.md:288-294`). Whether a failed-before-ready process removes its state, whether already-launched seats are compensated, and whether dirty project files are changed are **Unresolved**; inspect the exact failure branches and cleanup tests as assertions, not passes. |
| Operator-visible output | **Stated intent:** `rig status` and `rig ps` expose readiness; daemon readiness is not seat readiness (`docs/reference/getting-started.md:71-80`). **Observed in source:** restore output contains per-node statuses/warnings (`packages/cli/src/commands/up.ts:530-549`). | **Observed in source:** `awaiting-decision` is an explicit restore status and the guide directs the operator to resolve named auth/trust/permission prompts (`packages/daemon/src/domain/restore-orchestrator.ts:976-1006`; `docs/reference/getting-started.md:75-95`). Exact output for provider exit-before-ready and partial cleanup is **Unresolved**. |

**WT-3 disposition:** launch ordering, provider ownership, tmux probing,
resume-decision output, and managed-hook ownership are source-linked. Exit
before readiness, partial launch compensation, dirty-workspace effects, and
complete cleanup are named unresolved paths; no source-only claim says those
paths succeed or clean up completely.

## Trace 3 — assign and follow work

| Link | Normal path | Failure path |
|---|---|---|
| Input | **Stated intent:** send a bounded request, retain it in the queue, inspect the transition and repository artifact (`docs/reference/getting-started.md:90-126`). | Duplicate/retried action, stale owner, disconnected client, partial write, and missed notification are separate cases from the charter (`09-adversarial-review-and-research-charter.md:46-54`). |
| Command/client | `rig send`; `rig queue create`, claim, update, handoff, and show/transition commands (`packages/cli/src/commands/send.ts:230-283`; `packages/cli/src/commands/queue.ts:558-644,743-823`). | **Observed in source:** sender/claim identity is derived from the session transport; missing actor identity and invalid terminal closure are rejected (`packages/cli/src/commands/send.ts:230-237`; `packages/cli/src/commands/queue.ts:558-585`; `08-current-state-evidence.md:243-246`). Client timeout/retry identity and idempotence are **Unresolved**; inspect client retry policy and an authorized disconnected-client case. |
| Runtime/domain owner | **Observed in source:** daemon queue routes call the repository; SQLite queue items and append-only transitions are the durable owners (`packages/daemon/src/domain/queue-repository.ts:1165-1198,1507-1669`; `packages/daemon/src/db/migrations/024_queue_items.ts:19-52`; `025_queue_transitions.ts:13-29`). | **Observed in source:** ordinary create commits row/event before best-effort subscriber notification and pane nudge; terminal handoff stages a successor wake intent in the close/create transaction and delivers after commit (`packages/daemon/src/domain/queue-repository.ts:807-846,910-945,1114-1162,1320-1358,1608-1669`). |
| Persistence/process effect | **Observed in source:** the queue row stores owner/state/closure/delivery metadata and transitions provide an audit log (`08-current-state-evidence.md:224-241`). A regular nudge failure does not undo the durable row; failed or indeterminate terminal wake intents remain visible and are not automatically resent (`08-current-state-evidence.md:234-241`). | A partial database write is **Unresolved** beyond the repository transaction boundaries cited above. Disconnected-client retry, duplicate request convergence, and recipient acceptance are **Unresolved**; a committed row or wake intent is not proof of delivery or action. |
| Operator-visible output | **Stated intent:** operator reads queue row, full detail, transitions, and the repository artifact (`docs/reference/getting-started.md:117-126`). **Observed in source:** send verification distinguishes text appearing in the pane from agent acceptance (`packages/cli/src/commands/send.ts:256-272`). | **Observed in source:** delivery/nudge outcomes and failed/indeterminate wake states remain reportable through the queue/delivery records (`packages/daemon/src/domain/queue-repository.ts:996-1069,1113-1160`). Exact disconnected-client presentation and reconnect replay output are **Unresolved**. |

**WT-4 disposition:** ordinary create and terminal handoff are explicitly
separated. Actor identity, transaction boundaries, durable records, delivery
ledger, nudge failure, wake-intent recovery, and operator-visible distinctions
are source-linked. Client retry/idempotence, disconnect/reconnect replay,
partial-write behavior outside the cited transactions, and recipient
acceptance remain unresolved.

## Trace 4 — inspect results and reconcile state

| Consumer | Normal source path and output | Failure/reconciliation source path and limit |
|---|---|---|
| CLI | **Observed in source:** `rig status`, `rig ps`, queue show/transitions, and artifact inspection are the documented inspection commands (`docs/reference/getting-started.md:71-80,117-126`). CLI queries daemon state; queue output distinguishes durable task history from message delivery (`packages/cli/src/commands/queue.ts:558-644,743-823`; `packages/cli/src/commands/send.ts:256-272`). | **Observed in source:** daemon guards print fact/consequence/action; tmux observation distinguishes absent from transport-unavailable, and restore blocks on unknown (`packages/cli/src/daemon-lifecycle.ts:102-134`; `packages/daemon/src/adapters/tmux.ts:82-91,133-157`; `packages/daemon/src/domain/restore-orchestrator.ts:797-809`). |
| TUI | **Observed in source:** the TUI workflow model renders workflow status, packet owner/state, blockers, and detail lines (`packages/tui/src/execution/workflow-model.ts:16-29,66-105,144-205`); the connected journey test constructs the daemon-backed queue/event/workflow projection and renders it (`packages/tui/test/workflow-journey.test.ts:27-44,47-74`). **Test assertion:** the execution-view test includes unresolved occurrences and named unknowns in the rendered output (`packages/tui/test/execution-view.test.ts:246-269`). Reconnect behavior and live reconciliation after lost transport are **Unresolved**; no test was run. |
| Browser UI | **Observed in source:** the workflow instance page consumes the shared `GET /api/workflow/:id/trace` projection and renders the instance/trail (`packages/ui/src/components/workflow/WorkflowInstancePage.tsx:7-15,24-31,235-259`). The SSE hook opens `/api/workflow/sse`, invalidates the workflow query family on events, and closes the shared connection when idle (`packages/ui/src/hooks/useWorkflowSse.ts:1-17,22-23,34-59,61-100`). **Test assertion:** UI tests cover the EventSource binding and rendering parity (`packages/ui/test/workflow-sse.test.ts:1-10,15-25,33-85`; `packages/ui/test/workflow-render-parity.test.tsx:1-8,207-218`). Exact browser reconnect semantics, stale projection, and source-vs-live disagreement remain **Unresolved**; the tests were not run. |
| MCP | **Observed in source:** `rig mcp serve` checks daemon status, connects a stdio MCP transport, and `createMcpServer` exposes `rig_up`, `rig_down`, `rig_ps`, and `rig_status` tools through `DaemonClient` (`packages/cli/src/commands/mcp.ts:9-50`; `packages/cli/src/mcp-server.ts:14-52,58-135`). MCP maps HTTP, in-body, and structural daemon errors to `isError` text results (`packages/cli/src/mcp-server.ts:14-44`). **Unresolved:** the inspected MCP tools do not establish queue assignment, workflow reconciliation, or reconnect/replay behavior for these five journeys; evidence needed is a specific MCP tool-to-daemon path and its reconnect/output contract. |
| Source of truth | **Observed in source:** SQLite stores queue/session records and events are persisted before subscriber notification (`08-current-state-evidence.md:288-308,323-330`; `packages/daemon/src/domain/event-bus.ts:52-91`). tmux supplies separate process evidence (`packages/daemon/src/adapters/tmux.ts:82-91,133-157`). | **Inferred:** a consumer may need re-query/reconciliation after transport loss because durable state and live process observation are separate. Exact automatic reconciliation/replay for each consumer is **Unresolved**; no runtime disagreement was demonstrated. |

**WT-5 disposition:** CLI, TUI, and browser UI are cited only where source or
test assertions evidence them; MCP is explicitly unresolved rather than
assumed. Durable SQLite/event state and separate tmux process evidence are
mapped. Consumer reconnect, stale projection, and live disagreement remain
unresolved and are not presented as runtime behavior.

## Trace 5 — stop and resume without losing work

| Link | Normal path | Failure path |
|---|---|---|
| Input | **Stated intent:** stop the rig and later select the existing rig to resume (`docs/reference/getting-started.md:181-190`). | Snapshot failure, host reboot/lost tmux, live or uninspectable session, and missing resume token are distinct cases. |
| Command/client | `rig down`, then `rig up --existing`/existing-rig restore (`docs/reference/getting-started.md:181-190`; `packages/cli/src/commands/up.ts:287-315`). | The restore path returns blockers or decision-needed states instead of silently replacing a conversation (`packages/daemon/src/domain/restore-orchestrator.ts:782-812,976-1006`). |
| Runtime/domain owner | **Observed in source:** teardown attempts an `auto-pre-down` snapshot, kills selected tmux sessions, and clears matching OpenRig state (`packages/daemon/src/domain/rig-teardown.ts:103-135`); restore reconciles stored records against tmux before mutation (`packages/daemon/src/domain/restore-orchestrator.ts:239-250,782-812`). | **Observed in source:** snapshot failure is recorded and teardown continues (`packages/daemon/src/domain/rig-teardown.ts:103-113`); unknown tmux transport blocks restore rather than proving absence (`packages/daemon/src/adapters/tmux.ts:82-91,133-157`; `packages/daemon/src/domain/restore-orchestrator.ts:797-809`). |
| Persistence/process effect | **Observed in source:** snapshot persistence captures continuity/topology metadata, not a full project archive or process memory (`breakdown/gemini-review/claims/architecture/A8-snapshot-restore/README.md:27-41`). The normal teardown path changes OpenRig state and kills sessions; it does not call project deletion in the cited path (`08-current-state-evidence.md:248-266`). | **Inferred:** project files and resumable native conversation are different promises. Snapshot failure may lose live context, but the cited path alone does not prove deletion of saved files. Survival of staged, untracked, nested-repository, or interrupted-cleanup state is **Unresolved**. |
| Operator-visible output | **Observed in source:** CLI exposes teardown results and restore per-seat statuses/warnings (`packages/cli/src/commands/up.ts:530-549`; `docs/reference/getting-started.md:181-198`). | **Observed in source:** missing resume token maps to `awaiting-decision`; live/unknown sessions block restore (`packages/daemon/src/domain/restore-orchestrator.ts:782-812,976-1006`). Actual same-conversation restoration and dirty-worktree preservation are **Unresolved** and were not run. |

## Source-only closure of WT-1 through WT-5

- **WT-1:** every trace row identifies the command/client, runtime/domain
  owner, durable/process effect, and visible result, or names the missing link
  as unresolved.
- **WT-2:** install/start failures are split into handled setup/auth/ambiguity/
  daemon-response paths and unresolved malformed-config, occupied-endpoint, and
  interrupted-start cleanup paths.
- **WT-3:** launch-before-ready, partial launch, process/session divergence,
  managed hooks, and cleanup limits are mapped without claiming compensation or
  runtime success where source does not establish it.
- **WT-4:** ordinary assignment and terminal handoff are kept separate; retry,
  disconnect, reconnect, delivery, and acceptance limits are named explicitly.
- **WT-5:** CLI, TUI, browser UI, and MCP are not collapsed into one consumer;
  only evidenced source/test assertions are cited, with MCP and reconnect gaps
  explicitly unresolved.

This closes source-trace coverage only. It does not claim runtime behavior,
passing tests, measured usability, successful install/recovery, retained scope,
or any Gate A–D status. It does not edit the shared register, task-list
statuses, code, or another agent's output.