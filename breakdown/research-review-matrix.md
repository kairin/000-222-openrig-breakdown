# Independent source-research review matrix

Reviewed on 2026-09-27 for task **9a783**, the independent replacement review
for a6c2b. This is a completed review of the available evidence, not a claim
that every reviewed artifact meets its contract. All three artifacts are
**PARTIAL overall** for the specific reasons below. No verdict depends on a
board column, scheduling edge, owner response or successful runtime execution.

## Scope, revisions and evidence notation

Only this document is changed. Source artifacts, claims, the shared register,
code, task statuses and gate statuses are outside this review's write scope.
No Gate A–D is declared passed. No installation, agent lifecycle, recovery,
usability measurement or product test suite was executed. Tests read are not
tests passed. Owner answers and runtime results are future evidence, not
blockers to completing this matrix or accepting a bounded source-only outcome.

The local review baseline was `aa51d3581825ce286d9dc2b379e21bee92a2a1c6`.
Remote main was `9db3ed6c406be5c3d9a84720383fcf6b543169e6` when publication
was prepared. This document is published independently; it does not import the
unpublished planning/research history or claim those artifacts are on remote
main. Evidence objects below were available in the local Git object database.
An independent clone may need the coordinator to supply those objects before
reproducing the document review. That distribution limitation is explicit,
not a missing-artifact verdict against locally inspected evidence.

All evidence aliases below expand to an **absolute path plus exact commit**.
An alias followed by `:start-end` means the inclusive lines of that Git object,
not necessarily today's filesystem content. Stable destination paths identify
files; they do not assert that the artifact is integrated there.

| ID | Exact path | Inspected commit |
|---|---|---|
| C | `/home/kkk/Apps/openrig-breakdown/breakdown/research-capability-inventory.md` | `f287de77243bf81c974915f1c2a2f69136d35330` |
| W | `/home/kkk/Apps/openrig-breakdown/breakdown/research-workflow-traces.md` | `ddc6cf4d4e08720cfb506002658fb4a371a8d969` |
| S | `/home/kkk/Apps/openrig-breakdown/breakdown/research-state-invariants.md` | `03f508c53348d4a343c423cd751e7ab74dc706cb` |
| A | `/home/kkk/Apps/openrig-breakdown/breakdown/11-coordination-outcomes.md` | `aa51d3581825ce286d9dc2b379e21bee92a2a1c6` |
| H | `/home/kkk/Apps/openrig-breakdown/breakdown/09-adversarial-review-and-research-charter.md` | `aa51d3581825ce286d9dc2b379e21bee92a2a1c6` |
| E1 | `/home/kkk/Apps/openrig-breakdown/packages/cli/src/index.ts` | `9db3ed6c406be5c3d9a84720383fcf6b543169e6` |
| E2 | `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/rig-teardown.ts` | `9db3ed6c406be5c3d9a84720383fcf6b543169e6` |
| E3 | `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/snapshot-capture.ts` | `9db3ed6c406be5c3d9a84720383fcf6b543169e6` |
| E4 | `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/queue-repository.ts` | `9db3ed6c406be5c3d9a84720383fcf6b543169e6` |
| E5 | `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/event-bus.ts` | `9db3ed6c406be5c3d9a84720383fcf6b543169e6` |
| E6 | `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/restore-orchestrator.ts` | `9db3ed6c406be5c3d9a84720383fcf6b543169e6` |

C extends initial artifact commit `ac5e509375c350ed0cfdef57e47ce9477033ca15`.
W is a checkpoint object, not the HEAD of the workflow worktree. Its 108 lines
were byte-identical to the untracked file at
`/home/kkk/.cline/worktrees/a4d7b/openrig-breakdown/breakdown/research-workflow-traces.md`.
S's 157 lines matched its committed task-worktree file at
`/home/kkk/.cline/worktrees/c28a2/openrig-breakdown/breakdown/research-state-invariants.md`.
Neither availability nor a checkpoint implies acceptance.

**PASS** means the bounded source-research requirement is evidenced.
**PARTIAL** means useful evidence exists but a named requirement remains weak,
missing or inconsistent. **NOT EVIDENCED** means no supporting evidence was
found in the inspected artifact, not proof that the software lacks a mechanism.
Row IDs are review evidence IDs. Unless noted, findings are **observed in
source/document, high confidence**, scoped to these revisions. Interpretive
coverage judgments are **inferred, medium confidence**; correction checks name
the evidence that could change them. Unknown runtime outcomes are **unresolved**.

