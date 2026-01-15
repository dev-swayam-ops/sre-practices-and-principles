# Cheatsheet: Monitoring and Alerting Strategies

Quick reference for monitoring design and alert thresholds.

---

## Monitoring Pyramid

```
               User Impact ↑
          ┌─────────────────┐
          │  SLI Metrics    │ ← Availability, latency, errors
          │  (User-facing)  │
          ├─────────────────┤
          │  Service Metrics│ ← Per-endpoint metrics
          │  (Application)  │
          ├─────────────────┤
          │  Infrastructure │ ← CPU, memory, disk
          │  (Components)   │
          └─────────────────┘
       Measurement Detail ↓

Rule: Alert on SLI, monitor service, observe infrastructure
```

---

## What to Monitor vs. Alert On

| Metric | Monitor? | Alert? | Reason |
|--------|----------|--------|--------|
| Availability | ✓ | ✓ | Direct user impact |
| Error rate | ✓ | ✓ | Direct user impact |
| P99 latency | ✓ | ✓ | Affects user experience |
| CPU usage | ✓ | ✗ | Doesn't guarantee problem |
| Memory usage | ✓ | ✗ | OS handles with swapping |
| Disk usage | ✓ | ! | Alert if > 90% (space issues) |
| Network latency | ✓ | ✗ | Can't control it |

---

## Alert Threshold Templates

```
SLI Metric      Baseline    Warning           Critical
─────────────   ────────    ────────────────  ──────────────────
Availability    99.9%       < 99.5% (5m)      < 99% (2m)
Error Rate      0.1%        > 0.2% (5m)       > 0.5% (2m)
P99 Latency     150ms       > 250ms (5m)      > 500ms (2m)
P95 Latency     100ms       > 200ms (10m)     > 300ms (5m)

Wait time = how long condition must be true before alerting
(filters temporary spikes)
```

---

## Runbook Template

```
ALERT: [Alert Name]

SEVERITY: [Critical/Warning/Info]
OWNER: [Team/On-call]

DIAGNOSIS (What's wrong?):
  1. Check [specific metric]
  2. Look for [pattern]
  3. Review [logs/traces]

RESOLUTION (How to fix):
  EASY FIX:
  - Action: [restart/scale/rollback]
  - Time: ~[minutes]
  
  COMPLEX:
  - Step 1: [Investigate]
  - Step 2: [Gather data]
  - Step 3: [Escalate]

EXPECTED OUTCOME: [Metric returns to normal]
```

---

## Alert Design Checklist

- [ ] Alert based on SLI metric (not infrastructure)
- [ ] Threshold is 1.5-2x baseline (not too sensitive)
- [ ] Wait time = 5-10 min warning, 1-5 min critical
- [ ] Runbook exists and tested
- [ ] Team knows who to notify
- [ ] Dashboard exists for this metric
- [ ] Alert name is clear/actionable
- [ ] Combined with related alerts (not 47 separate)

---

## Dashboard Layout

```
┌─ QUICK STATUS (at top) ─────────────────────┐
│ System: GREEN / YELLOW / RED                │
│ Availability: 99.87% | Error Rate: 0.05%   │
│ P99 Latency: 145ms | Error Budget: 28min   │
└─────────────────────────────────────────────┘

┌─ TIME SERIES GRAPHS (6-24 hour window) ─────┐
│ [Availability] [Error Rate] [Latency]       │
│                                             │
│ [Requests/sec] [Resource %] [Errors Type]  │
└─────────────────────────────────────────────┘

┌─ DETAILED DRILL-DOWN ──────────────────────┐
│ By endpoint: Error rate | Latency | Volume │
│ By error type: 4xx | 5xx | Timeout | Other │
│ Dependencies: Health of DB, Cache, APIs     │
└─────────────────────────────────────────────┘
```

---

## Common Anti-Patterns & Fixes

| Anti-Pattern | Problem | Fix |
|---|---|---|
| Alert on CPU > 80% | Too noisy | Alert on error rate instead |
| No alert runbook | Team wastes time | Create 5-step runbooks |
| 47+ active alerts | All ignored | Consolidate to 5-10 critical |
| Dashboard everywhere | Can't see signal | One dashboard per audience |
| Alert on forecast | Too many false alarms | Alert on actual conditions |
| No wait time | Noisy on spikes | Add 5-10 min wait time |

---

## SLI Alert Framework

```
IF SLI breaches SLO for X minutes
  THEN page on-call
  
Example (99.9% SLO, 43 min budget):

IF availability < 99.5% for 5 minutes
  THEN alert WARNING (trend getting bad)

IF availability < 99% for 2 minutes
  THEN alert CRITICAL (page engineer)

IF error rate > 0.2% for 5 minutes
  THEN alert WARNING

IF error rate > 0.5% for 2 minutes
  THEN alert CRITICAL
```

---

## Implementation Tools

Most platforms support this pattern:
- Prometheus (open source)
- Datadog
- New Relic
- CloudWatch (AWS)
- Stackdriver (GCP)
- Azure Monitor

Key: They all let you define thresholds + wait times + actions.

---

**Remember**:
- Monitor broadly (collect lots of data)
- Alert narrowly (only on real problems)
- Dashboard widely (share what you see)
- Runbook deeply (fast resolution)

