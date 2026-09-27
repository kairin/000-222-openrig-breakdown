# 5. Simplification rules

## 5.1 What to keep

OpenRig lets Claude Code and Codex CLI agents operate together. It does not replace these agents (claims A1 and S1). This is possibly the most important function. Examine it before you remove other functions.

## 5.2 Rules for changes

1. Record the test results before you change the code.
   NOTE: The upstream project had some tests that failed before. See `.evidence/AB-full-suite-preexisting-failures.txt`.
2. Change one function at a time.
3. Run the tests after each change. Compare the results with the record from step 1.
4. Prefer removing unneeded functions before adding new ones. For a retained function, prove its replacement and rollback before removing the old path; this rule does not authorize destructive removal ahead of a dependency.
5. When you remove a function, also plan removal or migration of the files it owns on the computer. Examples are hooks, plugins and managed blocks in agent configuration files. Verify ownership, preserve user changes and required recovery data, and test cleanup before cutover; do not delete entire user configuration files.
6. When you change the code, change `docs/as-built/` in the same commit. If you do not keep a document up to date, remove it.
7. Use words that engineers know. For example, use "agent" or "role" if "seat" does not give more meaning.

CAUTION: IF YOU REMOVE A FUNCTION BUT NOT ITS FILES, OLD HOOKS AND PLUGINS CAN STAY IN THE CLAUDE CODE AND CODEX CONFIGURATION. THESE FILES CAN CHANGE HOW THE AGENTS OPERATE.

## 5.3 Maintenance without upstream

You do not get upstream updates. Thus you must do this work yourself:

- Update the dependencies.
- Apply security fixes. While the current npm dependency tree remains, run `npm audit` at regular intervals. This is a transitional maintenance instruction, not a reason to retain npm after Node elimination. Before removing it, define equivalent dependency, advisory and license checks for the retained Rust/tooling stack; no replacement tool is selected or verified here.
- Make sure that the tool operates with new versions of Claude Code and Codex CLI.

NOTE: A smaller code base needs less maintenance. This is one more reason to remove functions that you do not use.

## 5.4 Rust and Node-elimination constraint

The owner’s destination is fully Rust for all retained project functionality
and full Node removal from project-owned runtime, install, update, generation,
build, test and release paths. Non-Rust or Node exceptions require explicit
owner approval. A shell entrypoint without Node is not a Node dependency, but
retaining it still needs a boundary decision under the fully Rust goal. Do not merely
bundle Node, replace its launcher or leave required Node hooks behind. Browser
JavaScript and independently supplied agent tools have separate boundaries in
[10](10-rust-and-node-removal-plan.md#goal-and-boundary). A runtime-only milestone
must name remaining tooling dependencies and their later removal prerequisites.

The [task list](07-review-task-list.md) and Gates A–D in
[09](09-adversarial-review-and-research-charter.md#gates-and-open-decisions)
still control readiness. The firm destination does not preselect retained
features, authorize a rewrite or establish safe recovery. The source inventory
in 10 records Node hooks and ownership-sensitive configuration paths; its
future runtime acceptance tests remain unperformed. Documentation-only changes
must check links, citations, consistency and scope; they must not claim the
product test baseline or migration is complete.
