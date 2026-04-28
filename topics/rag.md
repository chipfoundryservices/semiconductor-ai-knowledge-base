# RAG, Retrieval, and Knowledge Bases

**Keywords**: rag,retrieval,knowledge base

---

**RAG, Retrieval, and Knowledge Bases**

> A comprehensive technical guide with mathematical foundations

---

**1. Overview**

**RAG (Retrieval-Augmented Generation)** is an architecture that enhances Large Language Models (LLMs) by grounding their responses in external knowledge sources.

**Core Components**

- **Generator**: The LLM that produces the final response
- **Retriever**: The system that finds relevant documents
- **Knowledge Base**: The corpus of documents being searched

---

**2. Mathematical Foundations**

**2.1 Vector Embeddings**

Documents and queries are converted to dense vectors in $\mathbb{R}^d$ where $d$ is the embedding dimension (typically 384, 768, or 1536).

**Embedding Function:**

$$
E: \text{Text} \rightarrow \mathbb{R}^d
$$

For a document $D$ and query $Q$:

$$
\vec{d} = E(D) \in \mathbb{R}^d
$$

$$
\vec{q} = E(Q) \in \mathbb{R}^d
$$

**2.2 Similarity Metrics**

**Cosine Similarity**

$$
\text{sim}_{\cos}(\vec{q}, \vec{d}) = \frac{\vec{q} \cdot \vec{d}}{\lVert \vec{q} \rVert \cdot \lVert \vec{d} \rVert} = \frac{\sum_{i=1}^{d} q_i \cdot d_i}{\sqrt{\sum_{i=1}^{d} q_i^2} \cdot \sqrt{\sum_{i=1}^{d} d_i^2}}
$$

**Euclidean Distance (L2)**

$$
\text{dist}_{L2}(\vec{q}, \vec{d}) = \lVert \vec{q} - \vec{d} \rVert_2 = \sqrt{\sum_{i=1}^{d} (q_i - d_i)^2}
$$

**Dot Product**

$$
\text{sim}_{\text{dot}}(\vec{q}, \vec{d}) = \vec{q} \cdot \vec{d} = \sum_{i=1}^{d} q_i \cdot d_i
$$

**2.3 BM25 (Sparse Retrieval)**

$$
\text{BM25}(Q, D) = \sum_{i=1}^{n} \text{IDF}(q_i) \cdot \frac{f(q_i, D) \cdot (k_1 + 1)}{f(q_i, D) + k_1 \cdot \left(1 - b + b \cdot \frac{|D|}{\text{avgdl}}\right)}
$$

Where:

- $f(q_i, D)$ = frequency of term $q_i$ in document $D$
- $\lvert D \rvert$ = document length
- $\text{avgdl}$ = average document length in corpus
- $k_1$ = term frequency saturation parameter (typically 1.2–2.0)
- $b$ = length normalization parameter (typically 0.75)

**Inverse Document Frequency (IDF):**

$$
\text{IDF}(q_i) = \ln\left(\frac{N - n(q_i) + 0.5}{n(q_i) + 0.5} + 1\right)
$$

Where:

- $N$ = total number of documents
- $n(q_i)$ = number of documents containing $q_i$

---

**3. RAG Pipeline Architecture**

**3.1 Pipeline Stages**

1. **Indexing Phase**
   - Document ingestion
   - Chunking strategy selection
   - Embedding generation
   - Vector storage

2. **Query Phase**
   - Query embedding: $\vec{q} = E(Q)$
   - Top-$k$ retrieval: $\mathcal{D}_k = \text{argmax}_{D \in \mathcal{C}}^k \text{sim}(\vec{q}, \vec{d})$
   - Context assembly
   - LLM generation

**3.2 Retrieval Formula**

Given a query $Q$ and corpus $\mathcal{C}$, retrieve top-$k$ documents:

$$
\mathcal{D}_k = \{D_1, D_2, ..., D_k\} \quad \text{where} \quad \text{sim}(Q, D_1) \geq \text{sim}(Q, D_2) \geq ... \geq \text{sim}(Q, D_k)
$$

