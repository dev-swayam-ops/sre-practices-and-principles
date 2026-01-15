# Exercises: Postmortems and Root Cause Analysis

10 exercises on writing postmortems and finding root causes.

---

## Exercise 1: Identify Symptoms vs Root Cause (Easy)

**Scenario**: Website is slow.

Data points:
- Homepage takes 10 seconds to load
- Database query takes 8 seconds
- No indexes on user table
- Database CPU at 5% (not high)

**Task**:
1. What's the symptom?
2. What's the cause?
3. Why is no index the root cause?

---

## Exercise 2: 5 Whys for a Bug (Easy)

**Scenario**: Email alerts not sending to customers.

Facts:
- Email service returned 500 error
- SMTP credentials wrong in config
- Credentials rotated but not updated
- No notification of credential rotation
- No automated credential sync

**Task**:
1. Ask "Why?" 5 times
2. Find the root cause
3. What's the real fix?

---

## Exercise 3: Blame vs Blameless (Easy)

**Scenario**: Engineer deployed code without testing, caused outage.

Blame version: "Engineer was careless and didn't test."

**Task**:
1. Rewrite as blameless
2. Focus on: Why did system allow untested code?
3. What system changes prevent it?

---

## Exercise 4: Timeline Reconstruction (Medium)

**Scenario**: Alert fired at 10:30. Service was actually slow at 10:20.

Timeline so far:
- 10:20: Database slow (not detected)
- 10:30: Alert fired (detects slow requests)
- 10:31: Engineer notices alert
- 10:45: Engineer investigates

**Task**:
1. What's the detection lag? (10 min)
2. Why so long?
3. How could it be faster?

---

## Exercise 5: Root Cause for Repeated Incident (Medium)

**Scenario**: Same incident happened 3 times in 6 months.

Incident: Database connections exhausted

Fixes applied:
- Incident 1: Restarted database
- Incident 2: Increased pool size
- Incident 3: ???

**Task**:
1. Why does it keep repeating?
2. What's the real root cause?
3. What permanent fix would work?

---

## Exercise 6: Impact Calculation (Medium)

**Scenario**: Incident lasted 15 minutes.

Data:
- 10,000 users affected
- 20% of transactions failed
- Average transaction = $50
- SLA says 99.9% uptime

**Task**:
1. Calculate: Failed transactions ($)
2. Calculate: SLA violation %
3. How to prevent repeats?

---

## Exercise 7: Action Items (Medium)

**Scenario**: Payment API down due to outdated SSL certificate.

Postmortem findings:
- Cert expired (root cause)
- No alert before expiry
- Manual renewal process
- Engineer forgot to renew

**Task**:
1. Preventive action: What stops repeats?
2. Detective action: How to find faster?
3. Reactive action: How to help next time?
4. Who owns each? Timeline?

---

## Exercise 8: Blameless Culture Question (Medium)

**Scenario**: Engineer makes mistake, causes 1-hour outage.

Question: Should engineer be fired?

Your company choices:
- Fire them (blame culture)
- Keep them, improve system (blameless)

**Task**:
1. Which approach prevents future incidents?
2. Why does blameless culture work better?
3. What happens if you blame?

---

## Exercise 9: Fishbone Diagram (Medium)

**Scenario**: API latency increased 10x.

Possible causes:
- Database: Slow query, no cache
- Code: Inefficient code, memory leak
- Network: High latency, packet loss
- Infrastructure: Low CPU, disk slow

**Task**:
1. Create fishbone diagram
2. Trace each to root cause
3. Which is most likely?

---

## Exercise 10: Postmortem Follow-up (Medium)

**Scenario**: Postmortem identified 5 action items.

One month later:
- 2 items done (database monitoring, runbook)
- 2 items in progress (alert tuning, code review)
- 1 item not started (training)

**Task**:
1. Why is follow-up important?
2. How to ensure completion?
3. What happens if items ignored?

---

## Self-Reflection

- ✓ Can I find root causes?
- ✓ Can I write a postmortem?
- ✓ Do I understand blameless?
- ✓ Can I track action items?
