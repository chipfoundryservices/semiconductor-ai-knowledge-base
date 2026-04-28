# System design for LLM applications

**Keywords**: system design, architecture, scaling, load balancing, caching, reliability, llm infrastructure

---

**System design for LLM applications** involves **architecting scalable, reliable infrastructure to serve AI capabilities to users** — addressing unique challenges like variable latency, high memory requirements, and non-deterministic outputs while applying traditional system design principles for load balancing, caching, and fault tolerance.

**What Is LLM System Design?**

- **Definition**: Architecture for production LLM serving at scale.
- **Challenges**: High latency, GPU costs, variable load.
- **Goals**: Reliability, performance, cost efficiency.
- **Approach**: Adapt traditional patterns for AI constraints.

**Why LLM System Design Differs**

- **Resource Intensive**: Single request may use 24GB+ GPU memory.
- **Variable Latency**: Responses take 100ms to 30s depending on length.
- **Stateful Conversations**: Context must be maintained across requests.
- **Non-Deterministic**: Same input can produce different outputs.
- **Expensive Operations**: Each token costs money.

**High-Level Architecture**

```
┌─────────────────────────────────────────────────────────────┐
│                        Clients                              │
│              (Web, Mobile, API consumers)                   │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                    Load Balancer                            │
│              (nginx, AWS ALB, CloudFlare)                   │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│                    API Gateway                              │
│    - Rate limiting                                          │
│    - Authentication                                         │
│    - Request routing                                        │
└─────────────────────────────────────────────────────────────┘
                            │
              ┌─────────────┼─────────────┐
              ▼             ▼             ▼
┌─────────────────┐ ┌─────────────┐ ┌─────────────────┐
│  Cache Layer    │ │ RAG Service │ │  LLM Service    │
│  (Redis)        │ │ (Retrieval) │ │  (vLLM/TGI)     │
└─────────────────┘ └─────────────┘ └─────────────────┘
                            │             │
                            ▼             ▼
                    ┌─────────────┐ ┌─────────────────┐
                    │ Vector DB   │ │  GPU Cluster    │
                    │ (Pinecone)  │ │  (H100s)        │
                    └─────────────┘ └─────────────────┘
```

**Key Components**

**API Layer**:
```python
from fastapi import FastAPI, HTTPException
from fastapi.middleware.cors import CORSMiddleware
import asyncio

app = FastAPI()

@app.post("/v1/chat/completions")
async def chat_completion(request: ChatRequest):
    # Validate
    if len(request.messages) == 0:
        raise HTTPException(400, "Messages required")
    
    # Check cache
    cache_key = hash_request(request)
    if cached := await cache.get(cache_key):
        return cached
    
    # Route to appropriate model
    model_endpoint = get_model_endpoint(request.model)
    
    # Generate (with timeout)
    try:
        response = await asyncio.wait_for(
            generate(model_endpoint, request),
            timeout=60.0
        )
    except asyncio.TimeoutError:
        raise HTTPException(504, "Generation timeout")
    
    # Cache and return
    await cache.set(cache_key, response, ttl=3600)
    return response
```

**Scaling Strategies**

**Horizontal Scaling**:
```
Load Pattern          | Strategy
----------------------|----------------------------------
Bursty traffic        | Auto-scaling GPU instances
Predictable peaks     | Scheduled scaling
Global users          | Multi-region deployment
Cost optimization     | Spot instances + fallback
```

**Caching Layers**:
```
Layer              | Cache What           | TTL
-------------------|----------------------|----------
Response cache     | Full responses       | 1-24 hours
Embedding cache    | Vector embeddings    | Days
KV cache           | Attention states     | Session
Prefix cache       | System prompts       | Hours
```

**Multi-Model Routing**

```python
def route_request(request):
    """Route to appropriate model based on complexity."""
    
    # Simple queries → small/fast model
    if is_simple_query(request):
        return "gpt-4o-mini"
    
    # Complex reasoning → large model
    if needs_complex_reasoning(request):
        return "gpt-4o"
    
    # Default
    return "gpt-4o-mini"

def is_simple_query(request):
    prompt = request.messages[-1].content
    return (
        len(prompt) < 100 and
        not any(word in prompt for word in 
                ["explain", "analyze", "compare"])
    )
```

**Reliability Patterns**

**Circuit Breaker**:
```python
class CircuitBreaker:
    def __init__(self, failure_threshold=5, reset_timeout=60):
        self.failures = 0
        self.state = "closed"
        self.last_failure = None
    
    async def call(self, func, *args):
        if self.state == "open":
            if time.time() - self.last_failure > self.reset_timeout:
                self.state = "half-open"
            else:
                raise CircuitOpenError()
        
        try:
            result = await func(*args)
            self.failures = 0
            self.state = "closed"
            return result
        except Exception as e:
            self.failures += 1
            self.last_failure = time.time()
            if self.failures >= self.failure_threshold:
                self.state = "open"
            raise
```

**Fallback Chain**:
```python
async def generate_with_fallback(request):
    providers = ["openai", "anthropic", "local"]
    
    for provider in providers:
        try:
            return await generate(provider, request)
        except Exception as e:
            logger.warning(f"{provider} failed: {e}")
            continue
    
    raise AllProvidersFailedError()
```

**Monitoring & Observability**

```
Metric              | What to Track
--------------------|--------------------------------
Latency (P50/P95)   | TTFT, total generation time
Throughput          | Requests/sec, tokens/sec
Error rate          | 4xx, 5xx, timeouts
GPU utilization     | Memory, compute usage
Cost                | Tokens per request, $/query
```

System design for LLM applications requires **balancing performance, reliability, and cost** — applying proven distributed systems patterns while adapting to the unique constraints of GPU-bound, high-latency inference workloads.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/system-design) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
