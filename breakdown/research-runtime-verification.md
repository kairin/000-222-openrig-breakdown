# Runtime verification protocol: WT-10, CAP-8 and Node removal

## Status and authority

**BLOCKED — preparation only; no product runtime cases executed.**

On 2026-09-27 the owner approved the proposed documentation plan and requested
commit, push, PR and merge. This authorizes documentation delivery, not unspecified
fault injection, agent spend, supported targets or access to live data. The final
target-specific run sheets still require owner approval before execution.

Source inspection used `aa51d3581825ce286d9dc2b379e21bee92a2a1c6`.
The focused publication branch starts at remote main
`9db3ed6c406be5c3d9a84720383fcf6b543169e6` to avoid implicitly publishing
16 unrelated local documentation commits. The application source inspected is
the existing TypeScript/Node implementation, not a Rust replacement.

Three requested documents were absent from the inspected checkout and remote
main: `breakdown/research-workflow-traces.md`,
`breakdown/research-capability-inventory.md`, and
`breakdown/research-state-invariants.md`. WT-10/CAP-8 could not be located.
Their exact wording and invariant IDs must be supplied before coverage approval.
Do not manufacture replacement research or mark those criteria complete.

The inspected `breakdown/10-rust-and-node-removal-plan.md` exists at the inspected
commit but is not yet on remote main. Its future acceptance section requires
isolated install/lifecycle proof, preservation and rollback, explicit external
tool exceptions, separate runtime/full-toolchain removal, and comparison against
the baseline. This protocol prepares those cases without importing the other
branch's documents or declaring Gates A–D passed. Retrieve that document from
the stated commit when reconciling criteria. Missing documents are named as text,
not broken local links.

Source reading and tests read or run in isolation are not proof of the full
runtime journeys. No Rust implementation, successful migration, or Node removal
is claimed. The runtime-verification card remains blocked; publishing this
protocol does not complete it or change external card state.

## Owner inputs required to unblock

Record each decision with owner, date, approval reference and affected case IDs.
Silence is not approval.

| Input | Required decision or resource |
|---|---|
| Authoritative criteria | Supply the three missing documents and revision; confirm the Node-removal plan revision. Map every WT-10/CAP-8 clause and state invariant to cases below. |
| Execution authority | Name the operator; authorize the exact cases, destructive fixture operations, process termination and fault injection. Set a time window, spend cap, network allowlist and stop conditions. |
| Supported targets | Specify OS/version/architecture, terminal provider/version and Claude/Codex version/install channel. A Linux container does not certify a desktop provider or another OS. |
| Retained interfaces | Decide CLI/TUI/UI/MCP scope, local versus cross-host operation, and notification transports. Approve exclusions explicitly. |
| Disposable environment | Provide VM/image digest or equivalent isolated target, access method, reset snapshot, export destination and destruction permission. No production mounts or host terminal sockets. |
| Agent access | Provision separate test accounts/credentials through a secret channel, model permissions and token budget. Do not put credentials into Git or evidence. |
| Artifacts and compatibility | Identify baseline/candidate hashes, distribution format, old-state support, upgrade/rollback version pairs, migration interruption points and uninstall retention policy. |
| Removal boundary | Ratify runtime-only intermediate versus full removal; external agent, browser JS, shell/native library and CI-provider exceptions. A permanent project Node exception changes the goal, not the result label. |
| Evidence and expected results | Approve storage/access/retention/redaction, readiness/time bounds, retry semantics, data-preservation contract and target-specific command sheets. |

Until these inputs exist, cases are **BLOCKED / NOT RUN**, not failed or
unsupported. A missing future candidate blocks candidate testing; it is not
evidence that a replacement exists.

## Isolation gate — RV-00

Before any product command, verify all of the following inside the approved
disposable target. Fail closed if any check is unknown.

1. Use a new test database and synthetic Git repository. Never use the owner's
   sole live database. Existing-state compatibility needs separately authorized,
   sanitized, consistent copies plus an independently preserved backup. Do not
   copy an active SQLite main file without its required WAL state or a supported
   consistent backup procedure.
2. Use a dedicated OS identity and fresh HOME, XDG directories, OPENRIG_HOME,
   agent configuration/transcript roots, install prefix and temporary directory.
   Inspect symlinks and mount destinations. Do not mount the owner's home,
   source worktrees, database, credential stores or terminal-provider sockets.
