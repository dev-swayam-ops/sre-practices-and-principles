# Cheatsheet: SLI, SLO, SLA Fundamentals

Quick reference guide for SLI/SLO/SLA metrics and error budget calculations.

---

## Quick Definitions

| Term | Definition | Example |
|------|-----------|---------|
| **SLI** | Service Level **Indicator** - What we measure | 99.92% uptime this month (observation) |
| **SLO** | Service Level **Objective** - What we target | 99.9% uptime per month (goal) |
| **SLA** | Service Level **Agreement** - What we promise | 99.8% uptime or 10% refund (contract) |

---

## The Relationship

```
┌─────────────────┐
│   SLA (Promise) │ ← Least strict (customer contract)
│     99.8%       │
└────────┬────────┘
         │
┌────────▼────────┐
│   SLO (Target)  │ ← Medium strict (internal goal)
│     99.9%       │   Usually SLO > SLA by 0.05-0.1%
└────────┬────────┘
         │
┌────────▼────────────┐
│  SLI (Measured)     │ ← Reality (what actually happened)
│   99.92% this month │   Should exceed SLO
└─────────────────────┘

Important: SLA ≤ SLO < Actual Performance (ideal)
```

---

## Error Budget Formula

```
Error Budget = (1 - SLO%) × Total Time Period

Example: SLO = 99.9% for 30 days
Error Budget = (1 - 0.999) × (30 × 24 × 60) minutes
Error Budget = 0.001 × 43,200 = 43.2 minutes
```

---

## Common SLOs and Error Budgets

| SLO % | Per Month | Per Week | Per Day | Annual | Meaning |
|-------|-----------|----------|---------|--------|---------|
| **99%** | 432 min | 100 min | 14.4 min | 3.6 days | Very relaxed; good for non-critical services |
| **99.5%** | 216 min | 50 min | 7.2 min | 1.8 days | Relaxed; good for secondary services |
| **99.9%** | 43 min | 10 min | 86 sec | 8.76 hrs | **Standard** for most services |
| **99.95%** | 22 min | 5 min | 43 sec | 4.38 hrs | Strict; for critical services |
| **99.99%** | 4.3 min | 1 min | 9 sec | 52 min | Very strict; for critical infrastructure |

**Rule of thumb**: Most services target 99.9%. Higher = more expensive.

---

## SLI Categories

### Availability
- **What**: % of requests that succeed (not 5xx errors)
- **Measured**: Valid responses / Total requests
- **Example**: 99.92% of API calls returned 2xx/3xx status
- **Threshold**: Alert if < 99.5% for SLO of 99.9%

### Latency
- **What**: Response time (use percentiles, not averages)
- **Measured**: Time from request to response
- **Example**: P99 latency = 200ms (worst 1% of requests)
- **Threshold**: Alert if P99 > 250ms for SLO of 200ms
- **Why P99?**: P50 average hides slow requests that annoy users

### Error Rate
- **What**: % of requests that fail
- **Measured**: Error responses / Total requests
- **Example**: 0.08% of requests returned 4xx/5xx
- **Threshold**: Alert if > 0.15% for SLO of 0.1%

### Throughput
- **What**: Requests per second (capacity)
- **Measured**: Successful requests / Time period
- **Example**: Service handled 50K req/sec (capacity = 100K req/sec)
- **Threshold**: Alert if < 90% of capacity

### Freshness
- **What**: How current is data?
- **Measured**: Max age of data < threshold
- **Example**: Recommendations refreshed < 5 minutes old
- **Threshold**: Alert if freshness > 10 minutes

---

## Setting Good SLOs

### DO ✓

- Set SLO **based on actual current performance** (measure first)
- Use **different SLOs for different services** (criticality varies)
- Set SLO **0.05-0.1% stricter than SLA** (provides buffer)
- Use **percentiles for latency** (P50, P95, P99; not average)
- **Review SLOs quarterly** (business needs change)
- Set SLO **to match business requirements** (not technical limits)
- Use SLOs to **guide deployment decisions** (error budget)

### DON'T ✗

