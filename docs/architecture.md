# Repository architecture

This document owns directory responsibilities, dependency boundaries, and build
conventions. See the [README](../README.md) for implementation status.

## Documentation ownership

Edit the owner when a rule or decision changes; other documents should summarize
only what their readers need and link to the details. Templates instantiate rules
for a campaign rather than defining competing policy.

| Concern | Authoritative home |
| --- | --- |
| Project status and entry points | [README](../README.md) |
| Contributor rules and documentation upkeep | [AGENTS.md](../AGENTS.md) |
| Repository layout, dependencies, and build conventions | This document |
| Research questions, precedents, and staged milestones | [Whitepaper](whitepapers/Reprogrammable_Blob_Research_Plan.md) |
| Measurement dimensions and acceptance methodology | [Whitepaper §11](whitepapers/Reprogrammable_Blob_Research_Plan.md#11-measurement-and-acceptance-procedure) |
| Representation choice, projection rules, and reversal criteria | [ADR 0001](decisions/0001-semantic-state-and-projections.md) |
| Campaign budgets, trial/worktree lifecycle, confirmation access, and run records | [Research policy](guides/autonomous-research.md) |
| External input provenance, retrieval, and storage | [External assets](../external/README.md) |
| A specific experiment's question, alternatives, and procedure | Its definition under `experiments/`; start with the [template](../experiments/_template/program.md) |
| Historical observations and artifact references | Immutable run records under `results/` or their indexed archive |

Decision records retain rationale and reversal criteria; experiment definitions
own the tests of those decisions. As executable contracts are implemented,
`contracts/` becomes the authority for their precise behavior and interchange
formats; guides and proposals should link to them.

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

Empty directories use `.gitkeep`; remove the placeholder when real contents arrive.
Add subdirectories when needed rather than reserving every possible research branch.

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

## Contract boundaries

Version behavioral semantics separately from schema versions and backend APIs.
Reject unsupported versions explicitly. Changes to comparison meaning follow the
[campaign rules](guides/autonomous-research.md#bound-the-comparison-preserve-freedom-to-explore).

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
