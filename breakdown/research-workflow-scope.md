# Research workflow scope — CAP-1 source reconciliation

## Boundary and provenance

This document supplies the source-reconciliation part of CAP-1. It does not
select retained workflows, answer owner questions, approve an architecture,
or complete CAP-1's owner-dependent scope choice. All five core journeys below
are candidates. Existing mechanisms are evidence, not reasons to retain them.
No installation, agent operation, recovery experiment, or product test was run.
No shared register, claim verdict, T1–T9 status, or Gates A–D status is changed.

Three immutable revisions distinguish the inputs:

- **S** — `aa51d3581825ce286d9dc2b379e21bee92a2a1c6`: inspected task checkout,
  root requirements, open questions, existing research, and implementation.
- **I** — `f287de77243bf81c974915f1c2a2f69136d35330`: inspected capability
  inventory from local Git history. The inventory file is absent from S;
  CAP-1 is at I, line 75, not an invented section of S.
- **P** — `9db3ed6c406be5c3d9a84720383fcf6b543169e6`: publication branch base
  (`origin/main` when this document was prepared). Publishing this file does
  not import the other research changes between P and S or the inventory at I.

The evidence catalogue gives absolute inspection paths and exact inclusive
line ranges. **The revision label is authoritative**, not the current contents
at that path. Inspect a historical citation with `git show <revision>:<path>`
using its repository-relative suffix. I was read with `git show`, not from a
present worktree file. Some research citations therefore need local Git objects
until those separate research branches are published; no link to a nonexistent
file on main is presented as current evidence.

### Evidence labels

- **Requirement:** explicit derivative intent, not proof of implementation.
- **Observation:** source mechanism read at S; documentation/help is labeled
  stated intent rather than execution evidence.
- **Assumption:** provisional connection between an outcome and a mechanism;
  an alternative is given. It is not an owner answer.
- **Unresolved:** an owner decision or evidence gap remains. Owner questions
  and runtime verification gaps are separate; neither can settle the other.

## Evidence catalogue

| ID | Revision, exact file/lines | What it establishes and limits |
|---|---|---|
| E1 | I — `/home/kkk/.cline/worktrees/7aa89/openrig-breakdown/breakdown/research-capability-inventory.md:43-57,73-89,96-111` | Capability names, CAP-1 deliverable and authority boundary, remaining scope and evidence gaps. Coverage does not establish value or retention. |
| E2 | S — `/home/kkk/.cline/worktrees/7aa89/openrig-breakdown/README.md:3-6,23-34` | Personal Claude/Codex team; less to learn, fewer moving parts, human operation, no loss of uncommitted work on stop, independence from upstream services. |
| E3 | S — `/home/kkk/.cline/worktrees/7aa89/openrig-breakdown/README.md:14-21,50-66,68-80` | Describes current upstream-derived implementation, coexistence warning, side effects and build prerequisites. These are not blanket retention requirements. |
| E4 | S — `/home/kkk/.cline/worktrees/7aa89/openrig-breakdown/breakdown/06-open-questions.md:3-15,17-41` | Existing README answers, remaining questions 1–7 and N1–N10, and required decision-record fields. |
| E5 | S — `/home/kkk/.cline/worktrees/7aa89/openrig-breakdown/breakdown/01-goal-and-scope.md:15-29` | Fully Rust retained functionality and full project Node removal are firm destination requirements, not approved architecture or completed migration. |
| E6 | S — `/home/kkk/.cline/worktrees/7aa89/openrig-breakdown/breakdown/09-adversarial-review-and-research-charter.md:40-65,154-178` | Five provisional normal/failure journeys, evidence discipline and unchanged research gates. |
| E7 | S — `/home/kkk/.cline/worktrees/7aa89/openrig-breakdown/breakdown/08-current-state-evidence.md:153-205,222-281` | Existing candidate spine and selected queue/recovery research. Its preservation inference is not a demonstrated no-loss contract. |
| E8 | S — `/home/kkk/.cline/worktrees/7aa89/openrig-breakdown/docs/reference/getting-started.md:54-80,90-126` | Upstream guide's setup/launch, readiness, assignment and inspection intent; not owner-selected workflow or observed success. |
| E9 | S — `/home/kkk/.cline/worktrees/7aa89/openrig-breakdown/packages/cli/src/commands/setup.ts:586-624` | Managed tmux configuration writes and warning/verification reporting in source. |
| E10 | S — `/home/kkk/.cline/worktrees/7aa89/openrig-breakdown/packages/cli/src/commands/up.ts:59-85,112-122` | Current launch options include project cwd, plan, existing rig and explicit fresh seats; remote dispatch sends these to the API. Not a full launch trace. |
| E11 | S — `/home/kkk/.cline/worktrees/7aa89/openrig-breakdown/packages/cli/src/commands/send.ts:252-272` | CLI help states prompt guards and that pane verification is not agent acknowledgement; this is stated interface intent, not a tested delivery guarantee. |
| E12 | S — `/home/kkk/.cline/worktrees/7aa89/openrig-breakdown/packages/daemon/src/domain/queue-repository.ts:1165-1198` | Post-commit nudge method skips disabled/unavailable transport and records a wake result; its comment distinguishes notification failure from queue mutation. |
| E13 | S — `/home/kkk/.cline/worktrees/7aa89/openrig-breakdown/packages/cli/src/commands/status.ts:43-86,100-114` | Distinct stopped/stale/unhealthy paths, API summaries and kernel-readiness display; not proof that all displayed state matches live agents. |
| E14 | S — `/home/kkk/.cline/worktrees/7aa89/openrig-breakdown/packages/daemon/src/domain/rig-teardown.ts:103-135` | Snapshot failure is recorded but teardown continues; session kill results control per-node cleanup. Delegated cleanup needs further tracing. |
| E15 | S — `/home/kkk/.cline/worktrees/7aa89/openrig-breakdown/packages/daemon/src/domain/restore-orchestrator.ts:976-1006` | A prior resumable session with no token and no explicit fresh request returns `awaiting-decision`; not a guarantee of exact conversation recovery. |

