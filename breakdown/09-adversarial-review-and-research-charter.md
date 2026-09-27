# Adversarial review and research charter

Status: research and learning plan, not an implementation decision.

Prepared on 2026-09-26 against checkout `147993aa` by an Astra review with repository reading and evidence gathering delegated to Luna. The companion [current-state evidence](08-current-state-evidence.md) records source locations, historical evidence, and limitations. Existing user instructions in the root README and this folder remain authoritative; this document elaborates how to investigate them.

## What the project is trying to achieve

The current goal is a smaller personal tool for running Claude Code and Codex CLI agents together as a team. Its owner should have less to learn, fewer moving parts to maintain, and enough clarity to set it up, run it, and fix it without asking an agent to interpret the system. Stopping must preserve uncommitted work. The derivative should be self-contained with respect to the original project's services and distributed resources.

The owner's destination is fully Rust for all retained project functionality and full Node removal from project-owned runtime, installation, update, generation, build, test and release paths, not merely a Rust-first preference or core-only hypothesis. Non-Rust or Node exceptions require explicit owner approval. This 2026-09-27 clarification is a stated requirement, not a source-derived result or implementation approval. Research must find the smallest understandable system that fulfills the useful workflow within that destination. It must still establish which behaviors, interfaces and subsystems earn their cost. See [10](10-rust-and-node-removal-plan.md) for exact source evidence, boundaries and conditional stages, and [07](07-review-task-list.md) for preserved T1–T9 statuses and dependencies.

The learning outcome is the ability to explain and eventually change this system with confidence: what happens for an owner action, which component owns each effect, where state lives, how failures are recovered, why complexity appeared, and what must remain true if the implementation changes.

## Review of the original six-stage proposal

The original proposal covered baseline, history, runtime, foundations, engineering workflow, and teaching material. Those are useful areas of study, but insufficient decision criteria for simplification.

| Weakness | Why it matters here | Required improvement |
| --- | --- | --- |
| “Full breakdown” has no completion boundary | The inherited project is broad; a comprehensive prose tour can grow indefinitely without helping the owner use it. | Inventory the whole capability surface, then study retained candidate workflows deeply. Record every deferred area and its dependency on those workflows. |
| Architecture is studied before the desired experience is explicit | Existing packages and command groups can become an accidental specification for the derivative. | Start with the owner's outcome, then map the implementation that supports it. Use a provisional workflow and expose the assumptions it makes. |
| Historical explanations can become plausible stories | A commit proves a change occurred; it does not automatically prove why its author made that choice. | Link causal claims to design text, issues, tests, or explicit commit descriptions. Mark reconstructed motivation as an inference. |
| Documents can appear more authoritative than current code | The inherited as-built documentation is explicitly tied to v0.3.1; the current package baseline is v0.5.16. | Use those documents as a reading map and check consequential claims against the current source. |
| Language migration is not tied to user benefit | The active workspace is currently TypeScript/Node, with no Rust implementation identified. A port may reproduce the same concepts and operating burden. | Compare language choices against deployment, process ownership, recovery, comprehensibility, and behavior compatibility. |
| Happy paths dominate ordinary code tours | This tool manages agent processes and working directories. The owner's explicit requirement to preserve unfinished work depends on shutdown and failure behavior. | Pair each core journey with interruption, restart, stale state, and partial-failure analysis. |
| “Make it simpler” has no observable meaning | Fewer files or more Rust lines can coexist with a harder user experience. | Establish a baseline of actions, decisions, concepts, dependencies, and recovery effort. Compare each proposal against it. |
| Deliverables are documents without a decision procedure | A large archive of explanations can become another maintenance burden. | Give every research question a decision it informs, evidence required, and an explicit condition for finishing. |

The revised plan keeps all six subject areas, but organizes them around decisions about the owner's workflow.

## Integrate the review work already defined here

The existing [review method](04-review-method.md) defines a review of 24 Gemini claims, with F1–F11 first and overlapping claims grouped as A5/F4/S2, A8/F9/S5, and F5/S3. That remains useful research work. Treat each claim as a question to check, record yes/part/no with current file/line evidence, and connect it to the capability inventory, owner workflow, or state map it informs. The Gemini material is a claim list; its conclusions do not become established facts through repetition.

