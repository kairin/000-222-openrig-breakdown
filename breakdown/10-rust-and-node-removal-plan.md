# 10. Rust and Node-removal planning baseline

## Status and evidence rules

This is the documentation deliverable for fa0bb, audited on 2026-09-27 against
source commit `9db3ed6c406be5c3d9a84720383fcf6b543169e6`. It records the requested
Rust and Node-removal direction, not an approved architecture or implementation.
Source citations below are repository-relative `path:line-range` references at
that commit. They describe source, not successful execution. Proposed stages
and acceptance criteria are requirements, not observations.

No application, installation, migration, recovery or agent workflow was run for
this audit. No Rust implementation was added. The skill router could not load
context because the daemon was stopped; it was not started for this work.
The claim register still has 24 unverified entries
(`breakdown/gemini-review/README.md:39-80`). This document does not complete
T2–T9, change claim verdicts, or satisfy Gates A–D. The worktree has no tracked
07 task-list document; this deliverable does not create one or copy main-checkout
task state. Dependent planning remains held for review.

## Goal and boundary

The requested destination is a simpler Rust-owned core and removal of the
project's Node dependency. The existing product goals remain personal use,
Claude Code and Codex CLI support, human-readable operation, preservation of
uncommitted work, and independence from upstream services
(`README.md:3-6,23-34`). A Rust launcher over a Node daemon is an intermediate
option, not completion of Node removal. A smaller required scope can be better
than a line-for-line port. Compare alternatives before selecting the boundary
(`breakdown/09-adversarial-review-and-research-charter.md:108-132,154-164`).

Use these separate acceptance boundaries; the owner must ratify them:

| Boundary | Proposed meaning | What does not count as removal |
|---|---|---|
| Product runtime and installation | Retained project-owned commands, core/daemon, terminal presentation, MCP if retained, hooks, runners, setup, upgrade and recovery operate without Node, npm or npx. | Bundling/renaming Node, using another JS runtime to host the same Node contract, or silently retaining a Node daemon. |
| Build, test and release tooling | Track separately. An interim runtime-only milestone can retain declared Node tooling; full project Node removal also replaces or retires required npm, TypeScript, Vite, Vitest and Node scripts. | Calling the whole repository Node-free while its required build/test/release path still uses Node. |
| Browser UI | Browser JavaScript is not Node. A retained static browser bundle can satisfy runtime-only removal if its server is non-Node. Its current build/test pipeline is a separate obligation. | Assuming React requires Node on the operator machine, or assuming static assets prove a Node-free build. |
| Embedded and generated assets | Project-owned scripts copied to user workspaces, hooks, skills and packaged recovery utilities are inside the product boundary when required by a retained workflow. | Auditing only imports in the main executables. |
| External tools | Claude Code, Codex CLI, terminal providers and other owner-approved tools remain explicit external contracts. Their own packaging/runtime requirements require independent verification. | Claiming the whole host is Node-free, or excluding a project-authored runner merely because it launches externally. |
| Archived material | Historical examples may remain clearly marked non-operational. Active instructions must match supported paths. | Deleting every occurrence of the word “node” (which also names domain entities), or retaining executable legacy instructions without warning. |

## Evidence-backed dependency inventory

This is a direct dependency and execution-surface inventory, not an exhaustive
transitive SBOM or a runtime trace. Manifest placement alone does not establish
where a library executes. Before a removal decision, expand retained paths
through the lockfile, dynamic imports, generated assets and package contents.

