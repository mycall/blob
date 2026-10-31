# Working in this repository

Use the [README](README.md) for project status and the
[architecture guide](docs/architecture.md) for ownership and dependency boundaries.
Keep external payloads in [ignored asset locations](external/README.md); track
provenance and reconstruction manifests.

For experiments, follow the [research policy](docs/guides/autonomous-research.md):
freeze comparisons, preserve candidate evidence, and enforce budgets and pivots.
Use worktrees for source-editing lineages, with evaluators and durable results
outside them. Worktrees do not isolate permissions or compute resources.

## Documentation

- When implementation, contracts, methods, evidence, or conclusions change,
  rewrite affected passages into a coherent current account. Remove stale guidance;
  do not append dated updates or running changelogs. Leave unrelated material alone.
- Distinguish proposals, implemented capabilities, validated behavior, and open
  questions. Separate host measurements, modeled estimates, and physical results;
  preserve assumptions, uncertainty, and limitations.
- Give each topic one authoritative home. Link conclusions to supporting evidence
  and keep rationale needed to understand current choices. Store alternatives,
  superseded decisions, and reversal criteria in `docs/decisions/`; use Git for
  routine edit history.
- Preserve versioned contracts and experiment inputs, results, and decisions needed
  for replay. Correct historical evidence through explicit linked amendments.
- Before finishing work, reconcile affected documentation with implementation and
  evidence. Keep `AGENTS.md` to durable rules and links, never progress logs,
  transcripts, or completed-task lists.

<!-- SEMBLE_START -->
## Code search

Use `mcp__semble__search` with the repository root and a focused query; read the
returned file and line directly. Use `content="docs"`, `"config"`, or `"all"` for
non-code searches. Use Grep only to enumerate every occurrence of a literal string.

CLI fallback: `semble search "query" . --content all --max-snippet-lines 10`.
If unavailable, use `uvx --from "semble[mcp]==0.7.1" semble` in place of `semble`.
<!-- SEMBLE_END -->
