# Data Model & Query lang

- IBM's Information Management System(IMS) built in 1968
- Network Model
    - One node can have many child nodes, like a tree
    - Pro
        - Was able to make one-many, many-many relationship easier
    - Con
        - Fetching a data node require traversal of each node in a sequential fashion
        - Write became difficult & costly
- Document Model(Schema-On-Read) vs Relational Model(Schema-on-write)
    - Where the schema is not defined(heterogenous schema), document model is appropriate
    - Where schemas are more or less the same, relational model is appropriate
- Declarative vs Imperative way
    - Most programming language follow imperative way
    ```SQL
        function getSharks() { var sharks = [];
            for (var i = 0; i < animals.length; i++) { if (animals[i].family === "Sharks") {
                sharks.push(animals[i]); }
            }
            return sharks; 
        }
    ```
    - Relation algebra has sharks = σfamily = “Sharks” (animals), which is a declarative type
    - Declarative is what most Relational DB uses, also CSS for that matter

## Graph like Data Model

- Best support for many-many relationships 
- Can be Property Graph Model(Neo4j, Titan, Infinite Graph) or Triple Store Model(Datomic, AllegroGraph, and others)
- We have declarative query languages such as Cyper, Sparql, Datalog 

### Property Graph

- Can be represented as a set of vertices & edges
- If we want to represent in a relation model, it'll look something like this:
```
CREATE TABLE vertices (
    vertex_id integerPRIMARYKEY, properties json
);
CREATE TABLE edges (
    edge_id integer PRIMARY KEY,
    tail_vertex integer REFERENCES vertices (vertex_id), head_vertex integer REFERENCES vertices (vertex_id), label text,
    properties json
);
CREATE INDEX edges_tails ON edges (tail_vertex);
CREATE INDEX edges_heads ON edges (head_vertex);
```
    