| Surface | Observed evidence | Migration obligation / unresolved decision |
|---|---|---|
| Workspace and engines | Four workspaces; root Node `^20 \|\| ^22 \|\| ^24` (`package.json:7-12,30-32`). Daemon has the same range (`packages/daemon/package.json:97-99`), but published CLI says `>=20` (`packages/cli/package.json:72-74`). | Reconcile the differing declared ranges; do not restate them as one universal “20+” requirement. |
| CLI entry and native ABI | Node shebang, Node built-ins, sibling-runtime re-execution and `better-sqlite3` probe (`packages/cli/src/bin-wrapper.ts:1-6,25-79`). Package exports both CLI and TUI bins and runs a Node postinstall check (`packages/cli/package.json:28-45`). | Replace the executable/install/ABI path, not only argument parsing. |
| CLI direct dependencies | 15 entries: Hono Node server/WS adapters, MCP SDK, daemon, Ajv, Ajv formats, better-sqlite3, Commander, Hono, JSONC parser, TOML, tar, ULID, YAML, Zod (`packages/cli/package.json:47-62`). | Decide each retained parsing, validation, archive, ID and protocol contract; no Rust library choices are approved here. |
| Daemon and shared domain imports | CLI bundles daemon; daemon exposes JS/type surfaces (`packages/cli/package.json:64-66`; `packages/daemon/package.json:7-69`). CLI launches daemon with `process.execPath` (`packages/cli/src/daemon-lifecycle.ts:651-660`). Daemon declares 8 direct dependencies: Hono Node server/WS, better-sqlite3, Hono, TOML, tar, ULID, YAML (`packages/daemon/package.json:79-88`). | A new CLI does not sever in-process domain imports or the child runtime. Map imported surfaces and API consumers before cutting over. |
| Persistence | SQLite uses WAL and foreign keys (`packages/daemon/src/db/connection.ts:1-13`); migrations are sorted by name, recorded and applied transactionally (`packages/daemon/src/db/migrate.ts:13-42`). | Decide old-state compatibility versus explicit fresh-state support; preserve recovery and migration ordering if compatibility is promised. |
| TUI | Node entry, child-process import (`packages/tui/src/main.ts:1-25`); YAML runtime dependency and TypeScript/Vitest tooling (`packages/tui/package.json:11-23`). | Retain/replace/remove terminal presentation by owner value. Keeping the current TUI prevents product runtime removal. |
| MCP | SDK/Zod imports and server wrapping the daemon HTTP API (`packages/cli/src/mcp-server.ts:1-4,47-55`). | Preserve protocol/error contracts if retained; not automatically covered by porting the human CLI. |
| Browser UI | 25 dependency entries, including React, Radix, TanStack, xterm, xyflow, fonts and styling; 10 dev dependency entries (`packages/ui/package.json:16-54`). TypeScript/Vite build, Vitest test, tsx capture (`packages/ui/package.json:7-14`). Daemon serves built files and SPA fallback (`packages/daemon/src/server.ts:806-835`). | Distinguish browser assets from Node build/dev/test and current Node hosting. Decide optional UI, removal, or retained non-Node hosting and later build replacement. |
| Managed Claude/Codex hooks | Claude writes a context collector and Node command, and installs a Node activity relay (`packages/daemon/src/adapters/claude-code-adapter.ts:709-719,740-748,780-819`). Codex constructs a Node relay command (`packages/daemon/src/adapters/codex-runtime-adapter.ts:189-200,1143-1145`). | Replace retained hooks and migrate only owned configuration; do not leave old commands behind or remove user-authored hooks. |
| Additional runners | Pi and stub command builders explicitly launch `node` with their runner entry (`packages/daemon/src/adapters/pi-runner-protocol.ts:164-188`; `packages/daemon/src/adapters/stub-runner-protocol.ts:89-108`). | Decide Pi support separately from required Claude/Codex support; replace or retire stub fixtures without losing equivalent validation. |
| Shipped operational scripts | Node shebangs/imports in upgrade inspection, plugin refresh, SQLite backup and telemetry migration (`packages/daemon/specs/agents/shared/skills/core/openrig-upgrade/scripts/inspect-upgrade.mjs:1-3`, `packages/daemon/specs/agents/shared/skills/core/openrig-upgrade/scripts/refresh-managed-plugin.mjs:1-5`, `packages/daemon/specs/agents/shared/skills/core/openrig-upgrade/scripts/backup-sqlite.mjs:1-5`, `packages/daemon/specs/agents/shared/skills/core/openrig-upgrade/scripts/migrate-telemetry-state-0.5.9.mjs:1-6`). | Audit copied skills and recovery paths, not just the core. Replace/remove required scripts and update the instructions that invoke them. |
| Build/test/generation | Root npm workspace builds, Node tests/guards/mirroring/context generation and tsc lint (`package.json:13-28`). Daemon uses tsc, Node copy/generation/evals, tsx and Vitest (`packages/daemon/package.json:71-77,89-95`); CLI has 3 dev dependencies (`packages/cli/package.json:67-70`), TUI 3 (`packages/tui/package.json:16-20`). | Retain as an explicit transitional exception only; replace necessary generators and tests before full project removal. |
| Packaging/CI/testbed | Packaging builds all four workspaces, generates context and stages bundled daemon (`scripts/build-package.sh:20-43,82-100,141-165`). Portability workflow sets up Node 22 (`.github/workflows/portability-report.yml:24-29`). Testbed installs Node/npm then local npm package (`docker/testbed/Dockerfile:26-46`). | Audit artifact contents and CI separately; dropping manifests first would break packaging and checks. |
| External agent setup and process tools | Setup offers npm installation of Claude Code and Codex (`packages/cli/src/commands/setup.ts:504-514,549-559`). tmux adapter uses Node filesystem/temp-file operations (`packages/daemon/src/adapters/tmux.ts:1-28`). | Separate project installation from external agent provisioning. Verify supported non-npm agent installation if required; Rust does not itself remove tmux or external authentication. |

