# A2: OpenRig runs as a local daemon: a Hono HTTP server with SQLite state, plus a TUI.

**Verdict:** yes — the stated components exist and are wired together; the description is not an exhaustive interface or storage inventory.

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> OpenRig operates as a local background daemon powered by a Hono HTTP server, supported by an SQLite state database and a Terminal User Interface7.

> Primary User Interfaces: Command-line interface and terminal user interface (rig tui)7

> State and History Management: SQLite database tracking topology graphs, chatrooms, and task queues8

## What the infographic says

Verbatim from `sources/agent_fleet_architectures_infographic.html`.

> tmux / Hono / SQLite

> OpenRig Daemon (Hono/SQLite)

> Host CLI ➔ SQLite Daemon ➔ OS tmux pane ➔ Claude Code CLI process.

## Sources Gemini cited

- [7] [GitHub - mvschwarz/openrig: Multi-agent harness that runs Claude](https://github.com/mvschwarz/openrig)
- [8] [OpenRig - Esoteric Labs — AI Studio](https://esoteric.run/work/openrig)

## How to check

Check `packages/daemon` dependencies and entry point, and whether `packages/tui` and `packages/ui` both exist as user interfaces (Gemini does not mention the web UI).

## Evidence from this repo

Reviewed against source baseline `9db3ed6c406be5c3d9a84720383fcf6b543169e6`.

- **Source observation:** The server constructs Hono, startup opens/migrates SQLite, and the entry point serves the application's fetch handler: `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/server.ts:476–480`; `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/startup.ts:240–249`; `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/index.ts:297–326`. SQLite uses `better-sqlite3`, WAL, and foreign keys: `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/db/connection.ts:1–14`.
- **Source observation:** Topology tables include rigs, nodes and edges; queues and chat have persisted records: `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/db/migrations/001_core_schema.ts:7–39`; `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/db/migrations/024_queue_items.ts:22–48`; `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/chat-repository.ts:34–51`.
- **Source observation:** The CLI package exposes `rig` and `openrig-tui`, and a `tui` command exists: `/home/kkk/Apps/openrig-breakdown/packages/cli/package.json:28–38`; `/home/kkk/Apps/openrig-breakdown/packages/cli/src/commands/tui.ts:39–47`.
- **Stated intent:** The web UI is experimental, maintenance-mode and best-effort; CLI is primary: `/home/kkk/Apps/openrig-breakdown/docs/reference/developing.md:35–39`. Its omission from Gemini does not negate the claimed TUI.

## Notes

**Inference:** “SQLite daemon” is shorthand: Hono handles requests and repositories persist state; SQLite is not itself the terminal dispatcher. The tmux part of the infographic is qualified in A3/A6.

**Runtime-unverified / limitations:** No server, TUI or browser was started; packaging, listener reachability and UI usability are not verified. “Local” does not imply loopback-only: the entry point uses a multi-host binding plan, explicitly mentioning loopback plus active Tailscale (`/home/kkk/Apps/openrig-breakdown/packages/daemon/src/index.ts:290–318`). Successful startup, database migrations and representative UI/API operations in an isolated installation would be needed for runtime confirmation. This result does not assert SQLite is the sole persistence medium.

