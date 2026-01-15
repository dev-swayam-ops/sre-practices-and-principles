# 00 - Introduction and SRE Mindset

## What You'll Learn

In this module, you'll understand the foundational principles of Site Reliability Engineering (SRE) and develop the mindset required to build and maintain reliable systems. You'll learn:

- What SRE is and why it matters
- Core SRE principles: automation, monitoring, and reliability
- The balance between velocity and stability
- How to think about systems reliability
- The role of an SRE engineer in modern infrastructure
- Practical approaches to reducing toil and improving automation

## Prerequisites

- Basic understanding of system administration or software development
- Familiarity with Linux/Unix command line (optional but helpful)
- No specialized tools required for this module

## Key Concepts

### What is SRE?

Site Reliability Engineering is an engineering discipline that applies software engineering principles to infrastructure operations. An SRE is someone who:

- Treats operations like a software engineering problem
- Automates manual work
- Measures and monitors system health
- Responds to incidents systematically
- Learns from failures

### Core SRE Principles

1. **Reliability Over Features**: A system that's down serves no features to users. Balance feature velocity with system stability.

2. **Humans Are Not Reliable**: Automate repetitive tasks. Humans should focus on complex problems.

3. **Measure Everything**: You can't improve what you don't measure.

4. **Embrace Failure**: Expect failures and build systems that handle them gracefully.

5. **Error Budgets**: Define acceptable failures and use them to balance innovation with stability.

6. **Reduce Toil**: Toil is repetitive, manual work that doesn't add lasting value. Minimize it.

### The SRE Mindset

**Think Like a Software Engineer**: Apply software engineering best practices to operations.
- Write code to solve operational problems
- Version control everything
- Test changes before production

**Data-Driven Decisions**: Use metrics and data, not intuition.

**Blameless Culture**: Focus on systems, not individuals, when failures occur.

**Continuous Improvement**: Aim for 0% toil. Automate your way to a better job.

## Hands-on Lab: Understanding SRE Principles in Action

### Objective
Apply SRE thinking to identify inefficiencies in a simple operational task.

### Scenario
You're managing a web application with the following manual tasks:
- Check server health every 30 minutes via SSH
- Manually restart failed services
- Send status emails to the team
- Update a spreadsheet with system metrics

### Lab Steps

**Step 1**: Identify the Problem
```bash
# Simulate a simple operational task
echo "Checking server health..."
uptime
df -h
netstat -an | grep LISTEN
```

**Expected Output**:
```
 14:23:15 up 45 days,  3:12,  2 users,  load average: 0.45, 0.32, 0.28
Filesystem     Size  Used Avail Use% Mounted on
/dev/sda1       50G   35G   12G  75% /
lo: flags=73<UP,LOOPBACK,RUNNING>
...
```

**Step 2**: Analyze the Work
```bash
# Count how much time this wastes in a week
echo "Manual health checks per week: 7 days * 48 checks = 336 checks"
echo "Time per check: ~10 minutes"
echo "Total wasted time: 336 * 10 = 3360 minutes = 56 hours/week!"
```

**Step 3**: Think Like an SRE - Automate It
```bash
# Create a monitoring script (the SRE way)
cat > /tmp/check_health.sh << 'EOF'
#!/bin/bash
# Simple health check script
LOAD=$(uptime | awk -F'load average:' '{print $2}' | awk '{print $1}')
DISK=$(df -h / | awk 'NR==2 {print $5}' | sed 's/%//')
MEMORY=$(free | awk 'NR==2 {printf "%.0f", $3/$2 * 100}')

echo "System Health Report:"
echo "Load Average: $LOAD"
echo "Disk Usage: $DISK%"
echo "Memory Usage: $MEMORY%"

# Alert if thresholds exceeded
if (( $(echo "$LOAD > 2.0" | bc -l) )); then
    echo "WARNING: High load average!"
fi

if (( DISK > 85 )); then
    echo "WARNING: Disk usage critical!"
fi
EOF

chmod +x /tmp/check_health.sh
/tmp/check_health.sh
```

**Expected Output**:
```
System Health Report:
Load Average: 0.45
Disk Usage: 75%
Memory Usage: 42%
```

**Step 4**: Schedule It (Remove Manual Intervention)
```bash
# Add to cron (automated execution every 30 minutes)
# crontab -e
# */30 * * * * /tmp/check_health.sh >> /var/log/health_check.log 2>&1

echo "✓ Automation configured"
echo "✓ 56 hours/week saved"
echo "✓ Errors reduced (scripts don't make mistakes)"
echo "✓ Time freed for strategic work"
```

### Validation

After setting up the health check script:

```bash
# Verify the script exists and is executable
ls -lh /tmp/check_health.sh

# Check the output
/tmp/check_health.sh

# Simulate scheduling by running it multiple times
for i in {1..3}; do
    echo "Run $i:"
    /tmp/check_health.sh
    sleep 2
done
```

**Success Criteria**:
- ✓ Script runs without errors
- ✓ Output is consistent and readable
- ✓ Can be scheduled without manual intervention
- ✓ Same output every time = reliable

### Cleanup

```bash
# Remove the test script
rm /tmp/check_health.sh

# Verify removal
ls /tmp/check_health.sh
```

**Expected Output**: `ls: cannot access '/tmp/check_health.sh': No such file or directory` (expected)

## Common Mistakes

1. **Ignoring Small Inefficiencies**: "It only takes 5 minutes" adds up over time. If it's repetitive, automate it.

2. **Treating SRE as Just Ops**: SRE is engineering. Write clean, tested code for operational tasks.

3. **Not Measuring Impact**: Before automating, measure the time and effort spent. This justifies the investment.

4. **Assuming Humans Can Be Reliable**: Humans get tired, distracted, and make mistakes. Systems shouldn't rely on perfection.

5. **Setting Unrealistic Reliability Targets**: 99.99% is much harder than 99.9%. Balance reliability with cost.

6. **Ignoring the Human Element**: Automation should free people for complex work, not eliminate jobs. Invest in team growth.

## Troubleshooting

| Problem | Solution |
|---------|----------|
| "This is just DevOps" | SRE is more focused on reliability and measurement. DevOps emphasizes collaboration. |
| "We don't have time to automate" | Not automating wastes more time. Start small with the most painful task. |
| "My systems are stable, why do I need SRE?" | SRE helps you stay stable **and** move fast. The philosophy applies everywhere. |
| "Management won't approve automation projects" | Show ROI: measure time saved, errors prevented, on-call hours reduced. |

## Next Steps

1. **Continue to Module 01**: Learn about SLI, SLO, and SLA - the metrics that define reliability.
2. **Reflect**: Identify 3 repetitive tasks in your current work. Which one would save the most time if automated?
3. **Practice**: Start writing one small automation script this week using the SRE mindset.
4. **Read**: Explore the "SRE Book" (https://sre.google/books/) for deeper insights.

---

**Key Takeaway**: SRE is about being smart with operations. Measure, automate, and continuously improve. Your future self will thank you.
