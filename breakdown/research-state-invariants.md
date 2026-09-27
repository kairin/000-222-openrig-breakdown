# State ownership, invariants, and recovery map

## Baseline, authority, and evidence rules

Inspected source and research baseline: **`ea7c268f576ada8434d3dae3e6ac972264910d4c`** (2026-09-27). Worktree: `/home/kkk/.cline/worktrees/c28a2/openrig-breakdown`. Only this document is owned by c28a2. This is source research, not implementation, an owner decision, independent review acceptance, or a Gate A–D result.

All numbered evidence links below resolve to exact paths and inclusive line ranges at that commit. Their link labels use repository-relative paths; the absolute inspected location is the worktree above plus that path. The previous claim baseline is `9db3ed6c406be5c3d9a84720383fcf6b543169e6`. `git diff` between that baseline and the inspected commit reports no changes under `packages` or `scripts`; older product-source citations can therefore be checked at this pin without silently changing their meaning.

Evidence labels:

- **O — observed-in-source:** executable code/schema implements the described mechanism; not a demonstrated outcome.
- **S — stated-intent:** documentation, comments, or test assertions describe a contract; not enforcement by themselves.
- **I — inferred:** a bounded implication of cited mechanisms; competing possibilities are stated.
- **U — unresolved:** missing or conflicting evidence; each U-ID below names the next evidence needed.

**Tests read are not tests passed.** No dependency installation, product test suite, daemon startup, agent launch, stop, resume, fault injection, or machine configuration was performed. Git/document checks are not runtime recovery tests.

### Prerequisite provenance