- Set SLO = SLA (no buffer for incidents)
- Set SLO = infrastructure uptime (99.99% database doesn't = 99.99% service)
- Set SLO based on "what sounds good" (99.99% because it sounds nice)
- Ignore error budgets (SLOs are useless if not used)
- Use only averages for latency (hides slow users)
- Set all services to same SLO (different criticality)
- Never review/adjust SLOs (world changes)

---

## Error Budget Usage

### Decision Framework

```
Can we deploy this risky change?

1. Calculate error budget remaining this period
2. Estimate risk (minutes of potential downtime)
3. Multiply risk estimate by likelihood

If: (Budget - Estimated Risk) > Safety Margin
    → APPROVE with monitoring
Else:
    → REJECT or reduce risk (feature flags, canary)
```

### Error Budget Burndown

```
Monthly budget: 43 minutes (99.9% SLO)

Day 1-5:   Spent: 2 min    Remaining: 41 min   (Pace: good)
Day 6-10:  Spent: 8 min    Remaining: 35 min   (Pace: good)
Day 11-15: Spent: 18 min   Remaining: 25 min   (Pace: on track)
Day 16-20: Spent: 45 min   Remaining: -2 min   (BREACHED! 7-day incident)
Day 21-25: Spent: 45 min   Remaining: -2 min   (Still breached, wait for reset)
Day 26-30: Spent: 45 min   Remaining: -2 min   (End of month, all breached)

Lesson: One large incident can consume entire month's budget
```

---

## SLI Measurement Table

| Service Type | Key SLI #1 | Key SLI #2 | Key SLI #3 |
|---|---|---|---|
| **Web API** | Availability (% 2xx) | P99 Latency | Error Rate |
| **Database** | Replication lag | Query latency P99 | Availability |
| **Cache** | Hit ratio | P99 fetch time | Availability |
| **Queue** | End-to-end latency | Queue depth | Consumer lag |
| **Search** | Query success % | P99 response time | Result freshness |
| **Payment** | Transaction success % | Latency P99 | Error rate |

---

## Calculating Monthly Uptime from Availability %

```
Availability % to Minutes Downtime (30 days)

99% = 432 minutes = 7.2 hours/month
99.5% = 216 minutes = 3.6 hours/month
99.9% = 43 minutes = 0.72 hours/month ← Most common
99.95% = 22 minutes = 0.37 hours/month
99.99% = 4.3 minutes = 0.07 hours/month
```

## Alert Thresholds

### For 99.9% SLO

| Metric | OK | WARNING | CRITICAL |
|--------|----|---------| ---------|
| Availability | > 99.8% | 99.5-99.8% | < 99.5% |
| P99 Latency | < 200ms | 200-250ms | > 250ms |
| Error Rate | < 0.05% | 0.05-0.1% | > 0.1% |
| Error Budget | > 20 min left | 10-20 min left | < 10 min left |

---

## SLO Violation Response

```
When you breach SLO:

IMMEDIATELY (within 1 hour):
- Page on-call engineer
- Create incident ticket
- Start status page update
- Gather metrics + logs

DURING incident:
- Active monitoring/response
- Customer communication (every 15 min)
- Root cause investigation begins

AFTER incident (within 24 hours):
- Post-mortem meeting scheduled
- Temporary mitigations in place
- Timeline documented

POST-MORTEM (within 1 week):
- Root cause analysis
- Action items to prevent recurrence
- Share learnings with team
```

---

## Questions to Ask Before Setting SLOs

1. **What do users care about?** → Defines SLIs
2. **What's the cost of 99.9% vs 99.99%?** → SLO realism
3. **What can we currently achieve?** → Baseline
4. **What does our competitor offer?** → Competitive parity
5. **What can we communicate to customers?** → SLA decision
6. **How will we measure this?** → Instrumentation
7. **Who will respond to breaches?** → On-call strategy

---

## Formulas Quick Reference

```bash
# Error Budget (minutes per month)
error_budget = (1 - slo_percentage) * 30 * 24 * 60

# Remaining Budget
remaining = total_budget - time_spent_on_incidents

# Safe to deploy?
if remaining > estimated_risk + 10_minute_safety_buffer:
    APPROVE
else:
    REJECT

# Monthly Uptime % to Minutes
minutes_downtime = (100 - uptime_percent) * 43200 / 100

# Incident Impact
minutes_of_budget = incident_downtime_minutes
if minutes_of_budget >= remaining_budget:
    status = "SLO BREACHED"
```

---

## Common Mistakes Quick Fixes

| Mistake | Fix |
|---------|-----|
| SLO = SLA | Add 0.05-0.1% buffer (SLO higher than SLA) |
| Infrastructure SLI | Measure end-to-end user experience |
| No error budget use | Tie deployment decisions to budget |
| SLO too high | Measure current performance, start there |
| All metrics same SLO | Different services = different SLOs |
| Never update SLOs | Review quarterly or when business changes |

---

**Remember**: 

> SLI/SLO/SLA are not just compliance. They're decision-making frameworks. 
> Use error budgets to balance velocity (shipping features) with stability (system reliability).
> Higher numbers aren't better—appropriate numbers are.

