# F6: It needs Node 20+, tmux and Claude Code or Codex CLIs that are already logged in.

**Verdict:** part — the ordinary local agent-launch path depends on host tools and native authentication, but “Node 20+” conflates different constraints and Claude/Codex logins are selected-runtime prerequisites, not universal daemon prerequisites.

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> The system depends on pre-existing, fully authenticated command-line harnesses13.

> OpenRig does not manage model credentials directly; instead, it executes host binaries10.

> System Prerequisites: Node.js 20+, tmux, active logins for Claude Code and/or Codex CLI13

## What the infographic says

Verbatim from `sources/agent_fleet_architectures_infographic.html`.

> tmux + Claude/Codex logins

> Requires pre-authenticated Claude Code / Codex CLIs already logged into your machine.

> Requires host Claude / Codex CLI logins

## Sources Gemini cited

- [10] [What is OpenRig](https://www.openrig.dev/what-is)
- [13] [Getting started - OpenRig](https://www.openrig.dev/docs/getting-started)

## How to check

Check `engines` in `package.json` and the doctor/preflight checks in the CLI.

## Evidence from this repo

Reviewed at source commit `9db3ed6c406be5c3d9a84720383fcf6b543169e6` (2026-09-27).

- **Observed in source:** `/home/kkk/.cline/worktrees/43d5c/openrig-breakdown/package.json:30–32` declares Node `^20 || ^22 || ^24`, excluding intervening/future major versions despite their being above 20.
- **Observed in source / unresolved mismatch:** `/home/kkk/.cline/worktrees/43d5c/openrig-breakdown/packages/cli/src/system-preflight.ts:34,124–159` instead accepts any Node major at least 20 and requires an available tmux control probe. Lines 161–192 additionally check writable state locations and the daemon endpoint, but do not check model login. `/home/kkk/.cline/worktrees/43d5c/openrig-breakdown/packages/cli/src/commands/up.ts:135–186` runs this system preflight before auto-starting a stopped daemon.
- **Stated intent:** `/home/kkk/.cline/worktrees/43d5c/openrig-breakdown/docs/reference/getting-started.md:54–64` requires tmux and authenticated Codex for this starter, explicitly not a Claude login or Herdr plugin, and distinguishes full `rig setup` checks from starter necessities.
- **Observed in source:** `/home/kkk/.cline/worktrees/43d5c/openrig-breakdown/packages/daemon/src/adapters/codex-runtime-adapter.ts:383–412` constructs and sends the native `codex` command to its bound tmux session. `/home/kkk/.cline/worktrees/43d5c/openrig-breakdown/packages/daemon/src/domain/rigspec-preflight.ts:142–144,293–310,395–405` admits Pi, terminal and stub as well as Claude/Codex, so the claimed harness pair is not an exhaustive runtime requirement.

## Notes

- **Scope:** local CLI auto-start and selected native starter/runtime, not every remote client or terminal-provider path. Authentication belongs to the selected native harness; the inspected launch path does not acquire model credentials. This is not a security audit proving absence of credential handling everywhere.
- **Contradictions:** package engines and system preflight disagree on accepted Node majors; source wins for what that check executes, package metadata for its declared support constraint. The discrepancy remains unresolved. The rig-spec documentation's runtime list at `/home/kkk/.cline/worktrees/43d5c/openrig-breakdown/docs/reference/rig-spec.md:270` is narrower than the current source set above.
- **Evidence still needed:** supported-version intent for the Node mismatch, clean-machine prerequisites for each selected provider/runtime, and native authentication-mode/version behavior. Missing binary, missing login, daemon health and seat readiness must not be collapsed into one check (see F7).
- **Runtime-unverified:** no install, doctor/preflight execution, credential inspection, login or native launch was performed.

