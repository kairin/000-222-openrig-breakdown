# Owner decision brief: Rust and Node-removal boundaries

## Status, authority and evidence limits

Source-research deliverable for the independent replacement of task 4f86a,
prepared on 2026-09-27. Research is complete for this brief; **N1–N10 remain
unanswered**. No owner answer, architecture approval, claim verdict, T5
completion, T6–T9 unblock, or Gate A–D completion is asserted.

The question set was inspected at commit
`aa51d3581825ce286d9dc2b379e21bee92a2a1c6` (revision **A**). The publishing
branch starts at GitHub main commit
`9db3ed6c406be5c3d9a84720383fcf6b543169e6` (revision **B**) to avoid publishing
unrelated local changes. The N1–N10 wording and planning documents cited here
belong to A; they must not be interpreted as citations to B's older documents.
All application, packaging and CI sources cited below are byte-identical
between A and B. Both revisions are exact Git object IDs, not moving branches.

The requested destination is fully Rust retained project functionality and
full project Node removal, including installation, update, generation, build,
test and release paths [E01]. Runtime-only removal is an intermediate milestone.
The personal Claude Code/Codex CLI workflow, understandable operation,
preservation of uncommitted work and independence from upstream services remain
goals [E02]. Reducing scope is an option; silently dropping a required agent or
silently accepting a permanent Node dependency is not.

Evidence labels:

- **Observed in source:** a mechanism is present; successful execution is not proved.
- **Stated intent:** a requirement or documented contract, not measured behavior.
- **Inferred:** a conditional consequence or tradeoff, not an observed outcome.
- **Unresolved:** missing verification or a choice reserved for the owner.

No application, agent, installer, migration, recovery helper, test, build or
documentation script was executed. No vendor runtime or CI action internals
were independently verified. Git and publication operations are not product
runtime evidence. The options and consequences below are **inferences** from
the cited facts and stated goals. Every rubric is a **recommendation, not a
decision**. Owner silence grants no exception. Research can finish without an
owner response; affected downstream choices remain open.

## Decision matrix

