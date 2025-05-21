# Personalized User Recommendation System using HNSW

## Overview
This project is a vector search engine built using the Hierarchical Navigable Small World (HNSW) algorithm for fast approximate nearest neighbor search over high-dimensional vectors. It indexes user vectors representing their interests and personality traits to enable quick and efficient similarity matching.

<br/>
## Project Goals

Provide fast and scalable vector similarity search.
Support dynamic addition and deletion of user vectors.
Allow querying for nearest neighbors given an input vector.
Maintain a structured index for real-time vector matching.

<br/>
## Architecture

The core of this project is the Hierarchical Navigable Small World (HNSW) index, a graph-based data structure optimized for efficient Approximate Nearest Neighbor (ANN) search in high-dimensional spaces.

### Key Components
1. User Vectors
   Each user’s vector is constructed by combining their responses to a structured questionnaire designed to capture multiple aspects of their personality, interests, and communication style. This vector serves as a numerical representation of the user for similarity search.
  Questionnaire Sections-
  About the user:
  Users rate statements about their communication and them as a person on a scale from 0 to 5. These numerical responses become part of the vector as continuous values.

  Interests:
  Users select from a list of interests such as Literature, Art, Music, Sports, Technology, and more. Each interest corresponds to a binary value (1 if selected, 0 otherwise) in the vector.
  
  Personality:
  Users rate various personality traits (e.g., empathy, leadership, conversational style) on a scale from 0 to 5. These again become continuous values within the vector.

  Example of a final user document-
  ```
  {
    "user_id": 5,
    "vector": [
      0.49, 0.27, 0.66, 0.30, 0.60, # about the user
      1, 0, 1, 0, 1, 0, 1, 0, # interests
      0.52, 0.32, 0.67, 0.74, 0.37, 0.23, 0.42 # personality
    ]
  }
  ```
3. HNSW Index
   The HNSW index maintains a multi-layer navigable small-world graph where each node corresponds to a user vector.
   - Multi-layer Graph:
   Higher layers act as shortcuts that accelerate the search, while the bottom layer holds all vectors for detailed search.

   - Navigability:
   The graph structure allows quick traversal to approximate nearest neighbors by hopping between nodes connected through short edges.

   - Dynamic Updates:
   Supports inserting new vectors and marking existing vectors as deleted without requiring a full rebuild.

   - Search:
   Given a query vector, the index efficiently finds a set of nearest user vectors by exploring the graph structure starting from an entry point.

4. Operations and Data Flow
   - Adding Users:
     Users provide their vector and a unique user ID, which is added to the index as a new node.
   - Deleting users:
     User vectors can be soft deleted (marked) in the index by their IDs to exclude them from future search results.
   - Querying:
     The system accepts input vectors and performs approximate nearest neighbor searches returning the closest user vectors.

<br/>
## Benefits of this architecture
- Scalability:
The graph-based HNSW allows scaling to millions of vectors with sub-linear query times.

- Real-time Updates:
Supports incremental additions and deletions without downtime or expensive re-indexing.

- Accuracy and Speed Tradeoff:
Approximate search offers a tunable balance between query speed and result accuracy.

- Modular Design:
Separating vector storage from the search index allows flexibility in data management and integration.
<br/>

## Future Improvements
- Persist the HNSW index to disk for faster startup.
- Add a REST API for external integrations.
- Enable batch processing of vectors.
- Enhance input validation and error handling.
- Optimize performance for very large datasets.
   
