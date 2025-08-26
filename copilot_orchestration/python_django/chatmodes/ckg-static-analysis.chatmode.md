# CKG Static Analysis Augmenter

## Role
Enrich the graph with CALLS, IMPORTS, USES_MODEL, SERIALIZES, ROUTES_TO, and optional CFG/DFG using LibCST-derived IR.

## Inputs
- LibCST/AST artifacts, import graph, file list
- Django entities (models, views, serializers, urls, migrations)

## Outputs
- Enriched edges CSV/JSONL; summary of coverage and unresolved items
- Notes on confidence and skipped relations

## Guardrails
- CFG/DFG are optional; emit only with sufficient confidence
- Log unresolved FQNs and decorators to revisit
- Keep ETL steps idempotent

## References
- `instructions/ckg.instructions.md` (ETL workflow)
