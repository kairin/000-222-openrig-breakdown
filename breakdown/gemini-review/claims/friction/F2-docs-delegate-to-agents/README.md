# F2: The docs tell humans to hand setup and operation over to their agents.

**Verdict:** yes — current documentation explicitly asks the user to delegate permission configuration and reviewed work to agents. This does not mean all setup or operation requires an agent.

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> The documentation frequently instructs human developers to delegate configuration and execution directly to their agents16.

## What the infographic says

Verbatim from `sources/agent_fleet_architectures_infographic.html`.

> OpenRig’s CLI is explicitly designed to be driven by both human developers AND autonomous AI agents. Because commands are built for automated agent consumption, documentation frequently tells humans: "Instruct your agent to run the rig CLI commands for you."

> It doesn't run agents itself; it hijacks host tmux sessions running Claude Code or Codex. It replaces standard developer terms with telecom abstractions (Rigs, Pods, Seats) and expects AI agents to execute its CLI commands.

## Sources Gemini cited

- [16] [OpenRig is the Terraform for Coding Agents (Launch Demo)](https://www.openrig.dev/blog/orchestrator)

## How to check

Read `docs/reference/getting-started.md` and the README. Can a human finish setup without asking an agent to do it?

## Evidence from this repo

Reviewed at source commit `9db3ed6c406be5c3d9a84720383fcf6b543169e6` (2026-09-27).

- **Stated intent:** `/home/kkk/.cline/worktrees/43d5c/openrig-breakdown/docs/reference/getting-started.md:8–15` links “Ask your agent to configure that choice”; lines 239–252 explicitly say the user chooses scope and the agent inspects/applies permissions, with an example delegation prompt.
- **Stated intent:** `/home/kkk/.cline/worktrees/43d5c/openrig-breakdown/docs/reference/getting-started.md:101–125` directs the human to send an outcome to the owner, which creates/claims a durable task and routes an independent check; the human can inspect queue state directly.
- **Counterevidence to mandatory delegation:** `/home/kkk/.cline/worktrees/43d5c/openrig-breakdown/docs/reference/getting-started.md:22–80` supplies direct TUI, prerequisite/login, preview and launch instructions. Lines 268–282 describe optional recipes and editing a user-owned spec.
- **Observed in source:** `/home/kkk/.cline/worktrees/43d5c/openrig-breakdown/packages/cli/src/front-door.ts:276–283` implements the human bare-command TUI path; `/home/kkk/.cline/worktrees/43d5c/openrig-breakdown/packages/cli/src/commands/up.ts:68–88` exposes direct YAML launch and plan options.

## Notes

- **Scope:** the verdict confirms the narrow documentary claim, not Gemini's implication that humans cannot operate the product themselves. The infographic's separate process-hosting assertion is not evidence for delegation.
- **Contradiction / limitation:** current documentation contains both agent-delegation advice and direct human instructions. These coexist; interpreting the former as exclusive contradicts the latter. “Frequently” has not been quantified across the complete documentation corpus.
- **Inference / evidence still needed:** whether a new human can finish every setup step unaided needs a walkthrough on a specified clean environment; documentation and available command paths alone cannot prove that outcome.
- **Runtime-unverified:** no setup, login, launch, permission change or usability trial was performed.

