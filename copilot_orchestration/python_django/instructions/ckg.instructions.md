# Code Knowledge Graph (CKG) Integration for Python & Django

## Purpose and Scope

Keep the CKG focused on enabling agentic coding and advanced reasoning for Python/Django projects:
* Treat the codebase as a graph of interconnected entities (beyond plain text/ASTs).
* Provide structured, reliable context to LLMs (reduces hallucinations, improves navigation and refactoring).
* Capture inter-procedural behavior (data/control flow, call and import graphs).
* Build iteratively and keep it maintainable as the codebase evolves.

* This document is intentionally scoped to CKG modeling, extraction, and loading. For Django patterns, testing, and deployment see references at the end.

## Neo4j Modeling Conventions (CKG)

**Use Neo4j nodes for code entities and relationships for dependencies and behavior. Keep naming consistent:**
* Node labels: PascalCase (e.g., Project, Module, Model, View, Serializer, URLPattern, Field, Function, Class, Signal, Migration)
* Relationship types: UPPER_SNAKE_CASE (e.g., DECLARES, IMPORTS, CALLS, HAS_FIELD, USES_MODEL, SERIALIZES, ROUTES_TO)
* Properties: store stable identifiers and essential metadata (e.g., fqn, path, app_label, name, docstring)

**Minimal Django-oriented schema:**
* Nodes
    * Project(name)
    * Module(project, path)
    * Model(app_label, name)
    * Field(name, type, null, blank, related_name)
    * View(fqn, http_methods)
    * Serializer(fqn)
    * URLPattern(pattern, name)
    * Function(fqn), Class(fqn)
    * Signal(name), Migration(app_label, name, applied_at?)
* Relationships
    * DECLARES: Module → Class|Function|Model|Serializer|View
    * IMPORTS: Module → Module|Symbol
    * HAS_FIELD: Model → Field
    * USES_MODEL: View|Serializer|Form → Model
    * SERIALIZES: Serializer → Model
    * ROUTES_TO: URLPattern → View
    * CALLS: Function|Method → Function|Method
    * EMITS_SIGNAL: Model|View|Function → Signal
    * MIGRATES: Migration → Model|Field

**Constraints and helpful indexes (run once):**

```cypher
// Node keys
CREATE CONSTRAINT project_key IF NOT EXISTS
FOR (p:Project) REQUIRE p.name IS UNIQUE;

CREATE CONSTRAINT module_key IF NOT EXISTS
FOR (m:Module) REQUIRE (m.project, m.path) IS NODE KEY;

CREATE CONSTRAINT model_key IF NOT EXISTS
FOR (m:Model) REQUIRE (m.app_label, m.name) IS NODE KEY;

// Performance indexes (optional)
CREATE INDEX module_path IF NOT EXISTS FOR (m:Module) ON (m.path);
CREATE INDEX symbol_fqn IF NOT EXISTS FOR (s:Function) ON (s.fqn);
CREATE INDEX class_fqn IF NOT EXISTS FOR (c:Class) ON (c.fqn);
```

* Note: Local/cluster setup, Docker, and ops are covered in `docker.instructions.md`.

## Django-Specific Mapping

**Map common Django constructs to graph entities and edges so agents can reason:**
* Models and fields → Model, Field nodes with HAS_FIELD edges (capture related_name, null/blank, on_delete)
* Views and URLs → View, URLPattern nodes with ROUTES_TO edges; annotate View.http_methods
* Serializers → Serializer nodes with SERIALIZES to Model and USES_MODEL from Views when serializers access models
* Imports and declarations → DECLARES and IMPORTS from Modules to symbols and other Modules
* Signals and migrations → EMITS_SIGNAL, MIGRATES to show lifecycle effects
* Query usage (optional) → QUERIES edges with properties like filters/order_by when extracted

## Python/Django Code Extraction

**Preferred toolchain:**
* Parsing: LibCST (preserves formatting and allows robust symbol extraction)
* Generators: KG4Py, GraphGen4Code (when applicable) for Python code graphing

**What to extract (minimum):**
* Modules (files) with fully qualified symbols declared (Classes, Functions, Models, Views, Serializers)
* Imports (module and symbol level) to resolve FQNs
* Django specifics: models/fields (including reverse relations), views (class- and function-based), serializers, URL patterns, migrations
* Optional: docstrings, type hints, decorators, permission classes

## Static Analysis Integration and ETL

**ETL workflow (agent playbook):**
1. Parse with LibCST → capture Modules/Classes/Functions/Models/Views/Serializers/URL patterns
2. Resolve fully qualified names (imports + scopes)
3. Build edges: DECLARES, IMPORTS, CALLS, HAS_FIELD, USES_MODEL, SERIALIZES, ROUTES_TO, EMITS_SIGNAL, MIGRATES
4. Optional enrichment: call graph (dynamic dispatch resolving if possible), CFG/DFG edges when available, type inference
5. Batch export nodes/edges to CSV or JSONL
6. Load into Neo4j via `LOAD CSV` or Python driver transactions

**Suggested export shapes:**
* nodes.csv: id:ID, label:LABEL, properties as columns (e.g., fqn, path, app_label, name)
* edges.csv: :START_ID, rel_type, :END_ID, properties (e.g., method, count)

## Query Examples (Django-aware)

**Views touching a given model directly or via serializer:**

```cypher
MATCH (m:Model {app_label: $app, name: $model})
OPTIONAL MATCH (s:Serializer)-[:SERIALIZES]->(m)
OPTIONAL MATCH (v:View)-[:USES_MODEL]->(m)
OPTIONAL MATCH (v)-[:CALLS]->(:Function)<-[:DECLARES]-(:Module)-[:DECLARES]->(s)
RETURN DISTINCT v.fqn AS view, m.name AS model ORDER BY view;
```

**Endpoints without URL mapping (potential dead code):**

```cypher
MATCH (v:View)
WHERE NOT ( (:URLPattern)-[:ROUTES_TO]->(v) )
RETURN v.fqn LIMIT 100;
```

**Potential N+1 (heuristic when QUERIES edges captured):**

```cypher
MATCH (v:View)-[q:QUERIES]->(m:Model)
WITH v, m, count(q) AS calls
WHERE calls > $threshold
RETURN v.fqn, m.name, calls ORDER BY calls DESC;
```

## Best Practices and Pitfalls

* Build incrementally; keep loaders idempotent and use constraints to avoid duplicates
* Prefer properties over separate nodes for simple metadata (e.g., docstring on nodes)
* Keep schemas flexible; add labels/relationships rather than breaking changes
* Only include CFG/DFG edges if you can populate them reliably; otherwise mark as optional
* Ensure FQNs are consistent across loaders to enable stable joins

**Out of scope (covered elsewhere):**
* Django coding patterns, DRF design, security/caching → `django.instructions.md`
* Testing strategies and quality gates → `testing.instructions.md`
* Local/dev/prod environment and Neo4j Docker setup → `docker.instructions.md`

## References

* LibCST: https://libcst.dev/
* KG4Py: https://github.com/IBM/KG4Py
* GraphGen4Code: https://github.com/IBM/GraphGen4Code
* Neo4j Cypher Manual: https://neo4j.com/docs/cypher-manual/
* Related: `python_django/instructions/django.instructions.md`, `testing.instructions.md`, `docker.instructions.md`
