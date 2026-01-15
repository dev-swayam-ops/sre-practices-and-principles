# Exercises: SLI, SLO, SLA Fundamentals

Test your understanding of SLI, SLO, SLA, and error budgets with these 10 practical exercises.

---

## Exercise 1: Identify SLI vs SLO vs SLA (Easy)

**Scenario**: Your company has these statements:
- "Our database is up 99.99% of the time"
- "We target 99.9% API availability per month"
- "We promise customers 99.8% uptime or provide 25% refund"

**Task**:
1. Label each statement as SLI, SLO, or SLA
2. Explain why it fits that category
3. Is this a good set of values? Why or why not?

---

## Exercise 2: Define SLI for a Service (Easy)

**Scenario**: You work on a streaming video service (like Netflix). 

**Task**:
1. Identify 3-4 SLIs that matter most to users
2. For each SLI, write a measurable definition
3. Explain why you chose these and not others (e.g., not infrastructure metrics)

**Hint**: Think about user experience, not internal infrastructure.

---

## Exercise 3: Set Realistic SLOs (Easy)

**Scenario**: You have measured your service:
- Current availability: 99.85%
- Current P99 latency: 150ms
- Current error rate: 0.08%

**Task**:
1. Set an SLO for each metric (should be achievable but challenging)
2. Explain your reasoning for each SLO
3. How are these different from the current performance?

---

## Exercise 4: Calculate Error Budget (Medium)

**Scenario**: Your team sets an SLO of 99.5% availability.

**Task**:
1. Calculate the error budget in minutes for:
   - One month (30 days)
   - One week
   - One day
2. What does this mean for deployment risk?
3. If you had an outage that lasted 15 minutes, how much budget is left for the month?

---

## Exercise 5: Understand the SLI/SLO/SLA Buffer (Medium)

**Scenario**: Your product manager says "Our SLA and SLO should be the same—99.9%."

**Task**:
1. Explain why this is risky
2. Suggest a better SLA/SLO pair and justify the difference
3. What happens if you breach the SLA? Who pays for it?
4. What happens if you breach the SLO? What should you do?

---

## Exercise 6: Choose the Right SLI (Medium)

**Scenario**: You have these metrics available:
- Database uptime: 99.99%
- Web server response time: P50=50ms, P99=200ms
- Application error rate: 0.3%
- Network latency: 5ms average
- % of API responses that return data in < 500ms: 98%

**Task**:
1. Which of these should be your SLIs for an API service?
2. Which should NOT be SLIs and why?
3. Write formal SLO statements for your chosen SLIs

---

## Exercise 7: SLO Breach Analysis (Medium)

**Scenario**: Your SLO is 99.9% availability. This month:
- Week 1-3: Running at 99.92% (meeting SLO)
- Week 4: Major incident caused 45 minutes of downtime
- Remaining days: Need to calculate if SLO will be breached

**Task**:
1. Calculate monthly availability so far
2. Will this month meet the 99.9% SLO?
3. If yes, how much budget remains?
4. If no, by how much did you miss?
5. What should this incident teach the team?

---

## Exercise 8: Error Budget Deployment Decision (Medium)

**Scenario**: You're 3 weeks into the month. 
- Monthly error budget: 43 minutes (99.9% SLO)
- Budget used so far: 12 minutes (one small incident)
- Planned: High-risk feature deployment (estimated 8-10 min downtime risk)

**Task**:
1. Should you proceed with the deployment? Why?
2. What alternative approaches could you take?
3. How would you communicate this decision to management?
4. What would change your decision?

---

## Exercise 9: Multi-Service SLO Strategy (Medium)

**Scenario**: You manage three services:
- API Gateway (critical, used by all customers)
- Payment Service (critical, direct revenue impact)
- Recommendation Engine (nice-to-have, used by some customers)

**Task**:
1. Assign different SLOs to each service and justify your choices
2. Why should critical services have stricter SLOs?
3. How do you communicate different SLOs to customers?
4. How does this impact your engineering prioritization?

---

## Exercise 10: SLI to Dashboard (Medium)

**Scenario**: You've defined these SLIs:
- Availability: % of requests returning 2xx/3xx status
- Latency: P99 response time
- Error Rate: % of requests returning 4xx/5xx

**Task**:
1. Write down what you would show on a dashboard for each SLI
2. What threshold or alert would indicate you're approaching SLO breach?
3. How would you visualize error budget consumption over the month?
4. Who should see this dashboard and why?

---

## Self-Reflection Questions

After completing these exercises, ask yourself:

- ✓ Can I explain the difference between SLI, SLO, and SLA?
- ✓ Can I identify good SLIs for any service?
- ✓ Do I understand error budgets and how to use them?
- ✓ Can I make deployment decisions based on error budget?
- ✓ Can I explain SLI/SLO/SLA to both technical and non-technical stakeholders?

