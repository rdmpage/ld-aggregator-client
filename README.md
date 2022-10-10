# Linked data aggregator client

Exploring GraphQL interface to a SPARQL endpoint for biodiversity data. 

Basic idea is to do SPARQL CONSTRUCT queries and JSON-LD to output JSON-format results that are easy to convert to GraphQL results. Introduces layers of complexity, such as defining the GraphQL interface, managing JSON-LD framing, etc. Tradeoff is it gives user a defined set of queries and access to rich GraphQL clients.



