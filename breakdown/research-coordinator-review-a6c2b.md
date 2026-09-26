# Coordinator evidence review: 662c4, a4d7b and c28a2

## Decision and authority

Reviewed on 2026-09-27. **662c4: FAIL; a4d7b: FAIL; c28a2: FAIL** for final
source-research acceptance. These are artifact-review decisions, not board
mutations, replacement assignments, owner decisions or gate decisions. Useful
bounded source observations exist in all three; none is an accepted complete
deliverable. The review itself is complete for the snapshots identified below.
Later revisions require another review.

The controlling contract is
`/home/kkk/Apps/openrig-breakdown/breakdown/11-coordination-outcomes.md:119-191`
at `aa51d3581825ce286d9dc2b379e21bee92a2a1c6` (local main at inspection).
Its three output contracts are capability coverage (119-135), five normal/failure
journeys (137-152), and state/invariants (154-172); common evidence and serialized
reconciliation requirements are at 174-191. Its authority/no-go limits are at
105-117 and 264-287. This document does not change that contract.

No T5-T9 or Gate A-D approval is issued. T5 remains owner-input incomplete;
T6-T9 remain blocked. N1-N10, runtime observations and remaining gate work are
separate. Missing owner answers or unexecuted runtime scenarios are NOT reasons
to reject a bounded source document that otherwise meets its contract. The
failures here are missing source coverage, unsupported/mispinned citations,
incomplete reconciliation and, for a4d7b, missing final commit evidence.

All review findings below are **observed in source/document**, with **high
confidence for the specified snapshots only**, unless explicitly called
unresolved. No product tests, installation, daemon startup, agent launch,
teardown, recovery or usability experiment was run. Reading code, help text or
test assertions is not passing tests or demonstrating runtime behavior.

## Artifact identity and diff evidence

| Card | Reviewed artifact and immutable identity | Actual diff / final-commit result |
|---|---|---|
| 662c4 | Historical worktree path `/home/kkk/.cline/worktrees/662c4/openrig-breakdown/breakdown/research-capability-inventory.md`; recovered from `f287de77243bf81c974915f1c2a2f69136d35330`. SHA-256 `26105f9d90b95d5ef3491626c32060f05d689d26b628e747bed80d284297d6e6`; 159 lines. | Initial inspection found a clean worktree; it disappeared before execution. Git objects remain. `ac5e509375c350ed0cfdef57e47ce9477033ca15` introduces the document; final `f287de77` adds 33 lines of CAP blocker decomposition. Cumulative diff from `8bd371a8` adds only this 159-line document. No claim of a currently inspectable worktree or later final revision. |
| a4d7b | `/home/kkk/.cline/worktrees/a4d7b/openrig-breakdown/breakdown/research-workflow-traces.md`; untracked draft at HEAD `8bd371a8abdc1ac5813c1d2fa042516cb8e1f55d`. SHA-256 `f6307f9c728889cf34e71fce01f587be257f4f26022f0326c6a98ca2edc1454d`; 108 lines. | Staged/unstaged tracked diffs are empty; status reports this untracked document. HEAD is earlier shared integration, NOT a workflow final commit. The draft changed since planning; findings use this hash, not the earlier 107-line draft. |
| c28a2 | `/home/kkk/.cline/worktrees/c28a2/openrig-breakdown/breakdown/research-state-invariants.md` at `03f508c53348d4a343c423cd751e7ab74dc706cb`. SHA-256 `07ca9a7c9f22c2ba03bf0d2f9efc97181bd6c0f9ad3ce5dd126915ec57a7c525`; 157 lines. | Clean worktree. Final commit adds only the owned document, relative to `8bd371a8`. Supersedes the untracked 108-line draft seen during planning. |

Line references to the three artifact paths below always mean these snapshots,
not future worktree contents. Repository source references use the absolute
main destination path but are read from Git objects, not main's current bytes.
Final validation detected new unstaged edits in c28a2 after its clean committed
snapshot was reviewed. Those concurrent edits are not accepted or covered by
this decision; the owner must provide a new final commit for another review.

### Pin separation

- **S:** `9db3ed6c406be5c3d9a84720383fcf6b543169e6` — implementation source.
- **D:** `93069eb13ac621f8db445e7866af978169f3ff60` — integrated claim pages and
  earlier planning documents, not the later reconciled register.
- **R:** `8bd371a8abdc1ac5813c1d2fa042516cb8e1f55d` — reconciled register/task list.
- **C:** `aa51d3581825ce286d9dc2b379e21bee92a2a1c6` — controlling coordination baseline.

