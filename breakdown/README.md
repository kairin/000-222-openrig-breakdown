# OpenRig breakdown

This folder contains the analysis for a simpler version of OpenRig. Start with `01-goal-and-scope.md`.

| Document | Subject |
| :--- | :--- |
| [01-goal-and-scope.md](01-goal-and-scope.md) | The goal and the start point |
| [02-separation-from-upstream.md](02-separation-from-upstream.md) | The work that is complete, and the links to upstream that stay |
| [03-local-environment.md](03-local-environment.md) | Conditions on this computer |
| [04-review-method.md](04-review-method.md) | How to examine the Gemini review |
| [05-simplification-rules.md](05-simplification-rules.md) | Rules for changes to the code |
| [06-open-questions.md](06-open-questions.md) | Decisions that the owner must make |
| [07-review-task-list.md](07-review-task-list.md) | Review tasks, status, dependencies and completion evidence |
| [08-current-state-evidence.md](08-current-state-evidence.md) | Current source, history, runtime flows and Rust boundary evidence |
| [09-adversarial-review-and-research-charter.md](09-adversarial-review-and-research-charter.md) | Research plan and criteria for evaluating simplifications |
| [10-rust-and-node-removal-plan.md](10-rust-and-node-removal-plan.md) | Source-cited Node inventory, Rust/removal boundaries, owner decisions, conditional stages and acceptance criteria (fa0bb) |
| [gemini-review/](gemini-review/) | The Gemini review, divided into one folder for each claim |

The documents use ASD-STE100 Simplified Technical English.

As of 2026-09-27, all 24 claim pages agree with the
[claim register](gemini-review/README.md): 6 `yes`, 18 `part`.
[T2–T4](07-review-task-list.md#tasks) are source-review complete only.
T5 remains incomplete pending owner input, and T6–T9 remain blocked.

The Rust/Node-removal document is a planning baseline, not completed migration
work or Gate D approval. No Gate A–D is declared passed. Read the
[source-review limits and linked conclusions](gemini-review/README.md#source-review-status-2026-09-27)
before treating source mechanisms as runtime-verified behavior. The claim
reviews do not establish successful installation, recovery, work preservation,
measured usability or completion of the wider research program.
