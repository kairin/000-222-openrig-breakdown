# Capability inventory — source coverage and open evidence

## Evidence contract

Inspected source commit: **`ea7c268f576ada8434d3dae3e6ac972264910d4c`**. All source citations in this document refer to that Git tree, not to installed code or a running daemon. The later commit containing this document is a documentation commit. Inspection date: 2026-09-27.

The scope is capability coverage, not a retained-scope decision. No owner questions are answered, no retention/removal recommendation is made, and no Gate A–D status is assigned. No product tests, daemon, agent, installer, scenario, eval, Slack connection or recovery workflow was run. Reading a test or a source comment is not a passing test result.

Evidence labels:

- **O — observed-in-source:** executable wiring or behavior in the cited source; not demonstrated execution.
- **S — stated-intent:** documentation, comments, help or declarations that state purpose; not enough alone to establish effects.
- **I — inferred:** a connection supported indirectly; the inference is not a guaranteed behavior.
- **U — unresolved:** missing evidence, with the needed evidence stated.

Dispositions describe research, not product retention: **investigated** means the stated source chain was examined; **deferred** means the named deeper trace or runtime check remains open for the stated reason. No future work is called scheduled without an assignment. A family can have an investigated representative chain and deferred member-level evidence. The declaration ledger below makes that distinction explicit.

Reuse, not a second count audit: the accepted dependency/execution inventory and its reproducible package/Node/file counts remain in `breakdown/10-rust-and-node-removal-plan.md:66-117`; its original source pin is in `breakdown/10-rust-and-node-removal-plan.md:5-9`. The package/deployment map remains in `breakdown/08-current-state-evidence.md:64-130`. This document does not repeat those totals or promote them to measurements of capability, complexity or owner value. Task ownership and the coverage contract are in `breakdown/11-coordination-outcomes.md:119-135`.

Workflow association is narrow. The guide states setup/launch/inspect at `docs/reference/getting-started.md:54-80`, assign/follow work at `docs/reference/getting-started.md:90-126`, and stop/resume at `docs/reference/getting-started.md:181-190`. These are **S**, not proof of actual owner use. The existing workflow research at `breakdown/research-workflow-traces.md:3-7` has its own older pin and limits. Rows below associate a mechanism with a journey only where its source chain supports it. **Owner value remains U for every row**: source does not establish which optional surface the owner needs, its frequency of use, or its net benefit.

## Capability chains

Each C-row is an explicit disposition. Dependencies listed here are path-specific, not a duplicate package inventory. Registered members and adjacent files receive additional dispositions in the declaration ledger. References between C-rows reuse a shared chain, not an assertion that every consumer implements every operation.

### Core operation and durable work

| ID / capability / disposition | Entrypoint → implementation / consumer, with evidence | Effects, dependencies, supported journey and remaining evidence |
|---|---|---|
| C01 — CLI, setup and installation — investigated; host execution deferred | **O:** published bins and postinstall are declared at `packages/cli/package.json:28-45`; the wrapper checks/re-executes the runtime at `packages/cli/src/bin-wrapper.ts:25-79`; commands are registered at `packages/cli/src/index.ts:171-261`. Setup's prerequisite/install/config branches are at `packages/cli/src/commands/setup.ts:357-380` and `packages/cli/src/commands/setup.ts:504-624`. | **O:** shell/tool checks and managed tmux configuration edits; runtime/native-module prerequisites. **S:** setup journey above. **U:** clean-host installation, external authentication, interrupted install and reversibility require isolated host evidence. Packaging/update are C27, not inferred from successful argument parsing. |
| C02 — daemon, local API and persistence — investigated; live startup deferred | **O:** CLI spawns daemon with its runtime at `packages/cli/src/daemon-lifecycle.ts:651-660`; daemon resolves startup at `packages/daemon/src/index.ts:236-298`; DB initialization is at `packages/daemon/src/startup.ts:240-249`. SQLite settings are at `packages/daemon/src/db/connection.ts:1-13`; migration ordering/transactions at `packages/daemon/src/db/migrate.ts:13-42`. | **O:** process/listener and persistent schema effects; SQLite/native module and filesystem dependencies. API registration at `packages/daemon/src/server.ts:684-798` feeds CLI/MCP/UI/TUI consumers. Setup/inspection chain; **U:** occupied listener, permission failure, interrupted migrations and shutdown durability need runtime evidence. |
| C03 — bootstrap, launch, specs/packages/bundles — investigated representative chain; format branches deferred | **O:** `up` exposes source/plan at `packages/cli/src/commands/up.ts:68-80`; API invokes bootstrap at `packages/daemon/src/routes/up.ts:299-344`; bootstrap applies external installs/packages and instantiates topology at `packages/daemon/src/domain/bootstrap-orchestrator.ts:477-562`, with pod-aware launch at `packages/daemon/src/domain/bootstrap-orchestrator.ts:635-637`. Spec review returns review/error results at `packages/daemon/src/routes/spec-review.ts:7-41`; bundle handlers depend on archive, integrity and resource routers at `packages/daemon/src/routes/bundles.ts:8-34`. **O:** CLI `rig doctor` imports `@openrig/daemon/spec-conformance` and compares authored specifications against live topology (`packages/cli/src/commands/doctor.ts:1-17`). | **O:** install journal, resource installation, topology and agent process paths; YAML/JSON schemas, archives, filesystem, external tools and runtime adapters. Launch journey supported. **U:** complete legacy/pod/bundle branch equivalence, conflict cleanup and trust/approval behavior require branch-level traces and isolated exercises. Import/export/discovery are explicitly included, not assumed synonymous with `up`. |
| C04 — topology mutation, binding and archive — investigated representative chain; destructive variants deferred | **O:** CLI registration at `packages/cli/src/index.ts:186-197` and `packages/cli/src/index.ts:238-249`; rig add-member route validates input and calls `convergeOp` at `packages/daemon/src/routes/rigs.ts:785-829`; archive/unarchive writes and event tokens at `packages/daemon/src/routes/rigs.ts:478-520`. | **O:** topology operations, reversible archive state and events; DB, instantiator and runtime/terminal dependencies. **S:** source says supplied edges persist but have no edge-runtime behavior at `packages/daemon/src/routes/rigs.ts:791-795`; do not infer execution from graph edges. **U:** expand/grow/remove/shrink/destroy/adopt/bind cleanup and dirty-worktree safety need individual effect/ownership traces; no blanket preservation claim. |
| C05 — stop, snapshot and restore — investigated; recovery demonstration deferred | **O:** down route invokes teardown at `packages/daemon/src/routes/down.ts:22-55`; teardown attempts snapshot, kills sessions and cleans state at `packages/daemon/src/domain/rig-teardown.ts:103-176`. Restore refuses live/unknown sessions and can await a decision at `packages/daemon/src/domain/restore-orchestrator.ts:782-812` and `packages/daemon/src/domain/restore-orchestrator.ts:976-1006`. | **O:** process termination, session/binding cleanup, snapshot metadata and restore outcomes. Snapshot failure does not stop teardown. Stop/resume journey supported; tmux, DB and native resume metadata are dependencies. **U:** work preservation, full conversation continuity, crash-cart/fleet restore completeness and interrupted cleanup require isolated dirty-worktree/recovery evidence; snapshots are not established as full backups. |
| C06 — status, ps, transcripts and diagnostic projections — investigated representative reads; accuracy deferred | **O:** rig summary folds DB inventory at `packages/daemon/src/routes/rigs.ts:215-238`; status composes lifecycle/restore/kernel inputs at `packages/daemon/src/routes/rigs.ts:247-280`. Browser reads this summary at `packages/ui/src/hooks/useRigSummary.ts:19-37`; TUI wires hydration and recovery dependencies at `packages/tui/src/main.ts:21-33`. | **O:** visible read projections; persisted identity is not proof of live process health. Inspection journey supported. **U:** every diagnostic's stale/missing-source behavior, transcript retention/rotation and cross-client consistency need targeted traces and runtime comparisons. |
| C07 — send, capture, broadcast, walk and ask — investigated transport chain; orchestration variants deferred | **O:** CLI registration at `packages/cli/src/index.ts:204-217`; transport routes resolve/send/capture/broadcast at `packages/daemon/src/routes/transport.ts:93-105`, `packages/daemon/src/routes/transport.ts:170-205` and `packages/daemon/src/routes/transport.ts:217-275`; ask delegates to AskService at `packages/daemon/src/routes/ask.ts:6-21`. CLI send distinguishes verification and prompt override at `packages/cli/src/commands/send.ts:230-283`. | **O:** pane input/output and transport result; terminal/session identity dependencies. Assign/inspect journey supported for transport, not proof of agent acceptance or completion. **U:** walk sequencing, remaining ask search branches and every runtime's delivery behavior remain deeper traces; unavailable/prompt-blocked/retry behavior needs execution evidence. |
| C08 — durable queue, claim, handoff and human work — investigated; delivery outcome deferred | **O:** queue create handler passes identity and content to repository at `packages/daemon/src/routes/queue.ts:389-480`; repository persists creation at `packages/daemon/src/domain/queue-repository.ts:1310-1358` and separates nudge effects at `packages/daemon/src/domain/queue-repository.ts:1187-1197`; terminal transitions/wake intent are at `packages/daemon/src/domain/queue-repository.ts:1507-1669`. | **O:** SQLite work rows, transition evidence and post-commit transport attempts; assign/follow journey supported. Durable work is distinct from pane receipt. Slack continuation is C16. **U:** real retries, disconnected clients, recipient behavior and exactly-once end-to-end outcomes need experiments, not just transaction inspection. |
| C09 — stream intake, chat and classification — investigated stream/classifier chain; chat details deferred | **O:** stream emit route derives identity and calls store at `packages/daemon/src/routes/stream.ts:55-87`; store inserts immutable intake at `packages/daemon/src/domain/stream-store.ts:83-119`; project route classifies at `packages/daemon/src/routes/projects.ts:109-154`, and classifier checks lease/existence then inserts at `packages/daemon/src/domain/project-classifier.ts:102-145`. Chat is mounted separately at `packages/daemon/src/server.ts:739-742`. | **O:** intake/classification rows and read results; DB, identity, classifier lease and events. Source supports intake→classification, not a required owner workflow. **U:** remaining chat operations and classification usefulness require deeper consumer traces; owner value unresolved. |
| C10 — events and live refresh — investigated; reconnect demonstration deferred | **O:** EventBus inserts before notification at `packages/daemon/src/domain/event-bus.ts:56-91`; SSE subscribes then replays/buffers at `packages/daemon/src/routes/events.ts:12-73`; browser parses and notifies listeners at `packages/ui/src/lib/topology-events.ts:61-96`; TUI feature-detects and reconnects established streams at `packages/tui/src/live-events.ts:31-85`. | **O:** durable event rows, network streams and client refresh notifications. Inspection is supported; browser and TUI reconnect mechanisms are not identical. **U:** ordering under connection loss, poison events and UI refresh accuracy require runtime evidence. |

