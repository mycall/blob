# BLOB-R1: Research starting points and integrated first revision

Plan ID: `BLOB-R1` · Status: planned; implementation and validation not started.

This bounded plan owns the initial milestones and task list, plus a separately
budgeted formal-assurance follow-on after R1. The
[whitepaper](whitepapers/Reprogrammable_Blob_Research_Plan.md) owns research
hypotheses and the broader programme; the
[research policy](guides/autonomous-research.md) owns campaign operations.
Detailed methods belong in individual experiment definitions, instantiated from
the [template](../experiments/_template/program.md). Saving this plan does not
complete a milestone or establish experimental evidence.

## Purpose and milestones

Build a first revision that connects the whitepaper's live execution and discovery
proposals. CPU and CUDA are both required: an independent reference checks
conformance to the declared semantics, a conventional compiled CPU implementation
establishes a credible performance baseline, and CUDA tests interchangeable
execution and accelerated discovery. A small, script-driven RTL simulation lane
checks the same declared behavior in a hardware description without requiring a
physical FPGA.

| Milestone | Meaning | Exit evidence |
| --- | --- | --- |
| Z0 — Research zero point | Freeze what exists, what is assumed, and what remains untested. | Reconstructable repository snapshot, evidence inventory, questions, and acceptance methodology. |
| I0 — Ready to begin experiments | Establish the platform, toolchain, contracts, fixtures, and resumable laboratory. | Checked CPU/CUDA execution and RTL simulator admission, independent reference cases and deliberate fault detection, completed campaign definitions, and demonstrated pause/resume. |
| R1 — Integrated first revision | Exercise live editing, state migration, multiple backends, RTL simulation, discovery, inspection, and evidence collection together. | Reproducible demonstrations and comparison reports, including negative and inconclusive results. |

Z0 is a documentary baseline, not a claim of executable behavior. Measured
baselines are established after I0, before evaluating candidate improvements.

## Z0 and I0: establish the research foundation

### Freeze the zero point

- Record the source commit or reconstructable snapshot, whitepaper version,
  provisional representation decision, experiment outlines, and current
  implementation status.
- Separate existing evidence from reported observations and proposals. In
  particular, treat the reported CUDA environment as awaiting admission.
- Establish five initial questions:
  1. Can live changes preserve declared state and replay semantics?
  2. Can CPU and CUDA implementations agree under the same inputs and edits?
  3. When does specialization repay compilation, validation, and invalidation costs?
  4. Can bounded search produce useful validated findings within its budget?
  5. Can a bounded RTL implementation preserve the declared observable behavior
     and state transitions within the available host resources?
- Record what would falsify each claim and which R1 experiment addresses it. Keep
  the final language, machine architecture, and learned representation open.

### Admit the laboratory and choose implementation tools

Inventory the actual CPU, GPU, operating system, architecture, driver, compiler,
CUDA, and container environment. Admit native CPU execution, CUDA
calculation/readback, NVRTC compilation, and the profiling/checking features
actually used. Pin successful environments; retain failed probes.

Run a **16-active-hour toolchain spike**, comparing:

- C++ runtime and CUDA with Python reference/laboratory tools.
- Rust runtime with a CUDA native boundary and Python reference/laboratory tools.

Use the same miniature task: execute a stateful computation, export/import state,
apply a versioned change, call a CUDA implementation, and replay the result.

Both candidates must pass correctness and clean-build checks. Select the candidate
with fewer unresolved build, integration, and recovery problems; if tied, select
C++/CUDA to reduce the initial native boundary. Record the decision and reversal
criteria. If neither passes within the cap, close the spike as blocked and retain
its evidence.

### Freeze the first experimental contracts

Define a deliberately small, versioned graph/queue machine supporting the
streaming-filter workload:

- Bounded operations, buffers, queues, and execution steps.
- Exact integer arithmetic with explicitly defined overflow behavior.
- Stable identities, definition versions, ordered inputs, and versioned outputs.
- Parameter changes, compatible operation replacement, and history expansion.
- Explicit exhaustion, invalid-operation, stale-command, and migration-failure
  outcomes.
