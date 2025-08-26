# Write Cypher From Question

## Inputs
- question (string)
- params (object): app_label?, model?, view_fqn?, depth?, limit?

## Output
- Read-only Cypher with LIMIT and short rationale

## Template
```
Question: <your question>
Params: {"app_label": "<app>", "model": "<model>", "limit": 50}

-- Cypher --
MATCH ...
RETURN ...
LIMIT 50

Why this works: <2-3 lines>
```
