# A7: A discovery engine fingerprints tmux processes and can adopt unmanaged Claude Code or Codex sessions.

**Verdict:** part — discovery and adoption exist; uninterrupted execution is not established and adoption can send input.

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> The system also includes an operating-system-level discovery engine that fingerprints active tmux processes, enabling the control plane to adopt unmanaged Claude Code or Codex sessions into its managed topology without interrupting ongoing execution7.

## What the infographic says

Not mentioned.

## Sources Gemini cited

- [7] [GitHub - mvschwarz/openrig: Multi-agent harness that runs Claude](https://github.com/mvschwarz/openrig)

## How to check

Search the daemon for discovery/adopt logic. Decide whether this feature is needed at all in a simplified tool.

## Evidence from this repo

Reviewed against source baseline `9db3ed6c406be5c3d9a84720383fcf6b543169e6`.

- **Source observation:** Scanner enumerates tmux sessions/windows/panes and gathers PID, cwd and foreground command; failed metadata reads can leave nulls: `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/tmux-discovery-scanner.ts:31–73`. Coordinator filters managed/claimed sessions, fingerprints panes and persists evidence: `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/discovery-coordinator.ts:33–93`.
- **Source observation:** Fingerprinting uses cmux signals, command patterns for Claude/Codex, pane content and cwd configuration, returning confidence rather than proof: `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/session-fingerprinter.ts:24–35,70–108,112–154`.
- **Source observation:** Binding requires an active discovery record, existing target rig/node, no existing tmux binding, and compatible known runtime. It transactionally records the binding/session/claim: `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/claim-service.ts:278–337`. After commit it sets metadata, may provision a collector, starts transcript capture, and sends an identity hint: same file, lines 339–372. That hint is text plus `C-m`: same file, lines 183–200.
- **Stated intent:** The class comment describes atomic adoption without package installation/guidance merging/hooks: `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/claim-service.ts:99–103`. The executable post-adoption work above is a necessary qualification to that comment.

## Notes

**Inference:** The inspected bind path attaches existing processes rather than launching replacements, but “no restart” is narrower than “no interruption.” Discovery is tmux-scoped and heuristic, not guaranteed recognition of every unmanaged OS process. Input injection and context provisioning prevent treating adoption as observation-only.

**Runtime-unverified / limitations:** No live session was adopted. Non-interruption remains unresolved: evidence needed is a before/after trace of an actively working Claude/Codex session, including PID, ongoing tool execution, terminal input and provider conversation state. False-positive/negative fingerprint rates likewise require a representative session corpus. Whether the feature is worth keeping is deliberately not assessed despite the original “How to check” suggestion.

