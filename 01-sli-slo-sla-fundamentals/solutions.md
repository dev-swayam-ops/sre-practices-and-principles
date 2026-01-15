# Solutions: SLI, SLO, SLA Fundamentals

Detailed solutions for all 10 exercises with explanations.

---

## Solution 1: Identify SLI vs SLO vs SLA

**Answer**:

| Statement | Category | Explanation |
|-----------|----------|-------------|
| "Our database is up 99.99% of the time" | **SLI** | Measured observation of infrastructure |
| "We target 99.9% API availability per month" | **SLO** | Internal target/objective |
| "We promise customers 99.8% uptime or provide 25% refund" | **SLA** | Customer contract with compensation |

**Is this a good set?** 

❌ **NO**, for two reasons:

1. **Wrong SLI measurement**: "Database uptime" (99.99%) is infrastructure, not customer-facing. Should measure API success rate instead.

2. **Inverted SLO/SLA relationship**: SLO (99.9%) is HIGHER than SLA (99.8%). It should be the other way!
   - Correct: SLA 99.8%, SLO 99.9% (gives 0.1% buffer)
   - Current: SLA 99.8%, SLO 99.9% means SLO is easier to meet (wrong incentive)

**Corrected Version**:
- SLI: "% of API requests returning 2xx/3xx status per month"
- SLO: "99.9% of requests return 2xx/3xx"
- SLA: "99.8% uptime guaranteed; 25% refund if breached"

---

## Solution 2: Define SLI for a Streaming Video Service

**Answer**:

**Best SLIs** (in order of importance):

1. **Availability**: % of requests that successfully start playback
   - Measured: Requests that return playable content within 3 seconds
   - Why: If video won't start, nothing else matters to the user
   - NOT: Just "API uptime" (infrastructure metric)

2. **Stream Quality**: % of viewers experiencing full-bitrate streaming
   - Measured: Completed playback at target bitrate (P95 of viewers)
   - Why: Buffering = bad experience = user leaves
   - NOT: "Network bandwidth" (too technical)

