---
name: brainstorming
description: "Use before any creative work - creating features, building components, adding functionality. Analyzes codebase, creates design draft, then asks all questions at once."
---

# Brainstorming Ideas Into Designs

## Overview

Turn ideas into complete design + implementation plan documents. Sequential process (analyze → design → plan) but NO interruptions until the document is complete. Questions at the end, then iterate if needed.

## The Process

### Step 1: Analyze (silent)

**DO NOT ask questions or show output.** Gather context:

- Read relevant files, docs, recent commits
- Understand existing patterns and architecture
- Identify constraints and dependencies
- Form your own opinion about the best approach
- Note uncertainties for Open Questions section

### Step 2: Write Design Section (silent)

**DO NOT ask questions or show output.** Write directly to file:

```markdown
# [Feature] Design

## Goal
[What we're building and why]

## Approach
[Your recommended approach with reasoning]

## Architecture
[Components, data flow, integration points]

## Implementation Notes
[Key files to modify, patterns to follow]

## Testing Strategy
[How to verify it works]

## Open Questions
[Things you're uncertain about - these become your questions at the end]
```

### Step 3: Write Implementation Plan Section (silent)

**DO NOT ask questions or show output.** Append to the same file:

```markdown
---

## Implementation Plan

**Tech Stack:** [Key technologies/libraries involved]

### Task 1: [First Component]

**Files:**
- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123-145`
- Test: `tests/exact/path/to/test.py`

**Steps:**
1. Write failing test for [specific behavior]
2. Run test to verify it fails
3. Implement minimal code to pass
4. Run tests, verify pass
5. Commit: `git commit -m "feat: add [component]"`

### Task 2: [Second Component]
[Same structure...]

### Task N: [Final Integration/Cleanup]
[Same structure...]
```

Save to: `docs/plans/YYYY-MM-DD-<topic>-design.md`

### Step 4: Single Checkpoint

**NOW show output and ask questions.** Present everything at once:

```
Design + implementation plan ready: <absolute-path>/docs/plans/YYYY-MM-DD-<topic>-design.md

[Brief summary of approach - 2-3 sentences]
[Number of implementation tasks: N]

Questions:
1. [Question from Open Questions]
2. [Question from Open Questions]

Options:
• "go" - Proceed as designed
• Answer questions - I'll update design+plan based on answers, then ask again
• "edit" - You edit the doc, I'll wait
```

**Open the document in VSCode:**
```bash
code <absolute-path>/docs/plans/YYYY-MM-DD-<topic>-design.md
```

**Wait for user response.**

### Step 5: Iterate or Finalize

Based on response:
- **"go"** → Commit doc, proceed to next workflow phase
- **Answers provided** → Re-run Steps 2-4 (update design AND plan based on answers, show updated doc, ask if more questions)
- **"edit"** → Wait for user to edit, then re-read doc and proceed

**Loop until user says "go".**

## Output

**Required deliverable:**
- Design + implementation plan at `docs/plans/YYYY-MM-DD-<topic>-design.md`
- Committed to git

**When done:** Report the document path. The workflow orchestrator handles what comes next.

## Implementation Plan Guidelines

Each task should be bite-sized (2-5 minutes):
- Exact file paths always
- Complete code snippets (not "add validation")
- Exact test commands with expected output
- TDD: test first, then implement
- Frequent commits after each task

## Key Principles

- **Sequential but uninterrupted** - Design then plan, but no stops in between
- **Questions at the end only** - Never ask mid-process
- **Iterate on answers** - Update BOTH design and plan when user provides answers
- **YAGNI ruthlessly** - Remove unnecessary features from designs
- **Sensible defaults** - If you can make a reasonable choice, make it

## Anti-Patterns

| Old Way (don't do) | New Way |
|-------------------|---------|
| "What's the scope?" | Analyze codebase, propose scope |
| "Should we use X or Y?" | Pick one, explain why, add to Open Questions |
| "Here's the design, ok?" then "Here's the plan, ok?" | Write both, ONE checkpoint at end |
| Ask question, wait, continue | Collect ALL questions, ask once |
| Update only design on feedback | Update BOTH design and plan |
