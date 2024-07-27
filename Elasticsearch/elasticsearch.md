# Elasticsearch

# Introduction

- Powerful search engine
- Near real-time search and analytics capabilities
- Full-text search
- Distributed architecture
- RESTful API

# Ecosystem

### 1. Elasticsearch

### 2. Kibana

### 3. Logstash

### 4. Beats

# New Terms

RDBMS

- Databases → Tables → Columns/Rows

Elasticsearch

- Clusters → Indexes → Shards → Documents (key-val)

# Why Elasticsearch so fast?

## 1. Build on top of Apache Lucene - Search engine library

- Directory abstraction
    - Lucene uses a `Directory` abtraction to represent a location where index files are stored (normaly, it is stored in RAM) ⇒ that ‘s why Elasticsearch need a lot of RAM to have  the best performance
- Index Files
    - Within the specified directory, Lucene stores its index data in a series of files
    - These files contain the inverted index, stored fileds, and other necessary data for Lucene’s operation
- Inverted index
    - Elasticsearch uses a data structure called an `inverted index` that support very fast full-text search
    - An inverted index lists every unique word that appears in any document and identifies all of the documents each word occurs in
    - Ex
        - The Best Pasta Recipe with Pesto
        - Delicious Pasta Carbonara Recipe
            
            ![Screenshot 2024-07-06 at 03.40.08.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/9fcc8c66-1ae3-4968-8c12-c965bd33059f/81a52446-483e-49b8-bf35-3cc5fd4c957f/Screenshot_2024-07-06_at_03.40.08.png)
            

## 2. Architecture

### `_Cluster`

- A cluster is a collection of one or more nodes that together hold the entire data and provide indexing and search capability
- A cluster can scale horizontally by adding more nodes, allowing it to handle larger datasets and more search requests
- Cluster can be replicated to avoid single point of failure

### `_Node`

- Node is a single instance in Elasticsearch
- Node has a unique ID and a name
- Node belong to a single cluster
- Node can belong to multiple roles
    
    ![Screenshot 2024-07-03 at 00.45.50.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/9fcc8c66-1ae3-4968-8c12-c965bd33059f/b3827a89-f088-4c8c-be0a-bdba974573e8/Screenshot_2024-07-03_at_00.45.50.png)
    

⇒ Multiple nodes can working in parallel ⇒ Improve store capacity and query perfomance

Ex:

- 100k request
    - 1 Node takes 5s
    - 5 Nodes take 1s

### `_Shard`

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/9fcc8c66-1ae3-4968-8c12-c965bd33059f/04b9e91c-6486-4472-994a-6b741d43042f/Untitled.png)

- The shard is where is data is stored on disk
- An index can have multiple shard, distributing across multiple Node in the cluster → Increase query capacity, redundancy
- If cluster grows (or shrinks), Elasticsearch automatically migrates shards to rebalance the cluster
- Type of shards
    - Primary
    - Replica
- The size of the Shard limited by the capacity of the Node, best practice is less than 200M docs and size between 10GB and 50GB
- Example:
    - I have index 600k docs about order in my cluster
    - But each node can only hold 200k docs
    - I can use 3 nodes to hold all of this

Sharding can speed up your search

Shard your index into multiple nodes, each node can process parallels, at the same time ⇒ It can increase the search speed

Replica shards

The shards can be replica into multiple node to increase the availability and search performance

### `_Index`

- Index not store data
- Index keep track where the documents are stored

# Node

## 1. Roles

- Roles represent the functions and responsibilities of the node
- You can define a node’s roles by setting `node.roles` in the `elasticsearch.yml`
- If you set the node.roles, the node is only assigned the roles you specify
- If you don’t, the node is assigned the following roles
    - master, data, data_hot, data_warm, data_cold, ingest, ml, etc.

### `_Coordinating`

- Receive the request from client
- Request analysis
    - The node analyzes the request to understand what type of operation it is (search, indexing, deletion,etc.) and which data is involved
- Routing to Data Node
- Gathering results
    - The node gethers the individual results from the data nodes
- Response to the client

### `_Master`

- The master node is responsible for lightweight cluster-wide actions such as creating or deleting an index
- Tracking which nodes are part of the cluster, and deciding which shards to allocate which nodes
- `Dedicated master-sligible node`
    - A node that is configured to only serve as a master node and not to handle data (indexing and searching) or other roles
    - When a node is both master-eligible and handles data, it can become a bottleneck
    
    → Sepreating the master role from other roles can improve the stability and performance of the cluster
    

### `_Data`

- Data nodes hold the shards that contain the documents you have indexed
- Data node handled data related operations like CRUD, search, and aggregations
- It is important to monitor these resources and to add more data nodes if they are overloaded

## 2. Data in node

- Data is stored as documents
- It is the JSON object that stored in Elasticsearch under a uniqueID
- Multiple tier
    - data_content
    - data_hot
    - data_warm
    - data_cold
    - ingest

# Relevance ranking

![Screenshot 2024-07-05 at 15.28.24.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/9fcc8c66-1ae3-4968-8c12-c965bd33059f/72346c40-3d20-430b-8519-a169f649e228/Screenshot_2024-07-05_at_15.28.24.png)

Rank the search results by their similarity to a given search query ⇒ `_score`

Documents with high score, that’s mean the more revelance with the input

- Decision
- Recall

# Key features

## 1. Queries

Retrieve documents that match the criteria /kraɪˈtɪriən/

- Searching for terms in a field
    - Retrieve documents that contrain the search terms
- Searching for a phrase in a field
    - Option 1
        - Retrieve documents that contrain one word or multiple words in the given phrase
    - Option 2
        - Retrieve documents that contrain the given phrase
- Searching on multiple fields
- Combined Queries
    - Retrieve documents that matching combinations of other queries

## 2. Aggregations

Summarizes your data as metrics, statistics, and other analytics

- `Metric`aggregations are used to compute numeric values based on your dataset. It can be used to calculate the values of `sum`,`min`, `max`, `avg`, unique count(`cardinality`) and etc.
- Terms Aggregation
    - Get top 5 users with the highest number of transactions
- Computes the count of unique values for a given field
- Limiting the scope of an aggregation by add more query condition
- **`Date Histogram` Aggregation (STRONG)**
    - Aggregate data in an interval time
- Range Aggregation
    - from x value to y value in the field z
- Combined Aggregations
    - `metric aggregations` with `bucket aggregation`
    - Ex
        - Calculate the sum of revenue per day
        - Step 1: Using bucket aggregation to group data into daily bucket
        - Step 2: Using metric aggregation to sum

## 3. Mapping

# Pros and Cons

- `Resource intensive`
    - Hardware requirements: Elasticsearch can be resource-intensive, requiring sufficient memory, CPU, and storage resources, especially in large-scale deployments.
    - Indexing Overhead**:** Text analysis and indexing operations can be CPU and I/O intensive, affecting performance during peak loads or high indexing rates.
- `Data Consistency and Durability`
    - Eventual Consistency**:** Elasticsearch favors availability and partition tolerance over strong consistency, meaning that after updates, consistency across replicas may take time to propagate (eventual consistency).
    - Data Loss**:** In rare cases, due to node failures or network partitions during indexing operations, data loss can occur if replicas are not fully synchronized.