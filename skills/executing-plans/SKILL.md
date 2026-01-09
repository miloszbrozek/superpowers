---
name: executing-plans
description: Use when you have a written implementation plan to execute in a separate session with review checkpoints
---

# Executing Plans

## Overview

Load plan, review critically, execute all tasks, report when complete.

**Core principle:** Continuous execution - only stop for blockers or completion.

**Announce at start:** "I'm using the executing-plans skill to implement this plan."

## The Process

### Step 1: Load and Review Plan
1. Read plan file
2. Review critically - identify any questions or concerns about the plan
3. If concerns: Raise them with your human partner before starting
4. If no concerns: Create TodoWrite and proceed

### Step 2: Execute All Tasks
**Execute all tasks without interruption.**

For each task:
1. Mark as in_progress
2. Follow each step exactly (plan has bite-sized steps)
3. Run verifications as specified
4. Mark as completed
5. Immediately proceed to next task

**Do NOT stop between tasks to ask for feedback.** Keep going until either:
- All tasks are complete, OR
- You hit a blocker (see "When to Stop and Ask for Help")

### Step 3: Final Report
When ALL tasks complete:
- Show summary of what was implemented
- Show final verification output (tests passing)
- Say: "All tasks complete, tests passing."

## Output

**When all tasks complete:**
- All plan tasks executed
- All tests passing
- Report: "All tasks complete, tests passing."

The workflow orchestrator handles what comes next.

## When to Stop and Ask for Help

**STOP executing immediately when:**
- Hit a blocker mid-batch (missing dependency, test fails, instruction unclear)
- Plan has critical gaps preventing starting
- You don't understand an instruction
- Verification fails repeatedly

**Ask for clarification rather than guessing.**

## When to Revisit Earlier Steps

**Return to Review (Step 1) when:**
- Partner updates the plan based on your feedback
- Fundamental approach needs rethinking

**Don't force through blockers** - stop and ask.

## Remember
- Review plan critically first
- Follow plan steps exactly
- Don't skip verifications
- Reference skills when plan says to
- Keep going until all tasks complete
- Stop when blocked, don't guess
