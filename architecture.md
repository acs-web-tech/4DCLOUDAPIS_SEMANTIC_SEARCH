# Semantic Search API - Architecture Documentation
## Overview
Our Semantic Search API enables advanced search capabilities by understanding and processing the context of the business data provided by clients. 
This is achieved by converting textual data into vector embeddings, which allows for efficient and accurate matching of user queries with relevant content.
The API supports recommendation and filtering systems with semantic understanding, enhancing the relevance and precision of search results.
## Data Ingestion and Storage
- **Data Collection**: We collect business data from the client, which may include documents, articles, product descriptions, and other relevant textual content.
- **Vector Embedding Conversion**: The collected content is converted into vector embeddings using a pre-trained language model. This model transforms the text into high-dimensional vectors that capture the semantic meaning of the content.
## Preprocessing
```
from transformers import BertFastTokenizer
model = BertFastTokenizer('model-params')
# Sample text
text = "Tokenization is the process of breaking down text into smaller units called tokens."
model.tokenize(text)
```
```
Sample Output
['Tokenization', 'is', 'the', 'process', 'of', 'breaking', 'down', 'text', 'into', 'smaller', 'units', 'called', 'tokens', '.']
```
## Vector Embeddings
```
[ 0.00221179 -0.00761984 -0.00863784  0.0048019  -0.00191677  0.00765403  0.0024129  -0.00548141  0.00549636  0.00267502  0.00720963  0.00222149 -0.00332724  0.00543128  0.00794798 -0.00245991  0.00730025 -0.00147913 -0.0013198   0.00364289 -0.00377467 -0.00270385 -0.00350482 -0.00701469  0.0044041   0.00802503  0.00150852  0.00241609 -0.00298363 -0.0032633  0.00335768 -0.00752975  0.00291133  0.00566304  0.00305786 -0.00257623  0.00546343  0.00432486  0.00194288  0.00409517  0.0021689   0.00272466 -0.00396108 -0.00532654  0.00421494  0.00751572 -0.00435477  0.00280888  0.00185572  0.00433916]
```
## Vector Search Cypher query
```
// create vector index
CREATE VECTOR INDEX `abstract-embeddings`
FOR (n: Abstract) ON (n.embedding)
OPTIONS {indexConfig: {
 `vector.dimensions`: 1536,
 `vector.similarity_function`: 'cosine'
}};


// set embedding as parameter
MATCH (a:Abstract {id: $id})
CALL db.create.setNodeVectorProperty(a, 'embedding', $embedding);


// use a genai function to compute the embedding
MATCH (a:Abstract {id: $id})
WITH a, genai.vector.encode(a.text, "AI", { token: $token }) AS embedding
CALL db.create.setNodeVectorProperty(n, 'embedding', embedding);


// query vector index for similar abstracts
MATCH (title:Title)<--(:Paper)-->(abstract:Abstract)
WHERE title.text CONTAINS = 'hierarchical navigable small world graph'
CALL db.index.vector.queryNodes('abstract-embeddings', 10, abstract.embedding)
YIELD node AS similarAbstract, score

MATCH (similarAbstract)<--(:Paper)-->(similarTitle:Title)
RETURN similarTitle.text AS title, score;


// use cosine similarity for exact nearest neighbor search
// pre-filter vector search
MATCH (venue:Venue)<--(:Paper)-->(abstract:Abstract)
WHERE venue.name CONTAINS 'NETWORK'

WITH abstract, paper,
     vector.similarity.cosine(abstract.embedding, $embedding) AS score
WHERE score > 0.9

RETURN paper.title AS title, abstract.text, score
ORDER BY score DESC LIMIT 10;
```
**Response Generation**: The content with the highest similarity scores is returned to the user. Each response includes the matched content, metadata, and a relevance score.
```
[
    {
        "matched_content": "...content",
        "meta_data": {
            "title": "content"
        },
        "score": 0.7298728227615356
    }
]
```
### Caching for Latency Reduction
To enhance the performance and reduce the latency of the API, a caching mechanism is implemented. This cache stores frequently accessed data and precomputed results of common queries, ensuring faster response times.

- **Cache Implementation**: A cache layer is introduced to store the results of recent and frequent queries.
- **Cache Management**: The cache is regularly updated and managed to ensure it contains the most relevant and up-to-date information.
### Bulk Dataset Handling
Handling bulk datasets might result in longer response times due to the processing and embedding conversion requirements. We recommend the following practices for efficient handling of bulk data:
- **Batch Processing**: Split the dataset into smaller batches and process them sequentially.
- **Asynchronous Processing**: Use asynchronous processing techniques to handle data ingestion and embedding conversion without blocking the main workflow.
- **Incremental Updates**: Instead of reprocessing the entire dataset, update the embeddings incrementally as new data comes in.
## Use Cases
- **Recommendation System**: The Semantic Search API can be used to power recommendation systems by matching user preferences with relevant content based on semantic similarity.
- **Filtering System**: It can also be used to implement advanced filtering mechanisms, where content is filtered based on the semantic context rather than just keyword matching.
### Note
The Architecture  documentation is just an example not the replica of the API 