The existing [simplification rules](05-simplification-rules.md) also remain in force: remove before adding, change one function at a time, account for generated machine files, use understandable engineering terms, keep documentation synchronized, and maintain dependencies/security/external-agent compatibility independently of upstream. Its baseline-test and comparison requirements belong to a later implementation change. This source-research pass neither runs those tests nor claims that a baseline has been established.

After evidence review, use the existing choices—remove, hide behind a simpler default, rename, or retain when value earns the cost—and record dependencies and uncertainty for each. This charter adds learning and architecture criteria to that process; it does not replace the existing claim register or pre-approve any removal.

## A provisional workflow to investigate first

Use the following scenario as a research anchor, not a newly approved product requirement: the owner opens an existing project, starts a small Claude Code and Codex CLI team, gives it work, sees who is doing what, reviews an outcome, stops the team, and later resumes without losing unfinished changes.

Investigate each transition in both its normal case and one failure case. Do not assume the current queue, session, workspace, tmux, daemon, or UI abstraction is necessary simply because the scenario currently passes through it.

| Owner question | What must be traced | Failure or counterexample to inspect |
| --- | --- | --- |
| How do I install and start it? | Entry command, dependencies, configuration, daemon discovery/startup, readiness and diagnostics. | Missing tool, malformed configuration, existing process or occupied endpoint. |
| How do I put agents into my project? | Project/workspace identity, process creation, adapter behavior, terminal ownership, working directory and isolation. | Agent fails before readiness, process exits, workspace already has local changes. |
| How do I assign and follow work? | Work ownership, persistence, command/API path, notifications and the human-visible result. | Duplicate action, stale owner, disconnected client, partial write or missed notification. |
| How do I understand what happened? | Logs, task/session status, source of truth, presentation and diagnostic commands. | Stored status disagrees with the real process; UI or client reconnects. |
| How do I stop and come back later? | Shutdown ordering, signal handling, process cleanup, persistent state, Git/worktree behavior and recovery. | Abrupt termination, daemon restart, dirty files, untracked files, nested repositories and interrupted cleanup. |

The last column specifies research cases; it does not assert that the current system handles or fails them. This research pass does not execute those scenarios.

## Evidence discipline

Every consequential finding should carry an evidence identifier, repository commit, file/line or historical object, scope, and confidence. Use four labels consistently:

- **Observed in source:** the inspected code implements this path or mechanism at the recorded commit. This is not proof it works in the current machine environment.
- **Stated intent:** a README, design note, specification, or test expresses a goal or expected contract. A test that was only read has not been demonstrated to pass.
- **Inferred:** an explanation that connects evidence but is not explicitly established by it; include a plausible competing explanation.
- **Unresolved:** the evidence needed to settle the question has not been obtained, or sources disagree.

Use user instructions and the derivative's stated goals to decide what is wanted. Use the exact current source to determine implemented mechanisms. Use observed runtime behavior, when separately investigated, to determine what actually happens in a specified environment. Use historical documents to reconstruct previous intent and assumptions. These answer different questions; none substitutes for all the others.

Maintain a contradiction register with the conflicting claims, source dates/commits, likely explanation, and next resolving observation. Examples already warranting attention include the as-built version gap and older open questions that may have been answered by the current root README.

Keep links into existing source and history instead of copying large code blocks or whole historical branches. The local Git object database already contains the fetched refs; inspect historical files or a temporary worktree only when a specific question needs them. Branch names are useful clues, not a reliable chronological architecture sequence.

## Research work packages and completion conditions

