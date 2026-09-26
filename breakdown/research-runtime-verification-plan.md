# Runtime verification research protocol

## Status and authority

Date: 2026-09-27. Source baseline: `aa51d3581825ce286d9dc2b379e21bee92a2a1c6`.
This is the independent research replacement for 6f8ea. It defines future
experiments, not product changes or permission to run them. **Every runtime
result is NOT RUN.** No installation, agent, queue, recovery, update or uninstall
workflow was executed for this document. No runtime observation was collected.
No implementation, compatibility promise, retained-scope decision, or Gate A–D
pass is claimed. Documentation review and Git publication are not runtime proof.

The protocol is complete without runtime authorization. The checklist below
governs only later execution. Missing credentials, artifacts, scope decisions,
or draft acceptance do not block writing or reviewing this protocol. They must
not be filled with assumed results. Product execution requires separate approval;
approval to commit, publish or merge this document does not grant it.

Evidence labels:

- **Observed in source:** a mechanism in the cited source, not demonstrated behavior.
- **Stated intent:** a requirement or documented procedure, not a measured outcome.
- **Test assertion only:** inspected test code; execution result **NOT RUN**.
- **Inference:** a consequence to investigate, not a guarantee.
- **Unresolved:** evidence or policy is absent. The runtime result remains **NOT RUN**.

## Inputs and provenance

Absolute paths identify the inspected files. Source references S1–S12 and test
references T1–T4 are pinned to the baseline above; their line ranges are not
references to a future moving branch. In another checkout, map the suffix after
`openrig-breakdown/` to the same file at that commit. Draft paths identify the
actual sibling-worktree documents read, not integrated or accepted outputs.

| Input | Use and limit |
|---|---|
| `/home/kkk/.cline/worktrees/d1795/openrig-breakdown/breakdown/10-rust-and-node-removal-plan.md:22-64,150-171` | Owner destination and separate runtime/tooling/external-tool boundaries. Its historical citations retain their own `9db3ed6` baseline. Not migration approval. |
| `/home/kkk/.cline/worktrees/d1795/openrig-breakdown/breakdown/09-adversarial-review-and-research-charter.md:40-95` | Five journeys, failure cases, evidence discipline and separate state contracts. |
| `/home/kkk/.cline/worktrees/a4d7b/openrig-breakdown/breakdown/research-workflow-traces.md:9-108` | Available provisional workflow draft: install, launch, assignment, inspection, recovery and gaps. Cites `9db3ed6`; not treated as accepted or runtime proof. |
| `/home/kkk/.cline/worktrees/c28a2/openrig-breakdown/breakdown/research-state-invariants.md:28-95,142-157` | Available provisional state ledger: Git, SQLite, native history and managed files have different owners. Cites `9db3ed6`; review dependencies do not block this independent protocol. |

### Source-derived expectations

