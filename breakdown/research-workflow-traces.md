# Five source-level operator workflow traces

## Scope, baseline and evidence rules

Inspected implementation commit: **`ea7c268f576ada8434d3dae3e6ac972264910d4c`**. Implementation citations use this pin. Cross-check references X1–X3 use the main-workspace research snapshot **`dc8a6077baf37d9331d5ad0130dcd64e523d3c44`**; X4 uses accepted workflow merge **`1568d6b5911c5148e6c3e84715389f1009099c24`** (PR #6). Every evidence link names its exact commit, file and inclusive line range. Git comparison found no changes between the implementation and research pins under packages, scripts or the getting-started guide. Evidence IDs are local to this document; they do not change shared verdicts or certify a live workflow.

The five journeys follow the assigned [workflow contract][C1]. The local CLI and one local project are the primary scope. The guide's `first-project` example is an investigation anchor, not an approved retained product scope. Commands in this document were **read, not executed**. No installation, daemon, agent, queue mutation, stop, resume, product test or runtime fault injection was run. Reading a test is not running it.

Classification and confidence:

- **observed-in-source**: executable mechanism in the cited ranges; high confidence within the named branch, not a runtime success claim.
- **stated-intent**: guide, help text or prior review conclusion; not independent implementation proof.
- **inferred**: a bounded consequence of the cited ordering; medium confidence and no broader guarantee.
- **unresolved**: evidence is missing; no outcome confidence. The gap ledger names the next evidence needed.

Each table has a normal and failure path through all five links. A failure means the operator's requested outcome is not established; it need not mean an HTTP error. Daemon readiness, agent interactivity, notification delivery, task acceptance and correct work are different results.

## Trace 1 — install prerequisites and start

Scope: packaged CLI installation/setup, then explicit `rig daemon start`. Project launch follows in Trace 2. This split makes daemon readiness independently visible.

| Link | Normal path — W1-N | Failure path — W1-F |
|---|---|---|
| Input | **stated-intent:** install OpenRig, inspect `rig setup --dry-run`, check executables/auth in the launch shell, then apply only the intended setup. The starter needs tmux and Codex; full setup covers more tools ([I1]). | **observed-in-source:** request daemon start with tmux absent from PATH; preflight produces a failed `tmux` check ([I4]). |
| Client/command | **observed-in-source:** the package exposes `rig` and a Node postinstall ABI check; postinstall loads `better-sqlite3`. Setup invokes `runSetup` and prints individual steps; daemon start resolves configuration and runs `SystemPreflight` ([I2a], [I2b], [I3], [I5]). | **observed-in-source:** `rig daemon start` calls preflight, prints each failed check with reason/fix, sets exit code 1 and returns before calling `startDaemon` ([I5]). |
| Runtime owner | **observed-in-source:** the shipped postinstall owns the native-load check. CLI setup owns tool/config operations. CLI lifecycle reserves startup, initializes the instance and spawns the daemon; daemon entry passes the resolved DB path to `createDaemon` ([I2b], [I6a], [I6b], [I7], [I8]). **inferred:** npm's package installation is an external prerequisite, not daemon-owned. | **observed-in-source:** CLI `SystemPreflight` owns this rejection, not daemon startup. Its message is `tmux was not found in PATH.` with platform install hints ([I4], [I5]). |
| Durable/process effect | **observed-in-source:** setup can install tmux and write the managed tmux configuration block. Startup creates a detached Node child and log output; daemon opens SQLite with WAL/foreign keys and applies unapplied migrations transactionally. CLI publishes daemon state only after child/listener identity and liveness checks ([I6a], [I6b], [I7], [I9a], [I9b], [I9c], [I10a], [I10b]). | **inferred:** this rejected daemon-start request does not reach child spawn or daemon DB migration; earlier installation/setup effects are not undone. This is not a promise that preflight has no host effects or that installation was rolled back ([I5], [I7]). |
| Operator-visible result | **observed-in-source:** setup prints step status and either `Setup complete` or attention guidance. Successful start prints port/PID. Optional `--wait-for-kernel` polls separately and can still exit 1; daemon-ready is not kernel-agent-ready ([I3], [I5]). | **observed-in-source:** failed tmux check, why/fix text, exit 1. **inferred:** fix tmux in the intended launch shell and rerun checks before retrying; no new daemon from this request needs teardown ([I4], [I5]). |

**observed-in-source — additional edges:** occupied-port preflight reports the endpoint and a config/lsof remedy ([I4]). ABI postinstall failure prints its diagnostic and exits 1, but npm's resulting package state was not demonstrated ([I2b]). After child spawn, health/identity failure attempts SIGTERM of only that child; unconfirmed exit retains the reservation and reports the child PID, lock and log to inspect. A failed state publication removes only matching state ([I7], [I10b]). **unresolved:** interruption/cleanup and clean-machine installation require G1.

## Trace 2 — launch agents into the project

Scope: a pod-aware local spec with a Codex seat and successful prerequisite resolution. Failure selection: startup readiness requires attention, rather than silently treating a created tmux session as an interactive agent.

| Link | Normal path — W2-N | Failure path — W2-F |
|---|---|---|
| Input | **stated-intent:** select `first-project`, preview/plan, then launch with the intended working directory ([I1]). | **observed-in-source:** the same launch reaches an adapter/readiness result classified as `attention_required` ([L5b]). |
| Client/command | **observed-in-source:** `rig up first-project --cwd .` posts source, plan flag and resolved cwd to `/api/up`; the route records a running bootstrap operation and calls `BootstrapOrchestrator` ([L1], [L2]). | **observed-in-source:** the same client/request path carries the attention result back; this is not a separate fresh launch or automatic retry ([L1], [L2]). |
| Runtime owner | **observed-in-source:** bootstrap calls `PodRigInstantiator`; it creates rig topology and delegates session creation to `NodeLauncher`, then calls `StartupOrchestrator` with the runtime adapter. Startup projects resources, delivers context, launches the harness and checks readiness. Codex sends its constructed command to tmux ([L3a], [L3b], [L4], [L5a], [L5b], [L6a], [L6b], [L7]). | **observed-in-source:** startup owns the readiness/launch classification and persists failure status. Instantiator preserves the recoverable rig; bootstrap maps it to partial; the route builds the attention response ([L5b], [L8a], [L8b], [L3a], [L9]). |
| Durable/process effect | **observed-in-source:** tmux is created before the atomic session/binding/`node.launched` transaction; subscribers are notified after commit. Startup persists context and, after readiness/context actions, sets startup status `ready` and emits `node.startup_ready`. Codex may record a native resume token ([L4], [L5a], [L5b], [L5c], [L7]). | **observed-in-source:** status becomes `attention_required` and `node.startup_failed` is emitted. With attention nodes, instantiator does not run all-terminal cleanup: rig/session state and panes remain available for inspection. This does not prove the harness is alive. All-terminal failure instead attempts best-effort pane kills and rig deletion ([L8a], [L8b]). |
| Operator-visible result | **observed-in-source:** completed bootstrap returns HTTP 201 with rig/attach information; CLI prints status/stages, rig ID, warnings and attach command ([L2], [L10b]). | **observed-in-source:** attention yields HTTP 409, a fact/consequence/action diagnostic and per-node reasons. CLI prints them and exits 1. The consequence explicitly says members are not proven interactive and a runtime may have exited; action is to inspect named sessions before recovery ([L2], [L9], [L10a]). |

**observed-in-source — other cleanup boundaries:** a failed session DB transaction attempts tmux cleanup ([L4]); that is not proof cleanup succeeded. A CLI apply timeout prints that the daemon may still be processing, suggests `rig ps`, and exits 1 instead of asserting rollback ([L1]). **unresolved:** actual Claude/Codex launch, partial-file cleanup, attention recovery and live readiness require G2. A model/config declaration is not native runtime proof ([I1]).

## Trace 3 — assign and follow work

Scope: a real, transport-identified seat creates a local queue item for a valid pane-bound destination. An unbound human shell is not instructed to impersonate a seat. The guide's initial `rig send` conversation and subsequent durable queue assignment are separate steps ([Q0]).

| Link | Normal path — W3-N | Failure path — W3-F |
|---|---|---|
| Input | **stated-intent:** give a bounded outcome; the agent creates/claims durable work. **observed-in-source:** queue route resolves sender identity and requires destination/body ([Q0], [Q2]). | **observed-in-source:** same valid assignment, but its post-commit terminal notification returns a definite failure or throws a non-timeout error ([Q4a]). |
| Client/command | **observed-in-source:** `rig queue create` posts body, destination, optional ID and nudge options to `/api/queue/create`; `queue claim`/`update` operate on the item; `show`/`transitions` follow it ([Q1], [Q7], [R1]). | **observed-in-source:** create uses the same request; client receives the persisted item even when transport failure is recorded as a nudge outcome. It does not get a fabricated transaction rollback ([Q1], [Q3a], [Q4a]). |
| Runtime owner | **observed-in-source:** daemon queue route validates identity/input and calls `QueueRepository.create`; repository owns the SQLite transaction and post-commit wake send ([Q2], [Q3a], [Q3b], [Q4a]). | **observed-in-source:** `performWakeSend` catches/classifies transport failure; `maybeNudge` records the outcome separately. Gateway destinations have a different owner and are not covered by this pane-bound trace ([Q4a]). |
| Durable/process effect | **observed-in-source:** one transaction creates a pending item, created transition and `queue.created` event; then subscribers are notified and a verified-send request is made to transport. Claim/update are separate later requests, not consequences guaranteed by create ([Q3a], [Q3b], [Q4a], [Q7]). | **observed-in-source:** `failed:<detail>` is written to `last_nudge_result` with `last_nudge_attempt`; already-committed item remains. Timeouts instead classify as indeterminate; successful but unverified sends become `delivered-ack-pending`. No transport or `--no-nudge` skips this send ([Q4a], [Q4b]). |
| Operator-visible result | **observed-in-source:** route returns HTTP 201 and reread row; CLI renders JSON (pretty JSON by default). Operator can inspect item and transition history. **stated-intent:** delivery is not agent acknowledgement or a reviewed result ([Q1], [Q2], [Q5], [Q0], [Q6]). | **observed-in-source:** HTTP 201 still represents successful persistence; generic output does not set an error exit code for nudge failure. Read recorded `lastNudgeResult` with `show` ([Q4c]); `queue undelivered` is the explicit failed-create-nudge inspection surface ([Q4b], [Q5], [Q8], [R1]). **inferred:** inspect destination and existing row before retrying; recreating work is not notification repair. |

**observed-in-source — retry boundary:** an explicit duplicate item ID returns the existing row only when source and destination match; it does not compare the whole body or emit another event/nudge. Reusing that ID with different identities throws `qitem_id_reuse` ([Q3a]). This is not blanket exactly-once behavior for fresh-ID retries. The available state map distinguishes ordinary create from terminal handoff's transactional successor wake intent ([C3]); this trace does not extend that guarantee to ordinary create. **unresolved:** recipient acceptance, real wake delivery, disconnected retry and terminal-handoff recovery require G3.

## Trace 4 — inspect results and diagnose state

Scope: inspect the durable work result with `queue show`/`transitions`, then correlate with status and actual project artifact. Failure selection: a requested queue item does not exist. It is a complete rejected read path, not a hypothetical reconnect scenario.

| Link | Normal path — W4-N | Failure path — W4-F |
|---|---|---|
| Input | **stated-intent:** ask whether assigned outcome was completed and independently checked; read the exact artifact, not only delivery status ([Q0]). | **observed-in-source:** operator requests an absent item ID from this daemon ([R2], [R3]). |
| Client/command | **observed-in-source:** `rig queue show <id> --full --json` gets `/api/queue/<id>`; `rig queue transitions <id>` gets its transitions. Default show is bounded and supplies a full-record command ([R1]). | **observed-in-source:** identical GET request with an unknown ID; error responses bypass preview formatting and go to `printResult` ([R1]). |
| Runtime owner | **observed-in-source:** queue routes call `QueueRepository.getById` and `listTransitions`; `getById` selects SQLite state and adds available delivery-ledger outcome ([R2], [R3]). | **observed-in-source:** repository returns null on no row; route returns HTTP 404 with `qitem_not_found` before transition listing or item rendering ([R2], [R3]). |
| Durable/process effect | **observed-in-source:** item lookup selects the stored row and derives its presentation; it does not claim an agent process is live. Transition history is fetched separately ([R2], [R3]). **inferred:** these reads do not themselves complete, claim or reopen the task. | **observed-in-source:** the no-row branch issues no queue mutation or process launch. **inferred:** failed lookup does not prove the task never existed on another instance, or that project work was lost ([R2], [R3]). |
| Operator-visible result | **observed-in-source:** full JSON record/transition output, or bounded preview with truncation metadata. **stated-intent:** inspect and exercise project artifact and confirm which candidate was reviewed; queue does not independently prove correctness ([R1], [Q5], [Q0]). | **observed-in-source:** JSON error `qitem_not_found`, CLI exit 1 (4xx). No repair guidance is added by this formatter ([Q5], [R1], [R2]). **inferred:** verify ID and target instance before retrying; no mutation from this lookup needs undoing. |

**observed-in-source — status cross-check:** `rig ps` requests `/api/ps`; that route delegates to `PsProjectionService`. Node inspection fetches per-rig nodes; default is snapshot-based activity, while `--full` requests more expensive pane evidence. A failed rig-list HTTP request prints a `rig status` hint and exits 2; a failed node fetch prints a warning and continues with other rigs ([R4a], [R4b], [R4c]). Do not describe every status read as a fresh tmux probe. Restore's separate running-session classifier distinguishes live/stale/unknown and catches probe errors as unknown ([S7]). **unresolved:** full projection/reconnect behavior and UI/TUI/MCP parity require G4; this document does not assert those clients are absent or excluded from product scope.

## Trace 5 — stop and resume

Scope: ordinary `rig down <rig>` without `--delete`, followed by `rig up <rig> --existing`. Normal resume assumes a usable snapshot, valid native token and successful runtime proof. Stopping the rig is not stopping the daemon.

| Link | Normal path — W5-N | Failure path — W5-F |
|---|---|---|
| Input | **observed-in-source:** request stop, then restore existing rig from eligible saved state ([S1a], [S1b], [S4]). | **observed-in-source:** stop encounters snapshot capture failure; separately, return finds a prior resume-if-possible seat with no token. These are independent failure conditions, not a claim that the first necessarily causes the second ([S2a], [S6]). |
| Client/command | **observed-in-source:** down posts rig ID/options to `/api/down`; existing up posts to `/api/up`, whose restore path selects a usable snapshot or checks current-state rehydrate eligibility ([S1a], [L1], [S4]). | **observed-in-source:** same stop request receives accumulated warnings; same restore request receives per-seat decision-needed status ([S1a], [S4], [S6]). |
| Runtime owner | **observed-in-source:** down route calls `RigTeardownOrchestrator`; snapshot capture owns recovery metadata capture. Restore route calls `RestoreOrchestrator`, which uses node launch/startup and runtime resume machinery ([S1b], [S2a], [S3], [S4], [S5a], [S5b], [S5c]). | **observed-in-source:** teardown catches snapshot exceptions and proceeds. Restore classifies missing token before that seat's stale-state clearing and launch, returning `awaiting-decision` ([S2a], [S6]). |
| Durable/process effect | **observed-in-source:** snapshot and event commit together; capture contains topology/session/continuity/startup metadata. Stop halts transcript rotation, kills sessions, marks successfully killed/already-absent sessions exited and clears bindings transactionally; managed guidance cleanup and service teardown also run. Restore launches a new process/session, carries native resume through startup and requires joined identity proof before reporting `resumed` ([S2a], [S2b], [S3], [S5a], [S5b], [S5c], [S5d]). | **observed-in-source:** failed capture adds `Snapshot failed: ...` but does not veto killing sessions. Kill failure leaves that node's session/binding unchanged; deletion, if requested, is blocked by kill failures. Missing-token return starts no session for that seat and does not clear its prior state. Other seats/global restore work are not claimed mutation-free ([S2a], [S6]). |
| Operator-visible result | **observed-in-source:** clean down prints stopped/session count, snapshot and restore instruction. Restore prints aggregate result and per-node statuses; JSON carries response fields ([S1a], [S4], [S8], [L1]). | **observed-in-source:** snapshot error yields warning text/session kill count and CLI exit 2 even though ordinary down route returns HTTP 200. Missing-token restore prints reason and explicit fresh/manual-restore alternative; TTY asks before deliberate fresh launch, while headless remains decision-needed ([S1a], [S1b], [S6], [S8]). |

**observed-in-source — preservation boundary:** teardown selects the rig's Claude guidance file or Codex `AGENTS.md`; cleanup strips managed blocks, may rewrite remaining text and deletes a file when stripped content is empty ([S9a], [S9b]). Thus “stop does not touch project files” is too broad. Snapshot metadata is not a complete checkout, Git index, untracked-file, transcript or volume backup ([S3], [C4]). Live or unknown process evidence can block restore; missing-token refusal is not automatic fresh replacement ([S6], [S7], [S10]). **unresolved:** dirty/index/untracked/nested-worktree preservation, service volume cleanup, reboot recovery and actual conversation continuity require G5.

## Cross-check against available capability and state evidence

**observed-in-source (document availability):** the dedicated research documents are absent at the implementation pin but present at the separately pinned main-workspace snapshot: capability inventory [X1] and state ledger [X2], [X3]. They were read from Git objects, not assumed from a dirty working copy. The accepted dependency/execution inventory [C2], selected state map [C3] and claim-review qualifications [C4] remain inputs. Do not confuse the integrated CAP-4 baseline with acceptance of the ongoing expanded capability card 662c4.

| Available inventory/state area | Workflow alignment and disposition |
|---|---|
| CLI/package/native ABI, external setup, daemon and SQLite ([C2]) | Investigated in W1. Installation outcome, package contents and interrupted migrations remain G1; no dependency removal decision. |
| tmux, adapters, managed hooks/files; rig/node/session/binding/resume ([C2], [C3]) | Investigated local Codex/startup path in W2 and lifecycle in W5. Stored session, interactive readiness and native continuity are not interchangeable. Other adapters and hook ownership need G2/G5. |
| Queue ownership/history, events and notification ([C3], [C4]) | W3 follows ordinary create and W4 reads result/history. Event persistence precedes wake. Terminal handoff, remote transactions and acceptance remain qualified; see G3. |
| CLI, TUI, browser UI and MCP ([C2]) | CLI primary paths investigated. Other consumer implementations and reconnect parity deferred from paired traces with explicit G4 evidence needs, not marked removed or retained. |
| Snapshot, transcript, filesystem and process state ([C3], [C4]) | W5 confirms conditional resume and best-effort capture/cleanup, not a backup contract. Managed-guidance edits narrow the earlier “project directory untouched” reading. G5 remains open. |
| Runners, operational upgrade/backup scripts, build/test/generation, packaging/CI/testbed, lockfile/native install and update/reinstall ([C2]) | Inventory cross-check only. These are adjacent workflows, not silently covered by a successful local start. G6 names missing package/install/update evidence. |
| Multi-host, explicit workflow engine, context/spec/plugin and workspace surfaces named in capability assignment ([C1]) | Only incidental local spec/projection/workspace dependencies are followed here. The available inventory's explicit omissions remain explicit below; G6 prevents an exhaustive-capability claim. |

The accepted linked claims distinguish durable identity from liveness, snapshot recovery from backup, and a wake intent from receipt or correct action ([C4]). These traces preserve those qualifications. The available-artifact reconciliation is complete below; acceptance of other ongoing cards, integration into main, and Gates A–D remain separate. The user's continuation/closure instruction does not authorize changing their outputs or decisions.

### Capability inventory reconciliation

**observed-in-source (research artifact):** [X1] gives eleven named surface rows and explicit omissions. The following covers every row without upgrading its evidence or deciding owner value. W1–W5 correspond to its provisional J1–J5, not a promise that every capability is required.

| Inventory row | Workflow cross-check | Disposition / next evidence |
|---|---|---|
| CAP C1 — MCP | W1/W2/W4/W5 share daemon operations; accepted PR #6 preserves MCP error mapping ([X4]). | Consumer evidence retained below; full tool parity/reconnect remains G4. |
| CAP C2 — Browser UI | W4 inspection; workflow trace page and SSE refresh are present in PR #6 ([X4]). | Do not erase this prior source evidence; runtime reconnect remains G4. |
| CAP C3 — TUI | W4 result projection and adjacent W3/W5 operations ([X1], [X4]). | Prior rendering evidence retained; no live parity claim, G4. |
| CAP C4 — Gateway/Slack | Adjacent W3 delivery/W4 inspection, not W3's pane-bound transport ([X1], [Q4a]). | Implemented connector and deferred model-divergence notification remain distinct; end-to-end delivery G3/G6. |
| CAP C5 — Pi | Alternate W2/W5 adapter, not demonstrated by Codex trace ([X1]). | Runtime/provider configuration and teardown evidence remain G2/G5. |
| CAP C6 — Stub/test-system | Verification infrastructure for journey-like cases, not operator success ([X1]). | Preserve distinction between daemon fixture runner and unproven test-system scenario consumer; G6, no tests run. |
| CAP C7 — Plugins | Adjacent W2 projection/W4 discovery ([X1]). | Read-only discovery does not establish writer/cleanup chain; G2/G6. |
| CAP C8 — Specs/workspace/context | W1/W2 inputs and projections; W3 context is only an adjacent use ([X1], [L5a]). | Concrete launch chain traced; complete library lifecycle remains G6. |
| CAP C9 — Multi-host | Adjacent distributed W3/W4, outside the local paired trace ([X1]). | No remote transaction or retry guarantees borrowed from local create; G3/G6. |
| CAP C10 — Providers/services | W2 process creation and W5 delegated service teardown ([X1], [S11], [S12]). | Compose policies now explicit below; other provider lifecycles remain G2/G5. |
| CAP C11 — Generated docs/skills | W1 packaging/W2 startup context, not merely passive prose ([X1]). | Packaging/generation failure and ownership remain G6. |
| Explicit omissions in CAP-4 | Workflow/views/images/files/terminal/health/queue API families, copied assets, Slack deferral and scenario invocation ([X1]). | Core queue/start/stop subsets are traced here; remaining adjacent paths stay G6, not silently claimed complete. |

### State ledger and accepted workflow reconciliation

**observed-in-source (research artifact):** [X2] separates project catalog/files, Git index and worktrees, seat/session/occupant identity, native conversation/transcript, queue/history, events/snapshots, managed configuration and daemon/services. W1's instance/DB effects, W2's projection/session ordering, W3's commit-before-wake, W4's read-only lookup and W5's conditional restore agree with those distinctions. [X3] expressly leaves exhaustive writers/deleters and runtime preservation unresolved; this document does not close those state-card obligations.

**observed-in-source — delegated deletion:** W5 calls service teardown even without `--delete`. The service owner reads persisted policy (default `down`); Compose makes `leave_running` a no-op and adds `--volumes` for `down_and_volumes` ([S11], [S12]). Thus configured volumes can be removed by ordinary rig stop. Moreover, teardown awaits the service call but does not inspect a returned `{ok:false}`; only a thrown error reaches its warning catch ([S2a], [S11]). **inferred:** a non-throwing Compose failure can be absent from the CLI's teardown warnings. Actual volume effects and error presentation remain G5. This narrows, rather than repeats, any broad “files untouched” claim.

**observed-in-source (accepted artifact):** PR #6 already included TUI workflow-model rendering, browser workflow-page/shared SSE invalidation, and MCP stdio-to-daemon tooling with `isError` mapping ([X4]). Those source findings are retained as inherited evidence, not newly executed checks. Its referenced tests remain test assertions, not passes. This revision adds complete five-link failure pairs, direct command/route/result citations and concrete cleanup boundaries; it does not invalidate the earlier artifact's acceptance or broaden its runtime claims.

**stated-intent / unresolved:** the available state ledger explicitly cites PR #6 as accepted ([X3]), but that does not make this new revision independently reviewed or merged. Artifact integration remains the normal handoff, not an unsatisfied source-evidence link. No prerequisite is claimed accepted solely because a Kanban column changed.

## Missing evidence and next evidence needed

Rows G1–G6 are **unresolved**. G0 records the closed availability/cross-check gap. Source-path completion is separate from runtime demonstration.

| ID | Missing evidence | Next evidence needed |
|---|---|---|
| G0 | **Closed for this source-only card:** available capability/state artifacts and accepted workflow PR are pinned and reconciled above ([X1]–[X4]). | Future accepted revisions require a delta check; do not mistake the ongoing capability/state cards or main integration for already-completed acceptance. No missing input remains for this available-artifact comparison. |
| G1 | Clean installation/auth; missing tools, occupied endpoint, interrupted child startup and migration/cleanup outcomes. | Authorized disposable home/DB and package install; record versions, commands, output/exit status, lock/state/log files and process/DB state before/after each fault. Check leftover package/setup changes separately. |
| G2 | Actual agent readiness, Claude parity, cwd/isolation, partial launch/projection cleanup and attention recovery. | Isolated Claude/Codex rigs; capture topology/session rows, projected-file diffs, process identity and native terminal output through successful, attention and terminal-failure launch. Prove cleanup rather than assuming best-effort calls succeeded. |
| G3 | Recipient acceptance/completion, missed wake repair, disconnect/duplicate handling and terminal-handoff wake recovery. | Disposable queue/transport with controlled failures; record item/transition/event/wake state and terminal evidence before/after commit, response loss and restart. Compare same-ID and new-ID retries; observe recipient action separately from pane text. |
| G4 | Stale-process display, reconnect/replay and CLI/TUI/UI/MCP consistency. | Trace each additional consumer's query/subscription/projection implementation, then run authorized disconnection and stale-process cases; compare display to DB and independently observed process state. No client parity claim follows from W4. |
| G5 | Work preservation and exact conversation continuity across stop, snapshot failure, kill failure, reboot and optional service/workspace cleanup. | Inspect delegated cleanup/removal paths; on disposable copies compare tracked bytes, staged index, untracked files, nested repos/worktree metadata, managed guidance, transcripts and service volumes before/after. Verify native token/process identity and resumed conversation independently. |
| G6 | Full capability disposition and adjacent install/update/packaging/plugin/remote workflows beyond the cross-checked baseline. | Use the inventory reconciliation above to select remaining entrypoints through owners/effects/output. Recheck later accepted inventory deltas; use package/update/remote demonstrations only under separate authorization. Do not infer retained product scope. |

## Validation and review boundary

Coverage contract: W1-N/F through W5-N/F each contain Input, Client/command, Runtime owner, Durable/process effect and Operator-visible result. Each unresolved item names next evidence. Citations below are commit-pinned, not floating working-tree line numbers.

Validation for this documentation-only revision uses Git-tree citation/range checks, reference-definition checks, five-journey/two-path/five-link coverage checks, manual source-to-claim review, `git diff --check` and a one-owned-path diff check. Product tests and lifecycle demonstrations are not substitutes for these checks and were not run. Check results and resulting documentation commit are reported in the task handoff, avoiding a self-referential commit claim here.

No implementation, shared index/register, owner decision or Gate A–D change is part of this document. The five source-level paired traces and available-artifact cross-check are complete. This document does not itself record independent reviewer acceptance, runtime proof, main integration or a gate pass.

## Pinned evidence references

[C1]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/breakdown/11-coordination-outcomes.md#L119-L190 "breakdown/11-coordination-outcomes.md:119-190"

[C2]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/breakdown/10-rust-and-node-removal-plan.md#L66-L91 "breakdown/10-rust-and-node-removal-plan.md:66-91"

[C3]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/breakdown/08-current-state-evidence.md#L283-L330 "breakdown/08-current-state-evidence.md:283-330"

[C4]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/breakdown/gemini-review/README.md#L39-L69 "breakdown/gemini-review/README.md:39-69"

[I1]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/docs/reference/getting-started.md#L54-L80 "docs/reference/getting-started.md:54-80"

[I2a]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/package.json#L28-L45 "packages/cli/package.json:28-45"

[I2b]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/scripts/check-abi.mjs#L104-L122 "packages/cli/scripts/check-abi.mjs:104-122"

[I3]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/src/commands/setup.ts#L756-L797 "packages/cli/src/commands/setup.ts:756-797"

[I4]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/src/system-preflight.ts#L138-L192 "packages/cli/src/system-preflight.ts:138-192"

[I5]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/src/commands/daemon.ts#L124-L227 "packages/cli/src/commands/daemon.ts:124-227"

[I6a]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/src/commands/setup.ts#L357-L380 "packages/cli/src/commands/setup.ts:357-380"

[I6b]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/src/commands/setup.ts#L586-L624 "packages/cli/src/commands/setup.ts:586-624"

[I7]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/src/daemon-lifecycle.ts#L581-L665 "packages/cli/src/daemon-lifecycle.ts:581-665"

[I8]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/index.ts#L236-L319 "packages/daemon/src/index.ts:236-319"

[I9a]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/startup.ts#L240-L249 "packages/daemon/src/startup.ts:240-249"

[I9b]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/db/connection.ts#L7-L13 "packages/daemon/src/db/connection.ts:7-13"

[I9c]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/db/migrate.ts#L13-L42 "packages/daemon/src/db/migrate.ts:13-42"

[I10a]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/src/daemon-lifecycle.ts#L688-L748 "packages/cli/src/daemon-lifecycle.ts:688-748"

[I10b]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/src/daemon-lifecycle.ts#L750-L795 "packages/cli/src/daemon-lifecycle.ts:750-795"

[L1]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/src/commands/up.ts#L384-L422 "packages/cli/src/commands/up.ts:384-422"

[L2]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/routes/up.ts#L369-L424 "packages/daemon/src/routes/up.ts:369-424"

[L3a]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/bootstrap-orchestrator.ts#L635-L734 "packages/daemon/src/domain/bootstrap-orchestrator.ts:635-734"

[L3b]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/rigspec-instantiator.ts#L1255-L1298 "packages/daemon/src/domain/rigspec-instantiator.ts:1255-1298"

[L4]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/node-launcher.ts#L126-L238 "packages/daemon/src/domain/node-launcher.ts:126-238"

[L5a]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/startup-orchestrator.ts#L157-L223 "packages/daemon/src/domain/startup-orchestrator.ts:157-223"

[L5b]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/startup-orchestrator.ts#L228-L325 "packages/daemon/src/domain/startup-orchestrator.ts:228-325"

[L5c]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/startup-orchestrator.ts#L392-L445 "packages/daemon/src/domain/startup-orchestrator.ts:392-445"

[L6a]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/rigspec-instantiator.ts#L1924-L1964 "packages/daemon/src/domain/rigspec-instantiator.ts:1924-1964"

[L6b]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/rigspec-instantiator.ts#L2110-L2178 "packages/daemon/src/domain/rigspec-instantiator.ts:2110-2178"

[L7]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/adapters/codex-runtime-adapter.ts#L383-L412 "packages/daemon/src/adapters/codex-runtime-adapter.ts:383-412"

[L8a]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/startup-orchestrator.ts#L496-L512 "packages/daemon/src/domain/startup-orchestrator.ts:496-512"

[L8b]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/rigspec-instantiator.ts#L1510-L1596 "packages/daemon/src/domain/rigspec-instantiator.ts:1510-1596"

[L9]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/routes/up.ts#L32-L67 "packages/daemon/src/routes/up.ts:32-67"

[L10a]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/src/commands/up.ts#L444-L487 "packages/cli/src/commands/up.ts:444-487"

[L10b]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/src/commands/up.ts#L608-L635 "packages/cli/src/commands/up.ts:608-635"

[Q0]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/docs/reference/getting-started.md#L90-L126 "docs/reference/getting-started.md:90-126"

[Q1]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/src/commands/queue.ts#L520-L554 "packages/cli/src/commands/queue.ts:520-554"

[Q2]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/routes/queue.ts#L389-L479 "packages/daemon/src/routes/queue.ts:389-479"

[Q3a]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/queue-repository.ts#L1310-L1358 "packages/daemon/src/domain/queue-repository.ts:1310-1358"

[Q3b]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/queue-repository.ts#L1438-L1499 "packages/daemon/src/domain/queue-repository.ts:1438-1499"

[Q4a]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/queue-repository.ts#L1187-L1307 "packages/daemon/src/domain/queue-repository.ts:1187-1307"

[Q4b]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/queue-repository.ts#L3330-L3338 "packages/daemon/src/domain/queue-repository.ts:3330-3338"

[Q5]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/src/commands/queue.ts#L139-L146 "packages/cli/src/commands/queue.ts:139-146"

[Q6]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/src/commands/send.ts#L256-L272 "packages/cli/src/commands/send.ts:256-272"

[Q7]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/src/commands/queue.ts#L558-L641 "packages/cli/src/commands/queue.ts:558-641"

[Q8]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/src/commands/queue.ts#L1134-L1169 "packages/cli/src/commands/queue.ts:1134-1169"

[R1]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/src/commands/queue.ts#L965-L1007 "packages/cli/src/commands/queue.ts:965-1007"

[R2]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/routes/queue.ts#L970-L985 "packages/daemon/src/routes/queue.ts:970-985"

[R3]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/queue-repository.ts#L2939-L2952 "packages/daemon/src/domain/queue-repository.ts:2939-2952"

[R4a]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/src/commands/ps.ts#L921-L929 "packages/cli/src/commands/ps.ts:921-929"

[R4b]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/src/commands/ps.ts#L1068-L1107 "packages/cli/src/commands/ps.ts:1068-1107"

[R4c]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/routes/ps.ts#L7-L16 "packages/daemon/src/routes/ps.ts:7-16"

[S1a]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/src/commands/down.ts#L182-L247 "packages/cli/src/commands/down.ts:182-247"

[S1b]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/routes/down.ts#L22-L61 "packages/daemon/src/routes/down.ts:22-61"

[S2a]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/rig-teardown.ts#L82-L167 "packages/daemon/src/domain/rig-teardown.ts:82-167"

[S2b]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/rig-teardown.ts#L170-L176 "packages/daemon/src/domain/rig-teardown.ts:170-176"

[S3]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/snapshot-capture.ts#L60-L166 "packages/daemon/src/domain/snapshot-capture.ts:60-166"

[S4]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/routes/up.ts#L97-L195 "packages/daemon/src/routes/up.ts:97-195"

[S5a]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/restore-orchestrator.ts#L1085-L1106 "packages/daemon/src/domain/restore-orchestrator.ts:1085-1106"

[S5b]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/restore-orchestrator.ts#L1333-L1358 "packages/daemon/src/domain/restore-orchestrator.ts:1333-1358"

[S5c]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/restore-orchestrator.ts#L1375-L1399 "packages/daemon/src/domain/restore-orchestrator.ts:1375-1399"

[S5d]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/restore-orchestrator.ts#L1450-L1493 "packages/daemon/src/domain/restore-orchestrator.ts:1450-1493"

[S6]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/restore-orchestrator.ts#L976-L1006 "packages/daemon/src/domain/restore-orchestrator.ts:976-1006"

[S7]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/restore-orchestrator.ts#L782-L812 "packages/daemon/src/domain/restore-orchestrator.ts:782-812"

[S8]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/src/commands/up.ts#L522-L607 "packages/cli/src/commands/up.ts:522-607"

[S9a]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/rig-teardown.ts#L202-L231 "packages/daemon/src/domain/rig-teardown.ts:202-231"

[S9b]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/managed-blocks.ts#L88-L116 "packages/daemon/src/domain/managed-blocks.ts:88-116"

[S10]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/restore-orchestrator.ts#L239-L250 "packages/daemon/src/domain/restore-orchestrator.ts:239-250"

[Q4c]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/queue-repository.ts#L3536-L3545 "packages/daemon/src/domain/queue-repository.ts:3536-3545"

[X1]: https://github.com/kairin/openrig-breakdown/blob/dc8a6077baf37d9331d5ad0130dcd64e523d3c44/breakdown/research-capability-inventory.md#L3-L90 "breakdown/research-capability-inventory.md:3-90"

[X2]: https://github.com/kairin/openrig-breakdown/blob/dc8a6077baf37d9331d5ad0130dcd64e523d3c44/breakdown/research-state-invariants.md#L69-L81 "breakdown/research-state-invariants.md:69-81"

[X3]: https://github.com/kairin/openrig-breakdown/blob/dc8a6077baf37d9331d5ad0130dcd64e523d3c44/breakdown/research-state-invariants.md#L83-L159 "breakdown/research-state-invariants.md:83-159"

[X4]: https://github.com/kairin/openrig-breakdown/blob/1568d6b5911c5148e6c3e84715389f1009099c24/breakdown/research-workflow-traces.md#L71-L84 "breakdown/research-workflow-traces.md:71-84"

[S11]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/service-orchestrator.ts#L136-L163 "packages/daemon/src/domain/service-orchestrator.ts:136-163"

[S12]: https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/adapters/compose-services-adapter.ts#L80-L100 "packages/daemon/src/adapters/compose-services-adapter.ts:80-100"
