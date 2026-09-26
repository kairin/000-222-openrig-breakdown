# S1: OpenRig works with the CLI agents people already use instead of replacing them.

**Verdict:** yes — integration with existing external agents is implemented; popularity, benefit and historical origin are not established by that mechanism.

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> OpenRig is engineered not as an autonomous coding agent, but as a local meta-harness—a control plane that wraps, monitors, and connects pre-existing, third-party terminal coding agents such as Anthropic’s Claude Code and OpenAI’s Codex CLI7.

> Originating from the private Agent Focus framework and licensed under Apache 2.0, OpenRig addresses the sprawl that emerges when developers run multiple uncoordinated terminal sessions simultaneously7.

## What the infographic says

Verbatim from `sources/agent_fleet_architectures_infographic.html`.

> You have a deliberate need to coordinate pre-existing Claude Code / Codex terminal processes through tmux sessions with persistent telecom-style seats. Be prepared for high setup friction.

## Sources Gemini cited

- [7] [GitHub - mvschwarz/openrig: Multi-agent harness that runs Claude](https://github.com/mvschwarz/openrig)

## How to check

Confirm how many agent runtimes are supported and how much code each adapter takes. This is likely the core worth keeping.

## Evidence from this repo

Reviewed against source baseline `9db3ed6c406be5c3d9a84720383fcf6b543169e6`.

- **Source observation:** Claude and Codex launch paths invoke those external binaries, while Pi runs as an external RPC child: `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/adapters/claude-code-adapter.ts:281–296`; `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/adapters/codex-runtime-adapter.ts:383–412`; `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/adapters/pi-runner.ts:568–580`. This is the same mechanism examined in A1, not independent corroboration.
- **Source observation:** Five adapters are registered: Claude Code, Codex, Pi, stub and terminal (`/home/kkk/Apps/openrig-breakdown/packages/daemon/src/startup.ts:844–845`). This does not mean five third-party coding agents: stub is a pane-hosted Node runner and terminal is an infrastructure shell/no-op adapter (`/home/kkk/Apps/openrig-breakdown/packages/daemon/src/adapters/stub-runtime-adapter.ts:1–14`; `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/adapters/terminal-adapter.ts:13–48`). Registration alone does not certify production readiness of any runtime.
- **Source observation:** Direct adapter file sizes, counted with `wc -l`, are Claude 991, Codex 1,490, Pi 427, stub 322, terminal 50 lines. Exact file extents: `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/adapters/claude-code-adapter.ts:1–991`; `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/adapters/codex-runtime-adapter.ts:1–1490`; `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/adapters/pi-runtime-adapter.ts:1–427`; `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/adapters/stub-runtime-adapter.ts:1–322`; `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/adapters/terminal-adapter.ts:1–50`. These include comments/blanks and exclude runners, shared transport, resume adapters and tests; they are not total integration-cost estimates.
- **Stated intent / documentary evidence:** The repository license text is Apache 2.0: `/home/kkk/Apps/openrig-breakdown/LICENSE:1–5`. That verifies the quoted licensing label, not the claimed private “Agent Focus” origin.

## Notes

**Inference:** OpenRig wraps these existing harnesses rather than replacing their inference engines. Compatibility with installed versions/accounts and preservation of a user's habitual workflow do not follow from command construction alone.

**Runtime-unverified / limitations:** No authenticated harness was launched. Runtime support needs versioned launch/readiness/message checks; “people already use,” reduced session sprawl, and “high setup friction” need user/operational evidence rather than source counts. The Agent Focus provenance remains explicitly unresolved: a search for `Agent Focus`/`agent-focus` in the root README, archive and docs found no confirming text; an attributable historical source or repository history tracing that origin is needed. No retention recommendation or strength/weakness classification is made. Seat persistence and linked-group conclusions are not reviewed here.

