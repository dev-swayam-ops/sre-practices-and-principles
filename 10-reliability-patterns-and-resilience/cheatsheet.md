# Cheatsheet: Reliability Patterns and Resilience

Quick reference for resilience patterns.

---

## Key Patterns

| Pattern | Purpose | When to Use |
|---------|---------|------------|
| **Timeout** | Fail fast | Always, on all external calls |
| **Retry** | Handle transient errors | For API calls (not DB) |
| **Circuit Breaker** | Prevent cascading | Calling external services |
| **Fallback** | Degrade gracefully | Non-critical features |
| **Bulkhead** | Isolate failures | Separate thread/resource pools |

---

## Circuit Breaker States

```
CLOSED (normal):
- Service is healthy
- Requests go through normally
- Count failures

OPEN (broken):
- Service is unhealthy (> 5 failures)
- Requests fail immediately (no call)
- Fail fast, don't waste time
- Try again after timeout (30 sec)

HALF-OPEN (testing):
- Service might be recovered
- Allow 1 test request
- If success → CLOSED
- If fail → OPEN
```

---

## Timeout Guidelines

```
Recommended timeout = P99 latency + 50% buffer

Example:
API P99 latency: 200ms
Buffer: 50% = 100ms
Recommended timeout: 300ms

Too low (100ms):
- Cancel requests at 200ms (bad)

Too high (5000ms):
- Wait too long for failure (bad)

Rule: Use P99 + buffer
```

---

## Retry Strategy

```
Retry only transient errors:
✓ Network timeout
✓ 503 Service Unavailable
✓ 429 Rate Limited

Don't retry permanent errors:
✗ 400 Bad Request
✗ 401 Unauthorized
✗ 404 Not Found

Exponential backoff:
Retry 1: Wait 100ms
Retry 2: Wait 200ms
Retry 3: Wait 400ms
Max: 3 retries

Success rate = 1 - (error_rate)^(retries+1)
```

---

## Fallback Decision Matrix

| Data Type | Fallback | Handle |
|-----------|----------|--------|
| Product catalog | Cache | Return stale |
| Recommendation | None | Skip feature |
| Inventory count | None | Error (critical) |
| User profile | Cache | Return stale |
| Account balance | None | Error (critical) |

---

## Bulkhead Example

```
Total threads: 100

With bulkhead:
- Database pool: 30 threads
- Cache pool: 40 threads
- API pool: 30 threads

If DB fails:
- 30 threads stuck
- 70 still available
- Service degrades, doesn't die
```

---

## Health Check Pattern

```
Health check endpoint: /health

Return:
- 200 OK: Service healthy
- 503 Service Unavailable: Unhealthy

Circuit breaker uses health check:
- Periodically call /health
- If 503: Increase failure count
- If 200: Reset failure count
```

---

## Resilience Testing Checklist

```
☐ Unit tests for timeout logic
☐ Unit tests for retry logic
☐ Unit tests for circuit breaker
☐ Integration tests with timeout
☐ Integration tests with circuit breaker
☐ Chaos test: Kill dependency service
☐ Chaos test: Network partition
☐ Chaos test: Slow responses
☐ Load test: Under high load
☐ Monitoring: Track timeouts/retries/circuits
```

---

## Monitoring Metrics

| Metric | Good | Warning | Critical |
|--------|------|---------|----------|
| Timeout % | < 0.1% | 0.1-1% | > 1% |
| Retry success | > 95% | 80-95% | < 80% |
| Circuit state | CLOSED | HALF-OPEN | OPEN > 5 min |
| Error rate | < 0.1% | 0.1-1% | > 1% |

---

## Common Failure Patterns

```
Cascading failure:
Service A → Service B → Service C
B fails → A waits → A fails → User impact

Prevention:
- Timeout at each level
- Circuit breaker on each call
- Fail fast at each level

Retry amplification:
Service fails → Client retries
100 clients × 3 retries = 300 calls
Overwhelms recovering service

Prevention:
- Exponential backoff
- Limit retries to 3
- Jitter (random delay)
```

---

**Remember**: Resilience is about graceful degradation, not perfection.
