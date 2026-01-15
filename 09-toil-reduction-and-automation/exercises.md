# Exercises: Toil Reduction and Automation

10 exercises on identifying and automating toil.

---

## Exercise 1: Identify Toil (Easy)

**Scenario**: List these tasks:

1. Debugging a complex database deadlock
2. Restarting service every Monday
3. Building new feature
4. Manually adding users to system
5. Investigating SLA violations

**Task**:
1. Which are toil?
2. Which are valuable work?
3. Why the difference?

---

## Exercise 2: Measure Toil in Hours (Easy)

**Scenario**: Your tasks this week:

- Responding to alerts: 10 hours
- Debugging: 8 hours
- Deploying: 6 hours
- Team meetings: 8 hours
- Manual provisioning: 8 hours

**Task**:
1. How much is toil? (percentage)
2. How much is creative? (percentage)
3. Is this healthy?

---

## Exercise 3: Calculate ROI (Easy)

**Scenario**: Manual task takes 1 hour per week.

Automation effort: 4 hours

**Task**:
1. How many weeks to break even?
2. What's the benefit after 1 year?
3. After 3 years?

---

## Exercise 4: Toil Tracking (Medium)

**Scenario**: You've decided to automate. First, measure.

Potential tasks to automate:
- User provisioning: 2 hours/month
- Log rotation: 1 hour/month
- Backup restore: 4 hours/month (emergencies)
- Monitoring dashboard: 1 hour/month
- Runbook updates: 2 hours/month

**Task**:
1. Rank by ROI (highest hours first)
2. Pick top 3 to automate
3. Estimate automation effort for each

---

## Exercise 5: Script Error Handling (Medium)

**Scenario**: You write a script that runs in cron.

Script:
```bash
#!/bin/bash
cp -r /var/data /backup/
```

**Task**:
1. What happens if copy fails?
2. How will you know?
3. Add error handling
4. What should happen if it fails?

---

## Exercise 6: Automation Maintenance (Medium)

**Scenario**: You automated provisioning 6 months ago.

Now:
- System API changed
- Script still uses old API
- Failures are silent
- New hires can't provision

**Task**:
1. Why did automation break?
2. How to prevent?
3. Who maintains scripts?
4. How often to test?

---

## Exercise 7: Partial Automation (Medium)

**Scenario**: Task is hard to fully automate:

Manual incident response:
- Detect problem (automated alert)
- Triage severity (human judgment)
- Page on-call (automated)
- Coordinate fix (human)
- Postmortem (human)

**Task**:
1. What can be automated?
2. What needs human judgment?
3. How to combine?

---

## Exercise 8: Automation vs Manual Decision (Medium)

**Scenario**: Two options:

Option A: Automate (5 hours effort)
Option B: Keep manual (1 hour monthly toil)

**Task**:
1. Calculate: When to use each?
2. If task runs 1 year → Use automation?
3. If task runs 2 years → Use automation?
4. Breakeven point?

---

## Exercise 9: Monitoring Automated Tasks (Medium)

**Scenario**: You automated certificate renewal.

Now what can go wrong?
- Script runs but fails (silent)
- Certificate expired anyway
- Backup fails (not noticed)
- Wrong certificate renewed

**Task**:
1. How to know if script succeeded?
2. What metrics to track?
3. How to alert on failure?

---

## Exercise 10: Building Automation Culture (Medium)

**Scenario**: Your team has >50% toil.

To reduce, you need:
- Time to automate
- Skills (scripting)
- Culture change
- Tooling

**Task**:
1. How to start?
2. Pick first 3 automations
3. How to convince skeptics?
4. How to measure success?

---

## Self-Reflection

- ✓ Can I identify toil?
- ✓ Can I measure ROI?
- ✓ Can I script automation?
- ✓ Do I understand toil tradeoffs?
