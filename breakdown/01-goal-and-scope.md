# 1. Goal and scope

## 1.1 Goal

The owner of this repository does not send changes to the upstream project.

The goal is a simpler version of OpenRig. The upstream version is too difficult to understand and to operate.

A simpler tool must do these things:

- Give less information that a person must keep in mind at one time.
- Use words that software engineers know.
- Keep the functions that have a real value for the owner.

## 1.2 Start point

The repository is a copy of `mvschwarz/openrig`. The table gives the condition of the copy on 2026-09-26.

| Item | Value |
| :--- | :--- |
| Version | 0.5.16 |
| Last upstream commit | `5c547030` |
| Number of commits | 3033 |
| Packages | 5 (`cli`, `daemon`, `tui`, `ui`, `test-system`) |
| TypeScript files in `packages/` | Approximately 2018 |
| Scripts in `scripts/` | 57 |
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
| `gemini-review/` | The Gemini review, divided into one folder for each claim |

The `docs/` folder contains the upstream documents. These documents describe the upstream version. When you change the code, these documents can become incorrect.

## 1.4 Language

These documents use ASD-STE100 Simplified Technical English. Names of commands, files and products stay in their initial form.

NOTE: There was no ASD-STE100 dictionary check on this text. The text follows the writing rules. Some words can be outside the approved vocabulary.
