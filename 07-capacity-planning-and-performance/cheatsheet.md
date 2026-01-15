# Cheatsheet: Capacity Planning and Performance

Quick reference for capacity planning and optimization.

---

## Capacity Planning Formula

```
Current Capacity × 3 = Target Capacity

Example:
- Current: 1,000 req/sec
- Target: 3,000 req/sec

Why 3x?
- 1x: No safety margin
- 2x: Limited headroom, risky
- 3x: Good balance (safe + cost-effective)
- 5x: Expensive, wasteful
```

---

## Growth Forecasting

```
Annual Growth Rate (AGR): Y% per year
Future = Current × (1 + Y%)^n

Example (20% growth):
Year 1: 1,000 × 1.2 = 1,200
Year 2: 1,000 × 1.2² = 1,440
Year 3: 1,000 × 1.2³ = 1,728
Year 5: 1,000 × 1.2⁵ = 2,488

Key: Start planning when 75% capacity reached
```

---

## Common Bottlenecks & Solutions

| Bottleneck | Symptom | Quick Fix | Real Fix |
|------------|---------|-----------|----------|
| **Database** | Slow queries | Add cache | Add index, optimize |
| **CPU** | 90%+ usage | Auto-scale | Optimize code |
| **Memory** | Swapping | Add RAM | Reduce memory usage |
| **Network** | High latency | Reduce payload | Add CDN |
| **Connections** | Pool exhausted | Increase limit | Connection pooling |

---

## Performance Optimization Priority

```
1. Measure (identify actual bottleneck)
2. Optimize database (biggest gains)
3. Add cache (if database still slow)
4. Optimize code (marginal gains)
5. Scale infrastructure (last resort)

Remember: Optimize before scaling!
```

---

## Load Testing Checklist

```
☐ 50% capacity test (normal operation)
☐ 100% capacity test (peak load)
☐ 150% capacity test (breaking point)

Measure:
☐ Response time (P50, P95, P99)
☐ Error rate
☐ CPU/Memory usage
☐ Database connections
☐ Throughput

Document:
☐ When does it break?
☐ Graceful degradation?
☐ Recovery time?
```

---

## Capacity Planning Timeline

```
Month 1-2:
- Measure current capacity
- Forecast 3-year growth

Month 3:
- Identify bottlenecks
- Plan optimizations

Month 4-6:
- Optimize code/database
- Run load tests

Month 7-12:
- Procurement planning
- Budget approval

Month 13-18:
- Order hardware/services
- Deploy infrastructure

Month 19-24:
- Verify capacity meets plan
- Monitor next growth phase
```

---

## Cost vs Performance Decision Matrix

```
                  Optimize | Scale
Cost (upfront)    Low $    | High $$$
Cost (monthly)    Low $    | Medium $$
Speed improvement 10x      | 2x
Implementation   2 weeks   | 2 months
Reversible       Yes       | Maybe

RECOMMENDATION: Always optimize first
```

---

## Headroom Calculator

```
Used = Current Usage
Total = Max Capacity
Headroom = (Total - Used) / Used × 100%

Example:
CPU: 70% used → 30% headroom
Database: 90% used → 11% headroom
Memory: 40% used → 150% headroom

Rule: Headroom < 20% → Scale soon
```

---

## Auto-scaling Rules

```
SCALE UP when:
- CPU > 70% for 5 minutes
- OR Memory > 80% for 5 minutes
- OR Response time > 2x baseline

SCALE DOWN when:
- CPU < 30% for 15 minutes
- AND Memory < 50%
- AND Response time normal

Cooldown: 5 minutes (prevent flip-flopping)
```

---

## Performance Metrics Table

| Metric | Good | Warning | Critical |
|--------|------|---------|----------|
| Response time P95 | <100ms | 100-500ms | >500ms |
| Error rate | <0.1% | 0.1-1% | >1% |
| CPU usage | <50% | 50-80% | >80% |
| Memory usage | <50% | 50-80% | >80% |
| DB connections | <70% | 70-90% | >90% |

---

**Remember**: Plan ahead, measure first, optimize before scaling.
