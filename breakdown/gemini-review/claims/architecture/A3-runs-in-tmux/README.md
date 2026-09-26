# A3: Agents run inside tmux (or cmux) sessions on the host, not in containers.

**Verdict:** part — tmux-backed execution is supported; cmux viewing and the categorical exclusion of containers need qualification.

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> The underlying agents run inside standard tmux terminal multiplexer sessions on the host operating system7.

> Unlike modern platforms that package agent execution inside self-contained runtimes or lightweight container sandboxes, OpenRig binds directly to host multiplexers like tmux and cmux7.

> This architectural choice creates significant friction during initial setup and routine operation13.

## What the infographic says

Verbatim from `sources/agent_fleet_architectures_infographic.html`.

> Host tmux Multiplexer / Session Sprawl

> Fragile OS Terminal Multiplexing (`tmux`)

> Instead of running containers (Docker) or pure Node scripts, OpenRig relies on hijacking host terminal sessions using tmux or cmux.

> tmux Meta-Harness

## Sources Gemini cited

- [7] [GitHub - mvschwarz/openrig: Multi-agent harness that runs Claude](https://github.com/mvschwarz/openrig)
- [13] [Getting started - OpenRig](https://www.openrig.dev/docs/getting-started)

## How to check

Find where the daemon spawns agent sessions. Is tmux required, optional, or one of several adapters? Note the repo also has a `docker/` folder.

## Evidence from this repo

Reviewed against source baseline `9db3ed6c406be5c3d9a84720383fcf6b543169e6`.

- **Source observation:** Claude and Codex commands execute through tmux adapters: `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/adapters/claude-code-adapter.ts:281–296`; `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/adapters/codex-runtime-adapter.ts:383–412`. Pi's runner spawns an external Pi process: `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/adapters/pi-runner.ts:568–580`.
- **Stated intent:** Terminal providers place/render already composed views, rather than defining agent execution: `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/terminal/terminal-provider.ts:1–29`. **Source observation:** The view composer builds SSH-to-tmux attachment commands: `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/terminal/view-composer.ts:150–155`. cmux/herdr surfaces should not be treated as interchangeable replacements for the tmux execution substrate.
- **Source observation:** Docker exists for services (`docker compose ... up -d`) and for a testbed that installs tmux and OpenRig inside a container: `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/adapters/compose-services-adapter.ts:64–77`; `/home/kkk/Apps/openrig-breakdown/docker/testbed/Dockerfile:19–24,42–64`.

## Notes

**Inference:** The traced ordinary launch path is not a per-agent container sandbox. That is narrower than “not in containers” under every deployment: the container testbed still uses tmux. “Instead of pure Node scripts” is also overbroad given the Node-based Pi runner.

**Runtime-unverified / limitations:** No host sessions, provider windows or containers were started. The claims of “significant friction,” “fragile,” “sprawl,” and “hijacking” are not established by an adapter's existence. They remain unresolved as measured behavior; evidence needed is a defined installation/operation study, failure observations and a comparison baseline. Actual isolation and portability require an isolated runtime inspection. No friction-page verdict or simplification recommendation is made here.