**3.3 Generation with Context**

$$
P(\text{Response} | Q, \mathcal{D}_k) = \text{LLM}(Q \oplus \mathcal{D}_k)
$$

Where $\oplus$ denotes context concatenation.

---

**4. Chunking Strategies**

**4.1 Fixed-Size Chunking**

- **Chunk size**: $c$ tokens (typically 256–1024)
- **Overlap**: $o$ tokens (typically 10–20% of $c$)

$$
\text{Number of chunks} = \left\lceil \frac{\lvert D \rvert - o}{c - o} \right\rceil
$$

**4.2 Semantic Chunking**

- Split by semantic boundaries (paragraphs, sections)
- Use sentence embeddings to detect topic shifts
- Threshold: $\theta$ for similarity drop detection

$$
\text{Split at } i \quad \text{if} \quad \text{sim}(s_i, s_{i+1}) < \theta
$$

**4.3 Recursive Chunking**

- Hierarchical splitting: Document → Sections → Paragraphs → Sentences
- Maintains context hierarchy

---

**5. Knowledge Base Design**

**5.1 Metadata Schema**

```json
{
  "chunk_id": "string",
  "document_id": "string",
  "content": "string",
  "embedding": "vector[d]",
  "metadata": {
    "source": "string",
    "title": "string",
    "author": "string",
    "date_created": "ISO8601",
    "date_modified": "ISO8601",
    "section": "string",
    "page_number": "integer",
    "chunk_index": "integer",
    "total_chunks": "integer",
    "tags": ["string"],
    "confidence_score": "float"
  }
}
```

**5.2 Index Types**

- **Flat Index**: Exact search, $O(n)$ complexity
- **IVF (Inverted File)**: Approximate, $O(\sqrt{n})$ complexity
- **HNSW (Hierarchical Navigable Small World)**: Graph-based, $O(\log n)$ complexity

**HNSW Search Complexity:**

$$
O(d \cdot \log n)
$$

Where $d$ is embedding dimension and $n$ is corpus size.

---

**6. Evaluation Metrics**

**6.1 Retrieval Metrics**

**Recall@k**

$$
\text{Recall@}k = \frac{\lvert \text{Relevant} \cap \text{Retrieved@}k \rvert}{\lvert \text{Relevant} \rvert}
$$

**Precision@k**

$$
\text{Precision@}k = \frac{\lvert \text{Relevant} \cap \text{Retrieved@}k \rvert}{k}
$$

**Mean Reciprocal Rank (MRR)**

$$
\text{MRR} = \frac{1}{\lvert Q \rvert} \sum_{i=1}^{\lvert Q \rvert} \frac{1}{\text{rank}_i}
$$

**Normalized Discounted Cumulative Gain (NDCG)**

$$
\text{DCG@}k = \sum_{i=1}^{k} \frac{2^{\text{rel}_i} - 1}{\log_2(i + 1)}
$$

$$
\text{NDCG@}k = \frac{\text{DCG@}k}{\text{IDCG@}k}
$$

**6.2 Generation Metrics**

- **Faithfulness**: Is response grounded in retrieved context?
- **Relevance**: Does response answer the query?
- **Groundedness Score**:

$$
G = \frac{\lvert \text{Claims supported by context} \rvert}{\lvert \text{Total claims} \rvert}
$$

---

**7. Advanced Techniques**

**7.1 Hybrid Search**

Combine dense and sparse retrieval:

$$
\text{score}_{\text{hybrid}} = \alpha \cdot \text{score}_{\text{dense}} + (1 - \alpha) \cdot \text{score}_{\text{sparse}}
$$

Where $\alpha \in [0, 1]$ is the weighting parameter.

**7.2 Reranking**

Apply cross-encoder reranking to top-$k$ results:

$$
\text{score}_{\text{rerank}}(Q, D) = \text{CrossEncoder}(Q, D)
$$

