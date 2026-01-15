# Solutions: Observability (Logs, Metrics, Traces)

Solutions for all 10 exercises.

---

## Solution 1: What to Log

**Answer**:

```
ALWAYS LOG:
✓ Errors (why things failed)
✓ Important transitions (login, payment, deploy)
✓ Performance events (slow queries, timeouts)

LOG ON ERROR ONLY:
- Every database query (too much noise)
- Every cache hit (volume too high)

Why not everything?
- Storage costs (10TB+ logs/day)
- Can't search through it
- Privacy issues (logs contain user data)
```

---

## Solution 2: Structure a Log

**Answer**:

```json
BEFORE (unstructured):
"User login failed at 2:30pm from IP 192.168.1.1"

AFTER (structured):
{
  "timestamp": "2026-01-15T14:30:00Z",
  "request_id": "req-12345",
  "event": "login_failed",
  "user_id": "user-789",
  "source_ip": "192.168.1.1",
  "error_reason": "password_mismatch",
  "duration_ms": 45,
  "service": "auth"
}

Benefits:
- Searchable by request_id, user_id, error_reason
- Metrics can parse structure
- Can filter by timestamp range
- Can correlate with other services
```

---

## Solution 3: Design Metrics

**Answer**:

```
KEEP ALWAYS:
✓ Error rate (critical for SLO)
✓ Response latency (user experience)

KEEP SOMETIMES:
⚠ Database connections (useful to track)

SKIP:
✗ Memory usage (OS handles it)
✗ User satisfaction (not measurable directly)

Why?
- Error rate & latency directly affect users
- Connections show load (useful for scaling)
- Memory/CPU are just infrastructure
- User satisfaction needs different measure
```

---

## Solution 4: Trace a Request

**Answer**:

```
Trace shows timing at each step:

User Request (2000ms total)
├─ API Gateway (20ms)
├─ Auth Service (50ms)
├─ Database Query (1500ms) ← BOTTLENECK
├─ Cache Fetch (100ms)
└─ Response Build (300ms)

Finding: Database query takes 75% of time
Trace helps because:
- You see exactly where time is spent
- Can identify bottlenecks visually
- Can see which service is slow
- Know what to optimize next
```

---

## Solution 5: Correlate Logs and Metrics

**Answer**:

```
Metrics: Error rate spiked 5%
Logs: 100 timeout errors in auth service

Analysis:
- Error rate spike correlates with auth timeouts
- Likely root cause: Auth service slow/down
- Could be: Database issue, network, scaling

Trace would show:
- Auth requests taking 5+ seconds
- Slow database query in auth
- OR auth → database connection failing

Next step:
1. Check auth service logs for errors
2. Check database status
3. Check network latency to database
4. Scale auth service if needed
```

---

## Solution 6: Multi-Service Tracing

**Answer**:

```
Service 1 → Service 2 → Service 3 → Service 4

CRITICAL: request_id must be passed through
Service 1: Generates request_id: "xyz-123"
Service 2: Receives request_id, logs it, passes to S3
Service 3: Receives request_id, logs it, passes to S4
Service 4: Logs request_id

Query: request_id="xyz-123" 
Returns logs from ALL 4 services in order ✓

If Service 3 drops request_id:
- Can't trace rest of request
- Service 4 logs won't correlate
- Breaks the chain ✗

Solution:
- Middleware auto-propagates request_id
- Code review to enforce
```

---

## Solution 7: Alert vs Observe

**Answer**:

```
Error rate spike → ALERT
- Direct impact on users
- Need immediate response
- Triggers on-call

Specific error message in logs → OBSERVE
- For debugging after alert
- Investigate root cause
- Not critical by itself

Slow database query → OBSERVE or ALERT
- If query makes error rate high → ALERT
- If query is just slow → OBSERVE
- Context matters

How they work:
1. ALERT triggers (error rate spike)
2. On-call page sent
3. Engineer logs in
4. Uses LOGS to find error message
5. Uses TRACE to see database query
6. Implements fix
```

---

## Solution 8: Sampling Traces

**Answer**:

```
10K requests/sec = 600M traces/day
Cost: Enormous

Sampling strategy:

DEFAULT: Trace 1% of requests
- 6M traces/day (manageable)
- Still see patterns

EXCEPTIONS: Trace 100% of:
- Error requests (important!)
- Requests taking > 1 second
- Requests from high-value customers
- Deployments (first hour)

Implementation:
Service logs: "trace_sampled: true/false"
Dashboard filters: "trace_sampled:true"

Benefits:
- Catch errors even at 1% sample
- Still affordable
- Can increase sample on demand
```

---

## Solution 9: Structured vs Unstructured

**Answer**:

```
SEARCHABLE:
✓ Structured JSON logs
  Query: error="db_timeout"
  Result: All DB timeout errors instantly

NOT SEARCHABLE:
✗ Free text "Request failed, check logs"
  Have to manually read all logs

AUTOMATION:
✓ Structured logs → Can trigger alerts
✗ Unstructured → Requires human review

Tradeoff:
- Structured = more work upfront
- Unstructured = faster to write, harder to search

RECOMMENDATION: Structured
```

---

## Solution 10: Observability Budget

**Answer**:

```
Costs:
- 1GB logs/day = $10
- 1000 metrics = $5
- Trace 10% = $20
Total: $35/day

KEEP:
✓ Logs ($10): Essential for debugging
✓ Metrics ($5): Essential for alerting

REDUCE:
⚠ Tracing (trace 1% not 10%): Save $18/day
  Reduce cost: $17/day
  Still catch errors

ROI:
- Reduce logs to critical events: Save $5/day
- Total: $12/day (66% cost reduction)
- Value: Still debug issues, still alert
- ROI: High

Strategy:
1. Log only errors + key events
2. Metrics unchanged
3. Sample traces at 1%
4. Store logs 7 days (not 30)
```

---

## Key Principles

1. **Logs**: Debug tool, keep important events only
2. **Metrics**: Trend/alert tool, cost-effective
3. **Traces**: Performance tool, sample intelligently
4. **Correlation**: Use request_id across all
5. **Cost**: Observability is expensive, optimize
