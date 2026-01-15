# Exercises: Reliability Patterns and Resilience

10 exercises on designing resilient systems.

---

## Exercise 1: Identify Failure Modes (Easy)

**Scenario**: User registration flow:
- Call user DB
- Call email service
- Call payment processor

Each has success/failure possibility.

**Task**:
1. List possible failures
2. Impact of each failure
3. How to handle gracefully?

---

## Exercise 2: Circuit Breaker Threshold (Easy)

**Scenario**: Service has circuit breaker.

Settings:
- Failure threshold: 5 errors
- Timeout before retry: 30 seconds

During outage:
- API fails 100 times in 2 minutes
- Circuit opens on failure 5
- 95 requests fail fast (no call)

**Task**:
1. How many seconds of slow fails prevented?
2. Why is this better?
3. What about threshold: 10? vs 2?

---

## Exercise 3: Timeout Strategy (Easy)

**Scenario**: Your service calls 3 APIs:

API A: Normally 50ms, can be 5 sec (rare)
API B: Normally 200ms, can be 10 sec
API C: Normally 500ms, can be 30 sec

**Task**:
1. Set timeout for each
2. Too low? (cancel good requests)
3. Too high? (wait too long)

---

## Exercise 4: Retry Logic (Medium)

**Scenario**: Payment API has:
- 1% transient error rate
- Single request fails 1% of time

**Task**:
1. With 0 retries: Success rate?
2. With 1 retry: Success rate?
3. With 3 retries: Success rate?
4. Math: (1 - 0.01)^(1+retries)

---

## Exercise 5: Exponential Backoff (Medium)

**Scenario**: Failing service recovers in waves.

Retries without backoff:
- Retry 1: 10ms later (still failing)
- Retry 2: 10ms later (still failing)
- Overwhelm recovering service

Retries with backoff:
- Retry 1: Wait 100ms
- Retry 2: Wait 200ms
- Retry 3: Wait 400ms
- Backoff gives service time to recover

**Task**:
1. Why does backoff help?
2. Why not just wait longer first time?
3. What if backoff too aggressive?

---

## Exercise 6: Fallback Strategy (Medium)

**Scenario**: Primary data source slow, cache available.

Options:
A) Wait for slow DB (good data, slow)
B) Return cache (fast, stale data)
C) Circuit breaker (fail)
D) Timeout + fallback (fast + cache)

**Task**:
1. Which is best?
2. When to use cache?
3. When to fail instead?

---

## Exercise 7: Graceful Degradation (Medium)

**Scenario**: E-commerce site, services:

- Product catalog (critical)
- Recommendations (nice-to-have)
- Reviews (nice-to-have)
- Inventory (critical)

If recommendations API fails, what do you do?

**Task**:
1. Return error? (worse UX)
2. Skip recommendations? (better UX)
3. How to decide?

---

## Exercise 8: Bulkhead Pattern (Medium)

**Scenario**: 100 threads available.

Without bulkhead:
- Database connection pool fails
- All 100 threads calling slow database
- All 100 threads hang
- Service dies

With bulkhead:
- Database: 30 threads max
- Cache: 40 threads max
- API: 30 threads max
- One service fails, others survive

**Task**:
1. Why is bulkhead better?
2. How to allocate threads?
3. What happens if allocation wrong?

---

## Exercise 9: Testing Resilience (Medium)

**Scenario**: You implemented circuit breaker.

How to verify it works?

Options:
A) Unit test (does code work?)
B) Integration test (with fake API?)
C) Chaos test (kill real service?)

**Task**:
1. Which tests are needed?
2. What if tests pass but prod fails?
3. When to do chaos testing?

---

## Exercise 10: Resilience Trade-offs (Medium)

**Scenario**: Payment processing.

Trade-offs:
- Timeout too low: Cancel good requests
- Timeout too high: Wait too long for failure
- Retry too much: Overwhelm service
- Fallback: Return stale data
- Circuit breaker: Reject requests fast

**Task**:
1. How to choose right values?
2. Different for different services?
3. How to monitor if working?

---

## Self-Reflection

- ✓ Can I identify failure modes?
- ✓ Do I understand circuit breaker?
- ✓ Can I set good timeouts?
- ✓ Do I know resilience patterns?
