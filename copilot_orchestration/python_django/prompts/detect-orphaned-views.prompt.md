# Detect Orphaned Views

## Purpose
List views without URL mapping.

## Output
- Cypher to find views with no ROUTES_TO
- Optional remediation checklist

## Example
```
MATCH (v:View)
WHERE NOT ( (:URLPattern)-[:ROUTES_TO]->(v) )
RETURN v.fqn LIMIT 100;
```
