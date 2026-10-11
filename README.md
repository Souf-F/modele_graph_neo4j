# Graph Database Model — Neo4j

Research comparing graph databases (Neo4j) with relational databases (MySQL), with Cypher query examples applied to a streaming platform scenario.

Holberton School France · June 2026

## What is a graph database?

Data stored as **nodes** (entities), **relations** (typed, directed links), and **properties** (key/value pairs). Built for highly connected data where SQL joins become a bottleneck.

```
Node ──[:RELATION]──► Node
 │                     │
properties           properties
```

## MySQL vs Neo4j

| Criterion | MySQL | Neo4j |
|-----------|-------|-------|
| Structure | Tables, rows, columns | Nodes, relations, properties |
| Query language | SQL | Cypher |
| Joins | Required, cost grows with depth | Replaced by graph traversal |
| Strength | Structured tabular data | Highly connected data |
| Use cases | E-commerce, CRM, ERP | Social graph, fraud detection, recommendations |
| Relation performance | Degrades with query depth | Constant regardless of graph size |
| Schema | Fixed | Flexible |

## Cypher example

Friends-of-friends movie recommendations — one traversal, no joins:

```cypher
MATCH (new:Movie {title:'Dune 2'})
      -[:IN_GENRE]->(g)<-[:IN_GENRE]-(other)
      <-[:WATCHED]-(user)
RETURN DISTINCT user.name
```

The same query in SQL would require 4+ joins with performance degrading as the network grows.

## When to choose Neo4j

- Relationships are the core of your data model
- Queries need to traverse multiple levels of connections
- The depth or shape of connections is unknown at query time

## Files

-  — full research notes with more examples and analysis
-  — original assignment questions

---

Soufiane Filali · Holberton School France
