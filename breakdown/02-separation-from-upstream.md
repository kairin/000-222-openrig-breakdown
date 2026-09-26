# 2. Separation from upstream

## 2.1 Work that is complete

These tasks were done on 2026-09-26:

1. The `upstream` Git remote was removed. The only remote is `origin` (`kairin/openrig-breakdown`).
2. The default repository for the `gh` command was set to `kairin/openrig-breakdown`.
3. The GitHub repository left the fork network of `mvschwarz/openrig`. The GitHub API now shows `fork: false`.

The results are:

- `git push` and `gh pr create` send changes only to `kairin/openrig-breakdown`.
- GitHub does not show a link to `mvschwarz/openrig`.
- You cannot get upstream updates with Git.
- You cannot join the fork network again.

## 2.2 Links to upstream that stay in the code

The Git separation does not remove the links to upstream in the files. The table shows the links.

| File | Link | Possible problem |
| :--- | :--- | :--- |
| `packages/daemon/src/domain/plugin-vendor-service.ts:59` | The daemon tries to download the `openrig-core` plugin from `github.com/mvschwarz/openrig-plugins`. | The daemon does this at each start (`packages/daemon/src/startup.ts:706`). Now that repository is empty. If upstream adds files, your daemon can install upstream code. |
| `archive/configuration/context7.json` | Archived from the root; it points to `context7.com/mvschwarz/openrig` with the upstream public key. | It is retained for reference and is no longer discovered as this repository's Context7 configuration. |
| `packages/cli/package.json` | The package name is `@openrig/cli`. The package is not private. `repository` and `author` point to upstream. | `npm publish` tries to publish into the upstream npm scope. |
| `packages/daemon/assets/plugins/openrig-core/.claude-plugin/plugin.json` | `homepage` and `repository` point to upstream. | Users see upstream as the owner. |
| `.github/` | `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`, and `SECURITY.md` live here, where GitHub also discovers community-health files. | Their contents still describe the upstream process and should be reviewed for this derivative. |

CAUTION: THE DAEMON TRIES TO DOWNLOAD CODE FROM AN UPSTREAM REPOSITORY AT EACH START. REMOVE THIS FUNCTION OR CHANGE THE ADDRESS BEFORE YOU USE THE DAEMON FOR REAL WORK.

## 2.3 Recommended actions

1. Remove the automatic plugin download, or change `REPO_BASE` to a repository that you own.
2. Keep the upstream Context7 profile archived; create a new root `context7.json` only if this derivative is intentionally published to Context7.
3. Set `"private": true` in `packages/cli/package.json`. Do this before you run a publish script.
4. Change `repository`, `author` and `homepage` fields to your repository.
5. Rewrite the upstream process files in `.github/` for this repository.

## 2.4 License and name

The Apache License 2.0 applies to the code. These conditions apply when you give copies of the code to other persons:

- Keep the `LICENSE` file.
- Put a clear notice in each file that you change. The notice must tell that you changed the file (section 4(b)).
- There is no `NOTICE` file in the repository. Thus section 4(d) does not apply now.

The license does not give permission to use the name "OpenRig" (section 6). If you publish the simpler tool, use a different name.

NOTE: These conditions apply only when you distribute the code. For private use on your computers, the conditions about notices do not apply.
