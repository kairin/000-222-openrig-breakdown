# State invariants and recovery boundaries

**Research status:** source reading only; no implementation choice or owner
decision. The accepted linked-claim pages and workflow-trace draft cite source
revision `9db3ed6c406be5c3d9a84720383fcf6b543169e6`. The state source paths
were inspected at that exact commit unless stated otherwise; claims inherited
from linked pages are explicitly identified as such. The claim register was
reconciled from integrated pages at
`93069eb13ac621f8db445e7866af978169f3ff60`. Line ranges refer to
repository-relative paths at the inspected source revision unless a different
revision is stated. The state draft originated at
`03f508c53348d4a343c423cd751e7ab74dc706cb`; this revision updates that owned
artifact only.

Use these labels literally:

- **Source observation**: behavior or intent visible in the named source.
- **Stated intent**: a product or schema comment says this is the intended
  contract; it is not runtime proof.
- **Inference**: a bounded implication drawn from source observations.
- **Unresolved**: source reading does not settle the behavior or owner policy.
- **Test assertion only**: a test file encodes an assertion; it was read, not
  executed, and does not establish passing behavior.

This note adds no verdict to the claim register. It reuses accepted linked
findings from [A5/F4/S2](gemini-review/claims/architecture/A5-seat-outlives-process/README.md),
[A8/F9/S5](gemini-review/claims/architecture/A8-snapshot-restore/README.md),
and [the accepted source-review summary](gemini-review/README.md#linked-group-conclusions).
Their claims were reviewed at `9db3ed6c406be5c3d9a84720383fcf6b543169e6`;
their tests were inspected, not run. The summary explicitly leaves workflow
traces, this state research, runtime validation and owner decisions open.

## Recovery is several different promises

- **Stated intent:** the root README requires stopping without losing
  uncommitted work (`README.md:3-6,23-34` at `9db3ed6c`). That states an
  outcome; it does not identify all files or establish that each teardown path
  achieves it. This is the **file-preservation promise**, not a conversation
  recovery contract.
- **Source observation:** a snapshot reads topology, session/resume metadata,
  checkpoints, startup context and selected continuity/service receipt state;
  it persists the snapshot and `snapshot.created` event in one SQLite
  transaction (`packages/daemon/src/domain/snapshot-capture.ts:60-166`).
- **Source observation:** when any live session exists, teardown attempts this
  snapshot before killing sessions, but records a capture error and continues. Successful per-session
  termination then updates session status and clears its binding; optional rig
  deletion is a separate transaction (`packages/daemon/src/domain/rig-teardown.ts:103-193`).
- **Source observation:** when no live session exists, the `alreadyStopped`
  branch skips snapshot capture, cleans selected guidance files, attempts
  service teardown, then stops or deletes (`packages/daemon/src/domain/rig-teardown.ts:85-100`).
- **Stated intent:** rig and node deletion must not erase event history;
  snapshots likewise have no rig foreign key and are intended to survive rig
  deletion (`packages/daemon/src/db/migrations/003_events.ts:6-21`,
  `packages/daemon/src/db/migrations/004_snapshots.ts:6-18`).
- **Inference:** database continuity metadata, Git working files and native
  conversation history are separate persistence systems. A saved snapshot can
  support a restore attempt without containing all three.
- **Source observation:** restore classifies a prior session without a usable
  token as `awaiting-decision` before launch, and reports explicit resume
  failure/attention rather than silently calling it resumed
  (`packages/daemon/src/domain/restore-orchestrator.ts:976-1006,1175-1237`).
  This is a **conversation-recovery boundary**; it says nothing about whether
  the checkout, Git index, or untracked files remain intact.
- **Unresolved:** source inspection does not prove that uncommitted files
  survive power loss, external volume teardown, manual cleanup, or every
  failure path. The accepted A8/F9/S5 pages make this same boundary explicit;
  no real reboot or recovery was demonstrated.

## State inventory

| State surface | Authority and identity lifetime | Creators, writers and deleters | Atomicity, events and disagreement |
| --- | --- | --- | --- |
| **Project catalog and project files** | **Source observation:** the workspace catalog (`workspace.yaml` by default) supplies project `id` and relative `root`; duplicate IDs are rejected. Catalog entries resolve to real paths; without a catalog, the workspace root is one project (`packages/daemon/src/domain/workspace/project-read.ts:10-16,61-81`; `packages/daemon/src/domain/workspace/project-catalog.ts:15-37`). **Inference:** project identity persists only as long as the external catalog/project source exists; it is distinct from rig/seat IDs. | **Source observation:** `ensureDefaultWorkspace` creates missing default `SPEC.md`, `project.yaml`, `workspace.yaml`, `.gitignore`, and directories after prechecking collisions; it skips existing paths (`packages/daemon/src/domain/workspace/default-workspace-scaffold.ts:47-54,97-152`). `work-install` reads catalog and `project.yaml` for addressing (`packages/cli/src/lib/work-install.ts:206-247`). Project readers validate identity, mission root and source containment (`packages/daemon/src/domain/workspace/project-read.ts:44-59,86-103`). **Unresolved:** no canonical project/catalog rename or delete authority was established. | **Source observation:** conflicting catalog/manifest IDs and missing/escaping roots fail closed (`packages/daemon/src/domain/workspace/project-read.ts:47-59,70-84`). Scaffold writes are filesystem operations, not part of a SQLite transaction. **Inference:** changed roots need explicit rebinding; source does not prove files moved or recovered. |
| **Repository index and tracked/untracked/ignored files** | **Source observation:** snapshot payload enumerates rig/topology/session/continuity/startup/checkpoint/receipt data, not repository contents or Git index (`packages/daemon/src/domain/snapshot-capture.ts:60-147`; A8 accepted claim, `breakdown/gemini-review/claims/architecture/A8-snapshot-restore/README.md:25-28`). **Inference:** index, staged blobs, unstaged edits, untracked files, and ignored files are not reconstructible from that SQLite snapshot. | **Source observation:** additive workspace initialization writes only missing scaffold files and skips existing paths (`packages/daemon/src/domain/workspace/default-workspace-scaffold.ts:97-142`). **Unresolved:** project/agent/tool/Git actors that can write, reset, clean or delete working files have not been exhaustively enumerated. No preservation guarantee follows. | **Source observation:** snapshot capture and session/service teardown are separate from Git/filesystem effects (`packages/daemon/src/domain/rig-teardown.ts:103-145`; `snapshot-capture.ts:149-165`). **Inference:** there is no transaction spanning DB, Git index, and working-file bytes. **Unresolved:** crash, disk, external volume, Git clean/reset, and user deletion paths require separate evidence. |
| **Git worktrees** | **Source observation:** execution view treats `worktree_path=...` in queue text as a hint and asks Git for branch/HEAD (`packages/daemon/src/domain/execution-view.ts:730-785`). **Inference:** path hint is neither a registered-worktree identity nor a worktree archive. | **Unresolved:** no authoritative OpenRig create/remove owner established in the inspected source. The path could be created/removed by Git, user, agent, CI, or external tooling; exhaustive callers of `git worktree add/remove/prune` and delegated cleanup are not proven. | **Source observation:** unreachable worktree context leaves view values indeterminate (`execution-view.ts:923-930`). Teardown source does not establish worktree removal; separate destructive paths exist for services/state, not a general Git cleanup. **Unresolved:** registration-vs-filesystem disagreement, prune/recovery, and ownership decision. |
| **Rig, node, binding, session and occupant identity** | **Source observation:** `(rig_id, logical_id)` addresses a role; nodes, bindings, and sessions are separate rows (`packages/daemon/src/db/migrations/001_core_schema.ts:7-37`; `002_bindings_sessions.ts:6-29`). Session registration separately mints/continues occupant tenure (`session-registry.ts:89-110,135-141`). Accepted A5/F4/S2 separates role identity from process liveness (`breakdown/gemini-review/claims/architecture/A5-seat-outlives-process/README.md:27-37`). | **Source observation:** reconciler detaches missing sessions and leaves probe exceptions unresolved (`packages/daemon/src/domain/reconciler.ts:44-96`). Teardown marks successfully killed/missing sessions exited and clears bindings (`rig-teardown.ts:115-135,170-177`). Optional deletion removes rig rows separately and cascades topology by schema (`rig-teardown.ts:147-193`; `001_core_schema.ts:7-37`). Generation invalidation can release claims only with a generation UUID (`packages/daemon/src/domain/occupant-invalidator.ts:43-86`). | **Source observation:** row changes can share SQLite transaction; subscriber notification follows persistence (`packages/daemon/src/domain/event-bus.ts:52-91`). **Inference:** role identity can outlive process and occupant tenure; stale claims may be reset without deleting work. **Unresolved:** positive terminal/session presence is not model responsiveness or successful conversation recovery. |
| **Native conversation and pane transcript** | **Source observation:** session rows hold resume type/token, provenance and verification (`packages/daemon/src/domain/session-registry.ts:41-55,340-392`). Bounded terminal captures live separately at `<rig>/<session>.log` (`packages/daemon/src/domain/transcript-store.ts:181-203`; `node-launcher.ts:161-179`) and are not the harness's full conversation history (A8, `breakdown/gemini-review/claims/architecture/A8-snapshot-restore/README.md:25-28`). | **Source observation:** registration creates session/occupant tenure (`session-registry.ts:89-130`); rotation writes temp+rename and preserves boundary markers (`transcript-rotation.ts:101-149`); restore may append a boundary marker (`transcript-store.ts:249-264`). Snapshot includes selected resume metadata, not transcript bytes (`snapshot-capture.ts:67-80,125-147`). | **Source observation:** rig teardown stops rotation timers, not transcript deletion (`rig-teardown.ts:115-135`; `transcript-rotation.ts:159-173`). `rig destroy` explicitly targets the configured transcripts directory and deletes it or renames it to a backup (`packages/cli/src/destroy-helpers.ts:99-134,223-252`); default path is under OpenRig home unless overridden (`packages/cli/src/commands/destroy.ts:31-68`). **Unresolved:** provider-native history deletion/retention and external cleanup ownership. **Inference:** transcript, native conversation recovery, and repository files are three independent promises. |
| **Queue items and history** | **Source observation:** SQLite is the canonical live queue; schema rows keep source/destination seat names, state, body, closure metadata and claim timing. Markdown mirrors are read-only debug/export (`packages/daemon/src/db/migrations/024_queue_items.ts:4-18,22-51`). The transition schema describes append-only audit history (`packages/daemon/src/db/migrations/025_queue_transitions.ts:4-28`). | **Source observation:** domain operations create/update items and transitions. Ordinary create commits row/event before notification and a best-effort nudge; terminal handoff stages a wake intent with close+successor in one transaction, then delivers post-commit (`packages/daemon/src/domain/queue-repository.ts:807-846,910-945,1165-1198,1320-1358,1608-1669`; `breakdown/08-current-state-evidence.md:222-241`). Retention moves aged terminal qitem transitions into an archive in a per-item transaction and leaves the qitem row in place; readers union active and archived histories (`packages/daemon/src/domain/queue-retention.ts:124-217`; `packages/daemon/src/domain/queue-transition-log.ts:151-160`). | **Source observation:** generation retirement can release a claim to pending; wake recovery drains pending intents only; transition archival selects old terminal qitems and excludes live workflow frontiers (`packages/daemon/src/domain/queue-repository.ts:3424-3458,1143-1162`; `packages/daemon/src/domain/queue-retention.ts:142-173`). **Inference:** durable assignment is not proof of delivery/completion; archival moves history rather than deleting the intended audit trail. **Unresolved:** recipient action and runtime retry behavior remain unmeasured. |
| **Events and snapshots** | **Stated intent:** event rows have no rig/node foreign keys and are intended to outlive rig/node deletion; snapshots lack a rig FK (`packages/daemon/src/db/migrations/003_events.ts:6-21`; `004_snapshots.ts:6-18`). **Source observation:** event insert order is SQLite sequence; subscriber delivery occurs after commit (`event-bus.ts:52-91,105-143,161-178`). | **Source observation:** `EventBus.persistWithinTransaction` inserts events; snapshot capture transaction commits payload and `snapshot.created` together (`packages/daemon/src/domain/snapshot-capture.ts:125-166`). Periodic scheduler captures `auto-periodic` and prunes same-kind snapshots (`periodic-snapshot-scheduler.ts:56-89`); repository retention removes older snapshots of that kind, retaining at least one (`snapshot-repository.ts:184-209`). | **Source observation:** rig delete and `rig.deleted` event share DB transaction, then notification follows (`rig-teardown.ts:179-193`). Snapshot retention is distinct from rig deletion. **Unresolved:** destroy of the DB erases event/snapshot history unless backed up; bounded source search is not proof no other deletion path. DB event replay does not repair external filesystem, tmux, provider-history, or service-volume state. |
| **Owned vs user-owned configuration/hooks/projected files** | **Source observation:** catalog and project roots are filesystem inputs selected through workspace settings; project reads enforce realpath containment (`packages/daemon/src/domain/workspace/project-read.ts:10-42`). Ownership labels are not uniform: managed blocks carry sentinels, while global CLI config may be user-edited. | **Source observation:** setup edits a managed `~/.tmux.conf` block (`packages/cli/src/commands/setup.ts:586-615`); config reset may unlink the whole config (`packages/cli/src/config-store.ts:1184-1269`). Claude/Codex adapters reconcile hook projections (`claude-code-adapter.ts:696-821`; `codex-runtime-adapter.ts:107-224`). `removeManagedBlocksFromFile` strips recognized managed sentinels and deletes the target if no content remains (`packages/daemon/src/domain/managed-blocks.ts:88-116`). Teardown selects Claude's configured managed file or Codex `AGENTS.md`, then invokes this helper (`rig-teardown.ts:202-231`). | **Source observation:** regular `down` calls guidance cleanup both when sessions are live and already stopped; optional rig deletion is separate, and service teardown is delegated (`rig-teardown.ts:85-100,115-165`). This cleanup can delete a selected target if stripping leaves it empty. **Unresolved:** full adapter/global-hook/plugin/copied-asset removal and owner-edit collision behavior is not established by those calls; hash manifest stores only last-write marker, not original bytes (`projection-manifest-store.ts:19-60`). Snapshot projection descriptions are not file backups (`snapshot-capture.ts:104-147`). Do not generalize this managed-block behavior into user-file preservation. |
| **Daemon, processes, services and external volumes** | **Source observation:** daemon opens/migrates SQLite on startup (workflow draft `packages/daemon/src/startup.ts:240-249` at `9db3ed6c`); tmux evidence and stored session status are separate (A5/S2). | **Source observation:** normal teardown kills selected sessions, updates DB only on successful/missing-session response, and calls service orchestrator (`rig-teardown.ts:115-145`). Service policy defaults to `down`; configured `down_and_volumes` invokes `docker compose down --volumes`, while `leave_running` is a no-op (`service-orchestrator.ts:136-163`; `compose-services-adapter.ts:80-100`). Total launch failure has a separate best-effort orphan-session kill then rig-row deletion (`rigspec-instantiator.ts:144-180`). | **Source observation:** `rig destroy` plans state root, configured DB/WAL/SHM, and transcripts as targets; after daemon/port checks it recursively deletes or renames targets, recreates state root, and for `--all` attempts managed tmux kills (`packages/cli/src/destroy-helpers.ts:99-134,168-252`; CLI entry `packages/cli/src/commands/destroy.ts:31-68,166-207`). `--backup` renames targets; this is not a consistency transaction across DB/transcripts/tmux. `archiveRig` is soft archive retaining topology/snapshots, while `deleteRig` deletes the DB row (`rig-repository.ts:554-580`; migrations `001_core_schema.ts:17-37`). **Unresolved:** abrupt-exit/OS cleanup, external volumes and agent-owned content. Snapshot is not process memory or volume backup. |

## Invariants cross-checked against accepted workflow traces

The accepted workflow-trace artifact is
`breakdown/research-workflow-traces.md` at merge commit
`1568d6b5911c5148e6c3e84715389f1009099c24` (PR #6); it states source-only
coverage and no runtime behavior or gates (lines 5-12, 115-118). I also
inspected the earlier a4d7 draft at
`8bd371a8abdc1ac5813c1d2fa042516cb8e1f55d`, which its own lines 84-107 marked
unaccepted. Below, “trace” refers to the accepted PR #6 commit; the historical
draft is not used as accepted evidence.

1. **Source observation:** do not collapse project ID, Git repository/worktree,
   rig ID, logical seat ID, session ID and occupant generation into one identity.
   The schema gives them different owners and lifetimes
   (`packages/daemon/src/db/migrations/001_core_schema.ts:7-37`;
   `packages/daemon/src/db/migrations/002_bindings_sessions.ts:6-29`;
   accepted A5/S2 evidence at `breakdown/gemini-review/claims/architecture/A5-seat-outlives-process/README.md:27-37`).
2. **Source observation:** DB row+event transactions end at the SQLite boundary.
   Notifications, terminal operations, Git files, project configuration and
   native history are external effects or separate stores
   (`packages/daemon/src/domain/event-bus.ts:52-91`;
   `packages/daemon/src/domain/rig-teardown.ts:103-143`).
3. **Inference:** a stopped rig can leave a repository checkout present while
   conversation is unresumable; a reported resumed conversation can still leave
   external worktree/index state unverified. This is separation of claims, not
   a finding that either outcome is runtime-preserved or demonstrated.
4. **Source observation:** absent/stale resume evidence should surface a
   decision or failure state rather than be described as a recovered
   conversation (`packages/daemon/src/domain/restore-orchestrator.ts:976-1006,1175-1237`).
5. **Unresolved:** who owns creation/deletion of project roots and Git worktrees,
   and whether the owner's stop-preservation requirement includes staged,
   unstaged, untracked, ignored and external-volume data, requires explicit
   workflow evidence and owner decisions. This note does not decide them.
6. **Test assertion only:** linked A5/A8/F9/S5 test citations were inspected,
   not run. Their assertions are not evidence that an invariant passed here.

## Trace cross-check and evidence boundaries

The five trace headings cover setup/start, launch, assign/follow, inspect, and
stop/resume (`breakdown/research-workflow-traces.md:20-95` at merge commit
`1568d6b5`). The state distinctions align at the source-path level: queue
assignment is not delivery (`breakdown/research-workflow-traces.md:56-68`); a
persisted session is not live-process proof (`:86-95`); and snapshot metadata
is not a project archive or native conversation (`:86-95`). **Inference:** those comparisons
are consistent with the inventory above; they do not prove source coverage is
complete or the described behavior occurs at runtime.

The accepted trace identifies dirty tracked/staged/untracked files, exceptional
cleanup, abrupt stop/reboot and actual resume as unrun gaps
(`breakdown/research-workflow-traces.md:93-95` at `1568d6b5`). The prior WT-8
prompt was the research task charter in the unaccepted draft (`8bd371a8`, line
99), not acceptance evidence. This document fills the source-only map for the
listed surfaces, but does not turn runtime gaps into guarantees.

| Evidence status | Finding | What it does and does not establish |
| --- | --- | --- |
| **Source observation** | Accepted trace is present at merge commit `1568d6b5911c5148e6c3e84715389f1009099c24` (PR #6); it expressly says no lifecycle was run and no gate is claimed (`breakdown/research-workflow-traces.md:5-12,115-118`). | Accepted source trace, not runtime or test proof. |
| **Inference** | Its file-preservation and conversation-resume paths are separate: the trace says project files and resumable conversation are different promises, while source allows `awaiting-decision` (`breakdown/research-workflow-traces.md:86-95`; `rig-teardown.ts:103-165`; `restore-orchestrator.ts:976-1006`). | Makes distinct promises explicit; proves neither runtime preservation nor successful conversation recovery. |
| **Unresolved** | Accepted trace still leaves dirty-worktree preservation and actual same-conversation restore unresolved (`breakdown/research-workflow-traces.md:93-95`). | Next evidence: separately authorized before/after file/index test and real native resume observation. |
| **Test assertion only** | Referenced tests were not run; accepted trace says no installation/lifecycle/restore workflow was run (`breakdown/research-workflow-traces.md:5-12,115-118`). | No pass, runtime behavior, user outcome, or gate completion is asserted. |

### Delegated deletion and ownership paths inspected

- **Source observation:** `rig-teardown.ts:85-100,115-165` has distinct already-stopped and live-session flows. Both clean selected managed guidance; service teardown is delegated; optional rig deletion follows separate logic. Snapshot failure is best-effort and does not stop teardown (`:103-113`).
- **Source observation:** service delegation honors persisted policy: `service-orchestrator.ts:136-163` calls Compose; `compose-services-adapter.ts:80-100` leaves services running, removes containers/networks for `down`, or adds `--volumes` for `down_and_volumes`. Thus volume destruction is a distinct configured deletion path; no file-preservation claim may ignore it.
- **Source observation:** the separate all-launch-failed path in `rigspec-instantiator.ts:144-180` attempts orphan-session kills, then `deleteRig`; this is not the normal teardown path and does not run its snapshot/guidance cleanup sequence.
- **Source observation:** regular guidance cleanup selects a runtime-specific file and calls `removeManagedBlocksFromFile` (`rig-teardown.ts:202-231`). That helper deletes the whole target if stripping recognized managed blocks leaves an empty string, otherwise writes the remaining user text (`managed-blocks.ts:88-116`). This is bounded behavior for those sentinels/selected path, not blanket user-file safety.
- **Source observation:** `rig destroy` delegates to `buildDestroyPlan` and `executeDestroy` (`packages/cli/src/commands/destroy.ts:70-121,193-207`; `packages/cli/src/destroy-helpers.ts:99-134,168-252`). Plan includes state root, external DB/WAL/SHM and transcript directory, with backup-by-rename or recursive removal. Execution stops daemon, checks listener, can terminate a remaining listener, optionally kills managed tmux sessions for `--all`, removes/renames targets, and recreates root. **Unresolved:** this is sequential best-effort, not an atomic snapshot; failed kill/target operation and all provider history/config/plugin paths require further targeted review.
- **Source observation:** workspace scaffold checks paths before writing and skips existing files, but this is an initialization rule, not protection against later writers/removers (`default-workspace-scaffold.ts:97-142`). Project catalog readers identify/select roots; no canonical catalog/project deletion authority is established (`project-read.ts:10-15,61-81`).
- **Unresolved:** exhaustive writer/deleter attribution for project trees, Git index/worktrees, native provider conversation stores, transcripts, adapter hooks/assets, user config, and daemon home/volume is not established by this map. “Not found in inspected flow” is not “cannot be deleted.”

## Completion boundary and next evidence

This source map has been cross-checked against accepted workflow trace PR #6
(`1568d6b5911c5148e6c3e84715389f1009099c24`) and linked A5/F4/S2 and A8/F9/S5
source-review findings. This is source-only coverage; no runtime preservation
or recovery is claimed. No Gate A–D status is changed.

Required evidence still open: exhaustive catalog, project, Git worktree/index,
transcript/provider-history, hook/config, destroy and daemon-volume
writer/remover paths; actual normal stop, failed
snapshot, crash and host interruption results; queue/event effects across
external boundaries; a real native resume; and separate before/after proof for
tracked edits, staged/index state, untracked and ignored files. These are not
established by source or inspected test assertions. No tests were run and no
Gate A–D status is changed.
