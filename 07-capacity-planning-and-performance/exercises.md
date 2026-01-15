# Exercises: Capacity Planning and Performance

10 exercises on planning capacity and optimizing performance.

---

## Exercise 1: Identify Current Capacity (Easy)

**Scenario**: Your API metrics show:
- CPU: 45% of 16 cores
- Memory: 12GB of 32GB
- Database connections: 450 of 500
- Network bandwidth: 300Mbps of 1000Mbps

**Task**:
1. What's the bottleneck?
2. How much headroom before problems?
3. What fails first under load?

---

## Exercise 2: Growth Rate Calculation (Easy)

**Scenario**: Your service grows 25% per year.

Current traffic: 1,000 requests/sec
Target: 2x current (2,000 req/sec)

**Task**:
1. How many years until 2,000 req/sec?
2. When should you start planning?
3. What if growth is 50%? (faster)

---

## Exercise 3: Capacity Planning Targets (Easy)

**Scenario**: Your current peak load is 500 users.

Rule: Plan for 3x peak capacity.

**Task**:
1. What capacity should you have?
2. Why 3x?
3. What happens at 2x? At 4x?

---

## Exercise 4: Forecast Database Growth (Medium)

**Scenario**:
- Current database: 100GB
- Data grows 20GB/year
- You have 500GB total storage

**Task**:
1. When will you run out?
2. When should you upgrade? (plan ahead)
3. How long does upgrade take?

---

## Exercise 5: Performance Bottleneck Analysis (Medium)

**Scenario**: User complains: "App is slow."

Data:
- API response time: 500ms
  - Network latency: 50ms
  - Database query: 400ms
  - Code processing: 50ms

**Task**:
1. Where's the bottleneck?
2. What should you optimize first?
3. How much improvement possible?

---

## Exercise 6: Cost vs Performance (Medium)

**Scenario**: You can scale in two ways:

Option A: Buy bigger database ($10K)
- Improves speed 2x
- Cost: $10K now

Option B: Optimize queries first ($2K)
- Improves speed 10x
- Cost: $2K + 2 weeks work

**Task**:
1. Which should you do?
2. What's the ROI?
3. Should you do both?

---

## Exercise 7: Cache Hit Rate Impact (Medium)

**Scenario**:
- Database query: 100ms
- Cache hit: 1ms
- Current cache hit rate: 50%
- Average response time: ?

**Task**:
1. Calculate average response time
2. If cache hit improves to 80%, new average?
3. Is caching worth it?

---

## Exercise 8: Load Test Planning (Medium)

**Scenario**: Your current capacity is 1,000 req/sec.

You want to test at:
- 50% capacity (load test)
- 100% capacity (stress test)
- 150% capacity (breaking point)

**Task**:
1. Why test at different loads?
2. What metrics to measure?
3. When is system "breaking"?

---

## Exercise 9: Auto-scaling Decision (Medium)

**Scenario**: Your traffic spikes:
- Normal: 100 req/sec
- Peak: 500 req/sec (5x)
- Duration: 1 hour per day

Options:
1. Buy 5x capacity (expensive, over-provisioned)
2. Auto-scale (cheaper, needs setup)

**Task**:
1. Which is better?
2. How auto-scaling works?
3. What metric triggers scale?

---

## Exercise 10: Multi-Year Capacity Plan (Medium)

**Scenario**: Your business growing 40% per year.

Current: 500 req/sec
Capacity rule: Always have 3x current

**Task**:
1. Calculate demand for 5 years
2. Build capacity plan (when to upgrade?)
3. Estimate total cost
4. What if growth is 60% instead?

---

## Self-Reflection

- ✓ Can I identify bottlenecks?
- ✓ Can I forecast growth?
- ✓ Do I understand tradeoffs?
- ✓ Can I plan capacity?