These are different evidence layers. In particular, a claim page that reviews
source S need not itself exist in its reviewed form at S. At S, A5/A8 still
contain unverified verdicts and empty evidence sections. At D, they contain the
linked reviews; at R, the shared register reports their reconciliation.

## 662c4 criterion matrix and narrowly scoped rework

All artifact ranges in this section refer to
`/home/kkk/.cline/worktrees/662c4/openrig-breakdown/breakdown/research-capability-inventory.md`
at `f287de77`.

| ID / contract criterion | Result and exact finding | Required correction / acceptance check |
|---|---|---|
| RC-1: major capabilities, including optional surfaces | **FAIL**, lines 43-57. Thirteen rows cover CLI, daemon, adapters, spec/workspace, queue, observation, recovery, configuration, MCP, adjacent integrations, tooling, test-system and docs. No explicit disposition for multi-host or executable Workflow operation; generic operator “workflow relevance” is not a disposition for that product surface. | Add bounded rows for multi-host and Workflow, or explicit subrows with owner, evidence, dependencies, state, journey, status/reason and unknown owner value. Source S `/home/kkk/Apps/openrig-breakdown/packages/cli/src/index.ts:29,45,198,212` imports/registers host and Workflow commands. Do not choose whether to retain them. |
| RC-2: per-capability owner, entry evidence, dependencies, artifacts, journey, status | **PARTIAL**, lines 43-57. Table structure and investigated/scheduled/deferred distinctions exist; several entries rely only on package/claim summaries. Context/spec/plugin coverage is grouped without a clear per-surface evidence mapping. | Make every required surface traceable to a row/disposition. Add evidence IDs and exact document/source citations for summary reuse. Scheduled/deferred with reason is permitted; a full optional-surface runtime trace is not required for this card. |
| RC-3: reuse Node/package inventory without duplication | **PARTIAL**, lines 5-17, 120-138. Reuse and counting predicates exist, but document 10 is referenced under the default source pin S, where it does not exist. The same problem affects claims of an accepted register at S. | Pin reused planning documents to D/R as appropriate, separately from source S. Preserve counting predicates and their navigation-only limitation; do not use file counts as usability evidence. |
| RC-4: consequential evidence IDs, scope/confidence and citation correctness | **FAIL**, lines 43-57, 73-82. CAP-1 through CAP-8 identify proposed work, not evidence IDs attached to capability findings. No per-finding confidence scheme is provided. CLI `index.ts:93-110` is a dependency interface, not command composition. | Add a compact evidence key/scope/confidence scheme; cite actual registration if claiming composition. Fix the inventory link at line 7: target heading is `83-checkout-and-code-inventory`, not `checkout-and-code-inventory`. |
| RC-5: scope, unresolved items, no runtime/gate overclaim | **PASS with clarification**, lines 19-32, 61-118, 141-159. Limits and next work are visible. CAP-1 owner input, CAP-5 measurement, CAP-6 history, CAP-7 architecture and CAP-8 runtime work are not completed by their presence in a table. | Preserve these limits. CAP-2/3 overlap workflow/state assignments; route rather than duplicate. Do not make finishing all CAP proposals a prerequisite for this bounded capability inventory. |
| RC-6: final commit and exclusive diff | **PASS for recovered commit**, identity table above. | Restore/reassign the missing worktree only through the coordinator if rework needs it; do not infer acceptance from its removal or Trash column. |

**Decision: FAIL**, specifically RC-1 through RC-4. Rework is confined to the
capability artifact and its evidence metadata; no owner answers, runtime run,
new claim verdict, or shared-register edit is requested.

## a4d7b: all five journey checks

All artifact ranges in this section refer to
`/home/kkk/.cline/worktrees/a4d7b/openrig-breakdown/breakdown/research-workflow-traces.md`
at the draft hash above. All five headings exist; that is not five complete
normal/failure contracts.

