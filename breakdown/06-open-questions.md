# 6. Open questions

The owner must answer unresolved questions. Existing statements in the root
README answer some older questions; those answers are not new owner decisions
made by this audit. The answers change the work that follows.

| No. | Question | Why the answer is important |
| :-: | :--- | :--- |
| 1 | Answered in the README: personal use for one person's workflow (`README.md:3-6`). Is wider distribution now required? | Do not reopen the stated audience without a changed requirement. |
| 2 | Will you publish the tool, for example on npm? | If yes, you must change the name and the package name. You must also add the license notices (see 2.4). |
| 3 | Answered in the README: Claude Code and Codex CLI (`README.md:3-4`). Is any additional adapter required? | The current goal includes both. Removing one requires an explicit scope change. |
| 4 | Must the tool continue to use tmux? | tmux is a large part of the design (claim A3). Another method changes the architecture. |
| 5 | Do you keep the upstream `docs/` folder? | These documents become incorrect when the code changes. |
| 6 | Do you keep the upstream installation of `@openrig/cli` 0.5.15? | It uses the same command name and state folder as your version (see 3.2). |
| 7 | Do you keep the upstream Git history? | The history shows the origin of the code. It does not cause a problem if you keep it. |

## Rust and Node-removal decisions

The destination is fully Rust retained project functionality and full project
Node removal. Runtime-only removal is intermediate, not an alternative end goal.
Any non-Rust or Node exception requires an explicit owner decision. The questions below
define its acceptance boundary, not whether an unapproved rewrite is complete.
All remain open. The local research baseline records evidence and conditional
stages in `breakdown/10-rust-and-node-removal-plan.md` at commit `aa51d358`.
That document is not yet integrated into remote main; this update does not
publish or approve the separate research backlog.

| No. | Owner question | Decision needed before |
|---|---|---|
| N1 | Which runtime/install milestones should precede the required full build/test/release Node elimination? Any proposed permanent exception requires an explicit goal change, not a completion claim. | Gate C boundary recommendation and Gate D scope approval. |
| N2 | Is browser JavaScript acceptable? Which of CLI, terminal UI, browser UI and MCP must remain? May UI be optional or prebuilt during transition? | Choosing consumers and UI build/hosting obligations. Current surfaces: `packages/cli/package.json:28-38`; `packages/ui/package.json:7-54`; `packages/cli/src/mcp-server.ts:47-55`. |
| N3 | Must existing SQLite data, configuration, agent resume identities and hook installations migrate? Is an explicit fresh-state mode acceptable? What rollback must remain available? | State ownership transfer. Migration mechanism: `packages/daemon/src/db/migrate.ts:13-42`; hook ownership: `packages/daemon/src/adapters/claude-code-adapter.ts:740-748,780-819`. |
| N4 | Which operating systems and terminal providers are required? Is a daemon essential, optional, or removable if workflows remain safe? | Comparing Rust boundaries at Gate C. This audit does not choose a new process model. |
| N5 | Are vendor-managed agent runtimes/installers outside the Node-free promise? Must setup avoid npm even for external tools? | Installation acceptance. Current setup invokes npm for both agents (`packages/cli/src/commands/setup.ts:504-514,549-559`). Vendor runtime requirements are not verified here. |
| N6 | Is Pi support needed beyond the stated Claude/Codex goal? Which stub/testbed, upgrade and recovery paths must remain? | Retiring Node runners and shipped scripts; inventory in 10 identifies these paths. |
| N7 | What coexistence, renamed command/state paths, offline assets and distribution format are required? | Packaging and cutover. The current README warns of shared upstream commands/state (`README.md:17-21`) and requires no upstream service dependency (`README.md:33-34`). |
| N8 | Are tmux, Claude Code and Codex CLI allowed as external non-Rust tools? Are Docker and Git allowed in runtime, build or test environments, and in which roles? | Explicit external-tool allowlist before Gate C. Agent support remains required (`README.md:3-4`); current testbed installs Git/tmux and native build tools (`docker/testbed/Dockerfile:12-24`). Do not infer that a testbed dependency is a required product runtime dependency. |
| N9 | Are shell entrypoints allowed if they invoke no Node/npm/npx? May retained Rust code link native libraries such as SQLite or platform libraries, and may they require system packages? | Define fully Rust versus pure-Rust dependencies, static/dynamic linkage and permitted packaging glue. Shell without Node can meet Node removal but is not automatically a fully Rust implementation. Current native ABI check: `packages/cli/scripts/check-abi.mjs:104-122`. |
| N10 | Are provider-managed JavaScript CI actions allowed outside the project-controlled tooling boundary? Is Node forbidden only in project commands/artifacts, or throughout the CI host too? | CI acceptance scope. The workflow uses checkout/setup-node actions and explicitly runs a project Node script (`.github/workflows/portability-report.yml:21-32`). Action runtime internals require external verification; no provider-JS exception is assumed. |

