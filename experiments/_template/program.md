# Experiment program template

Status: draft specification.

Copy this directory to a named experiment under `experiments/`. Complete
[experiment.json](experiment.json) and the prompts below under the
[research policy](../../docs/guides/autonomous-research.md).
Record identifiers, hashes, limits, and policy references in JSON; use this file
for explanations and commands. Link shared rules rather than copying them.

## Question and falsification

REQUIRED: State the question, competing explanations, falsifying observation,
and parent campaign or decision.

## Scope and comparison

REQUIRED: Explain the allowed changes and how the controller enforces the scope
recorded in JSON. Identify any representation or evaluation change requiring a
separate campaign.

## Baseline and candidate families

REQUIRED: Explain the baseline, alternative starting points, behavior descriptors,
simple controls, and any temporary-regression hypothesis and milestone.

## Procedure and measurements

REQUIRED: Supply actual setup, build, check, run, and replay commands. Instantiate
the [measurement requirements](../../docs/whitepapers/Reprogrammable_Blob_Research_Plan.md#11-measurement-and-acceptance-procedure)
with task-specific thresholds and confirmation cases.

## Worktree and artifact lifecycle

REQUIRED: Instantiate the [worktree procedure](../../docs/guides/autonomous-research.md#worktrees-and-candidate-identity)
with campaign-specific locations, scheduling, and input/cache arrangements.

## Budget, progress, and pivots

REQUIRED: Justify limits and policy choices in JSON, define meaningful progress,
and explain permitted pivots or departures from the guide's defaults.

## Outcomes and interpretation

REQUIRED: Define task-specific promotion and archive decisions, how to reproduce
the comparison, and any retention exceptions under the
[record policy](../../docs/guides/autonomous-research.md#records-and-retention).
