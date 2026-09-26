# Introduction and side-by-side table

Verbatim from the Gemini sources. Not a claim to verify.

## From the report

# **Comparative Analysis of Autonomous Agent Architectures: Cline CLI, OpenRig, and Enterprise Software Factory Frameworks**

The transition from single-prompt conversational assistants to autonomous software engineering agents has exposed fundamental architectural challenges in multi-agent orchestration1. As engineering organizations seek to deploy autonomous agent fleets capable of navigating complex codebases, running build pipelines, and executing long-horizon tasks, the choice of orchestration pattern dictates operational reliability, system observability, and developer cognitive load1.  
A critical evaluation of Cline CLI and OpenRig illustrates two opposing paradigms in contemporary agent tooling: an integrated single-agent core that scales horizontally through visual task management and Git isolation, contrasted with a local meta-harness designed to supervise external terminal sessions through low-level operating system multiplexing6. Understanding the conceptual friction inherent in meta-harness systems like OpenRig clarifies the operational requirements for building manageable, enterprise-grade software factories3.

## **Structural Foundations and Operational Paradigms of Cline CLI and OpenRig**

The operational divergence between Cline CLI and OpenRig stems from fundamentally different design goals regarding process supervision, runtime encapsulation, and developer interaction6.

| Architectural Vector | Cline CLI | OpenRig |
| :---- | :---- | :---- |
| **Primary System Role** | Native, scriptable agent runtime and visual task orchestrator1 | Local meta-harness control plane supervising third-party CLI agents7 |
| **Core Execution Engine** | Cline Core (Node.js daemon exposing a gRPC service)1 | External agent harnesses (Claude Code, Codex CLI) hosted inside tmux \[cite: 8, 13\] |
| **Workspace Isolation Mechanism** | Automatic Git worktrees per task card to prevent branch collisions6 | Multiplexed terminal panes sharing local filesystems or repository paths7 |
| **Primary User Interfaces** | Interactive terminal CLI and local browser-based Kanban board6 | Command-line interface and terminal user interface (rig tui)7 |
| **State and History Management** | Event-driven local data directories, checkpoints, and task history6 | SQLite database tracking topology graphs, chatrooms, and task queues8 |
| **Multi-Agent Coordination Model** | Visual card dependency chains, coordinator agent, and shared mailbox6 | Addressable seats, peer terminal injection, and transactional work queues7 |
| **System Prerequisites** | Node.js 22+, API keys or local OpenAI-compatible endpoints6 | Node.js 20+, tmux, active logins for Claude Code and/or Codex CLI13 |
| **Governance and Control** | Scriptable approval hooks (--hook-command) and command allowlists6 | Behavioral markdown norms (CULTURE.md) and watchdog schedules7 |

## From the infographic

> The Two Opposing Paradigms of Agent Orchestration  
> Why did OpenRig feel so difficult, and what makes Cline CLI intuitive? Here is the visual dichotomy between an Integrated Agent Loop  
> and a Terminal Meta-Harness  
> .  
> Integrated Core  
> Node 22+ / gRPC  
> Cline CLI  
> An execution engine with its own native cognitive loop, automated Git worktree file isolation, and a visual browser-based Kanban fleet board.  
> ARCHITECTURE FLOW  
> Self-Contained  
> ●  
> cline --kanban  
> Visual UI  
> │ ↓ Spawns isolated tasks with dependencies  
> ●  
> Git Worktree per Card  
> Zero Collision  
> │ ↓ Direct inference via API / Ollama  
> ●  
> Cline Core (gRPC Loop)  
> Native State  
> Human Cognitive Load  
> Low (Intuitive Kanban)  
> Workspace Isolation  
> Git Worktree Branches  
> Target Persona  
> Individual Dev & Lead  
> External Dependencies  
> None (Self-Contained)  
> Explore Cline CLI Capabilities  
> Meta-Harness Control Plane  
> tmux / Hono / SQLite  
> OpenRig  
> Not a coding agent itself, but a supervisor daemon that wraps host tmux sessions running Claude Code or Codex CLI with telco-style routing.  
> ARCHITECTURE FLOW  
> Supervisory Wrapper  
> ●  
> OpenRig Daemon (Hono/SQLite)  
> Topology Engine  
> │ ↓ Injects keystrokes / commands into terminal  
> ●  
> Host tmux Multiplexer  
> Session Sprawl  
> │ ↓ Executes third-party CLI agents  
> ●  
> Claude Code / Codex CLI  
> External Auth  
> Human Cognitive Load  
> Very High (Telco Lexicon)  
> Workspace Isolation  
> Multiplexed Panes (Shared)  
> Target Persona  
> CLI Researchers / Power Users  
> External Dependencies  
> tmux + Claude/Codex logins  
> Diagnose Why OpenRig is Confusing  
> At-A-Glance Reality Check  
> 100%  
> Self-Contained (Cline)  
> One binary handles inference, workspace git isolation, tool approval, and web UI.  
> 3 Layers  
> Indirection (OpenRig)  
> Host CLI ➔ SQLite Daemon ➔ OS tmux pane ➔ Claude Code CLI process.  
> Zero Collision  
> Git Worktrees  
> Cline spins isolated disk directories automatically per task card to protect master.  
> Docker / k8s  
> Enterprise Standard  
> Software factories (OpenHands) isolate fleet agents in real microVM containers, not tmux.  
