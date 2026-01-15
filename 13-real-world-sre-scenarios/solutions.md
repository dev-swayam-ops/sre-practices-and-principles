# Solutions: Real-World SRE Scenarios

Solutions for all 10 exercises.

---

## Solution 1: Classify Severity

**Answer**:

```
1. PAYMENT API RETURNING 500
   Severity: SEV-1 (critical)
   Why: Users can't complete transactions
   Impact: Revenue impact, immediate
   Acceptable downtime: Minutes only
   Escalation: Yes, immediately

2. NOTIFICATIONS NOT SENDING
   Severity: SEV-2 (high)
   Why: Users miss alerts, but can work
   Impact: Not critical, can catch up later
   Acceptable downtime: Hours
   Escalation: Yes, within 30 min

3. REPORT TAKING 2 HOURS
   Severity: SEV-3 (medium)
   Why: Feature works, just slow
   Impact: User frustration, not blocking
   Acceptable downtime: Can wait
   Escalation: No, schedule for investigation

4. DASHBOARD SLOW (5 SEC)
   Severity: SEV-3 (medium)
   Why: Still functional, just slower
   Impact: User experience impact only
   Acceptable downtime: Can monitor
   Escalation: No, monitor and investigate
```

---

## Solution 2: Triage Under Pressure

**Answer**:

```
5 alerts firing:
CPU high, Memory high, Disk high, Error rate high, Response time high

TRIAGE PRIORITY:

1. ERROR RATE (first):
   Why: Tells you if service broken or just slow
   Check: Are requests failing or just slow?
   
   If errors: SEV-1 (service broken)
   If slow: SEV-2 (degraded)

2. RESPONSE TIME (second):
   Why: Understand scope of degradation
   Check: Response time for each endpoint
   
   If all slow: Infrastructure issue (CPU/Memory)
   If some slow: Application issue (specific query)

3. CPU/MEMORY/DISK (third):
   Why: Understand resource constraints
   Check: Which resource is bottleneck?
   
   CPU high: Runaway process or high load
   Memory high: Memory leak or too much data
   Disk high: Log explosion or data leak

DECISION:
- If CPU/Memory/Disk maxed → Kill/restart/cleanup
- If error rate high → Check service logs
- If only response time high → Investigate queries

ESCALATION TIMING:
- 0-5 min: Gather info
- 5-10 min: Trying fix
- 10-15 min: If no progress → Page expert
- 15+ min: SEV-1, bring everyone
```

---

## Solution 3: Multi-Service Failure Analysis

**Answer**:

```
Symptoms:
- Database slow (queries > 5 sec)
- Cache full (memory exhausted)
- API timeout (waiting for response)

ARE THEY RELATED?

Likely chain:
Cache fills up
→ App can't get from cache
→ App hits database
→ Too many connections
→ Database slow
→ App times out
→ API returns error

ROOT CAUSE: Cache memory leak

INVESTIGATION:
1. Check cache memory growth
   redis-cli INFO memory
   
2. Check database connections
   SHOW PROCESSLIST
   
3. Check application logs
   tail -100 /var/log/app.log

FIX ORDER:
1. IMMEDIATE: Restart cache
   Frees memory, reduces DB load
   
2. SHORT-TERM: Scale database
   Handle remaining load
   
3. LONG-TERM: Fix memory leak
   Prevent recurrence

Why this order:
- Restart cache fixes 90% quickly
- Scale DB handles remaining load
- Fix memory leak prevents next time
```

---

## Solution 4: Escalation Decision

**Answer**:

