# Cheatsheet: Incident Response and On-Call

Quick reference for handling incidents.

---

## Incident Severity Levels

| SEV | Impact | Examples | Response |
|-----|--------|----------|----------|
| **1** | Critical, customer-facing outage | Payment down, site 500 errors | Page on-call, incident commander |
| **2** | Significant degradation | API 10x slower, 50% users affected | Page on-call, business communication |
| **3** | Partial impact | One feature broken, workaround exists | Page on-call during business hours |
| **4** | Minor issue | Non-critical bug, can wait | File ticket, fix in next release |

---

## Incident Response Timeline

```
DETECT → DECLARE → TRIAGE → MITIGATE → RESOLVE → POSTMORTEM
  Alert   Severity    Impact   Workaround    Fix      Learn
  (5min)  (2min)      (3min)   (10min)      (hours)  (24-48h)

Goal: Triage + Mitigate in <15 minutes
```

---

## Quick Triage Checklist

- [ ] Is it customer-facing? (YES=SEV1, NO=lower)
- [ ] What percentage of users affected?
- [ ] Can users work around it?
- [ ] Is revenue impacted?
- [ ] Did we just deploy?
- [ ] Check metrics, logs, traces (observability)

---

## Incident Commander

```
Role: Coordinate response, prevent chaos

Responsibilities:
1. Declare severity
2. Assign people (Sarah: logs, Bob: metrics)
3. Escalate if needed (page tech lead after 15 min)
4. Update stakeholders (every 15-30 min)
5. Prevent scope creep ("Focus on mitigation, not debugging")

Authority:
- Can override normal processes
- Can page anyone
- Can make quick decisions
- Does NOT need to fix it themselves
```

---

## Mitigation vs Resolution

| Mitigation | Resolution |
|-----------|-----------|
| Quick (minutes-hours) | Slow (hours-days) |
| Temporary workaround | Permanent fix |
| Example: Rollback deploy | Rewrite buggy code |
| Example: Route to backup DB | Optimize slow query |
| **Do first** | Do after incident |

---

## Escalation Policy Template

```
SEV 1:
- Minute 0: Page primary on-call
- Minute 5: No response? Page backup
- Minute 15: Not fixed? Page tech lead
- Minute 30: Not fixed? Page manager

SEV 2:
- Minute 0: Page primary
- Minute 30: If not resolved, page tech lead

SEV 3:
- During business hours: Page on-call
- After hours: Can wait until morning
```

---

## Postmortem Template

```
INCIDENT: [Name]

TIMELINE:
- 14:00: Deploy happened
- 14:05: Alert fired
- 14:07: Engineer noticed
- 14:15: Fixed

ROOT CAUSE:
- Code change introduced bug in auth
- Bug not caught because no test for this scenario

IMPACT:
- Duration: 15 minutes
- Users: 50,000
- Customers affected: 12

ACTION ITEMS:
1. Add test for this scenario (John, by Friday)
2. Add canary deploy (Sarah, next week)
3. Improve rollback speed (Bob, next week)

WHAT WENT WELL:
✓ Fast response time
✓ Clear communication

WHAT TO IMPROVE:
- Test coverage (should catch this)
- Canary deploy would have caught bug early
```

---

## Blameless Postmortem Rules

```
BLAME LANGUAGE:
❌ "Engineer was careless"
❌ "Why did Bob do this?"
❌ "This was a stupid mistake"

BLAMELESS LANGUAGE:
✓ "The system allowed this to happen"
✓ "We lacked automated tests"
✓ "How can we make deploying safer?"

Focus on: Systems, not people
Outcome: Prevent repeats, not punish
Culture: Safe to fail, safe to report
```

---

## On-Call Best Practices

```
Rotation:
- 1 week primary, 1 week backup, 1 week off
- Stagger coverage (different timezones)

Load:
- Target: 1-2 pages per night average
- Max: 3 pages per night without burnout

Recovery:
- Day off after on-call shift ends
- No new meetings for 2 days after
- Can't do on-call 2 weeks in a row

Runbooks:
- Create for every recurring incident
- Update after every postmortem
- Test runbooks quarterly
```

---

## Runbook Structure

```
INCIDENT: Database Query Slow

DETECTION:
- Alert: Query time > 1 second
- Symptoms: Users report slowness, error rate up

IMMEDIATE ACTIONS (0-5 min):
1. Check query plan: EXPLAIN SELECT...
2. Check table size: SELECT COUNT(*)...
3. Check indexes: SHOW INDEXES

MITIGATION (5-15 min):
1. Increase statement timeout
2. Route traffic to read replica
3. Kill long-running queries

RESOLUTION (hours):
1. Optimize query (add index, rewrite)
2. Test in staging
3. Deploy and monitor

MONITORING:
- Alert if query > 1 second
- Track query time daily
- Review slow query log weekly
```

---

## What NOT to Do

```
❌ Blame anyone by name ("Bob's fault")
❌ Skip postmortem ("Too busy")
❌ Ignore action items ("We'll do better next time")
❌ Page too often (causes on-call fatigue)
❌ Assume mental notes count (document!)
❌ Over-scale before understanding issue
❌ Communicate vaguely ("We're working on it")
```

---

## What TO DO

```
✓ Respond fast (acknowledge in < 5 min)
✓ Mitigate quick (fix symptoms first)
✓ Communicate often (every 15-30 min)
✓ Investigate thoroughly (use observability)
✓ Document everything (for postmortem)
✓ Hold blameless postmortem
✓ Execute action items (prevent repeats)
```

---

**Remember**: Good on-call is: Fast response, clear communication, blameless learning, and team sustainability.