### Presentations, connectivity and runtimes

| ID / capability / disposition | Entrypoint → implementation / consumer, with evidence | Effects, dependencies, supported journey and remaining evidence |
|---|---|---|
| C11 — MCP — investigated stdio/server→daemon HTTP chain; per-tool effects remain mapped to owning capabilities | **O:** registration at `packages/cli/src/index.ts:200-200`; serve resolves daemon and connects stdio at `packages/cli/src/commands/mcp.ts:14-50`; server setup and representative `rig_up`/`rig_down` handlers are at `packages/cli/src/mcp-server.ts:48-107`; each of the 18 handler→HTTP target mappings is ledgered below (do not imply lines 52-105 cover all handlers). MCP result conversion/errors are at `packages/cli/src/mcp-server.ts:21-44`. | **O:** agent-facing stdio protocol, daemon HTTP requests and tool text/JSON response; durable/process effect belongs to each owning C-row. `rig_capture` maps to `/api/transport/capture` (`packages/cli/src/mcp-server.ts:337-360`) and chat watch returns bounded history rather than a stream (`packages/cli/src/mcp-server.ts:399-429`). **U:** external client compatibility, cancellation and live parity need runtime evidence; owner value unresolved. |
| C12 — browser UI — investigated shell, representative reads, and explicit route dispositions; action parity remains partial | **O:** React entry mounts App at `packages/ui/src/main.tsx:9-22`; App renders router and notice at `packages/ui/src/App.tsx:18-57`; routes import topology, project, workflow, files, settings, specs and lab consumers at `packages/ui/src/routes.tsx:23-77`; summary fetch is C06. Daemon serves assets/SPA and injects terminal token at `packages/daemon/src/server.ts:806-835`. **O:** the inventory route ledger includes routes that render placeholders/redirects: `/discovery` is a placeholder (`packages/ui/src/routes.tsx:449-464`); `/context`, `/mission-control`, `/slices`, `/progress`, `/steering`, and generic spec route redirect to other routes (`packages/ui/src/routes.tsx:266-276,488-530`). These paths are not separate complete feature consumers. | **O:** browser rendering, localStorage notice/token, HTTP consumers, route redirects/placeholders; React/router/query, browser and built assets. **S:** experimental/maintenance-mode notice at `packages/ui/src/App.tsx:7-8`, not this research's retention recommendation. **U:** remaining action-by-action writes, browser auth posture, accessibility, and feature completeness require review. Route registration alone does not prove a feature. |
| C13 — TUI, bare CLI front door and control socket — investigated; interactive verification deferred | **O:** executable declaration at `packages/tui/package.json:8-14`; bare-rig routing is declared at `packages/cli/src/index.ts:277-285`; main selects demo/live data and client at `packages/tui/src/main.ts:45-74`; socket path/state and parser dependencies at `packages/tui/src/socket-server.ts:13-19` and `packages/tui/src/socket-server.ts:24-85`; live refresh is C10. | **O:** terminal presentation, per-instance view state, local socket and daemon/CLI reads. Demo data is explicitly separate from live hydration. Inspection/recovery wiring is visible at `packages/tui/src/main.ts:21-33`. **U:** command registry action parity, socket permission/lifetime behavior and keyboard/mouse/recovery UX need deeper checks; no equivalence with browser/MCP inferred. |
| C14 — terminal providers and browser terminal — investigated service seam; provider internals deferred | **O:** terminal route opens a view through TerminalService at `packages/daemon/src/routes/terminal.ts:52-83`; service resolves provider, composes and checks preview identity before `openView` at `packages/daemon/src/domain/terminal/terminal-service.ts:143-182`. WebSocket route is wired at `packages/daemon/src/server.ts:713-720`; auth/broker attach at `packages/daemon/src/routes/terminal-ws.ts:29-125`. | **O:** view-open process requests and terminal network subscriptions; herdr/cmux provider availability, tmux, bearer and browser WS dependencies. **U:** provider-specific launches, resize/input/backpressure and remote SSH behavior need implementation/runtime traces. Owner value unresolved. |
| C15 — multi-host registry, routing and fleet reads — investigated representative durable/network chains; broader remote mutation recovery deferred | **O:** `rig host add/select` writes registry/config state (`packages/cli/src/commands/host.ts:458-550`); `/api/hosts` probes registered hosts and implements target approval/local pairing (`packages/daemon/src/routes/hosts.ts:127-145,151-221,246-385`); approved pairing persists a mode-0600 token and registry entry. Queue forwarding resolves only HTTP hosts, forwards to origin and creates no local row (`packages/daemon/src/routes/queue.ts:175-229`); fleet review returns local-plus-registered aggregation (`packages/daemon/src/routes/review.ts:74-96`). CLI send supports distinct SSH subprocess versus HTTP-direct remote daemon paths (`packages/cli/src/commands/send.ts:235-236,274-282`). | **O:** selected-host/config/token/registry artifacts, target human-gate qitem, remote HTTP/SSH process activity and host/fleet projections. Dependencies: registry/config, SSH or HTTP, bearer file, remote compatibility, human gate, filesystem/network. **U:** broader remote mutation retry and identity isolation require specific traces; no owner journey inferred. Workflow step pins separately refuse non-local workflow step host (C21); that restriction is not contradicted by generic remote queue forwarding. |
| C16 — gateway and Slack — investigated inbound/outbound chains; external delivery deferred | **O:** startup builds in-daemon subsystem at `packages/daemon/src/startup.ts:2309-2324`; network services start separately at `packages/daemon/src/index.ts:330-330`; enable/readiness routes at `packages/daemon/src/routes/gateway.ts:37-102`; configuration yields an inert wire if neither leg is ready at `packages/daemon/src/domain/gateway/slack/slack-subsystem.ts:199-218`. Outbound queue poller dispatches and inbound socket wires router/state at `packages/daemon/src/domain/gateway/slack/slack-subsystem.ts:472-567`. | **O:** attempted/delivered/seen JSONL, dead letters/receipts, queue work and thread map; secrets, Slack HTTP/Socket Mode and filesystem/DB. Inbound creates correlated work then marks seen at `packages/daemon/src/domain/gateway/slack/inbound.ts:197-230`; human reply calls existing resolve/update at `packages/daemon/src/domain/gateway/slack/slack-subsystem.ts:75-112`. **U:** live permissions/rate limits, delivery ambiguity and attachment transfer need external exercises. Source supports human-work continuation, not proof the owner uses Slack. |
| C17 — Claude Code and Codex managed seats — investigated launch/projection wiring; native readiness deferred | **O:** adapters receive managed hooks and filesystem ports at `packages/daemon/src/startup.ts:651-656`; Claude launch path at `packages/daemon/src/adapters/claude-code-adapter.ts:214-296`; Codex launches native command at `packages/daemon/src/adapters/codex-runtime-adapter.ts:311-394`; Claude managed hook projection at `packages/daemon/src/adapters/claude-code-adapter.ts:709-819`. | **O:** external harness commands in tmux and managed files/hooks; installed vendor CLIs, auth, runtime configuration and relay assets. Launch/continuity chain supported. **U:** native login, version-specific hook/trust behavior, partial launch and preservation of user-authored configuration need isolated tests. |
| C18 — Pi runner and resume — investigated; native RPC behavior deferred | **O:** Pi paths/resume are wired at `packages/daemon/src/startup.ts:509-518`; runtime adapter at `packages/daemon/src/startup.ts:658-659`; command builder launches Node at `packages/daemon/src/adapters/pi-runner-protocol.ts:164-188`; runner spawns `pi`, writes RPC, mirrors output, posts activity and writes sidecar at `packages/daemon/src/adapters/pi-runner.ts:568-622`. | **O:** Pi child process, pane output, activity HTTP and runner-state file; Pi executable/RPC contract, tmux and endpoint token. Optional launch adapter, not established owner workflow. **U:** native resume/fork correctness, env isolation and child failure require Pi execution. Activity and sidecar writes are best-effort, not proof of daemon receipt. |
| C19 — stub runner and terminal runtime — investigated; fixture adequacy deferred | **O:** stub and terminal are registered at `packages/daemon/src/startup.ts:660-664` and `packages/daemon/src/startup.ts:844-844`; stub writes readiness atomically at `packages/daemon/src/adapters/stub-runner.ts:54-61`; script dispatch implements compaction, chunked output, death and restore at `packages/daemon/src/adapters/stub-runner.ts:127-183`. Terminal adapter is no-op/ready and rejects fork at `packages/daemon/src/adapters/terminal-adapter.ts:19-48`. | **O:** sidecars, pane text, activity, process death and real compaction/restore helper calls; not a model. `slow_output` loops chunks without a wall-clock delay. Test-system consumer is C26. **U:** equivalence to native agent failure behavior is not established; terminal startup actions live outside this adapter. |
| C20 — optional Docker Compose services — investigated; service readiness deferred | **O:** startup injects Compose adapter/orchestrator at `packages/daemon/src/startup.ts:520-528`; bootstrap creates prelaunch hook at `packages/daemon/src/domain/bootstrap-orchestrator.ts:635-637`; adapter invokes `docker compose up`, status and policy-dependent down at `packages/daemon/src/adapters/compose-services-adapter.ts:64-119`; teardown invokes service cleanup at `packages/daemon/src/domain/rig-teardown.ts:137-145`. | **O:** external containers and possible volume removal when `down_and_volumes` is selected; Docker, Compose spec and readiness dependency. Source supports service-before-agent composition, not owner necessity. **U:** all readiness probes and volume/data safety need explicit runtime evidence. |

### Coordination, content and engineering surfaces

