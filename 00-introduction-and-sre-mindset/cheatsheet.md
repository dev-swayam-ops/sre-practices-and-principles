# Cheatsheet: SRE Mindset & Principles

Quick reference guide for core SRE concepts and principles.

---

## SRE Core Pillars

### 1. Automation
- **Goal**: Eliminate repetitive manual work
- **Principle**: If you do it more than once, automate it
- **Benefit**: Consistency, speed, scalability
- **Question to Ask**: "How much time would we save if we automated this?"

### 2. Reliability
- **Goal**: Build systems users can trust
- **Principle**: Balance features with stability
- **Benefit**: User trust, reduced on-call burnout, competitive advantage
- **Question to Ask**: "Is this system reliable enough for our users?"

### 3. Measurement
- **Goal**: Data-driven decisions
- **Principle**: You can't improve what you don't measure
- **Benefit**: Know what's working, identify bottlenecks
- **Question to Ask**: "What metrics prove we improved?"

---

## Key SRE Concepts

| Concept | Definition | Example | Why It Matters |
|---------|-----------|---------|---|
| **Toil** | Repetitive, manual work with no lasting value | Manual server restarts every day | Wastes time, scales poorly, error-prone |
| **Automation** | Using code/tools to replace manual processes | Self-healing systems, monitoring alerts | Frees engineers for complex work |
| **Error Budget** | Allowed amount of failures before hitting SLO | 99.9% SLO = 8.76 hrs downtime/year | Balances velocity with stability |
| **MTBF** | Mean Time Between Failures | System fails every 30 days on average | Shows system reliability |
| **MTTR** | Mean Time To Recovery | Takes 15 min to fix an outage | Shows team response speed |
| **On-Call** | Engineer on duty to respond to incidents | 1 week rotation per engineer | Someone always ready to respond |
| **Incident** | Unplanned interruption in service | Website down for 2 hours | Costs money, trust, reputation |
| **Post-Mortem** | Analysis after incident to prevent recurrence | "Why did the database crash?" | Continuous learning from failures |

---

## SRE Mindset Checklist

Use this to assess your team's SRE readiness:

- [ ] **We measure system reliability** (not just availability)
- [ ] **We automate repetitive tasks** (not manual processes)
- [ ] **We conduct blameless postmortems** (focus on systems, not people)
- [ ] **We have error budgets** (explicit reliability targets)
- [ ] **We track MTTR** (measure response time)
- [ ] **We reduce toil** (constantly automate manual work)
- [ ] **We use monitoring/alerting** (know about problems before users)
- [ ] **We test deployments** (prevent bugs in production)
- [ ] **On-call engineers are supported** (rotations, training, tools)
- [ ] **We learn from incidents** (postmortems drive improvements)

**Score**: 
- 7-10 ✓ Strong SRE culture
- 4-6 ✓ Getting there
- <4 ✓ Lots of room to grow

---

## Common SRE Metrics

### Availability Metrics

```
Availability = (Uptime / Total Time) × 100%

99.9% = 8.76 hours downtime/year = "Three Nines"
99.99% = 52 minutes downtime/year = "Four Nines"
99.999% = 5.26 minutes downtime/year = "Five Nines"
```

### Performance Metrics

| Metric | Definition | Good Target |
|--------|-----------|-------------|
| **Latency** | Time to respond to request | <100ms for web, <500ms for API |
| **Throughput** | Requests handled per second | Varies by app |
| **Error Rate** | % of requests that fail | <0.1% (99.9% success) |
| **MTBF** | Time between failures | Measured in days/weeks |
| **MTTR** | Time to recover from failure | <15 minutes ideal |

---

## The SRE Hierarchy of Needs

```
             /\
            /  \
           / Do  \           <- Implement high-end reliability (99.99%+)
          /______\
         /\      /\
        /  \    /  \
       / Do \  / Do  \       <- Automate complex workflows
      /______\/_____\
     /\            /\
    /  \          /  \
   / Do \   Do   / Do  \     <- Automate simple tasks, monitoring
  /______\____/______\
 /\                  /\
/  \                /  \
/ Do \    Do   Do  / Do  \   <- Runbooks, dashboards, alerting
/____\__/__\______/____\
/\                        /\
/  \                      /  \
/ Do \      Do        Do / Do  \ <- Measurement, logging, tracing
/____\____/____\________/____\

BASE: UNDERSTANDING + COMMUNICATION

(Each level builds on the one below)
```

