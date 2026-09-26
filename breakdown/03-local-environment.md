# 3. Local environment

## 3.1 The upstream installation

The upstream CLI is installed on this computer. The table shows the installation.

| Item | Value |
| :--- | :--- |
| Package | `@openrig/cli` 0.5.15 (from npm) |
| Commands | `~/.local/bin/rig`, `~/.local/bin/openrig-tui` |
| State folder | `~/.openrig` |

The repository is at version 0.5.16. Thus the installed version is not the same as the repository version.

## 3.2 Possible conflicts

A build of this repository uses the same command names and the same state folder. This causes these possible problems:

- You can start the upstream `rig` when you want to start your version.
- Your version can change data in `~/.openrig` that the upstream version uses.
- If you change the data format, the upstream version can stop working.

The daemon also writes to the configuration of the agents. For example:

- It writes the `openrig-core` plugin into `~/.openrig/plugins/`.
- It writes and removes managed hook blocks in the Codex configuration.
- It writes managed blocks for Claude Code (the last upstream release selects `CLAUDE.local.md`).

CAUTION: DO NOT OPERATE THE UPSTREAM VERSION AND YOUR VERSION WITH THE SAME STATE FOLDER. ONE VERSION CAN CHANGE DATA THAT THE OTHER VERSION USES.

## 3.3 Recommended actions

1. Make a decision: keep the upstream installation, or remove it.
2. If you keep it, find out if the state folder can change. Start with `getDefaultOpenRigPath` in the daemon.
3. Give the command of your version a different name, or do not install it globally.
4. Make a backup copy of `~/.openrig` before you start your version for the first time.

## 3.4 Workspace rules

The workspace file `AGENTS.md`, in the folder above this repository, applies to this repository. It gives these rules:

- Do not commit, push or change the Git history if the owner does not ask for it.
- Do not put secrets in files, commands, logs or output.
- Treat each folder in the workspace as a separate project.
