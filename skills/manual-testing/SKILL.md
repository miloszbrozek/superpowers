---
name: manual-testing
description: Use after implementation is complete to manually verify the feature works as expected - runs the project and interacts with it (browser for web apps, CLI for console apps)
---

# Manual Testing

> **INVOCATION REQUIRED:** This skill MUST be invoked via the Skill tool: `Skill(skill="manual-testing")`. Do NOT implement this skill's logic manually or describe its steps without first invoking it.

## Overview

Verify implemented features work correctly by running the project and interacting with it manually. Web apps get tested in a browser, CLI tools get tested in the terminal.

**Core principle:** Automated tests prove code works in isolation; manual testing proves the feature works for users.

**Announce at start:** "I'm using the manual-testing skill to verify the implementation manually."

## The Process

### Step 1: Identify Project Type

Read project documentation to determine:

1. **How to run the project** - check in order:
   - `README.md` - look for "Getting Started", "Development", "Running"
   - `package.json` - look for `scripts.dev`, `scripts.start`
   - `Makefile` - look for `run`, `dev`, `start` targets
   - Framework conventions (e.g., `npm run dev`, `python manage.py runserver`)

2. **Project type:**
   - **Web app** → Will use browser (MCP chrome-devtools)
   - **CLI/Console** → Will use terminal (Bash)
   - **Library** → Skip manual testing (no UI to test)
   - **API only** → Use curl/httpie in terminal

### Step 2: Start the Project

Run the project using discovered command:

```bash
# Run in background so we can interact with it
<start-command> &
```

Wait for startup to complete (look for "ready", "listening on", port number, etc.)

### Step 3: Manual Verification

#### For Web Applications

Use MCP chrome-devtools to interact with the browser:

1. **Navigate to the app:**
   ```
   mcp__chrome-devtools__navigate_page → app URL (usually http://localhost:3000 or similar)
   ```

2. **Take initial screenshot:**
   ```
   mcp__chrome-devtools__take_screenshot → verify page loaded
   ```

3. **Test the implemented feature:**
   - Navigate to relevant page/section
   - Fill forms if needed (`mcp__chrome-devtools__fill`)
   - Click buttons/links (`mcp__chrome-devtools__click`)
   - Take screenshots at each step to verify visual state
   - Check for console errors (`mcp__chrome-devtools__list_console_messages`)

4. **Verify expected behavior:**
   - Does the UI show what we expect?
   - Do interactions work correctly?
   - Are there any console errors?

#### For CLI/Console Applications

Use Bash to run and interact:

1. **Run the command with test inputs:**
   ```bash
   <command> <test-arguments>
   ```

2. **Verify output:**
   - Does it produce expected output?
   - Are error cases handled properly?
   - Does help text work?

3. **Test edge cases:**
   - Invalid inputs
   - Missing arguments
   - Boundary conditions

#### For APIs

Use curl or httpie:

1. **Test endpoints:**
   ```bash
   curl -X POST http://localhost:8000/api/endpoint -d '{"test": "data"}'
   ```

2. **Verify responses:**
   - Correct status codes
   - Expected response body
   - Error handling

### Step 4: Report Results

**If all tests pass:**
```
Manual verification complete:
- [What was tested]
- [Screenshots/output showing success]
- Feature works as expected.
```

**If issues found:**
```
Manual verification found issues:
- [Issue 1]: [description] [screenshot/output]
- [Issue 2]: [description] [screenshot/output]

These need to be fixed before proceeding.
```

### Step 5: Cleanup

Stop all processes that were started for testing:

1. **Find running processes:**
   ```bash
   # List background jobs started in this session
   jobs -l

   # Or find processes by port (e.g., for web apps)
   lsof -i :3000  # adjust port number as needed
   ```

2. **Stop the processes:**
   ```bash
   # Kill by job number
   kill %1

   # Or kill by PID
   kill <pid>
   ```

3. **Verify cleanup:**
   ```bash
   # Confirm no orphaned processes
   jobs -l
   ```

**Important:** Always clean up before finishing, even if issues were found. Leaving processes running wastes resources and can cause port conflicts in subsequent testing.

## Output

**When verification passes:**
- Report: "Manual verification complete. Feature works as expected."
- Include evidence (screenshots for web, output for CLI)

**When issues found:**
- List specific issues with evidence
- DO NOT proceed to next phase
- Return to implementation to fix

## MCP Tools Reference

For web testing, use these chrome-devtools MCP tools:

| Action | Tool |
|--------|------|
| Open page | `mcp__chrome-devtools__navigate_page` |
| Screenshot | `mcp__chrome-devtools__take_screenshot` |
| Click element | `mcp__chrome-devtools__click` |
| Fill input | `mcp__chrome-devtools__fill` |
| Fill form | `mcp__chrome-devtools__fill_form` |
| Check console | `mcp__chrome-devtools__list_console_messages` |
| Run JS | `mcp__chrome-devtools__evaluate_script` |

## When to Skip

- **Library without UI** - no manual testing needed
- **Pure refactoring** - behavior unchanged, automated tests sufficient
- **Documentation only** - no runtime behavior

In these cases, report: "Manual testing skipped - [reason]"

## Key Principles

- Always find and follow project's own run instructions
- Take screenshots/save output as evidence
- Test the actual feature that was implemented
- Don't proceed if issues are found
- Clean up (stop servers) when done
