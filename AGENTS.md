# 000-222-openrig-breakdown — AI Agent Guidelines

Single source of truth for AI agents in this repository. If `CLAUDE.md` or `GEMINI.md` exists, it points to this file.

## Security and secrets

- Never expose, print, log, commit, or include in diffs, prompts, fixtures, screenshots, or generated files any API key, token, password, credential, private key, session cookie, or other secret. Redact with `<REDACTED>`.
- Never ask the user to paste a secret into chat. Do not read or display secret-file contents. If a credential is missing, stop and explain how to provide it securely.
- Secrets live in the owner's `pass` password store. A command gets a secret only through `with-secret <service>/<name> -- <command>`. Never put a secret in `.env`, `.envrc`, `.envrc.local`, source code, config or documentation.
- Do not send personal or sensitive data to an external service unless the user explicitly authorizes it.

## Protected files

Do not rewrite these without an explicit per-file instruction from the user: `.gitignore`, `package.json` (at the root and in each `packages/*` folder), `package-lock.json`, `.github/workflows/portability-report.yml`. Read them freely. If a task needs one changed, stop and ask. `git show origin/main:<file>` is the authoritative original.

## Project overview

This repository is a personal, simplified derivative of the open-source OpenRig project (version 0.5.16). The tool runs several Claude Code and Codex CLI agents together as one team. The project is in research: the code is still the same as upstream, and no simplification is made yet. All analysis is in `breakdown/`. No design or plan is approved. Open decisions for the owner are in `breakdown/research-owner-decision-brief.md`.

## Technologies

- TypeScript on Node.js 20, 22 or 24.
- npm with workspaces: `packages/daemon`, `packages/ui`, `packages/cli` and `packages/tui`.
- The daemon is a Hono HTTP server with an SQLite database (`better-sqlite3`).
- Each agent runs in tmux. Teams are described in YAML files.
- Vitest for the workspace tests. `node --test` for the repository scripts in `scripts/`.
- GitHub Actions: `portability-report.yml` lists added lines that hold machine-specific values.

## Development workflow

```bash
npm install
npm run build
npm test
npm run lint
npm run test:ui
node scripts/portability-report.mjs --staged   # check staged lines for machine-specific values
```

Record the test results before you change code. Some upstream tests already failed before this project started. See `.evidence/AB-full-suite-preexisting-failures.txt`.

A task is done only when you show evidence: command output, a log line or a screenshot.

## Branches and pull requests

- Never push to `main` directly. Make a branch, push it, and open a pull request.
- Keep `CHANGELOG.md` up to date if the repository has one.
- This repository is public. Do not add a local absolute path, a personal email address or any machine-specific value. Private repository names and their GitHub links are allowed. Run the portability report before you commit.

## Documentation

- `README.md`: for users.
- `AGENTS.md` (this file): for agents. Update it when a tool, a command or a rule changes.

## Project notes

- The code still uses the upstream command names (`rig`, `openrig-tui`), package names (`@openrig/*`) and state folder. Do not install a build of this repository next to an installed upstream `@openrig/cli`. See `breakdown/03-local-environment.md`.
- The current code changes files outside this repository: Claude Code, Codex and tmux configuration in the user's home folder. It also tries to download a plugin from the upstream project when the daemon starts. Do not run it unless the user asks. The full list is in `archive/documents/README-upstream-original.md`.
- Upstream OpenRig code is under Apache-2.0. Original work in this project is under AGPL-3.0-only. `REUSE.toml` gives the license for each path.

## Git identity

Commit as `Mister K <678459+kairin@users.noreply.github.com>`. This is the
public GitHub name and the GitHub noreply email. Do not commit with another
name or with a personal email address. Check with `git config user.name` and
`git config user.email` before you commit.
