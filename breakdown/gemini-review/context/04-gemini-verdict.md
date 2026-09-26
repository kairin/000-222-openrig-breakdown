# Gemini's overall verdict, quiz, scores and recommendations

Verbatim from the Gemini sources. Not a claim to verify.

## From the report

Opening thesis of the "Root Causes" section:

> The difficulty developers encounter when onboarding onto OpenRig is not accidental; it is the direct outcome of its architectural philosophy, which prioritizes low-level systems control and distributed-agent bureaucracy over human ergonomics5.

## **Architectural Synthesis and Implementation Strategy**

Evaluating these platforms demonstrates that the perceived difficulty of OpenRig is structural rather than accidental13. OpenRig was architected to solve a specialized engineering challenge: supervising fleets of third-party command-line coding agents operating simultaneously over extended time horizons, without providing its own internal inference loop7. To prevent drift, it introduces rigorous distributed-systems protocols, terminal multiplexing, and administrative bureaucracy5. For teams that do not specifically require host-level tmux multiplexing of raw Claude Code and Codex CLI sessions, this architecture introduces excessive friction7.  
When designing a production-grade software factory or multi-agent engineering workflow, engineering organizations should adopt tools matching their required scale and governance boundaries3:  
For individual developers or small engineering teams seeking immediate productivity gains without infrastructure overhead, **Cline CLI** provides an optimal balance6. By launching the integrated Kanban board (cline \--kanban), developers gain the ability to run multiple parallel agents safely isolated in Git worktrees, manage card dependencies, and review code changes line by line without managing terminal multiplexers or distributed state databases6.  
For organizations seeking to construct fully autonomous, highly observable software factories operating across multiple repositories, **OpenHands** delivers the most robust enterprise architecture4. Its sandboxed container execution isolates autonomous agents from host infrastructure, its event-sourced state model guarantees complete session reproducibility, and its Git-native multi-agent coordination mirrors proven human engineering workflows4.  
Finally, for structured greenfield generation where predictable requirements decomposition is prioritized over dynamic terminal execution, **MetaGPT** offers an intuitive, document-driven alternative that models software development as a transparent assembly line18. Selecting between these alternatives allows organizations to scale agent capabilities effectively while maintaining clear observability and low operational cognitive overhead3.

## From the infographic

> Interactive Reality Check: "Should You Use OpenRig?"  
> Answer these 3 quick questions to determine if your engineering fleet genuinely benefits from OpenRig's meta-harness architecture.  
> 1. What are your agents executing inside?  
> Interactive tmux terminal sessions with Claude Code CLI  
> Direct API / Sandboxed Docker containers / Local files  
> 2. How do you want to manage concurrent agent tasks?  
> Durable addressable seats with transactional dispatch queues  
> Visual Kanban boards, Git worktrees, and PR reviews  
> Recommendation  
> Recommendation content will populate here.  

> Fleet Evaluation Matrix  
> Executive Decision Matrix & Architecture Fit  
> Compare cognitive load, setup friction, security isolation, and enterprise scalability across all major frameworks.  
> Multi-Vector Framework Radar  
> Scale: 1 (Lowest) - 10 (Highest)  
> Note: For Cognitive Friction, lower score indicates easier human usability.  
> Find Your Ideal Fleet Stack  
> Select your primary constraints to see the architecturally recommended orchestrator.  
> What is your team size and operational scope?  
> Solo Developer / Individual Contributor  
> Small Engineering Squad (2-10 devs)  
> Enterprise Software Org / Factory Fleet  
> What is your security requirement?  
> Local filesystem with Git Branch Isolation is fine  
> Strict containerized microVM isolation (Zero host access)  
> What is your desired primary interface?  
> Visual Kanban Board + Terminal CLI  
> Full Web Control Center + GitHub Integration  
> Clean Python Code / YAML definitions  
> Best Match  
> Alternative: OpenHands  
> Cline CLI + Kanban  
> Offers the fastest onboarding, zero daemon friction, and automatic Git worktree isolation for parallel tasks without host contamination.  
> Complete Framework Synthesis  
> Framework  
> Core Model  
> Cognitive Friction  
> Workspace Isolation  
> Fleet Orchestration  
> Best When...  
> Cline CLI  
> Node gRPC Daemon  
> Low (Familiar Kanban)  
> Git Worktree Per Card  
> Coordinator Agent + Cards  
> You want parallel local feature building without setup headaches.  
> OpenRig  
> tmux Meta-Harness  
> Very High (Telecom/Seats)  
> Multiplexed Terminal Panes  
> Addressable Seats & Queues  
> You are researching multi-CLI agents running in terminal sessions.  
> OpenHands  
> Event-Sourced SDK  
> Moderate (Standard Web)  
> Docker / MicroVM Sandboxes  
> Agent Canvas & GitHub PRs  
> Building an enterprise software factory with rigorous security.  
> MetaGPT  
> Corporate SOPs  
> Low (Predictable SOPs)  
> Output Directory Trees  
> Sequential Role Handoff  
> Scaffolding greenfield applications with PRD and architecture docs.  
> CrewAI  
> Role-Playing Crews  
> Low (Declarative YAML)  
> Python Environment Tools  
> Hierarchical Manager LLM  
> Orchestrating modular multi-agent scripts and business workflows.  
> ← Back to Software Factories  
> Return to Overview  
> ✦  
> TL;DR: The Agent Fleet Cheat Sheet  
> Why Cline CLI is easy:  
> It functions like a modern human tool. You install it, run cline --kanban, and watch tasks execute across safe Git worktrees without touching your master branch.  
> Why OpenRig feels impossible:  
> It doesn't run agents itself; it hijacks host tmux sessions running Claude Code or Codex. It replaces standard developer terms with telecom abstractions (Rigs, Pods, Seats) and expects AI agents to execute its CLI commands.  
> The best software factory for enterprise fleets:  
> OpenHands. It runs agents in actual Docker containers or microVMs, uses an event-sourced architecture for deterministic audit trails, and plugs directly into GitHub and Linear.  

Radar chart scores (1 lowest, 10 highest), from the page's script:

| Axis | Cline CLI | OpenRig | OpenHands |
| :--- | :-: | :-: | :-: |
| Cognitive Simplicity | 9 | 2 | 7.5 |
| Workspace Isolation | 8.5 | 4.5 | 10 |
| Observability / UI | 9 | 3.5 | 8.5 |
| Setup Velocity | 9 | 3 | 6.5 |
| Enterprise Scale | 7 | 5 | 9.5 |

Quiz results, from the page's script:

> You have a deliberate need to coordinate pre-existing Claude Code / Codex terminal processes through tmux sessions with persistent telecom-style seats. Be prepared for high setup friction.

> You want visual task isolation, Git worktrees, and low cognitive friction. OpenRig will overwhelm you with unnecessary tmux indirection and seat management.

> For scalable fleets executing outside tmux, OpenHands provides true Docker sandboxes, event logs, and GitHub PR workflows without terminal hijacking.

