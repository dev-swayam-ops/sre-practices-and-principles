# Cheatsheet: Observability

Quick reference for logs, metrics, and traces.

---

## Three Pillars at a Glance

| Pillar | What | When | Cost |
|--------|------|------|------|
| **Logs** | Events, errors, details | Debugging | Medium |
| **Metrics** | Aggregated numbers | Alerting, dashboards | Low |
| **Traces** | Request flow | Performance | High |

---

## What to Log

```
ALWAYS:
✓ Errors and exceptions
✓ Important state changes (login, payment)
✓ Performance warnings (slow query > 1 sec)
✓ Security events (failed auth)

SOMETIMES:
⚠ Info events (useful but can be noise)

NEVER:
✗ Debug logs on every line
✗ Passwords or PII
✗ Full request/response bodies (too much data)
```

---

## Structured Logging Template

```json
{
  "timestamp": "2026-01-15T14:30:00Z",
  "request_id": "req-abc123",
  "service": "auth-service",
  "level": "ERROR",
  "message": "Login failed",
  "error": "password_mismatch",
  "user_id": "user-789",
  "duration_ms": 45,
  "tags": ["security"]
}
```

---

## Key Metrics to Track

```
AVAILABILITY:
- Error rate (% of requests failing)
- Availability % (SLO metric)

PERFORMANCE:
- P50, P95, P99 latency
- Request throughput (req/sec)

RESOURCE:
- Database connection pool
- Queue depth
- Cache hit rate

BUSINESS:
- Requests by feature
- Conversion rate (if applicable)
```

---

## Distributed Tracing Example

```
Request Flow:
req-123 → API (10ms)
        → Auth Service (20ms)
           → User DB (15ms)
        → Payment Service (50ms)
           → Payment DB (40ms)
           → Third-party API (500ms)
        → Response (5ms)

Total: 585ms

Finding: Third-party API is bottleneck
Action: Add timeout, retry logic, cache
```

---

## Request Correlation with request_id

```
Service 1: generates request_id = "xyz-123"
Service 2: receives, logs with request_id
Service 3: receives, logs with request_id
Service 4: receives, logs with request_id

Query logs:
  request_id="xyz-123"
  
Returns full request journey with timing
```

---

## Alert vs Observe Decision

```
IF direct user impact
  THEN ALERT (page on-call)
ELSE OBSERVE (for debugging)

Examples:
- Error rate > 0.5% → ALERT
- High latency → ALERT (user sees it)
- Specific error message → OBSERVE
- Memory usage high → OBSERVE
- Database slow → OBSERVE or ALERT
```

---

## Observability Checklist

- [ ] All logs have request_id
- [ ] Logs in structured format (JSON)
- [ ] Key metrics tracked (availability, latency)
- [ ] Distributed tracing on slow requests
- [ ] Can correlate logs/metrics/traces
- [ ] Cost-effective sampling strategy
- [ ] Log retention policy clear
- [ ] Runbook references observability

---

## Common Tools

```
LOGGING:
- ELK Stack (Elasticsearch, Logstash, Kibana)
- Datadog, New Relic
- CloudWatch (AWS), Stackdriver (GCP)

METRICS:
- Prometheus
- Graphite
- InfluxDB

TRACING:
- Jaeger
- Datadog APM
- AWS X-Ray
```

---

## Sampling Strategy

```
Sample traces (too expensive to trace 100%):

DEFAULT: 1% of all requests
EXCEPTION: 100% of:
  - Error requests
  - Requests > 1 second
  - First hour after deployment
  - High-value customers

Cost: Minimal
Coverage: Catch errors + see patterns
```

---

**Remember**: Observability is about understanding your system, not collecting infinite data. Focus on: What do you need to know? What helps you debug?
