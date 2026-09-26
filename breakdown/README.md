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
| [08-current-state-evidence.md](08-current-state-evidence.md) | Current source, history, runtime flows and Rust boundary evidence |
| [09-adversarial-review-and-research-charter.md](09-adversarial-review-and-research-charter.md) | Research plan and criteria for evaluating simplifications |
| [10-rust-and-node-removal-plan.md](10-rust-and-node-removal-plan.md) | Source-cited Node inventory, Rust/removal boundaries, owner decisions, conditional stages and acceptance criteria (fa0bb) |
| [gemini-review/](gemini-review/) | The Gemini review, divided into one folder for each claim |

The documents use ASD-STE100 Simplified Technical English.

The Rust/Node-removal document is a planning baseline, not completed migration
work or Gate D approval. T2–T9 and the existing research gates are not completed
by this deliverable. Read the evidence limits before treating source mechanisms
as runtime-verified behavior.
