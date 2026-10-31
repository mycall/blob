# Blob - Build, Learn, Observe, Become: An open-ended research and continual, deterministic transformation

An experimental system for changing live computations while preserving inspectable
state, replayable behavior, and evidence about execution costs.

The repository currently contains a research proposal and a monorepo scaffold.
There is no executable runtime, build system, or test suite yet. Component folders
reserve responsibilities; they do not indicate implemented capabilities.

## Start here

- [Research plan](docs/whitepapers/Reprogrammable_Blob_Research_Plan.md): hypotheses,
  proposed experiments, and decision gates.
- [Repository architecture](docs/architecture.md): directory ownership, dependency
  boundaries, and conventions for implementation and evidence.
- [Representation decision](docs/decisions/0001-semantic-state-and-projections.md):
  provisional semantic-state baseline, English projections, and learned workspaces.
- [Working rules](AGENTS.md): instructions for contributors and coding agents.
- [Autonomous research policy](docs/guides/autonomous-research.md): bounded campaigns,
  exploration, worktrees, validation, and evidence retention.
- [Experiment template](experiments/_template/program.md): draft campaign procedure
  and machine-readable definition; no runner is implemented yet.
- [External assets](external/README.md): conventions for large datasets, downloads,
  and binaries kept outside Git.

## First implementation target

Build a replayable streaming-filter experiment with an independent reference
evaluator and a CPU execution backend. Exercise a parameter edit, an operation
replacement, a state migration, and recovery from a failed migration. Preserve
the inputs, logical outputs, and run provenance before adding further backends
or search strategies.

Language choices and build tooling remain open. Add executable setup and test
instructions here when the first implementation lands.

## License

[MIT](LICENSE).
