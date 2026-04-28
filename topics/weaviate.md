# Weaviate

**Keywords**: weaviate,vector,open

---

**Weaviate** is an **open-source vector database with built-in vectorization, hybrid search, and GraphQL API**, combining vector similarity search with traditional database features and enabling retrieval-augmented generation (RAG) workflows.

**What Is Weaviate?**

- **Definition**: Vector database with integrated vectorization and search
- **Key Feature**: Auto-vectorization (embed text on insert)
- **Hybrid Search**: Combine vector similarity with keyword search
- **API**: GraphQL for flexible querying
- **Deployment**: Docker, Kubernetes, or managed cloud
- **Use Cases**: Semantic search, RAG, recommendations, multi-modal search

**Why Weaviate Matters**

- **Built-in Vectorization**: No separate embedding step needed
- **Hybrid Search**: Unique advantage combining vector + keyword
- **Generative Search**: Integrate LLMs into search results
- **GraphQL**: Flexible query language (no fixed schema)
- **Open Source**: Self-hostable with Weaviate Cloud option
- **RAG Ready**: Designed specifically for RAG pipelines
- **Modular**: Extend with custom vectorizers and modules

**Key Features**

**Auto-Vectorization**:
- Built-in models: OpenAI, Cohere, HuggingFace
- Text automatically embedded on insertion
- No manual embedding step needed
- Configurable per class

**Hybrid Search**:
```graphql
query {
  Get {
    Article(
      hybrid: {query: "machine learning"}
      alpha: 0.5  # 0=keyword, 1=vector, 0.5=balanced
    ) {
      title
      content
      _additional {score}
    }
  }
}
```

**Generative Search (RAG)**:
```graphql
query {
  Get {
    Article(
      nearText: {concepts: ["quantum computing"]}
    ) {
      title
      content
      _additional {
        generate(
          singlePrompt: "Summarize this: {content}"
        ) {
          singleResult
        }
      }
    }
  }
}
```

**Multi-Tenancy**:
- Separate data per tenant
- Isolated security and performance
- Perfect for SaaS applications

**Vector Types**:
- Single vector per object (standard)
- Named vectors (multiple embeddings)
- Multi-modal vectors (CLIP for image+text)

**Quick Start Workflow**
```bash
# Pull image
docker pull semitechnologies/weaviate:latest

# Run with compose
docker-compose up

# Connect
curl http://localhost:8080

# Schema ready for queries immediately
```

**Python Example**
```python
import weaviate

# Connect
client = weaviate.Client("http://localhost:8080")

# Create schema
schema = {
    "class": "Article",
    "vectorizer": "text2vec-openai",  # Auto-vectorize!
    "properties": [
        {"name": "title", "dataType": ["text"]},
        {"name": "content", "dataType": ["text"]},
        {"name": "author", "dataType": ["text"]}
    ]
}
client.schema.create_class(schema)

# Add data (auto-vectorized!)
client.data_object.create(
    class_name="Article",
    data_object={
        "title": "AI in Healthcare",
        "content": "Artificial intelligence transforms medicine...",
        "author": "Jane Doe"
    }
)

# Vector search
result = client.query.get("Article", ["title", "content"]).with_near_text({
    "concepts": ["medical AI applications"]
}).with_limit(10).do()
```

**Built-in Vectorizers**

| Vectorizer | Use Case | Pros | Cons |
|-----------|----------|------|------|
| text2vec-openai | General purpose | Best quality | API costs |
| text2vec-cohere | Multilingual | Great for multi-lang | API costs |
| text2vec-huggingface | No API keys | Open source | Slower |
| text2vec-transformers | Local embeddings | Full control | Memory intensive |
| multi2vec-clip | Text + images | Multi-modal | Requires setup |

**Hybrid Search Example**
```python
# Combines keyword + vector search
result = client.query.get("Article", ["title"]).with_hybrid(
    query="machine learning",  # Keyword part
    alpha=0.5  # 50/50 keyword and vector
).do()
```

