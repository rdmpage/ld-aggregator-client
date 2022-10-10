# Example queries

## Organisations

Find organisations that have ROR identifiers.

```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX schema: <http://schema.org/>
SELECT DISTINCT ?name ?value 
FROM <https://orcid.org>
WHERE {
  ?organisation a schema:Organization  .
  ?organisation schema:name ?name .
  
  ?organisation schema:identifier ?identifier .
  ?identifier schema:propertyID "ROR" .
  ?identifier schema:value ?value .
} 
LIMIT 1000
```