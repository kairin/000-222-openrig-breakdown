# 4. Review method

## 4.1 The Gemini review

Gemini Deep Research wrote a review of OpenRig on 2026-09-26. The review has two documents:

- A written report with 29 sources.
- An infographic page with 5 sections.

The two documents are in `gemini-review/sources/`. They are not changed.

## 4.2 Limits of the review

Use the review as a list of statements to examine. Do not use it as a list of facts. These are the reasons:

- Gemini did not read the code. It used the website, the documentation, the README and a Reddit thread.
- Some sources are secondary, for example a Reddit thread and a review web site.
- The infographic gives scores from 1 to 10. The review does not tell how it calculated these scores.
- The review compares OpenRig with Cline CLI, OpenHands, MetaGPT and CrewAI. These comparisons do not tell about the code in this repository.

## 4.3 How the review is divided

The review is divided into 24 claims. Each claim is a statement that you can examine in the code. Each claim has its own folder in `gemini-review/claims/`.

| Group | Claims | Subject |
| :--- | :--- | :--- |
| `architecture/` | A1 to A8 | What OpenRig is and how it operates |
| `friction/` | F1 to F11 | Why OpenRig is difficult to use |
| `strengths/` | S1 to S5 | What OpenRig does well |

Each claim folder has one `README.md`. The file contains:

- The claim in one sentence.
- The exact text from the report and from the infographic.
- The sources that Gemini gave.
- An instruction that tells where to look in the code.
- Empty sections for the result, the evidence and notes.

The text that is not a claim is in `gemini-review/context/`. This text includes the Cline CLI section, the alternative tools and the Gemini conclusion.

## 4.4 Procedure

1. Select a claim.
2. Read the claim `README.md`.
3. Find the code or the document that proves the claim or disproves it.
4. Write the result in the claim `README.md`. Use one of these values:
   - `yes`: The claim is correct.
   - `part`: The claim is partially correct.
   - `no`: The claim is not correct.
5. Write the file name and the line number of the evidence.
6. Write the same result in the register in `gemini-review/README.md`.
7. Do steps 1 to 6 again for all the claims.

NOTE: Examine the friction claims (F1 to F11) first. They have the most effect on the simplification.

## 4.5 Claims that go together

Some claims describe one function from different directions. Make one decision for each group:

| Function | Claims |
| :--- | :--- |
| Seats that continue after a process stops | A5, F4, S2 |
| Snapshots and restore | A8, F9, S5 |
| Rules for ownership and handoff | F5, S3 |

## 4.6 First observations

These observations come from files that were read during the separation work. They are not results. Examine them with the procedure in 4.4.

| Claim | Observation |
| :--- | :--- |
| F6 | `package.json` requires Node.js `^20 \|\| ^22 \|\| ^24`, not "20 or higher". |
| F7 | `archive/configuration/context7.json` says the `first-project` starter requires a Codex CLI with a login. |
| F9 | This claim has the most danger if it is correct: work that is not committed can be lost. |
| F11 | The file `docs/reference/worktree-builds.md` shows that some worktree function can exist. |

## 4.7 After the procedure

1. Put each confirmed claim into one of two lists: strengths or weaknesses.
2. For each weakness, select one action:
   - Remove the function.
   - Hide the function behind a simpler default.
   - Change the name to a word that engineers know.
   - Keep the function, because its value is more than its cost.
3. Write the action and the reason in the claim `README.md`.
