---
description: Complete feature development workflow - from idea through brainstorming, implementation to completion
disable-model-invocation: true
---

# Feature Development Workflow

This workflow orchestrates the complete feature development process. Each phase has a specific skill and required output. **Minimize user interruptions - only stop when truly needed.**

Remember, set these varaibles at the start of the workflow:
INSIDE_WORKFLOW: "true"

## Workflow Overview

```
Phase 1: Setup          → using-git-worktrees     → isolated worktree
Phase 2: Design         → brainstorming           → design + implementation plan
Phase 3: Implementation → executing-plans         → working code + tests
Phase 4: Manual Testing → manual-testing          → verified feature works
Phase 5: Completion     → finishing-branch        → merged/PR/kept
```

**Key principle:** Worktree first. All artifacts (design docs, code, tests) are created inside the isolated worktree from the start.

## Phase 1: Workspace Setup

**Skill:** `superpowers:using-git-worktrees`

**Process:**

1. **Use the Skill tool:** `Skill(skill="using-git-worktrees")` - this loads the skill instructions
2. Follow the loaded skill instructions to create isolated worktree
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

1. **Use the Skill tool:** `Skill(skill="brainstorming")` - this loads the skill instructions
2. Follow the loaded skill to create design document WITH implementation plan
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

1. **Use the Skill tool:** `Skill(skill="executing-plans")` - this loads the skill instructions
2. Follow the loaded skill to execute all tasks without interruption
3. Run verifications as specified in plan

**Required output:**

- [ ] All plan tasks completed
- [ ] All tests passing

**NO CHECKPOINT.** When all tasks complete and tests pass, proceed directly to Phase 4.

---

## Phase 4: Manual Testing

**Skill:** `superpowers:manual-testing`

**Process:**

1. **Use the Skill tool:** `Skill(skill="manual-testing")` - this loads the skill instructions
2. Follow the loaded skill to identify how to run the project
3. Start the project
4. Manually verify the implemented feature:
   - **Web app:** Use browser (MCP chrome-devtools) - navigate, click, fill forms, take screenshots
   - **CLI/Console:** Run commands in terminal, verify output
   - **API:** Test endpoints with curl/httpie
5. Document results with evidence (screenshots/output)

**Required output:**

- [ ] Project running
- [ ] Feature manually tested
- [ ] Evidence collected (screenshots for web, output for CLI)
- [ ] Verification passed OR issues documented

**If issues found:** Return to Phase 3 to fix, then repeat Phase 4.

**If verification passes:** Proceed to Phase 5.

---

## Phase 5: Completion

**Skill:** `superpowers:finishing-a-development-branch`

**Process:**

1. **Use the Skill tool:** `Skill(skill="finishing-a-development-branch")` - this loads the skill instructions
2. Follow the loaded skill to verify tests and present 5 options to user
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

| Phase             | Skill               | Output           | User Interaction          |
| ----------------- | ------------------- | ---------------- | ------------------------- |
| 1. Setup          | using-git-worktrees | worktree         | None                      |
| 2. Design         | brainstorming       | design + plan    | Questions (if any) + "go" |
| 3. Implementation | executing-plans     | code + tests     | Only on blockers          |
| 4. Manual Testing | manual-testing      | verified feature | Only if issues found      |
| 5. Completion     | finishing-branch    | merged/PR        | Choose finish option      |

## Rules

1. **Complete each phase before proceeding** - no skipping
2. **Minimize interruptions** - only ask when truly needed
3. **Use TodoWrite** - track progress through phases
4. **Skills handle HOW** - this workflow handles WHAT and WHEN
5. **Manual testing is mandatory** - don't skip Phase 4

## Enforcement

**At each phase start:** Announce "Starting Phase N: [name] using [skill]"

**For each checklist:** Create TodoWrite todos from checklist items, mark as you complete them.

**Red Flags - you're skipping the workflow:**

| Thought                           | Reality                                            |
| --------------------------------- | -------------------------------------------------- |
| "Worktree is overkill for this"   | Phase 1 is not optional. Isolation prevents mess.  |
| "Design is clear from discussion" | Write the document. Phase 2 output is a file.      |
| "Let me just start coding"        | No code before Phase 3.                            |
| "Tests pass, let's merge"         | Phase 4 first. Manual testing is required.         |
| "Manual testing is unnecessary"   | Users interact manually. Verify it works for them. |
| "I'll document later"             | Documents are checkpoints, not afterthoughts.      |

## Starting the Workflow

Create TodoWrite with these items:

- [ ] Phase 1: Setup (using-git-worktrees)
- [ ] Phase 2: Design + Planning (brainstorming)
- [ ] Phase 3: Implementation (executing-plans)
- [ ] Phase 4: Manual Testing (manual-testing)
- [ ] Phase 5: Completion (finishing-branch)

Then **use the Skill tool**: `Skill(skill="using-git-worktrees")` to begin Phase 1.

---

## CRITICAL: How to Invoke Skills

**You MUST use the Skill tool to invoke skills.** Do NOT:
- Describe the skill's options yourself
- Implement the skill logic manually
- Say "I'm using the X skill" without actually calling the Skill tool

**Correct way:**
```
<tool_use>
<name>Skill</name>
<input>{"skill": "using-git-worktrees"}</input>
</tool_use>
```

The Skill tool loads the skill's instructions into your context. Without it, you're just guessing what the skill does.

**Why this matters:** In a recent session, Claude announced "Starting Phase 5 (finishing-a-development-branch)" but then presented 4 options instead of the required 5, and missed the "Show diff" option entirely because the skill was never actually loaded.