### Reproducible count reconciliation

At the source commit above, `git ls-files` gives 2,364 tracked `.ts`/`.tsx` files
under `packages/`: CLI 352, daemon 1,346, TUI 145, UI 521. There are 57 tracked
files under `scripts/`, 85 numbered TypeScript files in
`packages/daemon/src/db/migrations/`, and no tracked `.rs`, `Cargo.toml` or
`Cargo.lock`. These are repository observations, not performance or complexity
measurements. Five directories under `packages/` are not five npm workspaces:
`test-system` is not in `package.json:7-12`.

Reproduce from the repository root (tracked files only, includes tests, excludes
untracked/ignored build output):

```sh
git ls-files 'packages/*.ts' 'packages/*.tsx' | wc -l
git ls-files 'scripts/*' | wc -l
git ls-files 'packages/daemon/src/db/migrations/[0-9]*.ts' | wc -l
git ls-files '*.rs' '**/Cargo.toml' '**/Cargo.lock' Cargo.toml Cargo.lock
```

The old “approximately 2018” count in the goal document had no predicate.
It is now replaced with this defined count, without asserting code growth. The
2,364/57/85 counts in the earlier evidence report are confirmed by this tracked
file audit. Direct dependency counts above count manifest keys per package,
not unique libraries across packages or transitive/installed modules.

## Dependency-aware stages (conditional, not authorization)

Gates remain as defined in
`breakdown/09-adversarial-review-and-research-charter.md:154-164`. This outline
records ordering constraints for later planning; Gate D must pass before a
concrete implementation plan or implementation is authorized.

