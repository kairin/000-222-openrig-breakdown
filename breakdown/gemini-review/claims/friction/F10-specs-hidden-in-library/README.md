# F10: Rig specs live in an internal library reached through commands, not as editable files in the project.

**Verdict:** part — shipped specs have a discoverable library, but they are real YAML files and editable project-local copies can be launched directly. Library-only access is false.

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> Finally, topology blueprints are stored within an internal product library accessed via specialized commands rather than as transparent, editable configuration files within the working project directory, obscuring how agent parameters are structured13.

## What the infographic says

Not mentioned.

## Sources Gemini cited

- [13] [Getting started - OpenRig](https://www.openrig.dev/docs/getting-started)

## How to check

Find where built-in specs live in the package and whether a user can put their own spec file in the project directory and boot it.

## Evidence from this repo

Reviewed at source commit `9db3ed6c406be5c3d9a84720383fcf6b543169e6` (2026-09-27).

- **Observed in source:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/specs/rigs/launch/first-project/rig.yaml:1–31` is a concrete shipped YAML spec with relative agent references. `/home/kkk/Apps/openrig-breakdown/packages/cli/src/commands/specs.ts:230–251` implements library lookup and prints the source type and filesystem path (or JSON metadata).
- **Observed behavior trace:** `/home/kkk/Apps/openrig-breakdown/packages/cli/src/commands/up.ts:68–86,215–222` accepts YAML paths, distinguishes them from library names and resolves local paths absolutely. `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/routes/up.ts:202–237` classifies the source and resolves file-based inputs. `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/bootstrap-orchestrator.ts:212–231` reads/parses the supplied file and delegates pod-aware YAML to instantiation. Library registration is not the only entry path.
- **Stated intent:** `/home/kkk/Apps/openrig-breakdown/docs/reference/getting-started.md:278–285,312–314` instructs users to copy specs into their repository and run the edited local YAML. It warns to retain the surrounding layout because copying only the rig file breaks relative agent/culture references.
- **Stated discovery surface:** `/home/kkk/Apps/openrig-breakdown/docs/reference/getting-started.md:31–39` describes local source reading in the TUI, including provenance and unavailable/denied sources; this is documentation, not a demonstrated UI session.

## Notes

- **Scope:** shipped defaults need not start as project-local files, and customization must preserve dependency paths. Editable YAML does not mean every spec is self-contained or arbitrary input passes validation. Remote paths must be interpreted in their own host context; this review establishes the local path.
- **Contradiction:** Gemini's “rather than ... editable configuration files” conflicts with both the explicit direct-file parser and the current copy/edit/launch instructions. “Internal library” does not mean opaque database-only storage.
- **Inference / evidence still needed:** actual discoverability burden requires observing a new user find and edit a spec. Local launch success for a particular copied layout/native environment remains untested; no evidence here quantifies obscurity.
- **Runtime-unverified:** no spec was copied, edited outside this claim, registered or launched; CLI/TUI behavior was read statically.

