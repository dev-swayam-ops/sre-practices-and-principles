# Exercises: Introduction and SRE Mindset

Test your understanding of SRE principles with these 10 practical exercises.

---

## Exercise 1: Identify Toil in Daily Work (Easy)

**Scenario**: You spend 2 hours every morning manually checking 50 servers for disk space and restarting services if needed.

**Task**: 
1. Define this task as either "toil" or "engineering work"
2. Explain your reasoning
3. Suggest what you would automate first

**Hint**: Remember, toil is repetitive, manual work that doesn't add permanent value.

---

## Exercise 2: Apply SRE Mindset (Easy)

**Scenario**: Every Friday, you manually run backup scripts on 10 different servers, monitor progress, and send a status email.

**Task**:
1. Identify which SRE principle is being violated
2. Propose a solution using software engineering approach
3. Estimate time saved per month

---

## Exercise 3: Reliability vs. Velocity (Easy)

**Scenario**: Your team wants to deploy new features every day, but your system has had 3 outages in the last month, all from deployments.

**Task**:
1. Is the current velocity sustainable? Why or why not?
2. What should be balanced, and how?
3. Propose 2 changes to improve both reliability and velocity

---

## Exercise 4: Toil Reduction Planning (Medium)

**Scenario**: Your on-call engineer spends 4 hours every week on:
- Database log rotation (30 min, runs weekly)
- Certificate renewal checks (1 hour, runs monthly)
- Manual health checks (2.5 hours, runs daily)
- Incident post-mortems (30 min, runs as needed)

**Task**:
1. Identify which tasks are toil
2. Rank them by automation potential (1=easiest, 3=hardest)
3. Calculate total time that could be saved annually if all toil is automated

---

## Exercise 5: Measuring Success (Medium)

**Scenario**: You propose automating a task that currently takes 6 hours/week manually.

**Task**:
1. Design 3 metrics to measure the automation's success
2. Define what "success" looks like (improvement threshold)
3. How would you communicate this success to management?

---

## Exercise 6: Error Budget Thinking (Medium)

**Scenario**: Your manager says, "We need 100% uptime or we'll lose customers."

**Task**:
1. Explain why 100% uptime is impossible and impractical
2. Discuss the cost of pursuing 99.99% vs. 99.9% reliability
3. Suggest how you'd educate the team on error budgets

---

## Exercise 7: Blameless Culture (Medium)

**Scenario**: Production database crashed during a deployment. The engineer who deployed it is blamed in a meeting.

**Task**:
1. Explain why this approach contradicts SRE principles
2. Reframe the incident with an SRE mindset
3. Write 3 questions you'd ask to identify the real root cause (system/process, not person)

---

## Exercise 8: Automation ROI (Medium)

**Scenario**: Building a monitoring and alerting system takes 3 weeks but would save 15 hours/week in manual checks.

**Task**:
1. Calculate the break-even point (when savings exceed effort)
2. Calculate annual savings after break-even
3. What other benefits (non-time) would you mention to justify the investment?

---

## Exercise 9: Design a Simple Automation (Medium)

**Scenario**: You need to check if a service is running and restart it if it's down. Currently, this is checked manually every 30 minutes.

**Task**:
1. Write a simple pseudocode for a script that:
   - Checks if the service is running
   - Restarts it if it's down
   - Logs the action
   - Alerts if it fails 3 times in a row
   
2. Explain why this is better than manual checking

---

## Exercise 10: SRE in Your Team (Medium)

**Scenario**: You're tasked with introducing SRE principles to a team that currently uses a traditional DevOps model with manual deployments and firefighting.

**Task**:
1. List 5 concrete changes you'd propose in order of priority
2. For each change, explain how it aligns with SRE principles
3. Identify potential resistance and how you'd address it

---

## Self-Reflection Questions

After completing these exercises, ask yourself:

- ✓ Can I identify toil in my daily work?
- ✓ Do I think about automation ROI before starting projects?
- ✓ Can I explain SRE principles to non-technical stakeholders?
- ✓ Am I ready to learn about SLI/SLO/SLA (next module)?