| Work package | Questions and method | Deliverable and completion condition |
| --- | --- | --- |
| 1. Establish authority and coverage | Record checkout, dirty worktree, package baseline, local environment evidence, current derivative goals, inherited material and generated/vendor boundaries. Identify every major user capability and its owner package. | An evidence baseline and capability inventory. Every major area is marked investigated, scheduled, or deferred with a reason; no silent gaps. |
| 2. Trace the owner's workflow | For each scenario above, follow input → command/client → runtime component → durable state/process effect → visible result. Record errors, cleanup and restart implications. | Annotated flow maps and small behavior-contract tables. Each step has a source reference and a stated owner; unknown links remain visible. |
| 3. Explain state and hidden coupling | Inventory mutable stores, files, caches and live process state. Establish authority, writers, identity mapping, transaction boundaries, event ordering and reconciliation. | A state-ownership map and invariant ledger. For each core state item, explain who creates it, who changes it, who removes it, and how disagreement is resolved. |
| 4. Reconstruct consequential evolution | Select historical changes that explain today's coupling, user vocabulary, startup path, isolation, persistence and recovery. Compare before/after code at commits, then seek documented motivation. | A small milestone narrative. Each milestone answers what changed, evidence for why, which assumption remains relevant, and what a simpler derivative might learn. |
| 5. Measure and classify complexity | Count owner decisions, exposed concepts, manually required actions, runtime components, configuration touchpoints, and recovery steps for the chosen workflow. Distinguish necessary work from optional capability and duplicated machinery. | A baseline scorecard and per-capability keep/simplify/combine/remove/investigate candidates. Each candidate includes dependencies, user value and uncertainty; none is an implementation instruction. |
| 6. Compare Rust boundaries | Evaluate alternative architectures below using the observed contracts and deployment constraints. Include removal of unneeded behavior as an alternative to porting it. | A decision matrix and boundary sketch with unresolved feasibility questions. A preferred direction needs evidence for concrete owner benefit and a credible compatibility path. |
| 7. Assemble a learning path | Organize explanations by owner action and mechanism; connect source, history and invariants. Check that a reader can trace a new action without searching the entire repository. | A short reading guide, glossary, flow walkthroughs and teach-back exercises. Each exercise has evidence a reader can inspect and a question they should now be able to answer. |

The current research report supplies a baseline and selected source traces. These work packages define the remaining depth needed for a full reverse-engineering effort; they must not be reported as completed merely because this charter describes them.

## Contracts that a simplification or port must confront

Derive exact contracts from the source before choosing a replacement. The following are investigation categories, not claims that existing behavior satisfies them:

- **Preservation of work:** distinguish tracked edits, untracked files, uncommitted index state, worktree metadata, agent transcripts and the project itself. Establish exactly what “stop” may terminate or remove. Absence of a destructive call in one shutdown function is insufficient if cleanup is delegated elsewhere.
- **Ownership and identity:** distinguish the agent process, its session record, a workspace, a work item and the human's project. Determine which identities survive a restart and which are projections of a live process.
- **Durable transitions:** establish allowed work states, who may change them, transaction boundaries, concurrency rules and behavior after a partially completed action.
- **Events and observation:** determine whether clients obtain truth from stored state, events, polling or a mixture; inspect reconnect, ordering, duplication and loss assumptions.
- **Lifecycle and recovery:** record readiness, liveness, signal propagation, child-process ownership, stale-state cleanup and startup reconciliation. A single executable can still contain several independent lifecycles.
- **External agent integration:** identify which behavior belongs to Claude Code/Codex CLI and which is supplied by this project, including terminal interaction, prompts, hooks, permissions and result detection. Rust does not remove an external tool's contract.
- **Persistence and compatibility:** determine which databases, configuration files, installed assets, shell integrations and scripts would be affected by changed formats or command names. Decide explicitly which existing user data must carry forward.

The initial source research has already found four concrete reasons to investigate these contracts before drawing new package boundaries. See the companion evidence report for the inspected locations and qualifications:

| Current mechanism | What we can learn | Consequence for a simplification or Rust port |
| --- | --- | --- |
| Session rows persist independently of tmux liveness; restore distinguishes absent sessions from an unavailable/unknown probe. | Stored intent and the real process can disagree, and an inability to observe is a third state. | A replacement needs an explicit reconciliation policy; treating a stored “running” value as proof of a live process loses information. |
| Queue changes have durable latest state and a transition audit; actor identity comes from the transport. Ordinary create/handoff nudges record transport failure without undoing state. Terminal handoffs additionally stage a durable successor-wake intent in the transaction, deliver after commit and recover pending intents. The guide distinguishes message delivery from acceptance. | Ownership, delivery and acceptance are different events with different authorities. Different notification paths offer different recovery guarantees. | Combining commands or services requires preserving the essential state/identity contracts, including post-commit delivery failure and pending-intent recovery. Do not assume all notifications share the terminal-handoff guarantee. |
| Teardown catches snapshot failure, warns, and continues stopping sessions; restore separately handles live/unknown sessions and missing resume tokens. | Preserving project files, retaining a resumable agent conversation, and restoring a process are different promises. | Define which promises the owner needs and inspect every cleanup path before calling shutdown safe. This static observation does not demonstrate preservation under an actual failed run. |
| Setup manages a block in tmux configuration; the Claude adapter reconciles hook commands it owns while retaining user hooks. | Installed configuration has ownership boundaries beyond the repository tree. | Removing a feature must account for its managed artifacts without erasing user-owned configuration; self-contained deployment needs an inventory of these side effects. |

