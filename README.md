# 4DCLOUDAPIS SEMANTIC SEARCH
An API which gives an option to search by meaning rather than searching by keywords

# Semantic Search API

The Semantic Search API provides an efficient and accurate way to search through your data using natural language queries. This API leverages advanced machine learning models to understand the context and semantics of your search queries, delivering relevant results with high precision.

## Features

- **Natural Language Processing (NLP):** Understands and processes natural language queries.
- **Semantic Understanding:** Searches based on the meaning of the query, not just keyword matching.
- **Vector Embeddings:** Uses vector embeddings to represent data and queries in a high-dimensional space for better search accuracy.
- **Fast and Scalable:** Designed to handle large datasets with minimal latency.

## API Endpoints

### Search Endpoint

**URL:** `http://4dcloud.cloud/apis/search_vector`

**Method:** `POST`
**Headers:** `4dcloud-auth-token` -> "your api key"

**Description:** Searches the dataset based on the provided query.

**Request:**

- **Headers:**
  - `Content-Type: application/json`

- **Body:**
  ```json
  {
   "projectid":"project_id",
    "query":"your query",
    "result_count":10
  }
### Insertion Endpoint
**URL:** `http://4dcloud.cloud/apis/insertvector_json`
**Method:** `POST`
**Headers:** `4dcloud-auth-token` -> "your api key"

**Description:** Inserts the individual json objects as embeddings

**Request:**

- **Headers:**
  - `Content-Type: application/json`

- **Body:**
  ```json
     {
      "projectid":"projectid",
      "embeddingText":"description",
      "meta_data":"title",
      "concat_field":"title",
     "dataset":[{"title": "House of Gucci",
     "description": "Spanning three decades of love, betrayal, decadence, revenge, and ultimately murder, we see what a name means, what it&apos;s worth, and how far a family  will go for control."}],
    }
  
## Beta Developers Access Levels

1. **Basic Access:**
   - Query the system using our existing dataset there is no option to add your own dataset.
   
2. **Standard Access:**
   - Can Add your own dataset and query it.
   
3. **Advanced Access:**
   - Get access to our codebase.
   - Contribute to future upgrades and improvements.

4. **Admin Access:**
   - Full access to all features, including administrative controls and settings.
## Note :
Developers will be assigned in the respective levels only by considering the performance in testing. the desicion of assigning to diffrent level is managed by our team with proper scrutiny

## More Information
1. Read our architecture [architecture.md](architecture.md)
2. Read our Contributation guidelines [contribution.md](contribution.md)
3. For query or assistance reach us through technologies@4ducate.com
4. Applying for Access fill the following [Google form](https://forms.gle/tLLk6A7sD3gUV7Bz9)