| ID | Citation | Bounded expectation |
|---|---|---|
| S1 | `/home/kkk/.cline/worktrees/d1795/openrig-breakdown/scripts/smoke-fresh-install.sh:19-78` | **Observed in source:** builds in the checkout, packs/installs with npm, starts via Node, polls health, and removes checkout tarballs during cleanup. Do not run it in the research checkout. Its comments are not proof of isolation. |
| S2 | `/home/kkk/.cline/worktrees/d1795/openrig-breakdown/packages/daemon/src/openrig-compat.ts:7-18,26-75` | **Observed in source:** home and legacy environment/path fallback exist. Instance-home override alone does not isolate all user files. |
| S3 | `/home/kkk/.cline/worktrees/d1795/openrig-breakdown/packages/daemon/src/index.ts:236-298` | **Observed in source:** explicit DB/port settings, instance initialization, separate routing and bind variables, default loopback plus available Tailscale interface, and bind authentication checks. Initialization occurs before the shown bind-auth check; no-write-on-auth-failure is not inferred. |
| S4 | `/home/kkk/.cline/worktrees/d1795/openrig-breakdown/packages/daemon/src/domain/rig-teardown.ts:103-193,202-231` | **Observed in source:** snapshot failure is reported but teardown continues; successful kills clear session/binding state; kill failures block requested rig deletion. Managed guidance cleanup can write/delete files. |
| S5 | `/home/kkk/.cline/worktrees/d1795/openrig-breakdown/packages/daemon/src/domain/snapshot-capture.ts:125-166` | **Observed in source:** structured topology/continuity data and snapshot event commit together. No Git index/file archive appears in this payload. This is not a complete audit of every file writer. |
| S6 | `/home/kkk/.cline/worktrees/d1795/openrig-breakdown/packages/daemon/src/domain/queue-repository.ts:807-846,1114-1162` | **Observed in source:** terminal wake intent is staged inside the closure transaction; pane delivery is external. Recovery retries pending intents, not terminal failed/indeterminate ones; abandoned sending intents are reconciled separately. This is not general HTTP-create idempotency. |
| S7 | `/home/kkk/.cline/worktrees/d1795/openrig-breakdown/packages/ui/src/hooks/useRigEvents.ts:22-67` | **Observed in source:** graph query invalidation follows reconnect after an error, with debounce and subscription cleanup. Not proof that every UI query refreshes. |
| S8 | `/home/kkk/.cline/worktrees/d1795/openrig-breakdown/packages/ui/src/lib/topology-events.ts:16-108` | **Observed in source:** one EventSource hub keeps a bounded local replay cache. This is not proof of server-side gap replay. |
| S9 | `/home/kkk/.cline/worktrees/d1795/openrig-breakdown/packages/tui/src/live-events.ts:1-99` | **Observed in source:** an unavailable initial stream disables this leg; an established stream drop schedules reconnect. Test these separately; do not assume unlimited retry or identical browser behavior. |
| S10 | `/home/kkk/.cline/worktrees/d1795/openrig-breakdown/packages/daemon/src/domain/restore-orchestrator.ts:976-1006` | **Observed in source:** a prior resumable session without a token returns awaiting-decision unless fresh start is explicit. No new native conversation is proof of the old conversation resuming. |
| S11 | `/home/kkk/.cline/worktrees/d1795/openrig-breakdown/packages/daemon/specs/agents/shared/skills/core/openrig-upgrade/SKILL.md:230-266` | **Stated intent:** agent-led upgrade/rollback verifies runtime, listeners, DB, plugins and seats; old runtime is preferred with a still-valid DB. This is not evidence of an automatic updater or universal downgrade compatibility. |
| S12 | `/home/kkk/.cline/worktrees/d1795/openrig-breakdown/package.json:7-32` | **Observed in source:** four npm workspaces and required Node build/test/generation scripts. Current baseline is not Node-free. |

## Future execution authorization and environment checklist

All boxes are unfulfilled execution prerequisites, not outstanding document work.
The later operator must record values and evidence, not simply mark “approved.”

- [ ] Record authorizer, operator, approval date/expiry, scenario IDs, exact artifact
  versions and permitted commands. Explicitly authorize install/update/uninstall,
  fault injection, process signals, guest reboot and any native agent calls.
- [ ] Record OS, architecture, kernel, filesystem, VM/container image digest,
  runtime/tool versions, Git commit, artifact SHA-256 and schema versions. Use a
  VM for reboot/native-terminal tests; use a container only after proving its
  process, network, home and terminal-provider boundaries.
- [ ] Allocate one disposable guest and unique run ID. Do not mount the user's
  project, home, Docker socket, SSH agent, terminal socket, or live DB. No writable
  mounts outside the fixture. Do not copy the sole live database or credentials.
- [ ] Define absolute guest root `/var/tmp/openrig-runtime-<run-id>` and dedicated
  `home`, `instance`, `state`, `project`, `install`, `artifacts`, `tmp`, `evidence`
  children. Resolve real paths and reject symlinks escaping this root. Store a
  run-ownership marker before any cleanup is permitted.
