
# 术语扁平化与 MeSH URI 转换

## 扁平化概念字典

```
def flatten_concepts(concepts_dict):
    flat_list = []

    def recurse_terms(term_dict):
        for term, narrower_terms in term_dict.items():
            flat_list.append(term)
            if narrower_terms:
                recurse_terms(dict.fromkeys(narrower_terms, []))  # Use an empty dict to recurse
    
    recurse_terms(concepts_dict)
    return flat_list

# Flatten the concepts dictionary
flat_list = flatten_concepts(all_concepts)
```

然后我们将这些术语转换为 MeSH URI，以便我们可以将它们合并到我们的 SPARQL 查询中：

## 转换为 MeSH URI

```
#Convert the MeSH terms to URI
def convert_to_mesh_uri(term):
    formatted_term = term.replace(" ", "_").replace(",", "_").replace("-", "_")
    return URIRef(f"http://example.org/mesh/_{formatted_term}_")


# Convert terms to URIs
mesh_terms = [convert_to_mesh_uri(term) for term in flat_list]
```

然后，我们编写一个 SPARQL 查询来查找带有“口腔肿瘤”、其替代名称“口腔癌”或任何狭义术语标签的所有文章：

## SPARQL 查询与文章排名

```
from rdflib import URIRef

query = """
PREFIX schema: <http://schema.org/>
PREFIX ex: <http://example.org/>

SELECT ?article ?title ?abstract ?datePublished ?access ?meshTerm
WHERE {
  ?article a ex:Article ;
           schema:name ?title ;
           schema:description ?abstract ;
           schema:datePublished ?datePublished ;
           ex:access ?access ;
           schema:about ?meshTerm .

  ?meshTerm a ex:MeSHTerm .
}
"""

# Dictionary to store articles and their associated MeSH terms
article_data = {}

# Run the query for each MeSH term
for mesh_term in mesh_terms:
    results = g.query(query, initBindings={'meshTerm': mesh_term})

    # Process results
    for row in results:
        article_uri = row['article']

        if article_uri not in article_data:
            article_data[article_uri] = {
                'title': row['title'],
                'abstract': row['abstract'],
                'datePublished': row['datePublished'],
                'access': row['access'],
                'meshTerms': set()
            }

        # Add the MeSH term to the set for this article
        article_data[article_uri]['meshTerms'].add(str(row['meshTerm']))

# Rank articles by the number of matching MeSH terms
ranked_articles = sorted(
    article_data.items(),
    key=lambda item: len(item[1]['meshTerms']),
    reverse=True
)

# Get the top 3 articles
top_3_articles = ranked_articles[:3]

# Output results
for article_uri, data in top_3_articles:
    print(f"Title: {data['title']}")
    print("MeSH Terms:")
    for mesh_term in data['meshTerms']:
        print(f"  - {mesh_term}")
    print()
```
