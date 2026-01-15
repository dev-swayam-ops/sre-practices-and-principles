# Solutions: Introduction and SRE Mindset

Detailed solutions for all 10 exercises with explanations.

---

## Solution 1: Identify Toil in Daily Work

**Answer**:

1. **This is TOIL** - It's repetitive, manual, and doesn't create lasting value. Every day you do the same thing over again.

2. **Reasoning**:
   - Manual work: ✓ (done by hand)
   - Repetitive: ✓ (happens daily)
   - No permanent value: ✓ (same task tomorrow)
   - Scales linearly with servers: ✓ (add 10 more servers = double the work)

3. **Automate First**: Disk space checks and restart logic
   - Can be fully automated
   - Reduces alert fatigue
   - Gets you notified only when action is really needed

**SRE Principle Applied**: Reduce toil by automating manual operational work

---

## Solution 2: Apply SRE Mindset

**Answer**:

1. **Principle Violated**: "Humans are not reliable" + "Automation"
   - Manual processes are error-prone
   - Humans shouldn't do repetitive work
   - No scalability (more servers = more work)

2. **SRE Solution**:
   ```bash
   # Instead of manual backups:
   # - Use automated backup orchestration (e.g., with cron + monitoring)
   # - Use a backup service (managed solution)
   # - Write a script that:
   #   - Runs backups in parallel (faster)
   #   - Monitors success/failure
   #   - Sends alerts automatically (not emails)
   #   - Logs results
   #   - Retries failed backups
   ```

3. **Time Saved**:
   - Current: 2 hours/Friday = 8 hours/month
   - With automation: 30 minutes/month (setup + monitoring)
   - Savings: **7.5 hours/month = 90 hours/year**

**SRE Principle Applied**: Apply software engineering to operations; invest in automation.

---

## Solution 3: Reliability vs. Velocity

**Answer**:

1. **Is this sustainable?** NO
   - 3 outages in 1 month from deployments = ~1 outage every 10 days
   - Users lose trust in new features
   - On-call engineers burn out handling incidents
   - Velocity appears fast, but actual progress is slow (features break)

2. **What to Balance**:
   - **Velocity**: Deploying new features frequently
   - **Reliability**: Keeping the system stable
   - These are NOT opposites if done right!

3. **Proposed Changes**:

   a) **Implement Canary Deployments**
      - Deploy to 5% of users first
      - Monitor errors for 30 minutes
      - Roll back if issues detected
      - Benefit: Find bugs before they affect everyone
   
   b) **Add Pre-deployment Testing**
      - Automated tests catch bugs before production
      - Load tests catch performance issues
      - Benefit: Fewer incidents = faster deployments possible

4. **Result**: More frequent, safer deployments = both high velocity AND high reliability

**SRE Principle Applied**: Balance velocity with stability using systems engineering approaches.

---

## Solution 4: Toil Reduction Planning

**Answer**:

1. **Which tasks are toil?**
   - ✓ Database log rotation (30 min/week) - Repetitive, manual
   - ✓ Certificate renewal checks (1 hour/month) - Preventable, manual
   - ✓ Manual health checks (2.5 hours/week) - Repetitive, manual
   - ✗ Post-mortems (30 min/month) - Incident response, necessary learning

2. **Rank by Automation Potential**:
   - **#1 - Database log rotation (Easiest)**
     - Already runs on a schedule
     - Just need cron job + simple script
     - Time to implement: ~1 hour
   
   - **#2 - Manual health checks (Medium)**
     - Needs monitoring infrastructure
     - Time to implement: ~4 hours
     - But saves most time weekly
   
   - **#3 - Certificate renewal checks (Medium)**
     - Needs automation tools (certbot, etc.)
     - Time to implement: ~2 hours
     - But prevents production incidents

3. **Annual Time Savings**:
   - Log rotation: 30 min/week × 52 weeks = **26 hours/year**
   - Health checks: 2.5 hours/week × 52 weeks = **130 hours/year**
   - Cert renewal: 1 hour/month × 12 months = **12 hours/year**
   - **TOTAL: 168 hours/year saved** (equivalent to 1 full-time engineer!)

**SRE Principle Applied**: Quantify toil to justify automation investments.

---

## Solution 5: Measuring Success

**Answer**:

1. **Three Metrics**:

   a) **Time Saved Per Week**
      - Before: 6 hours/week manual
      - After: 0.5 hours/week (monitoring + 1 fix per month)
      - Goal: Reduce to ≤ 1 hour/week

   b) **Error Rate**
      - Before: 2-3 human errors per month (forgot to run, ran at wrong time)
      - After: 0% human errors
      - Goal: 0 errors for 3 months straight

   c) **Mean Time to Recovery (MTTR)**
      - Before: 30 minutes (manual detection + action)
      - After: < 5 minutes (automated detection + action)
      - Goal: Reduce MTTR by 80%

