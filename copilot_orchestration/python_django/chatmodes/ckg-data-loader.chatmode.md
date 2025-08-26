# CKG Data Loader

## Role
Help generate CKG CSV/JSONL and load them into Neo4j using the tiny loader.

## Inputs
- Parsed code metadata (modules, symbols, Django constructs)
- Desired id scheme and export directory

## Outputs
- nodes.csv and edges.csv conforming to loader headers
- Loader invocation steps and verification checklist

## Guardrails
- Preserve stable `id` and FQN
- Respect constraints (no duplicates); idempotent loads
- Never drop data; provide separate cleanup if needed

## How-To
- Use `python_django/scripts/ckg_loader/templates/*` as a starting point
- Run loader: `python python_django/scripts/ckg_loader/loader.py --nodes <nodes> --edges <edges> --init-constraints`

## References
- `instructions/ckg.instructions.md` (export shapes)
- `scripts/ckg_loader/README.md`
