---
name: brainstorming
description: "Use before any creative work - creating features, building components, adding functionality. Analyzes codebase, creates design draft, then asks all questions at once."
---

# Brainstorming Ideas Into Designs

## Overview

Turn ideas into design documents with minimal back-and-forth. Analyze first, ask questions once, then implement.

## The Process

### Step 1: Analyze (silent)

**DO NOT ask questions yet.** First, gather context:

- Read relevant files, docs, recent commits
- Understand existing patterns and architecture
- Identify constraints and dependencies
- Form your own opinion about the best approach

### Step 2: Create Draft Design Document

Write a complete design document based on your analysis:

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
[Things you're uncertain about - these become your questions]
```

Save to: `docs/plans/YYYY-MM-DD-<topic>-design.md`

### Step 3: Single Checkpoint

Present everything at once:

```
Design document ready: docs/plans/YYYY-MM-DD-<topic>-design.md

[Brief summary of approach - 2-3 sentences]

Questions before proceeding:
1. [Question from Open Questions]
2. [Question from Open Questions]
3. [Question from Open Questions]

Options:
• "go" - Implement as designed
• "q1: answer, q2: answer" - Answer questions, then implement
• "edit" - I'll modify the design doc, then continue
```

**Wait for user response. One checkpoint, not five.**

### Step 4: Finalize

Based on response:
- **"go"** → Commit doc, proceed to next workflow phase
- **Answers provided** → Update doc with answers, commit, proceed
- **"edit"** → Wait for user to edit, then commit and proceed

## Output

**Required deliverable:**
- Design document at `docs/plans/YYYY-MM-DD-<topic>-design.md`
- Committed to git

**When done:** Report the document path. The workflow orchestrator handles what comes next.

## Key Principles

- **Draft first, ask later** - Form your own opinion before asking
- **All questions at once** - No back-and-forth, one checkpoint
- **YAGNI ruthlessly** - Remove unnecessary features from designs
- **Sensible defaults** - If you can make a reasonable choice, make it
- **Open Questions section** - Put uncertainties there, not inline interruptions

## Anti-Patterns

| Old Way (don't do) | New Way |
|-------------------|---------|
| "What's the scope?" | Analyze codebase, propose scope |
| "Should we use X or Y?" | Pick one, explain why, add to Open Questions if uncertain |
| "Here's section 1, ok?" | Write full doc, one checkpoint at end |
| "What about edge case Z?" | Handle it in design or add to Open Questions |
| 5 questions over 5 messages | All questions in one message |