- One region per change transaction; effects go to a recorded mock sink.

Use a small integer signal pipeline with parameterized arithmetic and a
history-based filter. Expand history from 64 to 128 entries while preserving only
samples actually retained. Keep approximate floating-point behavior for a separate
comparison.

Implement an independent Python reference with hand-checked examples before
optimized evaluation. Shared serialization is allowed; shared execution or
migration algorithms are not.

For each core requirement, record its intended behavior, preconditions, observable
result, and a hand-checked example that would violate it. Keep these examples in
`contracts/conformance/`. Use a small fixed set of deliberately faulty transitions
or outcomes to check that the validation machinery rejects invented or reordered
history, a stale edit reported as successful, and publication without a recoverable
event. Require the valid counterparts to pass as well. I0 checks these fixtures;
R1 repeats them against the integrated transaction and recovery paths. Catching
these faults is evidence about the checks, not proof that the specification is
complete or captures every user need.

Freeze development, validation, and confirmation fixtures separately. Include
stable operation, gradual edits, bursts, repeated invalidation, and deliberate
failures. Freeze stateful trace-generation rules, seeds, size limits, and
minimization budgets with each campaign; keep confirmation inputs unavailable
to candidate selection under the research policy.

**I0 passes only when** the toolchain decision, required platform probes, reference
fixtures and deliberate fault checks, campaign manifests, and pause/resume smoke
test all pass. Hardware unavailability leaves I0 incomplete; CPU work can still
proceed.

## R1: connected execution, discovery, and RTL tracks

### Live execution and inspection

Build one running pipeline with four execution paths:

1. Generic CPU evaluator.
2. Conventional compiled CPU baseline.
3. Generic CUDA evaluator.
4. Specialized CUDA implementation produced through NVRTC.

The reference evaluator remains separate from these performance comparisons.

Implement preparation, validation, safe-point acquisition, snapshot migration,
publication, and retirement. Failed preparation or migration retains usable old
state. Interrupted publication must recover a consistent committed version and
history, following the [representation decision](decisions/0001-semantic-state-and-projections.md).

Provide a small local browser inspector showing:

- Input/output signals, current definitions, versions, and retained state.
- Controls for the three supported edit classes.
- Edit progress and actual outcomes.
- Execution backend, compilation activity, latency, and evidence links.

Bind admission evidence to the candidate identity, semantic contract version,
checker identity, and checked input domain. Show that scope in the inspector;
passing sampled traces and exhausting C03's finite domain are distinct claims.
Revalidate replacements when the candidate, contract, or relevant dependencies
change. Admission checks must pass before publication.

Inspector actions submit version-conditioned commands. Views cannot mutate
authoritative state directly. Record simple paired inspection/editing tasks and
human effort; automated correctness alone does not establish usability.

Add a bounded controller that can select validated implementations and fall back
to generic execution. Use a measured break-even rule for specialization; record
switches and their costs.

### CUDA discovery and bounded research

Implement the whitepaper's initial discovery comparisons:

| Experiment | Required exercise |
| --- | --- |
| C01 | Evaluate the same bounded machine on scalar CPU, parallel CPU, and CUDA thread/warp/block mappings. Check logical agreement and measure batch-size behavior. |
| C02 | Compare generic CUDA interpretation with NVRTC specialization across program sizes and edit rates, including full preparation and invalidation costs. |
| C03 | Enumerate a declared small, complete program/input domain on CPU and CUDA. Require identical counterexample sets and report the precise coverage boundary. |

Use the shared semantic contract and evidence formats, while keeping discovery
throughput separate from application throughput. A CUDA speedup or crossover is
a measured outcome, not a completion assumption.

Pilot search by varying CPU processing batch size under identical recorded edits.
Compare fixed, random, and simple local-search controls with equal total budgets.
Preserve proposals and lineage before evaluation; independently check every
candidate before promotion.

Implement a small evidence-linked decision graph connecting questions, candidates,
runs, counterexamples, decisions, and reversal conditions. A structured store and
inspector view are sufficient; a graph database is unnecessary.

