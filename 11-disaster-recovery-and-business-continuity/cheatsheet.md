# Cheatsheet: Disaster Recovery and Business Continuity

Quick reference for DR and backup planning.

---

## RTO and RPO Quick Guide

| Service Type | RTO Target | RPO Target |
|---|---|---|
| **Critical** (payment) | 15 minutes | 5 minutes |
| **Important** (user data) | 1 hour | 30 minutes |
| **Standard** (features) | 4 hours | 1 hour |
| **Optional** (analytics) | 24 hours | 12 hours |

---

## 3-2-1 Backup Rule

```
3 copies of data:
- 1 local (fast restore)
- 1 off-site same region (redundancy)
- 1 different region (geo disaster)

2 different media:
- Local NAS/disk (fast)
- Cloud storage/S3 (durable)

1 offline copy:
- Offline/archive (ransomware protection)
```

---

## Backup Frequency Decision

```
How often to backup?
- Critical: Every 5-15 minutes (continuous replication)
- Important: Every 1 hour
- Standard: Every 4-6 hours
- Optional: Every 24 hours

Based on RPO:
RPO 5 min → Backup every 5 min
RPO 1 hour → Backup every 1 hour
RPO 24 hours → Backup daily
```

---

## Backup Strategy Comparison

| Strategy | Cost | Restore Time | Complexity |
|---|---|---|---|
| **Full daily** | High | 1-2 hours | Low |
| **Full weekly + incremental** | Medium | 1-2 hours | Medium |
| **Continuous replication** | Very high | < 1 minute | High |
| **Archive monthly** | Low | 24+ hours | Low |

---

## Failover Types

```
ACTIVE-PASSIVE:
- Primary: All traffic
- Secondary: Idle standby
- RTO: 5-15 minutes (manual switch)
- Cost: 2x infrastructure

ACTIVE-ACTIVE:
- Both: Handle traffic
- Failover: Automatic
- RTO: < 1 minute
- Cost: More complex, 2x infrastructure
```

---

## Recovery Runbook Template

```
INCIDENT: Database corrupted at 10:00 AM

STEP 1: Declare disaster (2 min)
- Notify incident commander
- Estimated RTO: 2 hours

STEP 2: Provision recovery infrastructure (10 min)
- New DB instance (EC2 + storage)

STEP 3: Restore from backup (45 min)
- Download backup from S3
- Restore to new instance

STEP 4: Verify recovery (15 min)
- Data integrity checks
- Consistency verification

STEP 5: Failover (5 min)
- Update DNS/load balancer
- Update connection strings

TOTAL RTO: 77 minutes
```

---

## Replication Lag Impact

```
Lag: < 5 seconds
- Acceptable for most services
- Minimal data loss

Lag: 5-30 seconds
- Acceptable for non-critical
- Small data loss acceptable

Lag: > 1 minute
- Only for low-critical services
- Consider higher frequency
```

---

## Backup Testing Checklist

```
WEEKLY:
☐ Automated restore to staging
☐ Data integrity verified
☐ No data loss confirmed

MONTHLY:
☐ Manual full restore test
☐ Measure actual restore time
☐ Document any issues

QUARTERLY:
☐ Full recovery simulation
☐ Time entire procedure
☐ Verify RTO met
☐ Update runbook

ANNUALLY:
☐ Geo-disaster simulation
☐ Cross-region recovery test
☐ Full business continuity plan
```

---

## Storage Cost Optimization

```
COMPRESSION:
- Database: 50% reduction possible
- Files: 30-70% reduction (type dependent)
- ROI: Usually worth it

DEDUPLICATION:
- Identify duplicate blocks
- Save: 20-50% space
- Cost: CPU for dedup
- ROI: High for large datasets

TIERING:
- Hot (recent): Expensive, fast
- Warm (1 month old): Medium
- Cold (archive): Cheap, slow
- ROI: High for long retention

Example:
- Month 1: S3 standard ($0.023/GB)
- Month 2-3: S3-IA ($0.0125/GB)
- Month 12+: Glacier ($0.004/GB)
```

---

## Disaster Recovery Workflow

```
1. PLAN
   - Define RTO/RPO
   - Design backup strategy
   - Plan failover

2. IMPLEMENT
   - Set up backups
   - Configure failover
   - Automate where possible

3. TEST
   - Verify backups work
   - Test restore procedure
   - Measure RTO

4. MONITOR
   - Track backup success rate
   - Monitor replication lag
   - Alert on failures

5. MAINTAIN
   - Test quarterly
   - Update procedures
   - Adjust based on results
```

---

**Remember**: Disaster recovery is tested only when it's needed. Test before disaster strikes.
