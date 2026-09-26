# OpenHands, MetaGPT and CrewAI (reference designs)

Verbatim from the Gemini sources. Not a claim to verify.

## From the report

## **Alternative Architectures for Scalable Software Factories and Agent Fleets**

For engineering organizations that require large agent fleets or autonomous software factories without the operational opacity and multiplexer dependencies of OpenRig, several mature paradigms provide accessible, production-ready alternatives4.

### **OpenHands: Enterprise Agent Control Plane and Sandboxed Execution**

OpenHands represents the standard for scalable, containerized software development agents4. Rather than wrapping host terminal windows, OpenHands separates agent cognition from code execution through a formal client-server architecture17.  
At the execution core, the OpenHands Software Agent SDK utilizes an event-sourced architecture20. Agents, tool definitions, and language model interfaces are immutable, while all execution progress is recorded in an append-only event log20. Tool calls adhere to a type-safe tripartite structure: an Action schema validated via Pydantic, a sandboxed Execution mechanism, and an Observation return20. This model guarantees deterministic execution replays, robust session resumption, and complete auditability4.  
For fleet management, the OpenHands Agent Control Plane provides a centralized operational layer designed specifically to prevent agent sprawl4. Code execution, file mutations, and terminal commands are strictly quarantined within ephemeral Docker containers, micro-virtual machines, or remote Kubernetes pods, entirely removing the security risk of unconfined host filesystem access4. Multi-agent software engineering is organized around established version control primitives through architectures like CAID22. A lead agent decomposes high-level feature requirements into a dependency graph, dispatching subtasks to parallel agents that work in isolated Git worktrees, execute unit tests within isolated sandboxes, and submit clean pull requests for automated verification22. Visual oversight is maintained through the Agent Canvas interface or integrated directly into enterprise channels such as GitHub, Slack, and Linear4.

### **MetaGPT: Standard Operating Procedures as Software Assembly Lines**

MetaGPT adopts an assembly-line paradigm modeled directly after traditional software engineering organizations, formalizing the core concept that software engineering can be automated through standard operating procedures18.  
The framework simulates an entire software company by instantiating role-specialized agents that follow sequential standard operating procedures18. When provided with a single natural language requirement, the pipeline executes deterministically:

* A Product Manager agent analyzes the user prompt, conducts competitive landscape assessments, and drafts a structured Product Requirements Document18.  
* An Architect agent reviews the requirements, formulates system architecture documents, selects technical frameworks, and defines precise interface contracts and sequence logic18.  
* A Project Manager agent parses the technical design into an actionable, dependency-mapped task breakdown18.  
* Engineer agents write modular source code fulfilling each assigned task, which are subsequently validated by Quality Assurance agents that generate and execute unit test suites18.

MetaGPT eliminates cognitive complexity by providing a predictable, document-driven development pipeline18. Intermediate artifacts are written to a standard project directory as readable Markdown and source files, allowing human developers to pause the pipeline for review or inspect exactly how requirements translate into code18.

### **CrewAI: Hierarchical Delegation and Declarative Orchestration**

CrewAI provides a flexible, accessible framework for orchestrating collaborative agent teams using clean, human-readable abstractions18. The architecture focuses on role-playing multi-agent configurations where agents, available tools, and assigned objectives are defined in simple YAML files19.  
In software engineering scenarios, CrewAI simplifies coordination through its hierarchical process engine19. Rather than requiring developers to manually build routing graphs or script inter-agent terminal commands, CrewAI allows a designated manager model to evaluate high-level goals, dynamically delegate subtasks to specialized agents based on declared domain expertise, validate intermediate outputs, and synthesize final deliverables19. The framework operates entirely within the native programming environment, requiring no local daemons, terminal multiplexers, or complex system socket configurations, making it one of the most accessible multi-agent orchestrators available8.

