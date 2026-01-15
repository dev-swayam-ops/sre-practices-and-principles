# Solutions: Capacity Planning and Performance

Solutions for all 10 exercises.

---

## Solution 1: Identify Bottleneck

**Answer**:

```
Current usage:
- CPU: 45% (OK)
- Memory: 12GB / 32GB = 37% (OK)
- Connections: 450 / 500 = 90% (BOTTLENECK!)
- Network: 300 / 1000 = 30% (OK)

BOTTLENECK: Database connections (90% used)

Headroom:
- Database: 50 connections left = 11% headroom
- CPU: 9 cores available = 55% headroom
- Memory: 20GB available = 63% headroom

What fails first:
- Database connections (at 90%)
- Next: CPU (if still growing)

Action:
- Increase connection pool immediately
- Plan to scale database soon
- Monitor connection growth
```

---

## Solution 2: Growth Rate Calculation

**Answer**:

```
Current: 1,000 req/sec
Target: 2,000 req/sec
Growth: 25% per year (multiply by 1.25 each year)

Year 1: 1,000 × 1.25 = 1,250 req/sec
Year 2: 1,250 × 1.25 = 1,562 req/sec
Year 3: 1,562 × 1.25 = 1,953 req/sec
Year 4: 1,953 × 1.25 = 2,441 req/sec

Answer: ~3 years to reach 2,000 req/sec

Planning timeline:
- Start planning: Year 2
- Procurement: Year 2 (6 months before)
- Deployment: Year 3 (complete before needed)

At 50% growth (faster):
Year 1: 1,500 req/sec
Year 2: 2,250 req/sec
Answer: ~2 years

Key: Start planning well ahead of need
```

---

## Solution 3: Capacity Planning Targets

**Answer**:

```
Current peak: 500 users
Target capacity: 500 × 3 = 1,500 users

Why 3x?
- Safety margin for spikes
- Time to scale (takes weeks/months)
- Unexpected growth
- Multiple services (each has overhead)

At 2x capacity:
- 1,000 users: OK
- 1,100 users: Tight, risky
- 1,200 users: Overloaded

At 4x capacity:
- 2,000 users: Plenty headroom
- But: Too expensive to maintain
- Wasted resources

Recommendation: 3x is sweet spot
- Safe: Can handle 2x growth
- Cost-effective: Not over-provisioned
```

---

## Solution 4: Database Growth Forecast

**Answer**:

```
Current: 100GB
Growth: 20GB/year
Limit: 500GB

Timeline:
- Now: 100GB used
- Year 1: 120GB
- Year 2: 140GB
- Year 3: 160GB
- Year 4: 180GB
- Year 5: 200GB
- Year 20: 500GB (FULL!)

When to upgrade:
- At 80% (400GB): Time to upgrade
- That's 15 years away
- But upgrade takes 3 months
- So upgrade at year 12 or earlier

Actually:
- Year 12: 100 + (20 × 12) = 340GB (68%)
- Plan upgrade for year 13
- New DB ready before year 14 (80%)

Timeline: 12+ years (plenty of time)

But watch growth rate:
- If growth accelerates (30GB/year)
- Then: Full in 13 years instead of 20
- Adjust plan accordingly
```

---

## Solution 5: Bottleneck Analysis

**Answer**:

```
Response time: 500ms total
- Network: 50ms (10%)
- Database: 400ms (80%)
- Code: 50ms (10%)

BOTTLENECK: Database (80% of time)

What to optimize first:
✓ Database optimization (biggest gain)
- Add index? Could drop 400ms → 50ms
- Cache? Could drop 400ms → 10ms
- Both? Could drop 400ms → 5ms

Not worth optimizing:
- Network (only 50ms, already fast)
- Code (only 50ms)

Expected improvements:
- Add index: 400ms → 100ms (5x faster)
- Add cache: 400ms → 10ms (40x faster)
- Both: 400ms → 5ms (100x faster!)

ROI: Highest gain from database optimization
```

---

## Solution 6: Cost vs Performance Decision

**Answer**:

```
OPTION A: Buy bigger database
- Cost: $10K upfront
- Speed: 2x improvement
- Time: Available immediately
- Best for: When queries are simple/optimized

OPTION B: Optimize queries first
- Cost: $2K + engineer time (2 weeks)
- Speed: 10x improvement
- Time: 2 weeks delay
- Best for: When queries are slow

RECOMMENDATION: Do BOTH, in order

Week 1-2: Optimize queries ($2K)
- Gain: 10x speed
- Cost: $2K

Week 3-4: Still optimizing
- Evaluate results

Month 2: Buy new database ($10K)
- Gain: Another 2x
- Cost: $10K

Total: $12K investment, 20x faster

Why not just buy bigger DB?
- Optimizing is 5x cheaper
- And gives 5x better results
- Always optimize before scaling
```

---

## Solution 7: Cache Hit Rate Impact

**Answer**:

```
Database query: 100ms
Cache hit: 1ms
Current hit rate: 50%

Average response time:
- 50% hit cache (1ms)
- 50% miss cache (100ms)
- Average: (0.5 × 1) + (0.5 × 100) = 50.5ms

With 80% hit rate:
- 80% hit cache (1ms)
- 20% miss cache (100ms)
- Average: (0.8 × 1) + (0.2 × 100) = 20.8ms

Improvement: 50.5ms → 20.8ms (2.4x faster)

Is caching worth it?
✓ Yes: 2.4x improvement from simple cache
✓ Cost: Low (Redis, ~$1K)
✓ Effort: Moderate (implement cache layer)

Additional optimization:
- If database query becomes 50ms (optimized)
- At 50% hit rate: (0.5 × 1) + (0.5 × 50) = 25.5ms
- At 80% hit rate: (0.8 × 1) + (0.2 × 50) = 10.8ms
```

---

## Solution 8: Load Testing

**Answer**:

```
Why test at different loads?

50% capacity (load test):
- Normal operation
- Measure baseline performance
- Expected: Response time < SLA

100% capacity (stress test):
- At peak load
- Expected: Still works, degraded performance
- Measure: Can system handle peak?

150% capacity (breaking point test):
- Beyond peak
- Find where system breaks
- Expected: Graceful degradation or failure
- Measure: When does it break?

Metrics to measure:
- Response time (P50, P95, P99)
- Error rate
- CPU/Memory usage
- Database connections
- Throughput (req/sec)

System "breaking" when:
- Error rate > 1%
- Response time > 2x normal
- Can't scale further
- Manual intervention needed
```

---

## Solution 9: Auto-scaling Decision

**Answer**:

```
SCENARIO:
Normal: 100 req/sec
Peak: 500 req/sec (5x spike, 1 hour/day)

OPTION 1: Buy 5x capacity
- Cost: 5 × server cost = $50K
- Utilization: 100 req/sec / 500 capacity = 20%
- Waste: 80% idle most of day
- ROI: Bad (paying for capacity you don't use)

OPTION 2: Auto-scaling
- Cost: Servers + auto-scaling software = $15K
- Utilization: Always matches load
- Waste: Minimal
- ROI: Good (pay only for what you use)

RECOMMENDATION: Auto-scaling

How auto-scaling works:
1. Monitor: Track request rate
2. Trigger: If req/sec > 300, scale up
3. Scale: Add servers (takes 2-3 min)
4. Monitor: If req/sec < 100, scale down
5. Scale: Remove servers (takes 5-10 min)

Benefits:
- Cost savings: Only pay for used capacity
- Flexibility: Handle unexpected spikes
- Simplicity: Automatic, no manual intervention

Metrics that trigger scaling:
- Request rate (req/sec)
- CPU usage (%)
- Memory usage (%)
- Response time (ms)
```

---

## Solution 10: Multi-Year Capacity Plan

**Answer**:

```
Growth rate: 40% per year
Current: 500 req/sec
Rule: Always have 3x current capacity

Year 1:
- Demand: 500 × 1.4 = 700 req/sec
- Capacity: 700 × 3 = 2,100 req/sec (current)
- OK

Year 2:
- Demand: 700 × 1.4 = 980 req/sec
- Capacity needed: 980 × 3 = 2,940 req/sec
- Current: 2,100 req/sec
- Action: Scale up (due now)

Year 3:
- Demand: 980 × 1.4 = 1,372 req/sec
- Capacity needed: 1,372 × 3 = 4,116 req/sec
- Action: Scale up again

Year 4:
- Demand: 1,372 × 1.4 = 1,920 req/sec
- Capacity needed: 1,920 × 3 = 5,760 req/sec

Year 5:
- Demand: 1,920 × 1.4 = 2,688 req/sec
- Capacity needed: 2,688 × 3 = 8,064 req/sec

CAPACITY PLAN:
- Now: Have 2,100 req/sec capacity
- Year 2: Upgrade to 3,000 req/sec ($30K)
- Year 4: Upgrade to 6,000 req/sec ($50K)
- Year 6: Upgrade to 10,000 req/sec ($70K)

Total cost (5 years): ~$150K
Cost per year: ~$30K

At 60% growth (faster):
- Year 1: 500 × 1.6 = 800
- Year 2: 800 × 1.6 = 1,280
- Year 3: 1,280 × 1.6 = 2,048
- Year 4: 2,048 × 1.6 = 3,276
- Year 5: 3,276 × 1.6 = 5,242

Upgrades sooner (years 1, 3, 5)
Total cost: ~$200K (higher growth = higher cost)
```

---

## Key Principles

1. **Measure**: Know current capacity
2. **Forecast**: Predict 2-3 year growth
3. **Plan ahead**: Upgrades take time
4. **Optimize first**: Cheaper than scaling
5. **3x rule**: Always have 3x safety margin
