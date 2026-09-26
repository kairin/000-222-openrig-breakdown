# F7: Starter rigs such as `first-project` fail at boot when there is no Codex login.

**Verdict:** part — the current starter explicitly selects Codex and documents a required login; an immediate, universal whole-rig boot failure without login is not established.

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> If the host environment lacks an active Codex login, starter rigs like first-project fail immediately during boot13.

## What the infographic says

Not mentioned.

## Sources Gemini cited

- [13] [Getting started - OpenRig](https://www.openrig.dev/docs/getting-started)

## How to check

Find the `first-project` spec. Does it contain a Codex seat, and does boot fail or degrade when Codex is missing?

## Evidence from this repo

Reviewed at source commit `9db3ed6c406be5c3d9a84720383fcf6b543169e6` (2026-09-27).

- **Observed in source:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/specs/rigs/launch/first-project/rig.yaml:10–25` declares two Codex members, both using AgentSpec `profile: default`, without `codex_config_profile`.
- **Stated intent:** `/home/kkk/Apps/openrig-breakdown/docs/reference/getting-started.md:54–80` calls missing Codex login a launch blocker but separately instructs the operator to resolve seat authentication/trust/permission prompts after launch. Daemon health does not certify native readiness.
- **Observed behavior trace:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/bootstrap-orchestrator.ts:635–637` invokes pod instantiation. `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/rigspec-instantiator.ts:1241–1245` invokes modern rig preflight; `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/rigspec-preflight.ts:379–405,451–471` checks supported runtime/cwd and optional native profile loading, skipping the Codex profile probe when no native profile is declared. This inspected path does not enforce `codex login status` before creating the rig. The legacy version-command check at lines 93–105 of that file is not the modern starter's login gate.
- **Observed behavior trace:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/adapters/codex-runtime-adapter.ts:383–412` sends the native launch command; successful command dispatch alone can return success without a captured thread ID. `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/startup-orchestrator.ts:226–237,311–324` then distinguishes harness launch from readiness, returning attention-required, timeout or failure as appropriate.
- **Observed failure aggregation:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/rigspec-instantiator.ts:1509–1549` preserves attention-required rigs/sessions, while all terminal failures trigger cleanup. `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/adapters/codex-resume.ts:96–134` explicitly treats a detected OAuth refusal on resume as attention-required; that resume branch is not proof of fresh no-login behavior.

## Notes

- **Scope:** shipped two-seat starter at this commit; other starters/runtime choices differ. An absent executable, absent login, expired credential, trust prompt and permission prompt are distinct cases.
- **Contradiction:** the documented login prerequisite supports the dependency, but Gemini's “fail immediately during boot” is stronger than the inspected staged launch/readiness and recoverable-attention behavior. Whether a particular fresh no-login screen is recognized, times out or exits remains unresolved.
- **Inference / evidence still needed:** pin Codex version and authentication mode, then observe fresh no-login, missing-binary and expired-login launches separately, recording per-seat state, pane evidence, aggregate result and elapsed time. No blanket immediate-failure conclusion follows from a prerequisite sentence.
- **Runtime-unverified:** none of those scenarios was run; no credentials were read or changed, and no starter was launched.

