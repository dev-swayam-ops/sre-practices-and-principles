# Exercises: Change Management and Release Safety

10 exercises on safe deployments and release strategies.

---

## Exercise 1: Pre-deployment Checklist (Easy)

**Scenario**: You're about to deploy a new API endpoint.

**Task**:
1. List 5 things to verify before deploying
2. Why is each important?
3. What happens if you skip one?

---

## Exercise 2: Choose Release Strategy (Easy)

**Scenario**: Deploy options for three services:

1. Internal API (non-critical)
2. Payment API (critical)
3. User profile (moderate impact)

**Task**:
1. Pick strategy for each: rolling, canary, blue-green
2. Justify your choice
3. What's the risk?

---

## Exercise 3: Rollback Procedure (Easy)

**Scenario**: New deploy introduced a bug. Users complaining.

**Task**:
1. What do you do first?
2. How long should rollback take? (seconds, minutes, hours?)
3. After rollback, what's next?

---

## Exercise 4: Canary Metrics (Medium)

**Scenario**: Deploying canary to 5% of users.

Metrics:
- Current error rate: 0.05%
- Canary error rate: 0.08%
- Current latency P99: 150ms
- Canary latency P99: 200ms

**Task**:
1. Should you proceed? Or rollback?
2. What's acceptable change?
3. How long to monitor?

---

## Exercise 5: Database Migration (Medium)

**Scenario**: New code needs database schema change.

Challenge:
- Can't stop service (downtime not allowed)
- Can't have two schema versions (conflicts)
- Rollback must be instant

**Task**:
1. How do you deploy?
2. What order: code first or schema first?
3. How does rollback work?

---

## Exercise 6: Feature Flags (Medium)

**Scenario**: Deploy code for feature, but don't enable it yet.

Benefit:
- Code is deployed and tested
- Feature can be enabled/disabled without redeploy

**Task**:
1. How do feature flags help deployment?
2. When to use them?
3. When not needed?

---

## Exercise 7: Deployment Timing (Medium)

**Scenario**: You want to deploy today.

Options:
- Monday 2pm (busy, midday)
- Friday 4pm (quiet, before weekend)
- Tuesday 10am (slow, maintenance window)

**Task**:
1. Which is best? Why?
2. Which is worst? Why?
3. What about off-hours?

---

## Exercise 8: Load Test Before Deploy (Medium)

**Scenario**: New code handles payments.

Load test results:
- 1,000 req/sec: OK
- 2,000 req/sec: 5% error rate
- 3,000 req/sec: 25% errors

Current production load: 1,500 req/sec

**Task**:
1. Is it safe to deploy?
2. What's the safety margin?
3. What if traffic spikes to 2,000 req/sec?

---

## Exercise 9: Monitoring Before Deploy (Medium)

**Scenario**: Deployment happened, now what?

Metrics to monitor:
- Error rate
- Latency
- CPU usage
- Memory usage
- Database connections
- User complaints

**Task**:
1. Which is most important? Why?
2. How often to check (every min, hour)?
3. How long to monitor (30 min, 24 hours)?

---

## Exercise 10: Blast Radius (Medium)

**Scenario**: A bad deployment affects:

Option A: 100% of users (total outage)
Option B: 10% of users (canary gone wrong)

**Task**:
1. Which is easier to detect?
2. Which allows faster rollback?
3. How does canary prevent option A?

---

## Self-Reflection

- ✓ Can I plan a safe deployment?
- ✓ Do I understand release strategies?
- ✓ Can I design a rollback?
- ✓ Do I know what to monitor?