These findings also challenge a simplistic definition of “fewer moving parts.” Removing a daemon, queue, or terminal dependency can move its responsibilities elsewhere. The research must identify which responsibilities can disappear with scope reduction and which must still be implemented.

## What “simpler” and the fully Rust destination mean during research

Simplicity is primarily an owner outcome. Establish measurements before assigning numerical targets; no usability timings or runtime benchmarks were performed in this pass.

| Dimension | Evidence to collect | Question for a proposed design |
| --- | --- | --- |
| First useful run | Required installs, commands, configuration decisions and prerequisites. | Can the owner reach a working team with fewer decisions and clear feedback? |
| Daily operation | Concepts and commands needed to start, assign, inspect, stop and resume. | Can ordinary use be explained on one short workflow page? |
| Diagnosis and recovery | State locations, log locations, failure messages and corrective actions. | Can the owner explain and fix a failed or interrupted run? |
| Operational footprint | Required processes/services, runtime dependencies and downloaded upstream resources. | Which components can be optional, embedded, combined or removed? |
| Change comprehensibility | Number of independently maintained layers/contracts touched by a representative change. | Can a future maintainer predict the effects of a small change? |
| Preservation and reliability | Specified work-preservation and recovery contracts, supported by source now and targeted demonstrations later. | Does the simpler design maintain the behaviors the owner depends on? |

The destination is firm; the architecture, retained scope and transition order are not. A Rust launcher or CLI over the current Node daemon cannot meet Node elimination. A runtime-only milestone may retain named build/test tooling temporarily, but full removal must also replace or retire required Node generators, scripts, tests and release tooling. Browser JavaScript is not Node; external agent tools require their own stated boundary rather than an unsupported promise of a Node-free host. Retaining non-Rust browser functionality, shell entrypoints or native libraries requires an explicit owner boundary/exception decision, not just a claim of user value. The dependency and contract maps must demonstrate a credible path to the destination before a Rust boundary is recommended.

| Candidate | What it could improve | Main challenge or reason to reject it |
| --- | --- | --- |
| Simplify the current TypeScript system (comparison baseline or temporary step only) | Establishes which complexity can be removed through scope, defaults and interface design; provides a comparison baseline. | Retains Node; cannot be accepted as the requested final state. Any temporary use needs an exit dependency and later replacement/removal proof. |
| Rust CLI over the existing daemon (temporary bridge only) | Allows a narrower native command experience while retaining established domain behavior. | Adds another language and compatibility surface while retaining Node. Requires a concrete daemon/consumer exit path; not Node-elimination completion. |
| Rust core and CLI, optional presentation layer | May consolidate state/lifecycle ownership and make nonessential UI optional. | Requires deliberate behavior mapping for persistence, events, process control and external-agent integrations. |
| Full replacement of all retained components | Gives freedom to redesign a coherent small product. | Has the largest evidence and compatibility burden; a line-for-line rewrite risks preserving unnecessary complexity. |

Score each candidate against the same owner scenario, required behavior, runtime footprint, implementation scope, dependency seams, data transition, learning burden and reversibility. Do not invent weighted scores until priorities are established. Identify a counterexample that would make each candidate unattractive. The TypeScript candidate is a comparison baseline; mixed Rust/Node candidates are intermediate only. If evidence challenges the destination's feasibility, report it and seek an owner decision instead of lowering the goal. Gates A–D below are unchanged.