| ID / journey | Normal evidence and failure evidence present | Missing source-only acceptance evidence / rework |
|---|---|---|
| RW-1: install/start, lines 9-19 | Setup and up commands, daemon spawn/migrate references, ambiguity rejection and prerequisite checks. | Finish one coherent failure from input through diagnostic and residual state, rather than mixing missing tools, ambiguity and listeners. Show readiness/diagnostic implementation and setup partial-effect/cleanup implications. Distinguish intent in the guide from observed implementation. Other named cases may remain explicitly unresolved with next evidence. |
| RW-2: agents in a project, lines 21-31 | Runtime adapter interface, Claude/Codex command emission, restore preflight and missing-token decision. | Interface declarations do not prove launch topology/session writes. Add command-to-route/orchestrator-to-project/cwd/isolation and process-creation links, then one launch-before-ready/exit/partial-cleanup failure with output. Restore failure alone does not establish fresh-launch cleanup. |
| RW-3: assign/follow, lines 33-43 | CLI claim/update/handoff, SQLite schema, ordinary create versus terminal wake-intent transaction. | Complete request/route/actor validation and visible failure result for one coherent failed assignment/delivery; distinguish the send guard help contract from its implementation. Source S `queue-repository.ts:1320-1358` already provides bounded same-ID/source/destination retry behavior; do not leave all duplicate handling generically unknown. Preserve local versus cross-host and nudge-disabled qualifications. |
| RW-4: understand result, lines 45-55 | Status/ps/queue intent, persisted events and restore's unknown-probe behavior. | Trace actual status/log/client read and formatting path, including stale/disconnected result and refresh/reconciliation. Restore's probe guard is not evidence of how status/ps/UI presents disagreement. Explicitly map non-CLI surfaces to coverage dispositions; no mandatory runtime UI exercise. |
| RW-5: stop/return, lines 57-67 | Snapshot-before-kill, continue-after-capture-error, restore decisions and warnings. | Cite down command/route/output separately from up restore output. `up.ts:530-549` does not print teardown results. Follow managed-guidance and service teardown; document dirty tracked/index/untracked boundaries without claiming preservation. Add signal/interruption/restart links or precisely bounded unknowns and next source evidence. |

Additional failed criteria:

- **RW-6 — pin/link correctness:** line 5's charter anchor has an extra leading
  hyphen. At default pin S, the A3 range at line 31 ends at 48 but its file has
  41 lines; A2 at line 55 ends at 47 but has 39; A8 at line 64 ends at 41 but has
  28. The register range 76-113 at line 71 exceeds its 80 lines at S. Document 10
  and task list 07 are absent at S. Register lines 65-69 at S are not linked
  conclusions. Re-pin these to D/R and recheck semantics, not just bounds.
- **RW-7 — standalone citations:** lines 16, 29 and other rows use ambiguous
  basenames (`setup.ts`, `startup.ts`, `up.ts`) or bare line continuations.
  Several full-path citations point only to help text/interface contracts while
  the row says observed implementation. Expand paths, distinguish stated intent,
  and add finding IDs/confidence with exact source evidence.
- **RW-8 — actual capability reconciliation:** lines 69-82 and WT-7 at line 98
  substitute document 10 for 662c4's artifact. Map each actual capability row to
  journey/consumer/state effect or justified remaining disposition. Do not call
  662c4 accepted before a review accepts its revision.
- **RW-9 — final commit:** only an untracked draft exists. Commit only the owned
  artifact after correction and provide its final hash/diff for review.

**Decision: FAIL.** Normal/failure coverage is partial in every journey, not
absent altogether. The uncertainty, source-only and no-gate disclaimers at
lines 3-7, 19/31/43/55/67 and 104-108 must survive rework. WT-6's proposed runtime
exercise is separate from closure of source research. Do not require all listed
environmental failures to be executed, or demand owner answers to finish this
card. WT-1 through WT-8 are proposals, not completed evidence.

## c28a2 criterion matrix and narrowly scoped rework

All artifact ranges in this section refer to
`/home/kkk/.cline/worktrees/c28a2/openrig-breakdown/breakdown/research-state-invariants.md`
at `03f508c5`.

