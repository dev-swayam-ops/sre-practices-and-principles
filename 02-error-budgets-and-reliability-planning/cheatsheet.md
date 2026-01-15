# Cheatsheet: Error Budgets and Reliability Planning

Quick reference for error budget calculations and planning.

---

## Error Budget Formulas

```bash
# Monthly error budget (minutes)
Error Budget = (1 - SLO%) × 30 × 24 × 60

# Examples
99%:   ERROR_BUDGET = 432 min
99.5%: ERROR_BUDGET = 216 min
99.9%: ERROR_BUDGET = 43 min  ← Standard
99.95%: ERROR_BUDGET = 22 min ← Strict

# Daily burn rate
Daily Burn = Time Spent (min) / Days Elapsed

# Month-end forecast
Forecast = Daily Burn × 30

# Remaining budget
Remaining = Total - Spent

# Safe to deploy?
IF (Remaining > Risk + 10 min safety buffer) THEN DEPLOY
ELSE be conservative
```

---

## Budget Status Guide

| Budget Left | Status | Action |
|-------------|--------|--------|
| 30+ min | HEALTHY | Deploy risky changes, invest in reliability |
| 15-30 min | NORMAL | Deploy carefully, monitor closely |
| 5-15 min | CRITICAL | Only safe changes, no experiments |
| < 5 min | EMERGENCY | Freeze deployments, focus on stability |

---

## Deployment Risk Assessment

```
Risk Estimate × Probability = Budget Impact

Low Risk (1-3 min):
- Bug fixes
- Config changes
- Minor feature flags

Medium Risk (5-10 min):
- Service dependency changes
- Database schema updates
- API contract changes

High Risk (10-20+ min):
- Major refactors
- Core algorithm changes
- Infrastructure changes
```

---

## Reliability Investment Priorities

```
Budget Healthy (>30 min)?
├─ YES: Invest in reliability
│   ├─ Add redundancy
│   ├─ Improve monitoring
│   └─ Build resilience patterns
│
└─ NO: Pause deployments, improve stability
    ├─ Fix critical issues
    ├─ Optimize MTTR
    └─ Add safeguards
```

---

## Monthly Planning Checklist

- [ ] Calculate total budget at month-start
- [ ] Set deployment strategy (weekly targets)
- [ ] Identify critical reliability gaps
- [ ] Schedule reliability investments
- [ ] Brief team on budget constraints
- [ ] Track incidents daily
- [ ] Adjust strategy mid-month if needed
- [ ] Conduct post-month review

---

## Forecasting Indicators

| Indicator | Meaning | Action |
|-----------|---------|--------|
| Burn rate > 1.5 min/day | Will breach SLO | Pause deployments, improve reliability |
| Two incidents in one week | Pattern emerging | Investigate root causes |
| Incident MTTR > 30 min | Response too slow | Improve runbooks and alerting |
| Incidents same service | Systemic issue | Plan major improvement |

---

## Communication Templates

**Team Standup**:
"Budget status: $REMAINING min left, $DAYS days left. Daily burn: $RATE min/day. Forecast: ON TRACK / AT RISK."

**To Management**:
"We'll meet SLO this month. Recommend investing $X in reliability next month to reduce incident rate."

**To Customers**:
"SLA Status: HEALTHY. On track to meet 99.9% uptime guarantee."

---

## Quick Decision Tree

```
Should we deploy this change?

1. Calculate remaining budget
   ↓
2. Estimate deployment risk
   ↓
3. IF remaining > (risk + 10) THEN
      Deploy with monitoring
   ELSE
      Use feature flags OR
      Delay until next week OR
      Cancel (low priority)
```

---

**Remember**: Error budgets are communication tools. Share them with product, management, and customers. They transform abstract reliability into concrete business decisions.

