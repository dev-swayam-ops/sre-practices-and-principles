# Cheatsheet: Change Management and Release Safety

Quick reference for safe deployments.

---

## Release Strategies Comparison

| Strategy | Time | Risk | Rollback | Use Case |
|----------|------|------|----------|----------|
| **Rolling** | Minutes | Medium | Slow | Non-critical services |
| **Canary** | Hours | Low | Fast | Critical + new code |
| **Blue-Green** | Seconds | Low | Instant | High-impact changes |

---

## Pre-deployment Checklist

```
☐ Code reviewed (2 engineers)
☐ Tests passing (unit + integration)
☐ Load tested (at 2x capacity)
☐ Database migration tested
☐ Rollback procedure documented
☐ Monitoring configured
☐ On-call notified
☐ Team aware (no surprises)
☐ Timing OK (not Friday 4pm)
☐ Communication plan ready
```

---

## Canary Deployment Template

```
PHASE 1: 5% traffic (30 min)
- Deploy new version to 5% of servers
- Monitor: Error rate, latency, resources
- Success criteria: Same as current (< 0.1% errors)
- If bad: Rollback immediately

PHASE 2: 25% traffic (30 min)
- Deploy to 25% of servers
- Monitor same metrics
- Same success criteria
- If bad: Rollback

PHASE 3: 100% traffic
- Deploy to all servers
- Monitor for 24 hours
- Watch for delayed issues

Rollback at any phase: < 2 minutes
```

---

## Monitoring Thresholds

| Metric | Good | Warning | Critical |
|--------|------|---------|----------|
| Error rate | < 0.1% | 0.1-1% | > 1% |
| Latency P99 | < 150ms | 150-300ms | > 300ms |
| CPU usage | < 50% | 50-80% | > 80% |
| Memory | < 60% | 60-80% | > 80% |
| Connections | < 70% | 70-90% | > 90% |

---

## Deployment Timing Guidelines

```
BEST TIMES:
✓ Tuesday-Thursday, 9am-3pm
✓ When team is present
✓ Before holidays/weekends

AVOID:
✗ Friday after 2pm
✗ Holidays/weekends
✗ Monday chaos hour
✗ Evening/nights (unless automated)
```

---

## Rollback Procedure

```
STEP 1: Declare incident (< 1 min)
- Call incident commander
- Page on-call team
- Status message

STEP 2: Rollback (1-2 min)
- kubectl rollout undo [deployment]
- OR switch blue-green
- Verify services restart

STEP 3: Verify (2-5 min)
- Check error rate
- Check latency
- Check health
- Confirm: Fixed?

STEP 4: Communicate (< 5 min)
- Update status page
- Notify stakeholders
- Estimate RCA timeline

Total: < 10 minutes to restore service
```

---

## Feature Flags Example

```
Code:
if (featureFlags.get("new_payment")) {
  // New implementation
} else {
  // Old implementation
}

Control:
newPaymentFlow:
  enabled: false       // Disable instantly
  percentage: 5        // OR canary 5% of users
  users: ["user1"]     // OR specific users only
```

---

## Load Test Acceptance Criteria

```
Must test at:
- 50% current capacity (normal)
- 100% current capacity (peak)
- 150% current capacity (stress)

Success criteria:
- Error rate: < 0.5%
- Latency P99: < 2x baseline
- CPU: < 90%
- Memory: < 90%
- No hanging connections

If any fails: Fix before deploying
```

---

## Database Migration Pattern

```
Deploy 1: Add new column
- Old code: Writes to old column
- New code: Not yet deployed

Deploy 2: Update code
- New code: Reads/writes new column
- Old column: Still exists (fallback)
- Rollback: Revert code, use old column

Deploy 3: Cleanup
- Drop old column after weeks
- Safe: Confirmed new code working
- No rollback option (go forward only)
```

---

## Common Deployment Issues & Fixes

| Issue | Prevention |
|-------|-----------|
| Unknown errors in prod | Load test, not just unit tests |
| Can't rollback fast | Pre-practice, automate |
| Deployment breaks SLA | Canary + monitoring |
| Database issues | Test migration first |
| Capacity exhausted | Load test at 2x |
| No one available on Friday | Deploy Tuesday-Thursday |

---

**Remember**: Safe deployments are planned, tested, monitored, and quickly reversible.
