# Solutions: SRE Operational Runbooks

Solutions for all 10 exercises.

---

## Solution 1: Identify Alert

**Answer**:

```
1. CPU > 80%
   Problem: Service slow/unresponsive
   Quick check: ps aux --sort=-%cpu | head
   Likely cause: Runaway process, high load
   
2. Response time > 1 sec
   Problem: Users experience slow site
   Quick check: curl -w "@curl-format.txt" http://service
   Likely cause: Slow DB query, network latency
   
3. Error rate > 5%
   Problem: Many requests failing
   Quick check: tail -f /var/log/app.log | grep ERROR
   Likely cause: Service down, bad deployment
   
4. Disk > 90%
   Problem: May run out of space
   Quick check: df -h / (root), du -sh /var/log/*
   Likely cause: Large log files, disk leak
```

---

## Solution 2: Decision Tree for High Error Rate

**Answer**:

```
High error rate detected
│
├─ Check service health (systemctl status)
│  ├─ Service DOWN → Restart service → Done
│  └─ Service UP → Continue
│
├─ Check recent deployments (git log, docker images)
│  ├─ Bad deployment → Rollback → Done
│  └─ No recent change → Continue
│
├─ Check database connectivity (mysql ping, pg_isready)
│  ├─ DB down → Alert DB team → Done
│  └─ DB up → Continue
│
├─ Check slow queries (SHOW PROCESSLIST, EXPLAIN)
│  ├─ Long queries found → Kill queries → Done
│  └─ Queries normal → Continue
│
└─ Check logs for errors
   └─ Specific error → Handle accordingly
```

---

## Solution 3: Expected Outputs

**Answer**:

```
HEALTHY output:
● service.service - My Service
  Active: active (running) ✓
  CPU: 5%
  Memory: 200M

BROKEN output:
● service.service - My Service
  Active: inactive (dead) ✗
  Error: Failed to start

UNCLEAR output:
● service.service - My Service
  Active: failed (Result: exit-code)

Action by output:
✓ Active + running → All good
✗ Inactive/failed → Restart
? State unclear → Check logs

Runbook should specify:
"If status shows 'Active: active' → Service healthy
 If status shows 'inactive' or 'failed' → Restart
 If status shows 'restarting' → Wait 30 sec, recheck"
```

---

## Solution 4: Rollback Plan

**Answer**:

```
RESTART SERVICE runbook:

Before restart:
- Check if safe (no data in flight)
- Note current version
- Have rollback plan

Restart:
$ systemctl restart service
$ sleep 5
$ systemctl status service

If restart fails:
→ Rollback: systemctl start service (previous version)
→ Check: systemctl status service
→ If still fails: Manual deployment of known good version

If restart succeeds but still broken:
→ Rollback: Deploy previous version
→ If problem persists: Not a restart issue
→ Escalate: Page expert, open incident

Critical: ALWAYS have previous version
- Keep deployed artifact available
- Test rollback procedure
- Document rollback steps
```

---

## Solution 5: Actionable Steps

**Answer**:

```
WRONG: "Check logs"
- Too vague
- Unclear what to look for
- Different for each service

BETTER:
"Check application error logs:
 $ tail -100 /var/log/app.log | grep -i error
 
 Look for:
 ✓ Database connection errors → DB down
 ✓ OutOfMemory errors → Memory leak
 ✓ HTTP 500 errors → Code crash
 ✓ Timeout errors → Slow dependency
 
 If 50+ errors in 1 min:
 → Restart service
 → If persists → Rollback"

Makes it:
✓ Specific (exact command)
✓ Actionable (what to look for)
✓ Decision-based (what to do next)
```

---

## Solution 6: Testing Runbook

**Answer**:

```
TEST RUNBOOK before production:

1. STAGING TEST
   - Simulate the problem (high memory)
   - Follow runbook exactly
   - Measure time to resolution
   - Verify expected outputs match
   - Document any issues

2. TEAM DRILL
   - Run runbook without incident
   - Make sure team knows it exists
   - Measure time to understand
   - Get feedback
   - Improve clarity

3. CHAOS TEST
   - Introduce real failure
   - Let team use runbook
   - See what breaks
   - Update runbook
   - Repeat quarterly

What could break:
❌ Commands don't exist
❌ Wrong syntax for your environment
❌ Expected output different
❌ Missing prerequisites
❌ Runbook incomplete

When to practice:
- Before on-call shift
- After each major change
- Quarterly team drill
- After incident (verify runbook)
```

