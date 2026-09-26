# 6. Open questions

The owner must answer these questions. The answers change the work that follows.

| No. | Question | Why the answer is important |
| :-: | :--- | :--- |
| 1 | Who will use the simpler tool: only the owner, or other persons also? | If other persons use it, the documentation and the setup must be easy. |
| 2 | Will you publish the tool, for example on npm? | If yes, you must change the name and the package name. You must also add the license notices (see 2.4). |
| 3 | Which agents must the tool operate: Claude Code, Codex CLI, or the two? | Support for one agent only removes much code. |
| 4 | Must the tool continue to use tmux? | tmux is a large part of the design (claim A3). Another method changes the architecture. |
| 5 | Do you keep the upstream `docs/` folder? | These documents become incorrect when the code changes. |
| 6 | Do you keep the upstream installation of `@openrig/cli` 0.5.15? | It uses the same command name and state folder as your version (see 3.2). |
| 7 | Do you keep the upstream Git history? | The history shows the origin of the code. It does not cause a problem if you keep it. |
