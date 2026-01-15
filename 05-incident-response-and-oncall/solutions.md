# Solutions: Incident Response and On-Call

Solutions for all 10 exercises.

---

## Solution 1: Classify Incidents

**Answer**:

```
1. Payment processing down → SEV 1
   Why: Revenue impact, all users affected

2. One user's profile picture not loading → SEV 4
   Why: Single user, feature still works

3. API slow (5s vs 0.5s) → SEV 2
   Why: Partial impact on users, performance degraded

4. Login button missing 10% of time → SEV 1
   Why: 10% of users can't log in, critical feature

5. Email notifications delayed 1 hour → SEV 3
   Why: Feature works, just delayed, workaround: retry
```

---

## Solution 2: Incident Timeline

**Answer**:

```
Response time: 14:00 (deploy) → 14:07 (notice) = 7 minutes

Mitigation time: 14:07 (notice) → 14:15 (fixed) = 8 minutes

Better breakdown:
- 14:00-14:05: Silent (no alert)
- 14:05-14:07: Alert → Response = 2 min (alert latency)
- 14:07-14:10: Investigation = 3 min
- 14:10-14:12: Fix = 2 min
- 14:12-14:15: Verification = 3 min

Key improvements:
✓ Alert went off quickly (5 min)
✓ Engineer responded fast (2 min)
✓ Root cause found quickly (3 min)
✓ Rollback was simple (2 min)

Recommendation:
- Fast root cause = good observability
- Fast fix = simple rollback worked
```

---

## Solution 3: Escalation Policy

**Answer**:

```
Escalation order:
1. Primary on-call (must respond in 5 min, even if asleep)
   Why: They're scheduled for it
   
2. If no response in 5 min → Backup on-call
   Why: Someone must handle it

3. Once someone has it, page Tech Lead if:
   - SEV 1 + not fixed in 15 min
   - Needs decision beyond on-call scope
   Why: Expertise + authority

4. Page Manager if:
   - SEV 1 + unresolved after 30 min
   - Needs customer communication
   Why: Stakeholder updates

Incident Commander:
- Primary on-call until resolved
- Tech lead takes over if escalated
- Manager joins for communication only (doesn't direct fix)
```

---

## Solution 4: Root Cause vs Symptom

**Answer**:

```
SYMPTOMS:
❌ Error rate 15%
❌ Database connection refused
❌ Requests waiting 30 seconds

ROOT CAUSES (pick one):
✓ New code opens too many connections
✓ Database max connections = 100, but we opened 150
✓ Connection pool not releasing connections

QUICK MITIGATION:
1. Kill stuck connections
2. Reduce new deployments
3. Route traffic to backup DB

PERMANENT FIX:
- Cap connections at 80 (safety buffer)
- Add connection timeout (release after 5 min)
- Add monitoring for connection count
- Test high-load scenario
```

---

## Solution 5: Incident Commander

**Answer**:

```
Incident Commander role:
1. One person speaks (prevent chaos)
2. Ask smart questions:
   - What's the impact? (SEV level)
   - What changed? (deploy, config, traffic spike)
   - What's the mitigation? (quick first)
3. Delegate work (don't let everyone do everything)
4. Update stakeholders (status page, calls)

How to reduce chaos:
- "Everyone stop, I'm IC. Sarah, check metrics."
- "Bob, look at rollback options."
- "Tech lead, is this easy rollback?"
- Assign tasks instead of free-for-all

Key questions first:
Q1: What's failing? (API? DB? Cache?)
Q2: How many users? (100 vs 100K)
Q3: What changed? (Deploy? Traffic? Config?)

Order of operations:
1. Clarify impact (is it really SEV 1?)
2. Find quick fix (rollback, cache, reroute)
3. Apply quick fix
4. Investigate root cause (slower, more thorough)
5. Plan permanent fix (tomorrow)
```

---

## Solution 6: Postmortem Blame

**Answer**:

```
BLAME (wrong):
"Engineer deployed without testing. Engineer is careless."

BLAMELESS (right):
"We lack automated tests. Manual testing takes 2 hours.
The engineer was under time pressure and skipped manual test.
The buggy code deployed without detection."

System improvements:
1. Add automated tests (prevent bugs)
2. Add pre-deploy checks (catch bugs)
3. Add canary deploy (catch in production slowly)
4. Remove time pressure (let people test)

Why blameless is better:
- Fix system, not person
- Engineer doesn't hide mistakes
- Next engineer learns from system fix
- Team becomes safer to make mistakes
- Actually prevents next incident
```

---

## Solution 7: Mitigation vs Resolution

**Answer**:

```
MITIGATION (quick, temporary):
✓ Route traffic to backup DB (10 min)
✓ Add cache layer (5 min)
✓ Scale database (1 hour)

What to do: Choose the fastest one that works
Backup DB fastest? Use it.

RESOLUTION (slow, real fix):
✓ Optimize slow query (2 hours)
✓ Add indexes (1 hour)
✓ Redesign schema (1 day)

Timeline:
- 0-10 min: Apply mitigation (route to backup DB)
- 10-30 min: Error rate should drop
- During incident: Investigate root cause
- After incident: Fix query optimization
- Next day: Test and deploy permanent fix

Key point:
During incident: Get service working (mitigation)
After incident: Fix root cause (resolution)

Never: "Let's optimize the query" while users suffer
Always: Quick mitigation first, real fix later
```

---

## Solution 8: On-Call Burnout

**Answer**:

```
What causes burnout:
1. Too many pages (7+ per night)
2. Can't sleep between pages
3. High stress (critical services)
4. No time to recover

Prevention:
✓ Limit pages to 1-2 per night (average)
✓ Runbooks for common incidents (faster fix)
✓ Good alerting (fewer false alarms)
✓ Monitoring on weekends too (don't all go to engineer)
✓ Rotate frequently (1 week on, 2 weeks off)
✓ Recovery time required (no meeting after on-call)

During incident:
- Engineer doesn't need to be heroic
- Sleep deprivation reduces decision quality
- Mistakes happen when tired
- Better: Fresh engineer does better work
- Consider: Can we have 2 engineers on major incident?

Example schedule:
- Week 1: Alex on-call (no late meetings)
- Week 2-3: Alex off (recovery + normal work)
- Week 4: Alex on-call again
```

---

## Solution 9: Communication During Incident

**Answer**:

```
Status page (public):
- Update every 15 min: "Investigating payment issue"
- Don't guess: "Root cause unknown, ETA 1 hour"
- Be honest: "Fully down" or "20% impacted"

Executives (internal):
- Update every 30 min: "Rollback in progress"
- Give time estimate: "Expect 30 min to resolve"
- Escalate need: "Need DBA help"

Support team (internal):
- Instant updates: "What's status now?"
- Give talking points: "Tell customers rollback in progress"
- Give runbook: "Here's what to tell angry customers"

Update frequency:
- SEV 1: Every 15 min minimum
- SEV 2: Every 30 min
- SEV 3: When progress made

Don't update:
- Every 2 minutes (noise)
- With speculation ("probably database")
- Without new information
```

---

## Solution 10: Runbook Prevention

**Answer**:

```
Why repeats?
1. Root cause not fixed (only symptoms fixed)
2. Postmortem action items not done
3. No monitoring to detect early
4. No testing of prevention
5. Team didn't learn (new people restart the cycle)

What postmortem should do:
- Identify: What was the root cause?
- Assign: Who fixes it? When?
- Monitor: How do we detect next time?
- Test: Verify fix works
- Document: So others know

What's a runbook?
A playbook for common incidents

Example runbook:
"Database Query Slow"

Detection:
- Alert: DB query > 1 second

Response:
1. Check query plan (EXPLAIN)
2. Add index if missing
3. Verify error rate while fixing

Recovery:
1. Temporarily: Increase connection timeout
2. Permanently: Add index + monitor

Testing:
- Run runbook in staging quarterly
- Time yourself (know it takes 10 min)
- Update as improvements happen

Prevention:
- All queries must have index plan
- Load test before deploy
- Monitor slow query log daily
```

---

## Key Principles

1. **Severity**: Clear definitions prevent argument
2. **Speed**: Fast mitigation > perfect fix
3. **Communication**: Stakeholders need updates
4. **Blameless**: Fix systems, not people
5. **Learning**: Postmortems prevent repeats