## Capability acceptance — 662c4

| ID / contract | Verdict | Exact evidence and finding | Gap / acceptance check |
|---|---|---|---|
| C1 — major user capabilities, not package names alone (A:126-129) | PARTIAL | C:43-57 has 13 capability rows covering CLI, daemon, adapters, workspaces, queue, presentation, persistence, config, MCP, integrations, tooling, fixtures and context. Explicit multi-host and workflow-engine dispositions are missing; E1:29-45 imports host and workflow commands. | G1: explicitly account for both surfaces, even if deferred. |
| C2 — owner, entry evidence, dependencies, artifacts and journey per capability (A:130-133) | PARTIAL | C:45-57 supplies these columns, but several entries use claim names or document sections rather than exact evidence; some owners remain broad combinations. C:51 calls out snapshot files without distinguishing the DB snapshot payload shown at E3:125-164. | G1/G2: precise owner/entry/effect references and distinguish DB snapshots from files/volumes. |
| C3 — investigated/scheduled/deferred with reason (A:131-135) | PASS | C:36-41 defines shallow investigation honestly; C:45-57 gives every listed row a disposition and qualifies scheduled/deferred work. This passes for listed rows only; missing surfaces are C1. | No deeper workflow audit is required inside each inventory row. |
| C4 — no selected retained scope; unknown owner value remains unknown (A:132-133) | PASS | C:5-32,53-57 separates presence from value, retention and runtime outcomes. | Future owner evidence is P1, not a source-review blocker. |
| C5 — reuse Node/package evidence (A:134-135) | PASS | C:5-11,45-57,120-139 reuses existing maps and separates reproducible file counts from complexity or usability. | Link accuracy is reviewed separately under V1. |

## Five normal/failure workflow mappings — a4d7b

Every journey has a five-link table and a normal/failure column. This is useful
structure, not proof that every link was source-inspected. The findings below
do not require every proposed runtime scenario to have been executed.

| ID / contract A:144-152 | Verdict | Exact evidence; normal and failure mapping | Gap / acceptance check |
|---|---|---|---|
| W1 — install/start | PARTIAL | W:9-19 maps setup/preview/up, CLI lifecycle, daemon DB and readiness. Ambiguous-name rejection is source-cited, but failure effects and diagnostics combine prerequisite, listener and config cases with inference or intent. | G3: one coherent source-inspected failure from input to exact diagnostic and cleanup/restart effect; separately label the other untraced cases. |
| W2 — put agents in a project | PARTIAL | W:21-31 maps up, adapters, topology/session effects and status. Failure evidence largely traces restore/missing tokens, not initial exit-before-ready or partial launch cleanup. E6:976-1006 supports the missing-token decision, not all initial-launch effects. | G3: trace launch writes/readiness/compensation and a visible initial-launch failure, or explicitly identify the unavailable link and next source. |
| W3 — assign/follow work | PARTIAL | W:33-43 distinguishes durable queue writes, ordinary nudges, handoff wake intent and delivery/acceptance. E4:1608-1669 confirms terminal transaction then delivery. Failure entries switch between send safety and queue handoff without a complete matched client/output path. | G3: connect one queue failure and its response/output to the same action and transaction; do not generalize handoff guarantees to ordinary create. |
| W4 — understand results | PARTIAL | W:45-55 cites status/ps/history, durable state and live probes. Read effects and visible results are largely summary/inference; client reconnect and per-consumer presentation are expressly untraced. | G3: exact status/read/formatter source and one disagreement or disconnected-client path; optional surfaces may be explicitly deferred. |
| W5 — stop/return | PARTIAL | W:57-67 maps down/restore and snapshot-failure continuation. E2:103-143 confirms continued teardown and delegated service cleanup; E3:125-164 confirms metadata, not a Git archive. W:65 cites up output for both teardown and restore, leaving down output unproved. | G3/G4: down response/diagnostic source and delegated cleanup effects, with tracked/index/untracked distinctions and source-only unknowns. |
| W6 — distinguish daemon readiness, agent readiness, delivery and acceptance | PARTIAL | W:15-19,27-29,39-43 separates some concepts; delivery is explicitly not acceptance. Daemon/agent readiness boundaries still rely on guide wording. | G3: name each readiness authority/probe and its limit; no runtime readiness claim needed. |
| W7 — reuse selected flows/claims and retain provisional scope | PASS | W:5-7,31,43,55,67,82 cites prior research and disclaims product-scope approval and runtime success. | Citation precision remains V2. |
| W8 — cross-check actual capability artifact (A:182-189) | NOT EVIDENCED | W:69-82 substitutes the Node inventory for C; W contains no reference to the dedicated capability artifact or its commit. A:95-103 explicitly distinguishes these outcomes. | G3: crosswalk all C rows plus C1 omissions to a journey or justified deferral, naming C's revision. |

