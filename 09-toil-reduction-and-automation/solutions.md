# Solutions: Toil Reduction and Automation

Solutions for all 10 exercises.

---

## Solution 1: Identify Toil

**Answer**:

```
TOIL (repetitive, manual, unchallenging):
✓ Restarting service every Monday (same action)
✓ Manually adding users (formula-driven)

VALUABLE (creative, learning, important):
✓ Debugging complex deadlock (problem-solving)
✓ Building feature (creativity)
✓ Investigating SLA violations (learning)

Why the difference?
Toil: Could run same way next week/month/year
Valuable: Requires thinking, unique each time

Key distinction:
- If you could script it → It's toil
- If it requires judgment → It's valuable
```

---

## Solution 2: Measure Toil

**Answer**:

```
Total work: 40 hours

TOIL:
- Responding to alerts: 10 hours (reactive, routine)
- Deploying: 6 hours (if manual, repetitive)
- Manual provisioning: 8 hours (formula-driven)
Total toil: 24 hours

CREATIVE:
- Debugging: 8 hours
- Team meetings: 8 hours (discussing, planning)
Total creative: 16 hours

Toil percentage: 24/40 = 60%
Creative percentage: 16/40 = 40%

IS THIS HEALTHY? NO
- Goal: < 30% toil, > 70% creative
- Current: 60% toil (double the goal!)
- Action: Need to automate heavily

Quick wins:
- Automate deploying (6 hours → 1 hour)
- Automate provisioning (8 hours → 1 hour)
- Smart alerting (reduce responding)
Result: 60% → 30% toil
```

---

## Solution 3: Calculate ROI

**Answer**:

```
Manual task: 1 hour/week
Automation effort: 4 hours

Week 1: -4 hours (building)
Week 2: +1 hour saved, -3 net
Week 3: +1 hour saved, -2 net
Week 4: +1 hour saved, -1 net
Week 5: +1 hour saved, BREAK EVEN!

After 1 year:
- Saved: 52 weeks × 1 hour = 52 hours
- Cost: 4 hours
- Net benefit: 52 - 4 = 48 hours

After 3 years:
- Saved: 156 hours
- Cost: 4 hours
- Net benefit: 152 hours

Summary:
- Breakeven: 4 weeks
- 1-year benefit: 48 hours
- 3-year benefit: 152 hours

Recommendation: Automate (payback is quick)
```

---

## Solution 4: Toil Ranking by ROI

**Answer**:

```
Annual hours (estimate):
- User provisioning: 2 × 12 = 24 hours/year
- Log rotation: 1 × 12 = 12 hours/year
- Backup restore: 4 × 12 = 48 hours/year (emergencies)
- Monitoring dashboard: 1 × 12 = 12 hours/year
- Runbook updates: 2 × 12 = 24 hours/year

RANKING (by hours):
1. Backup restore: 48 hours/year
2. User provisioning: 24 hours/year (tied)
2. Runbook updates: 24 hours/year (tied)
4. Log rotation: 12 hours/year
4. Monitoring dashboard: 12 hours/year

TOP 3 TO AUTOMATE:
1. Backup restore (48 hours saved)
   - Effort: 16 hours (complex)
   - ROI: Payback in 4 months
   
2. User provisioning (24 hours saved)
   - Effort: 6 hours (moderate)
   - ROI: Payback in 9 weeks
   
3. Runbook updates (24 hours saved)
   - Effort: 4 hours (easy)
   - ROI: Payback in 1 week

Sequence: Start with #3 (easiest), then #2, then #1
```

---

## Solution 5: Script Error Handling

**Answer**:

```
ORIGINAL (no error handling):
#!/bin/bash
cp -r /var/data /backup/

Problems:
- If cp fails: Script continues silently
- If disk full: No error message
- If permissions wrong: Fails without notice
- Cron runs at 2am, no one notices

IMPROVED (with error handling):
#!/bin/bash
set -e  # Exit on any error

SOURCE="/var/data"
BACKUP="/backup/"

# Check source exists
if [ ! -d "$SOURCE" ]; then
  echo "ERROR: Source $SOURCE not found"
  exit 1
fi

# Perform copy
cp -r "$SOURCE" "$BACKUP" || {
  echo "ERROR: Backup failed"
  # Alert on-call or send email
  exit 1
}

# Verify backup
if [ ! -d "$BACKUP" ]; then
  echo "ERROR: Backup not found"
  exit 1
fi

echo "✓ Backup completed successfully"
exit 0

BETTER:
- Check disk space before copying
- Log to syslog
- Alert if any error
- Send success email weekly
```

---

## Solution 6: Automation Maintenance

**Answer**:

```
Why it broke:
- Script written once, never updated
- System API changed
- Script still uses old API
- Failures are silent (no logging/alerts)
- No owner = no one fixes it

Prevention:

1. OWNERSHIP
   - Clear owner assigned
   - Backup owner (rotation)
   - Handoff procedure

2. MONITORING
   - Alert on script failure
   - Weekly success email
   - Monitor if task actually completed

3. TESTING
   - Test script monthly (cron job)
   - Test on staging before production
   - Manual spot-check quarterly

4. DOCUMENTATION
   - Script purpose documented
   - Dependencies listed
   - How to manually run
   - How to troubleshoot

5. VERSIONING
   - Script in git (version control)
   - Change log
   - Easy to revert

Who maintains?
- Original author initially
- Team rotation (can't be one person)
- On-call team is backup

Test frequency:
- Auto: Weekly (automated test)
- Manual: Monthly (spot check)
- System API changes: Update and test
```