Record each later answer with its owner, date, rationale, affected scope and
acceptance test. Do not infer permission from silence. These questions and the
inventory do not finish T5 or unblock T6–T9; claim review and Gates A–D remain.

## Owner response request and decision record

**Documentation date: 2026-09-27. No new product answers recorded.** The owner's
instruction to proceed, commit, push, open a PR and merge authorizes delivery of
this documentation, not answers to N1–N10. Source verification is the agent's
responsibility; the owner is asked for product policy, not to repeat code review.

**Owner: please record your answers here for every high-impact unresolved item.**
Use one record per question (and separate subquestions where answers differ):

| Required field | What the owner must record |
|---|---|
| Question and status | N-number or blocker ID; answered, partially answered, or unanswered. |
| Answer | Explicit choice and any temporary exception, expiry or goal change. |
| Human owner and authority | Name or confirmed handle; product-owner authority or explicit delegation. |
| Decision date | Actual date of the decision, not this documentation date. |
| Rationale | Why this choice fits the required workflow and its tradeoffs. |
| Affected scope | Capabilities, workflows, data, environments and milestones affected. |
| Acceptance test | Observable pass/fail criteria, environment and required evidence; identify unrun tests. |
| Remaining blockers | Unanswered subquestions, named responsible human and affected gates. |

### Human blocker identities

- **H1 — requesting human/product owner:** the human issuing this task and delivery
  authorization. Personal name/confirmed handle has not been supplied. H1 must
  provide that identity and the product decisions below; do not infer identity
  from a filesystem username, GitHub login or agent ID.
- **H2 — runtime authorizer/operator assignment:** not yet named by H1. Needed
  before separately authorized disposable-environment runs, not to verify source.
- **H3 — human acceptance authority, if required:** not yet named by H1. Research
  review remains a separate coordinator/reviewer responsibility; do not invent a
  human sign-off requirement for routine technical verification.

These identifiers make the blockers addressable but are not invented personal
names. A fully named human handoff remains blocked until H1 confirms identities.

### Unanswered decision and gate map