## State and invariant acceptance — c28a2

| ID / contract | Verdict | Exact evidence and finding | Gap / acceptance check |
|---|---|---|---|
| S1 — core stores/files/caches/live processes; authority and lifetimes (A:161-163) | PARTIAL | S:58-65 covers project/catalog, Git/index, worktrees, rig/node/session, transcripts, queue, events/snapshots and config. S:61 discusses liveness, but there is no dedicated daemon/live-process/cache lifecycle inventory or explicit exclusion. | G4: map these surfaces to journey effects, with authority, lifetime and justified absent/out-of-scope entries. |
| S2 — creators/writers/deleters and reconciliation (A:161-163) | PARTIAL | S:58-65 identifies many writers and retention paths; project/worktree deletion and broader config cleanup are explicitly unresolved. This is honest uncertainty, but does not complete the ownership map. | G4: bound the source search; identify actors/path or explicit absence with next evidence, not owner answers as substitute for source tracing. |
| S3 — allowed transitions, transactions and event ordering (A:163-165) | PARTIAL | S:61-64,78-82 separates DB transactions from external effects. E4:1608-1669 and E5:52-91 support this boundary. Allowed state transitions and all client observation/reconnect rules are not enumerated. | G4: transition/enforcement and observation rows; split protocol uncertainty from future demonstrations. |
| S4 — session/liveness/resume distinctions (A:164) | PASS | S:61-62,72-88 distinguishes role, occupant, session, tmux and native resume; E6:976-1006 confirms decision-needed without a new launch. | No live model responsiveness or native recovery success inferred. |
| S5 — queue/history and notification guarantees (A:164-165) | PARTIAL | S:63 covers canonical queue, audit archival, wake intents and retirement; it does not give a complete transition/enforcement/failure pairing per invariant. | G4: link allowed mutations, failed transaction and post-commit wake recovery separately. |
| S6 — events/client observation (A:165) | PARTIAL | S:64 and E5:52-91 support persistence before notification, but subscriber reconnect/replay truth and caches remain unresolved. | G4, coordinated with G3: cite client source of truth and disagreement handling. |
| S7 — project/worktree metadata, snapshots/transcripts (A:165-170) | PASS | S:58-62,83-95 explicitly distinguishes staged/index, unstaged and untracked files from snapshot metadata, pane transcript and native conversation. E3:125-164 corroborates snapshot payload scope. | This PASS is for distinctions, not completed deletion mapping (S2). |
| S8 — owned versus user-owned config/hooks (A:166-167) | PARTIAL | S:65 names managed tmux blocks, config reset, adapter hooks, projection hashes and cleanup; wider plugin/hook collisions remain open. E2:202-231 can write or delete a selected guidance file. | G4: delegated cleanup and collision rules, never infer all user config survives from a projection hash. |
| S9 — each invariant: enforcement/contract, failure/restart, missing proof (A:168-170) | PARTIAL | S:72-95 contains six useful boundaries but not a per-invariant enforcement/failure/next-proof crosswalk. S:151-157 combines source omissions with live observations. | G4: one complete row per invariant, with unresolved source links and future runtime proof separated. |
| S10 — reuse linked findings; no teardown-absence preservation claim (A:171-172) | PASS | S:20-26,38-52,59-65 reuses linked claims and explicitly avoids treating one non-deletion path as global preservation. | Artifact revision precision remains V3. |

## Common evidence, consistency and scope checks

