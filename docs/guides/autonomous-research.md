# Autonomous research campaigns

## Status and scope

This guide owns campaign operations and pilot defaults. See the
[README](../../README.md) for implementation status and the
[experiment template](../../experiments/_template/program.md) for draft inputs.
A campaign freezes an instantiated copy of the applicable settings.

Research hypotheses and measurement methodology live in the
[whitepaper](../whitepapers/Reprogrammable_Blob_Research_Plan.md).
External input manifests follow [external assets](../../external/README.md).

## Bound the comparison, preserve freedom to explore

A campaign is a bounded comparison under one declared evaluation contract. Its
scope includes permitted edits, semantic and schema versions, candidate grammar,
reference and evaluator identities, workload splits, numerical rules, acceptance
criteria, and the resource envelope.

Freeze those inputs before establishing the baseline. Implementations may change
inside the declared scope. Changes to semantics, evaluation, workloads, grammar,
or behavior descriptors require a separately identified campaign with new baselines
and an explicit relationship to its parent. An evaluator bug invalidates affected
claims; preserve the observations, record the correction, and rerun comparisons.
Never silently reinterpret old scores under new rules.

The laboratory must enforce file scope, verify frozen identities, and collect
measurements independently of candidate-reported scores. Use read-only evaluator
inputs and isolated execution where needed; a prompt or a Git worktree alone does
not enforce those boundaries.

