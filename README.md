# Blob - Build, Learn, Observe, Become: An open-ended research and continual, deterministic transformation

An experimental system for changing live computations while preserving inspectable
state, replayable behavior, and evidence about execution costs.

The repository currently contains a research proposal and a monorepo scaffold.
There is no executable runtime, build system, or test suite yet. Component folders
reserve responsibilities; they do not indicate implemented capabilities.

## Start here

- [Research plan](docs/whitepapers/Reprogrammable_Blob_Research_Plan.md): hypotheses,
  proposed experiments, and decision gates.
- [BLOB-R1 research roadmap](docs/research-roadmap.md): zero point, readiness,
  integrated first revision, bounded task list, and pause/resume requirements.
- [Repository architecture](docs/architecture.md): directory ownership, dependency
  boundaries, and documentation ownership.
- [Representation decision](docs/decisions/0001-semantic-state-and-projections.md):
  provisional semantic-state baseline, English projections, and learned workspaces.
- [Working rules](AGENTS.md): instructions for contributors and coding agents.
- [Autonomous research policy](docs/guides/autonomous-research.md): bounded campaigns,
  exploration, worktrees, validation, and evidence retention.
- [Experiment template](experiments/_template/program.md): draft campaign procedure
  and machine-readable definition.
- [External assets](external/README.md): conventions for large datasets, downloads,
  and binaries kept outside Git.

## Initial implementation programme

Follow the [BLOB-R1 roadmap](docs/research-roadmap.md): freeze the research zero
point, establish experiment readiness, then build an integrated first revision.
It connects the streaming-filter live-edit experiment with discovery experiments
C01–C03, using an independent reference and both CPU and CUDA execution. A bounded,
script-driven Verilator/cocotb lane checks the same filter semantics in RTL
simulation. CPU-only work is an intermediate milestone; completing the revision
requires both execution tracks, the RTL comparison, and reproducible evidence.

Language choices and build tooling remain open pending a bounded toolchain spike.
The roadmap defines pilot limits and pause/resume requirements for intermittent
work. Add executable setup and test instructions here when implementation lands.

## License

[MIT](LICENSE).