Cross-encoder complexity: $O(k \cdot \lvert Q \rvert \cdot \lvert D \rvert)$

**7.3 Query Expansion**

- **HyDE (Hypothetical Document Embeddings)**:

$$
\vec{q}_{\text{HyDE}} = E(\text{LLM}(Q))
$$

- **Multi-Query Retrieval**:

$$
\mathcal{D}_{\text{merged}} = \bigcup_{i=1}^{m} \text{Retrieve}(Q_i)
$$

**7.4 Contextual Compression**

Reduce retrieved context before generation:

$$
C_{\text{compressed}} = \text{Compress}(\mathcal{D}_k, Q)
$$

---

**8. Vector Database Options**

| Database | Index Types | Hosting | Scalability |
|----------|-------------|---------|-------------|
| Pinecone | HNSW, IVF | Cloud | High |
| Weaviate | HNSW | Self/Cloud | High |
| Qdrant | HNSW | Self/Cloud | High |
| Milvus | IVF, HNSW | Self/Cloud | Very High |
| FAISS | Flat, IVF, HNSW | Self | Medium |
| Chroma | HNSW | Self | Low-Medium |
| pgvector | IVFFlat, HNSW | Self | Medium |

---

**9. Best Practices Checklist**

- [ ] Choose appropriate chunk size based on content type
- [ ] Implement chunk overlap to preserve context
- [ ] Store rich metadata for filtering
- [ ] Use hybrid search for better recall
- [ ] Implement reranking for precision
- [ ] Monitor retrieval metrics continuously
- [ ] Evaluate groundedness of generated responses
- [ ] Handle edge cases (no results, low confidence)
- [ ] Implement caching for common queries
- [ ] Version control your knowledge base

---

**10. Code Examples**

**10.1 Cosine Similarity (Python)**

```python
import numpy as np

def cosine_similarity(vec_q: np.ndarray, vec_d: np.ndarray) -> float:
    """
    Calculate cosine similarity between two vectors.

    $$\text{sim}_{\cos}(\vec{q}, \vec{d}) = \frac{\vec{q} \cdot \vec{d}}{\lVert \vec{q} \rVert \cdot \lVert \vec{d} \rVert}$$
    """
    dot_product = np.dot(vec_q, vec_d)
    norm_q = np.linalg.norm(vec_q)
    norm_d = np.linalg.norm(vec_d)
    return dot_product / (norm_q * norm_d)
```

**10.2 BM25 Implementation**

```python
import math
from collections import Counter

def bm25_score(
    query_terms: list[str],
    document: list[str],
    corpus: list[list[str]],
    k1: float = 1.5,
    b: float = 0.75
) -> float:
    """
    Calculate BM25 score for a query-document pair.
    """
    doc_len = len(document)
    avg_doc_len = sum(len(d) for d in corpus) / len(corpus)
    doc_freq = Counter(document)
    N = len(corpus)

    score = 0.0
    for term in query_terms:
**Document frequency**
        n_q = sum(1 for d in corpus if term in d)

**IDF calculation**
        idf = math.log((N - n_q + 0.5) / (n_q + 0.5) + 1)

**Term frequency in document**
        f_q = doc_freq.get(term, 0)

**BM25 term score**
        numerator = f_q * (k1 + 1)
        denominator = f_q + k1 * (1 - b + b * (doc_len / avg_doc_len))

        score += idf * (numerator / denominator)

    return score
```

---

**References**

1. Lewis, P., et al. (2020). "Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks"
2. Robertson, S., & Zaragoza, H. (2009). "The Probabilistic Relevance Framework: BM25 and Beyond"
3. Johnson, J., et al. (2019). "Billion-scale similarity search with GPUs" (FAISS)
4. Malkov, Y., & Yashunin, D. (2018). "Efficient and robust approximate nearest neighbor search using HNSW"

---

*Document generated for VS Code with KaTeX/LaTeX math support. Render with Markdown Preview Enhanced or similar extension.*

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/rag) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
