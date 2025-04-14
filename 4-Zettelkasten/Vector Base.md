
| Feature                   | Pinecone | Weaviate        | Milvus          | Qdrant          | Chroma          | ElasticSearch   |
| ------------------------- | -------- | --------------- | --------------- | --------------- | --------------- | --------------- |
| Open source               | ❌        | ✅               | ✅               | ✅               | ✅               | ❌               |
| Self-host                 | ❌        | ✅               | ✅               | ✅               | ✅               | ✅               |
| Cloud management          | ✅        | ✅               | ✅               | ✅               | ❌               | ✅               |
| Built for Vectors         | ✅        | ✅               | ✅               | ✅               | ✅               | ❌               |
| Queries per second        | 150      | 791             | 2406            | 326             | ?               | 700-1000        |
| Latency, ms               | 1        | 2               | 1               | 4               | ?               | ?               |
| Index types               | ?        | HNSW            | Multiple        | HNSW            | HNSW            | HNSW            |
| Role-based access control | ✅        | ❌               | ✅               | ❌               | ❌               | ✅               |
| Dynamic vs. Static        | ?        | Static sharding | Dynamic segment | Static sharding | Dynamic segment | Static sharding |
| Free hosted tier          | ✅        | ✅               | ✅               | ❌               | ❌               | ❌               |
| Pricing (50k vectors)     | $70      | $25             | $65             | $9              | Varies          | $95             |
| Pricing (20M vectors)     | $227     | $1536           | $309            | $281            | Varies          | $1225           |
