# Exercises: Error Budgets and Reliability Planning

10 practical exercises on using error budgets for planning.

---

## Exercise 1: Calculate Remaining Budget (Easy)

**Scenario**: Your service has 99.9% SLO. It's day 10, you've had 5 minutes downtime.

**Task**:
1. Calculate total monthly error budget
2. Calculate remaining budget
3. How many days left to be more careful?

---

## Exercise 2: Forecast Budget Burn (Easy)

**Scenario**: Day 12, spent 18 minutes. SLO is 99.9%.

**Task**:
1. Calculate current daily burn rate
2. Forecast total downtime by month-end
3. Will you breach SLO?

---

## Exercise 3: Deployment Risk Assessment (Easy)

**Scenario**: 
- Remaining budget: 25 minutes
- Planned deployment risk: 10 minutes
- Days left: 10 days

**Task**:
1. Is it safe to deploy? Why or why not?
2. What's your safety margin?

---

## Exercise 4: Multiple Deployments (Medium)

**Scenario**: Three risky deployments planned this week:
- Deployment A: 5 min risk
- Deployment B: 7 min risk
- Deployment C: 6 min risk
- Remaining budget: 28 min

**Task**:
1. Can you do all three? Why?
2. If not, prioritize them
3. How would you reduce risk for lower-priority ones?

---

## Exercise 5: Reliability Investment Planning (Medium)

**Scenario**: 
- Budget is at 35 minutes (very healthy)
- You can invest 2 weeks to add redundancy (reduce incident risk 40%)
- Or deploy features for 2 weeks

**Task**:
1. Which should you choose? Why?
2. How does this impact next month's budget?
3. What if budget was only 10 minutes left?

---

## Exercise 6: Incident Triage Decisions (Medium)

**Scenario**: An incident occurs, lasting 20 minutes.
- Remaining budget: 23 minutes
- You have two options:
  A) Continue normal deployments (cost: might breach SLO)
  B) Pause deployments for 2 weeks, improve reliability

**Task**:
1. Which should you choose?
2. What's the business trade-off?
3. How would you communicate this to your team?

---

## Exercise 7: SLO Adjustment Decision (Medium)

**Scenario**: For 3 months straight, you've breached 99.9% SLO.

**Task**:
1. Should you adjust SLO? Why?
2. What should new SLO be (show calculation)?
3. How do you explain this to customers?

---

## Exercise 8: Cross-Service Budget Strategy (Medium)

**Scenario**: You have two services:
- API Gateway: 99.95% SLO (critical, 22 min budget)
- Recommendations: 99% SLO (nice-to-have, 432 min budget)

**Task**:
1. Can you deploy risky changes to both this week?
2. Which service gets priority for deployment?
3. How do you allocate engineer time between them?

---

## Exercise 9: Budget Forecasting (Medium)

**Scenario**: Historical data shows:
- January: 2 incidents, 30 min total downtime
- February: 1 incident, 25 min downtime
- March: 3 incidents, 40 min downtime

**Task**:
1. What's the average downtime per month?
2. Is SLO of 99.9% (43 min) realistic?
3. What should you target next year?

---

## Exercise 10: Stakeholder Communication (Medium)

**Scenario**: Your CEO asks: "Why can't we deploy every day if our SLO is 99.9%?"

**Task**:
1. Explain the error budget concept in non-technical terms
2. Show why frequent deployment + high reliability is hard
3. Propose a deployment strategy that balances both

---

## Self-Reflection

- ✓ Do I understand how error budgets guide decisions?
- ✓ Can I forecast when we'll breach SLO?
- ✓ Can I plan reliability improvements strategically?
- ✓ Can I explain error budgets to non-technical people?

