# Exercises: SRE Operational Runbooks

10 exercises on creating and maintaining runbooks.

---

## Exercise 1: Identify Alert (Easy)

**Scenario**: Your monitoring has these alerts:

1. "CPU usage > 80%"
2. "Response time > 1 second"
3. "Error rate > 5%"
4. "Disk usage > 90%"

**Task**:
1. For each, what's the problem?
2. Quick check command?
3. Likely cause?

---

## Exercise 2: Decision Tree (Easy)

**Scenario**: High error rate alert.

Possible causes:
- Database down
- Slow queries
- Service restarted
- Deployment issue

**Task**:
1. What to check first?
2. How to distinguish causes?
3. Build decision tree

---

## Exercise 3: Expected Outputs (Easy)

**Scenario**: Runbook says "Check if service healthy"

Command: `systemctl status service`

**Task**:
1. What output means healthy?
2. What output means broken?
3. What if output unclear?

---

## Exercise 4: Rollback Plan (Medium)

**Scenario**: Runbook says "Restart service"

**Task**:
1. What if restart makes it worse?
2. How to rollback?
3. When to escalate?

---

## Exercise 5: Actionable Steps (Medium)

**Bad runbook**: "Check logs"
**Good runbook**: ?

**Task**:
1. Make "Check logs" actionable
2. Add: Command, what to look for
3. What next step depends on?

---

## Exercise 6: Testing Runbook (Medium)

**Scenario**: You created runbook for "High memory"

**Task**:
1. How to test before using in prod?
2. What could break?
3. When to practice?

---

## Exercise 7: Decision Under Pressure (Medium)

**Scenario**: Runbook has many branches.

Under incident pressure:
- Can't remember all steps
- Easy to skip steps
- Might make wrong decision

**Task**:
1. Simplify for clarity?
2. Add checklists?
3. Automate instead?

---

## Exercise 8: Runbook vs Automation (Medium)

**Scenario**: Runbook for "Restart service"

Could automate with:
- Health check → Auto restart
- Decision: Manual or auto?

**Task**:
1. What to automate?
2. What to keep manual?
3. How to decide?

---

## Exercise 9: Escalation Paths (Medium)

**Scenario**: Runbook can't fix problem.

Options:
- Call expert
- Escalate to team
- Page on-call
- Open incident

**Task**:
1. When to escalate?
2. To whom?
3. What info to pass?

---

## Exercise 10: Maintenance (Medium)

**Scenario**: Runbook created 1 year ago.

System changed: New service, different config
Runbook outdated: Commands don't work

**Task**:
1. How often to update?
2. How to know outdated?
3. Keep runbook current?

---

## Self-Reflection

- ✓ Can I identify problems?
- ✓ Can I create decision trees?
- ✓ Do I know what to automate?
- ✓ Can I test runbooks?
