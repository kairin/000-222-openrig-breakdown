# A4: Topology is declared in YAML as Rigs, Pods and Seats.

**Verdict:** yes — the pod-aware topology is declarative YAML; it is not the only accepted topology format.

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> OpenRig defines multi-agent systems declaratively through YAML specifications7.

> System topologies are structured into Rigs (the overarching project team), Pods (bounded context groups sharing common guidance and domain knowledge), and Seats (stable, addressable network roles, such as an engineering lead or quality assurance reviewer)7.

## What the infographic says

Verbatim from `sources/agent_fleet_architectures_infographic.html`.

> RIG / Team project scope / POD / Bounded context group / SEAT / Addressable durable role

## Sources Gemini cited

- [7] [GitHub - mvschwarz/openrig: Multi-agent harness that runs Claude](https://github.com/mvschwarz/openrig)

## How to check

Read `docs/reference/rig-spec.md` and the spec parser in the daemon. Are pods and seats both required, or can a rig be a flat list of agents?

## Evidence from this repo

Reviewed against source baseline `9db3ed6c406be5c3d9a84720383fcf6b543169e6`.

- **Source observation:** The codec parses YAML; the import route selects pod-aware instantiation when `pods` is an array: `/home/kkk/.cline/worktrees/36ea1/openrig-breakdown/packages/daemon/src/domain/rigspec-codec.ts:90–103`; `/home/kkk/.cline/worktrees/36ea1/openrig-breakdown/packages/daemon/src/routes/rigspec.ts:68–102`.
- **Source observation:** Pod-aware validation requires a nonempty pods array and member arrays. Instantiation creates pods and qualified `pod.member` identifiers: `/home/kkk/.cline/worktrees/36ea1/openrig-breakdown/packages/daemon/src/domain/rigspec-schema.ts:181–202,400–408`; `/home/kkk/.cline/worktrees/36ea1/openrig-breakdown/packages/daemon/src/domain/rigspec-instantiator.ts:1324–1345`. The YAML key is `members`, not a literal `seats` key.
- **Stated intent:** Rig/pod startup guidance is applied to members; pods are bounded contexts and edges address `pod.member`: `/home/kkk/.cline/worktrees/36ea1/openrig-breakdown/docs/reference/rig-spec.md:176–183,245–251`.
- **Source observation:** Legacy flat-node validation remains, and the import route normalizes/instantiates it rather than always demanding pods: `/home/kkk/.cline/worktrees/36ea1/openrig-breakdown/packages/daemon/src/domain/rigspec-schema.ts:1177–1185,1197–1215`; `/home/kkk/.cline/worktrees/36ea1/openrig-breakdown/packages/daemon/src/routes/rigspec.ts:105–114`.

## Notes

**Inference:** The headline accurately describes the current pod-aware model, not an exclusive configuration mechanism. Addressable member identifiers support the role/topology description; stability after process termination is A5's separate question and is not adjudicated here.

**Runtime-unverified / limitations:** No YAML was instantiated. Documentation says at least one member is required per pod, but the cited member validator loops over the array without a nonempty-length check. That is a source/documentation discrepancy, not proof that every downstream path accepts an empty pod. A targeted validation/preflight/instantiation test would resolve the end-to-end empty-pod behavior. Team “bounded context” semantics are stated organizational intent, not machine-enforced knowledge boundaries.