### Script-driven RTL simulation

Use **Verilator with a Python/cocotb test harness** in a separately selectable,
pinned Linux container. Verilator compiles Verilog/SystemVerilog into executable
C++ or SystemC models; cocotb supplies scripted stimulus and checks. Upstream
documents WSL2 use and cocotb integration, but compatibility with the actual
host architecture, tool versions, and container stack must pass local admission.
See the [Verilator overview](https://verilator.org/guide/latest/overview.html),
[installation requirements](https://verilator.org/guide/latest/install.html), and
[cocotb simulator support](https://docs.cocotb.org/en/stable/simulator_support.html).

Build the simulator and generated executable for the measured container host ISA.
Do not assume an available image supports ARM64 or silently substitute x86
emulation. If no compatible image exists, build from pinned upstream sources in
a native base image under the same preparation budget. Run simulation on the CPU;
CUDA remains an independent execution/discovery lane. This needs no FPGA board,
vendor tool licence, nested VM, or GPU passthrough into the RTL container.

Admission must demonstrate a scripted clean build, reset/clock/stimulus, an
independently checked result, a bounded waveform capture, and cancellation of the
entire build/simulator process group. Pin compatible Verilator, cocotb, Python,
C++ compiler, and image versions together. An admission failure blocks the RTL
milestone; it does not silently remove RTL from R1 or stop independent CPU/CUDA
development.

Run one campaign, **RTL01**, implementing the small integer streaming filter from
the common contract in synthesizable RTL. Keep the circuit structure fixed:
parameters and supported operation selection are registers, and history has a
maximum physical capacity of 128 entries with an initial logical capacity of 64.
Exercise parameter changes, compatible operation selection, and logical history
expansion at declared sample boundaries. Preserve retained samples and prevent
inactive memory from becoming invented history. Record that logical expansion
inside preallocated storage does not demonstrate physical memory resizing or FPGA
partial reconfiguration.

The harness drives the same versioned inputs and supported edits as the reference,
CPU, and CUDA paths. Expose ready/valid handshakes, logical sample/version IDs,
and a quiescent state export/import interface; define their mapping to semantic
boundaries in the campaign contract before collecting results. Compare outputs,
retained history, edit outcomes, and logical checkpoints. Allow different cycle
latencies while requiring the same logical observations. Check reset, backpressure,
overflow rules, stale edits, and rejection of invalid state imports without
changing committed state. Use a bounded small-domain enumeration plus the shared
recorded traces; state the coverage explicitly.

Use the common laboratory launcher for build, run, compare, replay, and report.
Retain RTL and testbench hashes, tool/image identities, inputs, failures, cycle
counts, host time, memory, and bounded waveforms. Keep simulated cycles separate
from simulator throughput and CPU/CUDA application timings. The result establishes
functional behavior and cycle counts for this circuit, not achievable FPGA clock
speed, placement/routing, resource utilization, power, or physical reconfiguration.

Pause at test/trial boundaries. On interruption, record consumed budget and restart
the test from its frozen inputs. Simulator checkpoint restoration is outside R1;
logical state export/import remains independently testable. No simulator process
continues running while the campaign is paused.

### Interfaces and ownership

Introduce versioned interfaces for:

- **Commands and outcomes:** intent, target/base version, proposed change, actual
  result, and evidence.
- **Backends:** execute, inspect, export/import state, drain/cancel, report costs,
  and reject unsupported behavior.
- **Experiments:** immutable candidate and fixture identities, limits,
  observations, checkpoints, and promotion decisions.
- **RTL adapter:** clock/reset and ready/valid stimulus, sample/version mapping,
  quiescent state export/import, cycle observations, and trace comparison. Keep
  simulator-specific control in the adapter, outside the semantic reference.

Keep the reference, runtime, backends, discovery, laboratory, and applications
within the existing [architecture boundaries](architecture.md). Provide root
commands for setup checks, builds, tests, runs, replay, pause, resume, and reports.
These are planned interfaces, not existing commands.

## Intermittent work, budgets, and evidence

Use the selected small-pilot limits:

- Toolchain spike: **16 accumulated active hours**.
- Each initial campaign: **30 attempted trials and two accumulated active hours**,
  whichever is reached first.
- The initial allocation contains six campaigns: live execution, C01, C02, C03,
  batch-size search, and RTL01. Together these allow at most **180 attempted
  trials and 12 accumulated active campaign hours**. Restarts, renamed campaigns,
  and corrective reruns consume the remaining allocation; extensions require an
  explicit revised allocation. These are experiment limits, not an estimate of
  total implementation effort.
- Per-trial limit: **120 seconds**, including preparation and checking.
- Existing hardware only; no paid services or live model calls.
- Freeze host/device memory and storage ceilings from admission results before
  starting; missing limits block a run. Populate the other resource ceilings
  required by the campaign template as well.

For RTL01, default to one simulation worker and at most two build jobs. Cap the
container memory at the smaller of 4 GiB or 25% of admitted host RAM, CPU usage at
two logical cores (one on a single-core allocation), and campaign artifacts,
including writable build caches, at 10 GiB. Disable waveforms for timing runs;
allow at most 256 MiB of waveform output per diagnostic trial within that storage
ceiling. Verify these limits can be enforced by the admitted container/launcher;
stop on exhaustion and retain a resource-limit outcome. Do not increase them
automatically. Pin and account for the separately provisioned read-only tool image
in the admission storage manifest. Preparation and simulator builds consume the
campaign's two-hour allowance; candidate builds also count toward the per-trial
limit. Failures to fit become evidence for a revised scope or allocation.

Paused time does not consume active execution budgets. Record calendar elapsed
time separately. Builds, failed attempts, cancellation, recovery, and evidence
persistence consume active budget. This plan's campaign elapsed limit means
accumulated active elapsed time across resumed segments; each individual trial
still has an uninterrupted wall-time limit. Encode that distinction explicitly
in campaign definitions and the loader rather than silently reinterpreting an
existing field. CPU/GPU consumption and other hard ceilings remain independent.

Persist campaign status, immutable inputs, source identity, completed trials,
remaining budgets, proposal-generator state, artifact references, and the next
operation. On resume, verify identities and environment; never silently continue
a changed comparison.

Pause at trial boundaries when possible. Interrupting a trial preserves an
interrupted outcome and charges its costs. Timing trials restart as new attempts;
measurements from opposite sides of a pause are never stitched together. A
mid-session checkpoint may test functional recovery, but does not qualify as an
uninterrupted performance measurement. Paused campaigns admit no new work and
leave no workers consuming resources after draining or cancellation completes.

Keep source-editing lineages in worktrees and evaluators/results outside them.
Parameter-only trials use immutable configurations. Resume the existing budget
ledger rather than creating a fresh allowance. Follow the research policy for
portfolio allocation, retention, confirmation access, and stagnation pivots.

Each implementation task also records its source checkpoint, completed checks,
unresolved failures, and next action so development can resume between work
sessions.

## Validation, completion, and exclusions

Run bounded stateful property tests using the frozen trace generators. Interleave
inputs, supported edits, injected failures, and recovery; compare outputs,
retained history, versions, and edit outcomes with the independent reference at
declared observation boundaries. Check that rejected edits preserve committed
state and replay reconstructs the same logical observations. Exercise the
deliberate faults from I0 against the integrated checks. Apply shared traces to
RTL01 where its declared interface supports the operations.

Preserve each failing trace and attempt to minimize it while retaining the same
failure. Store reusable minimized counterexamples in `corpus/`, linked to the
original run and semantic version. Record seeds and exact generated traces so
replay does not depend on rerunning a generator. Generation, fault checks, and
minimization consume the existing campaign budgets; stop at the cap and retain
the original failure if minimization is incomplete. Newly discovered cases enter
development or a subsequent frozen comparison, not the current confirmation set.

Require these acceptance scenarios:

- Valid conformance fixtures pass and every declared deliberate fault is detected;
  bounded generated traces agree with the reference, with coverage reported
  explicitly. Unresolved conformance failures leave the affected gate incomplete.
- Exact CPU/CUDA agreement for declared integer semantics and C03's finite domain.
- Correct version boundaries for parameter and operation changes.
- History expansion with no invented or lost retained samples.
- Stale edit rejection, exhaustion handling, failed migration, and interrupted
  publication.
- Replay reconstructing logical state and mock effects without a proposal service.
- Cancellation and pause/resume without lost evidence, duplicate promotion, or
  reset budgets.
- Baselines measured before search; finalists checked on untouched confirmation
  cases.
- Cold and reused execution reported separately, starting with five complete
  repetitions per candidate.
- RTL admission and RTL01 agreement at logical boundaries, including state
  retention, backpressure, invalid import rejection, enforced resource ceilings,
  and interruption/restart without lost evidence or reset budgets.

Use the whitepaper's provisional compatible-edit target of p95 below 100 ms and
unrelated UI-stall target of p99 below 50 ms. Declare migration and recovery
budgets before comparative runs. Report event counts and uncertainty; insufficient
tail samples leave those claims unvalidated.

Report accepted work, quality, coverage, backlog, invalid/discarded results, edit
latency, state continuity, recovery, compilation/checking costs, and research costs
separately. A claimed improvement must pass correctness and interaction gates and
exceed a preregistered practical margin and measurement uncertainty.

**R1 is complete when CPU, CUDA, and the bounded RTL lane run reproducibly, the
integrated behavior passes its declared contracts, and the evidence supports a
concrete next research decision.** Specialization or search may fail to improve
performance; a well-supported negative result still answers the research question.

Defer live learned models, full representation A/B/C comparisons, QEMU integration,
novel hardware cost models, synthesis/place-and-route, physical FPGA work,
simulator checkpoint restoration, general graph-topology migration, and formal
proof tooling. Preserve extension boundaries for them without making them
prerequisites for this revision. Host measurements and functional RTL simulation
do not establish physical hardware performance.

## After R1: bounded formal-assurance pilot

Status: planned follow-on; outside R1 completion and its six-campaign allocation.
Use R1's stable history-expansion contract, independent reference, and retained
counterexamples to test whether formal assurance earns its authoring, checking,
and maintenance cost. The [whitepaper's targeted assurance method](whitepapers/Reprogrammable_Blob_Research_Plan.md#lean-as-targeted-correctness-assurance)
owns proof policy and trust boundaries; this section defines the delivery order.

Before starting, freeze a separate campaign definition with active-time, attempt,
per-check, memory, and storage limits, including toolchain preparation and proof
maintenance. Use existing hardware and no paid services or live model calls.
Set the budget and promotion criteria before collecting comparative evidence.
Budget exhaustion yields an incomplete or inconclusive result, not an automatic
extension or a weakened theorem.

1. **Freeze the obligation and admit one prover.** State the filter invariant,
   preservation of retained sample order and values during 64-to-128 expansion,
   initialization of new storage without inventing valid history, and unchanged
   committed state on rejection. Review preconditions and failure behavior against
   R1's valid and faulty examples before proof attempts. Select Lean or Rocq using
   the [whitepaper's selection procedure](whitepapers/Reprogrammable_Blob_Research_Plan.md#rocqcoq-and-choosing-a-proof-ecosystem);
   compare both only if selection remains unresolved, within the same capped
   allocation. Pin the selected toolchain and dependencies, reproduce a clean
   proof check, and verify that the assurance gate rejects a deliberate proof hole
   or unapproved assumption. Keep the proof lane separately selectable.
2. **Prove the bounded migration model.** Formalize the transition and prove the
   frozen obligations under recorded assumptions. Prefer a parameterized capacity
   theorem when practical, with 64-to-128 as an explicit instance. Replay R1's
   cases against the executable model and implementation to expose mismatches.
   Keep the independent reference independent. Report an implementation passing
   those comparisons as tested against a proved model; a stronger claim requires
   an explicit refinement proof covering that implementation and its assumptions.
3. **Make assurance artifacts replayable.** Define a versioned proof manifest in
   `contracts/schemas/` binding the contract, model, theorem, assumptions, candidate,
   proof artifact, checker, dependencies, and implementation coverage. Link it
   from the research policy's run record. Add a documented proof-check command and
   a dedicated CI check that can reproduce the claim without a proposal service.
   Apply the whitepaper's axiom audit and distinguish kernel checking from native
   evaluation or foreign execution.
4. **Exercise change and admission.** Add one state field and change one migration
   precondition in separate controlled variants; measure proof repair and recheck
   effort and ensure evidence for the old claim cannot admit the changed candidate.
   Under a proof-required admission policy, keep the admitted implementation active
   while checking its replacement. Reject missing, stale, failed, or timed-out
   evidence before publication; passing a proof does not bypass other R1 checks.
5. **Decide whether to expand.** Compare tests alone with tests plus proofs for the
   same frozen obligations and candidate set. Record defects found, remaining
   model/implementation gaps, authoring and maintenance effort, check time, and
   admission latency. Preserve failures and unfinished attempts. Report separately
   whether the proof gate passed and whether the evidence supports further work.

A successful tooling pilot requires reproducible checked obligations, an audited
manifest, model/implementation comparisons with explicit coverage, and demonstrated
invalidation and admission behavior. A closed campaign may instead conclude that
the approach is too costly or remains inconclusive. Record that decision and its
reversal criteria before proposing broader replay, concurrency, CPU/CUDA refinement,
or hardware proofs; none of those broader claims follows from this pilot.

## Initial task list

All tasks are planned. A dash in the evidence column means no completion evidence
exists yet. Replace it with durable evidence links when checks pass; do not mark a
task complete solely because implementation exists. Keep current status here and
immutable observations in run records rather than appending progress narratives.

| ID | Dependencies | Deliverable and acceptance check | Status | Evidence |
| --- | --- | --- | --- | --- |
| Z0-01 | None | Freeze reconstructable source, evidence inventory, five questions, and falsification criteria. | Planned | — |
| I0-01 | Z0-01 | Admit and pin CPU/CUDA/NVRTC environments; retain successful and failed probe results. | Planned | — |
| I0-02 | Z0-01; I0-01 for CUDA checks | Complete the bounded toolchain spike and record selection with reversal criteria. | Planned | — |
| I0-03 | Z0-01 | Freeze semantic/interface contracts, independent reference, hand-checked cases, and corpus splits; verify valid fixtures pass and declared faulty transitions/outcomes are detected. | Planned | — |
| I0-04 | I0-01, I0-02, I0-03, I0-05 | Complete campaign definitions and minimal laboratory; verify limits, identity checks, evidence retention, and pause/resume. | Planned | — |
| I0-05 | I0-01 | Admit the native RTL container and script harness under RTL01 preparation limits; verify checked simulation, waveform bounds, resource enforcement, and cancellation. | Planned | — |
| R1-01 | I0-04 | Implement live-change transactions and replay; pass failure/recovery cases and bounded stateful traces, detect declared deliberate faults, and retain replayable counterexamples. | Planned | — |
| R1-02 | I0-04 | Implement four execution paths; establish CPU and CUDA baselines against the independent reference. | Planned | — |
| R1-03 | R1-01, R1-02 | Integrate inspector and bounded specialization controller; verify admission before publication, candidate-bound evidence and coverage views, edits, inspection fidelity, and interaction targets. | Planned | — |
| R1-04 | R1-02 | Run C01–C03 with complete comparison reports and explicit exhaustive-domain limits. | Planned | — |
| R1-05 | R1-01, R1-02 | Run bounded batch-size search with fixed/random/local controls, retained proposals, and evidence-linked decisions. | Planned | — |
| R1-06 | R1-03, R1-04, R1-05, R1-07 | Complete confirmation, replay, paired human tasks, integrated reports, documentation reconciliation, and next research decision. | Planned | — |
| R1-07 | I0-04, R1-01, R1-02 | Implement and run RTL01; compare logical outputs/state/edits, verify failure and restart behavior, and report cycles separately from host performance. | Planned | — |
