# Data Model & Query lang

- IBM's Information Management System(IMS) built in 1968
- Network Model
    - One node can have many child nodes, like a tree
    - Pro
        - Was able to make one-many, many-many relationship easier
    - Con
        - Fetching a data node require traversal of each node in a sequential fashion
        - Write became difficult & costly
- **Document Model(Schema-On-Read) vs Relational Model(Schema-on-write)**
    - Where the schema is not defined(heterogenous schema), document model is appropriate
    - Where schemas are more or less the same, relational model is appropriate
- **Declarative vs Imperative way**
    - Most programming language follow imperative way
    ```javascript
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
- Can be **Property Graph Model(Neo4j, Titan, Infinite Graph)** or **Triple Store Model(Datomic, AllegroGraph, and others)**
- We have declarative query languages such as **Cyper, Sparql, Datalog**

### Property Graph
- Can be represented as a set of vertices & edges
- If we want to represent in a relation model, it'll look something like this:
```SQL
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

#### Cypher Query Language
- Developed for Neo4j(Named after Neo Matrix)
```
CREATE
  (NAmerica:Location {name:'North America', type:'continent'}),
  (USA:Location      {name:'United States', type:'country'  }),
  (Idaho:Location    {name:'Idaho',         type:'state'    }),
  (Lucy:Person       {name:'Lucy' }),
(Idaho) -[:WITHIN]-> (USA) -[:WITHIN]-> (NAme
```
- A sample query will look something like this
```
MATCH
(person) -[:BORN_IN]-> () -[:WITHIN*0..]-> (us:Location {name:'United States'}),
(person) -[:LIVES_IN]-> () -[:WITHIN*0..]-> (eu:Location {name:'Europe'}) 
RETURN person.name
```
- So either we can look up or look behind, both ways works, which is what the Query Optimizer takes care of, finding the best possible way
- **Can be respresented in Relational SQL with a keyword __WITH RECURSIVE__ but the query would be much trickier**

### Triple Store Model
- Follows the **(Subject, Predicate, Object)** pattern, e.g Sam loves movies
- Subject should always be a vertex
- For object it depends
    - If the object is a primitive datatype, such as string or numeric, in that case it would be a property of the subject, e.g; Sam's age is 33, this can be represented as Sam as a vertex, having property {age: 33}
    - If the object is other than above, it can be a vertex, which subject connecting to object via an edge, which is the predicate, e.g; Sam is married to Andrea, this can be represent as Sam, Andrea as two vertex & marriage is the edge connecting them both


    
