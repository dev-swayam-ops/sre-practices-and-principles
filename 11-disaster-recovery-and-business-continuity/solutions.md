# Solutions: Disaster Recovery and Business Continuity

Solutions for all 10 exercises.

---

## Solution 1: Define RTO and RPO

**Answer**:

```
1. PAYMENT PROCESSING (critical)
   RTO: 15 minutes (users can't buy)
   RPO: 5 minutes (minimal data loss)
   Why: Revenue-generating, can't afford downtime

2. USER PROFILE (important)
   RTO: 1 hour (affects user experience)
   RPO: 30 minutes (moderate data loss ok)
   Why: Important but not revenue-critical

3. EMAIL NOTIFICATIONS (optional)
   RTO: 4 hours (can wait, no immediate impact)
   RPO: 1 hour (delayed emails acceptable)
   Why: Non-critical, can retry

4. ANALYTICS (non-critical)
   RTO: 24 hours (reporting delayed)
   RPO: 12 hours (daily data loss ok)
   Why: Strategic tool, not operational
```

---

## Solution 2: Backup Frequency

**Answer**:

```
Budget: 1TB total storage
Database growth: 100GB/day

Option 1: Daily backup (simple)
- 1 backup = 100GB
- Storage: 1TB / 100GB = 10 daily backups
- Keep: ~10 days of backups

Option 2: Full + incremental (better)
- Full backup: 500GB (Sunday)
- Incremental: 50GB/day (Mon-Sat)
- Weekly: 500 + 6 × 50 = 800GB
- Keep: 1TB / 800GB = 1 week of full + incrementals

Option 3: Full monthly + weekly incrementals
- Month 1: 500GB + 4 × 300GB = 1.7TB (over budget)
- Not viable with 1TB

RECOMMENDATION: Full + incremental weekly
- Full (monthly): 500GB × 12 = 6TB lifetime
- Keep: Latest 2 months (1TB)
- Retain: 1TB full + 2 weeks incrementals
```

---

## Solution 3: Backup Cost

**Answer**:

```
Monthly storage cost (1 year retention):

Scenario: Full weekly (500GB) + daily incremental (50GB)

Per week:
- Full: 500GB
- Incremental: 6 × 50GB = 300GB
- Total: 800GB/week

For 1 year = 52 weeks:
- Storage: 800GB × 52 = 41,600GB = 41.6TB
- Monthly: 41.6TB × 12 / 12 months = 41.6TB avg in storage
- Cost: 41,600GB × $0.023 = ~$950/month

One restore cost:
- Full backup: 500GB
- Restore cost: 500GB × $0.10 = $50
- Plus: Egress bandwidth fees
- Total restore: ~$100

ROI:
- If one critical restore needed per year: $100
- Storage cost: $950 × 12 = $11,400/year
- Insurance value: Worth it (prevents data loss > $11K)

Optimization:
- Compress backups: 500GB → 250GB (50% reduction)
- Archive old: Move year-old to cheaper tier
- Dedup: Remove duplicate data blocks
```

---

## Solution 4: Active-Passive Failover RTO

**Answer**:

```
Timeline:
- 10:00:00 - Failure happens
- 10:00:02 - Alert fires (detection: 2 sec)
- 10:02:00 - Incident commander declared (2 min)
- 10:05:00 - DNS updated (3 min)
- 10:08:00 - Secondary confirmed healthy (3 min)
- 10:10:00 - All clients reconnected (2 min)

Total RTO: 10 minutes

Bottleneck analysis:
1. Detection (2 sec) - OK
2. Incident response (2 min) - Could automate
3. DNS update (3 min) - Could automate
4. Health check (3 min) - Could automate
5. Client reconnect (2 min) - App-dependent

How to improve to < 5 min:
✓ Automate DNS update (50 sec → 5 sec)
✓ Automate health check (3 min → 30 sec)
✓ Improve detection (2 sec → 1 sec)
New RTO: ~2-3 minutes

Further: Use active-active (no switchover needed)
- Both handle traffic
- Failure = half capacity (not zero)
- RTO: < 1 minute
```

---

## Solution 5: Replication Lag Impact

**Answer**:

```
Replication lag: 10 seconds
Failure at 10:00:00

Data loss:
- Last 10 seconds of transactions lost
- Depends on transaction volume:
  Low: 10 transactions lost
  High: 1000 transactions lost

How to reduce lag:
✓ Faster network (decrease latency)
✓ Parallel replication (send multiple)
✓ Increase batch frequency
✓ Optimize replication code

Trade-off:
- Reduce lag 10s → 1s: Cost increase 10%
- Reduce lag 10s → 0s: Cost increase 100% (dual-write)
- Can't be zero without synchronous replication

For payment: 10s lag = unacceptable
For analytics: 10s lag = acceptable
```