The integrated register records the linked A5/F4/S2, A8/F9/S5, F5/S3 conclusions and source-only limits [E01](#e01). Coordination identifies a1fa6's eight-page ownership and the c28a2 acceptance/dependency contract [E02](#e02). A read-only inspection of `/home/kkk/.cline/kanban/workspaces/openrig-breakdown/board.json` on 2026-09-27 additionally found a1fa6's coordinator approval naming integrated commits `3ce80cea2db3e1bcaaf546ede874f3a4b8e36c6f` and `273417729f59cebbebb1cb87c1cb9b859926aacb`. That mutable board testimony is supplementary, not a pinned source guarantee.

**Workflow integration evidence (rechecked):** live GitHub PR #6 is merged; its head artifact is `9c5a639eb7b7ec9445e742caf9ca55414308aa2d`, merged by `1568d6b5911c5148e6c3e84715389f1009099c24` at 2026-09-26T22:36:16Z. This corrects the earlier conclusion based only on divergent local `main`. The merged workflow artifact was read and cross-checked [E59](#e59). PR integration establishes an available integrated document, not that every assertion or card criterion passed; its review API reports a COMMENTED review, not an approval. The mutable board still lists a4d7b In Progress. **U0 — card acceptance authority:** next evidence is the coordinator's disposition reconciling that board task with the merged revision. This map does not infer Done from a merge.

## Reading the ownership map

“Authority” means which source answers a particular question, not a claim that one database owns the entire machine. SQLite answers logical topology/work history; tmux answers terminal presence; a native harness owns its conversation store; project bytes/index are separate filesystem/Git state. A cached or persisted observation can disagree with the thing observed.

### Filesystem and workspace state

| ID / state | Authority, identity, lifetime | Creates / reads / writes / deletes | Transaction, disagreement, recovery |
|---|---|---|---|
| F1 — tracked project edits | **I:** filesystem bytes at the resolved cwd are the saved-work authority; neither session status nor a snapshot is a byte backup. Cwd comes from override/member/spec-root resolution [E04](#e04). Lifetime is independent of a session, subject to actual writers/removers. | **O:** profile resolver selects cwd; instantiator stores it; launcher passes it to tmux [E04](#e04). Adapters write guidance/resources there [E05](#e05), [E06](#e06). FileWriteService reads existing bytes and replaces allowlisted files [E07](#e07). **I:** users/native agents are additional writers and deleters; their tool behavior is outside this source. | **O:** allowlisted edit compares mtime/hash, fsyncs a same-directory temp and renames it; audit append happens later and may report failure after the edit landed [E07](#e07). Not a global concurrent-writer lock or DB/filesystem transaction. Stop has managed-file writes (D1), not a general byte-preservation proof. U1/U2. |
| F2 — index / staged changes | **I:** Git index is distinct from working-tree bytes; a managed rewrite may alter a staged file's working copy without replacing its index entry. | **O:** skill projection reads Git tracking/ignore status and refuses to modify a tracked ignore file [E08](#e08). No index write/reset is present in the traced cwd→launch→stop chain [E04](#e04), [E09](#e09). **U:** external Git/native-agent index mutation and all custom actions are not bounded by that chain (U1). | **O:** snapshot payload contains metadata/checkpoints, not an index archive [E10](#e10). **I:** no index reset in these paths is narrower than “all staged work survives.” Next proof is per-index and staged-blob comparison, not only file hashes (U1). |
| F3 — untracked files | **I:** filesystem paths/bytes, not Git commits, are authoritative. A file being untracked is not an ownership marker. | **O:** projections/scaffolds can create untracked files; teardown can unlink a guidance file whose non-managed remainder is empty; package rollback can delete a new target [E05](#e05), [E09](#e09), [E11](#e11). Skill loadout records owned digests before later removal [E08](#e08). | **O:** managed ignore files can hide generated artifacts from ordinary status; they do not back them up [E08](#e08). No general untracked-file recovery is provided by snapshot [E10](#e10). User-created untracked data requires separate preservation evidence (U1/U2). |
| F4 — worktrees, nested repos, Git metadata | **O:** workspace resolver classifies declared repo paths/longest containing cwd; launcher runs `tmux new-session -c`, not `git worktree add` [E04](#e04). **I:** an existing user-created worktree can be selected by cwd, but that is not automatic isolation. | **I:** Git/operator/external tools own creation/removal of the selected checkout and index; the inspected launch/stop path has no allocator/remover for Git worktrees. OpenRig reads repo root/tracking for projection and writes bounded managed ignore files [E08](#e08). | **U:** no universal nested-repo/worktree deletion or isolation guarantee follows from the bounded search. Next evidence: selected startup actions, service mounts, native tool commands, linked-worktree metadata, and a disposable multi-worktree run (U1). Build-worktree documentation is not an agent-worktree lifecycle contract [E12](#e12). |
| F5 — declared workspace metadata | **O:** rig ID keys `workspace_json`; node cwd is stored separately. Repository setter can replace/clear workspace JSON; resolver derives active repo from override/default and containment [E04](#e04), [E13](#e13). | Instantiator/repository create/write; whoami/inventory resolver reads; explicit clear or rig deletion removes the declaration [E13](#e13), [E14](#e14). | Invalid workspace JSON reads as null; clearing metadata is not filesystem removal [E13](#e13). **I:** declared paths may be stale or unavailable. U1 requires actual-path/identity comparison; no automatic recreation of repository bytes is established. |

### Logical identities, live processes, and recovery material

| ID / state | Authority, identity, lifetime | Creates / reads / writes / deletes | Transaction, disagreement, recovery |
|---|---|---|---|
| L1 — SQLite database, rig and node records | **O:** file-backed SQLite uses WAL and foreign keys; rig/node opaque IDs differ from human names; node logical name is unique within a rig [E14](#e14). | Startup opens/migrates DB [E15](#e15); RigRepository creates rigs; instantiator creates nodes; lifecycle/restore read and mutate topology. Archive writes a timestamp and read filters hide it; delete removes rows [E13](#e13), [E14](#e14), [E16](#e16). | Archive is reversible metadata, not deletion. Rig deletion cascades to nodes/edges; node deletion cascades to bindings/sessions/checkpoints/tenures [E14](#e14), [E17](#e17). Events/snapshots/queue destinations have different retention. Destroy can remove DB/WAL/SHM themselves (D5). U3. |
| L2 — session registration and binding | **O:** session ULID, reusable session name, node ID, and unique node binding are distinct identities [E14](#e14), [E18](#e18). Rows outlive a terminal until explicit cascade. | Registry registers/adopts, updates status/startup status and binding; launcher, reconciler, restore and readers consume them. Stop clears binding and marks the selected session exited; node deletion cascades [E09](#e09), [E18](#e18), [E19](#e19). | Launcher creates tmux first, then commits session/binding/event together and notifies; DB failure attempts terminal cleanup [E19](#e19). Registry primitives are not each an event-emitting transaction. Running row ≠ harness ready. U3/U4. |
| L3 — occupant tenure / generation | **O:** generation UUID and ordinal identify an occupant tenure on stable node ID; native session ID is a separate pointer. Same native ID can reuse tenure; node deletion cascades [E17](#e17), [E18](#e18). | Registry mints/reads; handover invalidator retires name-keyed telemetry and generation-keyed obligations [E20](#e20). | Registration invokes best-effort tenure minting; do not infer universal atomic tenure creation from a session row. Retiring-generation claims return to pending; absent generation skips that invalidation rather than stealing successor work. U4. |
| L4 — tmux/harness and daemon live state | **O:** terminal session name/pane and OS process liveness are external to SQLite. Launcher creates terminal, adapter launches harness; stop kills terminal [E04](#e04), [E09](#e09), [E19](#e19). | tmux/native tool controls process execution; daemon probes/records observations. Daemon lifecycle singleton records boot epoch/heartbeat/clean stop; a later boot replaces the prior singleton values [E21](#e21). | **O:** daemon SIGINT/SIGTERM shutdown stops services/timers/listeners and records clean epoch; the listed phases do not invoke rig teardown [E21](#e21). Abrupt exit can miss clean mark. **I:** daemon exit, agent exit, terminal loss and host reboot are different failures, not interchangeable “stop.” U3/U4. |
| R1 — resume metadata | **O:** session row stores runtime-specific token/type/provenance/freshness, not the conversation bytes. Capture derives Claude sidecar ID or Codex PID-log thread ID; callers persist [E22](#e22). | Hooks/operator/adoption/scrape supply values through registry; snapshot/restore read; explicit clear resets metadata; cascade/destroy removes rows [E18](#e18), [E22](#e22). | Nonempty-token guard; ranked writes reject lower provenance when supplied. A negative/inconclusive probe marks a retained token stale rather than clearing it. A provenance-omitting call bypasses the rank comparison [E18](#e18). Periodic refresh is read-based/fill-null; teardown's default refresh can probe [E22](#e22). U5. |
| R2 — native conversation/session history | **O:** OpenRig reads provider JSONL paths and launches native resume commands; parser skips corrupt/non-text lines and returns empty for unreadable files [E23](#e23). **I:** native harness storage, not OpenRig snapshot, is authoritative for native continuity. | Claude/Codex are external creators/writers of native history; OpenRig adapters/recap readers consume references. Native retention/deletion policy is **U5**, not inferred from OpenRig teardown. | Token format/freshness, accessible native history, process launch and exact conversation identity are separate checks. Missing token can stop before launch; failed resume/absent adapter may compensate; checkpoint rebuild is not exact native resume [E24](#e24). U5/U6. |
| R3 — OpenRig terminal transcripts / capture caches | **O:** bounded trailing capture by session name, stored at launcher-selected transcript path; not a complete native transcript [E19](#e19), [E25](#e25). | Rotation reads pane, preserves boundary headers, overwrites via temp+rename; transcript consumers read; stop deletes timer/freshness/generation cache, not ordinary transcript bytes. Destroy targets transcript directory [E25](#e25), [E26](#e26). | Timers/cache die with daemon; generation guard suppresses stale in-flight writes. Capture errors are best-effort and next tick retries. Old scrollback is lost by overwrite, not archived. U5/U6. |
| R4 — checkpoints / continuity / startup metadata | **O:** checkpoint ULID linked to node; summary/task/artifact references, not arbitrary file contents. Store appends and reads latest; snapshot reads continuity/startup rows [E10](#e10), [E27](#e27). | CheckpointStore creates; recovery reads; node cascade removes live checkpoints; snapshot may retain copied summary [E17](#e17), [E27](#e27). Restore can write checkpoint guidance to cwd [E24](#e24). | Capture reads precede snapshot transaction. Checkpoint “rebuilt” status is different from resumed history. Complete continuity/startup writer and deletion coverage beyond the captured/restored fields is **U6**. |
| R5 — snapshots | **O:** snapshot ULID/rig ID/kind/JSON; no FK to rig, so rig deletion alone does not remove snapshot [E14](#e14). | SnapshotCapture creates from topology/session/checkpoint/continuity/startup/receipt; repository lists/selects; restore reads; explicit prune and periodic kind retention delete [E10](#e10), [E28](#e28). | Snapshot + event atomic, notification after commit; input reads not a quiesced external-state transaction. Selection skips corrupt/unusable candidates, ranks periodic/pre-down sources. No queue rows, project/index archive, native-history archive, memory or service-volume bytes in payload. U6. |

### Queue, history, event, and configuration state

| ID / state | Authority, identity, lifetime | Creates / reads / writes / deletes | Transaction, disagreement, recovery |
|---|---|---|---|
| Q1 — queue items and claims | **O:** qitem ID keys latest state; source/destination are text, not process FKs [E29](#e29). | QueueRepository creates/claims/updates/handoffs; route/agent/client read. Generation retirement releases claims, not destination/row [E20](#e20), [E30](#e30). No queue-item hard-delete in the inspected retention path; whole DB destroy remains destructive [E26](#e26), [E31](#e31). | Claim accepts pending/blocked for matching destination, writes in-progress/generation/transition/event together. Precondition reads precede transaction; not arbitrary multi-writer CAS. Explicit-ID create absorbs same source/destination conflict without a second nudge; different identity rejects. Not general request-body equivalence [E30](#e30). U7. |
| Q2 — transition history / archive / wake side table | **O:** transition ID orders audit history; archive retains original ID; wake evidence keyed separately by transition [E31](#e31), [E32](#e32). | Repository appends through QueueTransitionLog; log readers union active/archive; retention archive-inserts then deletes active rows per qitem; telemetry retention instead hard-deletes [E31](#e31). | A failed batch can leave earlier qitems archived, later ones unmoved; per-item move is atomic. Live-frontier references exclude archival. Archive is not loss of audit identity. No archival event is emitted by the inspected move. U7. |
| Q3 — outbox and terminal wake intents | **O:** outbox ID/audit pointer/destination identify delivery evidence, independent of recipient action [E33](#e33). | Handoff stages intent with close/successor/events; delivery claims pending→sending and finalizes delivered/failed/indeterminate; startup reconciles abandoned sending and drains pending [E33](#e33), [E34](#e34). No remover is present in these paths; destroy removes DB. | External pane/network write occurs after commit. Sending abandoned at crash becomes indeterminate, not blindly resent by this drain. Ordinary create uses best-effort nudge instead of this transaction. Separate scheduled wake ladder can retry eligible failed baton nudges (I5), so “no retries anywhere” is false [E35](#e35). U7. |
| E1 — durable events and in-memory subscribers | **O:** SQLite sequence keys append history; rig/node references intentionally lack FK; EventBus owns inserts, callers own enclosing state transaction [E14](#e14), [E36](#e36). | Domains persist; bus/SSE clients read; in-memory subscriptions register/unregister; event rows survive rig delete. No row-retention/deleter appears in inspected bus; DB destroy deletes storage [E26](#e26), [E36](#e36). | Standalone emit persists then notifies. Envelope validates exact registered tokens before commit, drains after; subscriber exceptions isolated. Poisoned drain row records diagnosis; replay parser can throw. Sequence is not recipient acknowledgement. U8. |
| E2 — client views / reconnect caches | **O:** UI EventSource hub keeps bounded in-memory replay and clears when idle; graph hook invalidates query on matching events/reconnection. TUI parser reconnects established streams but this helper does not track event-ID cursor [E37](#e37). | Daemon query/event producer supplies observations; client query cache/hub creates/writes/reads; unmount/idle/reset discards them. | SSE subscribes before replay, buffers and de-duplicates sequence during replay, accepts Last-Event-ID [E38](#e38). This does not establish every consumer's cursor or exactly-once view behavior. Re-query is distinct from native/process recovery. U8. |
| C1 — guidance blocks | **O:** selected Claude guidance file or Codex `AGENTS.md`; block ID identifies projected content, not exclusive ownership of entire cwd [E05](#e05), [E09](#e09). | Adapters merge/create; native harness reads (contract); teardown strips recognized managed blocks and deletes empty remainder [E09](#e09). | Cleanup strips all recognized OpenRig/legacy blocks in the selected file, not only a rig-specific block. Non-managed text is retained but whitespace normalized. No Git-tracking guard/DB transaction here. Shared-cwd interaction and exceptions: U2. |
| C2 — Claude hooks / Codex config | **O:** Claude project-local settings and relay asset; Codex-home TOML activity-hook blocks, trust keys, resource fragments and project trust [E05](#e05), [E06](#e06). | Adapters provision/reconcile; harness reads; disable removes owned entries/block, not the entire config. Codex trust-feature residue and copied Claude relay asset are not removed by the cited disable methods. | Claude malformed settings fails closed unchanged; missing enable source/manifest does no mutation. Codex fragment validates before write; workspace-trust read exception falls back to empty content before write—do not generalize Claude's safety to this path [E06](#e06). Multi-file writes not atomic. U2/U9. |
| C3 — skill projections / manifests / ignore files / package journal | **O:** source IDs/digests and ownership manifest govern skill targets; install journal records target/backup/hash separately [E08](#e08), [E11](#e11). | Catalog/adapter installs; harness consumes; catalog removes unchanged deselected targets, rejects modified ones. InstallEngine rollback restores backup or deletes target lacking backup [E11](#e11). | Skill staging renames and compensates best-effort; not a power-loss transaction. Package rollback does not compare current target with afterHash before restore/delete. User edit since install can therefore be overwritten (**I**, competing case: no intervening edit). U2. |
| C4 — settings, host setup, libraries | **O:** SettingsStore resolves file/environment/default, writes/read-verifies JSON, resets by key or unlinks whole file; malformed JSON errors [E39](#e39). Setup replaces/appends managed tmux block [E40](#e40). | Operator commands/store/setup create/read/write; reset removes settings; library mutation removes user spec/context sources or explicitly forced images [E41](#e41). | File writes not coupled to SQLite; environment overrides can disagree with stored preference. Setup is not reversed by ordinary rig stop. Library scan/cache reflects source removal, not conversation recovery. Uninstall completeness and configuration precedence at every consumer: U9. |
| C5 — service state and volumes | **O:** persisted compose file/project name/policy/receipt describe intended services; Docker owns actual containers/volumes [E42](#e42). | Service orchestrator reads spec/receipt, invokes adapter up/down/status, updates receipt; compose down may remove volumes by policy. | `leave_running` no-ops externally; successful teardown clears receipt even then. `down_and_volumes` invokes `--volumes`. Receipt/snapshot is not service-data backup. U10. |

## Invariant, transaction, and disagreement ledger

| ID | Source-enforced boundary / qualification | Failure/restart response and missing proof |
|---|---|---|
| I1 — registration is not liveness | **O:** session/binding/event launch transaction follows terminal creation [E19](#e19); reconciler probes then atomically detaches+records event, preserving node/queue [E43](#e43). | **O:** probe exceptions retain row and return errors. Reconciler treats any non-present classified result as detachable. A present tmux shell is not necessarily a ready AI. U3/U4. |
| I2 — absent, unavailable, and unknown are not handled uniformly | **O:** `probeSession` returns `transport_unavailable`; `hasSession` collapses it to false. Restore classifier calls `hasSession`, so false→stale; only thrown errors→unknown [E44](#e44), [E24](#e24). | **Contradiction:** available workflow Trace 2/4 and A5-linked prose can be read as all unavailable probes blocking restore. Executable behavior is narrower. **U4:** inspect actual missing-socket/permission-error results and review intended caller policy; do not silently “fix” source or claim pages. |
| I3 — stop is not one transaction | **O:** live-session pre-down refresh/capture is best-effort; terminal kills precede per-node cleanup; guidance/services follow; requested rig delete is blocked by kill failures, not snapshot failure [E09](#e09). | Managed cleanup can throw after a kill/DB update; no whole-operation rollback encloses it. Already-stopped branch cleans guidance/services and can delete without capture. **I:** process-local work can be interrupted even with intact files; competing case: already flushed work survives. U1/U3/U10. |
| I4 — resume reference is not recovered history | **O:** missing token+prior session can return awaiting-decision before launch; attention-required keeps a live gated process; confirmed failed/blank launch invokes compensation [E24](#e24). | `rollbackToZeroSession` ignores the returned kill result and catches thrown failures before restoring DB state [E45](#e45). **I:** “no session running” output can overstate termination if kill failed. Exact-resume replay containment avoids startup reinjection; rebuild explicitly writes a checkpoint [E24](#e24). U5/U6. |
| I5 — durable work, wake attempt, receipt, and action differ | **O:** local handoff closes to handed-off, creates pending successor, records transitions/intents/events atomically, then delivers [E34](#e34). Ordinary create notifies/nudges after commit [E30](#e30). | Startup outbox drain retries only pending, not terminal failed/indeterminate [E33](#e33). **O:** separate wake ladder uses durable row/transition markers and `maybeNudge` for eligible failed batons; unconfirmed outcomes have an escalation path [E35](#e35). Thus accepted F5/S3 is correct about the named drain, not an exhaustive no-retry policy. U7. |
| I6 — local atomicity does not cross hosts | **O:** remote successor identity is deterministic; source-close conflict checked; successor-create precedes local close [E46](#e46). | **I:** interruption between databases can leave both open; same-ID re-drive can converge but is not global atomicity or exactly-once action. Compare both rows and delivery evidence, not just local HTTP success. U7. |
| I7 — history and latest state are complementary | **O:** queue history unions active/archive; retention moves per item. Generation release updates rows+transitions without an EventBus call in that method [E20](#e20), [E31](#e31). | Do not assume every durable mutation produces live invalidation. Event subscribers can fail without rolling back committed state [E36](#e36); SSE replay is available but malformed replay can fail [E38](#e38). U7/U8. |
| I8 — snapshot survives rig deletion, not every deletion | **O:** no rig FK, but explicit/kind pruning and destroy remove snapshots [E14](#e14), [E26](#e26), [E28](#e28). | Input reads happen before snapshot write transaction. A valid snapshot may reference removed files/native history/services; it cannot restore their bytes. U6/U10. |
| I9 — managed is not universally user-safe | **O:** digest guarded skill removal differs from block stripping, direct adapter overwrites, and unconditional install rollback [E05](#e05), [E06](#e06), [E08](#e08), [E11](#e11). | **I:** safeguards in one subsystem cannot prove another preserves user edits. Concurrent edits and crash windows remain U2; document path-specific evidence rather than promising a universal rollback. |

## Cleanup and delegated-deletion map

These are source-path boundaries, not commands executed during this research. Every row distinguishes target and compensation; none establishes blanket work preservation.

| ID / entry or delegate | Target, guard, and retained state | Recovery / evidence |
|---|---|---|
| D1 — rig down / optional delete | Kill selected live terminals; exited rows/bindings; selected guidance rewritten/unlinked; services delegated. Delete cascades node-owned rows, not event/snapshot/queue text destinations. Snapshot warning does not veto stop [E09](#e09), [E14](#e14), [E17](#e17). | Restore uses retained topology/current DB/snapshot/native references, not project archive. Managed cleanup exceptions and service returned-failure handling remain U2/U10. |
| D2 — remove node / pod lifecycle | Active queue work blocks removal unless explicit fallback; fallback rerouting occurs before kill. Detached claimed session can be preserved. After kill, node+events/roster transaction; cascades [E47](#e47). | Failed kill can leave already-rerouted work; no shared external/DB transaction. Pod removal has additional scope (U3). |
| D3 — failed launch / failed restore | Launcher attempts terminal cleanup if DB commit fails; all-terminal instantiation kills collected sessions best-effort and deletes rig; prelaunch service failure deletes topology. Restore compensation restores prior bindings/status [E19](#e19), [E45](#e45), [E48](#e48). | These are not rollback of every previously projected file or external service. Kill result checking differs by path. U2/U3/U6. |
| D4 — configured service teardown / env down | Delegates to Docker compose; leave-running/down/down-and-volumes are different policies; volume removal explicitly requested in command [E42](#e42). | Receipt clear does not recover volume contents. **U10:** native Docker/mount semantics and route reporting need target-specific evidence. |
| D5 — explicit destroy | Plans state root plus external DB/WAL/SHM/transcript targets; stops daemon/checks local listener; only proceeds once port clears. All-scope attempts managed tmux kills. Backup renames targets; otherwise recursive remove; recreates empty root [E26](#e26). | No automatic rehydration of removed bytes. **I:** a project placed under a configured target can be removed too; competing case: project lies outside targets. No path-independent preservation claim. U1/U3. |
| D6 — retention/pruning | Snapshot repository prunes; periodic scheduler retains newest kind-scoped snapshots. Queue retention archives transitions, hard-deletes aged watchdog history/usage samples [E28](#e28), [E31](#e31). | Queue history reader includes archive; telemetry expiry is not authoritative-history preservation. Destroy still removes both. U6/U7. |
| D7 — install rollback / skill reconciliation | Journal rollback restores backup or deletes new file; catalog digest-checks deselected owned targets then stages/removes and best-effort reverses changes [E08](#e08), [E11](#e11). | Project bytes can change even though no Git reset/worktree removal occurs. Audit journal/files are not one transaction. U2. |
| D8 — hook disable / config reset / library remove | Claude owned commands removed; Codex sentinel stripped; settings reset may unlink entire managed settings file. Spec remove unlinks user file, rename writes new then unlinks old; context remove recursively deletes nonbuiltin source; image force can override reference protection [E05](#e05), [E06](#e06), [E39](#e39), [E41](#e41). | Source rescans update caches but do not restore deleted files. Disabled hook is not complete uninstall; image removal can invalidate recovery references. U5/U9. |
| D9 — occupant invalidation | Deletes retiring name-keyed sidecar; stops generation-scoped watchdog jobs; releases generation claims [E20](#e20), [E49](#e49). | Unknown generation skips scoped invalidation; freshness/session checks remain important. U4. |
| D10 — scaffold and scratch cleanup | Slice scaffolding failure recursively removes slice directory and restores parent content [E50](#e50). FileWriteService removes temporary files on write/rename failures [E07](#e07). | These paths are separate from rig stop. Other package/build/restore scratch operations and arbitrary startup actions are not assumed safe for arbitrary paths: U11. |

## Cross-check with workflow traces and accepted linked evidence

All rows below are **O** comparisons of documents/source, not renewed claim verdicts. The five-journey comparison was also repeated against the merged workflow revision [E59](#e59): its install/start gaps remain explicit; launch compensation and dirty-workspace effects remain unresolved; queue retry language still needs I5’s separate-ladder qualification; inspection adds TUI/browser/MCP consumers without proving reconnect; stop/resume retains the I2 transport-classification overstatement. These are source disagreements, not reasons to edit the other card’s artifact. The accepted a1fa6 pages were read as eight pages, not three independent guarantees; [E01](#e01) supplies their linked register conclusions and [E51](#e51)–[E58](#e58) their exact reviewed evidence sections.

| Existing evidence | State/effect cross-check | Qualification / next evidence |
|---|---|---|
| Workflow 1 — install/start [E03](#e03) | L1/L4/C4: startup migration, daemon epoch, managed host setup. | Setup affects files outside checkout; ordinary stop is not uninstall. U9. |
| Workflow 2 — launch agents [E03](#e03) | F1–F5/L1–L4/C1–C3: cwd resolution, projection, terminal then DB registration, failure compensation. | Shared cwd is not automatic worktree isolation. Classified-unavailable probe caveat I2 corrects the blanket unknown/unavailable reading. U1/U4. |
| Workflow 3 — assign/follow [E03](#e03) | Q1–Q3/I5/I6: latest row, audit transition, wake intent, cross-host partial completion. | Broaden retry coverage to separate ladder, without claiming recipient action. U7. |
| Workflow 4 — inspect [E03](#e03) | E1/E2/L4: durable observation versus terminal probe, UI invalidation, SSE/TUI reconnect. | Source callback/stream availability is not demonstrated UI convergence. U8. |
| Workflow 5 — stop/resume [E03](#e03) | D1–D9/R1–R5: snapshot warning, managed-file writes, volumes, explicit destruction, retention, conditional resume. | Saved project/index/untracked files, native conversation and process recovery have different guarantees. U1/U5/U6/U10. |
| A5/F4/S2 [E51](#e51), [E52](#e52), [E53](#e53) | Persistent logical identity and role destination survive missing terminal; generation claims can release; schema deletion boundary explicit. | Retention ≠ reachability; probe exception ≠ classified transport unavailable (I2). No change to accepted verdicts. |
| A8/F9/S5 [E54](#e54), [E55](#e55), [E56](#e56) | Capture payload excludes full filesystem/history/volumes; best-effort capture failure continues stop; restoration remains conditional. | Additional pruning/destroy/rollback and managed stripping paths narrow preservation claims. Compensation output needs kill-result qualification (I4). |
| F5/S3 [E57](#e57), [E58](#e58) | Local handoff/intents transactional; notification later; pending-only startup drain; remote split transaction. | The separate scheduled ladder is additional recovery, not a contradiction of the drain's selection SQL. Claims' broad retry/comment wording must remain scoped (I5). |

## Unresolved evidence and bounded next actions

Unresolved source coverage is explicit rather than hidden behind an assertion of completeness. Runtime demonstrations below are proposed future evidence, **not executed and not authorization to execute them**. Owner questions are not answered here.

| ID | Unresolved point | Next evidence needed / boundary |
|---|---|---|
| U0 | Merged workflow revision cross-checked, but board-card acceptance is not reconciled. | Coordinator confirms disposition of a4d7b against merged artifact `9c5a639eb7b7ec9445e742caf9ca55414308aa2d`, addressing I2/I5 qualifications. No shared edits or board mutation. |
| U1 | Universal preservation of tracked edits, index, untracked data, nested repos and linked worktree metadata; custom path overlap with destroy. | Audit selected startup actions/services/native tool commands and resolved destructive targets; separately authorized disposable run comparing tracked bytes, staged blobs/index, untracked bytes, nested repos, `.git` link/common-dir metadata before/after normal stop, failed snapshot, abrupt stop and resume. |
| U2 | Concurrent edits, shared-cwd block ownership, partial projection/uninstall and rollback crash windows. | Trace actual selected resource/install plan and all outer error handlers; fault-inject interruption and edit-after-install in disposable paths, inspect user text/digests/backups/manifest and emitted errors. Review path-specific expectations, not a new owner policy. |
| U3 | Full cascade/lifecycle surface beyond listed core tables; pod removal, process leftovers after failed cleanup, abrupt daemon death. | Follow later migration rebuilds and remaining lifecycle delegates for the selected rig; inspect process tree/DB/WAL and errors under kill/DB failure, daemon signal versus rig stop. Do not infer OS termination from DB compensation. |
| U4 | Probe policy discrepancy and occupant identity correctness across adoption/same-native relaunch/handover. | Compare probeSession versus hasSession on no socket, absent target and permission errors; inspect adapter-specific consumers, tenure reservation/mint failures and generation-stamped evidence across a disposable handover. Coordinator reviews contradiction I2. |
| U5 | Native history creation/flush/retention/deletion, fork/session-source storage and exact recovered identity. | Inspect supported native Claude/Codex version storage contracts plus OpenRig source-fork/image writers; authorized per-provider resume with original ID/history markers and missing/corrupt/stale token cases. A TUI/foreground-process heuristic alone is insufficient. |
| U6 | Complete continuity/startup metadata writer/remover chain, snapshot consistency and compensation success. | Follow checkpoint/continuity/startup routes and migrations; compare capture input time versus commit, referenced file availability and current-occupant selection; test failed rollback kill and checkpoint rebuild versus native resume separately. |
| U7 | Full queue transition/actor authorization surface, multi-writer/client retry races, scheduler interaction and recipient acceptance. | Inspect update/closure/park/claim routes and all timer/stuck-sweep/ladder wiring, including archive readers; fault-inject handoff commit/send/finalize and cross-host close interruption with stable IDs and two-DB evidence. Observe acknowledgement/action independently. |
| U8 | All consumer cursors, poison replay, disconnected UI/TUI/MCP convergence and event deletion outside mapped bus. | Trace each retained client's opener/cursor/query invalidation and event maintenance paths; reconnect with duplicate/missed/malformed rows and compare canonical queries to rendered views. Scope/retention choices remain owner questions. |
| U9 | Complete configuration uninstall and precedence at every consumer, backup/credentials/generated asset cleanup. | Trace settings routes, setup/update/uninstall scripts, profile/config projection and credential stores; compare owned/user configuration before/after disable/reset/uninstall in an authorized disposable home. No universal reversibility claim. |
| U10 | Service-data preservation and error reporting for returned `{ok:false}` versus thrown teardown error. | Trace service caller/route response propagation and actual compose mounts/volume ownership; authorized disposable-volume test for each policy, including snapshot failure and already-stopped branch. Metadata receipt is not byte backup. |
| U11 | Exhaustive auxiliary cleanup beyond core operator journeys (bundles, context Git imports, images, build/generated/scratch, custom agents). | Expand call-graph search from selected entrypoints into resolved deletion targets and external subprocess arguments; classify temp-only versus user/artifact deletion and validate recovery evidence. No search-negative proof of whole-repository safety. |

## Validation and acceptance disposition

### Decomposed completion pass

The user requested decomposition and Backlog placement, then continued investigation. The existing card was retained (no duplicate research cards). Its prompt now contains S1–S4; the supported Kanban `workspace.saveState` API with an expected revision moved c28a2 to Backlog, confirmed by `workspace.getState` at revision 309. This operational record is not source behavior and does not amend gate or owner decisions.

| Work package | Bounded closure condition | This pass |
|---|---|---|
| S1 — project and continuity ownership | Identify catalog authority, scaffold mutation, startup/continuity writers and cascade deletion; distinguish missing callers from proven runtime writes. | Additional source inspected; findings below, E60–E62. |
| S2 — fork and managed artifacts | Follow provider fork command, new token capture, image writer/remover, projection marker and CLI reset paths. | Additional source inspected; E63–E65. Native-provider storage durability remains external evidence, not a required runtime experiment here. |
| S3 — delegated cleanup | Follow pod removal and service error propagation; classify context/bundle/restore-packet cleanup targets. | Additional source inspected; E66–E68. Remaining auxiliary source paths are explicitly listed, not declared covered. |
| S4 — acceptance and handoff | Validate exact citations, row/link coverage, owned-file-only diff and commit; distinguish remaining source gaps from runtime proposals. | Checks run after edits. Done remains conditional on full source-path coverage, not just row counts. |

### Additional source findings from the completion pass

- **O — project catalog is a separate authority (F1/F5):** `workspace.yaml` maps project ID to root; duplicate IDs reject, selection resolves realpath, catalog/manifest disagreement and changed selected root produce named errors. The additive scaffold creates missing files/directories after collision checks, skips existing files, and has no filesystem transaction or failure rollback around the writes [E60](#e60). **I:** project identity can survive agent restart but a changed catalog/root needs reselection; it is not rig/node identity. No catalog deletion authority is established by these read/scaffold functions; U1/U11 retain the external-writer audit.
- **O — startup metadata writer and deletion (R4):** after pre-launch file delivery, StartupOrchestrator inserts/replaces node startup context unless preservation was requested; persistence failure reports failed startup. It stores projection/file/action descriptions, not original file bytes. The node-keyed schema cascades on node deletion. PodRepository upserts continuity status/artifact JSON under `(pod_id,node_id)`; either pod/node deletion cascades continuity, while node pod membership becomes null when only the pod row is removed [E61](#e61). A repository-wide source search for `createCheckpoint` and `updateContinuityState` found their definitions but no production call sites under `packages/daemon/src`; **U:** these methods alone do not prove an active checkpoint/continuity writer. Next evidence: dynamic callers or a configured integration invoking those methods. Snapshot/restore readers remain evidenced by E10/E24/E27.
- **O — pod removal is sequential (D2):** lifecycle shrink calls removeNode for each member; if a later member fails after prior removal, it returns a partial result with removed logical IDs. Only after the loop does a transaction persist `pod.deleted` and delete the pod, then notify [E62](#e62). Already removed members are not resurrected by a later failure. Raw PodRepository deletion is a narrower operation than this lifecycle workflow.
- **O — native fork delegates history copying (R2):** Claude adapter invokes native `--resume … --fork-session`, polls for a post-fork ID and errors if capture fails; Codex adapter invokes native `fork`, captures a thread ID and errors if absent. The returned token comes from capture, not a direct assignment of parent ID [E63](#e63). **I:** comments intend a new identity, but the cited branches do not themselves compare captured ID against parent; native tool/version correctness remains U5. Rebuild instead resolves existing artifact paths as fresh-start `send_text`, reports missing paths, and fails if none exist. It does not copy an entire conversation store [E63](#e63).
- **O — agent-image lifetime:** SnapshotCapturer derives source token/cwd, constructs a name/version manifest and calls the library installer with explicitly supplied files (empty map by default). Installer writes manifest/stats/optional files; consumption mutates stats and pin/unpin creates/deletes a sentinel [E64](#e64). Image prune/delete is already mapped in D8/E41; removing the image source may invalidate future forks but does not prove deletion of native provider history. These images are not full native conversation backups.
- **O — two additional managed-state owners (C3/C4):** ProjectionManifestStore upserts only the last hash/time/spec/category per target path and reads it for classification; its readability probe reports unavailable storage. It stores no original bytes. CLI ConfigStore independently writes/read-verifies its config and reset without a key unlinks the whole configured file [E65](#e65). These are not covered by merely naming the daemon SettingsStore. Whole-store destroy remains their ultimate deletion boundary; general uninstall completeness remains U9.
- **O — service failure propagation (D1/D4):** ServiceOrchestrator returns `{ok:false}` on compose failure. RigTeardown awaits it but does not inspect the returned result; only a thrown error adds a service warning on the live-session branch, and the already-stopped branch swallows thrown errors. Ordinary down route can therefore return 200 when service failure is represented only by the ignored result. In contrast, the explicit environment-down route checks `result.ok` and returns 500 on failure [E09](#e09), [E42](#e42), [E66](#e66). This source disagreement is resolved; U10 now concerns actual service/mount data and external failure outcomes, not uncertainty about these callers.
- **O — bounded auxiliary cleanup (D10):** composed context-pack creation records whether it created the target, deletes that directory after write failure, writes manifest last, and also deletes on failed post-write discoverability. Bundle source cleanup recursively removes its extraction directory best-effort. Restore-packet writer uses a sibling temporary directory, validates on-disk summary, renames to final target, and removes the temporary directory after failure [E67](#e67). These target classes are not synonymous with arbitrary project checkout deletion. Crash/race preservation remains unproven.
- **O — search boundary:** product searches for `DELETE FROM events`, `DELETE FROM queue_items`, `DELETE FROM outbox_entries` and `DROP TABLE events` found no matches in the searched production packages/scripts (test files excluded). This is a bounded lexical observation, **not** proof against dynamic SQL, whole-store destroy, operator deletion or external tools. A shell-deletion sweep also identified package-build, VM-bootstrap, smoke-install and testbed-image paths [E68](#e68). The inspected package build derives its target paths from the repository root and deletes CLI bundled daemon/UI/TUI plus the vendored daemon dependency. Smoke-install invokes that build and its exit trap removes its mktemp directory **and all CLI tarballs**; testbed build removes its mktemp context. The VM bootstrap match is printed reset instructions, **not an executed deletion in that function**. This distinguishes generated/scratch deletion from prose suggesting an operator deletion; other auxiliary callers remain U11.

### Remaining completion blockers after S1–S3

The new findings close the specific startup/continuity schema, pod-removal, native-fork command, image-write, CLI-reset and service-result propagation questions. They do not close every question in U1–U11. In particular, auxiliary shell cleanup and its configured target/caller coverage, all selected custom actions, and complete managed uninstall coverage still need source inspection. Native-provider flush/recovery and fault-injection evidence remain future runtime work; they are not a reason by themselves to fail this source-only card. Card acceptance reconciliation U0 is operational and separate from the already completed merged-workflow comparison.

The source map explicitly covers every requested state class and the normal/delegated cleanup paths identified above. It does **not** claim an exhaustive whole-product writer/deleter proof: U3/U5–U11 identify remaining breadth and runtime evidence. The merged workflow revision has now been cross-checked; **card acceptance authority remains U0**, distinct from that completed document comparison. Submit as source research with those limitations, not as a passed full acceptance checklist or a gate result.

Document checks: verify every pinned source link's blob exists and line range is in bounds; verify evidence reference definitions/usages; check required state/cleanup/invariant IDs and U0–U11 next-evidence rows; inspect citations for semantic support; run `git diff --check`; ensure only the owned Markdown output is staged/committed. No product tests are represented by these checks. Final command results and output commit are reported in the handoff rather than embedding a self-referential commit ID here.

## Pinned evidence index

Each E-ID below is the exact path/range evidence for the scoped claims above; multiple ranges under one ID deliberately expose the caller/delegate boundary.
### E01

- [breakdown/gemini-review/README.md:39–69](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/breakdown/gemini-review/README.md#L39-L69)

### E02

- [breakdown/11-coordination-outcomes.md:58–84](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/breakdown/11-coordination-outcomes.md#L58-L84)
- [breakdown/11-coordination-outcomes.md:154–191](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/breakdown/11-coordination-outcomes.md#L154-L191)

### E03

- [breakdown/research-workflow-traces.md:9–67](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/breakdown/research-workflow-traces.md#L9-L67)
- [breakdown/research-workflow-traces.md:84–108](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/breakdown/research-workflow-traces.md#L84-L108)

### E04

- [packages/daemon/src/domain/profile-resolver.ts:144–148](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/profile-resolver.ts#L144-L148)
- [packages/daemon/src/domain/rigspec-instantiator.ts:1406–1435](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/rigspec-instantiator.ts#L1406-L1435)
- [packages/daemon/src/domain/node-launcher.ts:125–142](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/node-launcher.ts#L125-L142)
- [packages/daemon/src/adapters/tmux.ts:331–342](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/adapters/tmux.ts#L331-L342)
- [packages/daemon/src/domain/workspace/workspace-resolver.ts:26–102](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/workspace/workspace-resolver.ts#L26-L102)

### E05

- [packages/daemon/src/domain/managed-blocks.ts:34–85](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/managed-blocks.ts#L34-L85)
- [packages/daemon/src/adapters/claude-code-adapter.ts:730–821](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/adapters/claude-code-adapter.ts#L730-L821)

### E06

- [packages/daemon/src/adapters/codex-runtime-adapter.ts:153–224](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/adapters/codex-runtime-adapter.ts#L153-L224)
- [packages/daemon/src/adapters/codex-runtime-adapter.ts:520–568](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/adapters/codex-runtime-adapter.ts#L520-L568)
- [packages/daemon/src/adapters/codex-runtime-adapter.ts:594–677](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/adapters/codex-runtime-adapter.ts#L594-L677)

### E07

- [packages/daemon/src/domain/files/file-write-service.ts:90–208](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/files/file-write-service.ts#L90-L208)

### E08

- [packages/daemon/src/domain/skill-catalog.ts:510–625](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/skill-catalog.ts#L510-L625)
- [packages/daemon/src/domain/skill-catalog.ts:778–860](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/skill-catalog.ts#L778-L860)
- [packages/daemon/src/domain/skill-catalog.ts:873–962](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/skill-catalog.ts#L873-L962)

### E09

- [packages/daemon/src/domain/rig-teardown.ts:73–198](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/rig-teardown.ts#L73-L198)
- [packages/daemon/src/domain/rig-teardown.ts:202–231](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/rig-teardown.ts#L202-L231)
- [packages/daemon/src/domain/managed-blocks.ts:88–116](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/managed-blocks.ts#L88-L116)

### E10

- [packages/daemon/src/domain/snapshot-capture.ts:60–166](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/snapshot-capture.ts#L60-L166)

### E11

- [packages/daemon/src/domain/install-engine.ts:46–170](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/install-engine.ts#L46-L170)

### E12

- [breakdown/gemini-review/claims/friction/F11-no-workspace-isolation/README.md:29–46](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/breakdown/gemini-review/claims/friction/F11-no-workspace-isolation/README.md#L29-L46)

### E13

- [packages/daemon/src/domain/rig-repository.ts:152–190](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/rig-repository.ts#L152-L190)
- [packages/daemon/src/domain/rig-repository.ts:554–579](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/rig-repository.ts#L554-L579)

### E14

- [packages/daemon/src/db/connection.ts:7–13](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/db/connection.ts#L7-L13)
- [packages/daemon/src/db/migrations/001_core_schema.ts:6–49](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/db/migrations/001_core_schema.ts#L6-L49)
- [packages/daemon/src/db/migrations/002_bindings_sessions.ts:6–28](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/db/migrations/002_bindings_sessions.ts#L6-L28)
- [packages/daemon/src/db/migrations/003_events.ts:6–21](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/db/migrations/003_events.ts#L6-L21)
- [packages/daemon/src/db/migrations/004_snapshots.ts:6–18](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/db/migrations/004_snapshots.ts#L6-L18)

### E15

- [packages/daemon/src/startup.ts:240–249](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/startup.ts#L240-L249)

### E16

- [packages/daemon/src/domain/rig-repository.ts:97–117](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/rig-repository.ts#L97-L117)
- [packages/daemon/src/domain/rig-repository.ts:554–579](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/rig-repository.ts#L554-L579)

### E17

- [packages/daemon/src/db/migrations/005_checkpoints.ts:6–21](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/db/migrations/005_checkpoints.ts#L6-L21)
- [packages/daemon/src/db/migrations/060_occupant_tenures.ts:3–31](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/db/migrations/060_occupant_tenures.ts#L3-L31)

### E18

- [packages/daemon/src/domain/session-registry.ts:89–128](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/session-registry.ts#L89-L128)
- [packages/daemon/src/domain/session-registry.ts:226–308](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/session-registry.ts#L226-L308)
- [packages/daemon/src/domain/session-registry.ts:311–392](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/session-registry.ts#L311-L392)
- [packages/daemon/src/domain/session-registry.ts:427–446](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/session-registry.ts#L427-L446)

### E19

- [packages/daemon/src/domain/node-launcher.ts:130–238](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/node-launcher.ts#L130-L238)

### E20

- [packages/daemon/src/domain/occupant-invalidator.ts:45–85](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/occupant-invalidator.ts#L45-L85)
- [packages/daemon/src/domain/queue-repository.ts:3424–3458](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/queue-repository.ts#L3424-L3458)

### E21

- [packages/daemon/src/domain/daemon-lifecycle-store.ts:19–70](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/daemon-lifecycle-store.ts#L19-L70)
- [packages/daemon/src/index.ts:345–403](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/index.ts#L345-L403)

### E22

- [packages/daemon/src/domain/resume-token-capture.ts:46–95](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/resume-token-capture.ts#L46-L95)
- [packages/daemon/src/domain/resume-metadata-refresher.ts:107–200](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/resume-metadata-refresher.ts#L107-L200)

### E23

- [packages/daemon/src/domain/session-jsonl.ts:33–77](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/session-jsonl.ts#L33-L77)
- [packages/daemon/src/domain/native-resume-probe.ts:28–71](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/native-resume-probe.ts#L28-L71)
- [packages/daemon/src/domain/native-resume-probe.ts:74–128](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/native-resume-probe.ts#L74-L128)

### E24

- [packages/daemon/src/domain/restore-orchestrator.ts:782–812](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/restore-orchestrator.ts#L782-L812)
- [packages/daemon/src/domain/restore-orchestrator.ts:976–1006](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/restore-orchestrator.ts#L976-L1006)
- [packages/daemon/src/domain/restore-orchestrator.ts:1175–1285](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/restore-orchestrator.ts#L1175-L1285)

### E25

- [packages/daemon/src/domain/transcript-rotation.ts:49–172](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/transcript-rotation.ts#L49-L172)

### E26

- [packages/cli/src/destroy-helpers.ts:125–174](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/src/destroy-helpers.ts#L125-L174)
- [packages/cli/src/destroy-helpers.ts:181–271](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/src/destroy-helpers.ts#L181-L271)
- [packages/cli/src/commands/destroy.ts:105–116](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/src/commands/destroy.ts#L105-L116)

### E27

- [packages/daemon/src/domain/checkpoint-store.ts:24–78](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/checkpoint-store.ts#L24-L78)

### E28

- [packages/daemon/src/domain/snapshot-repository.ts:21–38](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/snapshot-repository.ts#L21-L38)
- [packages/daemon/src/domain/snapshot-repository.ts:71–125](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/snapshot-repository.ts#L71-L125)
- [packages/daemon/src/domain/snapshot-repository.ts:157–209](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/snapshot-repository.ts#L157-L209)
- [packages/daemon/src/domain/periodic-snapshot-scheduler.ts:71–84](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/periodic-snapshot-scheduler.ts#L71-L84)

### E29

- [packages/daemon/src/db/migrations/024_queue_items.ts:3–51](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/db/migrations/024_queue_items.ts#L3-L51)

### E30

- [packages/daemon/src/domain/queue-repository.ts:1320–1358](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/queue-repository.ts#L1320-L1358)
- [packages/daemon/src/domain/queue-repository.ts:2038–2117](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/queue-repository.ts#L2038-L2117)

### E31

- [packages/daemon/src/domain/queue-retention.ts:125–217](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/queue-retention.ts#L125-L217)
- [packages/daemon/src/domain/queue-retention.ts:235–288](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/queue-retention.ts#L235-L288)
- [packages/daemon/src/domain/queue-transition-log.ts:145–206](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/queue-transition-log.ts#L145-L206)

### E32

- [packages/daemon/src/db/migrations/073_queue_transition_wakes.ts:3–20](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/db/migrations/073_queue_transition_wakes.ts#L3-L20)

### E33

- [packages/daemon/src/db/migrations/027_outbox_entries.ts:15–29](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/db/migrations/027_outbox_entries.ts#L15-L29)
- [packages/daemon/src/domain/outbox-handler.ts:122–168](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/outbox-handler.ts#L122-L168)
- [packages/daemon/src/domain/outbox-handler.ts:228–260](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/outbox-handler.ts#L228-L260)
- [packages/daemon/src/domain/queue-repository.ts:996–1069](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/queue-repository.ts#L996-L1069)
- [packages/daemon/src/domain/queue-repository.ts:1113–1162](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/queue-repository.ts#L1113-L1162)
- [packages/daemon/src/startup.ts:2179–2200](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/startup.ts#L2179-L2200)

### E34

- [packages/daemon/src/domain/queue-repository.ts:1507–1565](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/queue-repository.ts#L1507-L1565)
- [packages/daemon/src/domain/queue-repository.ts:1608–1669](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/queue-repository.ts#L1608-L1669)

### E35

- [packages/daemon/src/domain/queue-wake-ladder.ts:325–351](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/queue-wake-ladder.ts#L325-L351)
- [packages/daemon/src/domain/queue-wake-ladder.ts:527–558](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/queue-wake-ladder.ts#L527-L558)
- [packages/daemon/src/domain/queue-wake-ladder.ts:668–725](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/queue-wake-ladder.ts#L668-L725)
- [packages/daemon/src/index.ts:345–350](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/index.ts#L345-L350)

### E36

- [packages/daemon/src/domain/event-bus.ts:43–143](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/event-bus.ts#L43-L143)
- [packages/daemon/src/domain/event-bus.ts:161–240](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/event-bus.ts#L161-L240)
- [packages/daemon/src/domain/event-bus.ts:250–274](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/event-bus.ts#L250-L274)

### E37

- [packages/ui/src/lib/topology-events.ts:61–108](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/ui/src/lib/topology-events.ts#L61-L108)
- [packages/ui/src/hooks/useRigEvents.ts:22–68](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/ui/src/hooks/useRigEvents.ts#L22-L68)
- [packages/tui/src/live-events.ts:31–98](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/tui/src/live-events.ts#L31-L98)

### E38

- [packages/daemon/src/routes/events.ts:12–76](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/routes/events.ts#L12-L76)

### E39

- [packages/daemon/src/domain/user-settings/settings-store.ts:884–920](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/user-settings/settings-store.ts#L884-L920)
- [packages/daemon/src/domain/user-settings/settings-store.ts:990–1045](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/user-settings/settings-store.ts#L990-L1045)
- [packages/daemon/src/domain/user-settings/settings-store.ts:1088–1126](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/user-settings/settings-store.ts#L1088-L1126)
- [packages/daemon/src/domain/user-settings/settings-store.ts:1137–1150](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/user-settings/settings-store.ts#L1137-L1150)

### E40

- [packages/cli/src/commands/setup.ts:586–624](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/src/commands/setup.ts#L586-L624)

### E41

- [packages/daemon/src/domain/spec-library-service.ts:205–216](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/spec-library-service.ts#L205-L216)
- [packages/daemon/src/domain/spec-library-service.ts:243–264](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/spec-library-service.ts#L243-L264)
- [packages/daemon/src/domain/context-packs/context-pack-library-service.ts:175–201](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/context-packs/context-pack-library-service.ts#L175-L201)
- [packages/daemon/src/routes/agent-images.ts:325–341](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/routes/agent-images.ts#L325-L341)
- [packages/daemon/src/routes/agent-images.ts:425–442](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/routes/agent-images.ts#L425-L442)

### E42

- [packages/daemon/src/domain/service-orchestrator.ts:140–175](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/service-orchestrator.ts#L140-L175)
- [packages/daemon/src/adapters/compose-services-adapter.ts:81–110](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/adapters/compose-services-adapter.ts#L81-L110)

### E43

- [packages/daemon/src/domain/reconciler.ts:44–96](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/reconciler.ts#L44-L96)

### E44

- [packages/daemon/src/adapters/tmux.ts:292–328](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/adapters/tmux.ts#L292-L328)

### E45

- [packages/daemon/src/domain/restore-orchestrator.ts:1110–1129](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/restore-orchestrator.ts#L1110-L1129)

### E46

- [packages/daemon/src/routes/queue.ts:309–380](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/routes/queue.ts#L309-L380)

### E47

- [packages/daemon/src/domain/rig-lifecycle-service.ts:390–455](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/rig-lifecycle-service.ts#L390-L455)

### E48

- [packages/daemon/src/domain/rigspec-instantiator.ts:1352–1362](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/rigspec-instantiator.ts#L1352-L1362)
- [packages/daemon/src/domain/rigspec-instantiator.ts:1515–1540](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/rigspec-instantiator.ts#L1515-L1540)

### E49

- [packages/daemon/src/domain/context-usage-store.ts:96–144](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/context-usage-store.ts#L96-L144)

### E50

- [packages/cli/src/commands/scope.ts:375–406](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/src/commands/scope.ts#L375-L406)

### E51

- [breakdown/gemini-review/claims/architecture/A5-seat-outlives-process/README.md:26–45](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/breakdown/gemini-review/claims/architecture/A5-seat-outlives-process/README.md#L26-L45)

### E52

- [breakdown/gemini-review/claims/friction/F4-seat-vs-process/README.md:26–46](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/breakdown/gemini-review/claims/friction/F4-seat-vs-process/README.md#L26-L46)

### E53

- [breakdown/gemini-review/claims/strengths/S2-durable-seats/README.md:24–44](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/breakdown/gemini-review/claims/strengths/S2-durable-seats/README.md#L24-L44)

### E54

- [breakdown/gemini-review/claims/architecture/A8-snapshot-restore/README.md:23–42](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/breakdown/gemini-review/claims/architecture/A8-snapshot-restore/README.md#L23-L42)

### E55

- [breakdown/gemini-review/claims/friction/F9-teardown-despite-failed-snapshot/README.md:26–48](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/breakdown/gemini-review/claims/friction/F9-teardown-despite-failed-snapshot/README.md#L26-L48)

### E56

- [breakdown/gemini-review/claims/strengths/S5-fleet-restore/README.md:24–42](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/breakdown/gemini-review/claims/strengths/S5-fleet-restore/README.md#L24-L42)

### E57

- [breakdown/gemini-review/claims/friction/F5-process-overhead/README.md:34–58](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/breakdown/gemini-review/claims/friction/F5-process-overhead/README.md#L34-L58)

### E58

- [breakdown/gemini-review/claims/strengths/S3-ownership-prevents-drift/README.md:26–50](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/breakdown/gemini-review/claims/strengths/S3-ownership-prevents-drift/README.md#L26-L50)

### E59

- [breakdown/research-workflow-traces.md:20–115](https://github.com/kairin/openrig-breakdown/blob/9c5a639eb7b7ec9445e742caf9ca55414308aa2d/breakdown/research-workflow-traces.md#L20-L115) — separately pinned integrated workflow revision; product citations inside retain their own baseline.
- Operational provenance: `gh pr view 6 --json url,state,mergedAt,mergeCommit,headRefOid,reviews` returned MERGED with the head/merge/time recorded above. Live API testimony is not a source-code citation or a task acceptance verdict.

### E60

- [packages/daemon/src/domain/workspace/project-read.ts:10–84](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/workspace/project-read.ts#L10-L84)
- [packages/daemon/src/domain/workspace/project-catalog.ts:15–36](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/workspace/project-catalog.ts#L15-L36)
- [packages/daemon/src/domain/workspace/default-workspace-scaffold.ts:97–152](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/workspace/default-workspace-scaffold.ts#L97-L152)

### E61

- [packages/daemon/src/domain/startup-orchestrator.ts:195–224](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/startup-orchestrator.ts#L195-L224)
- [packages/daemon/src/domain/pod-repository.ts:80–111](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/pod-repository.ts#L80-L111)
- [packages/daemon/src/db/migrations/014_agentspec_reboot.ts:6–46](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/db/migrations/014_agentspec_reboot.ts#L6-L46)
- [packages/daemon/src/db/migrations/015_startup_context.ts:6–14](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/db/migrations/015_startup_context.ts#L6-L14)

### E62

- [packages/daemon/src/domain/rig-lifecycle-service.ts:540–573](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/rig-lifecycle-service.ts#L540-L573)
- [packages/daemon/src/domain/rig-lifecycle-service.ts:602–632](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/rig-lifecycle-service.ts#L602-L632)

### E63

- [packages/daemon/src/adapters/claude-code-adapter.ts:245–279](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/adapters/claude-code-adapter.ts#L245-L279)
- [packages/daemon/src/adapters/codex-runtime-adapter.ts:345–380](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/adapters/codex-runtime-adapter.ts#L345-L380)
- [packages/daemon/src/domain/session-source-rebuild-resolver.ts:54–88](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/session-source-rebuild-resolver.ts#L54-L88)

### E64

- [packages/daemon/src/domain/agent-images/snapshot-capturer.ts:61–104](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/agent-images/snapshot-capturer.ts#L61-L104)
- [packages/daemon/src/domain/agent-images/agent-image-library-service.ts:149–210](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/agent-images/agent-image-library-service.ts#L149-L210)
- [packages/daemon/src/domain/agent-images/agent-image-library-service.ts:306–372](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/agent-images/agent-image-library-service.ts#L306-L372)

### E65

- [packages/daemon/src/domain/projection-manifest-store.ts:19–82](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/projection-manifest-store.ts#L19-L82)
- [packages/cli/src/config-store.ts:1184–1269](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/src/config-store.ts#L1184-L1269)

### E66

- [packages/daemon/src/routes/down.ts:34–60](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/routes/down.ts#L34-L60)
- [packages/daemon/src/routes/env.ts:105–130](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/routes/env.ts#L105-L130)

### E67

- [packages/daemon/src/domain/context-packs/context-pack-library-service.ts:359–399](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/context-packs/context-pack-library-service.ts#L359-L399)
- [packages/daemon/src/domain/bundle-source-resolver.ts:99–113](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/bundle-source-resolver.ts#L99-L113)
- [packages/daemon/src/domain/bundle-source-resolver.ts:182–195](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/daemon/src/domain/bundle-source-resolver.ts#L182-L195)
- [packages/cli/src/restore-packet/packet-writer.ts:260–300](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/packages/cli/src/restore-packet/packet-writer.ts#L260-L300)

### E68

- [scripts/build-package.sh:4–21](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/scripts/build-package.sh#L4-L21)
- [scripts/build-package.sh:140–152](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/scripts/build-package.sh#L140-L152)
- [scripts/vm-bootstrap/two-daemon-start.sh:39–51](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/scripts/vm-bootstrap/two-daemon-start.sh#L39-L51)
- [scripts/smoke-fresh-install.sh:16–37](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/scripts/smoke-fresh-install.sh#L16-L37)
- [scripts/build-testbed-image.sh:32–40](https://github.com/kairin/openrig-breakdown/blob/ea7c268f576ada8434d3dae3e6ac972264910d4c/scripts/build-testbed-image.sh#L32-L40)
