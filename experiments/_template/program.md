# Experiment program template

Status: draft; no experiment runner or validation tool is implemented.

Copy this directory to a named experiment under `experiments/`. Complete the
[experiment definition](experiment.json) and the sections below using the
[research policy](../../docs/guides/autonomous-research.md). The template is not
permission to run an unbounded campaign.

## Question and falsification

REQUIRED: State one question, the alternative explanations, and the observation
that would reject the proposed mechanism. Identify the parent campaign or decision.

## Scope and comparison

REQUIRED: Name the editable files or parameters and explain the allowed changes.
Record frozen artifacts and hashes in the experiment definition: semantics,
grammar, reference, evaluator, workload splits, numerical and cost rules.
Describe how the controller enforces scope independently of the candidate.

## Baseline and candidate families

REQUIRED: Identify the accepted baseline, alternative starting points, useful
behavior descriptors, and simple search controls. Specify any bounded branch
allowed to regress temporarily, its milestone, and its exit condition.

## Procedure and measurements

REQUIRED: Supply actual setup, build, conformance, screening, and replay commands
once those tools exist. State session boundaries, quality/coverage requirements,
missing-result rules, practical margins, repetitions, and uncertainty handling.
Declare final confirmation access and the treatment of exposed confirmation cases.

## Worktree and artifact lifecycle

REQUIRED: Identify the control checkout, committed baseline or recorded source
snapshot, branch/worktree naming, and trial source-retention method. Put results
outside candidate worktrees. Specify external inputs, mutable cache isolation,
resource scheduling, interruption handling, integration checks, and cleanup.

## Budget, progress, and pivots

REQUIRED: Fill all resource ceilings and policy references in the definition.
Specify what counts as meaningful progress, how lineage history survives restarts,
and which pivots are permitted under the parent allocation. Record any departure
from the guide's pilot defaults before the campaign begins.

## Outcomes and interpretation

REQUIRED: Define promotion gates and archive categories. Specify candidate and
result retention, including bulky artifacts, and how to reproduce comparisons.
Keep trial history in immutable run records. Rewrite this program when its current
procedure changes; preserve the version used by prior campaigns.
