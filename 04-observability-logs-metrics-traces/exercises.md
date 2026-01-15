# Exercises: Observability (Logs, Metrics, Traces)

10 exercises on designing observability into your systems.

---

## Exercise 1: Identify What to Log (Easy)

**Scenario**: Your API has these potential log points:
- Every request arrives
- Every database query
- Every cache hit/miss
- Every user action
- Every error

**Task**:
1. Pick 3 things to always log
2. Pick 2 things to log only on error
3. Why not log everything?

---

## Exercise 2: Structure a Log (Easy)

**Scenario**: Your team logs: `"User login failed at 2:30pm from IP 192.168.1.1"`

**Task**:
1. Rewrite as structured (JSON) log
2. Add fields for searching
3. Include request ID

---

## Exercise 3: Design Metrics (Easy)

**Scenario**: Your service has these measurements:
- Response time
- Error count
- Database connections
- Memory usage
- User satisfaction (?)

**Task**:
1. Pick 2-3 metrics to always track
2. Pick 2 that might be noise
3. Justify choices

---

## Exercise 4: Trace a Request (Medium)

**Scenario**: User request takes 2 seconds (expected: 200ms).

**Task**:
1. List 5 places a trace would show time spent
2. Identify the bottleneck
3. How would trace help find it?

---

## Exercise 5: Correlate Logs and Metrics (Medium)

**Scenario**:
- Metrics show: Error rate jumped from 0.1% to 5%
- Logs show: 100 timeout errors in auth service

**Task**:
1. What's the likely root cause?
2. What trace data would confirm?
3. Next debugging step?

---

## Exercise 6: Design for Multiple Services (Medium)

**Scenario**: Request goes through 4 services. Need to trace it.

**Task**:
1. What field must ALL services include?
2. How does request trace flow?
3. What happens if one service drops the ID?

---

## Exercise 7: Alert vs Observe (Medium)

**Scenario**: You could alert on:
- Error rate spike (alert)
- Specific error message in logs (observe)
- Slow database query (trace)

**Task**:
1. Which should trigger alert? Why?
2. Which is for investigation? Why?
3. How do they work together?

---

## Exercise 8: Sampling Traces (Medium)

**Scenario**: Your service gets 10K requests/sec. Tracing all is expensive.

**Task**:
1. Should you trace 100% of requests?
2. How would you sample 1% efficiently?
3. How would you ensure critical requests are traced?

---

## Exercise 9: Structured vs Unstructured (Medium)

**Scenario**: Compare these logs:
- `"Request failed, check server logs"`
- `{"error": "db_timeout", "request_id": "x", "service": "api"}`

**Task**:
1. Which is searchable?
2. Which helps automate alerting?
3. How would you query each?

---

## Exercise 10: Observability Budget (Medium)

**Scenario**: Logging/metrics/tracing costs money:
- 1GB logs/day: $10
- 1000 metrics/service: $5
- Tracing 10% of requests: $20

**Task**:
1. What would you keep? Why?
2. What could you drop?
3. How would you measure ROI?

---

## Self-Reflection

- ✓ Can I design what to log?
- ✓ Do I understand how metrics help?
- ✓ Can I trace a request through services?
- ✓ Do I know when to observe vs alert?