- [ ] Start from a cleared environment. Set `HOME`, `TMPDIR`, `XDG_CONFIG_HOME`,
  `XDG_DATA_HOME`, `XDG_CACHE_HOME`, and `XDG_STATE_HOME` to fixture paths. Set
  `OPENRIG_HOME` to `instance`, `OPENRIG_DB` to `state/state.db`, `OPENRIG_PORT`
  to the allocated port, and `OPENRIG_BIND_HOST=127.0.0.1`. Clear all inherited
  `OPENRIG_*` and `RIGGED_*` first; add only these reviewed values and any explicit
  scenario-specific client routing/auth values. No inherited fleet/seat identity.
- [ ] Verify CLI routing/config precedence from the tested version and pin its
  client endpoint to that guest loopback port. Record resolved workspace, context,
  skills and topology roots. Do not assume bind host sets client routing (S3).
  Isolate legacy `.rigged`, native agent config/history and tmux configuration too.
- [ ] Use a guest-only terminal provider. Record its socket, process namespace and
  fixture session names. Prove it cannot enumerate or kill host sessions. Never
  use a shared tmux server or host-wide `kill-server`/process-name kill.
- [ ] Reserve a free guest port and independently record listener ownership.
  Deny ingress and deny egress by default. Disable guest Tailscale exposure.
  Public-bind negative tests stay inside an isolated network namespace with no
  route outside it; never expose a real unauthenticated service.
- [ ] Record an allowlist for package fetches and native-agent endpoints. Use
  dedicated test credentials with spend/time limits and no production access.
  Record secret handling/redaction; do not persist raw bearer tokens, API keys or
  private conversation data in evidence. Use fixture-only daemon/terminal tokens.
- [ ] Verify observer tools before starting: process/exec trace, listener listing,
  file hashing, Git inspection, DB consistent-copy inspection, timestamps and
  guest-local network fault control. Trace short-lived subprocesses, not just `ps`.
- [ ] Record timeout/retry limits before execution: default daemon readiness 30 s,
  agent readiness 120 s, reconnect observation 90 s, shutdown 30 s, one deliberate
  HTTP retry per retry case. Deviations require a recorded reason before the run.
  A timeout is not success; preserve evidence and stop the case.
- [ ] Approve cleanup scope and evidence retention. Abort on any host path, live
  endpoint, unexpected credential, ownership ambiguity, uncontrolled spend or
  out-of-fixture write. Freeze evidence; do not repair user/live data.

## Fixtures and common measurement procedure

Use a fresh fixture per case. Baseline and candidate runs must not share writable
state. Copy only synthetic stopped-state fixtures or consistent backups made
inside the guest; SQLite WAL/SHM files must not be ignored by a naive live copy.

| Fixture | Required content |
|---|---|
| F1 clean distribution | Immutable local release archive, checksum, empty private install prefix, no monorepo dependency resolution, no pre-existing instance. Build a baseline archive only in a disposable source copy. |
| F2 Git preservation | Local-only repository, no remotes/hooks, initial commit; tracked unstaged edit; staged edit; one file with staged version A and unstaged version B; staged add/delete; untracked text/binary files; ignored sentinel. Add nested repo and linked worktree as separately recorded variants. |
| F3 state/ownership | Synthetic rig with two seats, unique queue bodies/IDs, events, snapshots and resume records. User-authored hook/config/guidance sentinels next to managed content. A third unrelated fixture seat is the cleanup control. |
| F4 faults | Guest-only listener occupying the target port; missing-tool PATH variants; invalid/absent dedicated credentials; malformed configuration; controlled projection permission failure; local proxy for request/response/stream interruption. |
| F5 native continuity | Separate Claude and Codex test sessions. Record native session ID and a random conversation-only marker not written into project files or supplied in the resume prompt. No real user history. |
| F6 update pair | Known baseline A and candidate B, immutable archives/checksums, documented schema compatibility, synthetic A-state backup plus user-edited managed-file variants. |

