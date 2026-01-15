# Cheatsheet: Toil Reduction and Automation

Quick reference for identifying and reducing toil.

---

## Toil vs Valuable Work

| Toil | Valuable Work |
|------|---------------|
| Repetitive | Unique |
| Manual | Creative |
| Scriptable | Requires judgment |
| Same every time | Different each time |
| Low learning | High learning |

Examples:
- ❌ Manual user provisioning = Toil
- ✓ Debugging production issue = Valuable
- ❌ Restarting service every week = Toil
- ✓ Building new feature = Valuable

---

## ROI Calculation Formula

```
Breakeven (weeks) = Automation effort (hours) / 
                    (Manual hours per week)

Example:
- Automation effort: 8 hours
- Manual task: 2 hours/week
- Breakeven: 8 / 2 = 4 weeks

At 1 year:
- Hours saved: 52 weeks × 2 hours = 104 hours
- Cost: 8 hours
- Net benefit: 96 hours
```

---

## Toil Tracking Template

```
TASK 1: Manual provisioning
- Frequency: 2 users/week
- Time: 30 min each
- Annual hours: 52 hours
- Pain: Errors, delays
- Automation effort: 6 hours
- Payback: 2 months

TASK 2: Log rotation
- Frequency: Daily
- Time: 5 min
- Annual hours: 30 hours
- Pain: Fills disk
- Automation effort: 2 hours
- Payback: 1.5 weeks

TASK 3: Backup verification
- Frequency: Weekly manual check
- Time: 30 min
- Annual hours: 26 hours
- Pain: Can't verify all
- Automation effort: 8 hours
- Payback: 2 months
```

---

## Bash Script Template with Error Handling

```bash
#!/bin/bash
set -e  # Exit on error

# Configuration
TASK="BackupDB"
LOG_FILE="/var/log/backup.log"

# Logging
log() {
  echo "[$(date +'%Y-%m-%d %H:%M:%S')] $1" | tee -a $LOG_FILE
}

# Error handling
trap 'log "ERROR: Task failed"; exit 1' ERR

log "Starting $TASK"

# Main work
if [ condition ]; then
  log "✓ Task completed"
  exit 0
else
  log "✗ Task failed"
  exit 1
fi
```

---

## Automation Monitoring Checklist

```
☐ Script logs execution
☐ Log includes timestamp + status
☐ Exit code indicates success/failure
☐ Alert on failure (email, Slack)
☐ Alert if not run (missed schedule)
☐ Track success rate
☐ Weekly manual verification
☐ Automated monthly test
☐ Update script on system changes
```

---

## Common Automation Tasks & Effort

| Task | Effort | Annual Hours | Payback |
|------|--------|--------------|---------|
| User provisioning | 6 hours | 52 hours | 2 weeks |
| Log rotation | 2 hours | 30 hours | 1 week |
| Backup verification | 8 hours | 26 hours | 2 weeks |
| Certificate renewal | 4 hours | 2 hours | Too high |
| Deployment | 12 hours | 40 hours | 5 weeks |
| Health checks | 4 hours | 10 hours | 2 weeks |

---

## Healthy Toil Targets

```
By Role:

SRE: < 30% toil, > 70% engineering
On-Call: < 50% toil, > 50% engineering
Junior: 40% toil, 60% engineering (learning)
Senior: < 20% toil, > 80% engineering

Weekly checklist:
☐ Track time on each task
☐ Categorize: Toil vs Creative
☐ % toil calculated
☐ Identify biggest toil source
☐ Plan automation for next sprint
```

---

## Automation Maintenance Schedule

```
Weekly:
- Monitor if automated tasks succeeded
- Check logs for errors
- Alert on failures

Monthly:
- Manual spot-check of automation
- Verify output is correct
- Update runbooks if needed

Quarterly:
- Review automation effectiveness
- Test rollback procedures
- Refresh documentation

Annually:
- Full audit of all automations
- Remove unused scripts
- Plan next round of automation
```

---

## Script Execution Pattern

```
Development:
1. Write script locally
2. Test in staging
3. Test with actual data
4. Dry-run on production
5. Schedule in cron

Monitoring:
- Log every execution
- Alert on failures
- Weekly manual check
- Alert if not executed

Maintenance:
- Git version control
- Change log
- Owner assigned
- Backup owner
- Test schedule
```

---

## Toil Reduction Timeline

```
Month 1: Measure
- Track time on tasks
- Identify top toil (hours/impact)

Month 2: Build
- Automate easiest 3 tasks
- Test thoroughly

Month 3: Deploy & Monitor
- Roll out automations
- Monitor success rate

Month 4-6: Maintenance
- Fix issues
- Add error handling
- Build team skills

Month 6+: Expand
- Automate next tier
- Reduce toil further

Goal: 50% toil → 30% in 6 months
```

---

**Remember**: Automate to free engineers for creativity. Toil is waste; automation is the answer.