## Cited candidate scope table

Capability names below map to the inventory's named rows, not fabricated CAP
numbers: CAP-1 is a research task, not a capability identifier (E1).

| Core candidate journey / affected capabilities | Explicit requirements | Source observations and stated intent | Assumptions, with alternative | Unanswered owner decisions / evidence limits |
|---|---|---|---|---|
| **Prepare/start** — command-line entry/setup; daemon lifecycle; local configuration/install; packaging | A person can set up, run and fix it; less to learn; no upstream-service dependency (E2). Full project Node removal includes install/build/test/release (E5). | Guide recommends inspecting setup changes, checking prerequisites, previewing and planning (E8). Setup source writes a managed tmux block and reports warnings (E9). README warns about command/state collisions and external file writes (E3). | A local existing-project setup is a useful research anchor (E6), not an approved exclusive operating mode. Alternative: a packaged setup or a different service lifecycle. | Q2, Q4, Q6, N1, N4, N5, N7–N10, W1, W7. Supported platforms, prerequisites, permitted writes and daemon retention remain unresolved. No clean install was demonstrated. |
| **Launch agents** — runtime integration; topology/specification; project/workspace lifecycle | Run several Claude Code and Codex CLI agents as a team (E2); this does not specify an exact team size or require both in every launch. | Guide launch path uses a starter, cwd override and separate seat-readiness checks (E8). CLI exposes spec/library/bundle launch, plan, cwd, existing and fresh options (E10). Existing research identifies the example as a Codex pair, not a mixed team (E7). | Small team in an existing project is provisional (E6). Alternative: a different team shape or isolation policy while retaining support for both required agents. YAML, tmux and current topology vocabulary are not automatically required. | Q3-extension, Q4, N3–N6, N8, N9, W1, W2. Owner must choose team/isolation and launch acceptance needs. Authentication, trust prompts and partial startup remain research cases, not validated behavior. |
| **Assign/follow work** — queue/handoff/messaging; persistence; runtime integration | The product is a team harness (E2). No root requirement explicitly mandates a durable queue, handoff lineage or the upstream review process. | Guide gives a bounded request via send, then uses queue rows/transitions and artifacts (E8). Send help distinguishes pane delivery from acknowledgement (E11). Queue source separates post-commit nudge handling from stored work (E12); existing research describes ownership and handoff (E7). | Durable assignment may support the desired team outcome. Alternative: direct messaging with less coordination state if the owner accepts its limits. Neither option is selected. | N2, N3, W3, W4, W6. Required assignment, persistence, retries, handoff and review guarantees are unresolved. Delivery is not acceptance or completion; no work was assigned in this research. |
| **Inspect** — status/diagnostics/presentation; CLI/TUI/UI/MCP consumers; documentation | Human-readable operation and recovery, less terminology and cognitive load (E2). No specific presentation surface is required by these goals. | Status source separates daemon health and kernel readiness and reports summary failures (E13). Guide requires checking seat readiness, queue transitions and the reviewed artifact, not only message delivery (E8). Inventory locates separate presentation surfaces without approving them (E1). | Status plus durable task history may be sufficient. Alternative: terminal-only inspection, a TUI, browser or other client chosen for owner value. A current UI is not retained by default. | Q5, N2, W4, W5, W6. Required information, review evidence and interface remain unresolved. No usability measure or live-state consistency result is claimed. |
| **Stop/resume** — shutdown/recovery; snapshots/persistence; project/workspace lifecycle; runtime integration | Stopping must never lose uncommitted work (E2). Exact conversation, process or identity continuity is not separately promised by the README. | Teardown tries a snapshot, continues on snapshot failure, then kills sessions and calls cleanup (E14). Restore can stop for an explicit fresh-start decision (E15). Existing research distinguishes checkout files from resumable conversation state (E7). | Returning later is a provisional journey (E6). Alternative: preserved project/task data with deliberate fresh agents rather than exact conversation continuity, only if the owner accepts it. No inferred permission for fresh state. | N3, N4, N6–N9, W2, W7, W8. Required resume/rollback semantics and interruption cases remain unresolved. Dirty/untracked files, nested repositories, delegated cleanup and failed snapshots need deeper traces and separately authorized runtime proof. |

