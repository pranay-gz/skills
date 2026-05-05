---
name: copilot-context-diff
description: Measure how many tokens any operation adds to the context window. Use this before/after creating a file, running a task, invoking a skill, or executing any workflow.
trigger: user-invocable
---

# Measuring Context Token Diff

To measure how many tokens an operation costs, you **MUST** follow this 3-step workflow strictly:

---

## STEP 1: CAPTURE BEFORE SNAPSHOT (Run this FIRST)

**REQUIRED:** Execute this command now and capture the output:

```bash
LOG=$(ls -t ~/.copilot/logs/process-*.log | head -1)
BEFORE=$(grep "CompactionProcessor" "$LOG" | tail -1 | grep -oE '[0-9]+' | head -1)
echo "BEFORE_SNAPSHOT: $BEFORE tokens"
```

**Output format (example):**
```
BEFORE_SNAPSHOT: 31062 tokens
```

**⚠️ DO NOT PROCEED until you have reported the BEFORE snapshot above.**

---

## STEP 2: RUN YOUR OPERATION

Execute the task, create the file, invoke the skill, or run the workflow you're measuring.

**⚠️ DO NOT SKIP THIS STEP.**

---

## STEP 3: CAPTURE AFTER SNAPSHOT & CALCULATE DIFF (Run this AFTER operation completes)

**REQUIRED:** Execute this command now and report all three values:

```bash
LOG=$(ls -t ~/.copilot/logs/process-*.log | head -1)
AFTER=$(grep "CompactionProcessor" "$LOG" | tail -1 | grep -oE '[0-9]+' | head -1)
DIFF=$((AFTER - BEFORE))
echo "AFTER_SNAPSHOT: $AFTER tokens"
echo "CONTEXT_DIFF: +$DIFF tokens"
```

**Output format (example):**
```
AFTER_SNAPSHOT: 31772 tokens
CONTEXT_DIFF: +710 tokens
```

**⚠️ ALL THREE VALUES must be reported: BEFORE, AFTER, and DIFF.**

---

## Required Report Template

After all 3 steps, report in this format:

```
📊 CONTEXT DIFF MEASUREMENT
  BEFORE: XXX tokens
  AFTER:  XXX tokens
  DELTA:  +XXX tokens
```

## Key Notes

- CompactionProcessor writes to logs after **every** assistant turn — AFTER measurement is always current
- Log file auto-selected as most recently modified — no manual lookups needed
- **You MUST complete all three steps. Skipping any step invalidates the measurement.**