Key insight: Start at the base. You can't automate what you don't understand.

---

## Quick Decision Tree: Should We Automate This?

```
Does this task happen more than once a month?
├─ NO → Don't automate (not worth it)
└─ YES → Continue

Is it repetitive and manual?
├─ NO → It's complex, keep it manual
└─ YES → Continue

Does it take more than 30 minutes/month?
├─ NO → Not urgent, lower priority
└─ YES → Continue

Can we predict when it needs to happen?
├─ NO → Manual for now, revisit later
└─ YES → AUTOMATE THIS

Expected ROI = (Time saved per month) × 12 ÷ (Development hours needed)
If ROI > 5x, high priority
If ROI < 2x, low priority
```

---

## Anti-Patterns (What NOT to Do)

| ❌ Anti-Pattern | ✓ SRE Way |
|-----------------|-----------|
| Manual deployments | Automated CI/CD pipeline |
| No monitoring | Comprehensive metrics + alerting |
| Blame culture | Blameless postmortems |
| Reactive firefighting | Proactive monitoring + automation |
| No clear SLO | Defined SLO + error budget |
| High on-call burden | Reduced toil = less on-call |
| No runbooks | Clear incident response procedures |
| Learn nothing from failures | Systematic postmortems |
| "Hope it doesn't break" | Redundancy + graceful degradation |
| Manual at scale | Automation that scales |

---

## Quick Reference: SRE vs Traditional Ops

| Aspect | Traditional Ops | SRE |
|--------|-----------------|-----|
| Problem Solving | Manual troubleshooting | Automation + code |
| Failure Response | "Fix it!" | Understand root cause |
| Improvement | Ad-hoc | Systematic (postmortems) |
| Tools | Manual scripts | Orchestration systems |
| Reliability | Hope for best | Measure + optimize |
| Learning | Repeat same mistakes | Prevent through action items |
| On-Call | Very demanding | Managed via automation |

---

## Useful Formulas

### Error Budget Calculation
```
Error Budget = (1 - SLO) × Total Time
Example: 99.9% SLO = (1 - 0.999) × 525,600 min/year = 525.6 minutes
```

### MTBF / MTTR
```
System Reliability = MTBF / (MTBF + MTTR)
Example: If system fails every 30 days and takes 1 hour to fix:
Reliability = 720 / (720 + 1) = 99.86%
```

### Toil ROI
```
ROI = (Hours saved per year × Hourly cost) / Development hours
Example: Automation saves 100 hours/year at $50/hour, took 40 hours to build:
ROI = (100 × 50) / 40 = 125% (payback in 5 months)
```

---

## SRE Principles Summary

1. **Automate**: Eliminate manual work
2. **Measure**: Track everything important
3. **Learn**: Blameless postmortems drive improvement
4. **Balance**: Velocity + stability = sustainable growth
5. **Scale**: Systems should scale without proportional effort
6. **Simplify**: Complex operations are brittle

---

## When Things Go Wrong: Incident Response

**During Incident**:
1. Declare incident
2. Gather team
3. Establish communication channel
4. Assign incident commander
5. Collect logs/metrics
6. Execute known procedures (runbooks)
7. Communicate status to stakeholders
8. Fix the issue

**After Incident**:
1. All stakeholders attend postmortem
2. Timeline of events documented
3. Root causes identified (systems, not people)
4. Action items assigned
5. Lessons learned shared
6. Prevent recurrence

---

## Further Reading

- **"SRE Book"** by Google: Free online, foundational principles
- **"SRE Workbook"** by Google: Practical exercises
- **Concept**: "Error Budgets" - define acceptable failure rates
- **Concept**: "Blameless Postmortems" - learn without blame

---

**Remember**: SRE is a mindset, not a role. Apply these principles everywhere and watch your systems (and team) improve.
