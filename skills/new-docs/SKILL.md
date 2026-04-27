---
description: Generate implementation-grade project documentation from source code into `docs`
---

# New Docs

Generate project documentation directly from the current codebase.

## Arguments

`$ARGUMENTS`

Use arguments as optional scope controls (for example: target subdirectory, include/exclude patterns, or language focus). If no arguments are provided, document the entire repository.

## Core Rules

- Follow drafting guidance from `new-docs` skill (this skill) for document writing standards.
- Store all generated documentation in `docs`.
- Prefer multiple focused files over a single large file.
- Cover **all code modules** in scope, including (but not limited to): `.py`, `.html`, `.js`, `.ts`, `.tsx`, `.jsx`, `.java`, `.sh`, `.go`, `.rs`, `.php`, `.rb`, `.cs`, `.kt`, `.swift`, `.sql`, `.yaml`, `.yml`.
- Documentation must be detailed enough that Claude Code can implement the full project from docs alone.
- Derive docs from source-of-truth files and code; avoid inventing behavior.
- Use `planner` agent to draft a plan before your actions.

## Workflow

1. **Inventory the codebase**
   - Identify project type(s), runtimes, entry points, module boundaries, and infra files.
   - Build a complete module index for all relevant source files in scope.

2. **Map architecture and behavior**
   - Trace request/data/control flow across layers.
   - Capture dependencies, external services, config/env requirements, scripts/commands, and operational behavior.

3. **Generate `docs/` set (small, focused files)**
   - `docs/README.md` — navigation index for all generated docs.
   - `docs/architecture.md` — system structure, boundaries, and flow.
   - `docs/modules.md` — module catalog with purpose and ownership by path.
   - `docs/runtime.md` — setup, scripts, execution model, env/config.
   - `docs/data-model.md` — storage schemas, entities, and relationships.
   - `docs/integrations.md` — third-party APIs/services and interaction contracts.
   - `docs/operations.md` — deploy/runbook, health checks, failure/rollback paths.
   - `docs/implementation-guide.md` — end-to-end rebuild blueprint.
   - `docs/modules/<path-safe-module-name>.md` — one file per significant module or tightly related module group.

4. **Per-module documentation requirements**
   For each documented module, include:
   - File path(s) and responsibility.
   - Public interfaces (functions/classes/endpoints/CLI commands).
   - Inputs/outputs, side effects, and invariants.
   - Internal dependencies and call relationships.
   - Error handling and edge cases.
   - Security considerations and validation boundaries.
   - Reimplementation notes (what must be preserved for parity).

5. **Coverage validation**
   - Produce a coverage matrix in `docs/modules.md` mapping every discovered source module to a documentation target.
   - Explicitly list any skipped/generated/vendor files and the reason.
   - If coverage is incomplete, continue until all in-scope modules are documented.

6. **Staleness and provenance**
   - Add generated markers and scan metadata (date, scope, files scanned).
   - Preserve manually written sections when updating existing docs.

7. **Final summary**
   - Report created/updated files in `docs`.
   - Report module coverage totals and any intentional exclusions.

## Output Quality Bar

The generated documentation must provide enough architectural, interface, and behavioral detail for full-project reconstruction without reading the original code.
