---
description: Complete feature development workflow - from idea through brainstorming, planning, implementation to completion
disable-model-invocation: true
---

# Feature Development Workflow

This workflow orchestrates the complete feature development process. Each phase has a specific skill and required output. **DO NOT skip phases or proceed without completing outputs.**

## Workflow Overview

```
Phase 1: Design       → brainstorming      → design document
Phase 2: Setup        → using-git-worktrees → isolated worktree
Phase 3: Planning     → writing-plans      → implementation plan
Phase 4: Implementation → [choose method]  → working code + tests
Phase 5: Completion   → finishing-branch   → merged/PR/kept
```

## Phase 1: Design

**Skill:** `superpowers:brainstorming`

**Process:**
1. Invoke the brainstorming skill
2. Follow its process completely
3. Save design document

**Required output:**
- [ ] Design document at `docs/plans/YYYY-MM-DD-<topic>-design.md`
- [ ] Document committed to git

**Checkpoint:** Show user the design document path. Ask: "Design complete. Ready to set up workspace?"

**DO NOT proceed to Phase 2 until design document exists and is committed.**

---

## Phase 2: Workspace Setup

**Skill:** `superpowers:using-git-worktrees`

**Process:**
1. Invoke the using-git-worktrees skill
2. Create isolated worktree for this feature
3. Run project setup and verify tests pass
4. **CRITICAL: Change working directory to the worktree**

**Required output:**
- [ ] Worktree created at reported path
- [ ] Dependencies installed
- [ ] Baseline tests passing
- [ ] **Working directory changed to worktree** (all subsequent work happens here)

**Checkpoint:** Report worktree path and test results. Ask: "Workspace ready at `<path>`. Ready to create implementation plan?"

**DO NOT proceed to Phase 3 until worktree is ready AND you have cd'd into it.**

**All subsequent phases (3, 4, 5) operate inside the worktree.**

---

## Phase 3: Planning

**Skill:** `superpowers:writing-plans`

**Process:**
1. Invoke the writing-plans skill
2. Create detailed implementation plan with bite-sized tasks
3. Save plan document

**Required output:**
- [ ] Plan document at `docs/plans/YYYY-MM-DD-<feature>-plan.md`
- [ ] Document committed to git

**Checkpoint:** Show user the plan. Ask: "Plan complete. Choose implementation method:"
1. **Subagent-Driven** (this session) - fresh subagent per task, review between tasks
2. **Batch Execution** (this session) - execute in batches, review between batches

**DO NOT proceed to Phase 4 until user chooses implementation method.**

---

## Phase 4: Implementation

**Based on user choice from Phase 3:**

### Option A: Subagent-Driven Development

**Skill:** `superpowers:subagent-driven-development`

**Process:**
1. Invoke the subagent-driven-development skill
2. Follow its process for each task
3. Two-stage review after each task (spec compliance, then code quality)

### Option B: Batch Execution

**Skill:** `superpowers:executing-plans`

**Process:**
1. Invoke the executing-plans skill
2. Execute tasks in batches of 3
3. Report and wait for feedback between batches

**Required output (both options):**
- [ ] All plan tasks completed
- [ ] All tests passing
- [ ] Code reviewed and approved

**Checkpoint:** "All tasks complete, tests passing. Ready to finish the branch?"

**DO NOT proceed to Phase 5 until all tasks are done and tests pass.**

---

## Phase 5: Completion

**Skill:** `superpowers:finishing-a-development-branch`

**Process:**
1. Invoke the finishing-a-development-branch skill
2. Verify tests pass on final code
3. Present 4 options to user
4. Execute chosen option
5. Clean up worktree if applicable

**Required output:**
- [ ] Tests verified passing
- [ ] User chose: merge locally / create PR / keep as-is / discard
- [ ] Choice executed
- [ ] Worktree cleaned up (if applicable)

**Workflow complete.**

---

## Quick Reference

| Phase | Skill | Output | Checkpoint Question |
|-------|-------|--------|---------------------|
| 1. Design | brainstorming | design doc | "Ready to set up workspace?" |
| 2. Setup | using-git-worktrees | worktree | "Ready to create plan?" |
| 3. Planning | writing-plans | plan doc | "Choose implementation method" |
| 4. Implementation | subagent/executing | code + tests | "Ready to finish branch?" |
| 5. Completion | finishing-branch | merged/PR | - |

## Rules

1. **Complete each phase before proceeding** - no skipping
2. **Create outputs before checkpoints** - documents must exist
3. **Wait for user confirmation** - at each checkpoint
4. **Use TodoWrite** - track progress through phases
5. **Skills handle HOW** - this workflow handles WHAT and WHEN

## Enforcement

**At each phase start:** Announce "Starting Phase N: [name] using [skill]"

**For each checklist:** Create TodoWrite todos from checklist items, mark as you complete them.

**Red Flags - you're skipping the workflow:**

| Thought | Reality |
|---------|---------|
| "Design is clear from discussion" | Write the document. Phase 1 output is a file. |
| "Worktree is overkill for this" | Phase 2 is not optional. Isolation prevents mess. |
| "Plan is in my head" | Write it to file. Phase 3 output is a document. |
| "Let me just start coding" | No code before Phase 4. |
| "Tests pass, let's merge" | Phase 5 first. Present options. |
| "I'll document later" | Documents are checkpoints, not afterthoughts. |

## Starting the Workflow

Create TodoWrite with these items:
- [ ] Phase 1: Design (brainstorming)
- [ ] Phase 2: Setup (using-git-worktrees)
- [ ] Phase 3: Planning (writing-plans)
- [ ] Phase 4: Implementation
- [ ] Phase 5: Completion (finishing-branch)

Then invoke `superpowers:brainstorming` to begin Phase 1.