## Reconciliation, not new owner answers

1. **Audience and agents are already stated.** E2 answers the original personal-use
   and Claude/Codex questions. E4 asks only whether those requirements must be
   extended. Do not reopen them as blank choices or drop an adapter silently.
2. **Current design is not target scope.** E3 describes Node, daemon, SQLite,
   tmux, YAML, queue and TUI. E2 says functions must earn their cost; E6 forbids
   treating existing abstractions as necessary. Retention remains unresolved.
3. **The starter is not the requirement.** E7/E8's Codex-oriented example does
   not supersede support for Claude and Codex in E2. Team composition is W1.
4. **Setup conflicts with the destination, not with the existence of the goal.**
   E3's current Node prerequisites and upstream download warning coexist with
   E2's independence goal and E5's full Node-removal destination. They describe
   work still needed, not approved permanent exceptions. N1 and N5–N10 remain open.
5. **Safe stop is stronger than a snapshot attempt.** E14 does not prove E2's
   no-loss requirement. E15 does not guarantee resumability. E7's filesystem
   preservation statement is explicitly an inference; it cannot replace cleanup
   tracing and failure evidence. Owner clarification W8 must not weaken E2 by default.
6. **Publication revision differs from research revision.** P lacks the later
   question reconciliation and CAP-1 inventory used here. E4/E5/I are explicitly
   pinned inputs, not silent updates to main's shared documents. Their status
   and integration remain outside this file's scope.

## Unanswered owner decisions and affected capabilities

**Every row below remains Unresolved.** These are local cross-references, not
edits to the shared question register. Q1-extension and Q3-extension are
conditional scope-change questions; the existing audience and two-agent support
remain requirements without a new answer. Repository-wide choices are included
even when they do not select a daily journey.

### Existing questions — complete carry-forward

| ID | Owner question still open | Affected capabilities / journeys | Exact source |
|---|---|---|---|
| Q1-extension | Is wider distribution now required beyond personal use? | Packaging, documentation, installation; prepare/start. | E4, line 9; E2, lines 3–6. |
| Q2 | Will the tool be published, for example on npm? | Naming, licensing, packaging/install; prepare/start. Publication does not authorize retaining npm against E5. | E4, line 10. |
| Q3-extension | Is any adapter beyond Claude Code and Codex CLI required? | Runtime integration, optional runners; launch, assign, inspect, resume. | E4, line 11; E2, lines 3–4. |
| Q4 | Must tmux remain? | Terminal/runtime integration and daemon lifecycle; launch, inspect, stop/resume. | E4, line 12. |
| Q5 | Keep the upstream docs folder? | Documentation/skills and maintenance; all journeys' instructions. | E4, line 13. |
| Q6 | Keep the upstream CLI installation at version 0.5.15? | Local configuration, naming/state coexistence; prepare/start and recovery. | E4, line 14. |
| Q7 | Keep upstream Git history? | Repository provenance/maintenance; no direct daily workflow selection. | E4, line 15. |
| N1 | Which runtime/install milestones precede full build/test/release Node elimination? Is any permanent exception proposed as an explicit goal change? | Packaging, install, build/test/generation/release; cross-cutting transition, not workflow retention. | E4, line 28. |
| N2 | Is browser JavaScript acceptable? Which CLI, terminal UI, browser UI and MCP surfaces must remain? May UI be optional or prebuilt during transition? | Presentation, MCP, packaging; prepare, assign, inspect and lifecycle control. | E4, line 29. |
| N3 | Must existing SQLite data, configuration, resume identities and hooks migrate? Is explicit fresh-state mode acceptable? What rollback is required? | Persistence, managed config, queue, runtime identity; all journeys, especially stop/resume. | E4, line 30. |
| N4 | Which operating systems and terminal providers are required? Is the daemon essential, optional or removable if workflows remain safe? | Host service, runtime integration, packaging; all journeys. | E4, line 31. |
| N5 | Are vendor runtimes/installers outside the Node-free promise? Must setup avoid npm even for external tools? | Agent setup, installation, external dependency boundary; prepare/start and launch. | E4, line 32. |
| N6 | Is Pi needed? Which stub/testbed, upgrade and recovery paths must remain? | Optional runners, test-system, maintenance and recovery; prepare, launch and stop/resume. | E4, line 33. |
| N7 | What coexistence, renamed commands/state paths, offline assets and distribution format are required? | Packaging, local configuration, state migration; prepare/start and stop/resume. | E4, line 34. |
| N8 | Are tmux, Claude Code and Codex CLI allowed external non-Rust tools? Are Docker and Git allowed in runtime/build/test, and in which roles? | Runtime integration, workspace, testbed, packaging; all journeys and tooling. No test dependency automatically becomes a runtime requirement. | E4, line 35. |
| N9 | Are Node-free shell entrypoints allowed? May Rust link native SQLite/platform libraries and require system packages? | Entrypoints, persistence, packaging; prepare/start, stored work and recovery. | E4, line 36. |
| N10 | Are provider-managed JavaScript CI actions allowed? Is Node forbidden in project commands/artifacts only, or throughout the CI host? | CI/build/test/release; repository-wide delivery boundary, not a daily journey requirement. | E4, line 37. |