Before each run, save: sanitized command/environment, artifact and tool versions,
resolved roots, PID/executable/start-time tree, listeners, database schema and
relevant rows, file SHA-256/size/mode/symlink target manifest, Git HEAD/branch,
`git status --porcelain=v2 -z`, `git ls-files --stage -z`, staged/unstaged binary
diffs, untracked/ignored inventory, and worktree registration. Capture index bytes
as supporting evidence; preservation verdicts compare staged entries/blobs and
working bytes, not index stat-cache bytes alone.

Run one normal case, then one fault at a time from a fresh copy. Record the exact
fault boundary and timing. Capture stdout/stderr, exit code, HTTP response,
request correlation IDs, event/transition sequence, actual external delivery,
process/file/network trace and human-visible output. After a bounded wait, repeat
the baseline observations and produce a delta. A process, DB row, pane message,
agent acknowledgment and completed work are five different observations.

Use two independent verdict fields in future evidence: **source-conformance**
and **safety/desired contract**. Reproducing best-effort destructive behavior can
conform to source while failing safety. Neither is a research-gate decision.
If a fault did not reach its intended seam, classify the experiment inconclusive,
not passed. This document's result fields remain **NOT RUN** until actual evidence
is collected in a separately authorized run record.

## Protocols

### RV-01 — Clean installation and start

**Runtime result: NOT RUN.** Inputs: S1–S3, S12; fixtures F1/F3.

1. Inside a fresh guest, install the checksummed release into F1's private prefix.
   Capture install scripts, dependency resolution, file writes and network access.
2. Start with explicit isolated home/DB/port and loopback binding. Poll readiness
   for at most 30 s and tie the health response to the expected executable/PID,
   listener, instance and schema. Inspect status through the installed client.
3. Stop, verify process/listener release, and start once again on the same fixture.

Pass criteria: install resolves without monorepo leakage; the correct daemon is
ready in bounds; state is inside the fixture; restart uses that state; no orphan
owned process/listener remains after stop. Fail on outside writes, wrong listener,
false health attribution, unresolved dependencies or unexplained state loss.
Cleanup: common cleanup below, including private prefix. S1 is a reference only;
never run its build or tarball cleanup against this research checkout.

### RV-02 — Prerequisite, authentication, configuration and listener failures

**Runtime result: NOT RUN.** Inputs: S1–S3; fixtures F1/F4.

Start from the successful control fixture separately for each case: remove one
required tool from PATH; use absent/invalid agent auth; use invalid daemon client
auth; request a disallowed bind without its required token; supply malformed
configuration; occupy the port with an unrelated server; terminate startup before
readiness. Keep all bind tests guest-local. Record the exact command/version;
preflight/help/dry-run commands are also product commands, not executed here.

Observe exit/HTTP status, bounded prompts/timeouts, diagnostic text, process tree,
listener owner and all partial files/DB rows. Restore only that fault and retry
once. Pass criteria: no false readiness or unrelated-process adoption/termination;
no auth bypass or secret disclosure; errors identify the failed layer; retry has
no unexplained duplicate process/state. Partial writes must be classified rather
than assumed absent (S3). Source behavior for each diagnostic remains unresolved
until its exact handler is traced. Cleanup: stop only the fixture-owned collision
server, remove fault config/credentials and perform common cleanup.

### RV-03 — Agent launch and partial cleanup

**Runtime result: NOT RUN.** Inputs: S4, T1; fixtures F2/F3/F4/F5.

Launch one Claude and one Codex seat independently, then the pair. Record cwd,
native executable, projection writes, session/binding identity and readiness.
Repeat with projection denied before launch, missing harness, exit before ready,
and one member failing after its sibling is ready. Retry only the failed member
after removing the fault. Exercise a failed process-kill response separately.

Observe what was created before failure and whether cleanup owns it. Pass criteria:
no false native readiness, no duplicate active occupant, unrelated/sibling state
unchanged, user-owned files preserved, and every residual owned process/artifact
either removed or explicitly reported recoverable. Do not infer native behavior
from T1's mocked Pi adapter. Kill failure must not be reported as successful
deletion (S4). Cleanup: inventory and terminate only known fixture descendants;
compare config/guidance sentinels before removing the guest.

