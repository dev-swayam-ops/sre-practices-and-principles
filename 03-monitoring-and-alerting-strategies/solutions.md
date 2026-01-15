# Solutions: Monitoring and Alerting Strategies

Solutions for all 10 exercises.

---

## Solution 1: Identify SLI Metrics

**Answer**:

```
E-Commerce Checkout (what users care about):

SLI #1 - Availability
  Metric: % of checkout requests that complete successfully
  Why: If checkout fails, revenue is lost
  Exclude: "API uptime" (infrastructure, not user experience)

SLI #2 - Latency
  Metric: P99 checkout completion time
  Why: Slow checkout = abandoned carts
  Exclude: "Database latency" (too low-level)

SLI #3 - Error Rate  
  Metric: % of checkouts returning error
  Why: Errors directly impact revenue
  Exclude: "Cache hit rate" (internal detail)
```

---

## Solution 2: Write Alert Thresholds

**Answer**:

```
Baseline: 0.08% error rate (normal)

WARNING threshold: 0.15% (roughly 2x normal)
- Alert after: 10 minutes (allow noise filtering)
- Severity: Yellow, notify team
- Implies: Degradation, not emergency

CRITICAL threshold: 0.5% (more than 5x normal)
- Alert after: 2 minutes (fast page on-call)
- Severity: Red, page engineer immediately
- Implies: Major outage

Rationale:
- Don't alert at every tiny increase (0.09%)
- Do alert when it's clearly abnormal (2x+)
- Critical threshold > WARNING threshold
- Wait time filters noise but catches real issues quickly
```

---

## Solution 3: Dashboard Design

**Answer**:

**At a Glance (Top, Large)**:
1. Availability (% green/red indicator)
2. Error rate (current %)
3. P99 latency (current ms)
4. Error budget remaining (%)
5. Time since last incident

**For Troubleshooting (Lower, Detailed)**:
- Request volume by endpoint
- Error rate by type (4xx vs 5xx)
- Latency percentiles (P50, P95, P99)
- Resource usage (CPU, memory, disk)
- Dependency health (databases, caches, APIs)

---

## Solution 4: Alert Fatigue Analysis

**Answer**:

```
47 alerts is too many. Regroup:

CRITICAL (page engineer immediately):
- Availability < 99%
- Error rate > 0.5%
- Core dependency down
→ ~5 alerts

WARNING (notify team):
- Availability 99-99.5%
- Error rate 0.2-0.5%
- Minor degradation
→ ~5 alerts

INFO (log only):
- All other infrastructure metrics
→ Ignore or aggregate

Result: 10 meaningful alerts instead of 47
(team will actually respond to these)
```

---

## Solution 5: Runbook Creation

**Answer**:

```
RUNBOOK: High Error Rate (> 0.5%)

Step 1: Assess Severity (1 min)
  - Check error rate graph
  - Is it still rising? Or already stabilizing?
  - Look at last incident postmortem - same issue?

Step 2: Identify Error Type (2 min)
  - Are errors 4xx (client) or 5xx (server)?
  - View error logs for patterns
  - Recent deploy? Check deployment log.

Step 3: Check Dependencies (3 min)
  - Is database accessible?
  - Are external APIs responding?
  - Check monitoring for downstream services.

Step 4: If Easy Fix (5 min)
  - Restart service? Rollback? Scale up?
  - Apply known fix from postmortem.

Step 5: If Complex (escalate)
  - Get help from team lead.
  - Create incident ticket.
  - Start observability investigation.

Total time to diagnosis: ~5 minutes
```

---

## Solution 6: Metric Selection

**Answer**:

```
ALERT ON:
✓ Error rate: 0.3% (direct user impact)
✓ Latency P99: 250ms (users waiting = bad)

DON'T ALERT ON:
✗ Database CPU: 85% (might be fine for hours)
✗ Memory usage: 78% (kernel can evict if needed)

When infrastructure metrics matter:
- Database CPU: Only if query latency also increased
- Memory: Only if causing slowness (monitor latency instead)

Rule: Alert on user-facing impact, not resource utilization
```

---

## Solution 7: Alert Threshold Tuning

**Answer**:

```
50 fires/day for "P99 > 200ms" means:

OPTION A: Threshold too sensitive
- Reality: P99 is often > 200ms, this is normal
- Solution: Increase to P99 > 300ms or higher

OPTION B: Threshold correct but service degrading
- Reality: Service often slow, legitimately problematic
- Solution: Investigate and fix root cause

How to decide:
1. Check historical graph: Is P99 always > 200ms?
2. Is SLO met? (If SLO met, alert threshold too strict)
3. Are users complaining? (If no, not a real problem)

Most likely: Threshold too strict
Action: Increase to P99 > 300ms OR investigate why so slow

Result: Alert fires only for real problems
```

---

## Solution 8: Multi-Tier Alerting

**Answer**:

```
INFO (Log only, don't notify):
- CPU > 60%
- Memory > 70%
- Disk > 80%
(These are normal, not actionable)

WARNING (Slack notification):
- Error rate 0.1-0.3%
- P99 latency 200-400ms
- Availability 99-99.5%
(Needs monitoring, not urgent)

CRITICAL (Page on-call):
- Error rate > 0.5%
- Availability < 99%
- Any service dependency down
(Immediate action needed)

Decision logic:
- Would you wake engineer for this? → CRITICAL
- Should team know about this? → WARNING
- Just noise? → INFO
```

---

## Solution 9: Dashboards for Different Audiences

**Answer**:

**For On-Call Engineer**:
- Availability (real-time)
- Error rate (by type)
- Latency percentiles
- Recent deployments
- Dependency status
- Resource utilization
(Detailed, troubleshooting focus)

**For Product Manager**:
- SLO attainment (%)
- Error budget remaining
- Incident count (this month)
- Feature performance (new features slower?)
- Comparison to competitors (if known)
(Business metrics focus)

**For Customers (Public Status)**:
- Overall system status (Green/Yellow/Red)
- Availability (last 30 days)
- Incident history
- Known issues
(High-level, trust-building)

Key: Each audience sees different level of detail
```

---

## Solution 10: Incident Response with Alerts

**Answer**:

```
Three alerts fire simultaneously:
1. High error rate (0.8%)
2. High latency (P99 = 500ms)
3. Low availability (98%)

Triage order: START WITH ERROR RATE

Why?
- Error rate is most direct user impact
- If requests are erroring, that explains everything
- High error + high latency + low availability = one problem

Diagnosis tree:
├─ 4xx errors? → Application bug, check recent deploy
├─ 5xx errors? → Server problem
│  ├─ Database down? → Latency + errors
│  ├─ Memory error? → Latency + errors
│  └─ Service crashed? → Availability + errors
└─ Mixed? → Dependency timeout

Resolution order:
1. Check recent deployments (last 2 hours)
   If found → Rollback
2. If not deploy, check database
   If down → Restart/failover
3. If still no cause → Check logs for patterns
```

---

## Summary: Best Practices

1. Alert on SLI metrics (availability, latency, errors)
2. Don't alert on infrastructure (CPU, memory, disk)
3. Alert thresholds = 1.5-2x baseline (not hair-trigger)
4. Fewer alerts = better response (5-10 critical, 5-10 warning)
5. Every alert needs a runbook
6. Dashboards should tell the story at a glance

Next: Implement these in your monitoring system (Prometheus, DataDog, CloudWatch, etc.)