3. Start with an environment allowlist. Remove inherited OPENRIG/RIGGED endpoint,
   host, session and identity selectors. Set the explicit sandbox URL, port and
   database path after checking source command resolution. HOME isolation alone
   is insufficient: the CLI accepts OPENRIG_URL and legacy RIGGED_URL overrides.
4. Bind only sandbox interfaces. Block host/live daemon routes. Use isolated
   terminal sockets/sessions. Inventory baseline processes, listeners, mounts
   and routes; verify the test instance identity as well as its endpoint.
5. Restrict outbound network access to approved package/vendor endpoints. Enable
   process-execution, filesystem and network observation before installation.
   Choose OS-native tooling only after the target is known; record coverage gaps.
6. Prove reset/export works with a harmless fixture. Keep evidence outside paths
   subject to cleanup. Stop if a command resolves outside the sandbox, an unknown
   process owns a resource, a secret appears in logs, or budget is exceeded.

Never use blanket process-name kills, host-wide cleanup or production rollback.
Capture PID start time, executable and sandbox ownership before sending signals.
Cleanup failures are evidence, not permission to broaden the cleanup target.

## Reproducible run-sheet contract

Create one approved run sheet per case/target/version before execution. This
document is a protocol specification, not a copy-and-run shell script. Do not
execute commands with unresolved values or infer flags from prose.

Each sheet must contain:

- Criterion/invariant IDs, source revision, artifact checksum, target image,
  owner approval reference and expected result (including allowed differences).
- Literal commands and argument vectors, absolute working directories and paths,
  sanitized environment, input payloads, fixture seed and exact tool versions.
- Setup, before-state capture, operation, fault injection, bounded observation,
  after-state capture, assertions, cleanup and reset in that order.
- Explicit timeout values, retry counts, signal targets, network fault rules and
  the observation that proves the injected fault reached the intended phase.
- Expected exits/errors, durable state transitions, process ownership and visible
  result. An unexpected baseline behavior is recorded, not silently blessed as
  the future acceptance contract.
- A second run from a reset image using the same sheet. Keep both results; never
  discard the failed first attempt when a rerun passes.

Review CLI parser/source before pinning executable commands. For example, the
existing smoke path uses `rig daemon start --port ... --db ...`, `/healthz`, and
`rig daemon stop`; validate the packaged version and target selection before
using these. Commands for queue, restore, providers and uninstall must come from
the selected version, not guesses or an assumed universal uninstall verb.

## Case matrix

All cases below are initially **BLOCKED / NOT RUN**. Split rows into independent
subcases for every approved target, agent, interface, failure and version pair.
Exact WT-10/CAP-8 mappings await the authoritative documents.