---

## Solution 7: Simplify for Clarity

**Answer**:

```
COMPLEX RUNBOOK:
- 50+ steps
- Many nested branches
- Hard to remember
- Easy to skip steps

UNDER INCIDENT PRESSURE:
- Panic, can't think clearly
- Skip complex steps
- Make wrong decision
- Situation worse

SOLUTION:

1. CREATE QUICK VERSION:
   - First 5 critical steps only
   - Clear decision: Continue or escalate?
   - Done in 5 minutes

2. CREATE FULL VERSION:
   - For deep investigation
   - Reference if quick fix doesn't work
   - No time pressure

3. USE CHECKLISTS:
   - Clear boxes ☐
   - Can't forget steps
   - Removes cognitive load

Example:
QUICK (5 min):
☐ Check service status
☐ Check recent errors
☐ If fixable → Fix, else escalate

FULL (for reference):
[Detailed steps if simple fix didn't work]
```

---

## Solution 8: Runbook vs Automation

**Answer**:

```
AUTOMATE if:
✓ Detectable automatically (metric/alert)
✓ Safe action (no data loss)
✓ Same fix every time
✓ Fast resolution matters
Examples:
- Auto-restart crashed service
- Auto-scale if CPU high
- Auto-clear cache if memory high

KEEP MANUAL if:
✗ Needs context/judgment
✗ Irreversible action
✗ Different fixes per situation
✗ Rare/complex problem
Examples:
- Database schema changes
- Data corruption recovery
- Scaling decisions
- Financial transactions

DECISION FRAMEWORK:
Cost of automation > Cost of manual?
→ Don't automate
Cost of automation < Cost of manual?
→ Automate

Example: "Restart service"
- Auto: Cheap to automate, low risk, saves time
→ AUTOMATE

Example: "Database failover"
- Manual: Complex, data critical, requires judgment
→ MANUAL
```

---

## Solution 9: Escalation Paths

**Answer**:

```
ESCALATION DECISION:

1. THRESHOLD: Can runbook resolve it?
   ✓ Yes → Execute runbook → Done
   ✗ No → Continue

2. COMPLEXITY: Is it beyond runbook?
   ✓ Simple fix → Try one more thing
   ✗ Complex → Escalate immediately

3. TIME: How long is acceptable?
   ✓ 5 minutes left → Escalate now
   ✗ 30 minutes left → Try one more step

ESCALATION LEVELS:

Level 1: On-call for service
Level 2: Team lead (complex debugging)
Level 3: Platform team (infrastructure)
Level 4: Vendor support (external service)

EXAMPLE:
Step 1: Try restart (runbook)
Step 2: Try rollback (runbook)
Step 3: If still broken after 10 min → Escalate to team lead
Step 4: If no progress after 20 min → Page on-call engineer
Step 5: If ongoing > 1 hour → Declare SEV-1 incident

When passing info:
- What you tried
- What worked/didn't
- Current status
- Time since incident started
- Business impact
```

---

## Solution 10: Keep Runbooks Current

**Answer**:

```
RUNBOOK MAINTENANCE:

Update frequency:
✓ After major system change
✓ After incident (if runbook used)
✓ Quarterly review
✓ When commands fail

How to know outdated:
❌ Commands don't exist
❌ Service names changed
❌ Config file locations different
❌ New dependencies added
❌ Deprecated features used

MAINTENANCE PROCESS:

1. ASSIGN OWNER:
   - Team owns runbook
   - Someone responsible for updates
   - Part of on-call duties

2. VERSIONING:
   - Date last updated
   - Git history
   - Easy rollback if needed

3. VALIDATION:
   - Quarterly test
   - After system change
   - Before using in incident

4. FEEDBACK:
   - After each incident: Update runbook
   - Team suggestions: Implement
   - New problems: New runbook

Template header:
```
# Runbook: High Memory Usage
**Owner**: Backend Team
**Last Updated**: 2026-01-15
**Tested**: 2025-12-15
**Next Review**: 2026-03-15
```
```

---

## Key Principles

1. **Actionable**: Specific commands, not advice
2. **Tested**: Works in your environment
3. **Fast**: 5-10 minutes to resolution
4. **Clear**: Anyone can follow it
5. **Complete**: All edge cases covered
6. **Maintained**: Updated when system changes
