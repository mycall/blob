# Working in this repository

Follow the [repository architecture](docs/architecture.md) for directory ownership,
dependency boundaries, and evidence handling. See the [README](README.md) for
current implementation status and the research entry point.

Keep downloaded or built external payloads in the ignored locations described in
[external assets](external/README.md). Track their provenance and reconstruction
instructions in manifests, not the payloads themselves.

For experiment campaigns, follow the [autonomous research policy](docs/guides/autonomous-research.md).
Freeze each comparison, preserve candidate evidence, and enforce declared budgets
and pivots. Use isolated worktrees for source-editing lineages; keep the evaluator
and durable results outside them. A worktree is not a security or resource boundary.

## Keep documentation coherent with current understanding

Maintain documentation when implementation, semantic contracts, experimental
methods, evidence, or working conclusions change. Each maintained document
should explain its subject coherently without requiring readers to reconstruct
a sequence of updates.

- Rewrite affected passages instead of appending corrections, dated updates,
  or accounts of how the project arrived at its current state.
- Clearly distinguish proposals, implemented capabilities, validated behavior,
  and unresolved questions. Do not present a planned tool or interface as existing.
- Keep claims tied to their evidence and scope. Distinguish host measurements,
  modeled estimates, and physical results; preserve relevant assumptions,
  uncertainty, and limitations.
- When evidence changes a conclusion, update the current explanation and link
  to the supporting experiment or decision record.
- Give each topic one authoritative home and link to it elsewhere.
- Keep rationale needed to understand current choices. Store superseded choices,
  alternatives, and reversal criteria in decision records; use Git for routine
  edit history.
- Preserve versioned contracts and recorded experiment inputs, results, and
  decisions needed for replay. Correct historical evidence through explicit,
  linked amendments rather than silently rewriting it.

Keep `AGENTS.md` focused on durable working rules and links. Do not turn it
into a progress log, session transcript, or list of completed work.

Before completing a change, reconcile affected documentation with both the
implemented state and the evidence available. Leave unrelated material alone.

<!-- SEMBLE_START -->
## Semble Code Search

A `semble` MCP server is available with two tools:
- `mcp__semble__search` — search the codebase with a natural-language or code query.
- `mcp__semble__find_related` — find code similar to a specific file and line.

Use `mcp__semble__search` to find where something is implemented — instead of using Grep or Glob to discover files. After semble returns the file and line, navigate there directly and read that file. Do not grep for the same content again.

Pass `content="docs"` to the MCP search tool for documentation and prose, `content="config"` for config files, or `content="all"` for everything. On the CLI, use `--content docs`, `--content config`, or `--content all` instead.

For CLI fallback or sub-agents without MCP access, use:

```bash
semble search "authentication flow" ./my-project --max-snippet-lines 10
semble search "deployment guide" ./my-project --content docs
semble search "database host port" ./my-project --content config
semble find-related src/auth.py 42 ./my-project
semble search "save model to disk" ./my-project --top-k 10
```

The index is built on first run and cached automatically. If `semble` is not on `$PATH`, use `uvx --from "semble[mcp]==0.5.5" semble`.

### Workflow

1. Call `mcp__semble__search` with a query describing what the code does or its name. The tool returns results with 10 lines of context each (function/class signature + first body lines, enough to confirm the location).
2. Navigate directly to the top result's file and line. Read only the function or class at that location.
3. Make the edit. Do not re-search or grep for the same content.
4. Set the MCP search tool's `content` field to `docs`, `config`, or `all` when searching beyond code.
5. Optionally use `mcp__semble__find_related` with `file_path`, `line`, and the same `content` selection to discover similar code elsewhere.
6. Use Grep only when you need every occurrence of a literal string across the whole repo (e.g., all callers of a renamed function).
<!-- SEMBLE_END -->