| ID | Protocol and fault | Required observations / acceptance |
|---|---|---|
| RV-01 Clean install/start | Reset pristine image; install checksum-pinned artifact without monorepo dependencies; start, inspect and perform one useful operation; repeat start. | Record installs/downloads/config choices, healthy instance identity, owned listener and full child execution. Repeat start must follow the approved existing-instance contract with no unrelated process adopted or killed. |
| RV-02 Failure diagnostics | Reset between missing executable/prerequisite, absent or invalid auth, malformed config, and occupied-listener subcases. The listener is a known disposable sentinel. | Prove each fault; record exit/status, actionable diagnostics, bounded wait and changed files. Sentinel remains alive; no accidental fallback to another daemon, false readiness or leaked partial installation. |
| RV-03 Agent launch/readiness | Launch Claude and Codex separately, then a mixed team. Exercise auth/trust/model gates, early exit, timeout and one member failing after another is ready. | Capture native identity and terminal readiness, not just PID liveness. Inspect partial cleanup, DB/session rows, owned processes/workspaces and hook/config diffs. Preserve healthy members or stop them only according to the approved contract. |
| RV-04 Queue/notifications | Submit normal work; repeat the same action and distinct creates; drop connection before commit and after commit before response; disconnect receiver; interrupt notification delivery; retry and restart. | Record request identity, queue rows, ownership and transition audit before/after. Separate commit, delivery and acceptance. Check operation-specific duplicate semantics; do not assume creates deduplicate. Test ordinary nudges separately from terminal-handoff durable wake intents, including recovered pending intent and repeated drain. |
| RV-05 Client reconnect | For each retained CLI/TUI/UI/MCP surface, disconnect transport, mutate synthetic state elsewhere, reconnect; repeat with client restart and daemon restart. | Compare presentation/API result to durable state; record lost/duplicated events, stale-status indication and resynchronization. CLI means a failed request and new request/retry, not an assumed persistent connection. MCP includes client/session reinitialization. Excluded surfaces need an owner decision. |
| RV-06 Dirty work | Build a repository with unstaged tracked edits, staged additions/deletions, different staged and unstaged contents in one file, untracked and binary files; include nested repos/worktrees if retained. Repeat normal stop, restore and partial cleanup. | Compare HEAD, index entries, staged and unstaged binary diffs, status, untracked contents/hashes, modes/symlinks and worktree metadata. A clean-looking status or snapshot row alone is not proof. No agent may edit the sentinel files during preservation checks. |
| RV-07 Snapshot failure | On a reset fixture inject a deterministic capture or persistence failure at a reviewed seam; distinguish transaction failure from post-commit notification failure. Run teardown and attempt recovery. | Prove the fault was reached. Record warning/exit, snapshot/event atomicity, whether stopping continues, owned process cleanup and dirty-work comparisons. Separately label file preservation and conversation recovery; one can succeed while the other fails. |
| RV-08 Abrupt stop/restart/resume | Record a synthetic conversation and native ID for each agent. Terminate owned daemon and agent processes independently at defined phases, then restart/restore; include missing resume identity and stale/unknown liveness. | Capture reconciliation, old/new process trees, durable queue/session state and shutdown receipts. Prove native conversation continuity using prior synthetic exchange plus native identity/transcript and terminal evidence. A fresh conversation with replayed text is not native resume. |
| RV-09 Update/rollback/uninstall | Install approved old artifact; create dirty work/data and user-owned hooks/config; update to pinned candidate; interrupt migration/update at approved points; reset and test rollback, repeated actions and uninstall. | Compare schema/migration ledger, consistent DB exports/integrity, files, hook ownership, command resolution, listeners and children. Verify documented retained data and restoration path. Do not assume downgrade is supported; label unsupported pairs explicitly. Never erase user-owned hooks/config to obtain a clean result. |
| RV-10 Runtime Node removal | Only with an actual candidate: run RV-01–09 and retained hooks/helpers/recovery with Node/npm/npx absent from project process access. Inspect archives, embedded/generated scripts and all descendant executions. | No project-owned Node execution or bundled/renamed Node dependency. A PATH-only check or grep is insufficient; observe execs including absolute paths. Trace network fetches and lazy downloads. External-agent exceptions are measured separately; do not claim a Node-free host. |
| RV-11 Full project Node removal | Only with an actual candidate and approved toolchain: clean build, test, generate, package and release verification without required project Node/npm/npx. | Record all tooling and generated artifacts, UI pipeline, CI actions and operational instructions. Runtime-only success cannot pass this case. Verify provider runtime exceptions separately. No publishing a real release as a test without separate approval. |
| RV-12 Baseline comparison | Repeat identical required workflows on the current baseline and eventual candidate with equivalent fixtures/targets. | Count installs, decisions, commands, processes, config touchpoints and manual recovery steps. Show user benefit and any regression; Rust language adoption is not a simplicity measurement. |

Fault injection must be scoped and reproducible. Prefer sandbox proxy/network
rules for disconnects and reviewed fixture-only storage/permission failures for
snapshot tests. Do not add production failpoints in this documentation task.
If no safe deterministic injection exists, label that subcase blocked, explain
the gap and request a separately reviewed test-harness change. Do not substitute
a mocked unit test and call it an end-to-end failure run.

## Evidence bundle and verdicts

Use a unique run ID. Preserve a manifest, approval/scope reference, complete
command transcript, sanitized environment, logs, before/after state and checksums
in the approved evidence store. Export before cleanup, then append cleanup proof.
Never commit credentials, owner databases or raw private conversations.