### RV-04 — Queue retry, disconnect and wake recovery

**Runtime result: NOT RUN.** Inputs: S6, T3; fixtures F3/F4.

Use separate cases for ordinary create, claim/update and terminal handoff. With a
guest proxy, drop a request before forwarding, then drop a response after server
commit. Query durable state before one deliberate retry of the same input. Record
whether the API actually supports a request-id/idempotency contract; do not invent
one. Exercise stale owner, concurrent claimant, notification failure and disconnect
after wake delivery. Restart with synthetic pending, sending, failed and
indeterminate wake cases, preserving their provenance.

Observe item IDs, owner/generation, transitions, events, intent states, delivery
count and recipient acknowledgment separately. Pass criteria for terminal closure:
all transaction-owned records or none; no duplicate committed closure; pending
recovery is bounded; failed/indeterminate wakes are not silently re-sent (S6).
For ambiguous ordinary retries, require visible reconciliation instructions; lack
of deduplication is a documented contract gap, not an invented exactly-once pass.
Fail safety on silent loss, false delivery/completion or unauthorized ownership.
Cleanup: export queue/history evidence, remove proxy faults, then dispose of the
entire synthetic instance; never close or mutate real queued work.

### RV-05 — Client reconnect and state disagreement

**Runtime result: NOT RUN.** Inputs: S7–S9, T2; fixtures F3/F4.

Establish each client independently: CLI request/re-query, TUI activity stream,
browser graph EventSource, terminal WebSocket, and MCP if retained. Cut transport,
change fixture state via a second authorized client, then restore transport. Test
initial endpoint absence separately from a dropped established stream. Repeat
client close/unmount during reconnect. For MCP, first identify its supported
transport and re-query semantics; no SSE/replay guarantee is assumed.

Capture retry timing, subscriptions, status indicators, query refreshes, missed or
duplicate events, terminal input/output and final DB/process truth. Pass criteria:
retained views converge within the predeclared 90 s observation window or clearly
report unavailable/stale state; no duplicate action/input or leaked subscription;
closing clients cancels owned retries. S9's unavailable initial stream is a source
exception, not proof of autonomous convergence. Compare browser graph refresh
with other UI queries separately. Cleanup: close clients, verify timers/sockets
are gone, remove proxy fault and use common cleanup.

### RV-06 — Dirty tracked, staged and untracked preservation

**Runtime result: NOT RUN.** Inputs: S4/S5; fixtures F2/F3.

Disable agent edits for the preservation control. Capture the full Git/file
baseline. Independently apply normal rig stop, supported delete/cleanup options,
failed launch cleanup, RV-07 snapshot failure, and RV-08 interruption/restart.
Run nested-repository and linked-worktree variants without treating either as
implicitly supported. Inspect user-edited managed guidance/hooks as well.

Pass criteria: tracked working bytes, staged entry modes/blob IDs, unstaged delta,
untracked and ignored sentinel bytes, nested repository state and worktree
registration remain unchanged unless a separately authorized managed-only change
was specified. A staged version must not be replaced by the working version.
Fail on reset/clean, silent removal, overwritten user content, or detached/broken
worktree registration. Snapshot success is not preservation evidence (S5).
Cleanup: save the comparison manifest outside the disposable project but inside
the evidence root; do not use `git reset`/`git clean` to conceal differences.

### RV-07 — Snapshot failure

**Runtime result: NOT RUN.** Inputs: S4/S5; fixtures F2/F3.

First capture a normal snapshot and compare payload/event. For a precise failure
case, a future authorized test may install a temporary SQLite trigger in the
synthetic DB that aborts insertion into the confirmed snapshot table only; record
the exact schema/trigger and remove it afterward. Do not patch production code.
Trigger stop, then observe snapshot/event records, error output, process kills,
session/binding changes and F2 preservation. Test broader disk-full/read-only
failure in a separate guest case because it can block more than capture.

