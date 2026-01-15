# Solutions: Reliability Patterns and Resilience

Solutions for all 10 exercises.

---

## Solution 1: Identify Failure Modes

**Answer**:

```
Failure modes:
1. User DB unreachable
   - Impact: Can't create user (critical)
   - Handle: Retry, then fail with message
   
2. Email service down
   - Impact: Can't send verification email
   - Handle: Queue email, send later
   
3. Payment processor down
   - Impact: Can't process payment
   - Handle: Fail and notify user (try later)

Graceful handling:
✓ Create user even if email fails (queue it)
✓ Accept registration even if payment fails (defer)
✓ Don't fail all if one service fails
✓ Retry transient failures
✓ Fallback for non-critical services
```

---

## Solution 2: Circuit Breaker Benefit

**Answer**:

```
Scenario:
- API fails 100 times in 2 minutes
- Circuit opens at 5 failures
- 95 requests get fast fail (no call)

Calculation:
Without circuit breaker:
- 100 requests × 5 second timeout = 500 seconds
- Plus overhead = ~600 seconds total

With circuit breaker:
- 5 requests with timeout = 25 seconds
- 95 requests fail fast = ~1 second
- Total = ~26 seconds
Savings: 574 seconds! (10 minutes saved)

Impact:
- Without: User waits minutes for error
- With: User gets error in milliseconds
- Much better UX

Threshold considerations:
- Too low (2): Might open on temporary spike
- Too high (10): More cascading damage
- Sweet spot (5): Balance fast-fail with tolerance
```

---

## Solution 3: Timeout Strategy

**Answer**:

```
API A: Normal 50ms, rare 5 sec
API B: Normal 200ms, rare 10 sec
API C: Normal 500ms, rare 30 sec

Recommended timeouts (P99 + buffer):
API A: 1 second (covers rare 5 sec spikes)
API B: 5 seconds (covers rare 10 sec spikes)
API C: 10 seconds (covers rare 30 sec spikes)

Too low:
- API A timeout 200ms: Cancel requests at 5 sec (bad)

Too high:
- API A timeout 10 sec: Wait too long for failure

Strategy:
- Get P99 latency (what 99% complete by)
- Add 50% buffer
- Result is good timeout

Monitoring:
- Track timeouts per API
- If > 1% timeouts: Investigate
- If 0% timeouts: Might be too high
```

---

## Solution 4: Retry Success Rate

**Answer**:

```
Formula: Success = 1 - (error_rate)^(1 + retries)

API has 1% error rate = 0.01

With 0 retries:
- Success = 1 - (0.01)^1 = 1 - 0.01 = 0.99 = 99%

With 1 retry:
- Success = 1 - (0.01)^2 = 1 - 0.0001 = 99.99%

With 3 retries:
- Success = 1 - (0.01)^4 = 1 - 0.00000001 = 99.9999%

Key insight:
- More retries = exponentially better success
- But: Don't retry forever (wastes time)
- Stop after 3 retries (good balance)

Note:
- Only works for TRANSIENT errors
- Don't retry permanent failures (auth error, not found)
```

---

## Solution 5: Exponential Backoff

**Answer**:

```
Without backoff:
Retry 1: Immediate (service still broken)
Retry 2: Immediate (service still broken)
Retry 3: Immediate (service still broken)
Result: 1000 requests hit recovering service
Outcome: Makes recovery slower

With backoff:
Retry 1: Wait 100ms (service still broken)
Retry 2: Wait 200ms (service starts recovering)
Retry 3: Wait 400ms (service mostly recovered)
Result: 100 requests hit recovering service
Outcome: Allows recovery, less load

Why not wait longer first time?
- First attempt might succeed (service OK)
- Don't wait if not needed
- Exponential: Increasing wait = fair balance

Too aggressive backoff:
- Retry 1: 1000ms, Retry 2: 2000ms
- Too slow overall
- User has to wait longer

Too lenient:
- Retry 1: 10ms, Retry 2: 20ms
- Still overwhelming
- Not enough time to recover
```

---

## Solution 6: Fallback Strategy

**Answer**:

```
BEST: Option D (Timeout + fallback)

Why:
- Timeout: Don't wait forever
- Fallback: Return cached data
- Result: Fast response, slightly stale

When to use cache fallback:
✓ Data doesn't need real-time (product catalog)
✓ Cache is recent (< 5 min old)
✓ Better stale data than error

When NOT to use cache:
✗ Inventory (need accurate count)
✗ Account balance (need current)
✗ Critical operations (error is better)

Implementation:
try:
  return fetch_db_with_timeout(5sec)
except Timeout:
  return fetch_cache()
```

---

## Solution 7: Graceful Degradation

**Answer**:

```
Services:
- Product catalog: CRITICAL (show products)
- Recommendations: NICE (suggest products)
- Reviews: NICE (show feedback)
- Inventory: CRITICAL (check stock)

If recommendations API fails:
✓ Skip recommendations (best UX)
✓ Show product + inventory + reviews
✓ Better experience than error page

If reviews API fails:
✓ Skip reviews
✓ Show product + recommendations + inventory

If catalog API fails:
✗ Don't skip (user can't buy anything)
✗ Return error (correct action)

Decision framework:
- Critical service fails → Return error
- Nice-to-have fails → Skip gracefully

Implementation:
try:
  recommendations = get_recommendations()
except:
  recommendations = []  # Empty, not error
```

---

## Solution 8: Bulkhead Pattern

**Answer**:

```
Without bulkhead (100 threads):
- All threads available to all services
- DB connection fails
- All 100 threads try to call DB
- All 100 block/hang
- Service dies (no threads available)

With bulkhead (allocate threads):
- DB pool: 30 threads max
- Cache pool: 40 threads max
- API pool: 30 threads max
- DB fails: Only 30 threads stuck
- Cache + API: 70 threads still available
- Service survives (degraded, not down)

How to allocate:
- Critical service: More threads (40%)
- Important service: Moderate (30%)
- Optional service: Less (20%)

What if allocation wrong?
- Too many for one: Other services starved
- Too few: Bottleneck in critical path
- Monitor: Adjust based on load patterns

Key benefit: Isolation
- Failure in one = Others unaffected
```

---

## Solution 9: Testing Resilience

**Answer**:

```
UNIT TESTS:
✓ Does circuit breaker code work?
✓ Does retry logic work?
✓ Mock failures, verify behavior
When: During development

INTEGRATION TESTS:
✓ Does circuit breaker work with real API?
✓ Does timeout actually timeout?
✓ Use test double (fake API)
When: Before each deployment

CHAOS TESTS:
✓ Kill real service, see what happens
✓ Network partition, slow requests
✓ Production (careful!) or staging
When: Monthly/quarterly
Purpose: Find assumptions that break

Why tests pass but prod fails:
- Network conditions different
- Load different
- Timing issues
- Unexpected interactions
- Real data patterns

Recommendation:
1. Unit tests (always)
2. Integration tests (always)
3. Chaos tests (regular, planned)
4. Production monitoring (continuous)
```

---

## Solution 10: Resilience Trade-offs

**Answer**:

```
TIMEOUT:
- Too low: Cancel good requests (bad)
- Too high: Wait too long (bad)
- Solution: Set to P99 latency + 50% buffer

RETRY:
- Too much: Overwhelm service (bad)
- Too little: Miss transient fixes (bad)
- Solution: 3 retries with exponential backoff

FALLBACK:
- Stale data: Wrong results (bad)
- No fallback: Errors (bad)
- Solution: Fallback for non-critical, error for critical

CIRCUIT BREAKER:
- Too strict: Reject good requests (bad)
- Too lenient: Cascading failures (bad)
- Solution: 5 failures threshold, 30 sec retry

Different for different services?
✓ YES
- Critical service: Lower timeout, fewer retries
- Optional service: Higher timeout, more retries
- External API: Strict circuit breaker
- Internal service: Lenient circuit breaker

How to monitor if working:
- Track timeout rate (should be 0-1%)
- Track retry success (how many succeed on retry?)
- Track circuit breaker state (open/closed)
- Track error rate (should be stable)

Adjust over time:
- If timeouts high: Increase timeout
- If retries fail: Reduce retry count
- If circuit opens too often: Increase threshold
```

---

## Key Principles

1. **Expect failures**: Failures happen, plan for them
2. **Fail fast**: Don't waste time on broken services
3. **Timeouts**: Always set timeouts
4. **Retry wisely**: Exponential backoff
5. **Graceful degradation**: Skip optional, fail critical
6. **Isolation**: Bulkheads prevent cascading
7. **Test failures**: Verify patterns work
