# Gemini review of OpenRig (2026-09-26)

Source material produced by Gemini Deep Research when asked to compare
[Cline CLI](https://cline.bot/cli) with [OpenRig](https://openrig.dev/docs/getting-started)
and suggest easier tools for running large agent fleets.

Gemini based the review on OpenRig's website, docs and README, plus a Reddit
thread. It did not read the code. Treat every statement as a claim to verify
against this repo, not as a finding.

## Layout

| Folder | What it holds |
| :--- | :--- |
| `sources/` | The two original Gemini documents, unchanged |
| `claims/` | One folder per claim, holding every passage from both documents that makes that claim |
| `context/` | The parts that are not claims about this repo: the introduction, Cline, the alternatives, Gemini's verdict and the citation list |

Each claim folder has a `README.md` with the claim, the verbatim report and
infographic passages, the sources Gemini cited, a hint on how to check it,
and empty sections for the verdict, evidence and notes. A passage can appear
in more than one claim when it supports several.

## How to work through it

1. **Check each claim against the code.** Fill in its verdict (`yes`
   confirmed, `part` partly true, `no` wrong) and the file and line that
   settles it, in both the claim's README and the register below.
2. **Sort the verified claims into strengths and weaknesses.** A confirmed
   weakness becomes a candidate for simplification.
3. **Decide what to do with each weakness:** remove the feature, hide it
   behind a simpler default, rename it, or leave it alone because it earns
   its cost.

Some claims are two sides of one feature and should be decided together:
A5, F4 and S2 (durable seats); A8, F9 and S5 (snapshots); F5 and S3
(process overhead).

## Claim register

Verdict: `?` unverified, `yes` confirmed, `part` partly true, `no` wrong.

### Architecture

| # | Claim | Verdict |
| :- | :--- | :-: |
| [A1](claims/architecture/A1-no-own-agent-loop/) | OpenRig has no agent loop of its own; it supervises Claude Code and Codex CLI sessions. | ? |
| [A2](claims/architecture/A2-daemon-hono-sqlite-tui/) | OpenRig runs as a local daemon: a Hono HTTP server with SQLite state, plus a TUI. | ? |
| [A3](claims/architecture/A3-runs-in-tmux/) | Agents run inside tmux (or cmux) sessions on the host, not in containers. | ? |
| [A4](claims/architecture/A4-yaml-rigs-pods-seats/) | Topology is declared in YAML as Rigs, Pods and Seats. | ? |
| [A5](claims/architecture/A5-seat-outlives-process/) | A seat keeps its identity, context and queue ownership when its process dies or resets. | ? |
| [A6](claims/architecture/A6-terminal-injection-and-queue/) | Agents talk by injecting text into each other's terminals, or through a transactional task queue. | ? |
| [A7](claims/architecture/A7-discovery-adopts-sessions/) | A discovery engine fingerprints tmux processes and can adopt unmanaged Claude Code or Codex sessions. | ? |
| [A8](claims/architecture/A8-snapshot-restore/) | Snapshots capture a running topology and try to restore it after a reboot. | ? |

### Friction

| # | Claim | Verdict |
| :- | :--- | :-: |
| [F1](claims/friction/F1-cli-built-for-agents/) | The CLI is built for agents and humans at once, so its output and commands favour machines. | ? |
| [F2](claims/friction/F2-docs-delegate-to-agents/) | The docs tell humans to hand setup and operation over to their agents. | ? |
| [F3](claims/friction/F3-unfamiliar-jargon/) | Jargon such as rig, pod, seat and continuity policy replaces familiar terms like repo, branch and task. | ? |
| [F4](claims/friction/F4-seat-vs-process/) | Operators must tell a logical seat apart from the live process behind it. | ? |
| [F5](claims/friction/F5-process-overhead/) | Claim transactions, handoff protocols, verification contracts and CULTURE.md add heavy process overhead. | ? |
| [F6](claims/friction/F6-host-prerequisites/) | It needs Node 20+, tmux and Claude Code or Codex CLIs that are already logged in. | ? |
| [F7](claims/friction/F7-first-project-needs-codex/) | Starter rigs such as `first-project` fail at boot when there is no Codex login. | ? |
| [F8](claims/friction/F8-product-team-rate-limits/) | `product-team` starts four Claude Code instances at once and can hit account rate limits. | ? |
| [F9](claims/friction/F9-teardown-despite-failed-snapshot/) | Teardown carries on even if the snapshot fails, which can lose uncommitted work. | ? |
| [F10](claims/friction/F10-specs-hidden-in-library/) | Rig specs live in an internal library reached through commands, not as editable files in the project. | ? |
| [F11](claims/friction/F11-no-workspace-isolation/) | Agents share one filesystem; there is no per-agent git worktree isolation. | ? |

### Strengths

| # | Claim | Verdict |
| :- | :--- | :-: |
| [S1](claims/strengths/S1-wraps-existing-agents/) | OpenRig works with the CLI agents people already use instead of replacing them. | ? |
| [S2](claims/strengths/S2-durable-seats/) | Seats stay addressable across crashes and restarts (same mechanism as A5). | ? |
| [S3](claims/strengths/S3-ownership-prevents-drift/) | Explicit ownership and handoffs stop long-running swarms from drifting. | ? |
| [S4](claims/strengths/S4-governance-hooks/) | Watchdog schedules and CULTURE.md give some governance over agents. | ? |
| [S5](claims/strengths/S5-fleet-restore/) | Snapshots allow a whole fleet to be brought back after a reboot (same mechanism as A8). | ? |