| ID / capability / disposition | Entrypoint → implementation / consumer, with evidence | Effects, dependencies, supported journey and remaining evidence |
|---|---|---|
| C21 — workflows — instantiate/project and remote-boundary chains investigated; other transitions remain open | **O:** CLI workflow operations call `/api/workflow` and render CLI projections (`packages/cli/src/commands/workflow.ts:253-440`); route delegates instantiate/project at `packages/daemon/src/routes/workflow.ts:196-256`; runtime creates instance and entry queue item with durable state/event in its notification envelope at `packages/daemon/src/domain/workflow-runtime.ts:710-797`. Browser list/instance routes consume the surface at `packages/ui/src/routes.tsx:66-69,177-195`. Dependencies: workflow spec/cache, role/owner resolution, SQLite instance/frontier/queue, event bus, optional human gate. **O:** runtime refuses every non-local host-pinned workflow step at instantiation (`packages/daemon/src/domain/workflow-runtime.ts:443-457`). A separate remote queue HTTP forwarding path exists (`packages/daemon/src/routes/queue.ts:175-229`), but no workflow-runtime consumer adapts a pinned step to it; remote workflow execution is not supported by the cited chain. | **O:** durable instance/frontier/current-step/bound-rig state, queue item/event, CLI/UI projections; remote pin refusal. **U:** full compile/revise/abort/resume/deadline/exception transition coverage and owner workflow value. The remote queue mechanism is not evidence of remote workflow execution. |
| C22 — watchdog, health, attention and notifications — investigated registration/read paths; automated policy effects deferred | **O:** watchdog validates/registers and emits at `packages/daemon/src/routes/watchdog.ts:67-139`; health routes return projection findings at `packages/daemon/src/routes/health.ts:14-52`; attention reads human-addressed queue and proof state at `packages/daemon/src/routes/attention.ts:24-78`. Optional notification adapter setup is at `packages/daemon/src/startup.ts:1480-1496`. | **O:** job registration/event and user-facing findings/requests; DB, timers/policies, filesystem observations and optional delivery providers. Inspection supported, not guaranteed autonomous recovery. **U:** each policy's firing/retry/escalation, external webhook/ntfy delivery, attention truncation and health accuracy need fuller timer→action traces and live failure evidence. |
| C23 — workspace, project, scope, files, review and proof — investigated representative read/write chains; remaining mutations deferred | **O:** workspace validation delegates at `packages/daemon/src/routes/workspace.ts:37-73`; config scaffolds at `packages/daemon/src/routes/config.ts:56-64`; file route checks expected mtime/hash then invokes atomic writer at `packages/daemon/src/routes/files.ts:209-267`; writer rename/audit handling at `packages/daemon/src/domain/files/file-write-service.ts:150-206`. Review returns composed agents/rig/fleet at `packages/daemon/src/routes/review.ts:45-95`; proof judge resolves identity, records and notifies at `packages/daemon/src/routes/proof.ts:28-48`; CLI proof add writes an artifact at `packages/cli/src/commands/proof.ts:419-431`. | **O:** workspace files, proof artifacts/judgments, audit and read models; allowlisted roots, indexer, expected revisions, identity and DB/event dependencies. Inspection and authoring supported in source, not a decision about this owner's process. A write may succeed before audit append fails. **U:** every scope/create/move/approve/freeze action, symlink boundary and partial filesystem update needs detailed trace; this is not the separate state-invariant ledger. |
| C24 — plugins, skills, context and generated content — discovery, composition and delivery-input chains investigated; full projection ownership unresolved | **O:** plugin CLI consumes discovery at `packages/cli/src/commands/plugin.ts:147-169,198-205`; startup vendors/projects global router skill at `packages/daemon/src/startup.ts:712-758`; vendor copies/version-checks at `packages/daemon/src/domain/plugin-vendor-service.ts:130-186`. Skill loadout delegates to daemon export with explicit apply at `packages/cli/src/commands/skill.ts:48-97`. Context routes list/sync/compose at `packages/daemon/src/routes/context-packs.ts:59-117`; composition writes files/manifest at `packages/daemon/src/domain/context-packs/context-pack-library-service.ts:360-379`. The pieces route returns assembled text/bytes/missing members for delivery flags (`packages/daemon/src/routes/context-packs.ts:220-242`); CLI resolver rejects missing members and resolves text (`packages/cli/src/context-resolve.ts:1-61`); `rig send --context` forwards resolved content (`packages/cli/src/commands/send.ts:220-282`). | **O:** installed plugin/skill files and composed context manifests; disk libraries, runtime manifests, version/hash rules, configured roots and session transport. Generation: package builder invokes generator (`scripts/build-package.sh:76-89`), generator copies members/writes manifests (`scripts/generate-context-packs.mjs:395-409`), startup discovers binary-relative library (`packages/daemon/src/startup.ts:761-789`). Startup says pack expansion is unsupported there and separates delivery verbs from library discovery. Mirror source/target are `scripts/mirror-skills.mjs:21-28`. **U:** external harness receipt/reading/acting on delivered text, per-runtime skill projection cleanup/ownership, and conflict recovery remain unverified; availability or composition is not proof of ingestion. |
| C25 — policies, config, auth/provider, usage and continuity — investigated representative paths; cross-feature completeness deferred | **O:** permission policy apply records choice through shared setup step at `packages/cli/src/commands/policy.ts:298-314`; config reads/scaffolds through SettingsStore at `packages/daemon/src/routes/config.ts:35-84`; auth exposes profile save/switch/seat registry at `packages/cli/src/commands/auth.ts:54-155`; provider route invokes account switch at `packages/daemon/src/routes/provider.ts:90-127`; telemetry queries usage projection at `packages/daemon/src/routes/telemetry.ts:19-59`. Seat handover constructs DB/tmux/runtime/recap service at `packages/daemon/src/routes/seat.ts:40-80`; agent-image fork uses convergence at `packages/daemon/src/routes/agent-images.ts:184-289`. | **O:** configuration/profile operations, provider commands, usage read results and continuity service inputs are present. **S:** auth seat metadata is not proof of live account (`packages/cli/src/commands/auth.ts:110-110`). **U:** secret persistence, actual permission enforcement versus recorded policy intent, rig-mode policy precedence, handover/compaction/fork effects and stale usage need deeper implementation/consumer traces. No native session-equivalence claim. |
| C26 — test-system, scenario runners and evals — runner and fixture-tree reachability investigated; execution not claimed | **O:** standalone scenario entry scans `packages/daemon/test/fixtures/scenarios`, requires built CLI/daemon and calls the pipeline (`packages/daemon/scripts/run-scenarios.mjs:24-71`); pipeline validates/scaffolds/spawns (`packages/daemon/test/helpers/scenario-pipeline.ts:303-384`) and refuses container-mode per-seat stub scripts (`:340-351`). Eval entry directly loads `packages/test-system/evals/cases` (`packages/daemon/scripts/run-evals.mjs:34-67`), supports fake/live providers, optional grade writes and cleanup (`:69-130`). **O reachability limit:** search found no source consumer of `packages/test-system/scenarios`; the scenario runner targets the daemon fixture tree instead. | **O:** separate scenario/eval roots and process/state/result effects; built CLI/daemon, tsx, tmux, optional TUI/Docker/native-seat dependencies. README scenario-runner-awaiting status is stale relative to runner implementation (`packages/test-system/README.md:36-42` versus runner cited above); it does not prove execution or passage. **U:** dynamic/manual invocation of test-system scenarios, per-scenario compatibility, seeded-red evidence, container parity and live eval validity. No tests were run for this inventory. |
| C27 — build, tests, release, install/update and CI — investigated representative process chains; artifact verification deferred | **O:** root scripts at `package.json:13-28`; package builder compiles/stamps/copies at `scripts/build-package.sh:20-89`; postinstall ABI check at `packages/cli/scripts/check-abi.mjs:104-122`; fresh-install smoke invokes npm/Node at `scripts/smoke-fresh-install.sh:35-78`. CI invokes portability report into summary at `.github/workflows/portability-report.yml:17-33`. Repository gate runner imports lock/hermeticity/run helpers and defines verdict path at `scripts/gate-lane.mjs:7-23`. | **O:** build artifacts, install/native checks, reports and test-runner processes; toolchain and packaging dependencies are reused from accepted baseline, not recounted. **S:** operational upgrade instructions invoke inspection/backup/plugin helpers at `packages/daemon/specs/agents/shared/skills/core/openrig-upgrade/SKILL.md:75-125`. **U:** packaged contents, external CI action runtimes, installer/update/rollback correctness and full helper-call closure require separate execution/artifact audit. Repository test gate machinery is not research Gates A–D. |
| C28 — docs, demos, UI twin/labs, spikes and archives — investigated declarations; current operational relevance deferred | **S:** guide offers journeys cited above. **O:** demo shell starts daemon/rig and runs resume probes (`demo/run.sh:7-52`); twin script declares build/Chrome artifact machinery and imports process/filesystem helpers (`packages/ui/twin/capture/twin-capture.ts:1-24`); UI declares lab imports (`packages/ui/src/routes.tsx:70-77`). | **O:** demo has real process effects, not just inert prose. Twin actually builds, captures twice, compares and writes a diff at `packages/ui/twin/capture/twin-capture.ts:130-177`; it depends on npm, Chrome and Git. This is not proof a capture succeeded. **U:** twin implementation/output verification, spike relevance, archive consumers, examples and operational scripts require individual traces before calling them supported or inert. The file coverage ledger includes these roots rather than silently excluding them. Owner value unresolved. |

## Consequential distinctions and unresolved evidence

### Follow-up source traces after decomposition

These traces close specific previously deferred links, not all remaining member-level research. Source pin and evidence labels remain unchanged.

| Capability | Declared entrypoint → implementation → consumer/effect | Disposition and limits |
|---|---|---|
| C22 watchdog scheduled delivery | **O:** daemon supervision starts scheduler (`packages/daemon/src/index.ts:326-326`); startup injects jobs/engine (`packages/daemon/src/startup.ts:1974-1980`); scheduler schedules timeout, lists active jobs, checks cadence and evaluates (`packages/daemon/src/domain/watchdog-scheduler.ts:120-159`). Engine invokes policy (`packages/daemon/src/domain/watchdog-policy-engine.ts:333-333`), performs delivery and records history/condition receipt/event (`packages/daemon/src/domain/watchdog-policy-engine.ts:495-557`). Production delivery creates a continuity baton when present, sends through session transport and reports failure (`packages/daemon/src/startup.ts:1815-1839`). | Investigated timer→policy→transport/durable-receipt chain. **O:** completed continuity action is recorded separately from transport success; condition receipt is banked only on positive delivery. **U:** individual registered policies, firing conditions and live delivery remain distinct checks. No autonomous recovery guarantee. |
| C25 provider account switching | **O:** API switch calls the service (`packages/daemon/src/routes/provider.ts:103-127`), startup constructs ProviderServiceImpl (`packages/daemon/src/startup.ts:1047-1047`), implementation prechecks account/auth and assumes a live conversation (`packages/daemon/src/domain/provider/provider-service-impl.ts:91-102`). Switch returns `failed_safely`, either for unsafe precheck or `switch_execution_not_yet_wired` (`packages/daemon/src/domain/provider/provider-service-impl.ts:105-114`). | Investigated **declared but non-operational switching endpoint**. Its visible effect is a refusal result, not a changed account. This is a concrete implementation boundary, not merely missing runtime proof. **U:** separate CLI auth profile switching must still be traced; it must not be conflated with this API. Owner value unresolved. |
| C24/C25 authored seat recap | **O:** CLI recap-write loads shared export, validates topology destination, reads content and calls writer, then prints path/chain size (`packages/cli/src/commands/context.ts:728-757`). Export resolves to seat-recap store (`packages/daemon/src/seat-recap-store-surface.ts:5-13`). Writer checks Markdown addressability, renames existing recap to a collision-disambiguated predecessor and writes current content (`packages/daemon/src/domain/context-packs/seat-recap-store.ts:50-77`). | Investigated command→shared implementation→durable file/visible result. Depends on configured topology root and filesystem. **O:** rename of old recap precedes current-file write; no atomic multi-file transaction is demonstrated. **U:** interruption behavior and actual successor ingestion remain unverified; writing a recap does not prove continuity. |

