# 1. Goal and scope

## 1.1 Goal

The owner of this repository does not send changes to the upstream project.

The goal is a simpler version of OpenRig. The upstream version is too difficult to understand and to operate.

A simpler tool must do these things:

- Give less information that a person must keep in mind at one time.
- Use words that software engineers know.
- Keep the functions that have a real value for the owner.

The requested direction is a Rust-owned core and removal of the project's Node
dependency. This is a goal, not a completed port or an approved architecture.
The [Rust and Node-removal plan](10-rust-and-node-removal-plan.md) defines the
runtime, build/test/tooling, UI, embedded-script and external-tool boundaries.
The owner must confirm the removal boundary and compatibility promises. Keep
the research gates in [09](09-adversarial-review-and-research-charter.md): source
inventory alone does not authorize implementation or complete T2–T9.

The existing personal-use, Claude Code/Codex CLI, safe-stop and independent
operation goals remain in force (`README.md:3-6,23-34`). Rust must serve those
goals, not preserve every upstream feature merely to port it.

## 1.2 Start point

The repository is a copy of `mvschwarz/openrig`. The historical rows describe
the 2026-09-26 starting point; source counts were rechecked on 2026-09-27 at
`9db3ed6c406be5c3d9a84720383fcf6b543169e6`. The count definitions and commands
are in [10](10-rust-and-node-removal-plan.md#reproducible-count-reconciliation).

| Item | Value |
| :--- | :--- |
| Version | 0.5.16 |
| Last upstream commit | `5c547030` |
| Number of commits | 3033 |
| Package directories / npm workspaces | 5 directories (`cli`, `daemon`, `tui`, `ui`, `test-system`); 4 active workspaces, excluding `test-system` (`package.json:7-12`) |
| TypeScript files in `packages/` | 2,364 tracked `.ts`/`.tsx` files, including tests; supersedes the undefined approximate 2018 count |
| Files in `scripts/` | 57 tracked files, not all executable scripts |
| License | Apache License 2.0 |

## 1.3 Scope of this folder

The `breakdown/` folder contains the analysis of OpenRig. It contains these documents:

| Document | Subject |
| :--- | :--- |
| `01-goal-and-scope.md` | The goal and the start point |
| `02-separation-from-upstream.md` | The work that is complete, and the links to upstream that stay |
| `03-local-environment.md` | Conditions on this computer |
| `04-review-method.md` | How to examine the Gemini review |
| `05-simplification-rules.md` | Rules for changes to the code |
| `06-open-questions.md` | Decisions that the owner must make |
| `08-current-state-evidence.md` | Source evidence and its limits |
| `09-adversarial-review-and-research-charter.md` | Research gates and alternatives |
| `10-rust-and-node-removal-plan.md` | Node inventory, boundaries and conditional migration stages |
| `gemini-review/` | The Gemini review, divided into one folder for each claim |

The `docs/` folder contains the upstream documents. These documents describe the upstream version. When you change the code, these documents can become incorrect.

## 1.4 Language

These documents use ASD-STE100 Simplified Technical English. Names of commands, files and products stay in their initial form.

NOTE: There was no ASD-STE100 dictionary check on this text. The text follows the writing rules. Some words can be outside the approved vocabulary.
