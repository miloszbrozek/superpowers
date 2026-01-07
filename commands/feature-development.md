---
description: Complete feature development workflow - from idea through brainstorming, implementation to completion
disable-model-invocation: true
---

# Feature Development Workflow

This workflow orchestrates the complete feature development process. Each phase has a specific skill and required output. **Minimize user interruptions - only stop when truly needed.**

## Workflow Overview

```
Phase 1: Setup        → using-git-worktrees → isolated worktree
Phase 2: Design       → brainstorming       → design + implementation plan
Phase 3: Implementation → executing-plans    → working code + tests
Phase 4: Completion   → finishing-branch    → merged/PR/kept
```

**Key principle:** Worktree first. All artifacts (design docs, code, tests) are created inside the isolated worktree from the start.

## Phase 1: Workspace Setup

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
- [ ] **Working directory changed to worktree**

**NO CHECKPOINT.** Report worktree path and test results, then proceed to Phase 2.

**All subsequent phases operate inside the worktree.**

---

## Phase 2: Design + Planning

**Skill:** `superpowers:brainstorming`

**Process:**
1. Invoke the brainstorming skill
2. Create design document WITH implementation plan (see skill for format)
3. Save to `docs/plans/YYYY-MM-DD-<topic>-design.md` (inside worktree)

**Required output:**
- [ ] Design + implementation plan document
- [ ] Document committed to git

**Checkpoint:** The brainstorming skill has ONE checkpoint with options:
- "go" → Automatically proceed to Phase 3
- Answer questions → Update doc, then proceed
- "edit" → Wait for user edit, then proceed

**DO NOT ask additional questions. After "go", proceed directly to Phase 3.**

---

## Phase 3: Implementation

**Skill:** `superpowers:executing-plans`

**Process:**
1. Invoke the executing-plans skill
2. Execute tasks in batches of 3
3. Report and wait for feedback between batches

**Required output:**
- [ ] All plan tasks completed
- [ ] All tests passing
- [ ] Code reviewed and approved

**Checkpoint:** "All tasks complete, tests passing. Ready to finish the branch?"

**DO NOT proceed to Phase 4 until all tasks are done and tests pass.**

---

## Phase 4: Completion

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

| Phase | Skill | Output | User Interaction |
|-------|-------|--------|------------------|
| 1. Setup | using-git-worktrees | worktree | None |
| 2. Design | brainstorming | design + plan | Questions (if any) + "go" |
| 3. Implementation | executing-plans | code + tests | Per-batch reviews |
| 4. Completion | finishing-branch | merged/PR | Choose finish option |

## Rules

1. **Complete each phase before proceeding** - no skipping
2. **Minimize interruptions** - only ask when truly needed
3. **Use TodoWrite** - track progress through phases
4. **Skills handle HOW** - this workflow handles WHAT and WHEN

## Enforcement

**At each phase start:** Announce "Starting Phase N: [name] using [skill]"

**For each checklist:** Create TodoWrite todos from checklist items, mark as you complete them.

**Red Flags - you're skipping the workflow:**

| Thought | Reality |
|---------|---------|
| "Worktree is overkill for this" | Phase 1 is not optional. Isolation prevents mess. |
| "Design is clear from discussion" | Write the document. Phase 2 output is a file. |
| "Let me just start coding" | No code before Phase 3. |
| "Tests pass, let's merge" | Phase 4 first. Present options. |
| "I'll document later" | Documents are checkpoints, not afterthoughts. |

## Starting the Workflow

Create TodoWrite with these items:
- [ ] Phase 1: Setup (using-git-worktrees)
- [ ] Phase 2: Design + Planning (brainstorming)
- [ ] Phase 3: Implementation
- [ ] Phase 4: Completion (finishing-branch)

Then invoke `superpowers:using-git-worktrees` to begin Phase 1.