| ID / contract criterion | Result and exact finding | Required correction / acceptance check |
|---|---|---|
| RS-1: core state inventory and actors | **PARTIAL**, lines 54-65. Nine rows cover important stores and files. Project/catalog/worktree actors remain openly unresolved. Live-process and cache state lacks a corresponding create/change/remove/reconcile treatment. | Add bounded process/cache rows for states actually used by the journeys, including daemon/tmux liveness and relevant in-memory timers/locks. For unresolved actors, name the search boundary and next source path/evidence; do not demand knowledge of external-agent internals. |
| RS-2: allowed transitions, authority, transaction/event ordering | **FAIL**, lines 61-64, 67-95. Selected create/handoff/retirement effects and identity distinctions exist, but no allowed-transition/enforcement ledger for queue and session/readiness/resume/wake state. | Give each invariant an ID, allowed transition/actor/guard, enforcement or contract, failure/restart case and missing proof. E.g. S `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/queue-repository.ts:2038-2055` explicitly restricts claim destination and claimable pending/blocked states. Map notification states separately from work states. |
| RS-3: client observation/reconciliation | **PARTIAL**, lines 61, 63-64. DB/event persistence and queue-history union are evidenced, but client reconnect/replay/refresh policy is not mapped. | Link the relevant workflow observation paths to store/event authority and precise unresolved mechanisms. Distinguish boot reconciler transport-unavailable detachment from restore's fail-closed unknown probes. |
| RS-4: preservation and delegated deletion | **PARTIAL**, lines 59-65, 151-157. Correctly separates Git, native history and pane transcripts; records managed-guidance writes and snapshot retention. Service-volume deletion already established in linked F9 is omitted from the ledger. | Add the delegated service policy and state effect (source anchors below). Keep ordinary checkout, managed files and optional volumes distinct. Source mapping can be completed now; real no-loss/resume evidence remains NOT RUN. |
| RS-5: evidence metadata and pins | **FAIL**, lines 5-8, 12, 59, 61-62, 77. Default S makes A5 `27-37` exceed its 30 lines. A8 `25-28` is blank/unreviewed evidence at S, not its accepted finding. “Source observation” is defined as behavior OR intent, weakening the required distinction. | Pin claim documents to D and register to R, retain implementation pin S; use the four contract labels consistently, add evidence IDs/confidence and per-invariant failure/missing-proof links. |
| RS-6: workflow/linked-claim reconciliation | **PARTIAL**, lines 97-132. The final commit genuinely adds a draft cross-check, but says citation checks passed despite the anchor/pin failures above; it acknowledges that bounds are not semantic validation. Broad agreement is not transition-by-transition coverage. | Correct the verification claim, link each invariant to the corrected workflow effect/failure and linked claim, then re-review after accepted workflow evidence. Cite prerequisite acceptance records rather than treating old “pending” prose as proof of today's board state. |
| RS-7: final commit, ownership, limitations | **PASS**, identity table and lines 142-157. Only the owned document changed; no runtime/gate success claimed. | Preserve these limits; split source-resolvable gaps from future runtime proof instead of saying all remaining observations are unavailable from source alone. |

**Decision: FAIL**, specifically missing RS-1/3/4 coverage plus RS-2/5/6.
An explicitly unresolved mechanism is permitted; silently unrepresented
transitions, ambiguous evidence pins and omitted linked cleanup findings are not.

## Cross-artifact reconciliation and verified source boundaries

These are reusable bounded observations, **not acceptance of the three outputs**.
Paths in this section are at source S unless D/R is explicitly specified.

- **RX-1 — readiness versus liveness:**
  `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/reconciler.ts:44-96`
  treats a non-present probe, including transport-unavailable, as detachable at
  this boot boundary; thrown probe errors are retained as errors. In contrast,
  `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/restore-orchestrator.ts:239-250,782-812`
  blocks on live/unknown sessions. Its `976-1006` missing-token case returns
  awaiting-decision before launch. Workflow 2/4/5 and ledger session/resume rows
  must preserve these different call-site policies, not promise uniform recovery.
- **RX-2 — durable work is not delivery/acceptance:**
  `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/queue-repository.ts:1320-1358`
  commits create before notification/nudge; same explicit ID with matching
  source/destination returns the existing row. `807-846,910-945,1507-1669` stage
  and guard intended terminal wake with close/successor; `nudge:false` is an
  explicit exception. `1114-1162` leaves failed/indeterminate intents unretried
  and reconciles abandoned sending separately. `3424-3458` releases retiring
  generation claims to pending. Workflow 3 and the ledger agree broadly but
  still need actor/transition/visible-result mapping. The linked F5/S3 conclusion
  at R `/home/kkk/Apps/openrig-breakdown/breakdown/gemini-review/README.md:61-69`
  also limits cross-host atomicity and cognitive-drift claims.
- **RX-3 — snapshot is not file backup:**
  `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/snapshot-capture.ts:60-166`
  reads state before assembling metadata and transactionally storing snapshot/event.
  It does not archive Git/index/native history/volumes. Source reads preceding
  persistence are not a quiesced snapshot of all external state.
