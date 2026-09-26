# 11. Coordination outcomes for d5d13 and f7f4e

## Decision, authority and evidence boundary

Recorded on 2026-09-27 against historical baseline `93069eb1`, then updated
against main `8bd371a8`. This document records the
bounded documentation delegation from coordinator **d47aa** to **92f0b**.
It does not make a new decomposition decision. d47aa owns triage, card
creation, dependency links, specialist assignment and cross-component decisions.
The human owner supplies product choices. **f4958** is the exclusive serialized
claim-register and shared-document integrator, not a second coordinator.

The coordinator accepted fa0bb's replacement outcome, **1f28b**, and integrated
it through `3ea45964` and `93069eb1`. This acceptance covers documentation, not
migration authorization. The updated inventory, questions and conditional stages
exist in this baseline. The earlier report that the update was missing no longer
applies. This document is the replacement documentation outcome for d5d13/f7f4e;
it does not independently approve or close their Kanban cards.

Decision provenance: d47aa's explicit delegation and the read-only board snapshot
at `/home/kkk/.cline/kanban/workspaces/openrig-breakdown/board.json`, inspected
on 2026-09-27. In that snapshot, 1f28b's review records acceptance and integration;
43d5c has a scope correction; f4958 has an explicit integrator-only handoff.
Board contents are mutable operational evidence, not a versioned source contract.
The later inspection confirms the three research cards and dependency edges
recorded below. f4958's accepted integration commit `eeff0881` is integrated on
main as `8bd371a8`; its register and task-list contents match that commit.
The board's Review column can lag this accepted document result.

### Verified document references

References D1–D7 below are explicitly pinned to historical commit `93069eb1`.
Their stable absolute paths identify the main destination, not a promise that
today's file has the same line numbers or status. Read those ranges from that
Git object when checking the historical citations.
Historical source findings inside these documents retain their own stated
commits; citing them does not mean their runtime behavior was demonstrated here.

| Ref | Evidence and relevance |
|---|---|
| D1 | `/home/kkk/Apps/openrig-breakdown/breakdown/10-rust-and-node-removal-plan.md:3-20,66-91,119-148` — accepted documentation scope, direct dependency/execution inventory, conditional stages and separation of document acceptance from Gate D. |
| D2 | `/home/kkk/Apps/openrig-breakdown/breakdown/06-open-questions.md:3-15,17-41` — README answers reconciled; N1–N10 all remain open; answers require owner, date, rationale, scope and acceptance test. |
| D3 | `/home/kkk/Apps/openrig-breakdown/breakdown/07-review-task-list.md:23-49,51-76` — T2–T5 requirements, blocked T6–T9 and preserved status baseline, not a second queue. |
| D4 | `/home/kkk/Apps/openrig-breakdown/breakdown/09-adversarial-review-and-research-charter.md:40-95` — five normal/failure journeys, evidence labels, coverage and state-ledger completion conditions. |
| D5 | `/home/kkk/Apps/openrig-breakdown/breakdown/09-adversarial-review-and-research-charter.md:154-178` — Gates A–D, Node-removal constraints and source-research limits. |
| D6 | `/home/kkk/Apps/openrig-breakdown/breakdown/08-current-state-evidence.md:64-130,183-281,283-339,412-443` — package/deployment inventory, selected flows, coupling/state table and further research leads. |
| D7 | `/home/kkk/Apps/openrig-breakdown/breakdown/04-review-method.md:44-65` — verdict/evidence procedure and linked claim groups. |

Current integration evidence, pinned separately to `8bd371a8`:

- `/home/kkk/Apps/openrig-breakdown/breakdown/gemini-review/README.md:39-59,80-113`
  records 24 reconciled verdicts: six `yes`, eighteen `part`, none unreviewed.
- `/home/kkk/Apps/openrig-breakdown/breakdown/07-review-task-list.md:29-52`
  records T2–T4 source-review complete only, T5 incomplete pending owner input,
  T6–T9 blocked and no Gate A–D passed. These current results supersede the
  historical status assertions in D1/D3, not their evidence limits.

## Coverage and duplication decision: d5d13

The three existing claim batches now cover all 24 claim pages, with disjoint
write ownership. All 24 pages at this baseline have a yes/part/no verdict and
research content. f4958 has now integrated the register: T2–T4 are source-review
complete only, with six `yes` and eighteen `part`. Neither those verdicts nor
integration establish runtime correctness or complete the wider research gates.

| Existing card | Exclusive claim-page batch | Count |
|---|---|---:|
| 43d5c | F1, F2, F3, F6, F7, F8, F10, F11 | 8 |
| a1fa6 | A5/F4/S2, A8/F9/S5, F5/S3 | 8 |
| 36ea1 | A1–A4, A6–A7, S1, S4 | 8 |

