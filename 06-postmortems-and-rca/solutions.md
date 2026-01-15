# Solutions: Postmortems and Root Cause Analysis

Solutions for all 10 exercises.

---

## Solution 1: Symptoms vs Root Cause

**Answer**:

```
SYMPTOM: Website slow (10 seconds)

CAUSE: Database query slow (8 seconds)

ROOT CAUSE: No index on user table
- Query scans entire table (1M rows)
- Without index, takes 8 seconds
- With index, would take 50ms

How to verify root cause:
1. Check query execution plan
2. See if index is missing
3. Add index, test latency drops

Real fix: Add index on user_id column
```

---

## Solution 2: 5 Whys for Email Bug

**Answer**:

```
Q1: Why aren't emails sending?
A: Email service returns 500 error

Q2: Why 500 error?
A: SMTP authentication fails

Q3: Why authentication fails?
A: Credentials in config are wrong

Q4: Why wrong credentials?
A: Credentials rotated, but config not updated

Q5: Why no update notification?
A: No automated credential sync process

ROOT CAUSE: No automation for credential updates

Real fix options:
1. Use secrets manager (auto-sync)
2. Alert on credential rotation
3. Automate credential updates
4. Add test that checks connectivity

Best: Add secrets manager + automated tests
```

---

## Solution 3: Blame vs Blameless

**Answer**:

```
BLAME (wrong):
"Engineer was careless and didn't test."

BLAMELESS (right):
"We lacked automated tests, and manual testing 
takes 2 hours. Under time pressure, the engineer 
skipped manual test. The system didn't enforce 
testing."

System changes:
1. Add automated test suite
2. Require tests before merge
3. Run tests in CI/CD (prevent untested code)
4. Provide training on testing

Why blameless works:
- Fixes system, not person
- Prevents next engineer from repeating
- Creates safe culture (people report mistakes)
- Actually prevents incidents
```

---

## Solution 4: Timeline and Detection Lag

**Answer**:

```
Detection lag: 10 minutes (10:20 slow → 10:30 alert)

Why so long?
- Metrics collected every 1 minute
- Alert evaluated every 5 minutes
- Alert triggered, then sent (5 min total)
- Result: 10 min lag from slow to alert

How to make faster:
1. Increase metric frequency (1s instead of 1m)
2. Evaluate alerts every 5 seconds (not 5 min)
3. Use real-time alerts (not batch)
4. Target: < 1 minute latency

Cost: More CPU for metrics
Benefit: Detect problems faster
```

---

## Solution 5: Repeated Incident Root Cause

**Answer**:

```
Why repeating?
- Incident 1 fix: Restart (temporary)
- Incident 2 fix: Bigger pool (temporary)
- Incident 3: Problem returns (not fixed)

Real root cause: Something keeps opening too many 
connections

Possible causes:
1. Code leaks connections (doesn't close them)
2. Traffic spike opens new connections
3. Connection timeout too long
4. Application creates many short-lived connections

Real fix (find the actual cause):
1. Monitor connection count over time
2. Find what opens connections
3. Fix the leak or limit connections
4. Add alert for pool > 80%

Action:
- Find code that opens connections
- Fix leak or add pooling
- Test under high load
- Monitor daily
```

---

## Solution 6: Impact Calculation

**Answer**:

```
Failed transactions:
10,000 users × 20% failure = 2,000 failed transactions
2,000 × $50 = $100,000 lost revenue

SLA violation:
- SLA: 99.9% uptime = 43.2 minutes down/month
- Incident: 15 minutes
- This incident: 15 / (30 days × 24 hours × 60 min) 
             = 0.007% downtime
- SLA violation: Yes (0.007% > 0.1% allowed)

Business impact:
- Revenue loss: $100,000
- Customer trust: Damaged (users failed transactions)
- Reputation: Negative feedback
- Churn risk: Some customers may leave

Prevention:
- Add monitoring/alerts
- Improve deploy process
- Implement canary deploys
```

---

## Solution 7: Action Items

**Answer**:

```
ROOT CAUSE: No alert before cert expiry

PREVENTIVE (stop it happening):
1. Implement cert monitoring (Owner: John, Due: Friday)
   - Set alert 30 days before expiry
   - Automate cert renewal
   
2. Use automatic renewal (Owner: Sarah, Due: Next week)
   - Let-'s Encrypt with auto-renewal
   - Eliminates manual renewal

DETECTIVE (find faster if it happens):
3. Add cert expiry alert (Owner: Samantha, Due: Friday)
   - Monitor: Certificate expiration date
   - Alert: 30 days before, 7 days before, 1 day before

REACTIVE (help next time):
4. Create SSL troubleshooting runbook (Owner: Team, Due: Friday)
   - Steps to check cert status
   - Steps to renew manually
   - Steps to restart service

TIMELINE:
- This week: Cert alert, troubleshooting runbook
- Next week: Auto-renewal implementation
- Ongoing: Monitor cert dashboard
```

---

## Solution 8: Blameless Culture Impact

**Answer**:

```
If you fire them (blame culture):
❌ Engineer gets scared
❌ Next engineer hides mistakes
❌ Incident repeats with different person
❌ Culture of blame, not learning
❌ Good engineers leave (feel unsafe)

If you keep them (blameless culture):
✓ Engineer learns from mistake
✓ Team learns what went wrong
✓ System improves (prevents repeats)
✓ Engineer becomes more careful
✓ Incident might not repeat

Data:
- Google's SRE: Blameless postmortems → Fewer repeats
- Companies with blame culture → Higher incident rates
- Blameless teams report more incidents (safe to report)

Conclusion:
Keep the engineer, improve the system.
The engineer didn't cause the incident; 
the system allowed it.
```

---

## Solution 9: Fishbone Diagram

**Answer**:

```
Build fishbone with branches:

            DATABASE
              / \
        Slow   No
       Query  Cache
         \    /
API LATENCY SPIKE
         /    \
    Leak   High
   Memory  Memory
      \    /
            CODE

Find root:
1. Check query execution plan → Slow query found
2. Check database CPU → Not high (not database)
3. Check code memory leak → Found: Thread pool grows
4. Check network latency → Normal (not network)

Root cause: Thread pool memory leak

Fix:
- Fix thread pool code
- Add memory monitoring
- Test under high load
```

---

## Solution 10: Postmortem Follow-up

**Answer**:

```
Why follow-up matters:
- Postmortem only works if items are completed
- If ignored, same incident repeats
- Accountability: Who's responsible?
- Verification: Did fix actually work?

Tracking method:
Week 1: All items assigned
Week 2: Check progress (50% done?)
Week 3: Complete overdue items
Week 4: Final verification + close

What happens if ignored:
- Item not done → Same incident happens
- Example: "Add monitoring" ignored
  → Another outage, could have been prevented
- Credibility lost: Team ignores postmortems

Solution:
1. Assign clear owner to each item
2. Set deadline (not just "soon")
3. Check progress every week
4. Don't start new incidents without completing fixes
5. Report: "X% action items completed"
```

---

## Key Principles

1. **Root cause**: Ask "Why?" 5 times
2. **Blameless**: Focus on system, not person
3. **Action items**: Concrete, assigned, deadlined
4. **Follow-up**: Track until complete
5. **Learning**: Prevent repeats