| Stage | Prerequisite | Required output / exit condition |
|---|---|---|
| 0. Establish scope and evidence | Current inventory; T2–T5 research remains incomplete | Review claims and linked groups, trace required workflows, reconcile owner decisions in 06, label source versus runtime proof. Gate A is not met merely by this inventory. |
| 1. Choose retained behavior | Stage 0; T6–T7 and Gate B | Value/dependency matrix comparing removal, simplification and preservation; baseline user actions/dependencies/recovery effort; explicit counter-evidence. No mechanical port of all packages. |
| 2. Choose Rust boundary | Stage 1; T8 and Gate C | Compare simpler TypeScript, Rust CLI over old daemon, Rust core/CLI with optional UI, and full replacement against identical workflows. Resolve state, protocol, platform and external-tool obligations. An interim mixed system needs an exit condition. |
| 3. Authorize later implementation planning | Stage 2; T9 and Gate D | Owner/reviewer records scope, compatibility promises, Node-removal tier, unresolved risks and go/no-go. Until then downstream implementation planning remains held. |
| 4. Prove a bounded replacement later | Gate D plus separately approved implementation plan | Start with a retained end-to-end slice, isolate data and commands from upstream, compare outputs/errors and recovery against the old baseline. Define rollback and no-loss tests before changing state ownership. |
| 5. Transfer retained runtime paths later | Stage 4 evidence; agreed contracts | Replace core/state/lifecycle before removing its consumers; migrate CLI/TUI/MCP and hooks/runners with versioned seams. Prove safe restart, retries, stop and ownership cleanup. Old Node components are explicit temporary dependencies, not success. |
| 6. Cut over distribution and tooling later | All retained runtime paths and rollback verified | Replace installer/updater and shipped scripts; decide UI artifact path; replace/retire Node build/test/generation/CI paths for full removal. Inspect archives and generated instructions before deleting old packages/lockfiles. |
| 7. Verify and retire later | Scope-specific acceptance below | Independent clean-environment proof, documented exceptions and tested recovery; remove obsolete code only after consumers and rollback obligations are addressed. |

## Acceptance criteria

### This documentation deliverable

- Goal, boundary, direct dependency/execution inventory, open owner decisions,
  gated order and future acceptance criteria are linked from the index.
- Observations identify a source commit and exact file/line evidence or a
  reproducible count predicate. No source reading is presented as runtime proof.
- Verify every changed document, local links, citation ranges and diff hygiene.
  No claim pages/register, code, task state, or research-gate completion changes.
- Reviewer acceptance of these documents is separate from Gate D approval.

### Future migration, not yet tested or achieved

- Owner ratifies runtime-only versus full project removal, required UI/MCP,
  platforms, old-state support and external-tool exceptions before release.
- On each supported target, install the release in an isolated environment
  without Node/npm/npx available to project-owned processes. Exercise start,
  inspect, send/queue, stop, restart/recovery, retained presentations, hooks,
  upgrade and uninstall. Record process execution, generated files, network
  access and artifact contents; a grep-only audit is insufficient.
- Demonstrate preservation of uncommitted work, user-owned hooks/configuration
  and supported data. Test interrupted migrations, repeated operations, failed
  stop/recovery and rollback on copies, never the owner's sole live database.
- If external tools need Node, prove the project boundary separately and state
  that exception. Do not claim a wholly Node-free host. Verify agent install and
  authentication requirements independently rather than inferring vendor internals.
- A runtime-only milestone explicitly lists remaining build/test/UI tooling.
  Full project removal additionally builds, tests, generates and packages all
  retained artifacts without Node/npm/npx; CI and operational docs match.
- Compare the same required workflows against the baseline: fewer setup and
  maintenance dependencies and no hidden increase in recovery effort. Rust
  language adoption alone is not evidence of a simpler or safer product.

## Documentation verification record

On 2026-09-27 this pass reread the five changed documents (01, 06, 08, 10 and
the breakdown index), checked local link targets and source citation ranges,
and reran the count commands above. Results: 2,364 / 57 / 85, no tracked Rust
files; manifest dependency counts were also recalculated. `git diff --check`
passed. `node scripts/check-docs-guard.mjs` passed, and
`node --test scripts/check-docs-guard.test.mjs` passed all five tests. That guard
checks inherited docs placement, not the truth of migration claims; source
inspection and the explicit evidence limits remain necessary. No full product
suite, installer, daemon or recovery workflow was run. These checks validate
the bounded documentation change, not Node removal or migration readiness.
