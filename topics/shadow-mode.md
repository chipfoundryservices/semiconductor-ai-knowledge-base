# Shadow mode deployment

**Keywords**: shadow mode, canary deployment, a b testing, model comparison, safe rollout, production testing

---

**Shadow mode deployment** runs **new models alongside production without affecting user experience** — sending traffic to both old and new models, comparing outputs, and validating performance before fully switching, enabling safe validation of model changes in real production conditions.

**What Is Shadow Mode?**

- **Definition**: New model receives production traffic but doesn't serve responses.
- **Purpose**: Validate model behavior with real data before launch.
- **Mechanism**: Duplicate requests to shadow model, compare results.
- **Risk**: None to users — only production model serves responses.

**Why Shadow Mode Matters**

- **Real Traffic**: Test patterns that synthetic data misses.
- **Performance**: Measure latency under production load.
- **Quality**: Compare outputs at scale.
- **Confidence**: Build evidence before full rollout.
- **Rollback-Free**: Issues don't affect users.

**Shadow Mode Architecture**

```
User Request
     │
     ▼
┌─────────────────────────────────────────────────────────┐
│                    API Gateway                          │
└─────────────────────────────────────────────────────────┘
     │
     ├──────────────────────────┐
     │                          │ (async)
     ▼                          ▼
┌─────────────────────┐  ┌─────────────────────┐
│  Production Model   │  │   Shadow Model      │
│  (serves response)  │  │   (logs only)       │
└─────────────────────┘  └─────────────────────┘
     │                          │
     ▼                          ▼
  [Response]              [Log for Analysis]
     │                          │
     └──────────────────────────┘
                  │
                  ▼
         ┌───────────────────┐
         │   Comparison DB   │
         └───────────────────┘
```

**Implementation**

**Basic Shadow Proxy**:
```python
import asyncio
from fastapi import FastAPI, Request

app = FastAPI()

async def call_production(request):
    """Call production model and return response."""
    return await production_model.generate(request)

async def call_shadow(request):
    """Call shadow model and log result."""
    try:
        result = await shadow_model.generate(request)
        await log_shadow_result(request, result)
    except Exception as e:
        logger.error(f"Shadow model error: {e}")

@app.post("/v1/generate")
async def generate(request: Request):
    body = await request.json()
    
    # Start shadow call (don't await)
    asyncio.create_task(call_shadow(body))
    
    # Return production response
    response = await call_production(body)
    return response
```

**Traffic Splitting**:
```python
import random

def should_shadow(request, shadow_percentage=10):
    """Determine if request should be shadowed."""
    return random.random() < shadow_percentage / 100

@app.post("/v1/generate")
async def generate(request: Request):
    body = await request.json()
    
    # Only shadow some traffic
    if should_shadow(body, shadow_percentage=25):
        asyncio.create_task(call_shadow(body))
    
    return await call_production(body)
```

**Comparison Analysis**

**Metrics to Compare**:
```
Metric               | How to Compare
---------------------|----------------------------------
Latency              | Shadow P50/P95 vs. production
Output match         | Exact match rate
Semantic similarity  | Embedding similarity of outputs
Error rate           | Shadow failure rate
Token usage          | Cost comparison
Quality              | LLM-as-judge or human eval
```

**Comparison Script**:
```python
def analyze_shadow_results():
    results = load_shadow_comparisons()
    
    analysis = {
        "total_samples": len(results),
        "exact_match_rate": sum(r["exact_match"] for r in results) / len(results),
        "avg_similarity": sum(r["semantic_similarity"] for r in results) / len(results),
        "shadow_latency_p50": percentile([r["shadow_latency"] for r in results], 50),
        "shadow_latency_p95": percentile([r["shadow_latency"] for r in results], 95),
        "prod_latency_p50": percentile([r["prod_latency"] for r in results], 50),
        "shadow_error_rate": sum(r["shadow_error"] for r in results) / len(results),
    }
    
    return analysis
```

**Automated Quality Check**:
```python
async def evaluate_shadow_quality(prod_response, shadow_response, prompt):
    """Use LLM to judge which response is better."""
    judge_prompt = f"""
    Compare these two responses to the prompt.
    
    Prompt: {prompt}
    
    Response A: {prod_response}
    Response B: {shadow_response}
    
    Which is better? Answer: A, B, or TIE
    Brief justification:
    """
    
    judgment = await judge_llm.generate(judge_prompt)
    return parse_judgment(judgment)
```

**Rollout Decision**

**Go/No-Go Criteria**:
```
Metric               | Threshold
---------------------|------------------
Latency (P95)        | < 1.2x production
Error rate           | < production
Quality win rate     | > 50%
Semantic similarity  | > 0.95
Shadow coverage      | > 10K requests
```

**Gradual Rollout**:
```
Phase 1: Shadow 5% → validate
Phase 2: Shadow 25% → validate  
Phase 3: Shadow 100% → validate
Phase 4: Canary 5% real traffic
Phase 5: Gradual 5% → 25% → 50% → 100%
```

**Best Practices**

- **Sample Traffic**: Don't shadow 100% if not needed.
- **Async Execution**: Shadow shouldn't slow production.
- **Cost Awareness**: Shadow traffic costs money.
- **Time-Bound**: Set duration for shadow experiment.
- **Automated Alerts**: Notify on significant differences.

Shadow mode deployment is **the safest way to validate model changes** — by running new models against real production traffic without user impact, teams can catch issues that testing missed and build confidence before committing to a full rollout.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/shadow-mode) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
