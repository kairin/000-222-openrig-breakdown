# 7. Review task list

This list tracks the work needed to review the Gemini claims and reach a
well-supported simplification plan. The claim register in
[`gemini-review/README.md`](gemini-review/README.md) remains the source of truth
for the verdict of each individual claim. This list tracks larger tasks and
their dependencies.

## Status key

- **Done**: The stated completion evidence is recorded in the linked document.
- **Source-review complete**: Claim pages and register agree, with source
  evidence and explicit limits. Runtime validation and broader gates are not
  complete merely because this status is reached.
- **In progress**: Work has started, but the completion evidence is incomplete.
- **Not started**: Work has not started.
- **Blocked**: Work cannot be completed until a dependency or owner decision is
  resolved.

Change a status only when the completion evidence supports it. Add evidence or
decision links when a task is marked Done. If the plan changes, record why in
the relevant research or decision document.

## Tasks

| ID | Task | Depends on | Status | Completion evidence / next step |
| :--- | :--- | :--- | :--- | :--- |
| T1 | Record the derivative's goal, current code baseline, and source-evidence limits. | — | Done | Recorded in the root [`README.md`](../README.md), [`08-current-state-evidence.md`](08-current-state-evidence.md), and [`09-adversarial-review-and-research-charter.md`](09-adversarial-review-and-research-charter.md). Update these if consequential facts change. |
| T2 | Review friction claims F1–F11 against current code and documents. | T1 | Source-review complete | All 11 pages have sourced verdicts and limits; the [friction register](gemini-review/README.md#friction) agrees: 2 `yes`, 9 `part`. Runtime outcomes remain unverified. |
| T3 | Review the linked claim groups together: A5/F4/S2, A8/F9/S5, and F5/S3. | Relevant T2 claims; architecture/strength evidence as needed | Source-review complete | All eight linked pages record shared conclusions, exact evidence and limits; see the [linked-group conclusions](gemini-review/README.md#linked-group-conclusions). These overlap T2/T4, not eight additional claims. |
| T4 | Review architecture claims A1–A8 and strengths S1–S5. | T1; coordinate linked claims through T3 | Source-review complete | All 13 pages have verdicts and source locations matching the [architecture](gemini-review/README.md#architecture) and [strengths](gemini-review/README.md#strengths) registers: 4 `yes`, 9 `part` combined. No runtime confirmation is inferred. |
| T5 | Reconcile the owner questions with the current README and record decisions or remaining questions. | T2–T4 findings where relevant; owner input for unresolved choices | In progress — owner input incomplete | [`06-open-questions.md`](06-open-questions.md) records existing README answers and open choices, including N1–N10. Obtain explicit owner decisions and reconcile relevant claim findings; do not infer answers from source-review completion. |
| T6 | Classify verified claims as strengths or weaknesses and choose an action for each weakness. | T2–T5 | Blocked | Claim pages state the evidence-based classification and whether to remove, simplify by default, rename, or keep the function, with a reason. |
| T7 | Compare simplification candidates against the owner's goals and dependencies. | T6; Gate A understanding | Blocked | A decision matrix records user value, dependencies, behavior/data effects, removal cost, expected change to the baseline, and evidence that could disprove the expected benefit (Gate B). |
| T8 | Evaluate a Rust direction against the same required workflows and contracts. | T7; preservation/recovery obligations understood | Blocked | Record candidate boundaries, owner benefit, compatibility/recovery obligations, alternatives, strongest counter-evidence, and a bounded validation approach (Gate C). |
| T9 | Decide whether the evidence is sufficient for an implementation plan. | T7–T8; unresolved high-impact decisions explicit | Blocked | Record retained scope, operating mode, compatibility promises, open decisions, and a justified go/no-go for implementation planning (Gate D). |

## Working order

1. T2–T4 source review is complete as of 2026-09-27, after all 24 page verdicts
   were reconciled with the register. Recheck affected pages and the register
   together when consequential source facts change; prioritize friction claims.
2. Keep related claims together through T3; do not treat overlapping claims as
   independent evidence. Their runtime questions remain open.
3. Finish owner-input reconciliation in T5 before choosing what to change.
   T6–T9 remain blocked; source review does not finish capability coverage,
   workflow traces, state/invariant research or measured behavior studies.
4. Do not start implementation planning merely because the claim review is
   complete. Pass the evidence and decision gates in order.

No Gate A–D is declared passed by this update. See the
[register's source-review limits](gemini-review/README.md#source-review-status-2026-09-27).

## Historical baseline (2026-09-27, before claim integration)

The following paragraph records the earlier task-list baseline. It is not the
current status; the Tasks table above supersedes it.

> The current claim register marks all 24 claims unverified (`?`). Therefore T2,
> T3, and T4 are not complete, even though T1 has produced useful source-level
> research.

That baseline's research limits still apply. The research does not by itself
establish measured usability, runtime correctness, or successful installation
and recovery; see the limits in
[`09-adversarial-review-and-research-charter.md`](09-adversarial-review-and-research-charter.md).

## Node-elimination dependencies (fa0bb addendum)

Historical provenance: the original addendum copied the main checkout's
pre-existing, uncommitted task-list baseline on 2026-09-27 without changing
that checkout. At that time it recorded T1 Done; T2–T4 Not started;
T5 In progress; T6–T9 Blocked. Those are dated baseline statuses, not the
current T2–T4 results above. This section maps the conditional stages in
[10](10-rust-and-node-removal-plan.md#dependency-aware-stages-conditional-not-authorization)
to the existing tasks; it does not create a second task queue.

| Stage in 10 | Existing dependency | Required evidence before advancing |
|---|---|---|
| 0: scope and evidence | T1 baseline; complete T2–T4 and unresolved T5 work | Source-backed claim reviews and owner choices, required workflow traces, visible contradictions; Gate A understanding. The Node inventory alone is insufficient. |
| 1: retained behavior | T6 depends on T2–T5; T7 depends on T6 and Gate A | Classification and dependency/value matrix, including removal effects and counter-evidence; Gate B. |
| 2: Rust boundary | T8 depends on T7 and understood preservation/recovery obligations | Compare compliant end states against the same contracts; TypeScript-only and Rust-over-Node options are baselines or temporary bridges, not Node-elimination completion; Gate C. |
| 3: planning authorization | T9 depends on T7–T8 and explicit high-impact decisions | Gate D go/no-go, retained scope, compatibility promises and a path to full project Node elimination. No approval is inferred from this addendum. |
| 4: later bounded replacement | T9/Gate D approval and a separately approved implementation plan | Isolated end-to-end proof, baseline comparison, rollback and no-loss tests. |
| 5: later runtime transfer | Stage 4 proof and agreed contracts | Core/state/lifecycle and consumer/hook/runner dependencies addressed; existing Node paths remain transitional until replaced or retired. |
| 6: later distribution/tooling cutover | All retained runtime paths and rollback verified | Installer, shipped scripts, UI build choice, generators, tests, packaging and CI have a non-Node path or are retired. |
| 7: later retirement verification | Stages 4–6 evidence | Clean-environment acceptance from 10, documented external-tool boundary and safe recovery; then remove obsolete dependencies. |

The firm goal is fully Rust for all retained project functionality and full
Node removal from project-owned runtime, installation, update, generation,
build, test and release paths. Owner questions choose scope and migration order, not silent permanent
Node exceptions. A runtime-only milestone is intermediate. A proposed permanent
exception requires an explicit goal change and cannot be called full removal.
None of the later implementation stages has been started by this documentation.