---

## Solution 6: Backup Verification

**Answer**:

```
Never tested restore = HIGH RISK

What could go wrong:
✗ Backup file corrupted (can't restore)
✗ Wrong format (can't read)
✗ Missing files (incomplete restore)
✗ Outdated retention (can't restore from 1 year ago)
✗ Encryption key lost (can't decrypt)

"Backup doesn't work until proven otherwise"

How to verify:
WEEKLY:
- Automated: Restore to staging
- Verify: Data integrity check
- Check: All records present
- Duration: < 1 hour

MONTHLY:
- Full restore test (manual)
- Measure actual restore time
- Document problems found
- Update runbook

QUARTERLY:
- Simulate complete failure
- Full recovery procedure
- Time the entire process
- Verify RTO met

Testing checklist:
☐ Backup created successfully
☐ File not corrupted
☐ Can read file format
☐ Restore to new instance
☐ Data integrity verified
☐ No data loss
☐ Consistent state verified
```

---

## Solution 7: Incremental Backup ROI

**Answer**:

```
Full daily backup:
- Size: 500GB/day × 7 = 3.5TB/week
- Storage: 3.5TB
- Restore: 40 min

Full + incremental:
- Size: 500GB + 6 × 50GB = 800GB/week
- Storage: 800GB (77% savings)
- Restore: 40 min (full) + 40 min (incrementals) = 80 min

Trade-off:
SAVE: 2.7TB storage (=$60/month)
COST: Extra 40 min restore time

Worth it? YES
- Save $720/year in storage
- Extra 40 min is acceptable (still < 2 hour RTO)
- More complex (requires testing)

Recommendation: Use full + incremental
```

---

## Solution 8: Geographic Redundancy

**Answer**:

```
Single region (US-East only) = DISASTER RISK

If US-East destroyed:
❌ Backups lost
❌ Can't recover
❌ Total data loss
❌ Business dies

What happens:
- Disaster at 10:00
- No backups = Can't recover
- Hours lost = Days recovery (if possible)
- Impact: Total business interruption

Better strategy (3-2-1 rule):
3 copies:
- Local: 7 days (fast restore)
- Same region: 30 days (backup)
- Different region: 1 year (DR)

Implementation:
- Daily backup: Local NAS
- Weekly backup: S3 US-East (regional)
- Weekly backup: S3 cross-region (DR)

Cost: +50% for cross-region
Benefit: Protects against regional failure
```

---

## Solution 9: Runbook Automation Priority

**Answer**:

```
Current RTO: ~75 minutes
Target RTO: < 60 minutes

Steps:
1. Provision DB: 10 min (AUTOMATE = 5 min saved)
2. Restore backup: 45 min (complex, hard to automate)
3. Verify data: 15 min (Can automate = 10 min saved)
4. Switch DNS: 5 min (AUTOMATE = 4 min saved)
5. Monitor: Ongoing (automatic)

PRIORITY ORDER (by savings):
1. Provision DB: Automate (save 5 min)
   - IaC/Terraform
   - Already takes 10 min, hard to reduce

2. Switch DNS: Automate (save 4 min)
   - Update Route53/DNS programmatically
   - Can be done by runbook

3. Verify data: Automate (save 10 min)
   - Integrity checks
   - Data completeness
   - Can run in parallel with restore

Final RTO: 10 + 45 + 5 + 5 = 65 minutes
Status: Still over 60 min target

To hit 60 min:
- Parallelize: Start verification while restoring
- Result: 75 → 50 minutes
```

---

## Solution 10: Business Continuity Priorities

**Answer**:

```
Value per day:
- API: $100K lost
- User: $10K lost
- Analytics: $0 (delayed)

Priority ranking:
1. PRODUCT API (highest value)
   RTO: 15 min (needs failover)
   RPO: 5 min (continuous replication)
   Cost: High but justified

2. USER SERVICE (moderate value)
   RTO: 1 hour (backup ok)
   RPO: 30 min (hourly backup)
   Cost: Moderate

3. ANALYTICS (low value)
   RTO: 24 hours (can restore next day)
   RPO: 12 hours (daily backup)
   Cost: Low

Allocation of resources:
- API: 50% budget (active-active, continuous replication)
- User: 30% budget (hourly backups, failover capable)
- Analytics: 20% budget (daily backups, can restore anytime)

Total cost: $X
- API: 0.5X (failover + replication)
- User: 0.3X (backup + failover)
- Analytics: 0.2X (backup only)
```

---

## Key Principles

1. **Define RTO/RPO first**: Determines strategy
2. **Test recovery**: Backup untested = doesn't work
3. **Geo-redundancy**: Protect against regional failure
4. **Automation**: Faster recovery
5. **Gradual degradation**: Not all-or-nothing
