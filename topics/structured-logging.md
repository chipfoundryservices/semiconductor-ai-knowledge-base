# Structured Logging

**Keywords**: structured logging,json,searchable

---

**Structured Logging** is the **practice of emitting log records as machine-parseable structured data (typically JSON) rather than unstructured human-readable text** — enabling powerful querying, aggregation, alerting, and analysis of AI system behavior, performance, and errors using SQL-like queries and dashboards rather than brittle string parsing and grep-based log hunting.

**What Is Structured Logging?**

- **Definition**: A logging approach where each log entry is a structured data object with defined fields (timestamp, level, message, request_id, user_id, model, latency_ms, token_count) rather than a free-form text string — making log data queryable like a database table.
- **Contrast with Unstructured Logging**:
  - Unstructured: `[INFO 2024-01-15 10:32:15] Model predicted 'cat' with 0.92 confidence in 145ms`
  - Structured: `{"timestamp": "2024-01-15T10:32:15Z", "level": "INFO", "event": "prediction", "class": "cat", "confidence": 0.92, "latency_ms": 145, "model_version": "v4.2", "request_id": "req_abc123"}`
- **Queryable**: Structured logs can be queried with SQL-like syntax — SELECT AVG(latency_ms) WHERE model_version = 'v4.2' AND confidence > 0.9 — impossible with unstructured text.
- **Industry Standard**: Modern observability platforms (Datadog, Splunk, Elasticsearch, CloudWatch Logs Insights) natively query structured JSON logs.

**Why Structured Logging Matters for AI Systems**

- **Performance Analysis**: Query `AVG(llm_latency_ms) GROUP BY model_name` to compare model performance across versions — impossible without structured fields.
- **Error Diagnosis**: Filter `WHERE error_type = 'rate_limit' AND retry_count > 3` to identify systematic retry failures — requires structured error fields.
- **Cost Monitoring**: Aggregate `SUM(input_tokens + output_tokens) GROUP BY user_id, DATE` for per-user token cost accounting — requires token count fields in every log.
- **Hallucination Tracking**: Log fact-check results structurally — `{"event": "fact_check", "result": "failed", "claim": "...", "source_contradiction": "..."}` — then query failure rates over time.
- **Alerting**: Alert on error_rate > 0.05 WHERE model = 'gpt-4o' or P95_latency > 5000 — requires numeric fields in structured log data.
- **Audit Compliance**: Reconstruct complete request histories for compliance audits by querying structured logs filtered by user_id, request_id, or date range.

**Structured Logging Implementation**

**Python with structlog (Recommended)**:
```python
import structlog
from datetime import datetime

logger = structlog.get_logger()

def process_llm_request(request_id: str, user_id: str, query: str) -> str:
    start_time = datetime.utcnow()

    try:
        response = llm.generate(query)
        duration_ms = (datetime.utcnow() - start_time).total_seconds() * 1000

        logger.info(
            "llm_request_completed",
            request_id=request_id,
            user_id=user_id,
            model="gpt-4o",
            input_tokens=count_tokens(query),
            output_tokens=count_tokens(response),
            latency_ms=round(duration_ms),
            success=True
        )
        return response

    except RateLimitError as e:
        logger.warning(
            "llm_rate_limit",
            request_id=request_id,
            user_id=user_id,
            retry_after=e.retry_after,
            success=False
        )
        raise
```

**Output JSON**:
```json
{
  "timestamp": "2024-01-15T10:32:15.234Z",
  "level": "info",
  "event": "llm_request_completed",
  "request_id": "req_abc123",
  "user_id": "usr_456",
  "model": "gpt-4o",
  "input_tokens": 342,
  "output_tokens": 187,
  "latency_ms": 1847,
  "success": true
}
```

**Key Fields for AI System Logs**

| Field | Type | Purpose |
|-------|------|---------|
| timestamp | ISO 8601 | Time correlation |
| request_id | UUID | Request tracing |
| user_id | String | Per-user analysis |
| session_id | String | Conversation tracking |
| event | String | Log type classification |
| model | String | Model version tracking |
| input_tokens | Integer | Cost accounting |
| output_tokens | Integer | Cost accounting |
| latency_ms | Integer | Performance monitoring |
| retry_count | Integer | Reliability tracking |
| error_type | String | Error classification |
| rag_chunks_retrieved | Integer | RAG performance |
| confidence | Float | Quality tracking |
| success | Boolean | Success rate monitoring |

**Log Level Strategy for AI Systems**

- **DEBUG**: Full prompts and responses (development only — high volume, PII risk).
- **INFO**: Request completion with token counts, latency, model version.
- **WARNING**: Retries, rate limits, format corrections, low-confidence outputs.
- **ERROR**: Failed requests after max retries, validation failures, unexpected exceptions.
- **CRITICAL**: Service-wide failures, circuit breaker trips, data loss events.

**Structured Log Querying Examples**

In CloudWatch Logs Insights:
```sql
-- Average latency by model
fields @timestamp, model, latency_ms
| filter event = "llm_request_completed"
| stats avg(latency_ms) as avg_latency by model
| sort avg_latency desc

-- Error rate by hour
filter success = 0
| stats count() as errors by bin(1h)

-- Token cost by user (top 10)
filter event = "llm_request_completed"
| stats sum(input_tokens + output_tokens) as total_tokens by user_id
| sort total_tokens desc
| limit 10
```

**PII Handling in Logs**

AI system logs must handle personally identifiable information carefully:
- Never log raw user query content in production without PII scrubbing.
- Log query metadata (length, topic classification) rather than content.
- Apply field-level encryption or masking for sensitive structured fields.
- Ensure log retention policies comply with GDPR, CCPA data deletion requirements.
- Use separate log streams for high-sensitivity data with stricter access controls.

Structured logging is **the observability foundation that transforms AI systems from black boxes into monitorable, debuggable, and auditable production infrastructure** — by emitting machine-parseable structured data from every significant operation, teams gain the ability to answer operational questions — why did that request fail, which model version is slower, which users are approaching token limits — with queries rather than grep, enabling data-driven AI operations at scale.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/structured-logging) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