The pages are the individual README files under
`/home/kkk/Apps/openrig-breakdown/breakdown/gemini-review/claims/`.
The union is A1–A8, F1–F11 and S1–S5, with no repeated owner. In particular,
F4/F5/F9 belong only to a1fa6, and its linked batch contains eight, not seven,
pages. Combined T2 evidence requires both 43d5c and a1fa6. Preserve the linked
mechanism conclusions and friction-first review order (D3, D7).

**No duplicate claim cards are justified.** Only f4958 may integrate
`/home/kkk/Apps/openrig-breakdown/breakdown/gemini-review/README.md`
under the coordinator handoff. That integration is now accepted in `8bd371a8`;
future shared updates remain exclusively f4958's responsibility. The result is
verified from the register and task list, not inferred from a card's column.
This delegation does not edit that register or statuses.

**No duplicate Node inventory or owner-question reconciliation card is justified.**
1f28b covers these documentation outcomes (D1, D2). Its inventory explicitly is
not an exhaustive transitive SBOM or runtime trace. N1–N10 answers remain external
human-owner inputs; reconciliation did not answer them or complete T5. Further
retained-path audits may be needed later, but are not new authorized cards here.

Three residual source-research outcomes are approved by d47aa. These are gaps in
depth and coverage, not an absence of prior research:

1. Capability coverage beyond the existing package/deployment and Node inventories.
2. Complete normal/failure traces for the five charter journeys beyond selected flows.
3. A core state/invariant ledger beyond the selected coupling and state table.

D6 already contains five useful flows, but they are not identical to the five
charter journeys: setup/launch is combined, messaging and handoff are separated,
and recovery has a separate flow. Do not count headings as proof of complete
end-to-end coverage. Claim reviews add source evidence; they do not replace a
capability coverage map or a state create/change/remove/reconcile ledger (D4).

## Approved source-research boundaries

These are output contracts supplied by d47aa, not cards created by 92f0b.
The coordinator has created and linked exactly three residual research backlog
cards: **662c4** (capability), **a4d7b** (workflows), and **c28a2** (state).
They are not replacement or implementation cards. Each is assigned to a Cline
specialist using provider `openai-codex`, model `gpt-6-luna`. Each
researcher writes only the corresponding output in their own assigned task
worktree. The absolute paths below are stable destinations in main; main is
read-only to specialists. No claim rewriting, shared
index/register edits, code, prototype, runtime experiment or machine setup is
authorized. Read existing research and claim evidence first; cite and extend it
instead of copying or re-reviewing it. Report contradictions to d47aa.

### Capability coverage — 662c4

Owned output destination in main; edit the corresponding file in 662c4's own task worktree:
`/home/kkk/Apps/openrig-breakdown/breakdown/research-capability-inventory.md`.

Acceptance:

- Inventory major user capabilities from current entrypoints and component
  responsibilities, not package names alone. Include core operation and optional
  UI, TUI, MCP, multi-host, workflow, context/spec/plugin, workspace and adapter
  surfaces identified by existing evidence.
- For each capability, record owner component, entrypoint/source evidence,
  dependencies, state/managed artifacts, relevant journey and coverage status:
  investigated, scheduled or deferred with a reason. Do not claim retained scope
  has been chosen. Record unknown owner value as unknown.
- Reuse D1's Node inventory and D6's package/deployment map. Every major area
  must have a disposition; distinguish examined coverage from remaining research.

### Five normal/failure workflow traces — a4d7b

Owned output destination in main; edit the corresponding file in a4d7b's own task worktree:
`/home/kkk/Apps/openrig-breakdown/breakdown/research-workflow-traces.md`.

Acceptance:

- Cover all five D4 journeys: install/start; put agents in a project; assign/follow
  work; understand the result; stop/return. For each, trace input → command/client
  → runtime owner → durable state/process effect → visible result.
- Provide one normal and at least one source-inspected failure path per journey,
  including error reporting, cleanup/restart implications and unknown links.
  Distinguish daemon readiness, agent readiness, delivery and acceptance.
- Reuse D6's selected flows and the relevant claim findings. Record source
  contracts, not a successful installation, measured usability or tested recovery.
  The provisional scenario is a research anchor, not an approved product scope.

### Core state and invariant ledger — c28a2

Owned output destination in main; edit the corresponding file in c28a2's own task worktree:
`/home/kkk/Apps/openrig-breakdown/breakdown/research-state-invariants.md`.