### Remaining cross-cutting gaps

1. **O — gateway activation is not Slack readiness.** Subsystem `start()` catches wiring failure; `startServices()` is separate (`packages/daemon/src/domain/gateway/gateway-subsystem.ts:123-151`). Missing connector configuration can produce an active subsystem with an inert Slack wire (C16). **U:** live API scopes, Socket Mode, network failure/replay, attachment security and end-to-end duplicate suppression need external evidence.
2. **O — Slack work has a durable/process chain.** State-store construction and thread DB are at `packages/daemon/src/domain/gateway/slack/slack-subsystem.ts:249-254`; delivery marks attempted, posts, then marks delivered at `packages/daemon/src/domain/gateway/slack/slack-delivery.ts:225-283`; inbound queue/seen ordering is C16. Inbound file port writes local media at `packages/daemon/src/domain/gateway/slack/slack-subsystem.ts:140-188`, wired at `packages/daemon/src/domain/gateway/slack/slack-subsystem.ts:519-529`. **I:** these mechanisms address replay/continuation hazards; they do not prove exactly-once external effects.
3. **O/S conflict — test-system narrative lags implementation.** The README says scenarios await a runner (`packages/test-system/README.md:36-42`), while a runner and real pipeline exist (C26). Stub header also says seeded behaviors are later (`packages/daemon/src/adapters/stub-runner.ts:10-12`), but executable dispatch handles them (C19). **U:** full authored-scenario compatibility and seeded evidence are still needed. Neither stale prose nor present code proves the suite passes.
4. **O — remote capability is not uniform.** Queue forwarding exists (C15); workflow instantiate rejects remote step pins (C21). **U:** determine all reachable remote workflow paths and reconcile error wording with routing implementation before claiming remote workflow support.
5. **Generated context, installed skills and actual agent delivery differ.** C24 now traces generation, discovery/composition, pieces→resolver→send delivery input; C17 traces harness projection. **U:** trace each selected profile/skill through prompt/hook ingestion and external harness observation. Availability or transport of text is not proof an agent read it.
6. **Optional UI routes and exports are not uniformly complete.** C12 classifies identified placeholder/redirect routes; C13 groups source-visible TUI action classes; the export ledger names direct CLI importers and unresolved no-consumer exports. **U:** remaining action-by-action reducer/effect and error-path checks, especially UI labs and the remaining unresolved `attention`, `crash-cart`, and `gateway-protocol` exports.
7. **U — owner value for optional features.** No source observation can settle which consumer, provider, coordination process or external integration the owner wants. Required evidence is owner usage/requirements and reviewer-accepted workflow relevance. This document records neither answers nor recommendations.
8. **U — live behavior for all rows.** Source traces require separate isolated normal/failure exercises for installation, agents, queue, shutdown, state preservation and recovery. This inventory adds no runtime evidence and makes no test-pass claim.

### Acceptance crosswalk (source-only)

The task acceptance contract has six criteria. This crosswalk separates documentation evidence from execution/owner evidence; it does not change any Gate A–D status.

| Criterion | Current evidence and disposition |
|---|---|
| 1. Every major and optional surface has an explicit row/disposition. | **Partially evidenced:** C01–C28, declaration ledgers, and residual coverage matrix disposition the major/optional families, including identified source-reachable versus bounded no-caller classes; not every member chain has yet passed semantic review. |
| 2. Entrypoint-to-consumer/effect/dependency chains are evidenced. | **Partially closed:** UI route classes, TUI action classes, retired gateway-process reachability, context resolver→delivery-input/refusal, test-system scenario/eval tree split, export importer reconciliation, MCP citation precision, and remote queue/workflow refusal distinction have pinned citations. The `gateway-protocol` export has no production importer in the searched pin; dynamic/external consumption is bounded unresolved reachability. The capability-family matrix below explicitly bounds remaining available-source chains by family; until those are individually closed, criterion 2 does not pass. |
| 3. Citations resolve at the pinned source. | **Structural PASS, semantic acceptance pending:** validator resolves ranges against `ea7c268f576ada8434d3dae3e6ac972264910d4`. The exact reviewer candidate returned 497 ranges/129 source files; this updated working candidate now has 511 ranges/132 files. Independent review must rerun against the published SHA; member-level semantic gaps are still explicitly open. |
| 4. No unapproved recommendation, owner answer, test-pass claim, or gate-status change. | **Evidenced by document scope:** the evidence contract disclaims these decisions and claims; optional-feature owner value remains unresolved. No product execution is represented as passed. |
| 5. Only the owned inventory file changes. | **PASS for PR #20 scope so far:** GitHub reports only `breakdown/research-capability-inventory.md` changed; recheck after publishing the next correction. |
| 6. Citation/file-coverage validation and `git diff --check` pass. | **Structural PASS on current working candidate:** local citation bounds, declaration ledgers, file-pattern matching and diff checks pass at 511 / 132; re-run against publication commit. File patterns do not prove semantic coverage. |

Accordingly, the inventory is not yet an acceptance-pass or a basis for marking the CAP-4 work Done. Runtime-only gaps (for example live Slack credentials, native harness behavior, or owner value) remain legitimate unresolved evidence; the source-chain omissions explicitly named above do not.

### Residual semantic source coverage matrix

These entries turn the remaining family-level flags into bounded work, not an assertion of completion. A generic tracked-file glob is not a source-chain citation. For each residual family the declaration and searched consumers below identify the last link seen and the missing link; use additional exact source citations before marking it investigated. No owner-value or runtime uncertainty excuses these code-inspection gaps.

| Family / bounded coverage class | Declaration or entrypoint and searched consumer evidence | Presently established; exact source work still required |
|---|---|---|
| C12 browser operational routes vs redirects/placeholders/labs | Route declarations in `packages/ui/src/routes.tsx:98-535`; concrete redirect/placeholder outcomes at `:266-276,449-464,488-530`; router consumers imported at `:23-77`. | Redirects/placeholders now explicitly classified. Operational page components and their hooks/API calls still need a member-level list of mutation/read effects, failure outcomes and matching daemon consumer per route. Labs at `:312-351` must be separated from product routes. |
| C13 TUI local view actions vs external effects | Literal registry at `packages/tui/src/commands/registry.ts:67-276`; socket parser/action boundary at `packages/tui/src/socket-server.ts:1-123`; main `onNative`, `onWork`, crash-cart CLI call and local-read callback at `packages/tui/src/main.ts:184-230`. | Source establishes view-action classes, local allowlisted reads, native terminal attach (refuses non-local target), startup CLI subprocess, and socket-only state/navigation boundary. It does not yet trace every registry `build` to reducer/render, nor every error result; remaining verbs should be enumerated before closure. Do not call navigation-only verbs product mutations. |
| C25 permission-policy inputs and launch enforcement | CLI policy choice entry `packages/cli/src/commands/policy.ts:298-314`; resolution/provenance `packages/daemon/src/domain/rigspec-instantiator.ts:1877-2003`; posture map `packages/daemon/src/adapters/yolo-mode.ts:36-72`; Claude launch `packages/daemon/src/adapters/claude-code-adapter.ts:226-230`, Codex launch/fork `packages/daemon/src/adapters/codex-runtime-adapter.ts:323-380`, Pi launch `packages/daemon/src/adapters/pi-runtime-adapter.ts:237-281`; terminal binding branch `packages/daemon/src/domain/rigspec-instantiator.ts:2181-2243`. | Member-level enforcement chains are cited: resolved provenance reaches adapters; posture becomes harness launch flags for Claude/Codex, resource trust (not native policy) for Pi, and no harness posture flag for terminal. Remaining source closure: enumerate each policy input/spec’s precedence/default/refusal and match to this chain; no universal authorization over subsequent tool actions is claimed. |
| C21 workflow transition families | CLI operations `packages/cli/src/commands/workflow.ts:253-440`; route endpoints `packages/daemon/src/routes/workflow.ts:185-280`; runtime methods in `packages/daemon/src/domain/workflow-runtime.ts` (instantiate/project cited in C21). | Instantiate/project and remote host-pin refusal are traced. Remaining task: search runtime/route/CLI for compile, revise, continue/resume, abort, deadline, exception/stuck, keepalive and boot-sweep paths; per operation record mutation/event/queue effect, validation/failure/refusal result or searched-consumer absence. |
| C03/C24 specs, plugins and skills | Builtin/user spec roots are scanned by `SpecLibraryService` at `packages/daemon/src/startup.ts:1208-1222`; plugin discovery is constructed with OpenRig plugin root and Claude/Codex caches at `:1224-1244`; plugin, skill and context CLI consumers at C24. | Selected discovery/vendor, skill apply and context paths have representative source chains. Residual: per-spec/profile selection, every projection target/cleanup/refusal, and the dynamically selected install paths need member-level reconciliation. A declared file with no matching selected consumer remains explicitly unclaimed, not assumed delivered. |
| C26/C27 scripts and test-system | Root/package scripts and named runners at C26/C27; test-system scenarios no-consumer limitation and eval-case consumer at C26; `scripts/*` ledger gives broad disposition only. | Named build/scenario/eval/gate chains are traced. Remaining task: enumerate tracked operational scripts, find package/CI/docs/manual callers, capture writes/processes and argument/error/disabled behavior, or record searched-caller limitation. Do not require running product tests for this source inventory. |
| C28 twin, demo, spikes and archives | Demo launcher and twin capture cited at C28; UI twin `packages/ui/twin/capture/twin-capture.ts:1-24,130-177`; `packages/tui/spike/*`, `spike/*`, `archive/*` matched by file ledger. | Demo/twin representative process/build effects established. Remaining task: exact tracked entrypoints and source consumer search for every spike/archive artifact; label CI/test/demo-only, production caller, or no caller found. Dynamic/external use remains unresolved after the static search. |

### Integration ownership and research result state

The read-only lanes were `6d8af` C01–C10, `bb9e8` C11–C16, `b0542` C17–C22/C25, and `f87d9` C23–C24/C26–C28. Reviewer `2c469` audited PR #20 commit `228a3d94e13346a1cf544e9e803e8706f9c28705`, confirmed source-backed corrections, and returned a bounded coverage matrix requirement. This candidate adds that matrix and more exact consumer lines; independent re-audit of the published SHA remains required. Kanban follow-up `8f996` owns remaining source closure. No duplicate research lanes were created. Completion remains blocked on source gaps, not owner/runtime answers.

