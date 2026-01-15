# Solutions: Change Management and Release Safety

Solutions for all 10 exercises.

---

## Solution 1: Pre-deployment Checklist

**Answer**:

```
5 Critical checks before deploy:

1. CODE REVIEW (prevents bugs)
   - 2 engineers reviewed
   - No security issues
   - Follows standards
   
2. TESTING (catches regressions)
   - Unit tests pass
   - Integration tests pass
   - No failures
   
3. LOAD TEST (prevents capacity issues)
   - Tested at 2x current load
   - Performance acceptable
   - No memory leaks
   
4. ROLLBACK PROCEDURE (enables quick recovery)
   - Documented + practiced
   - Takes < 5 minutes
   - Instant or fast
   
5. MONITORING (detects problems)
   - Alerts configured
   - Dashboards ready
   - On-call notified

If you skip:
- Code review → Bugs reach production
- Testing → Regressions (things break)
- Load test → Crashes under normal load
- Rollback procedure → Can't recover from issues
- Monitoring → Don't know if deployment broke
```

---

## Solution 2: Choose Release Strategy

**Answer**:

```
1. INTERNAL API (non-critical)
   Strategy: ROLLING
   Why: Non-critical, lower risk
   Timeline: Standard deployment
   Rollback: Can wait hours if needed

2. PAYMENT API (critical)
   Strategy: CANARY
   Why: Payment = highest impact
   Procedure: 5% → 25% → 100%
   Advantage: Catch issues at 5% before full rollout
   Rollback: Instant if issues found

3. USER PROFILE (moderate impact)
   Strategy: BLUE-GREEN
   Why: Affects many users, need instant switch
   Advantage: Toggle between versions instantly
   Rollback: Instant (just switch back)

Key differences:
- Rolling: No downtime, gradual
- Canary: Test first, then rollout
- Blue-green: Instant switch, instant rollback
```

---

## Solution 3: Rollback Procedure

**Answer**:

```
When issue detected:

STEP 1: DECLARE (right now!)
- Call incident commander
- Page on-call team
- Status: "Rolling back deployment"

STEP 2: ROLLBACK (immediately, don't debug)
- kubectl rollout undo
- OR: Switch blue-green
- Time: < 2 minutes

STEP 3: VERIFY (is it fixed?)
- Check error rate
- Check latency
- Check customer complaints
- Time: 2-5 minutes

STEP 4: INVESTIGATE (after service stable)
- Root cause analysis
- Why did it slip past testing?
- Fix: Update tests, deployment process
- Time: After incident resolved

Timeline:
- Seconds: Declare problem
- Seconds: Rollback initiated
- Minutes: Service restored
- Hours: Root cause investigation
- Days: Fix + redeploy

Key: Rollback BEFORE investigation
(Service health > understanding problem)
```

---

## Solution 4: Canary Metrics

**Answer**:

```
Current error rate: 0.05%
Canary error rate: 0.08%
Change: +0.03% (60% increase)

Current latency P99: 150ms
Canary latency P99: 200ms
Change: +50ms (33% increase)

DECISION: ROLLBACK (too much change)

Why?
- Error rate increased 60%
  Expected acceptable: < 10% increase
  Actual: 60% (too much)
  
- Latency increased 33%
  Expected acceptable: < 10% increase
  Actual: 33% (too much)

Both indicate problem in new code
Rolling out 100% would be risky

Acceptable change thresholds:
- Error rate: < 10% increase
- Latency: < 10% increase
- Success rate: < 1% decrease
- Resource usage: < 20% increase

Monitoring duration:
- 30 minutes minimum (catch obvious issues)
- Watch for trending (degrading slowly?)
- Delayed issues (memory leak appears later)
```

---

## Solution 5: Database Migration

**Answer**:

```
Challenge: Schema change + zero downtime + instant rollback

Solution: EXPAND-CONTRACT pattern

Step 1 (Expand): Add new column
- Old code: Writes to old column
- New code: Not deployed yet
- Can rollback: Just drop new column

Step 2 (Migrate): Copy data
- Old column → New column
- Slowly, in background
- No service disruption

Step 3 (Backfill): Update code
- New code: Reads/writes new column
- Old column: Still exists (fallback)
- Can rollback: Code reverts to old column

Step 4 (Cleanup): Drop old column
- New code running for weeks
- Confident: No issues
- Safe to drop old column

Timeline:
Deploy 1: Add column (backward compatible)
Deploy 2: Code uses new column (can fallback to old)
Deploy 3: Drop old column (cleanup)

Rollback at each step:
- Before deploy 1: Nothing to rollback
- After deploy 1: Drop new column
- After deploy 2: Code reverts to old column
- After deploy 3: New column required (can't rollback)

Key: Make changes backward compatible
```

---

## Solution 6: Feature Flags

**Answer**:

```
Feature flags decouple code deploy from feature enable

Example:
// Code (deployed)
if (featureFlags.newPaymentFlow) {
  // Use new code
} else {
  // Use old code
}

// Feature flag (controlled by config)
newPaymentFlow: true / false

Benefits:
✓ Deploy code before feature is ready
✓ Enable/disable without redeploy
✓ Canary: Enable for 5% of users
✓ Instant rollback: Just disable flag
✓ A/B testing: 50% users get feature

When to use:
✓ New features (not ready to release)
✓ Risky changes (gradual rollout)
✓ A/B testing (measure impact)
✓ Kill switches (disable if issues)

When NOT needed:
✗ Bug fixes (deploy = enable)
✗ Performance optimizations (no flag)
✗ Backwards-incompatible changes (complex)

Implementation:
- Config file (simple but requires redeploy to change)
- Database (dynamic, can change anytime)
- Service (request-based targeting)
```

---

## Solution 7: Deployment Timing

**Answer**:

```
BEST: Tuesday 10am (maintenance window)
Why:
- Engineering team present
- Not busy (can focus)
- If issue: Can work on it all day
- Not Friday (avoid weekend issues)
- Not Monday (avoid Monday chaos)

WORST: Friday 4pm
Why:
- Issue happens at 5pm
- Engineers leaving
- No one available to fix
- Weekend support unavailable
- Issue persists until Monday

Alternative: Monday 2pm (busy)
Why:
- Team is in, focused mode
- Can address issues
- But: Competing with other work
- Less ideal than Tuesday

Off-hours deployment (night):
- Only if fully automated
- Minimal risk (bug fix, simple change)
- Monitoring in place
- On-call engineer watching
- Not for critical systems

Ideal schedule:
- Tuesday-Thursday: 9am-3pm
- NOT Friday after 2pm
- NOT Monday chaos time
- NOT evenings/weekends
- Ask team: When do they feel safe deploying?
```

---

## Solution 8: Load Test Evaluation

**Answer**:

```
Test results:
- 1,000 req/sec: OK
- 2,000 req/sec: 5% error rate
- 3,000 req/sec: 25% errors

Current production: 1,500 req/sec
Max observed: 1,800 req/sec

SAFETY ANALYSIS:
- At 1,500 req/sec: Works fine
- At 2,000 req/sec: 5% error rate (NOT OK)
- Safety margin: Only 500 req/sec above current

DECISION: NOT SAFE TO DEPLOY

Why?
- Current load is 1,500 req/sec
- Forecast peak could be 2,000 req/sec (normal growth)
- At 2,000: 5% error rate (unacceptable)
- SLO: 99.9% success = 0.1% error rate
- 5% > 0.1% = VIOLATES SLO

What to do:
1. Optimize code (cache, indexes, etc.)
2. Re-load test
3. If still < 2,000 req/sec capacity: Add servers

Alternative analysis:
If you trust current won't exceed 1,800 req/sec:
- At 1,800: Extrapolate ~2% error rate
- Still too high (SLO: 0.1%)
- Still NOT safe
```

---

## Solution 9: Monitoring Priority

**Answer**:

```
MOST IMPORTANT: Error rate
Why:
- Directly impacts users
- Indicates deployment failure
- Actionable: If high → rollback
- Threshold: > 0.2% = investigate

SECOND: Latency (P99)
Why:
- Users notice slowness
- May indicate resource issue
- Threshold: > 2x normal = investigate

THIRD: CPU/Memory/DB
Why:
- Indirect impact
- May not notice immediately
- Useful for diagnosis later

Check frequency:
- First 5 minutes: Every 30 seconds
- Next 25 minutes: Every 2 minutes
- Next 30 minutes: Every 5 minutes
- After 1 hour: Every 15 minutes
- After 6 hours: Daily checks

Total monitoring:
- Critical window: 1 hour (tight monitoring)
- Extended window: 24 hours (periodic checks)
- Long-tail issues: 7 days (weekly review)

If any metric bad:
- Don't investigate
- Just rollback
- Investigate after service stable
```

---

## Solution 10: Blast Radius

**Answer**:

```
OPTION A: 100% outage
- All users affected
- Easy to detect (immediate complaints)
- Instant visibility
- All monitoring screams
- Faster to respond (obvious problem)
- BUT: More damage while you respond

OPTION B: 10% canary
- 10% of users affected
- Harder to detect (noise in metrics)
- Need good monitoring
- May see 0.1% error rate (not obvious)
- Slower to respond (must identify pattern)
- BUT: 90% users unaffected

How canary prevents option A:

Without canary:
New code deployed to 100%
↓
Bug found
↓
All users affected
↓
$1M revenue loss
↓
Rollback (10 minutes)

With canary:
New code deployed to 5%
↓
Bug found (in 5% monitoring)
↓
Only 5% affected
↓
$50K revenue loss
↓
Rollback instant

Advantage: Canary limits damage to 5% vs 100%
Cost: Takes slightly longer to detect (need monitoring)
```

---

## Key Principles

1. **Plan**: Checklist, strategy, monitoring
2. **Test**: Load test, not just unit tests
3. **Deploy**: Canary for risky changes
4. **Monitor**: Error rate is key metric
5. **Rollback**: Fast and practiced