| Platform | Primary Abstraction | Execution Environment | Coordination Mechanism | Cognitive Load | Best Architectural Fit |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Cline CLI** | Agent loop with Kanban task board6 | Local machine with Git worktrees per task6 | Coordinator agent, shared mailbox, dependency cards6 | Low; familiar Kanban and CLI interfaces6 | Parallel local feature development and CI/CD pipelines1 |
| **OpenRig** | Meta-harness control plane7 | Host terminal multiplexer (tmux) sessions8 | Inter-seat messaging, transactional queues, chatrooms7 | Very High; deep distributed systems lexicon13 | Experimental multi-agent research across CLI harnesses5 |
| **OpenHands** | Control plane with Agent SDK4 | Sandboxed Docker containers, MicroVMs, Kubernetes4 | Event-sourced state, Git-based CAID dependency trees4 | Moderate; web control center with clear APIs4 | Enterprise autonomous software factories and fleet automation4 |
| **MetaGPT** | Software company assembly line18 | Local filesystem output (workspace/)18 | Sequential SOP handoffs (PRD, Architecture, Code, QA)18 | Low; predictable corporate role hierarchy18 | Greenfield project scaffolding and document generation18 |
| **CrewAI** | Role-based autonomous crews19 | Local Python runtime or containerized tools19 | Hierarchical delegation directed by manager LLM19 | Low; declarative YAML configuration files19 | Flexible task automation and business process workflows19 |

## From the infographic

> Production-Ready Frameworks  
> Better, Easier Software Factory Alternatives  
> If you want to orchestrate large fleets of coding agents without wrestling with OpenRig's terminal multiplexing and complex distributed jargon, three established paradigms lead the industry.  
> Enterprise Factory  
> Docker / k8s  
> OpenHands  
> The de facto standard for open-source autonomous software factories. Built around an Event-Sourced Agent SDK and sandboxed container execution.  
> ✔  
> Sandboxed in microVMs / Docker  
> ✔  
> Event-Sourced audit logs & replays  
> ✔  
> Native GitHub / Linear PR pipeline  
> ✔  
> CAID Multi-Agent git dependency trees  
> Best suited for:  
> Scalable company-wide autonomous fleets requiring isolated container security.  
> Assembly Line SOPs  
> Document-Driven  
> MetaGPT  
> Models software development as a deterministic factory assembly line governed by corporate Standard Operating Procedures (SOPs).  
> ✔  
> Product Manager ➔ PRD generation  
> ✔  
> Architect ➔ System UML & Design  
> ✔  
> Project Manager ➔ Tasks breakdown  
> ✔  
> Engineers & QA ➔ Code + Unit Tests  
> Best suited for:  
> Greenfield repository generation from simple natural-language specs with zero cognitive load.  
> Role Delegation  
> Declarative YAML  
> CrewAI  
> The simplest framework for multi-agent role-playing teams. High-level manager agents dynamically delegate subtasks via clean YAML declarations.  
> ✔  
> Pure Python runtime, zero daemons  
> ✔  
> Human-in-the-loop approvals built-in  
> ✔  
> Hierarchical or sequential workflows  
> ✔  
> Huge library of pre-built tools  
> Best suited for:  
> Custom business automation, data research, and flexible developer orchestration scripts.  
> How Scalable Software Factories Avoid the "OpenRig Trap"  
> Compare how OpenHands, MetaGPT, and Cline CLI resolve the four biggest operational headaches:  
> Challenge  
> OpenRig Approach  
> Cline CLI Approach  
> OpenHands Factory Approach  
> Execution Isolation  
> tmux windows on host OS (shared disk)  
> Git worktree branches per card  
> Sandboxed microVM / Docker container  
> Observability & UI  
> tmux TUI & machine JSON logs  
> Browser Kanban & Inline diff viewer  
> Agent Canvas web UI + Linear / GitHub  
> State Recovery  
> Non-blocking snapshots (risk of loss)  
> Git commits & task checkpoints  
> Immutable event-sourced append logs  
> External Auth  
> Requires host Claude / Codex CLI logins  
> Direct API keys, Ollama, OpenRouter  
> Enterprise LLM proxy / Key rotation  
