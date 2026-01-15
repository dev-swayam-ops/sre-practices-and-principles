# Solutions: Error Budgets and Reliability Planning

Solutions for all 10 exercises with explanations.

---

## Solution 1: Calculate Remaining Budget

**Answer**:

```
Total budget = (1 - 0.999) × 30 × 24 × 60 = 43.2 minutes

Spent so far: 5 minutes
Remaining: 43.2 - 5 = 38.2 minutes

Days left: 20 days (30 - 10 = 20)
```

**Interpretation**: Still very healthy. Can deploy risky changes this week.

---

## Solution 2: Forecast Budget Burn

**Answer**:

```
Daily burn rate = 18 min / 12 days = 1.5 min/day
Total forecast = 1.5 min/day × 30 days = 45 minutes

Comparison:
- Budget: 43 minutes
- Forecast: 45 minutes
- Result: WILL BREACH by ~2 minutes
```

**Action**: Pause risky deployments starting now. Focus on reliability improvements.

---

## Solution 3: Deployment Risk Assessment

**Answer**:

```
Remaining: 25 min
Risk: 10 min
Safety margin: 25 - 10 = 15 min

Is it safe? YES ✓
- You have 15 min buffer after deployment
- Days left (10) is enough for normal incidents
- Recommendation: APPROVE with monitoring
```

---

## Solution 4: Multiple Deployments

**Answer**:

```
Total risk: 5 + 7 + 6 = 18 minutes
Remaining: 28 minutes
Safety buffer needed: 10 minutes minimum

Can you do all three? YES, barely
28 - 18 = 10 min (exactly matches minimum buffer)

Better approach: Don't do all three
- Deploy A (5 min) + B (7 min) = 12 min spent, 16 min buffer ✓
- C is lower priority, defer or reduce risk with feature flags

If must do C: Use canary deployment (5% traffic first) to reduce risk from 6 to 2 min
```

---

## Solution 5: Reliability Investment Planning

**Answer**:

```
Scenario A (Budget = 35 min): INVEST IN RELIABILITY
- Healthy budget (> 30 min remaining)
- More incidents cost more than engineering time
- Prevent future incidents > shipping features now
- 40% risk reduction = huge ROI

Scenario B (Budget = 10 min): PAUSE DEPLOYMENTS
- Critical budget situation
- Must invest in reliability immediately
- Every incident now breaches SLO
- No choice: improve stability first

Next month impact:
- If you invest: Budget will be healthier (fewer incidents)
- If you don't: Budget will be tighter (more incidents)
```

---

## Solution 6: Incident Triage Decisions

**Answer**:

```
After incident: 23 min budget remains (already tight)

Option A (Continue deployments):
- Risk: Will likely breach SLO
- Reward: Ship features
- Outcome: Breach SLO = SLA penalties, customer trust

Option B (Pause + improve):
- Risk: Slower feature velocity this month
- Reward: Prevent SLO breach, healthier next month
- Outcome: Meet SLO, build trust

CHOICE: Option B (Pause and invest)

Explanation to team:
"We're in a critical reliability situation. The incident exhausted
our safety margin. We'll pause riskier deployments and invest 2 weeks
in stability. Next month we'll have budget again for features."
```

---

## Solution 7: SLO Adjustment Decision

**Answer**:

```
Pattern: Repeatedly breaching 99.9% SLO

Questions:
1. Is the SLO realistic? NO
2. Is the service unstable? YES (but improving)
3. What's the trend? (improving or getting worse?)

If improving: Keep SLO, give it 1-2 more months
If stable/worse: Adjust SLO

If adjusting, calculate new target from historical average:
- Jan: 30 min
- Feb: 25 min
- Mar: 40 min
- Average: 31.67 min

New SLO recommendation: 99.93% (50 min budget)
This gives you cushion while maintaining reliability focus.

Customer communication:
"We're adjusting our internal SLO based on operational data.
Our SLA to you remains unchanged at 99.8%. This change allows us
to focus on reliability improvements."
```

---

## Solution 8: Cross-Service Budget Strategy

**Answer**:

```
API Gateway (99.95% = 22 min/month):
- Critical path, all users affected
- Tight budget, must be conservative
- Deployment risk: < 5 min only

Recommendations (99% = 432 min/month):
- Optional feature, graceful degradation possible
- Huge budget, can be aggressive
- Deployment risk: < 50 min acceptable

Deployment Strategy This Week:

1. API Gateway: Only low-risk deployments
   - Bug fixes, config changes
   - No architectural changes
   - Deploy during low-traffic hours

2. Recommendations: Can do all planned work
   - Major feature refactors OK
   - Canary deployments can be skipped (budget allows)
   - Can deploy any time

Engineer Allocation:
- 60% on API Gateway (critical path, tighter feedback loop)
- 40% on Recommendations (but has slack budget)
- When API Gateway budget is healthy: rebalance 50/50

Answer to questions:
Q: "Can we deploy to both?"
A: "Yes, but different strategies. API Gateway is conservative, Recommendations can be aggressive."
```

---

## Solution 9: Budget Forecasting

**Answer**:

```
Historical downtime:
- Jan: 30 min
- Feb: 25 min
- Mar: 40 min
- Average: (30 + 25 + 40) / 3 = 31.67 min/month

SLO Evaluation:
- Current: 99.9% (43 min budget)
- Average actual: 31.67 min
- Assessment: SLO is REALISTIC
  (You're within budget with some safety margin)

Next year planning:
- Continue 99.9% SLO (you can maintain it)
- Set service SLA at 99.8% (10 min buffer from SLO)
- Invest in preventing incidents (not more infrastructure)
- Goal: Reduce average to 20-25 min by next year

If incidents continue at 35+ min average:
- Would need to adjust SLO up to 99.88% (50 min)
- Or invest heavily in reliability
```

---

## Solution 10: Stakeholder Communication

**Answer**:

**Plain English Explanation**:

"Think of error budget like a monthly crash allowance. Our SLO promises customers 99.9% uptime, which gives us 43 minutes of downtime per month.

If we deploy every day, we risk creating incidents. Each incident uses up our budget. When budget runs out before month-end, we either breach our SLA (expensive) or stop deploying (slow).

Our strategy: Deploy features early in the month when budget is high, pause deployments mid-to-late month when budget is low. Plus, we invest in stability improvements to reduce incidents overall."

**Visual Explanation**:

```
Monthly Timeline (99.9% SLO = 43 min budget)

Week 1-2: Healthy budget (35+ min left)
└─ DEPLOY OFTEN: Take on feature work, refactors

Week 2-3: Normal budget (15-35 min left)
└─ DEPLOY CAREFULLY: Low-risk changes, monitoring on

Week 3-4: Low budget (< 15 min left)
└─ FREEZE: Only critical hotfixes, no experiments

If incident happens: Budget decreases → Deployments pause earlier
```

**To CEO**:

"We CAN deploy frequently. But not every change is safe. High-risk changes cost us budget (minutes of allowed downtime). The real question isn't 'can we deploy?' but 'do we have safety margin for this risk?'

We balance velocity and stability by:
1. Deploying safe changes anytime
2. Batching risky changes early in month
3. Investing in reliability when budget is healthy
4. Pausing when budget is tight

This keeps us profitable (meeting SLA) AND fast (shipping regularly)."
```

---

## Next Steps

You've mastered error budgets. Next: Learn how to measure and monitor these metrics in real-time with observability tools.