```
Situation:
- Working 12 minutes
- Made progress: 10% → 5% error rate
- Good idea for next fix
- Still not resolved

DECISION: Escalate now (Option B)

Reasoning:
✓ 12 minutes is reasonable time to try
✓ Error rate improving (not stuck)
✓ Getting expert help = faster resolution
✓ Expert might spot what you missed

Don't wait because:
✗ 5 more minutes might not be enough
✗ You might be approaching wrong direction
✗ Expert eyes catch issues faster

ESCALATION TIMING:
SEV-1: 0-5 min → Get help, < 10 min escalate
SEV-2: 0-15 min → Get help, 15 min escalate
SEV-3: 0-30 min → Get help if stuck, escalate

GOOD ESCALATION:
"Hi, payment API still at 5% error rate.
Made progress from 10%.
I''ve tried [A, B, C].
Next idea is [D].
Can you help? Here''s what I found..."

BAD ESCALATION:
"It''s broken, help me"
```

---

## Solution 5: Parallel Investigation

**Answer**:

```
Only one person, many things to check.
This is normal in incidents.

ORGANIZE YOURSELF:

1. PRIORITIZE (2 min)
   What matters most?
   - Service health first
   - Then infrastructure
   - Then code/logs

2. BATCH CHECKS
   Group related commands:
   
   Batch A (service):
   - systemctl status service
   - curl http://service/health
   - tail -f /var/log/app.log
   
   Batch B (infrastructure):
   - ps aux --sort=-%cpu
   - free -h
   - df -h
   
   Batch C (database):
   - mysql -e "SHOW PROCESSLIST"
   - Check slow query log

3. RUN IN PARALLEL (mentally)
   Check A while B is running
   Check B while logs load
   Reduce idle time

4. WHEN TO CALL FOR HELP
   - 5 minutes in: Have theory
   - 10 minutes in: Have evidence
   - At 10 min: Call for help (if not resolved)
   
   Don't wait for perfect information
   Call when you have direction

EXAMPLE:
- 0-3 min: Quick checks (service, infra)
- 3-5 min: Make hypothesis
- 5-10 min: Gather evidence
- 10 min: Have direction? Call for help
- With help: Parallel investigation is faster
```

---

## Solution 6: Guess vs Verify

**Answer**:

```
3 guesses:
A) Recent deployment
B) Database slow
C) Cache expired

TIME: 2 minutes only

VERIFY (don't guess):

Quick verification (60 sec total):
1. Recent deployment? (10 sec)
   git log --oneline -1
   git show --name-only
   
2. Timing correlation? (10 sec)
   grep "3:00" /var/log/app.log | wc -l
   Did it start right after deploy?
   
3. Database slow? (20 sec)
   mysql -e "SHOW PROCESSLIST" | grep Query
   Count: If > 50 queries: Yes, else: No
   
4. Cache? (20 sec)
   redis-cli INFO stats | grep hits/misses
   If misses high: Cache problem

RESULT (in 60 sec):
You know which is most likely
Not guessing, but informed

PROBABILITY:
- Deployment correlation strongest (timing matches)
- Database load increasing (cascading from cache miss)
- Cache might be symptom, not cause

NEXT STEP:
"Deploy seems correlated with start time.
I''ll rollback as safest fast fix."

WHY VERIFY:
- Wrong guess = wrong fix = time wasted
- Verification takes < 1 minute
- Confidence in fix increases
```

---

## Solution 7: Fix Selection

**Answer**:

```
Database slow, 3 options:

OPTION 1: Kill slow queries (1 min)
- Pros: Very fast, immediate relief
- Cons: Temporary, might break things
- Risk: Low (queries just delayed)
- When: Try FIRST

OPTION 2: Restart database (5 min)
- Pros: Often fixes issues
- Cons: Brief downtime (5 min)
- Risk: Medium (might lose connections)
- When: If option 1 doesn't work

OPTION 3: Scale database (20 min)
- Pros: Permanent fix
- Cons: Takes longer, infrastructure change
- Risk: Medium (new setup might have issues)
- When: If temporary fixes didn't work

BEST APPROACH: Escalating fixes

At 0 min: Option 1
At 1 min: Check if worked
At 2 min: If not, start option 2
At 5 min: Check if worked
At 7 min: If not, start option 3 (in parallel with option 2)

REASONING:
- Fast wins first (option 1)
- Fallback ready (option 2)
- Permanent solution planned (option 3)
- Don't jump to 20-minute fix immediately
```

