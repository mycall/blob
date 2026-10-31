# Representation comparison

Status: proposed experiment outline; no implementation, populated campaign
manifest, or measurements exist. Instantiate the
[campaign template](../_template/program.md) and fill its budgets and frozen
artifacts before running. Follow the
[research policy](../../docs/guides/autonomous-research.md).

## Question

Does the provisional [semantic-state baseline](../../docs/decisions/0001-semantic-state-and-projections.md)
improve faithful editing, replay, and understanding enough to justify its
representation and maintenance cost? Does a learned workspace add useful discovery
or interpretation capabilities at a matched total budget?

## Alternatives

| Variant | Representation and interface |
| --- | --- |
| A: English-centered | Intent and working descriptions primarily in prose; interpretation produces the executable changes required by the common task contract. |
| B: Explicit state | Versioned semantic records and transitions, with English and visual views. |
| C: Augmented state | The same explicit commit boundary as B, with a replaceable learned workspace for retrieval, analogy, and proposals. |

All variants face the same observable task requirements and independent reference.
Do not secretly give A a maintained semantic knowledge base identical to B and
then call the comparison English-centered. Record the structured execution bridge
and its costs in each variant. Preserve enough artifacts to test replay; report
any inability to meet that requirement instead of weakening the common gate.

C can begin with ordinary embeddings or recorded proposals. A claim about J-space
specifically requires access to appropriate model internals, a documented lens,
and a separately admitted implementation; ordinary embeddings are not evidence
about J-space.

## Tasks and checks

Use the streaming filter as a common domain:

1. Change a parameter, replace an operation, and expand history capacity while
   preserving the samples actually available.
2. Interpret ambiguous and paraphrased instructions; retain uncertainty rather
   than committing an unsupported interpretation.
3. Edit an English or visual view based on a stale version; detect the conflict
   and revalidate any rebased command.
4. Deliberately fail a migration and interrupt publication; reconstruct the
   actual outcome and logical state without repeating external effects.
5. Explain current behavior and its evidence, then replace the interpreter/model
   used for new proposals. Replay old history without calling that model.
6. Introduce a previously unrepresented concept. Measure representation-extension
   effort and information loss instead of automatically treating the task as invalid.

Evaluate interpretation of new requests separately from replay of recorded
decisions. For new requests, use matched model resources where possible; any
model mismatch is an explicit confound. For replay, use each variant's preserved
inputs and accepted interpretations under the original rules.

## Measurement and decision

Record intent-to-command errors, unresolved ambiguity, incorrect commitments,
semantic differences after projection edits, replay failures, explanation factual
errors, and evidence traceability. Measure latency, model calls, resource use,
schema/adapter maintenance, and model-replacement effort. Count useful discoveries
only after independent validation under the same task contract.

Use paired human tasks to assess understanding and correction effort; automated
agreement alone does not establish usability. Use independent final confirmation
cases, counterbalance presentation order, and preserve negative results.

Test B against A before investing in C. Promote C only if it improves specified
outcomes enough to justify its added cost while preserving the commit boundary.
Reconsider B if its representation repeatedly omits needed meaning or if A meets
the same requirements with better outcomes. If evidence is inconclusive, retain
the simpler baseline and record the uncertainty rather than declaring a winner.
