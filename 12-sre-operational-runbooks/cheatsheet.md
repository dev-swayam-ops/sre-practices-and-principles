# Cheatsheet: SRE Operational Runbooks

Quick reference for creating runbooks.

---

## Runbook Structure Template

```
# RUNBOOK: [Alert Name]

## Quick Check (1 min)
$ [command]
Expected: [output]

## Diagnosis (2-5 min)
$ [command]
Look for: [symptoms]

## Resolution (variable)
Option 1: [action]
Option 2: [action]

## Validation
$ [command]
Expected: [output]

## Rollback
$ [command]
```

---

## Common Runbook Triggers

| Alert | Quick Check | Likely Cause |
|-------|-------------|--------------|
| CPU > 80% | ps aux --sort=-%cpu | Runaway process |
| Memory > 80% | free -h | Memory leak |
| Error rate > 5% | tail -f logs | Bad deployment |
| Disk > 90% | df -h | Large logs |
| Response time > 1s | curl -w timing | Slow query |
| Service down | systemctl status | Crashed |

---

## Decision Tree Template

```
Problem detected
│
├─ Quick check 1
│  ├─ Yes → Action A → Done
│  └─ No → Continue
│
├─ Quick check 2
│  ├─ Yes → Action B → Done
│  └─ No → Continue
│
└─ Escalate
```

---

## Expected Output Checklist

```
For each command in runbook:

☐ What does success look like?
☐ What does failure look like?
☐ What does ambiguous look like?
☐ Next step for each outcome?

Example:
$ systemctl status service

SUCCESS: Active: active (running)
FAILURE: Active: inactive (dead)
UNCLEAR: Active: activating
```

---

## Resolution Options Template

```
Option 1: Quick fix
- Pros: Fast, reversible
- Cons: Might not work
- Risk: Low
- When: Try first

Option 2: Restart
- Pros: Often works
- Cons: Brief downtime
- Risk: Medium
- When: If option 1 fails

Option 3: Rollback
- Pros: Get to known state
- Cons: Data loss risk
- Risk: Medium
- When: If restart fails
```

---

## Validation Commands by Service Type

| Service | Health Check | Details Check |
|---------|-------------|---|
| **Web** | curl http://service/health | curl -w "@timing.txt" |
| **Database** | mysql -e "SELECT 1" | SHOW PROCESSLIST |
| **Cache** | redis-cli ping | redis-cli INFO |
| **Queue** | queue health endpoint | queue depth |
| **Container** | docker ps | docker logs |

---

## Runbook Testing Checklist

```
BEFORE using in production:

☐ Test each command in staging
☐ Verify expected outputs match reality
☐ Time each step
☐ Complete runbook in < 10 minutes
☐ Team member reviews it
☐ Practice with team drill
☐ Document any issues found
☐ Get approval before go-live
```

---

## Maintenance Schedule

```
After incident:
☐ Did runbook work?
☐ Any steps missing?
☐ Any steps wrong?
☐ Update runbook

Quarterly:
☐ Test runbook
☐ Verify commands still work
☐ Update timestamps
☐ Team drill

After system change:
☐ Service names changed?
☐ Config locations changed?
☐ New dependencies?
☐ Update runbook
```

---

## Escalation Decision Matrix

| Time Spent | Action Taken | Next Step |
|-----------|-------------|-----------|
| 0-2 min | Started | Follow runbook |
| 2-5 min | Tried quick fix | Continue or escalate |
| 5-10 min | Multiple attempts | Escalate to team |
| 10+ min | Still broken | Page on-call |
| 30+ min | No progress | Declare SEV-1 |

---

## Red Flags (When to Escalate)

```
❌ Command doesn't exist
❌ Output unexpected
❌ Step takes > 2x expected time
❌ Same fix tried 3x, still failing
❌ Data corruption suspected
❌ Unsure of next step
❌ Multiple failures occurring

→ ESCALATE immediately
```

---

**Remember**: Runbook is for fast action, not learning. Keep it simple.
