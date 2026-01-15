# Exercises: Monitoring and Alerting Strategies

10 practical exercises on building effective monitoring and alerting.

---

## Exercise 1: Identify SLI Metrics (Easy)

**Scenario**: You monitor an e-commerce checkout service.

**Task**:
1. Identify 3 SLI metrics users care about
2. For each, explain why it matters
3. Exclude infrastructure metrics (like CPU)

---

## Exercise 2: Write Alert Thresholds (Easy)

**Scenario**: Your API's error rate baseline is 0.08% normally.

**Task**:
1. Set warning and critical alert thresholds
2. How long should you wait before alerting? (5 min? 1 min?)
3. Justify your thresholds

---

## Exercise 3: Dashboard Design (Easy)

**Scenario**: You need a dashboard for your on-call engineer.

**Task**:
1. List 5 metrics they should see at a glance
2. Which should be large/prominent?
3. Which are for troubleshooting detail?

---

## Exercise 4: Alert Fatigue Analysis (Medium)

**Scenario**: Your team has 47 active alerts. They ignore half.

**Task**:
1. Categorize alerts by importance (critical/warning/info)
2. Consolidate related alerts into one
3. How many alerts should remain? Why?

---

## Exercise 5: Runbook Creation (Medium)

**Scenario**: Alert fires: "Error rate > 0.5%"

**Task**:
1. Write a 5-step runbook for on-call engineer
2. Include: check steps, diagnosis, actions
3. How long should it take to identify fix?

---

## Exercise 6: Metric Selection (Medium)

**Scenario**: Choose 2 from these metrics:
- Database CPU: 85%
- API response time: P99 = 250ms
- Memory usage: 78%
- Error rate: 0.3%

**Task**:
1. Which 2 are most important to alert on?
2. Why should you ignore the CPU/memory?
3. When would CPU matter?

---

## Exercise 7: Alert Threshold Tuning (Medium)

**Scenario**: Alert "Latency P99 > 200ms" fires 50x/day.

**Task**:
1. Is threshold too sensitive or too low?
2. Should you increase threshold or investigate?
3. How would you decide?

---

## Exercise 8: Multi-Tier Alerting (Medium)

**Scenario**: Design alerts for different severity levels.

**Task**:
1. Write rules for WARNING (inform team)
2. Write rules for CRITICAL (page engineer)
3. Write rules for INFO (log only)

---

## Exercise 9: Dashboard for Different Audiences (Medium)

**Scenario**: Different teams need different information.

**Task**:
1. Design dashboard for on-call engineer (troubleshooting)
2. Design dashboard for product manager (business metrics)
3. Design dashboard for customers (public status)

---

## Exercise 10: Incident Response with Alerts (Medium)

**Scenario**: Multiple alerts fire within minutes:
- High error rate
- High latency
- Low availability

**Task**:
1. Which alert should on-call check first? Why?
2. Which suggests database problem vs. code problem?
3. Write a triage decision tree

---

## Self-Reflection

- ✓ Can I identify which metrics to monitor?
- ✓ Do I understand alert threshold tuning?
- ✓ Can I design dashboards for different uses?
- ✓ Can I write effective runbooks?