| ID / contract A:174-191 | Verdict | Exact evidence and finding | Gap / acceptance check |
|---|---|---|---|
| V1 — C citations/links | PARTIAL | C:7 has a broken heading anchor. C:45 cites imports plus dependency interface lines as command composition; E1:93-110 is an interface, not registration. Other rows use indirect document references. | G2: correct anchor and mechanism ranges; pin reused document revisions. |
| V2 — W citations/links | PARTIAL | W:5 has a broken charter anchor. W:16,27-29,39-41,51,62-65 uses basenames or bare ranges; W:5 broadly pins citations to source revision despite citing later claim reviews. | G3: full paths, explicit document revisions, correct anchor, and semantic check of every consequential link. |
| V3 — S citations/links | PARTIAL | S:5-8 globally pins line ranges to source baseline; S:61,77 cites later A5 review lines outside that baseline. S:102-125 mixes worktree snapshots with revision evidence. | G4: separate source, reviewed-claim and historical-worktree revisions. |
| V4 — evidence IDs and confidence for consequential findings | PARTIAL | C:43-57 has capability labels; W:9-67 has trace labels; S:58-95 has state/invariant labels. None consistently supplies stable finding IDs plus explicit confidence per consequential finding as A:176-178 requires. | G2/G3/G4: stable IDs, scoped claim, revision, exact evidence and confidence for each finding. |
| V5 — observed/intent/inferred/unresolved discipline | PARTIAL | C:19-28 and W:7 define labels; S:10-18 defines source observation as behavior **or intent**, and S:30 labels an owner requirement a source observation. Competing explanations for inferences are not consistently given (H:58-63). | G2/G3/G4: use consistent labels; distinguish stated requirements from mechanisms; qualify inference and alternative explanation. |
| V6 — unresolved points name next evidence | PARTIAL | C:75-82, W:92-102 and S:127-157 offer next actions, but source investigations, owner answers and runtime tests are sometimes combined as review blockers. | G2/G3/G4: classify each next step as source correction or future evidence; P1/P2 are not source-acceptance blockers. |
| V7 — capability/workflow/state reconciliation | PARTIAL | S:110-113 agrees with W on selected boundaries; S:118-122 checks C visibility. W8 remains absent and there is no complete effect-to-state crosswalk. | G3/G4: reconcile each workflow effect to a ledger row and capability; unresolved disagreements remain explicit. |
| V8 — current versus historical coordination statements | PARTIAL | S:114-125,129-146 calls W uncommitted and cites pending 92f0b approval from a historical worktree. A:254-262 also contains historical pending language. W now has checkpoint evidence; neither that nor board testimony proves acceptance. | G4/G5: date each claim; record checkpoint availability separately from accepted integration; reconcile authority from actual artifact/commit records. |
| V9 — no runtime, owner, retention or gate success invented | PASS | C:30-32, W:5-7, S:142-157 explicitly disclaim these outcomes. | No owner answers or runtime execution required to pass this disclaimer check. |
| V10 — original commit diff ownership | PASS | Git diffs for C's initial/update commits, W checkpoint and S commit each change only their named research document. | No source artifact, claim, code or register correction was made by this review. |

## Citation-validation record and semantic checks

Read-only Git-object checks examined explicit repository-path citation
occurrences (not unique files or every abbreviated reference). At source commit
`9db3ed6c`, C's 12 full-path occurrences had valid ranges. W had 37 full-path
occurrences: four references to later claim/register content exceeded the
declared baseline. S had 69 repository-relative full-path occurrences, excluding
two absolute worktree references: two A5 ranges exceeded that baseline.
Using documentation baseline `aa51d358` for breakdown references made all
12/37/69 ranges valid. That normalization locates the likely intended evidence;
it does **not** repair the artifacts' incorrect blanket revision declarations.
Basename citations still need explicit paths, and valid bounds do not establish
semantic support.

The specific mismatches are W:31 (A3:36-48), W:55 (A2:34-47), W:64
(A8:27-41), W:71 (register:39-58,76-113), and S:61,77 (A5:27-37).
The artifacts themselves contain the full claim paths. These are review-content
references, not changes to the inspected implementation baseline.

Of nine Markdown link occurrences checked against documentation baseline
`aa51d358`, seven resolve and two heading anchors fail:

- C:7 should identify `83-checkout-and-code-inventory`, not
  `checkout-and-code-inventory`.
- W:5 should identify `a-provisional-workflow-to-investigate-first`, without
  the extra leading hyphen.
- C's other three, W's other one and S's three link occurrences resolve there.
  Some targets are absent on publication-base remote main; these historical
  checks are not claims about remote integration.

Semantic source checks directly read E1:1-110, E2:103-231, E3:125-166,
E4:1608-1669, E5:52-91 and E6:976-1006. They confirm, respectively:
visible host/workflow entry surfaces and a mis-targeted composition citation;
best-effort snapshot with continued and delegated cleanup; structured DB
snapshot plus event transaction; transactional handoff wake intent followed by
delivery; persistence separate from notification; and missing-token stop-and-ask.
These checks support the bounded findings above, not an exhaustive independent
re-trace of every source citation. Unchecked semantic completeness remains
PARTIAL rather than being promoted by a successful bounds check.