## Declaration and file coverage ledger

Additional inspected seams, kept separate from broad parity claims:

- **O — shared code is consumed in-process, not only through HTTP.** CLI skill import (`packages/cli/src/commands/skill.ts:2-12`) resolves the export (`packages/daemon/src/skill-loadout-surface.ts:1-6`) to catalog reconciliation. Apply stages bytes, updates Git exclusions, swaps directories and writes ownership manifest (`packages/daemon/src/domain/skill-catalog.ts:877-934`). **U:** all conflict/rollback and shared-working-directory cases remain unverified. This closes C24's explicit apply effect, not every exported domain surface.
- **O — Slack configuration and human registry have local CLI consumers.** Slack setup imports the shared surface and saves configuration through an operation receipt (`packages/cli/src/commands/slack.ts:69-108`; file write at `packages/daemon/src/domain/gateway/slack/config.ts:80-85`). Human add imports registry and refuses a second distinct human (`packages/cli/src/commands/gateway.ts:139-163`). **U:** all registry operations and multi-human hand-authored behavior need deeper traces; no multi-human management capability is inferred from fragment support.
- **O — alternative gateway process declaration is retired and has no reachable production caller in the searched pin.** Its module forbids production callers (`packages/daemon/src/domain/gateway/spawn-gateway.ts:1-8`); the function would spawn the compiled process (`:28-43`), while the only located caller is test-only (`packages/daemon/test/gateway-spawn-e2e.test.ts:3,84`). Normal daemon startup constructs the in-process subsystem (C16). This establishes no current source-reachable production path in the searched consumers, not a retention/removal decision or a claim about external manual invocation.
- **O — ask is evidence retrieval, not an established model-answering service.** AskService returns structured or transcript/chat excerpts and insufficient-evidence guidance (`packages/daemon/src/domain/ask-service.ts:151-229`) to C07's route. No extra model inference is claimed.
- **O — chat has its own durable chain.** C09's mounted send/history routes call repository and emit (`packages/daemon/src/routes/chat.ts:18-64`); repository inserts messages/topics (`packages/daemon/src/domain/chat-repository.ts:41-64`). SSE consumer path subscribes and sends history (`packages/daemon/src/routes/chat.ts:67-121`). **U:** reconnect, ordering and owner workflow value remain unresolved.
- **O — queue creation reaches SQL, not just a repository interface.** C08's create transaction calls the writer; SQL row, transition and event writes are visible at `packages/daemon/src/domain/queue-repository.ts:1450-1499`. This supports the durable-effect claim but not a live handoff result.
- **O — Compose readiness produces receipts.** C20's orchestrator calls adapter, polls/evaluates and stores receipt (`packages/daemon/src/domain/service-orchestrator.ts:39-108`). **U:** external health observations and failure cleanup remain runtime gaps.
- **O — access checks are scoped mechanisms.** Bearer comparison uses timing-safe equality (`packages/daemon/src/middleware/auth-bearer-token.ts:36-50`), and terminal WS has its own guard (C14). **S:** single-user/no-OAuth posture is stated at `packages/daemon/src/middleware/auth-bearer-token.ts:11-22`. **U:** full route-by-route authorization, origin/identity and read confidentiality audit is deferred; neither middleware existence nor bearer injection establishes universal API protection.

The following ledger is a completeness backstop, not an assertion that every listed file was read line by line. **O** declarations are pinned. Each declaration has a C-row owner. Shared effects use that row's chain; where a member's distinct effects were not traced, its disposition is **deferred: member-level consumer/effect evidence needed**. That is an explicit evidence gap, not a dropped surface. File groups cover tracked files at the pin; new files at a later commit require a new reconciliation.

### CLI registrations

Every registered command factory is listed. The factory name is a source identifier, not necessarily its public spelling. The C-row supplies the examined chain and its limits.

| Registration | Family | Declaration evidence |
|---|---|---|
| `startCommand` | C02 | `packages/cli/src/index.ts:173-173` |
| `daemonCommand` | C02 | `packages/cli/src/index.ts:174-174` |
| `statusCommand` | C06 | `packages/cli/src/index.ts:175-175` |
| `snapshotCommand` | C05 | `packages/cli/src/index.ts:176-176` |
| `restoreCommand` | C05 | `packages/cli/src/index.ts:177-177` |
| `crashCartCommand` | C05 | `packages/cli/src/index.ts:178-178` |
| `gatewayCommand` | C16 | `packages/cli/src/index.ts:179-179` |
| `parkedCommand` | C05 | `packages/cli/src/index.ts:180-180` |
| `exportCommand` | C03 | `packages/cli/src/index.ts:181-181` |
| `importCommand` | C03 | `packages/cli/src/index.ts:182-182` |
| `uiCommand` | C12 | `packages/cli/src/index.ts:183-183` |
| `tuiCommand` | C13 | `packages/cli/src/index.ts:184-184` |
| `packageCommand` | C03 | `packages/cli/src/index.ts:185-185` |
| `bootstrapCommand` | C03 | `packages/cli/src/index.ts:186-186` |
| `requirementsCommand` | C03 | `packages/cli/src/index.ts:187-187` |
| `discoverCommand` | C03 | `packages/cli/src/index.ts:188-188` |
| `attachCommand` | C04 | `packages/cli/src/index.ts:189-189` |
| `bindCommand` | C04 | `packages/cli/src/index.ts:190-190` |
| `adoptCommand` | C04 | `packages/cli/src/index.ts:191-191` |
| `bundleCommand` | C03 | `packages/cli/src/index.ts:192-192` |
| `upCommand` | C03 | `packages/cli/src/index.ts:193-193` |
| `downCommand` | C05 | `packages/cli/src/index.ts:194-194` |
| `archiveCommand` | C04 | `packages/cli/src/index.ts:196-196` |
| `unarchiveCommand` | C04 | `packages/cli/src/index.ts:197-197` |
| `hostCommand` | C15 | `packages/cli/src/index.ts:198-198` |
| `psCommand` | C06 | `packages/cli/src/index.ts:199-199` |
| `mcpCommand` | C11 | `packages/cli/src/index.ts:200-200` |
| `agentCommand` | C03 | `packages/cli/src/index.ts:201-201` |
| `rigCommand` | C03 | `packages/cli/src/index.ts:202-202` |
| `transcriptCommand` | C06 | `packages/cli/src/index.ts:203-203` |
| `sendCommand` | C07 | `packages/cli/src/index.ts:204-204` |
| `streamCommand` | C09 | `packages/cli/src/index.ts:205-205` |
| `queueCommand` | C08 | `packages/cli/src/index.ts:206-206` |
| `slackCommand` | C16 | `packages/cli/src/index.ts:207-207` |
| `projectCommand` | C09 | `packages/cli/src/index.ts:208-208` |
| `viewCommand` | C14 | `packages/cli/src/index.ts:209-209` |
| `terminalCommand` | C14 | `packages/cli/src/index.ts:210-210` |
| `watchdogCommand` | C22 | `packages/cli/src/index.ts:211-211` |
| `workflowCommand` | C21 | `packages/cli/src/index.ts:212-212` |
| `captureCommand` | C07 | `packages/cli/src/index.ts:213-213` |
| `broadcastCommand` | C07 | `packages/cli/src/index.ts:214-214` |
| `walkCommand` | C07 | `packages/cli/src/index.ts:215-215` |
| `askCommand` | C07 | `packages/cli/src/index.ts:216-216` |
| `chatroomCommand` | C09 | `packages/cli/src/index.ts:217-217` |
| `specsCommand` | C03 | `packages/cli/src/index.ts:218-218` |
| `contextCommand` | C24 | `packages/cli/src/index.ts:219-219` |
| `pluginCommand` | C24 | `packages/cli/src/index.ts:220-220` |
| `skillCommand` | C24 | `packages/cli/src/index.ts:221-221` |
| `agentImageCommand` | C25 | `packages/cli/src/index.ts:222-222` |
| `forkCommand` | C25 | `packages/cli/src/index.ts:223-223` |
| `workspaceCommand` | C23 | `packages/cli/src/index.ts:224-224` |
| `rigModeCommand` | C25 | `packages/cli/src/index.ts:225-225` |
| `policyCommand` | C25 | `packages/cli/src/index.ts:227-227` |
| `whoamiCommand` | C06 | `packages/cli/src/index.ts:228-228` |
| `configCommand` | C25 | `packages/cli/src/index.ts:229-229` |
| `fileCommand` | C23 | `packages/cli/src/index.ts:230-230` |
| `preflightCommand` | C01 | `packages/cli/src/index.ts:231-231` |
| `authCommand` | C25 | `packages/cli/src/index.ts:232-232` |
| `providerCommand` | C25 | `packages/cli/src/index.ts:233-233` |
| `usageCommand` | C25 | `packages/cli/src/index.ts:234-234` |
| `healthCommand` | C22 | `packages/cli/src/index.ts:235-235` |
| `doctorCommand` | C01 | `packages/cli/src/index.ts:236-236` |
| `expandCommand` | C04 | `packages/cli/src/index.ts:237-237` |
| `addMemberCommand` | C04 | `packages/cli/src/index.ts:238-238` |
| `createCommand` | C04 | `packages/cli/src/index.ts:239-239` |
| `growCommand` | C04 | `packages/cli/src/index.ts:240-240` |
| `reconcileSessionCommand` | C04 | `packages/cli/src/index.ts:241-241` |
| `envCommand` | C04 | `packages/cli/src/index.ts:242-242` |
| `unclaimCommand` | C04 | `packages/cli/src/index.ts:243-243` |
| `releaseCommand` | C04 | `packages/cli/src/index.ts:244-244` |
| `launchCommand` | C04 | `packages/cli/src/index.ts:245-245` |
| `removeCommand` | C04 | `packages/cli/src/index.ts:246-246` |
| `shrinkCommand` | C04 | `packages/cli/src/index.ts:247-247` |
| `destroyCommand` | C04 | `packages/cli/src/index.ts:248-248` |
| `setupCommand` | C01 | `packages/cli/src/index.ts:249-249` |
| `restoreCheckCommand` | C05 | `packages/cli/src/index.ts:250-250` |
| `restorePacketCommand` | C05 | `packages/cli/src/index.ts:251-251` |
| `compactPlanCommand` | C25 | `packages/cli/src/index.ts:252-252` |
| `compactCommand` | C25 | `packages/cli/src/index.ts:253-253` |
| `heartbeatCommand` | C22 | `packages/cli/src/index.ts:254-254` |
| `seatCommand` | C25 | `packages/cli/src/index.ts:255-255` |
| `handoverCommand` | C25 | `packages/cli/src/index.ts:256-256` |
| `startupProofCommand` | C25 | `packages/cli/src/index.ts:257-257` |
| `scopeCommand` | C23 | `packages/cli/src/index.ts:259-259` |
| `proofCommand` | C23 | `packages/cli/src/index.ts:261-261` |

