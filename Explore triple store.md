## Queries to explore a triple store

### Get all the types of things in a named graph

This tells us what entities we have.

```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX schema: <http://schema.org/>
SELECT ?type (COUNT(?type) AS ?count) 
FROM <https://zenodo.org>
WHERE {
  ?s rdf:type ?type .
  FILTER (?type != schema:PropertyValue)
} 
GROUP BY ?type
ORDER BY DESC(?count)
```

### Counts of literal values for a type of thing in a named graph

What are the most common types of values for the things

```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX schema: <http://schema.org/>
SELECT ?p (COUNT(?p) AS ?count) 
FROM <https://zenodo.org>
WHERE {
  ?s rdf:type schema:Person .
  ?s ?p ?o .
  FILTER isLiteral(?o)
} 
GROUP BY ?p
ORDER BY DESC(?count)
```

### Reachability between things in a named graph

See [SemSpect](https://www.semspect.de) and Wenzel et al. for concept of “reachability”. For a node of a given type (the “centre”) we get counts for edges linking to other types of things.

> Wenzel, M., Liebig, T., Glimm, B. (2021). HDT Bitmap Triple Indices for Efficient RDF Data Exploration. In: , et al. The Semantic Web. ESWC 2021. Lecture Notes in Computer Science(), vol 12731. Springer, Cham. https://doi.org/10.1007/978-3-030-77385-4_7


```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX schema: <http://schema.org/>
SELECT ?centre_type ?predicate ?reachable_type (COUNT(?reachable) AS ?count) 
FROM <https://zenodo.org>
WHERE {
  VALUES ?centre_type { schema:ScholarlyArticle } .
  ?centre rdf:type ?centre_type .
  
# outgoing edges from centre
  #?centre ?predicate ?reachable .
  
# incoming edges to centre
  ?reachable ?predicate ?centre .
  
  ?reachable rdf:type ?reachable_type .
  
  FILTER (?predicate != rdf:type)
} 
GROUP BY ?centre_type ?predicate ?reachable_type
```


### Summarise values for a subset of things in a named graph

This is like getting the first few rows in a database, for example the first few people in the database

```
PREFIX schema: <http://schema.org/>
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
CONSTRUCT {
  ?sub ?pred ?obj .
}
FROM <https://zenodo.org>
WHERE {
  ?sub ?pred ?obj .
  ?sub rdf:type schema:Person .
  FILTER isLiteral(?obj)
} LIMIT 10
```