All rows below are **unanswered**, blocked on **H1**. Gate references mean affected
evidence/decision obligations, not that an answer alone passes a gate. Definitions:
[Gates A–D](09-adversarial-review-and-research-charter.md#gates-and-open-decisions).

| Item | Required answer / affected scope | Affected gates |
|---|---|---|
| N1 | Required runtime/install milestones and final fully Rust/full Node-removal criteria covering build, test and release. Distinguish temporary mixed-runtime stages from permanent goal changes. | C, D |
| N2 | Retain/remove/optional decisions for CLI, TUI, browser UI and MCP; browser JavaScript allowance and transitional prebuilt assets. | B, C, D |
| N3 | Existing SQLite, config, hooks and resume-data migration; explicit fresh-state policy; rollback obligations and user-owned configuration preservation. | B, C, D |
| N4 | Supported OS and terminal-provider matrix; daemon required, optional or removable. Current implementation does not select the future operating mode. | B, C, D |
| N5 | Vendor-managed agent runtime/installer boundary; npm allowance for external tools versus project-controlled setup. | C, D |
| N6 | Pi, stub, testbed, fixtures/scenarios, upgrade and recovery scope. Presence in a test image does not make a tool a runtime requirement. | B, C, D |
| N7 | Coexistence with upstream; command/package/state paths; offline asset coverage and distribution/publication format. | B, C, D |
| N8 | Explicit allowlist for tmux, Claude Code, Codex CLI, Docker and Git, separately for runtime, build and test roles. | C, D |
| N9 | Shell entrypoints without Node; native libraries/system packages; pure-Rust versus Rust with native linkage; static/dynamic packaging allowances. | C, D |
| N10 | Provider-managed JavaScript CI actions; project-command/artifact boundary versus host-wide Node prohibition. | C, D |
| H-SCOPE | Confirm required owner journeys and capability value: prepare, launch, assign, inspect, stop/return remain a provisional scenario. Decide relevance of multi-host, workflow automation, gateway/Slack, plugins and generated context, without inferring removal from incomplete research. Extends N2/N6. | A scope inputs; B, C, D |
| H-PRESERVE | Define N3's preservation contract for staged/unstaged tracked edits, untracked/ignored files, nested repositories, Git worktree metadata, external volumes, transcripts and native conversations. State expected behavior after failed snapshots, abrupt stop/reboot, missing resume identities and interrupted cleanup. Repository preservation and conversation resume are separate promises. | A contract inputs; B, C, D |

For each acceptance test, specify the required result rather than marking source
observations as a pass. Examples of test *subjects*, not chosen policy, include
retained-client parity, migration/rollback byte and identity checks, clean install
on the chosen OS/provider matrix, offline asset use, and process/artifact/tooling
audits under the chosen Node boundary. Test implementation and execution are later
work with appropriate authorization.

## Research blockers and evidence provenance

The following inputs were read during planning. They are not silently treated as
accepted, integrated or runtime-verified. Remote main at the delivery baseline is
`9db3ed6c`; the task's local coordination baseline was `aa51d358`.

| Input | Explicit blocker carried forward | Responsibility / gate impact |
|---|---|---|
| Capability inventory, CAP-1/4/5/7 | Required journeys and retained capability value are unknown; measurements/comparison depend on scoped workflows. | H1: H-SCOPE and N1–N10. Source agents: remaining capability/consumer evidence. A–D obligations remain separate. |
| Capability inventory, CAP-2/3/6/8 | Workflow/state depth and historical rationale are incomplete; no isolated runtime demonstration. | Source researchers own traces/history; H2 assignment and operational authorization are needed for runtime work. A evidence and B/C comparison/validation inputs. |
| Workflow traces, WT-1–8 | Missing end-to-end failure, cleanup, reconnect, consumer and state links; preservation scenarios lack before/after evidence. | Source researchers verify mechanisms; H1 supplies policy through N2/N3 and H-PRESERVE. A–C evidence, D compatibility scope. |
| Workflow traces, WT-9 | Explicit owner answers are absent. | H1; T5 remains incomplete and T6–T9 remain blocked in the local research baseline. B–D downstream decisions. |
| Workflow traces, WT-10 | No measured install/launch/assign/inspect/stop/resume or failure/recovery run. | H2 unassigned; require disposable data/environment and explicit authorization, never the sole live database. Relevant A–C evidence and later validation; source reading is not runtime proof. |
| Workflow traces, WT-11 | Independent review acceptance is outstanding. | Assigned reviewer/coordinator, not automatically the human owner. Resolve any required human authority through H1/H3; documentation checks do not pass A–D. |
| State invariants, invariants 5 and completion boundary | Project/worktree write/delete authority, stop/failure/crash recovery and native resume proof remain incomplete; files, index, transcripts and conversations are distinct stores. | Source researchers verify paths; H1 decides H-PRESERVE; H2 enables later safe runs. A state understanding, C preservation/recovery, D compatibility. |
| Coordination outcomes, dependency outcome | Product choices are external human inputs; evidence integration does not authorize a prototype or migration. | d47aa owns coordination and f4958 shared integration under that historical handoff; neither is a named human product owner. T5/Gates A–D unchanged. |

Inspection locations and limitations:

- Capability: `/home/kkk/.cline/worktrees/662c4/openrig-breakdown/breakdown/research-capability-inventory.md`,
  sections “Review blockers and actionable work breakdown” and “Gaps to carry into
  deeper research.” Read during planning; this worktree was no longer available
  at delivery recheck. The summary above is from that inspection, not a fresh
  acceptance claim; retrieve the canonical artifact before relying on its full text.
- Workflows: `/home/kkk/.cline/worktrees/a4d7b/openrig-breakdown/breakdown/research-workflow-traces.md`,
  “Review blockers and actionable closure work.” The file was untracked in its
  specialist worktree at delivery recheck; its content is not a committed contract.
- State: `/home/kkk/.cline/worktrees/c28a2/openrig-breakdown/breakdown/research-state-invariants.md`,
  “Invariants for later workflow traces” and “Completion boundary and next evidence.”
  Specialist worktree HEAD at delivery recheck: `03f508c53348d4a343c423cd751e7ab74dc706cb`.
- Coordination: `breakdown/11-coordination-outcomes.md` at local commit `aa51d358`,
  “Disposition of all ten f7f4e suggestions” and “Dependency outcome and no-go boundary.”
  This file and the local T5 task list are not yet integrated into remote main.

The research documents themselves cite source baseline `9db3ed6c`; reading their
tests/citations does not establish passing behavior. Their missing integration,
remaining source work and runtime gaps are not questions the owner must verify.

## Completion boundary

This update delivers an explicit unanswered-question and blocker register, not
owner-authored answers. H1 still owes decisions and confirmed human identities.
No T5 completion, Gate A–D pass, T6–T9 unblock, architecture choice or implementation
authorization follows from asking these questions, merging this document or
accepting source research. The human-response card remains blocked and cannot be
closed by source agents. Technical verification and Git delivery belong to the
agent; product-policy decisions belong to the owner.
