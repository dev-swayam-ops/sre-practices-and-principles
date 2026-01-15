# Exercises: Real-World SRE Scenarios

10 exercises on handling real incidents.

---

## Exercise 1: Classify Severity (Easy)

**Scenarios**:

1. Payment API returning 500
   Impact: Users can't buy
   
2. Notifications not sending
   Impact: Users don't get alerts
   
3. Report taking 2 hours
   Impact: Delayed reporting
   
4. Dashboard slow (5 sec load)
   Impact: Users frustrated

**Task**:
1. Classify each severity
2. How long acceptable?
3. Escalation needed?

---

## Exercise 2: Triage Under Pressure (Easy)

**Scenario**: 5 alerts firing.

CPU high, Memory high, Disk high, Error rate high, Response time high.

All at 3:00 AM on-call.

**Task**:
1. Which to check first?
2. What's the decision flow?
3. When to call for help?

---

## Exercise 3: Multi-Service Failure (Easy)

**Scenario**: Database slow + Cache full + API timeout

All together. Are they related?

**Task**:
1. What's the root cause?
2. How to investigate?
3. Fix order?

---

## Exercise 4: Escalation Decision (Medium)

**Scenario**: Working on fix for 12 minutes.

Made progress (error rate 10% → 5%)
But still broken.

Options:
A) Keep trying, have good idea
B) Escalate now, maybe help
C) Wait 5 more minutes

**Task**:
1. Which is right?
2. What factors matter?
3. When to escalate?

---

## Exercise 5: Parallel Investigation (Medium)

**Scenario**: SEV-1 incident.

Only one person (you) available.
Multiple systems need checking simultaneously.

Can't check everything fast enough.

**Task**:
1. How to organize?
2. Call for help immediately?
3. What to prioritize?

---

## Exercise 6: Guess vs Verify (Medium)

**Scenario**: Error rate high.

Guess A: Recent deployment caused it
Guess B: Database too slow
Guess C: Cache expired

Time: 2 minutes. What do you do?

**Task**:
1. Check all 3 or assume one?
2. How to verify fastest?
3. Risk of guessing?

---

## Exercise 7: Fix Selection (Medium)

**Scenario**: Database slow. 3 options:

Option 1: Kill slow queries (1 min, temporary)
Option 2: Restart database (5 min, risky)
Option 3: Scale database (20 min, permanent)

Which to do?

**Task**:
1. Priority order?
2. When each is right?
3. How long to wait?

---

## Exercise 8: Communication During Incident (Medium)

**Scenario**: Incident ongoing for 15 minutes.

CEO: "What's happening?"
You: ???

**Task**:
1. What to say?
2. What not to say?
3. How often to update?

---

## Exercise 9: Post-Incident Improvements (Medium)

**Scenario**: Just resolved incident.

Root cause: Unknown configuration value caused deploy

**Task**:
1. How to prevent next time?
2. Runbook improvement?
3. Monitoring addition?

---

## Exercise 10: Incident Drill (Medium)

**Scenario**: Plan team incident drill.

Goals: Practice response, find gaps, improve process

**Task**:
1. What scenario to simulate?
2. How to make realistic?
3. How to measure success?

---

## Self-Reflection

- ✓ Can I classify incidents?
- ✓ Can I investigate under pressure?
- ✓ Do I know when to escalate?
- ✓ Can I lead through incident?