- **RX-4 — teardown can write/delete delegated state:**
  `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/rig-teardown.ts:103-143,202-231`
  continues after capture failure, clears successfully killed session state,
  cleans managed guidance and calls service teardown.
  `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/service-orchestrator.ts:140-161`
  selects persisted down policy;
  `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/adapters/compose-services-adapter.ts:81-98`
  includes `--volumes` for `down_and_volumes`. D
  `/home/kkk/Apps/openrig-breakdown/breakdown/gemini-review/claims/friction/F9-teardown-despite-failed-snapshot/README.md:29-48`
  already records this boundary. Workflow 5 and ledger preservation claims must
  carry it forward, without asserting that snapshot failure itself deletes work.
- **RX-5 — histories and retention:**
  `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/queue-retention.ts:124-217`
  archives then deletes transition rows per item, excluding live Workflow frontiers;
  `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/queue-transition-log.ts:151-160`
  unions active/archive history. The old append-only schema comment is intent,
  not proof that physical rows are never deleted. Snapshot pruning is separate
  (`/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/snapshot-repository.ts:184-209`).

## Validation method and limits

Read actual task status/diffs/final commit objects, not just card narratives.
Parsed explicit path/line citations, resolved them with `git show S:path`,
checked bounds, and read cited implementation ranges for their associated
claims. Basename references were resolved where unambiguous and flagged where
ambiguous; bare `:range` continuations still require owner normalization.
Checked Markdown target headings against each artifact's document baseline.
This is not a claim that every inherited citation in every linked document has
been recursively re-reviewed or that an absent lifecycle was proved absent.

Mechanical range scan: capability 9 unique resolved paths / 22 range
occurrences; workflow 24 / 87 (excluding ambiguous/missing paths); state 36 / 92
(excluding absolute cross-worktree operational references, inspected separately).
Bounds successes do not establish semantic support. Concrete failures are
listed above. Capability has one broken anchor among four link occurrences;
workflow has one among two; the state's three local links resolve at its
artifact baseline, but this does not cure its source-pin errors. Do not repeat
the state document's “checks passed” as full citation validation.

Git diff scope checks passed for the recovered capability and committed state
documents. The workflow final-commit check failed. Documentation-only checks
are not product test results. No npm build/test or machine setup was performed.

## Rework routing and f4958 handoff

**Rework requests:** RC-1..RC-4 to the capability owner; RW-1..RW-9 to the
workflow owner; RS-1..RS-6 to the state owner, only within their owned outputs.
Re-review corrected commits in capability -> workflow -> state order. These
requests are recorded here for coordinator routing; no agent restart, task
creation, board change or live message delivery is claimed.

The read-only board at
`/home/kkk/.cline/kanban/workspaces/openrig-breakdown/board.json` changed during
this work. It lists 662c4, f4958 and d47aa in Trash, c28a2 in Review, and empty
dependencies. It also lists existing overlapping follow-ups: df6f0 optional
capabilities, b0da1 workflow source closure, 821e1 state/preservation, 7aa89 owner
scope, 55605 owner options, d1795 runtime protocols, and 9a783 review matrix.
These are routing leads, NOT acceptance or reassignment evidence. In particular,
a4d7b/b0da1 and c28a2/821e1 need serialized write ownership. Do not create another
review matrix or silently compete with 9a783. The coordinator must reconcile
these assignments and historical prerequisites; this review does not restore
cards or infer acceptance from Trash/Review/empty links.

**To f4958: accepted research artifacts = none.** This review packet and RX-1..5
may inform status integration only as failed-review findings and bounded source
observations, not as accepted capability/workflow/state completion. Preserve
T2-T4's previously accepted source-review-only result, T5 owner-input incomplete,
T6-T9 blocked, and no Gate A-D passed. Preserve all unknowns and NOT RUN limits.
No shared status/index/register edit is authorized to this reviewer:

- `/home/kkk/Apps/openrig-breakdown/breakdown/07-review-task-list.md`
- `/home/kkk/Apps/openrig-breakdown/breakdown/README.md`
- `/home/kkk/Apps/openrig-breakdown/breakdown/gemini-review/README.md`

Live handoff is **unresolved**: f4958 is not an active card in the inspected
board, and `rig context list` reported the daemon stopped. No daemon was started.
The durable packet is available here; the coordinator must supply an active
recipient/acknowledgment before claiming delivery or integration.

## Publication boundary

The user requested commit, push, PR and merge of this review. Remote main was
at source S while local main was at C. To avoid publishing unrelated local
shared-file changes, this review branch starts at remote main and adds only
this document. Referenced local-only artifacts/contract commits remain Git
object evidence in the inspected repository; this PR does not publish, accept,
or merge them. Remote readers may need those exact artifacts supplied by their
owners before independently reproducing the whole review. Publishing this
review must not be represented as completion of the failed research cards.