---

## Solution 8: Communication During Incident

**Answer**:

```
CEO: "What''s happening?"

GOOD ANSWER:
"We''re investigating a payment API issue that started
at 3:00 PM. Error rate is 10%. We''re working on fix.
ETA: next update in 5 minutes."

Key elements:
✓ What: Payment API issue
✓ When: Started 3:00 PM
✓ Scope: 10% error rate
✓ Action: We''re investigating
✓ Next: ETA for update

BAD ANSWERS:
✗ "It''s broken, we don''t know why"
✗ "Maybe it''s the database? Or cache?"
✗ "Could take hours to fix"
✗ "We''re fine, no big deal"

COMMUNICATION TIMING:
At 5 min: "Found issue, working on fix"
At 10 min: "Applied fix, testing"
At 15 min: "Still investigating, calling expert"
At 20 min: "SEV-1 declared, have team on call"

FREQUENCY:
- SEV-1: Update every 5 minutes
- SEV-2: Update every 15 minutes
- SEV-3: Update when available

TONE:
- Confident (we''re solving it)
- Realistic (could take X minutes)
- Action-oriented (we''re doing Y)
```

---

## Solution 9: Post-Incident Improvements

**Answer**:

```
Root cause: Unknown config value caused deploy to fail

IMPROVEMENTS:

1. PREVENT RECURRENCE
   - Config validation in deploy process
   - Config documentation (all values listed)
   - Config schema checks (types, values)
   - Staging deployment test first
   
2. RUNBOOK UPDATE
   - Add: "Check config values"
   - Add: "Verify config matches schema"
   - Document which configs are critical
   
3. MONITORING
   - Alert: Deploy fails
   - Alert: Config validation fails
   - Metric: Deploy success rate
   
4. PROCESS
   - Code review: Check config changes
   - Pre-deployment: Validate config
   - Staging: Test with real config
   
5. DOCUMENTATION
   - Config guide (what each does)
   - Deploy process (config validation step)
   - Common config mistakes

POST-MORTEM (next day):
"What went well?"
"What could improve?"
"Action items for team"

TIMELINE for improvements:
Week 1: Config validation
Week 2: Documentation
Week 3: Monitoring alerts
Month: Process improvement
```

---

## Solution 10: Incident Drill Planning

**Answer**:

```
DRILL PLANNING:

SCENARIO SELECTION:
Pick: Multi-service failure (tests investigation skills)
Example: Database slow + Cache failure + API timeout

REALISM:
✓ Use real systems (staging)
✓ Real tooling (same as prod)
✓ Time pressure (real scenario timing)
✗ Don''t make it too hard (learning goal)
✗ Don''t fake outputs (confusing)

DRILL EXECUTION:

1. SETUP (10 min)
   - Inject failure into staging
   - Notify team (we''re starting drill)
   - Start clock

2. RUN (30 min)
   - Team investigates
   - Leaders guide (don''t solve)
   - Observer takes notes

3. RESOLUTION (10 min)
   - Team finds root cause
   - Applies fix
   - Validates recovery

4. DEBRIEF (20 min)
   - What went well?
   - What was confusing?
   - What would improve?
   - Improvements to process/runbook

MEASUREMENT:
✓ Time to detect problem (should be < 2 min)
✓ Time to diagnosis (should be < 5 min)
✓ Time to fix (should be < 10 min)
✓ Team confidence (should increase)

FREQUENCY:
- Monthly: Small drill (one scenario)
- Quarterly: Big drill (complex scenario)
- Annually: Company-wide drill
```

---

## Key Principles

1. **Triage first**: Understand scope before fixing
2. **Verify before assuming**: Checks are fast, wrong fixes are slow
3. **Parallel work**: Don't work alone, divide tasks
4. **Escalate early**: 10 minutes without progress = call help
5. **Communicate constantly**: Stakeholders and team
6. **Learn after**: Post-mortem improves next incident
