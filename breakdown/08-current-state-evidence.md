# 8. Current-state evidence for simplification research

## 8.1 Purpose and evidence limits

This note records a repository-level reading of the checkout at `147993aa`
(`main`, 2026-09-26). It supports a later choice about what to keep and how a
learner should approach the code. It does not make that choice.

2026-09-27 addendum: [10-rust-and-node-removal-plan.md](10-rust-and-node-removal-plan.md)
rechecks the 2,364 TypeScript / 57 scripts-directory file / 85 migration counts
at `9db3ed6c406be5c3d9a84720383fcf6b543169e6`, using tracked-file predicates,
and adds the requested Rust/Node-removal boundary and inventory. The rest of
this report retains its original source-reading baseline; it is not a new
runtime verification. The goal document now uses the defined count instead
of its older approximate 2018 figure.

Use the evidence according to its source:

- `README.md` and `breakdown/` state this derivative's current goals and rules.
- Current source proves mechanisms and boundaries. It does not prove that a
  feature is useful to its owner.
- Tests are not run in this research. Test files can show what behavior the
  project chose to encode, but not that the behavior passes here.
- Git history shows when code entered reachable history. A commit message does
  not prove why its author made the change.
- `docs/as-built/` is an inherited source map, not a current authority: its
  verified commit is `7eaf524c`, identified there as v0.3.1. This repository is
  v0.5.16. Recheck its claims against current source before relying on them.

The requested OpenRig skill router was read at
`/home/kkk/.agents/skills/openrig-skills/SKILL.md`. Its procedure is
`rig context list` → select the exact ref → `rig context get`. The router
command reported that the daemon is stopped. I did not start a daemon. The
repository copy of the router is
`packages/daemon/specs/agents/shared/skills/core/openrig-skills/SKILL.md`.
The locally mirrored skill tree has no `openrig-builder`,
`forming-an-openrig-mental-model`, or `openrig-user` file to load directly;
the CLI router could not retrieve those entries while the daemon was stopped.
No OpenRig operational command was needed for this read-only research.

## 8.2 The goal to use when judging features