### Research-derived workflow questions — not additional requirements

These questions expose decisions implicit in the provisional scenario. They do
not claim that the source already contains an owner request for each mechanism.

| ID | Unresolved owner question | Affected capabilities / journeys | Evidence that exposes the choice |
|---|---|---|---|
| W1 | Which actual project/team scenario should define daily scope: team size, roles, runtime mix per run, number of projects and local versus remote operation? | Runtime integration, topology/workspace and host service; prepare/start and launch. | E1, lines 45–48; E6, lines 42–49; E8, lines 97–100. |
| W2 | What working-directory/isolation model is needed: shared checkout or separate worktrees/branches, and what launch/cleanup rules apply to existing local changes? | Project/workspace lifecycle and runtime integration; launch and stop/resume. | E1, line 48; E6, lines 49,52; E10, lines 79–85. |
| W3 | Is direct messaging sufficient, or must assignment be durable with ownership, handoff/history, retry and duplicate handling? | Messaging, queue/handoff and persistence; assign/follow and resume. | E1, line 49; E7, lines 222–246; E11–E12. |
| W4 | What counts as accepted work and completed work? Is independent review required, and what result/artifact evidence must the owner see? | Work coordination, diagnostics and presentation; assign/follow and inspect. | E8, lines 102–126; E11, lines 263–264. |
| W5 | What minimum health, progress, logs, errors and recovery guidance must be visible, and through which N2-selected interface? | Status/observability, presentation and documentation; inspect and recovery. | E1, line 50; E6, line 51; E13. |
| W6 | Which adjacent capabilities have owner value: MCP, gateway/Slack, plugins/context/spec machinery, automated workflows or remote operation? Which may be omitted? | Optional integrations, documentation/context, topology and consumers; cross-cutting. No removal or retention is selected. | E1, lines 48,53–57,78,96–111; E6, line 44. |
| W7 | Which machine/project configuration changes and cleanup behavior are acceptable, and which upgrade/recovery paths must remain human-operable? | Local configuration/install, hooks and maintenance; prepare/start and stop/resume. | E2, lines 29–34; E3, lines 59–66; E9; N3/N6/N7. |
| W8 | Beyond mandatory preservation of uncommitted work, must return preserve task history, agent identity, conversation, transcripts or process state? What interruption cases and explicit fresh-start/rollback behavior are required? | Persistence, snapshots, runtime identity and workspace; stop/resume and inspect. | E2, lines 31–32; E6, line 52; E7, lines 248–281; E14–E15; N3. |

Later answers need owner, date, rationale, affected scope and acceptance test
(E4, lines 39–41). Approval to write, publish or merge this research is not an
answer to any product-scope question above.

## Completion and remaining limits

- All five core candidate journeys have explicit requirements or an explicit
  absence of a narrower requirement, cited source evidence, assumptions with
  alternatives, and unresolved owner choices.
- All seven original question rows and N1–N10 are reconciled; already-stated
  answers are distinguished from conditional extensions. W1–W8 expose remaining
  workflow choices without changing the shared register.
- Source reconciliation is supplied. Owner-approved retained scope is not.
  No workflow is retained solely because an implementation or guide exists.
- Deeper normal/failure traces, delegated cleanup/state ownership and optional
  consumer/value analysis remain separate CAP-2–CAP-4 research (E1, lines 76–78).
  Runtime preservation and usability remain unverified, not owner decisions
  that this document can infer. No Gate A–D or T1–T9 status is advanced.