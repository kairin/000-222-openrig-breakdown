# F3: Jargon such as rig, pod, seat and continuity policy replaces familiar terms like repo, branch and task.

**Verdict:** part — rig/pod/seat and continuity terminology are real; replacement of repositories, branches and tasks and a measured learning cost are not established.

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> OpenRig discards conventional developer abstractions—such as repositories, branches, tasks, and scripts—in favor of metaphors drawn from telecommunications and distributed enterprise management7.

> Topologies require users to understand Rigs, Pods, Seats, and continuity policies7.

> Rather than issuing an instruction to a designated task runner, an operator must route instructions to an abstract address7.

## What the infographic says

Verbatim from `sources/agent_fleet_architectures_infographic.html`.

> Bureaucratic & Telecommunications Jargon

> Instead of standard software engineering concepts (repos, branches, PRs, task cards), OpenRig forces you to master enterprise-telecom abstractions:

> Very High (Telco Lexicon)

> Very High (Telecom/Seats)

## Sources Gemini cited

- [7] [GitHub - mvschwarz/openrig: Multi-agent harness that runs Claude](https://github.com/mvschwarz/openrig)

## How to check

List every domain noun used in the CLI and docs (rig, pod, seat, lore, mission, refocus, wave, ...). For each, is there a plain word that would do?

## Evidence from this repo

Reviewed at source commit `9db3ed6c406be5c3d9a84720383fcf6b543169e6` (2026-09-27).

- **Stated intent:** `/home/kkk/Apps/openrig-breakdown/docs/reference/rig-spec.md:173–183,246–258,299–307` defines rig names, pods, optional continuity policy, member IDs and derived addresses. These describe topology and recovery, not Git branches.
- **Observed in source:** `/home/kkk/Apps/openrig-breakdown/packages/daemon/specs/rigs/launch/first-project/rig.yaml:2–25` concretely declares a rig, pod and two members with per-member runtime/cwd. `/home/kkk/Apps/openrig-breakdown/packages/daemon/src/domain/workspace/workspace-resolver.ts:26–50` still resolves named repositories and an active repository; ordinary repository concepts coexist with topology.
- **Stated intent:** `/home/kkk/Apps/openrig-breakdown/docs/reference/getting-started.md:101–125` uses both a seat address and a durable task/queue in a repository. Lines 225–235 distinguish work definition from workflow execution rather than replacing all work concepts with a seat.
- **Stated derivative goal, not a measured result:** `/home/kkk/Apps/openrig-breakdown/README.md:25–26` asks for familiar vocabulary; it is not evidence that a proposed renaming exists or that Gemini's learning-cost scores are valid.

## Notes

- **Scope:** claim-bearing vocabulary and its semantic roles, not an exhaustive glossary. A rig, repository, branch and task encode different things; no one-to-one substitution was established. Continuity policy is documented as optional, not a universally required authored field.
- **Contradiction:** “discards” repositories/tasks overstates the evidence: current workspace code and task instructions retain them. The claimed telecommunications origin and “very high” burden remain unsupported by these sources.
- **Inference / evidence still needed:** a complete vocabulary inventory and observed comprehension tasks would be needed to quantify unfamiliarity. No terminology replacement or simplification is proposed here.
- **Runtime-unverified:** only source/docs were inspected; no user study or runtime workflow was performed.

