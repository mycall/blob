# Repository architecture

## Current state

This is a research monorepo scaffold. The
[research plan](whitepapers/Reprogrammable_Blob_Research_Plan.md) describes proposed
capabilities and experiments; no evaluators, runtimes, backends, or applications
are implemented yet. The boundaries below guide their placement as they are built.

## Representation baseline

Use explicit, versioned semantic state and recorded transitions as the initial
authoritative representation. English, code, and visual interfaces express intent
and present views; learned workspaces remain replaceable proposal and analysis
components. This is a provisional design choice, not implemented behavior.

The [representation decision](decisions/0001-semantic-state-and-projections.md)
owns the rationale, command/event distinction, projection requirements, replay
boundary, and reversal criteria. Its
[comparison experiment](../experiments/representation-comparison/README.md)
keeps English-centered and learned alternatives open to testing.

Place semantic and transition rules in `contracts/semantics/`, interchange formats
in `contracts/schemas/`, and reference interpretations in `components/reference/`.
The runtime owns validated changes, publication, and recovery. Applications own
authoring and presentation; learned proposal adapters belong with discovery.
The laboratory owns evidence and comparison. These are responsibilities within
the existing layout, not a requirement for new services or a graph database.

## Directory ownership

| Path | Responsibility |
| --- | --- |
| `docs/whitepapers/` | Research proposals and substantial research papers, with explicit scope and status. |
| `docs/architecture.md` | Current repository organization and dependency rules. |
| `docs/decisions/` | Decisions, alternatives, supporting evidence, and reversal criteria. |
| `docs/guides/` | Procedures for development and laboratory operation. |
| `contracts/semantics/` | Versioned behavioral specifications, including numerical and live-change rules. |
| `contracts/schemas/` | Interchange formats for candidates, edits, state, events, and results. |
| `contracts/conformance/` | Small, reviewed examples with independently justified expected behavior. |
| `components/reference/` | Independent deterministic interpretation of declared semantics. |
| `components/runtime/` | Live definitions, safe points, migration, publication, and recovery. |
| `components/backends/` | Replaceable execution implementations and their adapters. |
| `components/discovery/` | Candidate generation, search strategies, and optional oracle adapters. |
| `components/lab/` | Trial orchestration, replay, evidence collection, comparison, and analysis. |
| `apps/cli/` | Command-line entry points that compose components. |
| `apps/inspector/` | Future live inspection and editing interface. |
| `corpus/` | Reusable versioned workloads, edit traces, and minimized counterexamples. |
| `experiments/` | Experiment-specific hypotheses, configurations, and provisional code. |
| `results/` | Small run manifests, artifact references, and curated reports. |
| `external/` | Tracked source manifests and ignored external payloads; see [external assets](../external/README.md). |
| `infra/containers/` | Dependency packaging for independently selectable laboratory lanes. |
| `infra/platform/` | Host inventory and platform admission probes. |
| `tools/` | Small repository development and build helpers. |
| `.artifacts/` | Ignored local output: large traces, binaries, caches, and raw runs. |

Follow the [autonomous research policy](guides/autonomous-research.md) for campaign
scope, portfolio budgets, stagnation rules, and worktree lifecycle. The
[experiment template](../experiments/_template/program.md) provides a draft program
and definition; neither is an implemented runner interface. Candidate worktrees
live under `.artifacts/worktrees/`, with run payloads outside those worktrees and
durable evidence retained independently of their cleanup.

Empty directories use `.gitkeep` so Git preserves the intended layout. Remove a
directory's placeholder when real contents are added. Add subdirectories when an
implementation needs them rather than reserving every possible research branch.

## Dependency boundaries

Contracts are the shared authority for behavior and interchange. They must not
depend on an implementation, application, or experiment.

- The reference evaluator may share schema and serialization utilities, but must
  independently implement the behavior used to validate optimized candidates.
  Do not share optimized execution, scheduling, or migration algorithms with it.
- The runtime owns live-change transactions and consumes backend interfaces.
  Concrete backends implement those interfaces; the runtime must not require a
  particular backend or import its internals.
- Discovery proposes candidates. It must not control the acceptance rules of the
  experiment judging those candidates. Ordinary execution and recorded replay
  must work without a search engine or model service.
- The laboratory composes evaluators, runtime, backends, and discovery for trials.
  Runtime and backend implementations must not depend on laboratory orchestration.
- Applications compose component APIs. Components must not import application or
  experiment code.
- Experiment code can depend on components. Promote reusable behavior into a
  component only with a documented interface and appropriate validation.

Keep source, unit tests, and package manifests with their component. Put shared
conformance cases in `contracts/conformance/` and reusable workloads in
`corpus/`. Avoid a general-purpose `common` package; give shared code a specific
responsibility and explicit consumers.

A directory is a responsibility boundary, not a requirement for a separate
process, service, package, or language. Split those only when needed.

## Contracts and evidence

Version behavioral semantics separately from schema versions and backend APIs.
Reject unsupported versions explicitly. A semantic change starts a new comparison
family; it must not silently change the meaning of an existing experiment.

Keep reference correctness separate from performance baselines. Also distinguish
CUDA used to execute a workload from CUDA used to evaluate a hypothetical machine.
Actual backend performance, evaluator wall time, and modeled target costs require
separate evidence fields even when they use the same hardware.

An experiment definition belongs in `experiments/`; reusable inputs belong in
`corpus/`; observations and their provenance belong in `results/`. Each run
should identify its contracts, source, candidates, inputs, environment, numerical
rules, random streams, and referenced artifacts. Preserve a source snapshot or
patch for dirty-tree runs; a commit identifier alone cannot reproduce them.

Keep small fixtures and manifests in Git. Store large immutable artifacts outside
Git and identify them by content hash and a documented retrieval location.
`.artifacts/` is local scratch storage, not a durable archive. Before relying on
a result, preserve its required artifacts in durable storage and record how to
retrieve and verify them. The storage provider and manifest schema remain open.

Use the [external asset conventions](../external/README.md) for downloaded datasets,
prebuilt tools, and external source builds. Run manifests should reference their
source manifests and verified hashes, not only machine-local paths.

Decision records should identify the question, alternatives, current choice,
evidence, assumptions, and the experiment that would reverse the choice.
Maintain the current explanation here or in the relevant guide; preserve
superseded decisions and immutable experimental evidence through explicit links.

## Build and validation conventions

Choose native build tools when the first implementation needs them. Expose a thin,
documented root command interface for checking, testing, running experiments, and
replaying results once those operations exist. No commands are implemented yet.

CPU development and contract validation must work without CUDA, model weights, or
hardware simulation tools. Keep optional toolchains and container lanes separately
selectable. Pin dependencies and record the environment used for measurements.

As implementations arrive, add fast contract and CPU replay checks to routine CI,
cross-component conformance checks for contract changes, and explicit GPU checks
on an admitted host. Report skipped checks separately from successful checks.

Develop the first streaming-filter experiment before adding further infrastructure.
Use its observed needs to refine these boundaries.