2. **What Success Looks Like**:
   - Save 5+ hours per week
   - Zero missed runs (100% reliability)
   - Faster recovery from issues

3. **Communicate to Management**:
   ```
   "Our 6-hour weekly manual task is now automated. Benefits:
   
   - Time Savings: 5 hours/week = $500/week = $26,000/year in engineer time
   - Reliability: Mistakes reduced from 2-3/month to zero
   - Speed: Issue response time cut from 30 min to 5 min
   
   Investment: 20 hours to build automation
   ROI: Fully recovered in 4 weeks, then pure savings"
   ```

**SRE Principle Applied**: Measure everything; use data to justify investment.

---

## Solution 6: Error Budget Thinking

**Answer**:

1. **Why 100% is Impossible**:
   - Power failures happen
   - Hardware fails
   - Network issues occur
   - Software bugs exist
   - Even Google, Facebook, Amazon have outages
   - Cost of 100% reliability → ∞ (infinite cost)

2. **Cost Comparison**:

   | Metric | 99.9% | 99.99% | 99.999% |
   |--------|-------|--------|---------|
   | Downtime/year | 8.76 hours | 52 minutes | 5.26 minutes |
   | Cost of infra | $100K | $500K | $2M+ |
   | Deployment frequency | Daily | Weekly | Monthly |
   | On-call fatigue | Low | Medium | High |

   **Insight**: Massive cost jump for tiny reliability gain at the top end.

3. **Educate the Team**:
   ```
   "We can build 99.9% reliability affordably and deploy daily.
   Or we can build 99.999% reliability expensively and deploy monthly.
   
   For most businesses, 99.9% is the sweet spot:
   - Less than 9 hours downtime/year (acceptable)
   - Allows rapid feature deployment (competitive advantage)
   - Reasonable engineering cost
   
   If we need higher reliability, we need a bigger budget—let's discuss trade-offs."
   ```

**SRE Principle Applied**: Use error budgets to balance reliability with business goals.

---

## Solution 7: Blameless Culture

**Answer**:

1. **Why Blaming Is Wrong**:
   - The engineer didn't want the outage
   - Blaming: Focus on person (ineffective)
   - SRE: Focus on systems/processes (effective)
   - Blaming causes engineers to hide failures (opposite of what we want)
   - We learn nothing about the real problem

2. **SRE Reframe**:
   ```
   FROM: "Engineer X made a bad deployment"
   
   TO: "Our deployment process allowed a critical bug to reach production.
        Let's improve our system."
   ```

3. **Root Cause Questions (Focus on System)**:

   a) **"What prevented us from catching this before production?"**
      - Did we have pre-deployment tests? If not, why?
      - Did tests pass? Then tests are inadequate.
      - Could code review catch it? If not, strengthen review process.
   
   b) **"Why wasn't this deployment rolled back faster?"**
      - Do we have monitoring for this issue?
      - Do we have automated rollback?
      - How long did it take to detect?
   
   c) **"How do we prevent this specific issue in the future?"**
      - New test? New monitoring? New validation?
      - Change deployment process?

4. **System Changes Proposed**:
   - Add automated tests for this bug type
   - Add monitoring for this error
   - Implement canary deployments
   - Faster rollback procedure

**SRE Principle Applied**: Create blameless culture; focus on systems, not individuals.

---

## Solution 8: Automation ROI

**Answer**:

1. **Break-Even Calculation**:
   ```
   Development time: 3 weeks = 120 hours
   Weekly savings: 15 hours
   
   Break-even = 120 hours ÷ 15 hours/week = 8 weeks
   
   After 8 weeks: All effort is paid back by time saved
   ```

2. **Annual Savings (After Break-Even)**:
   ```
   Total saved: 15 hours/week × 52 weeks = 780 hours/year
   At $50/hour engineer cost: $39,000/year savings
   
   5-year ROI: $39K × 5 = $195,000 saved
   ```

3. **Non-Time Benefits**:
   - **Reliability**: Automated process never misses checks
   - **Consistency**: Same output every time (errors removed)
   - **Alerting**: Faster issue detection/response
   - **Scalability**: Can handle 10x servers without more effort
   - **Morale**: Engineers do interesting work instead of manual tasks
   - **Documentation**: Code IS the documentation

**SRE Principle Applied**: Justify automation investments with ROI calculations.

---

## Solution 9: Design a Simple Automation

**Answer**:

**Pseudocode**:

```
SCRIPT: check_and_restart_service.sh

DEFINE:
  SERVICE_NAME = "my-app"
  MAX_RESTART_ATTEMPTS = 3
  LOG_FILE = "/var/log/service_check.log"
  ALERT_EMAIL = "oncall@company.com"

MAIN:
  1. Check if SERVICE_NAME is running
     IF running:
       log "Service is healthy"
       EXIT success
     ELSE:
       Continue to step 2

  2. Try to restart SERVICE_NAME
     FOR attempt = 1 to MAX_RESTART_ATTEMPTS:
       systemctl restart SERVICE_NAME
       SLEEP 5 seconds
       IF service is now running:
         log "Service restarted successfully on attempt {attempt}"
         CONTINUE with step 3
       ELSE:
         log "Restart attempt {attempt} failed"
         Continue to next attempt

  3. If still not running:
     log "Service failed to start after {MAX_RESTART_ATTEMPTS} attempts"
     send_alert(ALERT_EMAIL, "Service {SERVICE_NAME} needs manual intervention")
     
  4. EXIT with appropriate status code

LOGGING:
  - Timestamp
  - Service status (up/down)
  - Action taken (restart attempt #)
  - Result (success/failure)
```

**Why Better Than Manual Checking**:

| Aspect | Manual | Automated |
|--------|--------|-----------|
| Frequency | Every 30 minutes (if remembered) | Every 30 minutes (guaranteed) |
| Consistency | Varies (human error) | Always the same |
| Speed | 5 minutes to detect + restart | 30 seconds |
| Coverage | One person on-call | Works for all services |
| Learning | Repeat same task | System improves itself |
| Scale | Doubles with more services | Handles 100s with same effort |

**SRE Principle Applied**: Automate repetitive operational tasks with code.

---

## Solution 10: SRE in Your Team

**Answer**:

1. **Five Changes (In Priority Order)**:

   **#1 - Establish Monitoring & Alerting (Week 1-2)**
   - Install monitoring tool (Prometheus, DataDog, etc.)
   - Define basic health checks
   - Set up on-call alerts
   - Benefit: Stop firefighting, start being proactive
   - SRE Principle: "Measure everything"

   **#2 - Reduce Manual Deployments (Week 3-4)**
   - Implement CI/CD pipeline (GitHub Actions, Jenkins, etc.)
   - All deployments via pipeline (no SSH manual deploys)
   - Benefit: Faster, more reliable deployments; audit trail
   - SRE Principle: "Automate manual work"

   **#3 - Define SLOs (Week 5-6)**
   - Decide what "reliable enough" means (99.9%? 99.95%?)
   - Track against SLO
   - Use error budget to decide on features vs. stability
   - Benefit: Clear goals, data-driven decisions
   - SRE Principle: "Error budgets"

   **#4 - Implement Incident Postmortems (Week 7)**
   - Blameless postmortems for all incidents
   - Document root causes and preventions
   - Track action items
   - Benefit: Learn from failures, reduce repeats
   - SRE Principle: "Blameless culture"

   **#5 - Start Toil Reduction Program (Ongoing)**
   - Audit all manual tasks (make a list)
   - Prioritize by time + pain
   - Automate top 3 per month
   - Benefit: More time for strategic work
   - SRE Principle: "Reduce toil"

2. **How Each Aligns with SRE**:

   | Change | SRE Principle |
   |--------|---------------|
   | Monitoring | Measure everything |
   | CI/CD | Automate repetitive work |
   | SLOs | Data-driven decisions |
   | Postmortems | Learn from failures, blameless |
   | Toil reduction | Humans should do complex work |

3. **Address Resistance**:

   | Objection | Response |
   |-----------|----------|
   | "We're too busy to change" | We're burning out on manual work. Automation saves time. |
   | "We've always done it this way" | And it doesn't scale. We want sustainable growth. |
   | "This is too technical" | Start small. CI/CD is a basics. Tools do the work. |
   | "We need to maintain legacy systems" | SRE improves all systems, new or old. |
   | "Who has time to write all that code?" | Team effort, phased approach, starts paying back immediately. |

   **Pitch to Management**:
   ```
   "We're currently spending 40% of our time on repetitive manual work.
    SRE practices let us automate that, freeing the team for innovation.
    
    Benefit: Better reliability + faster features + happier engineers
    
    Let's start with monitoring and CI/CD (the quickest wins).
    These alone will save 10+ hours/week."
   ```

**SRE Principle Applied**: Build an SRE culture incrementally, starting with highest impact changes.

---

## Next: Ready for SLI/SLO/SLA?

You've learned the SRE mindset. Next module covers how to measure reliability scientifically.

Key question to reflect on: **What's the one toil in your current work that wastes the most time?**