| Question | Cited facts and limits | Options and tradeoffs against the goals | Exact owner-only decision | Consequence if unanswered | Recommendation — not a decision |
|---|---|---|---|---|---|
| **N1 — language/tool boundary and milestones** | **Stated intent:** full Rust retained functionality and full project Node elimination [E01]. **Observed:** root builds/tests/generators use npm, Node and TypeScript; the CLI has a Node postinstall check and bundles the daemon [E03, E04]. | Runtime/install first can reduce operator dependencies sooner, but leaves two toolchains and needs an exit condition. Replacing tooling first reduces maintenance dependencies but delays workflow benefits. A coordinated cutover avoids a long mixed period but increases migration and rollback risk. A Rust launcher over Node cannot meet the destination. | Specify the ordered runtime, install/update and full build/test/generation/release milestones, the permitted temporary dependencies and their exit conditions. Explicitly identify any permanent exception as a change to the goal. | No approved transition boundary or final acceptance definition; comparisons may continue, but a mixed system cannot be called complete. | Require a finite path from each temporary dependency to replacement or retirement. Compare effort and reversibility using the same Claude/Codex workflow. Do not invent dates or allow a permanent Node dependency by default. |
| **N2 — CLI, TUI, browser UI and MCP** | **Observed:** CLI packaging includes terminal and browser assets; the UI uses React and TypeScript/Vite/Vitest; the daemon serves a built bundle; MCP wraps the daemon API [E04–E07]. Browser JavaScript and Node hosting/build tools are distinct boundaries. | CLI-only reduces surface area but may lose valued visibility and MCP automation. CLI plus Rust TUI/MCP adds contract work but can preserve personal workflows. Optional/prebuilt browser UI can reduce operator setup during transition, but does not eliminate its Node build pipeline or settle the non-Rust functionality exception. Removing UI avoids those obligations only if its workflow value is not required. | Mark each of CLI, TUI, browser UI and MCP required, optional or removable; state whether browser JavaScript is allowed, including any generated browser/glue code boundary; define acceptable transitional prebuilt/optional UI arrangements. | Consumer contracts, UI hosting/build obligations and the meaning of retained functionality cannot be finalized. | Keep a surface only for a named owner action that another retained surface does not adequately cover. Evaluate MCP separately from human CLI use. Require explicit language exceptions and a non-Node production/build path for any retained browser option. |
| **N3 — state migration and rollback** | **Observed:** SQLite migrations are name-ordered and individually transactional; Claude hooks have exact ownership rules and preserve unparseable settings; Codex resume uses a stored token; teardown continues after snapshot failure [E08–E11]. These mechanisms do not prove migration, rollback or work preservation. | Compatible migration retains history and identities but requires version/ownership mapping and interrupted-migration recovery. Fresh state in an isolated namespace reduces conversion work but loses continuity unless old state and agent data remain accessible. Export/import may narrow coupling but can omit identities or hook ownership. Keeping old binaries is insufficient if shared state is mutated incompatibly. | Specify which database records, configuration, Claude/Codex resume identities, transcripts and installed hooks must carry over; whether an explicit fresh-state mode is acceptable; and the rollback duration, recovery point and treatment of work created after cutover. | No state ownership transfer, destructive cleanup or backward-compatibility claim can be accepted; migration design remains conditional. | Prefer reversible operations on copies and isolated state until later proof exists. Separate preservation of dirty/untracked/index work from conversation continuity and database recovery. Require a later interrupted-migration/rollback contract that also preserves user-authored configuration. |
| **N4 — OS, terminals and daemon** | **Observed:** setup has a macOS Homebrew path; the terminal-provider interface renders composed shell commands and distinguishes absent/degraded seats; CLI spawns a detached daemon; teardown owns tmux session stopping [E11–E14]. This is not a verified OS support matrix. | One primary OS/provider reduces maintenance but narrows use. Multiple providers improve choice at a compatibility cost. Retaining a Rust daemon can keep shared state/process ownership clear; an optional or daemonless design removes a service only if locking, observation, lifetime and recovery responsibilities are safely reassigned. Terminal presentation providers are not automatically replacements for tmux session ownership. | Name required OS/architecture targets and terminal providers, whether tmux is mandatory, and whether the daemon must be retained, may be optional, or may be removed subject to explicit safety contracts. | Packaging targets and process ownership cannot be chosen; neither macOS-only support nor daemon removal is authorized. | Start from the owner's actual machines and required start/inspect/stop/resume actions. Compare process models by recovery burden and work preservation, not executable count. Treat all support beyond inspected source as unverified until later target-specific proof. |
| **N5 — vendor installation/runtime boundary** | **Observed:** setup probes installed Claude/Codex commands and otherwise invokes npm installation; it separately checks Claude authentication [E15]. **Unresolved:** current vendor-native installers, bundled runtimes and their supported targets. | External preinstalled tools keep project setup small but shift provisioning to the owner. Project-managed non-npm provisioning can simplify first use but adds supply-chain/update responsibilities. Allowing vendor npm/runtime dependencies narrows the promise to the project; it cannot support a wholly Node-free-host claim. A project-authored npm invocation is not automatically outside project tooling just because its target is external. | Decide whether vendor-owned runtimes/installers are outside the promise, whether project setup may invoke npm for them, and whether the project provisions tools or only verifies owner-provided installations. | Installation acceptance remains undefined; neither a native vendor installation route nor a host-wide Node-free claim is justified. | Separate project commands from vendor internals explicitly. Prefer a clear readiness contract and independently verify any recommended vendor route before promising support. Preserve both required agents rather than letting installer convenience reduce scope. |
| **N6 — Pi, testbed, upgrade and recovery** | **Observed:** Pi and stub launch builders invoke Node; the Docker testbed installs Node/npm and stages stub assets; shipped upgrade instructions invoke Node inspection, SQLite backup and plugin-refresh helpers [E16–E18]. **Stated intent:** Claude/Codex, not Pi, are the required agents [E02]. | Retiring Pi narrows scope but requires an explicit retained-capability choice. Keeping Pi adds a runner/integration port. Replacing stub/testbed mechanisms preserves repeatable validation at a cost; deleting them without replacement loses evidence capability. Recovery helpers can be consolidated or replaced, but their backup/repair value does not disappear when Node files are removed. | Decide whether Pi remains; identify required stub simulations/testbed scenarios and upgrade, backup, repair and recovery capabilities, separately from whether their current implementations survive. | Optional runners cannot silently be retained as exceptions or removed as irrelevant; full tooling removal and recovery acceptance stay unresolved. | Retain capabilities for the personal workflow and credible no-loss verification, not upstream feature parity. Recommend removing unneeded Pi scope only after owner confirmation, while preserving equivalent safety checks for retained Claude/Codex behavior. |
| **N7 — coexistence, offline assets and distribution** | **Stated intent:** README warns that upstream and derivative share command/package/state names and prohibits installing them together; upstream services must not be required [E02]. **Observed:** packaging assembles daemon/UI/TUI, uses Node/npm and stages a bundled daemon; smoke instructions isolate home/database/port [E19, E20]. Those scripts were not run and do not prove fully isolated or offline operation. | A separate native distribution and namespace can permit safer parallel evaluation but needs migration and asset ownership rules. Replacement-only cutover reduces duplicate installs but tightens rollback constraints. Local source builds avoid publishing overhead but still must meet full build-tool removal. Bundled offline assets improve independence but add update/license/artifact obligations; independence from upstream services is not automatically zero network use. | Specify required upstream coexistence, command/package/state/config/endpoint isolation, offline operations and assets, target distribution formats, and whether wider publication is required beyond personal use. | A cutover/package cannot promise collision-free coexistence or offline completeness; current warnings remain applicable. | Prefer isolated evaluation and explicit names before installation beside upstream. Minimize distribution formats for the actual owner targets. Define offline acceptance per operation and review generated assets/hooks as well as binaries; do not infer permission to overwrite shared state. |
| **N8 — external non-Rust tools** | **Stated intent:** both Claude/Codex remain required [E02]. **Observed:** Codex is launched through tmux; the testbed installs Git, tmux and native build tools [E10, E17]. A testbed dependency does not establish a product runtime dependency. | A role-specific external allowlist can avoid expensive reimplementation while keeping the harness Rust. Banning every non-Rust tool expands scope beyond project code and may conflict with required vendor workflows. Keeping Docker for tests need not require Docker in daily operation. Git use for build metadata is distinct from runtime worktree mutation. | For tmux, Claude Code, Codex CLI, Docker and Git, mark allowed/required/forbidden separately for product runtime, setup/update, build, test and release, including any vendor-runtime exceptions linked to N5. | No external-tool acceptance boundary exists; dependencies cannot be grandfathered from upstream or inferred from the language goal. | Recommend the smallest role-specific allowlist consistent with the real workflow. Require each tool to justify its owner value and failure/recovery contract; scrutinize any Git/worktree operation that could affect unfinished work. Do not equate Rust project code with a pure-Rust host. |
| **N9 — shell glue and native dependencies** | **Observed:** the shell packaging and smoke paths invoke npm/Node; postinstall loads the better-sqlite3 native addon [E04, E19–E21]. These are examples of nested runtime obligations, not evidence that Rust must use a specific SQLite library. | Rust entrypoints reduce language-boundary ambiguity but may require more platform code. Node-free shell glue can simplify packaging yet remains non-Rust project logic unless approved. Native SQLite/platform linkage can preserve useful behavior but creates ABI, licensing and distribution obligations. Pure-Rust dependencies or static linking may reduce some host packages but are separate requirements, not guaranteed simplicity. | Define permitted shell entrypoints and their allowed responsibilities; whether native libraries are allowed; static/dynamic linkage expectations; and permitted target system packages/toolchains. | “Fully Rust” cannot be used to resolve implementation-language versus dependency-language boundaries; packaging prerequisites remain undecided. | Recommend explicit, narrow glue/native exceptions only where they reduce total operating burden. Inspect all invoked commands and shipped helpers. Require later clean-target artifact checks; a shell extension or absent Node shebang is not removal proof. |
| **N10 — CI actions and host scope** | **Observed:** the workflow selects checkout@v4 and setup-node@v4, requests Node 22 and runs the project's portability-report script [E22]. **Unresolved:** selected action runtime internals and the complete hosted-runner environment. | Allowing provider-managed JavaScript actions outside the project boundary reduces CI maintenance, but is an explicit exception and not a Node-free-host promise. Replacing project commands is necessary even with that exception. A host-wide ban requires control/audit of actions and runner images and may demand custom CI infrastructure; fewer project dependencies do not prove that host property. | Decide whether provider-managed JavaScript actions are allowed, whether Node is forbidden in project commands/artifacts or throughout the CI host, and how nested/composite action execution is classified and verified. | CI cannot be declared compliant merely by deleting setup-node or porting the report; acceptance scope remains open. | Recommend separating project-tooling elimination from provider internals, then recording any provider exception explicitly. Verify exact action revisions and their execution graph before asserting their runtime. Evaluate a whole-host ban against maintenance cost without silently weakening it. |

