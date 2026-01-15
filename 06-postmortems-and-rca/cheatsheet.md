# Cheatsheet: Postmortems and RCA

Quick reference for root cause analysis and postmortems.

---

## 5 Whys Technique

```
Problem: Database slow
Q1: Why?       → No index on query
Q2: Why?       → Code doesn't specify index
Q3: Why?       → Developer unaware of performance impact
Q4: Why?       → No code review for performance
Q5: Why?       → No performance testing requirement

ROOT CAUSE: Missing performance testing in CI/CD
FIX: Add performance tests before merge
```

---

## Fishbone Diagram (Cause & Effect)

```
          PEOPLE          PROCESS
           /  \            /  \
        Lack  Tired   No    Low
       Train  Eng    Review Quality
         \    /        \    /
          \  /          \  /
           \/            \/
    ─────────────────────────────
           PROBLEM
    ─────────────────────────────
           /\            /\
          /  \          /  \
       Old   Slow   High  Bad
      Code   DB   Traffic Network
       /  \    /    /  \
      /    \  /    /    \
    CODE              INFRASTRUCTURE
```

---

## Postmortem Template

```
INCIDENT: [Name, Date]

TIMELINE:
- HH:MM: What happened
- HH:MM: What happened next
- HH:MM: Issue detected
- HH:MM: Mitigation started
- HH:MM: Resolved

ROOT CAUSE:
[Explain why it happened - result of 5 Whys]

IMPACT:
- Duration: X minutes
- Users affected: X
- Revenue impact: $X
- SLA violation: Yes/No

ACTION ITEMS:
1. [Preventive action] (Owner: Name, Due: Date)
2. [Detective action] (Owner: Name, Due: Date)
3. [Reactive action] (Owner: Name, Due: Date)

FOLLOW-UP:
- Check progress: Week 1, 2, 3
- Close by: [Date]
```

---

## Blameless Postmortem Checklist

```
☐ No blame language ("careless", "stupid", "forgot")
☐ Focus on system, not person
☐ Use "we" instead of "they"
☐ Ask: "How did system allow this?"
☐ Ask: "What was person trying to do?"
☐ Ask: "What information did they lack?"
☐ Include timeline (facts, not opinions)
☐ Root cause is about system design
☐ Action items improve system
```

---

## RCA Techniques

| Technique | When to Use | Steps |
|-----------|------------|-------|
| **5 Whys** | Simple problems | Ask "Why?" 5 times until root found |
| **Fishbone** | Complex problems | Map all possible causes on diagram |
| **Tree Diagram** | Multiple causes | Show cause relationships hierarchically |
| **Fault Tree** | Safety-critical | Work backward from failure |

---

## Action Item Types

| Type | Example | Owner | Timeline |
|------|---------|-------|----------|
| **Preventive** | Add automated tests | Dev team | 1 week |
| **Detective** | Add monitoring alert | SRE team | 3 days |
| **Reactive** | Create runbook | On-call | 2 days |
| **Corrective** | Optimize query | DB team | 2 weeks |

---

## Postmortem Metrics

```
TRACK:
- % action items completed (target: 100%)
- Avg time to complete (target: 2 weeks)
- Repeat incidents (target: 0)
- MTTR improvement (measure: time to fix decreases)

MEASURE SUCCESS:
- Same incident doesn't repeat
- System becomes more resilient
- Team learns from mistakes
```

---

## Common RCA Mistakes

```
❌ Stopping at symptoms
   ✓ Keep asking "Why?" until you find system issue

❌ Blaming individuals
   ✓ Ask "How did system allow this?"

❌ Vague action items
   ✓ Specific: "Add index on user_id" (not "improve DB")

❌ No timeline for actions
   ✓ Set deadline: "by Friday" (not "soon")

❌ Ignoring action items
   ✓ Track weekly, don't close postmortem until done

❌ Long postmortems (3+ hours)
   ✓ Timebox to 1 hour, action items only
```

---

## Postmortem Timeline

```
Incident happens → Schedule postmortem within 24h
Postmortem meeting → 1 hour, identify root cause + action items
Action items assigned → Owner + deadline set
Week 1 → 50% should be in progress
Week 2 → 80% should be done
Week 3 → 100% complete, postmortem closed
Month later → Verify fix prevents repeat
```

---

**Remember**: Postmortems are about learning. Good postmortems prevent the same incident from happening twice.