### Daemon API mounts

These are route-family declarations, including families without a public CLI command. Family-specific endpoints are not all claimed investigated. Direct handlers and the terminal WebSocket seam follow the mount table.

| API mount | Family | Declaration evidence |
|---|---|---|
| `/api/rigs` | C04 | `packages/daemon/src/server.ts:684-684` |
| `/api/rigs/:rigId/sessions` | C04 | `packages/daemon/src/server.ts:685-685` |
| `/api/rigs/:rigId/cmux` | C14 | `packages/daemon/src/server.ts:687-687` |
| `/api/rigs/:rigId/nodes` | C04 | `packages/daemon/src/server.ts:688-688` |
| `/api/sessions` | C04 | `packages/daemon/src/server.ts:689-689` |
| `/api/adapters` | C17 | `packages/daemon/src/server.ts:690-690` |
| `/api/events` | C10 | `packages/daemon/src/server.ts:691-691` |
| `/api/rigs/:rigId/snapshots` | C05 | `packages/daemon/src/server.ts:692-692` |
| `/api/rigs/:rigId/restore` | C05 | `packages/daemon/src/server.ts:693-693` |
| `/api/crash-cart` | C05 | `packages/daemon/src/server.ts:694-694` |
| `/api/rigs/import` | C03 | `packages/daemon/src/server.ts:695-695` |
| `/api/packages` | C03 | `packages/daemon/src/server.ts:698-698` |
| `/api/agents` | C03 | `packages/daemon/src/server.ts:699-699` |
| `/api/bootstrap` | C03 | `packages/daemon/src/server.ts:700-700` |
| `/api/discovery` | C03 | `packages/daemon/src/server.ts:701-701` |
| `/api/bundles` | C03 | `packages/daemon/src/server.ts:702-702` |
| `/api/ps` | C06 | `packages/daemon/src/server.ts:703-703` |
| `/api/up` | C03 | `packages/daemon/src/server.ts:704-704` |
| `/api/info` | C02 | `packages/daemon/src/server.ts:705-705` |
| `/api/down` | C05 | `packages/daemon/src/server.ts:706-706` |
| `/api/kernel` | C02 | `packages/daemon/src/server.ts:707-707` |
| `/api/startup` | C02 | `packages/daemon/src/server.ts:708-708` |
| `/api/transcripts` | C06 | `packages/daemon/src/server.ts:709-709` |
| `/api/transport` | C07 | `packages/daemon/src/server.ts:710-710` |
| `/api/compaction` | C25 | `packages/daemon/src/server.ts:713-713` |
| `/api/activity` | C17 | `packages/daemon/src/server.ts:722-722` |
| `/api/ask` | C07 | `packages/daemon/src/server.ts:723-723` |
| `/api/wake-resolve` | C22 | `packages/daemon/src/server.ts:724-724` |
| `/api/specs/review` | C03 | `packages/daemon/src/server.ts:725-725` |
| `/api/specs/library` | C03 | `packages/daemon/src/server.ts:726-726` |
| `/api/plugins` | C24 | `packages/daemon/src/server.ts:727-727` |
| `/api/skills` | C24 | `packages/daemon/src/server.ts:729-729` |
| `/api/config` | C25 | `packages/daemon/src/server.ts:730-730` |
| `/api/context-packs` | C24 | `packages/daemon/src/server.ts:731-731` |
| `/api/agent-images` | C25 | `packages/daemon/src/server.ts:732-732` |
| `/api/whoami` | C06 | `packages/daemon/src/server.ts:735-735` |
| `/api/provider` | C25 | `packages/daemon/src/server.ts:738-738` |
| `/api/seat` | C25 | `packages/daemon/src/server.ts:739-739` |
| `/api/rigs/:rigId/chat` | C09 | `packages/daemon/src/server.ts:740-740` |
| `/api/stream` | C09 | `packages/daemon/src/server.ts:741-741` |
| `/api/queue` | C08 | `packages/daemon/src/server.ts:742-742` |
| `/api/workspace` | C23 | `packages/daemon/src/server.ts:743-743` |
| `/api/projects` | C09 | `packages/daemon/src/server.ts:744-744` |
| `/api/views` | C14 | `packages/daemon/src/server.ts:745-745` |
| `/api/watchdog` | C22 | `packages/daemon/src/server.ts:746-746` |
| `/api/workflow` | C21 | `packages/daemon/src/server.ts:747-747` |
| `/api/mission-control` | C23 | `packages/daemon/src/server.ts:748-748` |
| `/api/hosts` | C15 | `packages/daemon/src/server.ts:756-756` |
| `/api/slices` | C23 | `packages/daemon/src/server.ts:761-761` |
| `/api/review` | C23 | `packages/daemon/src/server.ts:763-763` |
| `/api/terminal` | C14 | `packages/daemon/src/server.ts:767-767` |
| `/api/rigs/:rigId/terminal` | C14 | `packages/daemon/src/server.ts:768-768` |
| `/api/missions` | C23 | `packages/daemon/src/server.ts:772-772` |
| `/api/files` | C23 | `packages/daemon/src/server.ts:774-774` |
| `/api/progress` | C23 | `packages/daemon/src/server.ts:775-775` |
| `/api/scope/audit` | C23 | `packages/daemon/src/server.ts:776-776` |
| `/api/scopes` | C23 | `packages/daemon/src/server.ts:778-778` |
| `/api/telemetry` | C25 | `packages/daemon/src/server.ts:780-780` |
| `/api/scope/approve` | C23 | `packages/daemon/src/server.ts:782-782` |
| `/api/proof` | C23 | `packages/daemon/src/server.ts:783-783` |
| `/api/steering` | C22 | `packages/daemon/src/server.ts:785-785` |
| `/api/health-summary` | C22 | `packages/daemon/src/server.ts:786-786` |
| `/api/health` | C22 | `packages/daemon/src/server.ts:787-787` |
| `/api/attention` | C22 | `packages/daemon/src/server.ts:788-788` |
| `/api/health-diagnosis` | C22 | `packages/daemon/src/server.ts:789-789` |
| `/api/gateway` | C16 | `packages/daemon/src/server.ts:791-791` |
| `/api/rigs/:rigId/env` | C04 | `packages/daemon/src/server.ts:792-792` |
| `/api/restore-check` | C05 | `packages/daemon/src/server.ts:793-793` |
| `/api/rig-mode` | C25 | `packages/daemon/src/server.ts:796-796` |

Direct handlers: **O**, `/healthz` (C02/C06), spec YAML/JSON export (C03), API-not-found and static SPA fallback (C12) are at `packages/daemon/src/server.ts:625-697` and `packages/daemon/src/server.ts:800-835`. Terminal WebSocket registration (C14) is at `packages/daemon/src/server.ts:713-720`. Middleware identity/auth/read-through are dependencies, not standalone proof of safe access; deeper security review is deferred.

### MCP tool ledger

**O:** each tool is an SDK callback that calls the daemon client; C11 owns transport/error conversion. Downstream effect evidence is in the referenced family, with unresolved parity as stated there.

| Tool | Effect family | Handler evidence |
|---|---|---|
| `rig_up` | C03 | `packages/cli/src/mcp-server.ts:59-82` |
| `rig_down` | C05 | `packages/cli/src/mcp-server.ts:83-107` |
| `rig_ps` | C06 | `packages/cli/src/mcp-server.ts:108-122` |
| `rig_status` | C02 | `packages/cli/src/mcp-server.ts:123-137` |
| `rig_snapshot_create` | C05 | `packages/cli/src/mcp-server.ts:138-154` |
| `rig_snapshot_list` | C05 | `packages/cli/src/mcp-server.ts:155-171` |
| `rig_restore` | C05 | `packages/cli/src/mcp-server.ts:172-189` |
| `rig_discover` | C03 | `packages/cli/src/mcp-server.ts:190-204` |
| `rig_bind` | C04 | `packages/cli/src/mcp-server.ts:205-229` |
| `rig_bundle_inspect` | C03 | `packages/cli/src/mcp-server.ts:230-252` |
| `rig_agent_validate` | C03 | `packages/cli/src/mcp-server.ts:253-269` |
| `rig_rig_validate` | C03 | `packages/cli/src/mcp-server.ts:270-286` |
| `rig_rig_nodes` | C06 | `packages/cli/src/mcp-server.ts:287-303` |
| `rig_send` | C07 | `packages/cli/src/mcp-server.ts:304-337` |
| `rig_capture` | C07 | `packages/cli/src/mcp-server.ts:338-362` |
| `rig_chatroom_send` | C09 | `packages/cli/src/mcp-server.ts:363-398` |
| `rig_chatroom_watch` | C09 | `packages/cli/src/mcp-server.ts:399-429` |
| `rig_add` | C04 | `packages/cli/src/mcp-server.ts:430-460` |

MCP chat watch returns bounded history rather than a stream (`packages/cli/src/mcp-server.ts:399-425`); add checks partial launch status as a structural error (`packages/cli/src/mcp-server.ts:449-455`). These differences prohibit a blanket CLI/MCP parity claim.

### Browser destinations

All C12. **O:** declared path→route component. **Deferred:** each destination’s complete actions/effects and redirects need member-level examination; imported feature names are not a completeness claim. C03/C04/C06/C21/C23/C24/C25 give representative backend chains. Labs additionally belong to C28.

