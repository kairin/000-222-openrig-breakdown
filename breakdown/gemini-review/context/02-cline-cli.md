# Cline CLI (reference design, not a claim about OpenRig)

Verbatim from the Gemini sources. Not a claim to verify.

## From the report

### **Cline CLI: Integrated Agent Runtime and Visual Kanban Orchestration**

Cline CLI represents the command-line evolution of Cline Core, the standalone engine that powers Cline's extensions across integrated development environments such as Visual Studio Code and JetBrains IDEs1. Architecturally, Cline Core executes as a Node.js process exposing a gRPC service that completely separates presentation layers from underlying agent execution loops1. This structural separation allows a single agent loop to maintain persistent task state, context history, and execution checkpoints whether operating within an interactive terminal, an IDE sidebar, or an automated continuous integration pipeline1.  
At the command-line level, Cline functions as a single self-contained binary installed globally via standard package management and requiring Node.js 22 or later6. The interface supports interactive terminal dialogues, background task execution via Zen mode, and headless scripting pipelines using structured JSON streaming and automatic tool approval6. Security and governance are enforced through approval policy hooks that inspect and intercept tool invocations before execution, alongside fine-grained command permission policies6.  
To address multi-agent fleet orchestration, Cline integrates a local browser-based Kanban interface executed directly from the primary binary6. The Kanban board treats tasks as cards within an asynchronous workflow6. Crucially, the platform solves the problem of file contention by automatically provisioning an isolated Git worktree for every card6. Parallel agents execute within separate branches, editing files and executing tests without risking branch collisions6. Tasks can be chained in dependency sequences where downstream cards trigger automatically upon predecessor completion6. Developers interact through inline diff reviews, providing line-level feedback directly into the agent's context, or deploy designated coordinator agents that supervise specialist worker agents via shared task boards and mailboxes6.

## From the infographic

> Native Agent Runtime & Fleet Kanban  
> How Cline CLI Operates: Simplicity by Design  
> Cline CLI builds directly upon Cline Core—the same battle-tested engine powering the popular VS Code extension. By delegating task fleet concurrency to Git worktrees and a visual local Kanban board, it removes operational guesswork.  
> Cline Architecture Blueprint  
> 1. INITIATION  
> $ cline --kanban  
> Launches local web server presenting visual board. Developer posts requirements as cards or triggers autonomous coordinator agents.  
> 2. ISOLATION ENGINE  
> git worktree add task-01  
> Creates isolated file system folder for each agent task. Parallel tasks never mutate each other's working trees or corrupt branches.  
> 3. SUPERVISION & AUDIT  
> --hook-command "validate.sh"  
> Every tool execution passes through deterministic policy hooks. Changes are inspected as unified visual diffs before merging.  
> Cline Local Kanban Simulator (Click Cards to Inspect)  
> Active Worktrees: 2  
> Queued (1)  
> Wait  
> #TASK-104  
> Blocked on #102  
> E2E Playwright Suite  
> Branch: feat/playwright-e2e  
> Running in Worktree (2)  
> Active  
> #TASK-102  
> Writing Code  
> Auth Middleware Migration  
> Worktree: .cline/wt-auth-jwt  
> #TASK-103  
> Running Tests  
> Stripe Webhook Handler  
> Worktree: .cline/wt-stripe  
> Diff Ready for Merge (1)  
> Ready  
> #TASK-101  
> 100% Tests Passed  
> User Schema Validation  
> Diff: +142 / -18 lines  
> Task #102: Auth Middleware Migration  
> RUNNING  
> Cline agent is rewriting express sessions to JWT cookie tokens in an isolated git worktree branch `feat/jwt-tokens`. Master branch files are completely untouched during this operation.  
> Isolation Path: /projects/app/.git/worktrees/wt-auth-jwt  
> Zero Branch Conflict  
> 1  
> Zero Config Terminal CLI  
> Run cline "prompt" directly in bash or zsh. Supports Zen background execution and pure headless JSON streaming for CI/CD pipelines.  
> 2  
> Coordinator Agent Hierarchy  
> A lead coordinator agent breaks epics down into visual Kanban cards, assigns sub-agents, and gathers outputs through an asynchronous mailbox queue.  
> 3  
> Granular Approval Hooks  
> Use --hook-command to pipe every shell tool or file edit through organizational security scanners or human approval gates before execution.  
