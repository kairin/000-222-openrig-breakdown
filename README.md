# Simplified agent-team harness (working name)

A personal, simplified derivative of the open-source OpenRig project. It runs
several Claude Code and Codex CLI agents together as one team. The goal is a
smaller tool that fits one person's workflow, with much less to learn and
keep in mind.

This project is independent. It is not affiliated with, endorsed by or
supported by the authors of OpenRig. It does not accept contributions to, or
send changes to, the original project.

## Status

Research. The code is still the same as upstream OpenRig 0.5.16. No
simplification has been made yet.

The current work reads the source and records evidence: what each workflow
does, which state it keeps, and which capabilities are optional. The owner's
stated goal is a fully Rust tool with no Node.js. The research examines how to
reach that goal. No design or plan is approved. Open decisions for the owner are in
[research-owner-decision-brief.md](breakdown/research-owner-decision-brief.md).

Until the tool is renamed, it still uses the upstream command names (`rig`,
`openrig-tui`), package names (`@openrig/*`) and state folder (`~/.openrig`).
Do not install a build of this repository next to an installed upstream
`@openrig/cli`. The two would share the same commands and state. See
[breakdown/03-local-environment.md](breakdown/03-local-environment.md).

## Goals

- **Less to learn.** Use words engineers already know (agent, task, branch)
  instead of a new vocabulary where the new word adds nothing.
- **Fewer moving parts.** Keep the functions that earn their cost. Remove the
  rest, together with the files they write on the machine.
- **Readable by a person.** A human should be able to set up, run and fix the
  tool without asking an agent to do it for them.
- **Safe with work in progress.** Stopping the team must never lose work that
  is not committed.
- **Self-contained.** No downloads from, or dependence on, the original
  project's services.

## How the work is organised

All analysis lives in [`breakdown/`](breakdown/README.md):

| Document | Subject |
| :--- | :--- |
| [01-goal-and-scope.md](breakdown/01-goal-and-scope.md) | The goal and the starting point |
| [02-separation-from-upstream.md](breakdown/02-separation-from-upstream.md) | What has been separated, and the links to upstream still in the code |
| [03-local-environment.md](breakdown/03-local-environment.md) | Conflicts with an installed upstream version |
| [04-review-method.md](breakdown/04-review-method.md) | How each claim about the tool is checked against the code |
| [05-simplification-rules.md](breakdown/05-simplification-rules.md) | Rules for changing the code safely |
| [06-open-questions.md](breakdown/06-open-questions.md) | Decisions still to make, including the final name |
| [07-review-task-list.md](breakdown/07-review-task-list.md) | Review tasks, status, dependencies and completion evidence |
| [08-current-state-evidence.md](breakdown/08-current-state-evidence.md) | Source, history and runtime evidence for the current code |
| [09-adversarial-review-and-research-charter.md](breakdown/09-adversarial-review-and-research-charter.md) | The research plan and the criteria for a simplification |
| [10-rust-and-node-removal-plan.md](breakdown/10-rust-and-node-removal-plan.md) | Node.js inventory, Rust boundaries and conditional stages (a plan, not approved) |
| [11-coordination-outcomes.md](breakdown/11-coordination-outcomes.md) | Research ownership, acceptance dependencies and migration dispositions |
| [gemini-review/](breakdown/gemini-review/README.md) | An outside review, split into 24 claims to check |

Research notes (source reading only; none is an approved decision):

| Document | Subject |
| :--- | :--- |
| [research-workflow-scope.md](breakdown/research-workflow-scope.md) | Candidate operator workflows (CAP-1) |
| [research-workflow-traces.md](breakdown/research-workflow-traces.md) | Traces of setup, work, inspection and recovery |
| [research-state-invariants.md](breakdown/research-state-invariants.md) | State invariants and recovery boundaries (CAP-3) |
| [research-capability-inventory.md](breakdown/research-capability-inventory.md) | Optional and adjacent capabilities (CAP-4) |
| [research-owner-decision-brief.md](breakdown/research-owner-decision-brief.md) | Owner decisions N1–N10 for Rust and Node.js removal |
| [research-runtime-verification-plan.md](breakdown/research-runtime-verification-plan.md) | Protocols for runtime checks (not run) |
| [research-runtime-verification.md](breakdown/research-runtime-verification.md) | Blocked runtime verification for WT-10, CAP-8 and Node.js removal |
| [research-review-matrix.md](breakdown/research-review-matrix.md) | Independent review of research artifacts |
| [research-coordinator-review-a6c2b.md](breakdown/research-coordinator-review-a6c2b.md) | Coordinator review of three research artifacts |

## What the code does today

- A local daemon (Hono HTTP server with an SQLite database) keeps track of a
  team of agents.
- Each agent is a Claude Code or Codex CLI session running in tmux.
- Teams are described in YAML files and started with `rig up`.
- Agents send messages to each other and share a task queue.
- A terminal UI (`rig tui`) shows the team.

## Before you run it

The current code changes files outside this repository. It writes hooks and
trust settings into `~/.claude.json`, the workspace `.claude/settings.local.json`,
`~/.codex/config.toml` and `~/.tmux.conf`. It also tries to download a plugin
from the original project's GitHub account each time the daemon starts. Back
up those files first. The full list is in the
[original README](archive/documents/README-upstream-original.md#what-openrig-changes-on-your-machine).

## Build and test from source

Requires Node.js 20, 22 or 24, and tmux.

```bash
npm install
npm run build
npm test
```

Record the test results before changing code. Some upstream tests already
failed before this project started (see
`.evidence/AB-full-suite-preexisting-failures.txt`).

## Origin and credit

This project is derived from [OpenRig](https://github.com/mvschwarz/openrig)
by Mike Schwarz, version 0.5.16. An archived copy of the upstream README is in
[README-upstream-original.md](archive/documents/README-upstream-original.md).

"OpenRig" is the name of the original project. It is used here only to
describe where this code came from. It is not the name of this project.

## License

Apache License 2.0. See [LICENSE](LICENSE). The original copyright notice is
kept. Files changed in this project will be marked as changed.
