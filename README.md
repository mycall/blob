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

## First implementation target

Start with the CPU slice of the
[streaming-filter experiment](docs/whitepapers/Reprogrammable_Blob_Research_Plan.md#first-experiment):
an independent reference, a CPU backend, and replayable live edits.

Language choices and build tooling remain open. Add executable setup and test
instructions here when the first implementation lands.

## License

[MIT](LICENSE).
