# Exercises: Disaster Recovery and Business Continuity

10 exercises on disaster recovery and backup planning.

---

## Exercise 1: Define RTO and RPO (Easy)

**Scenario**: Different services need different RTO/RPO.

Services:
1. Payment processing (critical)
2. Email notifications (optional)
3. User profile (important)
4. Analytics (non-critical)

**Task**:
1. Set RTO for each
2. Set RPO for each
3. Justify your choices

---

## Exercise 2: Backup Frequency (Easy)

**Scenario**: Database grows 100GB/day.

Budget: Can store 1TB total
Question: How often to backup?

**Task**:
1. Daily backup: Keep how many days?
2. Weekly backup: Keep how many weeks?
3. Monthly backup: Keep how many months?

---

## Exercise 3: Calculate Backup Cost (Easy)

**Scenario**:
- Daily backup: 100GB
- Full backup: 500GB
- Monthly incremental: 200GB

Costs:
- S3 storage: $0.023 per GB/month
- Restore: $0.10 per GB

**Task**:
1. Monthly storage cost (1 year retention)?
2. Cost of one restore (500GB)?
3. Is backup cost worth it?

---

## Exercise 4: Active-Passive Failover (Medium)

**Scenario**: Primary database fails.

Timeline:
- 10:00 - Failure detected (alert)
- 10:02 - Incident commander declared
- 10:05 - DNS updated to secondary
- 10:08 - Secondary confirmed healthy
- 10:10 - All clients reconnected

**Task**:
1. Total RTO?
2. What took longest?
3. How to improve?

---

## Exercise 5: Replication Lag (Medium)

**Scenario**: Continuous replication (database to backup).

Replication lag: 10 seconds
Failure at 10:00:00

**Task**:
1. Data loss how much?
2. How to reduce lag?
3. Trade-off?

---

## Exercise 6: Backup Verification (Medium)

**Scenario**: You have daily backups for 1 year.

Never tested restore.

**Task**:
1. What could go wrong?
2. How to verify backups?
3. How often to test?

---

## Exercise 7: Incremental Backup (Medium)

**Scenario**:
- Full backup: 500GB (every Sunday)
- Incremental: 50GB (daily)
- Keep 1 week

To restore Friday's data:
- Need: Full (Sunday) + Mon + Tue + Wed + Thu
- = 500GB + 4 × 50GB = 700GB

**Task**:
1. Time to restore 700GB?
2. Restore full + 4 incremental:
   40 min + 4 × 10 min = 80 min
3. Worth it vs full daily backup?

---

## Exercise 8: Geographic Redundancy (Medium)

**Scenario**: Backup in US-East only.

Disaster: US-East region destroyed

**Task**:
1. What happens to backups?
2. How long to recover?
3. Better strategy?

---

## Exercise 9: Runbook Execution (Medium)

**Scenario**: Database failed, you have runbook.

Steps:
1. Provision DB (automated, 10 min)
2. Restore backup (manual, 45 min)
3. Verify data (manual, 15 min)
4. Switch DNS (manual, 5 min)
5. Monitor (ongoing)

**Task**:
1. Total RTO?
2. Which steps to automate?
3. Priority?

---

## Exercise 10: Business Continuity Plan (Medium)

**Scenario**: Your company has:
- Product API (revenue-generating)
- User service (support backend)
- Analytics (reporting)

If you lose each for 1 day:
- API: $100K lost revenue
- User: Customers can't get help ($10K)
- Analytics: $0 (delayed reporting)

**Task**:
1. Order by importance
2. Which needs failover? (vs backup ok)
3. Allocation of backup resources

---

## Self-Reflection

- ✓ Can I define RTO/RPO?
- ✓ Can I design backup strategy?
- ✓ Do I understand failover?
- ✓ Can I test recovery?