## Gap routing and precise acceptance checks

Task IDs below were read from prompts, not columns, in the mutable snapshot at
`/home/kkk/.cline/kanban/workspaces/openrig-breakdown/board.json` on 2026-09-27.
This is assignment evidence only, not versioned completion evidence. No card is
created, reassigned or closed here. Local G labels are review findings, not new
board IDs. Existing specialists retain exclusive artifact ownership.

| Gap | Existing task / bounded routing | Acceptance check |
|---|---|---|
| G1 — missing/dispersed surface coverage (C1-C2) | **662c4** owns inventory corrections; **df6f0** already owns optional consumer research. Do not open another CAP-4 task. | Give host/multi-host and workflow engine explicit rows/dispositions; identify owner, entry/effect, dependencies/artifacts and journey or unknown relevance. Optional-surface evidence is supplied by df6f0, not independently duplicated. |
| G2 — inventory evidence hygiene (C2, V1, V4-V6) | **662c4** | Fix anchor/composition evidence, pin prior-document revisions, distinguish snapshot storage, add finding IDs/confidence and source-versus-future gap classification. Verify corrected ranges semantically. |
| G3 — workflow closure (W1-W6, W8, V2, V4-V7) | **b0da1**, the existing CAP-2/WT-1–WT-5 follow-up; **a4d7b** remains original artifact owner. Coordinator must serialize any same-path correction. | Five normal/failure maps each identify input, command/client, runtime owner, durable/process effect and visible result with exact evidence or a precisely named unresolved source link. Include cleanup/restart, actual C crosswalk, full citations and evidence labels. Runtime proof is not an exit condition. |
| G4 — state/source consistency closure (S1-S3, S5-S6, S8-S9, V3-V8) | **821e1**, existing CAP-3/WT-8 follow-up; **c28a2** remains original artifact owner. Serialize shared-path writes. | Each core state/invariant row identifies authority, actor/lifetime, mutations/removal, transitions/enforcement, atomic/event boundary, failure/restart and disagreement recovery. Trace delegated cleanup or bound explicit absence. Map W effects, pin revisions and label source unknowns separately from future demonstrations. |
| G5 — provenance and distribution (V8 and remote availability) | **d47aa** coordination; **f4958** only under serialized integration handoff | Supply immutable artifact/review refs, distinguish checkpoint versus acceptance, and reconcile dated records without treating columns as proof. Publish prerequisite evidence through its existing authorized integration process, not by expanding this PR. No shared-status edit is authorized here. |

No new follow-up card is needed: the inspected task prompts cover these gaps.
Cross-document correction ownership must be confirmed before those tasks edit
the same path; listing two task IDs does not authorize concurrent writes.

## Future evidence proposals — not matrix blockers

| Proposal | Existing work / acceptance boundary |
|---|---|
| P1 — owner choices and workflow-scope reconciliation | **7aa89** supplies cited requirements/assumptions/open-choice mapping; **55605** supplies options for N1–N10. Research completion requires the brief/table, not owner answers. Only the human can later decide retained scope, compatibility and exceptions. |
| P2 — executable runtime evidence | **d1795** supplies isolated verification protocols, authorization checklist, observables and cleanup. Protocol completion requires all proposed results labeled NOT RUN, not a live experiment. Separately authorized later execution could test install/start errors, launch cleanup, queue/reconnect, dirty tracked/staged/untracked preservation, snapshot failure, native resume and update/rollback. |
| P3 — broader comparison/measurement/history | C:79-81 and H:78-80 identify later counting, history and architecture questions. These are outside A's three bounded source-research contracts. Do not turn them into migration implementation cards or additional acceptance blockers for this matrix. |

## Matrix verification

The completed matrix has 33 criterion rows: 9 PASS, 23 PARTIAL and 1 NOT
EVIDENCED. A read-only validator resolved all 11 evidence aliases to their Git
objects and checked 102 alias/range references for valid inclusive bounds.
The repository documentation guard passed; its five Node test-runner tests
also passed. Those are documentation-tool checks, not product behavior tests.
Whitespace and one-file diff scope are checked before publication. No build,
generated-file update or product lifecycle is needed for this Markdown change.

The review is complete with explicit partial/absent evidence and routed checks.
Artifact correction, accepted integration, owner choices and runtime evidence
remain separate outcomes. No source artifact has been rewritten to make this
matrix pass, and no Gate A–D result follows from publishing it.