The current [root README](../README.md#L1) is more specific and newer in intent
than the still-open choices in `06-open-questions.md`:

- It is a personal derivative for one person's workflow.
- It runs several Claude Code and Codex CLI agents together as one team.
- It should take less learning and memory, use familiar engineer terms, retain
  only functions that earn their cost, and be readable enough to operate and
  repair without an agent.
- Stopping must not lose uncommitted work.
- It must not download from or depend on the original project's services.

The command surface and setup assumptions remain open in
`06-open-questions.md`, but the newer README already answers some of that
document's questions: the audience is personal use, and the intended harness
set includes Claude Code and Codex CLI. Do not treat those older rows as
unanswered without reconciling them with the README.

`03-local-environment.md` records an installed upstream CLI at 0.5.15 and its
commands/state path. This pass preserved that document but did not inspect the
user's installed binaries or home-directory state, so treat those machine
details as documented claims rather than independently verified facts.

The current README says the code has not yet been simplified. It also says the
current `rig setup`/daemon path changes machine and workspace configuration,
and that the daemon attempts an upstream plugin download at each start
(`README.md:12-20,59-66`). `02-separation-from-upstream.md` identifies the
specific plugin source and upstream links. Those are directly relevant to the
self-contained goal and to any removal plan.

## 8.3 Checkout and code inventory

Observed at this checkout:

| Item | Evidence |
|---|---|
| Version | `0.5.16`, root `package.json:2-4`; current README says simplification has not begun, `README.md:12-15` |
| Active npm workspaces | daemon, UI, CLI, TUI (`package.json:7-12`) |
| Runtime/tooling | ESM; Node `^20 || ^22 || ^24`; npm scripts, TypeScript compiler, Vitest (`package.json:6,13-20,30-32`) |
| Main implementation language | 2,364 `.ts`/`.tsx` files under `packages/` (counted with `rg --files`); 1,117 under `packages/*/src` |
| Other source assets | `scripts/` contains 57 files; daemon migration directory contains 85 numbered TypeScript migration files |
| Rust | No `.rs`, `Cargo.toml`, or `Cargo.lock` exists in the repository |
| CLI package dependencies | Hono, `better-sqlite3`, Commander, YAML, Zod/Ajv, tar, ULID and MCP SDK (`packages/cli/package.json:47-62`) |
| Daemon dependencies | Hono, `better-sqlite3`, TOML, tar, ULID and YAML (`packages/daemon/package.json:79-87`) |
| CLI composition | `createProgram` currently registers 85 command-builder values with `program.addCommand` (`packages/cli/src/index.ts:165-263`). This is a count of registrations, not unique user-visible command names or aliases. |

The file-count commands were `rg --files packages -g '*.ts' -g '*.tsx'` and,
separately, `rg --files packages/*/src -g '*.ts' -g '*.tsx'`. They include
source and test files, exclude ignored files per ripgrep defaults, and are
navigation measures only. Per-directory totals are CLI 352, daemon 1,346,
TUI 145, UI 521, and test-system 0, which sum to 2,364. The `src/` totals are
CLI 158, daemon 593, TUI 62, and UI 304. `01-goal-and-scope.md` originally said
“approximately 2018” TypeScript files without a counting predicate; that
stale figure has now been replaced with the rechecked tracked-file count in
10. The old approximation cannot establish growth. None of these counts
measures understandability or value.

### Deployment and runtime map

- The package path is npm + Node ESM, with `rig` and `openrig-tui` binaries
  declared by the CLI package (`packages/cli/package.json:28-36`).
- The daemon is a local Hono service backed by `better-sqlite3`; startup
  resolves its DB path and creates/migrates the database
  (`packages/daemon/src/index.ts:239-298`; `startup.ts:240-249`).
- An agent is an external Claude Code or Codex CLI process in a tmux session.
  The root README states that operating model (`README.md:50-57`); the runtime
  adapter interface says harness launch occurs inside tmux
  (`packages/daemon/src/domain/runtime-adapter.ts:121-150`). `rig setup`
  handles host prerequisites; it does not install the external agent account
  or authenticate on the user's behalf (`packages/cli/src/commands/setup.ts:357-380,570-584`).
- The CLI and TUI are separate workspace binaries; UI is a React/Vite
  workspace. The contributor guide calls the CLI primary and the web UI
  experimental, maintenance-mode, best-effort (`docs/reference/developing.md:1-33`).
  MCP is another CLI-wrapped API consumer (`packages/cli/src/mcp-server.ts:48-55`).
- Persistent state is not just one database: the default OpenRig home is
  `~/.openrig` (`packages/daemon/src/openrig-compat.ts:1-56`), with config,
  SQLite DB, host registry, transcripts, skills, context and workspace paths
  configured from there (`packages/cli/src/config-store.ts:185-236`). The
  daemon also interacts with project-level Claude files and `~/.tmux.conf`
  (`README.md:59-66`; `setup.ts:586-614`; `claude-code-adapter.ts:745-748`).

This describes the current deployment shape; it does not show that each
component is necessary for the owner's desired journey.

`test-system` is a repository package directory with runbooks, YAML scenarios,
and eval fixtures, but it is not one of the four npm workspaces
(`package.json:7-12`, `packages/test-system/README.md`). Its scenarios make
the breadth of encoded operational contracts visible; they do not establish
which ones the owner needs.

### As-built documentation age

`docs/as-built/README.md:14,28-30,56-81,93-96` states that its source check was
against commit `7eaf524c`, package version 0.3.1, and describes 14 architecture
modules plus four UI modules and the CLI reference. Current root version is
0.5.16. As a concrete drift signal, the old daemon core page says the schema
has 40 migrations (`docs/as-built/architecture/daemon-core.md:12,22-23,135-185`)
while the current migration directory contains 85 numbered migrations. The
old page's 58 CLI-group count is also not comparable to the 85 registration
count above; they are different measures at different revisions. Preserve the
as-built tree only if its source-grounding contract can be restored and kept
current.

### Applicable local instructions

`/home/kkk/Apps/AGENTS.md` applies. It says not to read or activate `.envrc`,
not to expose secrets, to treat child directories as separate projects, and
not to change Git history or publish without an explicit request. No local
`AGENTS.md` was found at the repository root or above it. The task requested
research and a new evidence note only; no tests or product changes were made.

## 8.4 Smallest useful operator journey supported by current sources

The current README's goal implies a human should be able to run and recover a
small team. The shortest documented journey is not yet a minimal product
contract; it is a candidate against which to judge complexity:

1. Prepare one project directory and check the runtime prerequisites. The
   authored [getting-started guide](../docs/reference/getting-started.md#L54)
   recommends a setup dry run and checks tmux and Codex authentication.
2. Preview and launch a small starter in that directory. The guide's first
   command path is `rig specs preview first-project`, `rig up first-project
   --cwd . --plan`, then `rig up first-project --cwd .`
   (`docs/reference/getting-started.md:66-80`). That starter is a two-seat
   Codex pair, not a mixed Claude/Codex pair (`:3-6,97-100`).
3. Check daemon and seat readiness (`rig status`, `rig ps --nodes ...`), then
   send the owner one bounded request (`docs/reference/getting-started.md:71-80,102-126`).
4. Read the durable queue row and transition log, inspect the repository
   artifact, and decide whether the work is actually complete
   (`docs/reference/getting-started.md:117-126`).
5. Stop and return to the same rig. The operator guide describes same-seat
   restart and recovery choices (`docs/reference/getting-started.md:181-190`);
   daemon code attempts an `auto-pre-down` snapshot before killing live
   sessions (`packages/daemon/src/domain/rig-teardown.ts:103-123`).

This is a useful product-level spine: prepare → launch → assign → inspect →
stop/resume. It is an inference from the stated goals and documented paths,
not an owner-approved scope. The guide is substantially more elaborate than
this five-step outline because it covers prompt permissions, queue ownership,
kernel readiness, terminal sharing, projects/scopes, and workflow escalation.

## 8.5 Five concrete runtime flows and the contracts they carry

### A. Setup and launch

`rig setup` checks or installs parts of the environment; it has branches for
tmux availability and Codex login, and edits a managed block in `~/.tmux.conf`
(`packages/cli/src/commands/setup.ts:357-380,586-624`). Root docs also list
Claude/Codex configuration writes and the daemon-start plugin download
(`README.md:59-66`; `packages/daemon/src/startup.ts:240-249` begins daemon
construction and database migration; `02-separation-from-upstream.md:2.2`
names the upstream plugin seam).

`rig up` has more than one meaning. CLI resolution can route a name to a new
library spec or to a stopped, snapshot-backed existing rig; when the same name
matches both, it instructs the user to use the path or `--existing`
(`packages/cli/src/commands/up.ts:287-339`). Its plan/preflight path and launch
readiness are separate from daemon health, as the current getting-started guide
states (`docs/reference/getting-started.md:17-20,66-80`).

Failure/recovery contracts: missing runtime/auth must be diagnosed before
assigning work; ambiguous rig/spec names stop instead of choosing silently;
kernel readiness is not equivalent to daemon health
(`getting-started.md:54-80`; `up.ts:287-315`).

### B. Send a message to an agent

`rig send` targets a seat or fans out by pod/rig. It derives actor identity from
the seat environment/transport header, not a free-form `--from`; it checks for
positive evidence of an interactive prompt before injecting text
(`packages/cli/src/commands/send.ts:230-283`; help text at `:252-272`).
Unknown/stale activity telemetry allows a send with an advisory; busy is not a
block. `--verify` proves only that text appeared in the pane, not that an agent
read or acted on it (`send.ts:256-272`).

Failure/recovery contracts: prompt/permission guards can refuse; only explicit
`--dangerously-interact --reason` overrides that guard and is audit-logged;
multi-recipient sends report independently. This is a subtle user boundary and
is part of the practical transport protocol, not merely CLI wording.

### C. Durable assignment and handoff

Queue rows and transitions are separate SQLite tables. The item stores owner,
state, closure reason/target, and delivery metadata; transitions form an
append-only audit log (`packages/daemon/src/db/migrations/024_queue_items.ts:19-52`,
`025_queue_transitions.ts:13-29`). Claim identity is derived from the seat
environment. The CLI's `queue update` says `done` requires an explicit closure
reason, while `queue handoff` creates a successor row for the next owner
(`packages/cli/src/commands/queue.ts:558-644,743-823`). Domain code requires
some closure reasons to name a target; the transactional repository owns the
source-close plus new-row operation
(`packages/daemon/src/domain/queue-repository.ts:1507-1669`).
For ordinary `queue create`, the row and event commit first, then subscriber
notification and a best-effort pane nudge occur; nudge failure is recorded and
does not undo the row
(`packages/daemon/src/domain/queue-repository.ts:1165-1198,1320-1358`). For terminal
handoff, a durable wake intent is staged in the close+create transaction, then
delivered after commit. Startup recovery drains pending intents; failed or
indeterminate outcomes remain visible and are not automatically re-sent
(`packages/daemon/src/domain/queue-repository.ts:807-846,910-945,1114-1162,1608-1669`).

Failure/recovery contracts: a missing seat identity yields an actor-required
error; a terminal `done` transition without a valid closure contract is
rejected. This means a simple-looking “task list” has ownership and lineage
rules coupled to auth identity and an audit trail.

### D. Stop and restore after daemon/host interruption

The teardown service captures `auto-pre-down` best-effort, then kills tmux
sessions and clears matching DB state (`packages/daemon/src/domain/rig-teardown.ts:103-135`).
If snapshot capture fails, it records a warning but proceeds with teardown
(`:103-113`). Restore compares DB rows with live tmux sessions before mutation;
live or uninspectable sessions block restore (`restore-orchestrator.ts:239-250,782-812`).
When an existing resumable seat has a prior session but no resume token, it
returns `awaiting-decision` and starts no session; a fresh start must be
explicitly requested (`restore-orchestrator.ts:976-1006`).

This protects against silently replacing a conversation with a fresh one, but
it does not mean every stop can resume the exact agent conversation. The
repository's uncommitted files remain in their project checkout during normal
`down` (inference: teardown source kills tmux sessions and mutates OpenRig
state; it does not remove the project checkout in this path). A failed
snapshot can still discard the live process/context. So the README's
uncommitted-work goal should distinguish durable filesystem changes from
resumable conversation state.

### E. Recover after reboot or lost tmux transport

The tmux adapter distinguishes `absent` from `transport_unavailable`, and
explicitly says a socket/transport failure does not prove that a session is
absent (`packages/daemon/src/adapters/tmux.ts:82-91,133-157`). Restore treats
tmux probe exceptions as unknown and blocks, rather than guessing
(`restore-orchestrator.ts:797-809`). The getting-started recovery table says a
host reboot can remove tmux sessions while the existing project and queue
remain, and calls for a separate decision before a fresh conversation
(`docs/reference/getting-started.md:181-190`).

This path explains why replacing “session” with a process ID or a queue row
would be unsafe: session identity, native resumability, tmux liveness, durable
task state and current human decision are separate facts.

## 8.6 Couplings a simpler implementation still has to account for

These are specific seams where reducing file count or switching language
alone would not remove the behavior contract.

1. **Session state is not process truth.** Session rows and bindings live in
   SQLite; tmux is probed independently. Restore classifies every DB-running
   session as live, stale or unknown, and fails closed on unknown. A native
   session can outlive daemon restart, while a persisted row can outlive a
   tmux server (`packages/daemon/src/domain/session-registry.ts:77-133`;
   `packages/daemon/src/adapters/tmux.ts:82-91,133-157`;
   `packages/daemon/src/domain/restore-orchestrator.ts:782-812`).
2. **Queue identity, durable state and wake delivery cross layers.** Queue
   routes derive actors from transport identity; the repository persists
   queue row + transition, and may nudge a destination session through
   `SessionTransport` after commit
   (`packages/cli/src/commands/queue.ts:558-585,743-823`;
   `packages/daemon/src/domain/queue-repository.ts:201-217,248-256,1165-1198`;
   migration `025` above).
   For a terminal handoff, a durable wake intent joins the source-close and
   successor-create transaction; startup retries only still-pending intents,
   while failed/indeterminate intents remain visible for out-of-band handling
   (`packages/daemon/src/domain/queue-repository.ts:807-846,1114-1162,1608-1669`). A regular queue create
   records the nudge result but does not roll back the row on nudge failure.
   The guide distinguishes message delivery from acceptance
   (`docs/reference/getting-started.md:90-126`).
3. **Filesystem projection accompanies runtime setup.** Runtime adapters
   project files and deliver startup content, then launch the harness inside
   a tmux session (`packages/daemon/src/domain/runtime-adapter.ts:21-51,121-150`). Claude hooks are
   installed into workspace `.claude/settings.local.json` and reconciled
   without deleting user-authored hooks (`packages/daemon/src/adapters/claude-code-adapter.ts:730-818`).
   Removing a provider path must also undo only owned artifacts, preserving
   user configuration (`breakdown/05-simplification-rules.md:13-18`).
4. **Events participate in durable operations.** `EventBus.emit` stores an
   event in SQLite; `persistWithinTransaction` allows an event to commit with
   the state change before subscribers are notified (`packages/daemon/src/domain/event-bus.ts:52-91`).
   Snapshot and teardown flows make durable snapshots/events before/around
   process changes (`packages/daemon/src/domain/snapshot-capture.ts:149-166`;
   `packages/daemon/src/domain/rig-teardown.ts:103-123`).

The state stores visible in these flows are:

| State | Current authority / evidence | Consequence for simplification |
|---|---|---|
| Rig, node, session, binding and resume token | SQLite records read/written by repositories and `SessionRegistry`; actual tmux presence is separately probed (`packages/daemon/src/domain/session-registry.ts:77-133`, `packages/daemon/src/adapters/tmux.ts:82-91`) | A durable identifier is not proof a process is alive or resumable. |
| Queue item and transition history | SQLite `queue_items` latest row plus append-only `queue_transitions` (`024_queue_items.ts:19-52`, `025_queue_transitions.ts:13-29`) | Keep ownership/history consistent across retries; do not equate a notification with task acceptance. |
| Events for clients | SQLite `events` rows, then subscriber notification (`packages/daemon/src/domain/event-bus.ts:52-91`) | Events have persistence and ordering contracts, not just UI refresh callbacks. |
| Project work | User's project directory; ordinary teardown kills tmux and updates OpenRig state, and does not call project deletion in the path read (`packages/daemon/src/domain/rig-teardown.ts:115-135`) | Uncommitted files and a live agent conversation have different preservation guarantees. This does not establish what every optional cleanup command deletes. |
| Provider configuration/hooks | User-level or project-level files with OpenRig-owned entries (`packages/cli/src/commands/setup.ts:586-614`, `packages/daemon/src/adapters/claude-code-adapter.ts:745-820`) | Cleanup needs ownership rules so removing OpenRig does not remove user-authored config. |

User/process identity crosses the CLI, HTTP and queue boundaries. For
example, the CLI documents that sender identity is derived from
`OPENRIG_SESSION_NAME` and stamped as `X-OpenRig-Session`; queue claim
commands ignore deprecated caller-supplied destinations
(`packages/cli/src/commands/send.ts:230-237`; `packages/cli/src/commands/queue.ts:558-585`). A
replacement that accepts arbitrary actor strings could break queue ownership
guarantees.

## 8.7 Rust boundary and likely port costs

There is no current Rust crate to extend. A Rust-first direction is therefore a
new implementation, not a switch of compiler on the existing source. The
current package/dependency boundary implies at least these port jobs:

- The CLI currently relies on Commander, JSON/YAML/TOML parsing, local files,
  and a sibling Node runtime check for the native SQLite binding
  (`packages/cli/package.json:47-62`; `packages/cli/src/bin-wrapper.ts:50-79`).
- The daemon exposes Hono HTTP/WebSocket routes, a synchronous SQLite API, 85
  numbered migrations, and SSE/event projections. Rust must preserve database
  ordering, state transitions, endpoint/JSON shapes, and transaction boundaries
  if it is to reuse existing data and UI/CLI consumers (`packages/daemon/package.json:79-87`;
  `event-bus.ts:52-91`; migration directory).
- The tmux contract is OS process invocation plus exact output parsing, shell
  quoting, transport-vs-absence classification, and temporary-file/buffer
  lifecycle (`tmux.ts:1-29,82-175`).
- Provider adapters launch Claude/Codex processes, capture native resume tokens,
  install/remove managed hooks, and reconcile startup files. Those are
  filesystem and native CLI contracts, not ordinary domain structs
  (`runtime-adapter.ts:121-150`; `claude-code-adapter.ts:730-818`).
- The UI is a separate React browser client; its TypeScript/Vite build and
  Vitest tests require Node tooling (`packages/ui/package.json:7-14,16-54`).
  The daemon serves its built assets (`packages/daemon/src/server.ts:806-835`).
  Browser JavaScript is not a Node runtime dependency. Top-level Node scripts
  generate and mirror skills/context (`package.json:21-28`). A Rust daemon
  alone does not remove those build/tooling dependencies, the Node TUI, or
  managed Node hooks; see the fuller inventory in 10.

These are observable compatibility costs, not an argument against Rust. A
small CLI-only prototype over a deliberately narrow JSON/SQLite contract would
have a smaller boundary than a daemon replacement. Which boundary to target is
an open architecture choice, not established by this research.

## 8.8 History and repository separation

Read-only Git observations:

- The reachable history begins at `f3a97b02` on 2026-03-23; initial scaffold
  `f3742e53` introduced the monorepo, Hono server, SQLite migrations, and Vite/
  React shell. Session registry (`4d4acd52`), tmux read/write adapters
  (`82221232`, `e5de37f6`), snapshots (`48603f55`) and restore
  (`9ec0a4f8`) also appear on 2026-03-23. These are commit subjects, not
  independently verified author motivations.
- Later tags run through `v0.5.16`; source added product layers after the
  March scaffold. Examples include workspace/workflow GA work (`a28f9b71`,
  2026-05-16), scope CLI release (`503424aa`, 2026-06-02), permission/context
  work through v0.5.0 (`20991e3c`, 2026-08-05), and queue invariant work in
  August (`24f4d8e5`, 2026-08-09). Use `git log -- path` for a narrower lineage.
- There are 12 named `origin/*` topic refs apart from `origin/main` and
  `origin/HEAD`. Eleven topic tips are ancestors of `main`; the exception is
  `origin/test-fixes-3-failures` at `1e0152f0`. Its merge base with `main` is
  `72b0a910ec6bac301d7a8939bdade0c19769b1ae`, and it has one commit not on
  `main`. Therefore the assertion that every branch tip is reachable from
  `main` is false. `git count-objects -vH` reports a 29.06 MiB pack.
- `git rev-list --count HEAD` is 3,034; `git rev-list --count --all` is
  3,444. `--all` includes commits reachable only through other refs (including
  historical refs), so the 410 difference is **not** a measure of unique
  commits on the one unmerged topic or of data to transfer. `--all --not main`
  counts 410 commits across those refs; only one is unique to the named
  unmerged topic.
- The worktree was clean before this evidence note was created. The final
  status after documentation edits is recorded by the researcher, not
  inferred from this pre-edit fact.

This repository has `origin` pointed at `kairin/openrig-breakdown` and no
`upstream` remote. `02-separation-from-upstream.md` documents the GitHub
fork-network separation. Preserving history retains code provenance but also
retains unrelated historical branch/tag reachability; no history rewrite is
needed to create a smaller product implementation.

## 8.9 Proposed learner order and unresolved evidence

For reverse engineering, a low-friction order is:

1. Read the current README goals and the shortest first-use guide. Decide what
   “one team” means for the owner's actual use before adopting the guide's
   starter as the product contract.
2. Trace one launch path: CLI name/spec resolution → daemon route → spec
   validation/preflight → startup orchestrator → runtime adapter → tmux.
3. Trace one message and one durable work handoff separately. Keep in view
   actor identity, SQLite commit, event delivery, tmux wake, and queue
   acceptance.
4. Trace `down` → automatic snapshot → tmux teardown → `up`/restore. Record
   which project files, database facts, session IDs and provider-native resume
   tokens survive each interruption.
5. Only then inventory each optional surface (UI, TUI, MCP, multi-host,
   workflows, plugins/context packs, scope/workspace, alternate runtimes) by
   owner value, dependencies, machine-written artifacts, and contracts it
   shares with the core.
6. Choose a first implementation boundary and language after measuring the
   protocol to preserve. Do not use source-file count or language preference
   alone as a proxy for simplicity.

This pass also read source that can inform, but does not decide, Gemini claims
in `gemini-review/`: A2 (daemon/SQLite packaging), A3 (tmux), A5
(persisted session identity), A6 (message injection and queue), A8 (snapshot
and restore), F3 (terminology), F5/F6 (runtime/process prerequisites), F7
(`first-project` Codex requirement), F9 (teardown after failed snapshot), S1
(wrapping native agents), S2 (continuity), S4 (managed hooks), and S5
(restore). These are leads, not `yes`/`part`/`no` results. The remaining
claims and each claim's evidence register require the claim-by-claim method
in `04-review-method.md`; no verdicts were written and no tests were run.

Open questions the repository does not answer:

- Is the first supported team a two-agent mixed Claude/Codex pair, a single
  harness pair, or a more flexible choice? Root README states both harnesses
  are in scope; the first-project example is Codex-only.
- Does the owner need the durable queue, or would direct terminal messaging
  plus a repository task file cover the same job? Source establishes the
  queue's guarantees, not its value to this owner.
- Which surfaces earn their runtime and learning cost: TUI, web UI, MCP,
  cross-host, context/spec library, workflow, scope, plugin system?
- What is the minimum restore promise: preserve project files, resume native
  agent conversation when possible, or both? Current source distinguishes
  these, while the root README's WIP safety statement does not define the
  acceptance test.
- Should the existing Node daemon/data model stay behind a new CLI, or should
  Rust own the daemon and a migration path for the existing SQLite state?
- Is `docs/as-built/` to be re-verified, replaced by a smaller map, or removed?