3. **Latency**: P99 time to first byte when pressing play
   - Measured: Time from play button to first data frame <3 seconds
   - Why: User expects instant response
   - NOT: "Server response time" (measurements don't reflect user perception)

4. **Error Rate**: % of sessions that complete without error
   - Measured: Sessions that don't error out mid-stream
   - Why: Crashes disrupt viewing
   - NOT: "Exception rate in logs" (some errors don't affect users)

**Why NOT**: 
- ❌ "Database uptime" = infrastructure, not user experience
- ❌ "Server CPU usage" = internal metric, not observable by users
- ❌ "API response time to metadata" = infrastructure detail, measure user-facing latency

---

## Solution 3: Set Realistic SLOs

**Answer**:

Based on current performance:

| Metric | Current | Proposed SLO | Justification |
|--------|---------|--------------|---|
| Availability | 99.85% | **99.9%** | Current is 99.85%, target slightly higher (achievable with some improvement) |
| P99 Latency | 150ms | **<180ms** | Current is good; allow small buffer for growth |
| Error Rate | 0.08% | **<0.1%** | Current is at this level; small safety margin |

**Reasoning**:

- **SLO should be above current performance but achievable**: If SLO = current, you always meet it (worthless). If SLO >> current, you'll always breach it (demoralizing).

- **The 0.05-0.1% gap**: Good SLOs are 0.05-0.1% above current, meaning you need modest improvements but aren't aiming for impossible perfection.

- **Different SLOs per metric**: Don't use the same percentage for everything. Each metric has different importance and cost.

---

## Solution 4: Calculate Error Budget

**Answer**:

**Given**: SLO = 99.5%

**Error Budget = (1 - SLO) × Total Time**

```
1. Monthly (30 days = 30 × 24 × 60 = 43,200 minutes)
   Error Budget = 0.5% × 43,200 = 216 minutes (3.6 hours)

2. Weekly (7 days = 7 × 24 × 60 = 10,080 minutes)
   Error Budget = 0.5% × 10,080 = 50.4 minutes (~50 minutes)

3. Daily (24 × 60 = 1,440 minutes)
   Error Budget = 0.5% × 1,440 = 7.2 minutes (~7 minutes per day)
```

**Implications**:

- **Monthly 216 minutes (3.6 hours)**: If your service has one 3-hour outage, you've used the entire month's budget.

- **Deployment Risk**: That's relatively generous! You could do:
  - Multiple risky deployments OR
  - One major infrastructure change OR
  - Still recover from accidental incidents

- **Remaining Budget**: 
  - Started with: 216 minutes
  - Used on outage: 15 minutes
  - Remaining: 201 minutes (almost the whole month left!)

**Conclusion**: 99.5% SLO is quite permissive. Good for services where reliability isn't critical.

---

## Solution 5: SLO and SLA Buffer Importance

**Answer**:

**Why SLO = SLA is risky:**

```
If SLO = SLA = 99.9%:
- You have NO buffer
- Any incident that breach SLA means you also breached SLO
- You can't make deployment decisions with any safety margin
- Customer expectations and team targets are identical (bad idea)

Expected outcome: You'll breach your SLA regularly
```

**Better approach**:

```
SLA = 99.9% (promise to customers)
SLO = 99.95% (internal target - STRICTER)

Buffer = 99.95% - 99.9% = 0.05% = 22 minutes/month

Interpretation:
- If you breach SLO, alert the team BEFORE affecting SLA
- Error budget calculations based on SLO, not SLA
- Deployment decisions made with 22-minute safety net
- Customers protected; team has room to operate
```

**SLO Breach vs SLA Breach**:

| | SLO Breach | SLA Breach |
|--|--|--|
| **What it means** | We didn't meet internal target | We broke customer contract |
| **Who notices** | Engineering team | Customers + Legal team |
| **Consequence** | Review operations, adjust plans | Pay refunds, reputation damage |
| **Action** | Add to postmortem, improve | Crisis management, customer calls |

**Example SLO/SLA pairs**:

- High-reliability: SLA 99.9%, SLO 99.95% (buffer: 0.05%)
- Standard: SLA 99.5%, SLO 99.7% (buffer: 0.2%)
- Flexible: SLA 99%, SLO 99.5% (buffer: 0.5%)

---

## Solution 6: Choose the Right SLI

**Answer**:

**Good SLIs (should include)**:

✓ **% of API responses returning data in < 500ms: 98%**
- This measures user-facing latency
- Directly reflects user experience
- Observable from user's perspective

✓ **Application error rate: 0.3%** (if user-facing)
- Measure: % of requests that succeed (return valid data)
- Important: Some app errors don't affect users (logged but not visible)

✓ **Web server response time: P99=200ms**
- Key latency percentile (P99 = worst experience for most users)
- P50 is less useful (average doesn't show worst cases)

**NOT good SLIs**:

❌ **Database uptime: 99.99%**
- Infrastructure metric, not user-facing
- A 5-nines database with a buggy app = bad user experience
- Measure the outcome (API success), not the component

❌ **Network latency: 5ms**
- Outside your control (ISP problem)
- Not user-facing (users don't care about network latency)
- Focus on what you can control and measure

**Proposed SLOs**:

```
1. Availability: 99.5% of requests return 2xx/3xx
   SLO: >= 99.5% per month

2. Latency: P99 response time
   SLO: P99 < 500ms for 99% of requests per month

3. Error Rate: Application errors
   SLO: < 0.3% error rate per month
```

---

## Solution 7: SLO Breach Analysis

**Answer**:

**Calculation**:

```
Baseline availability: 99.9%

Weeks 1-3 (3 weeks × 7 days × 100%):
- 99.92% × 21 days = exceeding SLO by 0.02%/day
- Reserves budget (slight overage, but within tolerance)

Week 4 (7 days):
- 45-minute outage = 45 / 10,080 minutes = 0.446% downtime
- Availability = 100% - 0.446% = 99.554% for the week

Month-to-date:
- Need to calculate exact: assume 30 days, ~25.6 days so far

Total downtime: ~12 minutes from Weeks 1-3 + 45 minutes Week 4 = 57 minutes
Monthly downtime allowed: 43 minutes (99.9% SLO)

Result: 57 minutes > 43 minutes = MISSED SLO by 14 minutes
```

**SLO Breached?** 
- ❌ YES. You had 43 minutes budget, used 57 minutes.

**Remaining budget?**
- ❌ NEGATIVE. You're 14 minutes over for the month.

**How to communicate**:
```
"We breached our 99.9% availability SLO this month by 14 minutes (0.033%).
The incident on Day 25 lasted 45 minutes and exhausted our error budget.
Team: Review the incident, update runbooks, and implement prevention.
"
```

**What this teaches**:
1. Incidents are expensive (45 min incident = depleted entire month's budget)
2. Need better detection/response (45 min is too long for incident)
3. SLO might be too strict (99.9% is achievable; consider 99.8% instead)

---

## Solution 8: Error Budget Deployment Decision

**Answer**:

**Budget Analysis**:
```
Total monthly budget: 43 minutes (99.9% SLO for 30 days)
Budget used so far: 12 minutes (37.5 days in, so pace is slightly better)
Budget remaining: 31 minutes
Deployment risk: 8-10 minute potential downtime
Buffer safety margin: 21-23 minutes
```

**Decision: ✓ APPROVE DEPLOYMENT**

**Reasoning**:
- You have 31 minutes of budget remaining
- Deployment risk is 8-10 minutes worst-case
- Even if deployment fails and you lose the full 10 minutes, you still have 21 minutes for the rest of the month
- Days remaining: ~8 days (high incident probability in remaining month time is low)

**How to Proceed**:
1. ✓ Deploy with comprehensive monitoring
2. ✓ Have rollback ready (should take <2 minutes)
3. ✓ Stagger deployment (canary 5%, monitor, then full rollout)
4. ✓ Keep on-call available for quick response

**Alternative Approaches** (if you wanted less risk):
1. **Feature flags**: Deploy code but disable feature for 100% of users. Gradually enable (0% → 10% → 50% → 100%) over days
2. **Shadow traffic**: Deploy to shadow servers, verify it works, then switch traffic
3. **Scheduled maintenance window**: Deploy during low-traffic period (smaller impact radius)
4. **Wait**: If you're nervous, wait until next month's fresh 43-minute budget

**Communicating to Management**:
```
"We have sufficient error budget to proceed with the payment service refactor.
Current budget: 31 minutes remaining. Deployment risk: ~10 minutes worst-case.
Mitigation: Staged rollout, canary deployment, 24/7 monitoring.
Go/No-Go: GO"
```

**What Changes the Decision**:
- ❌ If remaining budget < 10 minutes: REJECT (too risky)
- ❌ If deployment risk > 15 minutes: REJECT (too much risk)
- ❌ If it's day 28-30 of month: REJECT (less time to recover from incidents)
- ❌ If you have critical customer events happening this week: REJECT

---

## Solution 9: Multi-Service SLO Strategy

**Answer**:

**SLO Assignment**:

| Service | SLO | Justification |
|---------|-----|---|
| **API Gateway** | **99.95%** | Critical path. Every request goes through. Failures = broad impact. Must be most reliable. |
| **Payment Service** | **99.9%** | High value but smaller % of traffic. Failures are expensive. Must be reliable. |
| **Recommendation Engine** | **99%** | Optional feature. Failures degrade gracefully (show defaults). Can tolerate more downtime. |

**Monthly Error Budgets**:
- API Gateway: 22 minutes (stricter, more engineering investment)
- Payment Service: 43 minutes (moderate)
- Recommendation Engine: 432 minutes (relaxed, can be riskier)

**Why different SLOs**:

1. **Criticality**: API Gateway blocks all functionality → highest SLO
2. **Business impact**: Payment failures = revenue loss → high SLO
3. **User experience**: Recommendations are optional → lower SLO acceptable

2. **Cost**: 99.95% costs more to achieve than 99%. Invest where it matters most.

**Communicate to Customers**:

```
Our Service Level Agreements:

API Gateway: 99.95% uptime guaranteed
- Core platform availability
- Compensation: 25% refund if breached

Payment Service: 99.9% uptime guaranteed
- Payment processing and transactions
- Compensation: 20% refund if breached

Recommendation Engine: 99% uptime
- Personalized recommendations
- Compensation: 5% credit if breached
```

**Impact on Engineering Prioritization**:

- API Gateway gets resources for:
  - Auto-scaling
  - Multi-region redundancy
  - Advanced monitoring
  - Canary deployments

- Payment Service gets:
  - Database replication
  - Circuit breakers
  - Fallback mechanisms
  - Frequent alerts

- Recommendation Engine gets:
  - Standard monitoring
  - Graceful degradation
  - Feature flags for safer deployments

---

## Solution 10: SLI to Dashboard

**Answer**:

**Dashboard Visualization**:

1. **Availability (% of requests returning 2xx/3xx)**
   - Visual: Line chart showing % over time
   - SLO indicator: Red line at 99.9% target
   - Alert: Orange warning zone at 99.8%, red at breach
   - Update frequency: Every 1 minute

2. **Latency (P99 Response Time)**
   - Visual: Line chart showing milliseconds over time
   - SLO indicator: Green zone < 200ms, yellow 200-250ms, red > 250ms
   - Percentiles shown: P50, P95, P99 (not just average)
   - Update frequency: Every 5 minutes

3. **Error Rate (% of 4xx/5xx errors)**
   - Visual: Line chart showing % of errors
   - SLO indicator: Red line at 0.1% threshold
   - Alert: Yellow at 0.08%, red at breach
   - Update frequency: Every 1 minute

**Error Budget Visualization**:

```
Example: Monthly SLO = 99.9% (43 minutes budget)

Day 10:  ████████░░░░░░░░░░░░ Budget Used: 8 min (Pace: on track)
Day 20:  ████████████░░░░░░░░ Budget Used: 19 min (Pace: slightly fast)
Day 25:  ███████████████░░░░░░ Budget Used: 34 min (After incident)
Day 30:  ██████████████████░░░ Budget Used: 41 min (Final: within SLO)

Status: ON TRACK ✓
Days left: 5
Budget left: 2 minutes
Recommendation: Don't deploy this week (too risky)
```

**Who Should See This Dashboard**:

1. **Engineers (daily)**
   - Understand what's happening
   - Identify degradation early
   - Make deployment decisions
   - Respond to alerts

2. **On-Call Engineer (24/7)**
   - Main dashboard
   - Detailed metrics
   - Alert configuration

3. **Engineering Manager (weekly)**
   - SLO compliance view
   - Trend analysis
   - Capacity planning

4. **Product Manager (monthly)**
   - SLO achievements
   - Business impact
   - Customer communication

5. **Customers (public dashboard)**
   - High-level status (Availability + Latency only)
   - Not detailed enough to game metrics
   - Updated every 5 minutes

---

## Next: Ready for the Next Module?

You've mastered SLI/SLO/SLA. Next module covers monitoring and alerting—how to actually implement these metrics in real systems.

**Key reflection**: How would you change your deployments if you tracked error budget weekly instead of monthly?

