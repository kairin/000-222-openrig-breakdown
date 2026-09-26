# 5. Simplification rules

## 5.1 What to keep

OpenRig lets Claude Code and Codex CLI agents operate together. It does not replace these agents (claims A1 and S1). This is possibly the most important function. Examine it before you remove other functions.

## 5.2 Rules for changes

1. Record the test results before you change the code.
   NOTE: The upstream project had some tests that failed before. See `.evidence/AB-full-suite-preexisting-failures.txt`.
2. Change one function at a time.
3. Run the tests after each change. Compare the results with the record from step 1.
4. Remove a function before you add a new function.
5. When you remove a function, also remove the files that the function wrote on the computer. Examples are hooks, plugins and managed blocks in agent configuration files.
6. When you change the code, change `docs/as-built/` in the same commit. If you do not keep a document up to date, remove it.
7. Use words that engineers know. For example, use "agent" or "role" if "seat" does not give more meaning.

CAUTION: IF YOU REMOVE A FUNCTION BUT NOT ITS FILES, OLD HOOKS AND PLUGINS CAN STAY IN THE CLAUDE CODE AND CODEX CONFIGURATION. THESE FILES CAN CHANGE HOW THE AGENTS OPERATE.

## 5.3 Maintenance without upstream

You do not get upstream updates. Thus you must do this work yourself:

- Update the dependencies.
- Apply security fixes. Run `npm audit` at regular intervals.
- Make sure that the tool operates with new versions of Claude Code and Codex CLI.

NOTE: A smaller code base needs less maintenance. This is one more reason to remove functions that you do not use.