**Generative Search (RAG Example)**
```python
# Search + generate answer in one query
result = client.query.get("Article", ["title", "content"]).with_near_text({
    "concepts": ["quantum computing"]
}).with_generate(
    single_prompt="Summarize this article in 50 words: {content}"
).do()

# Result includes both search and generation
```

**Use Cases**

**Semantic Search**:
- Find similar documents
- More intuitive than keyword search
- Great for knowledge bases

**RAG (Retrieval Augmented Generation)**:
- Retrieve relevant documents
- Pass to LLM for generation
- Reduce hallucinations

**Recommendation Systems**:
- Find similar products/content
- Content-based recommendations
- User-based with embeddings

**Multi-Modal Search**:
- Search by text and images
- Medical imaging similarity
- Visual search experiences

**Knowledge Graphs**:
- Structured + vector search
- Connected data exploration
- Relationship discovery

**Customer Support**:
- Find similar past tickets
- Quick answer suggestions
- FAQ matching

**Deployment Options**

**Docker** (Single command):
```bash
docker run -p 8080:8080 semitechnologies/weaviate
```

**Kubernetes** (Cloud-native):
```bash
helm install weaviate weaviate/weaviate
```

**Weaviate Cloud** (Managed):
- Auto-scaling
- Multi-region support
- Automated backups
- Support included

**Self-Hosted** (Complete control):
- Docker, binary, or Kubernetes
- Full data privacy
- Custom configurations

**Pricing Model**

- **Open Source**: Free forever, self-hosted only
- **Weaviate Cloud Service**: $50-$999+/month based on scale
- **Enterprise**: Custom pricing with SLA

**Integration Ecosystem**

**LLM Frameworks**:
- **LangChain**: First-class Weaviate integration
- **LlamaIndex**: RAG pipeline support
- **Haystack**: Search framework integration

**Data Tools**:
- **Airflow**: Data pipeline orchestration
- **Kafka**: Event streaming
- **Databricks**: Data lakehouse integration

**APIs**:
- **REST API**: Standard HTTP queries
- **GraphQL**: Full query flexibility
- **WebSocket**: Real-time updates

**Weaviate vs Alternatives**

| Feature | Weaviate | Pinecone | Qdrant | Milvus |
|---------|----------|----------|--------|--------|
| Auto-Vectorize | ✅ | ❌ | ❌ | ❌ |
| Hybrid Search | ✅ | Limited | ✅ | ❌ |
| GraphQL | ✅ | ❌ | ❌ | ❌ |
| Generative Search| ✅ | ❌ | ❌ | ❌ |
| Open Source | ✅ | ❌ | ✅ | ✅ |
| Self-hosted | ✅ | ❌ | ✅ | ✅ |

**Best Practices**

1. **Choose Right Vectorizer**: OpenAI for quality, local for privacy
2. **Use Hybrid Search**: Combine vector + keyword for better results
3. **Query Optimization**: Use filters to reduce vector search space
4. **Module Selection**: Leverage generative search for RAG
5. **Backup Strategy**: Enable automated snapshots
6. **Monitoring**: Track query latency and success rates
7. **Schema Design**: Match use case to object structure
8. **Testing**: Validate results on representative queries

**Common Patterns**

**FAQ Search**:
1. Add FAQs as objects (vectorized automatically)
2. User asks question
3. Hybrid search finds relevant FAQ
4. Generative search summarizes answer

**Document Search**:
1. Upload documents
2. Chunk into sections
3. Each section auto-vectorized
4. Hybrid search + generate answers

**E-Commerce**:
1. Product catalog vectorized
2. User searches naturally
3. Semantic + keyword results
4. Similar product recommendations

**Customer Support**:
1. Ticket database auto-vectorized
2. New ticket compared to similar past tickets
3. Suggested solutions from similar cases
4. Team productivity boost

Weaviate is the **intelligence layer for semantic search and RAG** — unique combination of auto-vectorization, hybrid search, and generative capabilities makes it perfect for applications needing intelligent retrieval and production RAG pipelines.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/weaviate) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
