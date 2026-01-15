# Cheatsheet: Real-World SRE Scenarios

Quick reference for handling incidents.

---

## Severity Classification

| Level | Criteria | Response Time | Escalation |
|-------|----------|---------------|-----------|
| **SEV-1** | Service down, revenue loss | Immediate | All hands |
| **SEV-2** | Partial degradation, workaround exists | < 15 min | Team |
| **SEV-3** | Minor issue, no impact | < 1 hour | On-call |

---

## Triage Checklist

```
First 5 minutes:

☐ What's broken?
☐ How many users affected?
☐ Business impact?
☐ When did it start?
☐ Recent changes?

Outcome: Severity level set
```

---

## Investigation Priority

```
1. Service health (is it broken?)
2. Error patterns (what's failing?)
3. Resource usage (what's stressed?)
4. Recent changes (what changed?)
5. Dependencies (what's slow?)
6. Logs (what happened?)
```

---

## Common Root Causes

| Symptom | Check First | Likely Causes |
|---------|------------|----------------|
| High CPU | ps aux | Runaway process, high load |
| High memory | free -h | Memory leak, large cache |
| High errors | tail logs | Bad deploy, code crash |
| Slow response | top | Resource exhaustion |
| Connection timeout | netstat | Network issue, service down |

---

## Escalation Decision Tree

```
Working on problem
│
├─ Have hypothesis?
│  ├─ No → Gather data (5 min)
│  └─ Yes → Try fix
│
├─ Fix is working?
│  ├─ Yes → Monitor, declare resolved
│  └─ No → Continue to next
│
├─ Time spent > 10 min?
│  ├─ Yes → ESCALATE
│  └─ No → Try another approach
│
└─ Multiple approaches tried?
   ├─ Yes → ESCALATE
   └─ No → Continue
```

---

## Communication Template

```
UPDATE TEMPLATE:

"At [TIME], [SERVICE] [ISSUE].
Impact: [X] users, [BUSINESS IMPACT].
Status: Investigating [AREA].
ETA: Next update in [TIME]."

Example:
"At 3:00 PM, payment API started returning
500 errors. 10% of transactions failing.
Status: Investigating database slowness.
ETA: Next update in 5 minutes."
```

---

## Incident Command Structure

```
INCIDENT LEADER:
- Makes decisions
- Coordinates response
- Communicates status

LEAD INVESTIGATOR:
- Gathers evidence
- Runs diagnostics
- Suggests fixes

COMMUNICATIONS:
- Updates stakeholders
- Tracks timeline
- Documents decisions

EXECUTOR:
- Applies fixes
- Monitors changes
- Validates resolution
```

---

## Parallel Work Under SEV-1

```
At incident start, divide:

Person A: Check application logs
Person B: Check infrastructure
Person C: Check recent changes
Person D: Communicate updates

Reconvene: Every 3 minutes to share findings

Example findings:
A: "Errors starting 3:00 PM, SQL timeout"
B: "Database CPU 100%, 500 connections"
C: "Deploy at 2:45 PM, schema change"
D: Root cause identified in 6 minutes
```

---

## Post-Mortem Checklist

```
WITHIN 24 HOURS:

☐ Timeline: When each thing happened
☐ Root cause: Why it happened
☐ Impact: How many users affected
☐ Response: What we did well
☐ Gaps: What could improve
☐ Actions: Who does what to prevent

OUTCOME:
- Process improvements identified
- Monitoring/alerts improved
- Team learns from incident
```

---

## Incident Metrics

| Metric | Good | Warning | Critical |
|--------|------|---------|----------|
| MTTD (detection) | < 2 min | 2-5 min | > 5 min |
| MTTR (resolution) | < 15 min | 15-60 min | > 60 min |
| Recurrence | Never | Quarterly | Monthly |
| Team confidence | High | Medium | Low |

---

## Red Flags (Need Help Now)

```
❌ No hypothesis after 5 minutes
❌ Tried 2+ approaches, still broken
❌ Error rate getting worse
❌ Multiple services failing
❌ Data integrity at risk
❌ Cascading failures happening
❌ Can't reach key person

→ ESCALATE IMMEDIATELY
```

---

**Remember**: In incidents, staying calm and getting help early matter most.
