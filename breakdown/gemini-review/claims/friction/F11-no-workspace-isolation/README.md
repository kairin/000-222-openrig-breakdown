# F11: Agents share one filesystem; there is no per-agent git worktree isolation.

**Verdict:** part — the starter uses shared host working directories and the traced launch path does not create per-seat Git worktrees, but authored per-member directories permit user-managed separation. Universal absence of worktree use is not established.

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> Workspace Isolation Mechanism: Multiplexed terminal panes sharing local filesystems or repository paths7

## What the infographic says

Verbatim from `sources/agent_fleet_architectures_infographic.html`.

> Multiplexed Panes (Shared)

> tmux windows on host OS (shared disk)

> Multiplexed Terminal Panes

## Sources Gemini cited

- [7] [GitHub - mvschwarz/openrig: Multi-agent harness that runs Claude](https://github.com/mvschwarz/openrig)

## How to check

Read `docs/reference/worktree-builds.md`, which suggests worktree support exists. Is it automatic per seat, or opt-in?

## Evidence from this repo

Reviewed at source commit `9db3ed6c406be5c3d9a84720383fcf6b543169e6` (2026-09-27).

- **Observed defaults:** `/home/kkk/.cline/worktrees/43d5c/openrig-breakdown/packages/daemon/specs/rigs/launch/first-project/rig.yaml:10–25` assigns both members `cwd: "."`. `/home/kkk/.cline/worktrees/43d5c/openrig-breakdown/packages/cli/src/commands/up.ts:79–83` describes `--cwd` as an override for all members, not per-seat isolation.
- **Observed behavior trace:** `/home/kkk/.cline/worktrees/43d5c/openrig-breakdown/packages/daemon/src/domain/profile-resolver.ts:144–148` uses the global override when present; otherwise absolute member cwd is retained and relative member cwd resolves against the spec root. `/home/kkk/.cline/worktrees/43d5c/openrig-breakdown/packages/daemon/src/domain/rigspec-instantiator.ts:1924–1929` calls the node launcher; `/home/kkk/.cline/worktrees/43d5c/openrig-breakdown/packages/daemon/src/domain/node-launcher.ts:130–142` passes the node cwd to tmux. `/home/kkk/.cline/worktrees/43d5c/openrig-breakdown/packages/daemon/src/adapters/tmux.ts:331–336` emits `tmux new-session ... -c <cwd>`, not Git worktree creation.
- **Observed native boundary:** `/home/kkk/.cline/worktrees/43d5c/openrig-breakdown/packages/daemon/src/adapters/codex-runtime-adapter.ts:383–392` passes the binding cwd via Codex `-C` and selects a native sandbox posture. Native permission/sandbox controls are not automatic separate Git working trees.
- **Observed persistence link:** `/home/kkk/.cline/worktrees/43d5c/openrig-breakdown/packages/daemon/src/domain/rigspec-instantiator.ts:1406–1429` stores `configResult.config.cwd` on the created node, connecting profile resolution to the launcher's node-cwd fallback above.
- **Observed workspace metadata:** `/home/kkk/.cline/worktrees/43d5c/openrig-breakdown/packages/daemon/src/domain/workspace/workspace-resolver.ts:26–50,57–94` resolves declared repositories and containing paths; it does not allocate an isolated checkout.
- **Stated intent / scope correction:** `/home/kkk/.cline/worktrees/43d5c/openrig-breakdown/docs/reference/rig-spec.md:264–275` documents per-member cwd. `/home/kkk/.cline/worktrees/43d5c/openrig-breakdown/docs/reference/worktree-builds.md:1–25` concerns dependency resolution when developing OpenRig in worktrees; it is not proof of automatic per-agent worktree provisioning.

## Notes

- **Scope:** ordinary first-project launch and inspected cwd/terminal plumbing, not every authored startup action, custom service or external agent workflow. A shared host filesystem is compatible with separate Git worktrees and does not imply every agent must share a working directory.
- **Inference:** existing user-created worktrees can be selected through distinct member cwd values without the all-member override. This follows from generic path handling; a multi-worktree launch was not demonstrated. Worktrees themselves are not a filesystem security boundary.
- **Contradiction:** the broad “no per-agent ... isolation” wording conflates absence of automatic allocation in this path with inability to use separate existing paths. The suggested build document cannot resolve that distinction. No automatic Git allocation was found in the traced path; that bounded observation is not a repository-wide impossibility proof.
- **Evidence still needed:** a controlled launch with distinct existing worktrees, inspection of actual cwd/branch and cross-seat write permissions, plus auditing any selected startup scripts/native sandbox configuration before claiming end-to-end isolation. No isolation guarantee is asserted.
- **Runtime-unverified:** no worktree creation, launch, sandbox probe or concurrent-file-edit test was performed.