Acceptance:

- For core stores, files, caches and live processes used by these journeys,
  identify authority, creators/writers/deleters, identity lifetime, allowed
  transitions, transaction boundaries, event ordering and reconciliation.
- Cover session/liveness/resume distinctions; queue state/history and notification
  guarantees; events/client observation; project/worktree files and metadata;
  snapshots/transcripts; and owned versus user-owned configuration/hooks. Mark
  absent or unresolved mechanisms explicitly rather than assuming a guarantee.
- For each invariant, cite its enforcement or stated contract, the relevant
  partial-failure/restart case and missing proof. Distinguish tracked edits,
  untracked files and index state from resumable conversation/process state.
- Reuse D6 and a1fa6's linked evidence. An absence of deletion in one teardown
  function is not proof of preservation across all delegated cleanup paths (D4).

### Common evidence and integration checks

Each consequential finding needs an evidence ID, inspected commit, exact source
path/line range, scope and confidence. Use observed-in-source, stated-intent,
inferred and unresolved labels (D4). Tests read are not tests passed. Each
unresolved point must name the next evidence needed. Check local citations,
coverage and diff hygiene; do not run product lifecycles under this delegation.

The accepted 1f28b baseline and existing claim evidence are inputs to all three
outputs. The coordinator deliberately serialized prerequisites: all three wait
on accepted 92f0b; a4d7b also waits on 662c4; c28a2 also waits on a4d7b and a1fa6.
Disjoint file ownership does not authorize parallel acceptance or automatic
start. Source reading may later run in parallel only with an explicit coordinator
handoff. Before acceptance, reconcile workflow coverage with the capability map
and the state ledger with workflow effects and linked-claim findings. d47aa resolves
cross-component disagreements; researchers do not edit each other's outputs.
The coordinator owns concrete dependency links and shared integration sequencing.
Completing these documents supplies evidence for gates; it does not pass a gate.

## Disposition of all ten f7f4e suggestions

“Research-covered” means an existing or approved bounded research outcome owns
the subject; it does not mean research is complete. “Owner-gated” requires an
explicit human decision. “Implementation-gated” forbids implementation cards
until the documented gates and separate authorization support them.

| # | Suggested category | Coordinator disposition and evidence |
|---|---|---|
| 1 | Reconcile owner decisions/open questions | **Research-covered; owner-gated.** 1f28b reconciled existing answers and recorded N1–N10. No duplicate reconciliation card. The human owner must supply unresolved answers (D2); T5 is not complete. |
| 2 | User-visible workflows and compatibility contracts | **Research-covered by the three approved residual outputs**, using existing flows and claims. Compatibility promises themselves remain **owner-gated**, especially N3/N4/N7. Source contracts are not migration promises (D2, D4, D6). |
| 3 | Rust architecture/prototype spike | **Owner- and implementation-gated.** T8/Gate C compares architecture only after T7; a prototype requires Gate D and a separately approved implementation plan. No spike card is authorized (D1 stages 1–4; D3, D5). |
| 4 | Daemon/runtime migration | **Implementation-gated.** Current dependency/state evidence is covered, but process model, platforms and compatibility require N3/N4/N8/N9 and Gates C/D. Runtime transfer is a later conditional stage, not an approved package port (D1 stages 2–5; D2). |
| 5 | CLI/TUI migration | **Owner- and implementation-gated.** Current entrypoints/TUI are inventoried. N2 decides retained consumers; approved workflow research supplies contracts. No CLI/TUI rewrite card before agreed seams and later runtime-transfer approval (D1 inventory/stage 5; D2). |
| 6 | UI decision and chosen replacement | **Owner-gated** by N2 and the fully Rust/non-Rust boundary; any replacement is **implementation-gated**. Browser JavaScript, Node hosting and Node build tooling are separate inventory facts, not permission to retain React or choose a replacement (D1:22-64,82; D2). |
| 7 | Embedded Node assets/hooks | **Research-covered** by 1f28b's execution inventory and approved state/ownership research; **owner-/implementation-gated** for disposition or conversion. N3/N5/N6/N8/N9 govern retained contracts and exceptions. Preserve user-owned configuration (D1:83-85,88-91; D2, D4). |
| 8 | Build/test/repository tooling and CI | **Research-covered** by the inventory; **owner-/implementation-gated** for replacement. N1/N6/N8/N9/N10 determine transition, fixtures and external/provider boundaries. Full elimination includes required tooling, but no tool/library choice or CI rewrite is authorized (D1:50-64,86-90,134; D2). |
| 9 | Packaging/install/upgrade/rollback | **Research-covered** by current execution-path evidence and approved workflow/state research; **owner-/implementation-gated** for changes. N3–N9 affect data, platforms, external setup, coexistence and distribution. Later cutover requires runtime and rollback proof (D1:87-91,132-135; D2). |
| 10 | End-to-end parity and Node-removal audit | **Research-covered** for future acceptance requirements, not achieved parity or an exhaustive removal audit. **Implementation-gated** for executable validation/retirement after stages 4–6. Require isolated clean-environment, artifact/process and recovery evidence; grep alone is insufficient (D1:150-171; D3:66-69). |