Before recommending Rust scope, answer: Which concrete deployment or maintenance pain would it remove? Which contracts would need reimplementation? Which capabilities can disappear instead? Is the seam real in the current dependency graph? Can one bounded behavior demonstrate the premise in a later implementation phase? If these remain unknown, report uncertainty rather than endorsing a rewrite.

## Historical research that is actually useful

Study history when it helps explain a current decision. Candidate questions include when the daemon became necessary, how session and workspace ownership evolved, why coordination became more elaborate, what failure motivated a recovery path, how packaging accumulated downloaded assets, and whether previous simplifications were reversed. Start from relevant files and commits, then inspect related design records.

For each milestone, record the prior behavior, resulting behavior, exact commits, contemporary explanation if available, and the continuing constraint. Separate a constraint that still exists from one introduced by upstream's broader audience. Preserve alternative explanations when the historical record is incomplete.

Do not infer a feature's introduction date from a branch tip or assume all branch history is merged. The companion evidence report corrects the earlier conversation's reachability claim: one of the twelve fetched topic refs has a commit outside main. No branch archive is required for this investigation.

## Learner path and teach-back questions

1. Read the derivative's goals and explain the intended owner's daily outcome. Which parts are explicit requirements, and which remain choices?
2. Read one source trace from command to visible result. At each handoff, identify the owning component and the state it reads or changes.
3. Trace a session's creation, observation and shutdown. Explain how stored identity differs from a live process and how unfinished work is protected or where evidence is missing.
4. Trace one coordination action and its persistence/notification path. Explain what another agent or the human sees after a retry or restart.
5. Follow one historical change that introduced an important abstraction. Explain its documented motivation and whether the owner still needs the same generality.
6. Compare two simplification candidates using the same workflow and contracts. Explain which user decisions disappear and which implementation obligations remain.
7. Explain a proposed Rust boundary to another reader using the state map and dependency seams. Identify the strongest evidence against the proposal.

Use a compact vocabulary sheet linking current terms to plain-language meaning and source definitions. Where two terms seem redundant, first establish whether they encode different lifetimes or ownership; merging names without understanding that difference can hide necessary information.

## Gates and open decisions

**Gate A — enough understanding to discuss scope:** a capability inventory exists; core workflow traces have owners and evidence; contradictions and deferred areas are visible.

**Gate B — enough evidence to choose simplifications:** each candidate shows user value, dependencies, behavior/data implications, removal cost and expected change in the baseline scorecard. A proposal must say what would disprove its expected benefit.

**Gate C — enough evidence to recommend a Rust direction:** the candidate architectures have been compared against the same contracts; preservation and recovery obligations are understood; the intended Rust boundary has a specific owner benefit and a bounded way to validate it later.

**Gate D — ready for an implementation plan:** the retained product scope, operating mode, compatibility promises and unanswered high-impact questions are explicit. Implementation is a later task, not an automatic consequence of producing this research.

The Node-elimination requirement does not bypass any gate. At Gate C, show how
the candidate reaches a compliant final state; distinguish comparative baselines
and temporary mixed-runtime steps. At Gate D, record the runtime-only milestones,
remaining build/test/release dependencies, external-tool boundary and full-removal
acceptance criteria. A permanent project-owned Node exception requires an explicit
owner change to the goal; it is not completion of that goal. T2–T9 retain their
statuses in 07, and this charter does not authorize the later stages in 10.

Existing documents should be checked before treating any of these as open: supported operating systems, whether a daemon is essential, whether tmux is a chosen user interface or an internal dependency, which presentation surface is essential, whether existing installations/data must migrate, and exactly what self-contained installation means for bundled assets versus external agent executables. The current root README already names Claude Code and Codex CLI; older unanswered agent-set questions should be reconciled with it.

## Limits and upkeep

This pass performs source/history research and planning. It does not establish measured usability, runtime correctness, benchmark results, or successful installation/recovery on this machine. The companion report should state precisely which source paths were traced and which claims remain unverified.

Keep the learning material small enough to maintain: an index, evidence report, capability/state maps, a few essential walkthroughs, the decision matrix, and unresolved questions. Extend an existing document when it serves the same purpose. Record the inspected commit and update consequential claims when the code changes; a stale architecture map is a lead for investigation, not a current specification.