| Evidence | Minimum capture |
|---|---|
| Environment | UTC times, operator, OS/image digest, kernel/architecture, locale, tool/agent/provider versions, source and package hashes, working directories, environment allowlist, mounts/routes, resource limits and tracing tool versions. |
| Commands | Exact argv and sanitized env/stdin, stdout/stderr, exits/signals/duration, timeout/retry settings, fault timing and reproduction/reset sequence. Redacted values are marked and supplied securely for authorized reproduction. |
| Processes/network | Continuous child exec observation plus before/during/after PID/PPID trees, executable paths/start times, terminal sessions, listeners, destination/protocol records and downloaded artifact hashes. Report blind spots, not a false absence claim. |
| Artifacts/logs | Installed archive listing and digest, generated file manifests, managed/user hook/config diffs, daemon/client/agent logs, readiness screens and shutdown receipts. Restrict sensitive logs and document redaction. |
| Durable state | Consistent database backup/export, schema/migration ledger, relevant queue/session/snapshot/event records, integrity checks, native resume identities and synthetic transcript proof. Record WAL/backup method. |
| Work preservation | HEAD, index stage entries, staged/unstaged binary diffs, status including untracked paths, file hashes/bytes, modes/symlinks and worktree metadata, with an explicit allowed-difference list. |
| Outcome | Expected versus actual assertions, evidence links, failed/unrun steps, residual resources, reset result, reviewer, reproduction run ID and remaining scope. |

Verdicts:

- **PASS:** executed on the named target; every assertion has sufficient evidence.
- **FAIL:** executed and an acceptance assertion failed. Preserve evidence and
  report the defect; subsequent success does not erase this result.
- **BLOCKED:** a required approval, environment, artifact or safe method is absent.
- **NOT RUN:** no execution occurred; name the reason and dependency.
- **UNSUPPORTED:** owner explicitly excluded the target/feature/version pair;
  cite the decision. Lack of credentials is not an unsupported verdict.

Record execution status separately from verdict where helpful: all current
cases have execution NOT RUN and disposition BLOCKED. No product evidence
bundle exists for this preparation pass. Documentation checks are separate.

## Source leads and existing test limits

These paths are repository-relative leads at the inspected commit, not runtime
evidence. Confirm them at the eventual release revision when writing commands.

- `packages/cli/src/client.ts:122-163`: current/legacy URL overrides and identity.
- `packages/cli/src/commands/daemon.ts`: startup/stop/status command contracts.
- `packages/daemon/src/adapters/claude-code-adapter.ts` and
  `packages/daemon/src/adapters/codex-runtime-adapter.ts`: launch, hooks, readiness
  and native resume; Codex readiness is specifically at lines 433–451.
- `packages/daemon/src/domain/snapshot-capture.ts:149-164`: snapshot/event
  transaction followed by best-effort subscriber notification.
- `packages/daemon/test/queue-transactional-closure.test.ts` and
  `packages/cli/test/d14-loud-queue-transport-failure.test.ts`: intended retry and
  diagnostic contracts; the latter uses mocked lifecycle/transport dependencies.
- `scripts/smoke-fresh-install.sh`: builds, installs and starts a Node baseline;
  suppresses some output and deletes scratch evidence on exit. Do not execute
  unchanged as an evidence protocol. Preserve logs and prove isolation first.
- `docker/testbed/Dockerfile`: Node/npm-based Linux/stub testbed, not real
  Claude/Codex authentication, desktop-provider or Node-removal proof.

## Review and completion gate

1. Obtain missing authoritative criteria; map every clause to case/subcase and
   expected evidence. Review gaps against the state-invariant ledger.
2. Obtain owner scope/environment/authority; pin literal command sheets and
   safety assertions. Approve protocols before running anything.
3. Execute only authorized cases inside verified disposable environments.
   Independent reproduction/review must reference the same artifact and scope.
4. Publish evidence-linked outcomes, failed/unrun cases and explicit unsupported
   decisions. Update research links only when their authoritative files exist.
5. Close runtime verification only for the approved, demonstrated scope. Keep
   outstanding supported cases blocked/open. Do not use this document, a merged
   PR, source inspection or unit-test success to close runtime acceptance.

This preparation does not change research task statuses, claim verdicts or
Gates A–D. Approval of a Rust architecture/implementation remains a separate task.