| Declared path | Evidence |
|---|---|
| `/` | `packages/ui/src/routes.tsx:98-98` |
| `/topology` | `packages/ui/src/routes.tsx:108-108` |
| `/topology/rig/$rigId` | `packages/ui/src/routes.tsx:114-114` |
| `/topology/pod/$rigId/$podName` | `packages/ui/src/routes.tsx:120-120` |
| `/topology/seat/$rigId/$logicalId` | `packages/ui/src/routes.tsx:126-126` |
| `/for-you` | `packages/ui/src/routes.tsx:132-132` |
| `/project` | `packages/ui/src/routes.tsx:138-138` |
| `/project/mission/$missionId` | `packages/ui/src/routes.tsx:144-144` |
| `/project/slice/$sliceId` | `packages/ui/src/routes.tsx:150-150` |
| `/agents` | `packages/ui/src/routes.tsx:161-161` |
| `/fleet` | `packages/ui/src/routes.tsx:173-173` |
| `/workflows` | `packages/ui/src/routes.tsx:183-183` |
| `/workflow/instance/$instanceId` | `packages/ui/src/routes.tsx:189-189` |
| `/specs` | `packages/ui/src/routes.tsx:204-204` |
| `/specs/applications` | `packages/ui/src/routes.tsx:210-210` |
| `/specs/skills` | `packages/ui/src/routes.tsx:218-218` |
| `/specs/skills/$skillToken` | `packages/ui/src/routes.tsx:224-224` |
| `/specs/skills/$skillToken/file/$fileToken` | `packages/ui/src/routes.tsx:233-233` |
| `/specs/plugins` | `packages/ui/src/routes.tsx:244-244` |
| `/files` | `packages/ui/src/routes.tsx:250-250` |
| `/plugins/$pluginId` | `packages/ui/src/routes.tsx:259-259` |
| `/specs/$specKind/$specName` | `packages/ui/src/routes.tsx:271-271` |
| `/settings` | `packages/ui/src/routes.tsx:280-280` |
| `/settings/policies` | `packages/ui/src/routes.tsx:290-290` |
| `/settings/log` | `packages/ui/src/routes.tsx:295-295` |
| `/settings/status` | `packages/ui/src/routes.tsx:300-300` |
| `/search` | `packages/ui/src/routes.tsx:306-306` |
| `/lab/project-graphics-preview` | `packages/ui/src/routes.tsx:312-312` |
| `/lab/card-previews` | `packages/ui/src/routes.tsx:320-320` |
| `/lab/vellum-lab` | `packages/ui/src/routes.tsx:330-330` |
| `/lab/vellum-bg/a-large` | `packages/ui/src/routes.tsx:341-341` |
| `/lab/vellum-bg/b-small` | `packages/ui/src/routes.tsx:346-346` |
| `/lab/vellum-bg/c-allover` | `packages/ui/src/routes.tsx:351-351` |
| `/rigs/$rigId` | `packages/ui/src/routes.tsx:376-376` |
| `/rigs/$rigId/nodes/$logicalId` | `packages/ui/src/routes.tsx:382-382` |
| `/import` | `packages/ui/src/routes.tsx:391-391` |
| `/packages` | `packages/ui/src/routes.tsx:397-397` |
| `/packages/install` | `packages/ui/src/routes.tsx:403-403` |
| `/packages/$packageId` | `packages/ui/src/routes.tsx:409-409` |
| `/bootstrap` | `packages/ui/src/routes.tsx:415-415` |
| `/agents/validate` | `packages/ui/src/routes.tsx:421-421` |
| `/specs/rig` | `packages/ui/src/routes.tsx:430-430` |
| `/specs/agent` | `packages/ui/src/routes.tsx:436-436` |
| `/specs/library/$entryId` | `packages/ui/src/routes.tsx:442-442` |
| `/discovery` | `packages/ui/src/routes.tsx:453-453` |
| `/discovery/inventory` | `packages/ui/src/routes.tsx:468-468` |
| `/bundles/inspect` | `packages/ui/src/routes.tsx:474-474` |
| `/bundles/install` | `packages/ui/src/routes.tsx:480-480` |
| `/context` | `packages/ui/src/routes.tsx:491-491` |
| `/mission-control` | `packages/ui/src/routes.tsx:498-498` |
| `/slices` | `packages/ui/src/routes.tsx:505-505` |
| `/slices/$name` | `packages/ui/src/routes.tsx:511-511` |
| `/progress` | `packages/ui/src/routes.tsx:521-521` |
| `/steering` | `packages/ui/src/routes.tsx:528-528` |

### TUI registry and exported domain surfaces

**O:** TUI resource commands and literal verbs build view actions; grammar/socket consume the registry (C13). The registry already distinguishes resource drills (`packages/tui/src/commands/registry.ts:67-83`), terminal/file/settings/project/workflow operations (`:86-109`), tab/graph/style/scroll/copy view actions (`:129-208`), and filter/cross-navigation/palette/narrative actions (`:210-276`). These action classes have different effects; the socket exposes view-state/navigation actions while daemon-mutating acts are separately executed by `main.ts` (C13). **U:** mapping every verb to its reducer/render/error effect and checking interactive behavior remains open; do not infer browser/MCP parity. Registry comments about CI are stated intent, not test results.

| TUI literal verb | Evidence |
|---|---|
| `terminals` | `packages/tui/src/commands/registry.ts:87-87` |
| `terminal-preview` | `packages/tui/src/commands/registry.ts:88-88` |
| `attention` | `packages/tui/src/commands/registry.ts:89-89` |
| `read` | `packages/tui/src/commands/registry.ts:90-90` |
| `system` | `packages/tui/src/commands/registry.ts:96-96` |
| `config` | `packages/tui/src/commands/registry.ts:97-97` |
| `setting` | `packages/tui/src/commands/registry.ts:98-98` |
| `refresh` | `packages/tui/src/commands/registry.ts:99-99` |
| `timezone` | `packages/tui/src/commands/registry.ts:100-100` |
| `recent` | `packages/tui/src/commands/registry.ts:101-101` |
| `connections` | `packages/tui/src/commands/registry.ts:102-102` |
| `back` | `packages/tui/src/commands/registry.ts:103-103` |
| `projects` | `packages/tui/src/commands/registry.ts:104-104` |
| `project` | `packages/tui/src/commands/registry.ts:105-105` |
| `source` | `packages/tui/src/commands/registry.ts:106-106` |
| `mission` | `packages/tui/src/commands/registry.ts:107-107` |
| `workflow` | `packages/tui/src/commands/registry.ts:108-108` |
| `packet` | `packages/tui/src/commands/registry.ts:109-109` |
| `:` | `packages/tui/src/commands/registry.ts:111-111` |
| `/` | `packages/tui/src/commands/registry.ts:120-120` |
| `tab` | `packages/tui/src/commands/registry.ts:129-129` |
| `graph` | `packages/tui/src/commands/registry.ts:145-145` |
| `style` | `packages/tui/src/commands/registry.ts:155-155` |
| `scroll` | `packages/tui/src/commands/registry.ts:166-166` |
| `select-text` | `packages/tui/src/commands/registry.ts:179-179` |
| `top` | `packages/tui/src/commands/registry.ts:192-192` |
| `bottom` | `packages/tui/src/commands/registry.ts:201-201` |
| `find` | `packages/tui/src/commands/registry.ts:211-211` |
| `spec-of` | `packages/tui/src/commands/registry.ts:221-221` |
| `running` | `packages/tui/src/commands/registry.ts:234-234` |
| `help` | `packages/tui/src/commands/registry.ts:249-249` |
| `reqs` | `packages/tui/src/commands/registry.ts:259-259` |
| `narrative` | `packages/tui/src/commands/registry.ts:269-269` |

Resource-generated verbs and aliases also belong to C13 (`packages/tui/src/commands/registry.ts:67-86`); socket `state` belongs to C13 (`packages/tui/src/socket-server.ts:24-60`).

| Daemon export | Disposition | Evidence |
|---|---|---|
| `./attention` | **O:** TUI consumes shared `AttentionRead`/`AttentionItem` types in view state and rendering; `packages/tui/src/attention/attention-model.ts:1-15`, `packages/tui/src/pulse/pulse-model.ts:1-10`. | `packages/daemon/package.json:8-8` |
| `./project-catalog` | **O:** work-install planning reads project catalog; CLI importer at `packages/cli/src/lib/work-install.ts:1-5`; related workspace/project read chain C23. | `packages/daemon/package.json:9-9` |
| `./daemon-shutdown` | **O:** CLI daemon lifecycle consumes shutdown receipt/constants; `packages/cli/src/daemon-lifecycle.ts:1-16`; process stop effect belongs C02. | `packages/daemon/package.json:10-12` |
| `./crash-cart` | **O:** `rig crash-cart` lazily imports the export and composes daemon-state probes/discovery into structured read-only output (`packages/cli/src/commands/crash-cart.ts:59-105`); TUI invokes that CLI verb as its recovery/startup probe (`packages/tui/src/main.ts:184-205`). Effect is a visible crash-cart verdict/recovery view, not a restore mutation. Runtime recovery correctness remains unresolved. | `packages/daemon/package.json:14-16` |
| `./gateway-human-registry` | **O:** CLI human add/list/show/remove dynamically imports shared registry; add refuses unverifiable/second-human state before write (`packages/cli/src/commands/gateway.ts:139-163,204-223,245-280`). | `packages/daemon/package.json:18-20` |
| `./gateway-protocol` | **U — precise reachability limit:** searched pinned production packages for `@openrig/daemon/gateway-protocol`; no production importer was found. `packages/cli/tsconfig.json:16` is a compile-time path mapping, not a runtime consumer. Dynamic/external package consumption remains unresolved; export/build presence alone does not establish process reachability. | `packages/daemon/package.json:22-24` |
| `./spec-conformance` | **O:** CLI doctor compares authored specs to live topology; importer at `packages/cli/src/commands/doctor.ts:1-17`. | `packages/daemon/package.json:26-28` |
| `./seat-recap-store` | **O:** CLI `context recap-write` dynamically imports store, writes durable recap and prints predecessor count (`packages/cli/src/commands/context.ts:727-757`). | `packages/daemon/package.json:30-32` |
| `./gateway-slack` | **O:** CLI Slack commands dynamically import service; setup writes config through operation receipt and prompts token/verify/enable (`packages/cli/src/commands/slack.ts:67-109`). | `packages/daemon/package.json:34-36` |
| `./context-pack-taxonomy` | **O:** context install uses exported taxonomy constants (`packages/cli/src/lib/context-install.ts:1-4`); format validation/import effect in C24. | `packages/daemon/package.json:38-40` |
| `./skill-loadout` | **O:** CLI context/skill/work-install import loadout for resolution/apply (`packages/cli/src/commands/context.ts:34-40`, `packages/cli/src/commands/skill.ts:3-12`, `packages/cli/src/lib/work-install.ts:1-15`); durable apply effect C24. | `packages/daemon/package.json:42-44` |
| `./instance-initialization` | **O:** workspace init imports scaffolding at `packages/cli/src/commands/config-init-workspace.ts:1-14`; daemon lifecycle calls default workspace/instance initialization (`packages/cli/src/daemon-lifecycle.ts:9-16`). | `packages/daemon/package.json:46-48` |
| `./system-world` | **O:** work-install plan resolves selected system context via `packages/cli/src/lib/work-install.ts:7-15`; effects represented in C23/C24. | `packages/daemon/package.json:50-52` |
| `./project-lifecycle` | **O:** work-install imports mission composition validators (`packages/cli/src/lib/work-install.ts:12-15`); file planning/effect remains C23. | `packages/daemon/package.json:54-56` |
| `./health-projection` | **O:** `rig health` consumes projection types/constants and calls health APIs (`packages/cli/src/commands/health.ts:1-25`); C22 projection result. | `packages/daemon/package.json:58-60` |
| `./health-detectors` | **O:** CLI health uses detector projection type (`packages/cli/src/commands/health.ts:8-9`); runtime result consumer is C22. | `packages/daemon/package.json:62-64` |
| `./local-reading` | **O:** read-only subprocess imports allowlisted resolver (`packages/cli/src/local-reading.ts:1-20`); effects are constrained local file reads, not daemon writes. | `packages/daemon/package.json:66-68` |

