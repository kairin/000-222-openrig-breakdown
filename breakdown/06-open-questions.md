# 6. Open questions

The owner must answer unresolved questions. Existing statements in the root
README answer some older questions; those answers are not new owner decisions
made by this audit. The answers change the work that follows.

| No. | Question | Why the answer is important |
| :-: | :--- | :--- |
| 1 | Answered in the README: personal use for one person's workflow (`README.md:3-6`). Is wider distribution now required? | Do not reopen the stated audience without a changed requirement. |
| 2 | Will you publish the tool, for example on npm? | If yes, you must change the name and the package name. You must also add the license notices (see 2.4). |
| 3 | Answered in the README: Claude Code and Codex CLI (`README.md:3-4`). Is any additional adapter required? | The current goal includes both. Removing one requires an explicit scope change. |
| 4 | Must the tool continue to use tmux? | tmux is a large part of the design (claim A3). Another method changes the architecture. |
| 5 | Do you keep the upstream `docs/` folder? | These documents become incorrect when the code changes. |
| 6 | Do you keep the upstream installation of `@openrig/cli` 0.5.15? | It uses the same command name and state folder as your version (see 3.2). |
| 7 | Do you keep the upstream Git history? | The history shows the origin of the code. It does not cause a problem if you keep it. |

## Rust and Node-removal decisions

The requested direction is Rust and project Node removal. The questions below
define its acceptance boundary, not whether an unapproved rewrite is complete.
All remain open. Evidence and conditional stages are in
[10-rust-and-node-removal-plan.md](10-rust-and-node-removal-plan.md).

| No. | Owner question | Decision needed before |
|---|---|---|
| N1 | Is runtime/install removal an interim milestone, with full build/test/release removal as the end goal? Are any permanent Node tooling exceptions acceptable? | Gate C boundary recommendation and Gate D scope approval. |
| N2 | Is browser JavaScript acceptable? Which of CLI, terminal UI, browser UI and MCP must remain? May UI be optional or prebuilt during transition? | Choosing consumers and UI build/hosting obligations. Current surfaces: `packages/cli/package.json:28-38`; `packages/ui/package.json:7-54`; `packages/cli/src/mcp-server.ts:47-55`. |
| N3 | Must existing SQLite data, configuration, agent resume identities and hook installations migrate? Is an explicit fresh-state mode acceptable? What rollback must remain available? | State ownership transfer. Migration mechanism: `packages/daemon/src/db/migrate.ts:13-42`; hook ownership: `packages/daemon/src/adapters/claude-code-adapter.ts:740-748,780-819`. |
| N4 | Which operating systems and terminal providers are required? Is a daemon essential, optional, or removable if workflows remain safe? | Comparing Rust boundaries at Gate C. This audit does not choose a new process model. |
| N5 | Are vendor-managed agent runtimes/installers outside the Node-free promise? Must setup avoid npm even for external tools? | Installation acceptance. Current setup invokes npm for both agents (`packages/cli/src/commands/setup.ts:504-514,549-559`). Vendor runtime requirements are not verified here. |
| N6 | Is Pi support needed beyond the stated Claude/Codex goal? Which stub/testbed, upgrade and recovery paths must remain? | Retiring Node runners and shipped scripts; inventory in 10 identifies these paths. |
| N7 | What coexistence, renamed command/state paths, offline assets and distribution format are required? | Packaging and cutover. The current README warns of shared upstream commands/state (`README.md:17-21`) and requires no upstream service dependency (`README.md:33-34`). |

Record each later answer with its owner, date, rationale, affected scope and
acceptance test. Do not infer permission from silence. These questions and the
inventory do not finish T5 or unblock T6–T9; claim review and Gates A–D remain.