Source-conformance criterion: capture error is visible and teardown may continue
(S4); failed snapshot creation must not leave a false snapshot-created event (S5).
Safety criterion: repository/user files survive and recovery limitations are
explicit. Continuing after capture failure is not a safe-recovery pass. Do not
require rollback of all external effects merely because the DB transaction failed.
Cleanup: remove and verify absence of the test trigger/fault, retain failed-state
evidence, then dispose of the guest; never inject faults into a live database.

### RV-08 — Abrupt stop, restart and native resume

**Runtime result: NOT RUN.** Inputs: S4/S5/S10, T4; fixtures F2/F3/F5.

Record native session identity and conversation-only marker. Separate cases:
terminate the fixture daemon abruptly; terminate one native process; reboot the
guest; interrupt a confirmed transaction boundary. On restart inspect consistency
before requesting restore. Test valid, absent and stale native resume tokens and
unknown terminal-provider liveness independently. No automatic fresh fallback is
authorized for a prior conversation. Explicit fresh start is a separate case.

Observe DB integrity on a consistent copy, queue/history, stale bindings,
process/listener ownership, file/index preservation and restore diagnostics.
Verify real continuity using native session identity plus recall of the marker
without including it in the new prompt; neither recall nor an interactive pane
alone is enough. Pass criteria: no duplicate occupant, no silent fresh replacement,
honest missing/unknown-token status, durable committed work, and unchanged F2
state. Native provider unavailability is unresolved evidence, not resume success.
Cleanup: stop only fixture processes, revoke test sessions/credentials as allowed,
save sanitized evidence and destroy the guest.

### RV-09 — Update, rollback and uninstall

**Runtime result: NOT RUN.** Inputs: S1/S11/S12; fixtures F2/F3/F6.

Identify the actual supported A→B installation/update route and its ownership
manifest before use; do not assume an updater or product uninstall command exists.
Install A, create synthetic state and user edits, take a verified consistent
backup, then update to B. Repeat with interruption during artifact replacement
and, separately, migration. Check runtime/listener identity, schema, hooks,
projections and protected native seats at each step.

Rollback on another fixture copy. Use A with B-state only if compatibility is
established; otherwise restore the verified pre-update synthetic backup and
report the recovery point and any post-backup data not carried back. Never blindly
downgrade the sole database. Test uninstall through the chosen distribution's
owned mechanism. Default is to preserve project/state/user configuration; a
separate explicit purge scenario may remove only listed fixture-owned paths.

Pass criteria: version/process/schema agree, interruption is diagnosable, rollback
returns to a verified state without claiming later writes survived, and uninstall
removes owned executables/hooks/listeners without deleting user data. Unsupported
rollback/uninstall routes are gaps, not passed cases. Cleanup: preserve A/B and
backup hashes and ownership deltas, then common cleanup of all fixture prefixes.

### RV-10 — Eventual Node-free artifacts and processes

**Runtime result: NOT RUN.** Inputs: S1/S12 and removal-plan boundaries; F1/F6.

This is a future candidate protocol, not an expectation that the current baseline
is Node-free. Run baseline workflows with their declared dependencies first.
For a candidate, use an image without Node/npm/npx (including renamed/bundled
runtimes), no global caches, and audited executable paths. Inspect extracted
archives, dynamic dependencies, shebangs, generated hooks/runners, copied skills,
installer/updater/recovery helpers and operational instructions. Trace every exec
and network fetch through RV-01–RV-09, not just steady-state process names.

For full project removal, independently build, test, generate, package and release
retained artifacts from source in a Node-free toolchain image; inspect CI setup
and project scripts. A runtime-only milestone must list remaining Node tooling.
Treat browser JavaScript separately from Node, and non-Rust browser functionality
as a scope decision. Identify external Claude/Codex/provider requirements and
CI-provider action runtimes separately; an external exception does not establish
a wholly Node-free host or excuse a project-authored Node runner.