---

## Solution 7: Partial Automation

**Answer**:

```
Full incident response cycle:
1. Problem occurs
2. Alert fires (AUTOMATED)
3. Triage severity (HUMAN - judgment)
4. Page on-call (AUTOMATED)
5. Coordinate fix (HUMAN - judgment + action)
6. Restore service (MIXED - runbook + human)
7. Postmortem (HUMAN - learning)

What to automate:
✓ Alerting (detect problem fast)
✓ Paging (notify team fast)
✓ Runbook execution (steps are formula)
✓ Health checks (verify fix worked)

What needs human:
✓ Triage (requires context)
✓ Judgment calls (which fix? which team?)
✓ Postmortem (learning, discussion)

Example: Semi-automated incident response
1. Alert fires (auto)
2. Run diagnostic script (auto)
3. If obvious problem: Auto-fix (if safe)
4. If unclear: Page on-call with diagnostics
5. On-call decides: Fix or escalate

Benefit:
- Auto-fix simple problems (no paging)
- Diagnostic info ready for on-call
- Speeds resolution overall
```

---

## Solution 8: Automation vs Manual

**Answer**:

```
Automation cost: 5 hours
Monthly manual toil: 1 hour
Monthly cost: 1 hour

Decision:
Months × 1 hour = 5 hour breakeven

Month 1: Cost 5 hours (for automation)
Month 2-5: Save 1 hour/month
Month 6: BREAK EVEN (5 hours saved)
Month 7+: Pure savings

DECISION MATRIX:
- < 5 months of use: Probably don't automate
- 5-12 months of use: Automate (payback this year)
- > 12 months of use: Definitely automate

If task runs 1 year (12 months):
- Saved: 12 hours
- Cost: 5 hours
- Net: 7 hours benefit
- DECISION: Automate

If task runs 2 years (24 months):
- Saved: 24 hours
- Cost: 5 hours
- Net: 19 hours benefit
- DECISION: Definitely automate

If task runs 6 months:
- Saved: 6 hours
- Cost: 5 hours
- Net: 1 hour benefit
- DECISION: Borderline, but still positive (automate)

Breakeven: 5 months of task

Rule of thumb:
If task will run > 5 months: Automate
If task will run < 5 months: Keep manual
```

---

## Solution 9: Monitor Automated Tasks

**Answer**:

```
How to know if script succeeded:

1. EXIT CODE
   - 0 = success
   - non-zero = failure
   
2. LOGGING
   #!/bin/bash
   exec > /var/log/certrenew.log 2>&1
   echo "Starting certificate renewal..."
   
3. ALERTING
   - If exit code != 0: Send alert
   - If not run for 7 days: Send alert
   - If certificate expiring soon: Send alert

Metrics to track:
   - Last run time (should be recent)
   - Success/failure count
   - Execution time (slow = problem?)
   - Certificate expiration date

Alert conditions:
   - Script failed (exit code != 0)
   - Script didn't run (missed schedule)
   - Certificate expiring < 30 days
   - Certificate already expired

Example:
   0 2 * * * /opt/renew-cert.sh || \
     echo "Cert renewal failed" | mail -s "ALERT" ops@example.com

Better:
   - Use monitoring tool (Prometheus, CloudWatch)
   - Record metrics
   - Alert if metric bad
```

---

## Solution 10: Building Automation Culture

**Answer**:

```
Current state: 50% toil (too high)
Goal: 30% toil (healthy)

STEP 1: Leadership Buy-in
- Show data: "50% of time is toil"
- Show ROI: "Automate provisioning: 4-week payback"
- Allocate time: "20% of sprint for automation"
- Explain benefit: "Engineers do creative work"

STEP 2: Pick First 3 Automations (low-hanging fruit)
1. User provisioning (high ROI, moderate effort)
2. Log rotation (easy, quick win)
3. Config backup (medium, reduces risk)

Success = Quick wins = Momentum

STEP 3: Skill Building
- Training: Bash scripting workshop
- Pairing: Experienced + junior
- Documentation: Make it easy to contribute

STEP 4: Convince Skeptics
Show them:
- Before: "Provisioning takes 30 min, error-prone"
- After: "Provisioning in 5 min, zero errors"
- Time saved: "43 hours/year = 1 week of freedom"

STEP 5: Measure & Celebrate
- Track toil % monthly
- Celebrate automation wins
- Report time saved
- Reinvest in next automations

Culture shift takes time:
- Month 1: Build first automations
- Month 2-3: See benefit
- Month 6: Team believes in automation
- Month 12: 30% toil achieved

Key: Show value early, celebrate wins, make it easy
```

---

## Key Principles

1. **Measure**: Know your toil before automating
2. **ROI**: Calculate payback period first
3. **Error handling**: Automate carefully
4. **Monitoring**: Know if automation failed
5. **Culture**: Make automation valued
