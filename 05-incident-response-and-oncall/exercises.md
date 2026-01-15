# Exercises: Incident Response and On-Call

10 exercises on handling incidents and building on-call programs.

---

## Exercise 1: Classify Incidents (Easy)

**Scenario**: Which incidents are SEV 1, 2, 3, or 4?

1. Payment processing down (no one can buy)
2. One user's profile picture not loading
3. API slow (takes 5 sec instead of 0.5 sec)
4. Login button missing on 10% of page loads
5. Email notifications delayed by 1 hour

**Task**: Classify each and explain why.

---

## Exercise 2: Incident Timeline (Easy)

**Scenario**: Incident happened:
- 14:00 - Deploy version 2.1
- 14:05 - Alert: Error rate 10%
- 14:07 - Engineer notices alert
- 14:10 - Engineer finds root cause
- 14:12 - Rollback deployed
- 14:15 - Error rate back to 0.1%

**Task**:
1. How long to respond? (detect to action)
2. How long to mitigate? (action to fix)
3. What improved response time?

---

## Exercise 3: Escalation Policy (Easy)

**Scenario**: Your service is SEV 1 down. Who do you call?

People available:
- Primary on-call engineer (asleep)
- Backup on-call (answering phone)
- Tech lead (in meeting)
- Manager (at desk)

**Task**:
1. Order of escalation
2. When to stop escalating?
3. Who's the incident commander?

---

## Exercise 4: Root Cause vs Symptom (Medium)

**Scenario**: Error rate spiked after deployment.

Data:
- Logs show: "Database connection refused"
- Metrics show: Error rate 15%
- Trace shows: Requests waiting 30 seconds

What's the root cause? (symptom vs cause)

**Task**:
1. List symptoms
2. Find root cause
3. Permanent fix?
4. Quick mitigation?

---

## Exercise 5: Incident Commander (Medium)

**Scenario**: Major incident happening. 15 people on bridge call.

They're saying:
- "Try rolling back!"
- "Let me check the database!"
- "Should we page the database team?"
- "When will this be fixed?"

**Task**:
1. What's the incident commander's job?
2. How to reduce chaos?
3. What questions matter most?

---

## Exercise 6: Postmortem Blame (Medium)

**Scenario**: Incident postmortem. Two views:

Blame version: "Engineer deployed without testing. Engineer is careless."

**Task**:
1. Rewrite as blameless
2. What system changes would prevent it?
3. Why is blameless better?

---

## Exercise 7: Mitigation vs Resolution (Medium)

**Scenario**: Database query takes 30 seconds (should be 1 sec).

Options:
- Add cache (5 min to deploy)
- Optimize query (2 hours to code/test)
- Scale database (1 hour)
- Route traffic to backup DB (10 min)

**Task**:
1. What's mitigation (quick)?
2. What's resolution (real fix)?
3. Which do you do first? Why?

---

## Exercise 8: On-Call Burnout (Medium)

**Scenario**: Your on-call engineer:
- Got 7 pages last night
- Barely slept
- Still working today (required)
- Another 2 incidents likely

**Task**:
1. What causes burnout?
2. How to prevent it?
3. Is rest important during incident?

---

## Exercise 9: Communication During Incident (Medium)

**Scenario**: Payment system down. You're incident commander.

People need to know:
- Users checking status page
- Executive wants update
- Support team helping customers
- Engineers still investigating

**Task**:
1. What updates do you give? When?
2. Who talks to whom?
3. How often to update?

---

## Exercise 10: Runbook Prevention (Medium)

**Scenario**: Same incident happened 3 times in 6 months.

**Task**:
1. Why keep repeating?
2. What should postmortem do?
3. How to prevent 4th time?
4. What's a runbook?

---

## Self-Reflection

- ✓ Can I classify incident severity?
- ✓ Do I understand triage?
- ✓ Can I mitigate quickly?
- ✓ Do I know how to be blameless?
- ✓ Am I ready for on-call?