Promotion must pass the [measurement and acceptance requirements](../whitepapers/Reprogrammable_Blob_Research_Plan.md#11-measurement-and-acceptance-procedure).
Exploration may retain failed candidates or temporary regressions without replacing
the accepted baseline.

## Campaign definition

Copy `experiments/_template/` into a named experiment directory and complete both
files. Keep responsibilities distinct:

- `program.md` explains the question, falsification condition, permitted mechanisms,
  and interpretation of outcomes. Maintain it as a coherent current procedure.
- `experiment.json` records machine-readable scope, artifact identities, budgets,
  and references to policies. It is currently a draft format without a loader.
- Run records preserve the exact hashes or snapshots of both files used for a run.
  Later edits must not change the instructions associated with recorded evidence.

Replace every required null or placeholder before starting. Empty lists must be
explicitly justified when inapplicable. A zero GPU or model-service budget means
that resource is unavailable, not unlimited. Budgets and required policies cannot
be inferred from missing values.

Measure the unchanged baseline first and predeclare the experiment-specific
thresholds required by the acceptance methodology. Prefer simpler candidates
when outcomes are equivalent within those thresholds.

## Portfolio and stagnation defaults

Start with the following cost allocation across a research portfolio:

| Track | Pilot share | Purpose |
| --- | --- | --- |
| Refinement | 60% | Improve established candidates within a fixed comparison. |
| Alternatives | 25% | Explore different mechanisms, archived starting points, and simple controls. |
| Architecture discovery | 15% | Test new primitives, representations, or semantic hypotheses in separate campaigns. |

These are adjustable experimental defaults, not validated optimal ratios. Freeze
the chosen allocation and its accounting unit before a portfolio begins. Count
proposal generation, failed builds, checking, screening, and confirmation.
Track wall time, CPU/GPU time, and monetary cost separately; do not add incompatible
units. The designated cost measure controls allocation, with all resource ceilings
enforced independently. Aggregate small campaigns over the portfolio allocation
window instead of demanding fractional trials from each campaign.

Protect the exploration allocation even when refinement keeps producing small
wins. Record unused allocations and reasons; do not silently divert them to the
current winner. A new campaign or branch name does not reset a lineage's cost or
stagnation history.

Pilot review interval: ten attempted trials per lineage, or its declared elapsed
review interval, whichever comes first. Two consecutive reviews without meaningful
progress require a pivot or retirement of that lineage. Failed and cancelled
attempts count. A campaign's hard limits always take precedence over review timing.

Meaningful progress is evidence of a preregistered practical gain beyond noise, a
useful new behavioral niche, a reproducible counterexample, or resolution of a
named uncertainty. Syntactic novelty, repeated failures, and new explanations of
the same observation do not reset stagnation.

At a pivot, record the evidence and choose a concrete action: restart from an
independent candidate, transfer a mechanism from another lineage, test a different
representation, challenge a workload assumption, or propose a new grammar or
evaluation campaign. Renewing the same search requires an explicit allocation and
new rationale under the remaining parent budget. A preauthorized allocation policy
may make that decision automatically; the candidate cannot enlarge its own budget.

## Preserve alternatives and stepping stones

Maintain distinct archive roles: accepted choices, valid alternatives,
unvalidated stepping stones, and known failures. Record behavior descriptors
and lineage parents so independent restarts and transfers can be evaluated.
Use the [diversity rationale](../whitepapers/Reprogrammable_Blob_Research_Plan.md#22-wisdom-graphs-search-diversity-and-new-architectures)
to select descriptors; the campaign definition freezes them.

A temporary-regression branch needs a hypothesis, an intermediate milestone,
a cost ceiling, and an exit condition. For example, explicit state handles may
initially slow execution but enable cheaper migration. Preserve the stepping
stone and test that predicted benefit before extending its budget.

An invalid candidate cannot be promoted without independent conformance checks.

## Worktrees and candidate identity

Use one Git worktree per active experimental lineage when editing source.
Parameter-only trials can use immutable configurations against the same source
snapshot. Worktrees separate working files and branches; they share Git objects
and references, host resources, and potentially external caches. They are neither
security sandboxes nor independent GPU allocations.

Create each worktree from an explicit baseline commit. Uncommitted and untracked
files in the main checkout are not included automatically. First establish the
intended committed baseline, or deliberately materialize a recorded source snapshot
that includes the dirty patch and required untracked files. Never mix in unrelated
user changes or reset the user's checkout to run an experiment.

Example layout, relative to the control checkout:

```text
.artifacts/worktrees/<campaign>/<lineage>/   disposable candidate working files
.artifacts/runs/<campaign>/<trial>/         local logs and payloads, outside worktrees
results/<campaign>/<trial>/                 small durable manifests
```

A proposed setup sequence, after the intended baseline has been committed:

```bash
blob_repo=$(git rev-parse --show-toplevel)
blob_baseline=$(git rev-parse HEAD)
mkdir -p "$blob_repo/.artifacts/worktrees/E001"
git worktree add -b research/E001/handles \
  "$blob_repo/.artifacts/worktrees/E001/handles" "$blob_baseline"
```

The identifiers above are illustrative; use fresh campaign and lineage names.
Record the baseline, branch, worktree location, candidate parents, and actual
source identity in each trial. Separate source identity from physical location.

Before evaluating a candidate, preserve it as a commit or a content-addressed
source snapshot. Retain a durable Git reference or archived bundle for commits
that would otherwise become unreachable; a hash in a table does not keep an
abandoned commit alive. Rejected and crashed candidates need identities too.
Do not rely on reflogs as the experiment archive.

The control checkout or dedicated controller owns the frozen evaluator, budget
ledger, and results index. Candidate processes must not rewrite them. Share
external inputs through explicit absolute paths or read-only mounts and verify
hashes. Keep mutable build trees, environments, caches, and output paths separate
when their contents could affect another trial. Schedule access to the GPU and
record contention; adding worktrees does not justify concurrent timing trials.

Promote a validated candidate through a reviewed patch or commit into the accepted
implementation and rerun applicable checks in that integration context. Retire a
worktree only after preserving its source, manifests, and required artifacts.
Inspect dirty and untracked files first, then use `git worktree remove`; do not
force removal or delete branches/tags containing the only retained candidate.
An interrupted trial gets an interrupted outcome and preserved evidence before
its worktree is reused or removed.

## Trial loop and budget accounting

1. Freeze campaign artifacts, establish the baseline, and verify environment and
   input identities. Reserve resources for recording outcomes and final checks.
2. Select a candidate from the allocated track and record its hypothesis and parents.
3. Build and run cheap conformance checks. Stop invalid candidates early while
   preserving the failure and cost.
4. Screen eligible candidates. Short trials guide ranking only after their ranking
   reliability has been checked against complete sessions.
5. Repeat promising comparisons under controlled conditions, then apply independent
   confirmation gates. Keep rejected and inconclusive outcomes.
6. Update archive membership and the cost ledger; apply stagnation and hard limits
   before allocating the next trial.

Declare trial count, per-trial and campaign elapsed-time limits, CPU/GPU budgets,
peak memory, storage, and any model/service spending. Hard ceilings include failed,
cancelled, and interrupted work; retries consume budget. Stop admission early enough
to cancel or drain workers and persist evidence within the resource envelope.
A restart resumes the ledger rather than resetting it. If the remaining budget
cannot support confirmation, report a promising but unconfirmed candidate.

Apply the whitepaper's [session-cost methodology](../whitepapers/Reprogrammable_Blob_Research_Plan.md#compare-both-quantity-and-quality)
when measuring application behavior; keep it distinct from the campaign's
research budget.

## Confirmation and search evaluation

Use development cases for rapid feedback, validation cases for repeated selection,
and a final confirmation suite withheld from routine candidate generation.
Use the whitepaper's [measurement procedure](../whitepapers/Reprogrammable_Blob_Research_Plan.md#run-procedure)
for repetitions, ordering, and transfer checks.

Record access to confirmation results. Once they influence subsequent proposals,
treat those cases as development evidence for that search and establish a new
untouched confirmation suite before making another generalization claim.
Generated workloads supplement independent cases; they cannot replace the user
requirements or redefine success to favor the current candidate.

At portfolio reviews compare discovery methods with random search, simple local
search, and selected manual alternatives under matched total budgets. Report
validated gains, useful transfers, counterexamples, coverage, and uncertainty
reduced, together with costs. These are separate outcomes, not an unrestricted
self-awarded scalar score. No archive or search strategy guarantees global discovery.

## Records and retention

Store small immutable trial manifests under `results/<campaign>/<trial>/`, or
keep durable external records with tracked indexes. Record outcome separately
from the decision to promote, retain, or retire a candidate. Missing metrics
are null with a reason, not zero-valued performance.

### Minimum run record

This proposed record lists required provenance and result dimensions; it is not
an implemented schema. Reference external input manifests rather than duplicating
their retrieval and build instructions.

```yaml
run_id: E003-seed17-r2
campaign_id: E003
campaign_definition_hash: REQUIRED
program_hash: REQUIRED
lineage_id: REQUIRED
parent_candidate_ids: REQUIRED_OR_EMPTY_FOR_BASELINE
question: Does edit-aware specialization beat hotness-only tiering?
contract: stream_v1
source_commit: REQUIRED
source_snapshot_hash: REQUIRED_IF_DIRTY_OR_UNTRACKED_INPUTS
source_retention: REQUIRED_DURABLE_REF_OR_ARCHIVE
workspace_record: REQUIRED_FOR_SOURCE_EDITING_LINEAGE
container_digest: REQUIRED
environment_manifest: env-001.json
trace_hash: REQUIRED
candidate_hash: REQUIRED
evidence_kind: local_measurement
evaluator_hash: REQUIRED
reference_hash: REQUIRED
cost_model_hash: REQUIRED_IF_MODELED
logical_trial_ids: trials.json
random_stream_or_proposal_manifest: REQUIRED_OR_NOT_APPLICABLE
numerical_contract: REQUIRED
objective_manifest: objectives.json
comparison_mode: quantity_at_fixed_quality
work_unit: accepted_stream_sample
quality_contract_hash: REQUIRED
editing_contract_hash: REQUIRED
coverage_and_missing_result_policy: REQUIRED
assurance_policy_hash: REQUIRED
proof_manifest: OPTIONAL_IF_NO_PROOFS_REQUIRED
proof_ecosystem: lean_or_rocq_or_other_or_none
timing_scope: end_to_end_including_edits
budget_ledger: REQUIRED_CAMPAIGN_AND_PARENT_ALLOCATION
trial_outcome: REQUIRED_COMPLETED_INVALID_CRASHED_CANCELLED_INTERRUPTED_OR_INCONCLUSIVE
archive_decision: REQUIRED_PROMOTE_RETAIN_OR_RETIRE_WITH_REASON
confirmation_access_record: REQUIRED_OR_NOT_APPLICABLE
artifact_manifest: REQUIRED_HASHES_LOCATIONS_AND_RETENTION
results:
  quantity: quantity.json
  quality: quality.json
  editing_experience: experience.json
  assurance: assurance.json
  costs: costs.json
  events: events.jsonl
```

### Retention

Keep sufficient evidence for replay and diagnosis. Declare retention tiers and a
storage ceiling before running; preserve candidate identity and outcome even when
discarding bulky intermediate traces. Retain full diagnostic artifacts for required
confirmation and important counterexamples. Record any deletion that limits replay.
Local ignored directories alone are not durable storage.

Follow [AGENTS.md](../../AGENTS.md#documentation) when updating current conclusions.
Record the evidence and reversal test in the relevant decision; keep trial logs
in the archive.

## First pilot

Use the whitepaper's [streaming-filter pilot](../whitepapers/Reprogrammable_Blob_Research_Plan.md#first-experiment)
to exercise this procedure before broadening source-editing scope.

## Research basis

The whitepaper discusses [autonomous-loop precedents](../whitepapers/Reprogrammable_Blob_Research_Plan.md#bounded-autonomous-campaigns)
and [diversity mechanisms](../whitepapers/Reprogrammable_Blob_Research_Plan.md#separate-correctness-utility-and-novelty).
The allocation shares, review intervals, and worktree workflow here are provisional
Blob operating choices, not validated performance findings.
