# Generate Neo4j Constraints

## Inputs
- labels and properties in use

## Output
- Idempotent Cypher: CREATE CONSTRAINT/INDEX IF NOT EXISTS

## Contract
- Keys: Project(name), Module(project,path), Model(app_label,name), FQN-based for Class/Function/View/Serializer
- Include optional indexes for performance (path, fqn)