## Cross-question rubric and later answer record

**Recommendation, not a decision:** evaluate options against the same personal
workflow: start Claude/Codex agents in an existing project, assign and inspect
work, stop, and resume without losing unfinished changes. Use these criteria
in order of discussion, without invented numeric weights:

1. Does the option meet the Rust/Node destination, or require a named goal change?
2. Does it preserve required agent behavior, project work and user-owned settings?
3. Does it reduce setup, daily concepts, maintenance and recovery effort?
4. Can the transition be reversed, including new work created after cutover?
5. What evidence would disprove the expected benefit, and what later acceptance
   check would establish the promised behavior?

Discuss N1 with N2/N5/N8/N9/N10 to avoid conflicting definitions of “project”
and “fully Rust.” Discuss N3 with N4/N7 before changing process or state ownership.
Discuss N6 with N1 so deleting a test runner does not silently delete required
validation. These are decision dependencies, not a new gate/status register.

Each later owner answer should record question ID, owner, date, selected option,
rationale, affected scope, exceptions and acceptance test [E01]. Approval to
publish or merge this research is not an answer to any of those questions.
Unanswered choices prevent the affected downstream commitment, not completion
of this bounded research. No new runtime safety evidence is claimed.

## Evidence ledger

Every citation below is pinned to revision **A** =
`aa51d3581825ce286d9dc2b379e21bee92a2a1c6`. Paths are the absolute inspected
checkout paths; `:start-end` denotes inclusive lines. For another checkout,
remove `/home/kkk/.cline/worktrees/55605/openrig-breakdown/` and inspect that
repository path at A. Product source entries E02–E22 also exist unchanged at
revision B, which is available on the publication base. E01 specifically
describes the newer local question set and is not a claim about B's contents.

