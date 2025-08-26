# Map Django URLs to Views

## Purpose
List URL → View mapping and check for duplicates.

## Output
- Cypher to list ROUTES_TO
- Duplicate URL names report

## Example
```
MATCH (u:URLPattern)-[:ROUTES_TO]->(v:View)
RETURN u.name, u.pattern, v.fqn ORDER BY u.name;
```