## Dependency outcome and no-go boundary

The read-only board snapshot on 2026-09-27 confirmed these actual card
links, matching the coordinator's instruction. The graph records that snapshot;
d47aa owns any subsequent dependency changes. This agent does not edit the board.
During final validation, the live board no longer contained any of c28a2's
three edges (to 92f0b, a4d7b and a1fa6). Thus this graph records the verified earlier snapshot and coordinator
contract, not a guarantee of current scheduler enforcement. d47aa must reconcile
the live links before dispatch; no dependency was silently recreated here.
Here `prerequisite -> dependent` means the dependent waits for acceptance of the
prerequisite (the board stores the dependent in `fromTaskId`).

```text
92f0b -> 662c4
92f0b -> a4d7b
92f0b -> c28a2
662c4 -> a4d7b
a4d7b -> c28a2
a1fa6 -> c28a2
92f0b -> d5d13
92f0b -> f7f4e
```

| Card | Specialist / exclusive output in its own task worktree | Waits on |
|---|---|---|
| 662c4 | Capability: corresponding file for `/home/kkk/Apps/openrig-breakdown/breakdown/research-capability-inventory.md` | Accepted 92f0b |
| a4d7b | Workflows: corresponding file for `/home/kkk/Apps/openrig-breakdown/breakdown/research-workflow-traces.md` | Accepted 92f0b and 662c4 |
| c28a2 | State: corresponding file for `/home/kkk/Apps/openrig-breakdown/breakdown/research-state-invariants.md` | Accepted 92f0b, a4d7b and a1fa6 |

These are main destination paths, not a claim that the output files already
exist. Specialists edit corresponding files only in their own task worktrees.

fa0bb and 1f28b are Done. The three claim diffs and f4958's register integration
are accepted on main `8bd371a8`, with T2–T4 source-review complete only. This document fulfills
the requested d5d13/f7f4e documentation outcomes, with coordinator approval still
pending. All three new research cards remain in Backlog; none is started by this
delegation. No board mutation is performed here.

Research and implementation gate requirements remain separate from card links:

```text
accepted fa0bb/1f28b baseline at 93069eb1
  + existing disjoint claim batches
  + accepted f4958 register integration at 8bd371a8
  -> 92f0b outcome -> 662c4 -> a4d7b -> c28a2 (also requires a1fa6)
  -> d47aa acceptance/reconciliation; f4958 retains exclusive shared integration

T2–T4 source review complete + external owner answers -> T5 completion check
capability/workflow/state evidence + visible contradictions -> Gate A check
T2–T5 -> T6
T6 + Gate A -> T7 / Gate B
T7 + preservation/recovery contracts + owner choices -> T8 / Gate C
T7–T8 + explicit high-impact decisions -> T9 / Gate D
Gate D + separately approved implementation plan -> possible later implementation
```

T6–T9 and Gates A–D are not passed by this decision. D3's old status rows are
historical; current main explicitly records T2–T4 source-review completion only.
T5 remains owner-blocked (recorded as in progress with owner input incomplete).
Owner N1–N10 answers remain external.
**No Rust implementation or prototype cards are authorized.** The ten suggestions
are disposed above, not converted into speculative migration cards.

## Final replacement-outcome record

- **d5d13 → 92f0b:** this document supplies the coverage/duplication decision,
  bounded research acceptance and ownership for coordinator-created 662c4,
  a4d7b and c28a2. No duplicate triage or claim task is needed.
- **f7f4e → 92f0b:** the same deliverable supplies all ten migration-category
  dispositions, actual dependency graph and implementation no-go. No duplicate
  decomposition or speculative Rust/prototype card is needed.
- **92f0b** authored this documentation outcome and created no cards. **d47aa**
  owns acceptance/closure of both originals, scheduling and card links;
  **f4958** retains exclusive register/shared integration ownership.

No additional claim, Node-inventory, owner-question reconciliation or migration
cards are justified by this bounded decision. The three residual research cards
are the explicit capability/workflow/state work, not a claim that all research
is complete. Further uncovered work requires a coordinator decision.