- **E01 — stated intent / owner authority.** `/home/kkk/.cline/worktrees/55605/openrig-breakdown/breakdown/06-open-questions.md:17-41` (N1–N10 and later answer requirements); `/home/kkk/.cline/worktrees/55605/openrig-breakdown/breakdown/01-goal-and-scope.md:15-29` (destination and retained goals); `/home/kkk/.cline/worktrees/55605/openrig-breakdown/breakdown/09-adversarial-review-and-research-charter.md:108-132` (comparison rubric and conditional candidates).
- **E02 — stated intent.** `/home/kkk/.cline/worktrees/55605/openrig-breakdown/README.md:3-6,17-34` (audience, agents, collisions, work preservation and upstream independence).
- **E03 — observed in source.** `/home/kkk/.cline/worktrees/55605/openrig-breakdown/package.json:7-32` (workspaces, scripts and Node engine).
- **E04 — observed in source.** `/home/kkk/.cline/worktrees/55605/openrig-breakdown/packages/cli/package.json:28-74` (bins, assets, postinstall, dependencies and bundled daemon).
- **E05 — observed in source.** `/home/kkk/.cline/worktrees/55605/openrig-breakdown/packages/ui/package.json:7-54` (UI tooling and React dependencies).
- **E06 — observed in source.** `/home/kkk/.cline/worktrees/55605/openrig-breakdown/packages/daemon/src/server.ts:806-835` (static UI files, missing-bundle handling and SPA fallback).
- **E07 — observed in source.** `/home/kkk/.cline/worktrees/55605/openrig-breakdown/packages/cli/src/mcp-server.ts:1-4,21-55` (SDK, error mapping and daemon-client wrapper).
- **E08 — observed in source.** `/home/kkk/.cline/worktrees/55605/openrig-breakdown/packages/daemon/src/db/migrate.ts:13-42` (applied-migration tracking and per-migration transactions).
- **E09 — observed in source and documented ownership contract.** `/home/kkk/.cline/worktrees/55605/openrig-breakdown/packages/daemon/src/adapters/claude-code-adapter.ts:709-726,729-819` (Node collector, exact managed-hook ownership, validation and preservation of unparseable settings).
- **E10 — observed in source.** `/home/kkk/.cline/worktrees/55605/openrig-breakdown/packages/daemon/src/adapters/codex-runtime-adapter.ts:380-409` (token-specific resume and tmux launch); `:189-200` (Node activity relay command).
- **E11 — observed in source.** `/home/kkk/.cline/worktrees/55605/openrig-breakdown/packages/daemon/src/domain/rig-teardown.ts:24-29,103-143` (immediate stopping, best-effort snapshot and subsequent session/service teardown). This range does not audit every cleanup path.
- **E12 — observed in source.** `/home/kkk/.cline/worktrees/55605/openrig-breakdown/packages/cli/src/commands/setup.ts:332-358` (macOS Homebrew branch and tmux probe).
- **E13 — declared interface / stated contract.** `/home/kkk/.cline/worktrees/55605/openrig-breakdown/packages/daemon/src/domain/terminal/terminal-provider.ts:1-30,63-109` (provider/composition separation, liveness and partial rendering).
- **E14 — observed in source.** `/home/kkk/.cline/worktrees/55605/openrig-breakdown/packages/cli/src/daemon-lifecycle.ts:633-665` (daemon entry, instance initialization and detached child spawn).
- **E15 — observed in source.** `/home/kkk/.cline/worktrees/55605/openrig-breakdown/packages/cli/src/commands/setup.ts:497-559` (existing-tool probes, npm installs and Claude authentication).
- **E16 — observed in source.** `/home/kkk/.cline/worktrees/55605/openrig-breakdown/packages/daemon/src/adapters/pi-runner-protocol.ts:164-190`; `/home/kkk/.cline/worktrees/55605/openrig-breakdown/packages/daemon/src/adapters/stub-runner-protocol.ts:89-110` (Node runner commands and identity options).
- **E17 — observed build instructions, not an executed image.** `/home/kkk/.cline/worktrees/55605/openrig-breakdown/docker/testbed/Dockerfile:12-63` (testbed-only native tools, Node/npm, package install, stubs and entrypoint).
- **E18 — stated operational instructions.** `/home/kkk/.cline/worktrees/55605/openrig-breakdown/packages/daemon/specs/agents/shared/skills/core/openrig-upgrade/SKILL.md:75-125` (inspection, backup and plugin refresh Node helpers; documented backup limitations). Helper implementation guarantees were not tested.
- **E19 — observed build instructions.** `/home/kkk/.cline/worktrees/55605/openrig-breakdown/scripts/build-package.sh:20-43,141-175` (workspace builds, Node staging logic, bundled daemon and UI/TUI copies).
- **E20 — observed smoke instructions, not a passing test.** `/home/kkk/.cline/worktrees/55605/openrig-breakdown/scripts/smoke-fresh-install.sh:35-78` (npm install, dependency probes, isolated home/database/port and Node start/stop).
- **E21 — observed in source.** `/home/kkk/.cline/worktrees/55605/openrig-breakdown/packages/cli/scripts/check-abi.mjs:104-122` (Node-version/native-addon postinstall check).
- **E22 — observed workflow configuration.** `/home/kkk/.cline/worktrees/55605/openrig-breakdown/.github/workflows/portability-report.yml:17-33` (action references, Node setup and project script). This is not evidence of the actions' internal runtimes.

## Completion boundary

The deliverable is ten decision-ready rows with traceable facts, alternatives,
explicit owner choices, unanswered consequences and nonbinding rubrics. It is
not a full dependency SBOM, exhaustive state/cleanup audit, verified platform
matrix, vendor installation guide or Rust architecture selection.

Validation is static: reread the brief, check all ten rows and evidence IDs,
confirm citation ranges and revision identity, inspect whitespace/diff hygiene,
and ensure the only changed file is this brief. No edits to the question set,
shared register, task/status documents or research gates are part of this work.