### Tracked-file coverage groups

Patterns below use Python `fnmatchcase` over repository-relative paths from `git ls-tree -r --name-only` at the pin. In this check, `*` includes `/`. First matching row owns the file. This is **file disposition coverage**, not line-by-line investigation or a package/Node count. Cited files have the examined claims above; other members are explicitly deferred for the stated reason. Metadata is not silently counted as a user feature. No file-group disposition decides retention.

| Pattern | Family and disposition |
|---|---|
| `packages/cli/src/*` | C01–C25: declaration coverage only for the whole tree. Only chains cited in C-rows/addenda are semantically investigated; remaining command/helper consumers and effects are source gaps, not runtime deferrals. |
| `packages/daemon/src/*` | C02–C25: mount coverage only for the whole tree. Only cited route/domain chains are semantically investigated; remaining domain/middleware/migration/adapter/helper effects and alternative gateway caller closure are source gaps. |
| `packages/tui/src/*` | C13: entry/registry/socket/live seams and literal verbs are inventoried; each verb's dispatch/reducer/render/effect chain remains a source gap except where separately cited. |
| `packages/ui/src/*` | C12: route declarations and representative read are inventoried; each interactive action's hook/API/effect chain remains a source gap except where separately cited. |
| `packages/daemon/assets/*` | C17/C19/C24: shipped hook/continuity/plugin/guidance assets; cited wiring investigated; embedded helpers without cited caller/effect remain source gaps. |
| `packages/daemon/specs/*` | C03/C24: declarative rigs/agents/skills/workflows; selected chains investigated; per-spec consumer, selection and workflow transition chains remain source gaps. |
| `packages/daemon/context-packs-src/*` | C24: generation input; generator consumer investigated, per-pack/profile delivery deferred. |
| `packages/daemon/policies/*` | C25: policy inputs; apply/launch seams investigated; per-policy resolution-to-enforcement chains remain source gaps, not generic runtime deferrals. |
| `packages/test-system/*` | C26: scenarios, eval cases, topology fixtures and instructions; runner consumers investigated, per-case compatibility and seeded evidence deferred. |
| `packages/ui/twin/*` | C28: separate build/capture artifacts; orchestration investigated, fixture completeness and real captures deferred. |
| `packages/tui/spike/*` | C28: spike material; current consumer/relevance unresolved, deeper trace deferred rather than assumed production or dead. |
| `packages/*/test/*` | C26/C27: source-test material; only selected seams inspected; test results and comprehensive assertion coverage deferred, not executed. |
| `packages/*/scripts/*` | C26/C27: selected operational/build/test/probe/generator scripts investigated; scripts without a traced caller, mutation/output and failure effect remain source gaps. |
| `packages/ui/public/*` | C12/C28: static assets; SPA asset consumer investigated, individual asset use deferred. |
| `packages/*` | C01/C12/C13/C27: remaining package manifests, configuration, entry HTML and README metadata; declarations inspected, detailed tooling behavior deferred. |
| `scripts/*` | C24/C27: packaging, gates, guards, generated membership/digests, testbed/bootstrap and sync tools; named chains investigated, remaining invocation/effect paths deferred. |
| `skills/*` | C24/C28: distribution metadata and mirrored skill content; mirror chain investigated, per-skill operational consumer deferred. |
| `docs/*` | C28: instructions/reference/release/design material; cited journey intent inspected; other statements need source confirmation, not runtime authority. |
| `demo/*` | C28: executable example/probes; launch script chain investigated, probe internals and dirty-state impact deferred. |
| `docker/*` | C26/C27: container/testbed inputs; accepted baseline reused; actual build/runtime provisioning deferred. |
| `spike/*` | C28: executable experiment; present-day product caller and owner relevance unresolved, deferred. |
| `archive/*` | C28: historical material by placement; active consumers unresolved, no assumption that executable references are inert. |
| `assets/*` | C28: presentation assets; user-facing placement/usage deferred. |
| `config/*` | C27: shared toolchain config; manifest tooling chain investigated, individual option effects deferred. |
| `.github/*` | C27/C28: CI and contributor/security templates; portability workflow investigated, other process intent not treated as executed behavior. |
| `.evidence/*` | C27/C28: existing evidence artifacts; no trust or execution verdict imported; provenance/content review deferred. |
| `breakdown/*` | Research context, not product capability; accepted counts/workflow limits reused. Other outputs and shared registers unchanged. |
| `package.json` | C27: root script declarations investigated. |
| `package-lock.json` | C27: resolved dependency metadata; accepted Node inventory reused; transitive execution graph deferred. |
| `vitest.config.ts` | C27: test configuration; execution deferred. |
| `README.md` | C28: project intent, not an executed workflow. |
| `CHANGELOG.md` | C28: release statements; not imported as current source/runtime proof. |
| `LICENSE` | Distribution/legal metadata, not a runtime capability; legal interpretation outside this inventory. |
| `.gitignore` | Repository/build-output metadata; not a user capability. |

## Validation procedure and review boundary

Documentation validation is separate from application tests. The embedded command below checks every citation against the pinned Git blobs, reconciles the declaration ledgers, and checks file-group disposition coverage. It writes no files. It deliberately cannot decide whether a claim is true: the C-row/source review supplies that check. It also cannot turn a deferred member into an investigated chain. No Markdown links are used; source citations resolve through Git rather than the owner's machine or a mutable web branch.

Run from the isolated worktree root:

```sh
python3 - <<'PY'
import fnmatch, json, pathlib, re, subprocess
pin = 'ea7c268f576ada8434d3dae3e6ac972264910d4c'
root = pathlib.Path(subprocess.check_output(['git', 'rev-parse', '--show-toplevel'], text=True).strip())
owned = 'breakdown/research-capability-inventory.md'
doc = (root / owned).read_text()
body = doc.split('## Validation procedure and review boundary')[0]
cache = {}
def source(path):
    if path not in cache:
        cache[path] = subprocess.check_output(['git', 'show', f'{pin}:{path}'], text=True)
    return cache[path]
refs = re.findall(r'`([^`\n]+):(\d+)-(\d+)`', body)
assert refs, 'no citations'
for path, first, last in refs:
    assert 1 <= int(first) <= int(last) <= len(source(path).splitlines()), (path, first, last)
print(f'PASS citation ranges: {len(refs)} references; {len(cache)} source files')
checks = [
    ('CLI factories', 'CLI registrations', 'packages/cli/src/index.ts', r'program.addCommand\((\w+)\('),
    ('API mounts', 'Daemon API mounts', 'packages/daemon/src/server.ts', r'app.route\(\s*"([^"]+)"'),
    ('MCP tools', 'MCP tool ledger', 'packages/cli/src/mcp-server.ts', r'server.tool\(\s*"([^"]+)"'),
    ('UI paths', 'Browser destinations', 'packages/ui/src/routes.tsx', r'path:\s*"([^"]+)"'),
    ('TUI literal verbs', 'TUI registry and exported domain surfaces', 'packages/tui/src/commands/registry.ts', r'name:\s*"([^"]+)"'),
]
ledger = body.split('### CLI registrations')[1]
for label, heading, path, pattern in checks:
    section = body.split('### ' + heading + '\n')[1].split('\n### ')[0]
    entries = re.findall(pattern, source(path))
    missing = [entry for entry in entries if f'| `{entry}` |' not in section]
    assert not missing, (label, missing)
    print(f'PASS {label}: {len(entries)} declarations; no omissions')
exports = json.loads(source('packages/daemon/package.json'))['exports']
assert all(f'| `{entry}` |' in ledger for entry in exports)
print(f'PASS daemon exports: {len(exports)} declarations; no omissions')
assert 'registry.ts:67-86' in body and 'socket-server.ts:24-60' in body
assert 'server.ts:713-720' in body and 'server.ts:800-835' in body
patterns = re.findall(r'^\| `([^`]+)` \|', body.split('### Tracked-file coverage groups')[1], re.M)
files = subprocess.check_output(['git', 'ls-tree', '-r', '--name-only', pin], text=True).splitlines()
unmatched = [path for path in files if not any(fnmatch.fnmatchcase(path, pat) for pat in patterns)]
assert not unmatched, unmatched
unused = [pat for pat in patterns if not any(fnmatch.fnmatchcase(path, pat) for path in files)]
assert not unused, unused
print('PASS file dispositions: every pinned tracked file matched; no unused patterns')
families = re.findall(r'^\| (C\d\d) —', body, re.M)
assert len(families) == len(set(families)) and set(families) == {f'C{i:02}' for i in range(1, 29)}
assert all(c in families for c in re.findall(r'\bC\d\d\b', body))
print('PASS capability references: unique C01–C28; no dangling family IDs')
assert not re.search(r'\]\([^)]*\)', body), 'new Markdown link needs explicit validation'
print('PASS links: Git source citations only; no unchecked Markdown links')
PY
git diff --check
git diff --cached --check
git status --short
```

For single-file scope, compare the union of unstaged, staged and untracked paths with the owned path before commit. After commit, check `git diff-tree --no-commit-id --name-only -r HEAD` and a clean `git status --short`. No validation script or shared index is added. The final review report records actual results and documentation commit; it does not claim these checks verify the deferred product behavior.

### Validation record — 2026-09-27

The embedded read-only validator passed: 465 citation ranges across 115 source files; no omitted declarations in the CLI, API-mount, MCP, browser-path, TUI-literal or daemon-export ledgers; every pinned tracked file matched a disposition group; no unused patterns or dangling capability IDs. Citation syntax checks found no malformed range tokens. No Markdown link targets require separate resolution. Manual source review corrected a queue-creation citation to the actual create transaction and added its SQL writer evidence.

`git diff --check` and `git diff --cached --check` passed. The unstaged/staged/untracked-path union contained only the owned inventory file. These results concern documentation structure, source resolution and edit scope, not runtime correctness. All deferred evidence above remains open; no product test result or research-gate status is recorded.

Follow-up validation after decomposition initially passed with 478 citation ranges across 121 source files; later source-audit corrections increased the current candidate to 499 citation ranges across 130 source files. Watchdog delivery, provider-switch refusal, recap-write, UI/TUI dispositions, context delivery inputs, test-system tree reconciliation and export importers were incorporated. Structural coverage passes; final independent semantic audit and the residual member-level source questions remain open. This record does not claim runtime behavior or task completion.