Pass criteria: no project-required Node/npm/npx execution or hidden equivalent,
no required Node payload/download, and all retained scenario evidence available
for the claimed tier. Scan hits require classification; a clean grep is not proof.
Missing candidate artifacts or external-tool decisions leave results NOT RUN,
without blocking completion of this protocol. Cleanup: archive artifact manifests
and sanitized exec traces, then destroy both build and runtime guests.

## Tests read, not run

These are bounded excerpts inspected for protocol design. Other discovered tests
are not claimed as read. No unit/integration/browser suite was executed. Root test
scripts build/generate assets (S12), so they are not read-only research checks.

| ID | Inspected test excerpt and its limit | Execution result |
|---|---|---|
| T1 | `/home/kkk/.cline/worktrees/d1795/openrig-breakdown/packages/daemon/test/retry-first-start.test.ts:12-129` — mocked Pi projection failure/retry; assertions preserve sibling/history and refuse live/unknown states. Not Claude/Codex process proof. | NOT RUN |
| T2 | `/home/kkk/.cline/worktrees/d1795/openrig-breakdown/packages/ui/test/focused-terminal-reconnect.test.tsx:1-106` — mock WebSocket/xterm, fake timers, 3 s reconnect assertion. Not a network, authentication or real-terminal observation. | NOT RUN |
| T3 | `/home/kkk/.cline/worktrees/d1795/openrig-breakdown/packages/daemon/test/queue-transactional-closure.test.ts:22-140` — mock wake transport, separate-DB rejection and missing-outbox rollback assertions. Does not establish HTTP retry behavior. | NOT RUN |
| T4 | `/home/kkk/.cline/worktrees/d1795/openrig-breakdown/packages/daemon/test/native-resume-probe.test.ts:1-85` — synthetic pane classification and command-builder assertions. Not native conversation recovery. | NOT RUN |

## Cleanup and evidence record

For every case, including timeout/failure: first stop fault injection and preserve
sanitized logs/manifests. Stop through the fixture's explicit client endpoint
only after confirming its identity. If necessary signal individually recorded
PIDs only after matching executable, start time and run ownership to avoid PID
reuse. Check descendants and listeners independently of CLI success. Revoke test
credentials, close browser/native sessions, remove owned network rules/mounts,
then delete only the realpath-checked marked fixture or destroy its guest.
Never run broad `pkill`, host uninstall, shared terminal teardown or wildcard
cleanup against a source checkout. If ownership is uncertain, retain/quarantine
the guest and report it; do not guess. Verify no fixture resources remain outside
the retained evidence bundle. Cleanup failure is a failed isolation outcome.

A future run record must contain: run/scenario/subcase ID; approval reference;
environment/artifact identities; exact sanitized commands; pre-state; injection
seam and confirmation; timestamps/timeouts; outputs; process/listener/file/DB/Git
deltas; source-conformance and safety verdicts; unresolved observations; cleanup
receipt; reviewer. Evidence includes failed cases, not only successful screenshots.

**Runtime observations not collected:** RV-01 through RV-10 and every subcase are
**NOT RUN**. There are no install, auth, launch, retry, reconnect, preservation,
snapshot, resume, update, rollback, uninstall or Node-free measurements to report.

## Documentation completion boundary

Review checks only: read this document end to end; verify source/test citation
bounds at the pinned commit; check all ten protocols include fixtures, observations,
criteria and cleanup; confirm every runtime result says NOT RUN; inspect whitespace
and the single-file diff. These checks validate protocol structure and provenance,
not product behavior, independent acceptance, owner decisions or research gates.
No changes to source, tests, claim registers, task statuses or gate records belong
to this deliverable. Future observations must remain distinct from this source-only
plan, even after its documentation PR is merged.

Documentation verification on 2026-09-27: reread the complete document; checked
20 cited path/range references for existence and bounds; confirmed all cited
current-checkout files match the pinned baseline; checked all ten protocol result
and cleanup markers. These were static text/Git checks, not runtime experiments.
All runtime results and inspected-test execution results remain NOT RUN.