---
name: xcode-test
description: Build and test iOS apps on simulator using XcodeBuildMCP CLI
argument-hint: "[scheme name or 'current' to use default]"
---

# Xcode Test Command

<command_purpose>Build, install, and test iOS apps on the simulator using XcodeBuildMCP CLI. Captures screenshots, logs, and verifies app behavior.</command_purpose>

## Introduction

<role>iOS QA Engineer specializing in simulator-based testing</role>

This command tests iOS/macOS apps by:
- Building for simulator
- Installing and launching the app
- Taking screenshots of key screens
- Capturing console logs for errors
- Supporting human verification for external flows

## Prerequisites

<requirements>
- Xcode installed with command-line tools
- XcodeBuildMCP CLI installed (`npm install -g xcodebuildmcp` or `npx xcodebuildmcp@latest`)
- Valid Xcode project or workspace
- At least one iOS Simulator available
</requirements>

## Main Tasks

### 0. Verify XcodeBuildMCP CLI is Installed

<check_cli_installed>

**First, check if XcodeBuildMCP CLI is available.**

Try calling:
```bash
xcodebuildmcp --help
```

**If the command is not found:**

Tell the user:
```markdown
**XcodeBuildMCP CLI not installed**

Install it:
\`\`\`bash
npm install -g xcodebuildmcp
\`\`\`

Then run `/xcode-test` again.
```

**Do NOT proceed** until the command works.

</check_cli_installed>

### 1. Discover Project and Scheme

<discover_project>

**Find available projects:**
```bash
xcodebuildmcp simulator discover-projs --workspace-root .
```

**List schemes for the project:**
```bash
xcodebuildmcp simulator list-schemes --project-path ./Project.xcodeproj
```

**If argument provided:**
- Use the specified scheme name
- Or "current" to use the default/last-used scheme

</discover_project>

### 2. Boot Simulator

<boot_simulator>

**List available simulators:**
```bash
xcodebuildmcp simulator list-sims
```

**Build, install, and run in one step (preferred):**
```bash
xcodebuildmcp simulator build-run-sim --scheme [scheme_name] --project-path ./Project.xcodeproj --simulator-name "iPhone 17 Pro"
```

**Wait for simulator to be ready:**
Check simulator state before proceeding with testing.

</boot_simulator>

### 3. Build the App

<build_app>

**Build for iOS Simulator:**
```bash
xcodebuildmcp simulator build-sim --scheme [scheme_name] --project-path ./Project.xcodeproj --simulator-name "iPhone 17 Pro"
```

**Handle build failures:**
- Capture build errors
- Create P1 todo for each build error
- Report to user with specific error details

**On success:**
- Note the built app path for installation
- Proceed to installation step

</build_app>

### 4. Install and Launch

<install_launch>

**Build, install, and launch in one command:**
```bash
xcodebuildmcp simulator build-run-sim --scheme [scheme_name] --project-path ./Project.xcodeproj --simulator-name "iPhone 17 Pro"
```

**Start capturing logs:**
```bash
xcodebuildmcp logging start-sim-log-cap --simulator-id [uuid] --bundle-id [app.bundle.id]
```

</install_launch>

### 5. Test Key Screens

<test_screens>

For each key screen in the app:

**Take screenshot:**
```bash
xcodebuildmcp ui-automation screenshot --simulator-id [uuid] --return-format path
```

**Review screenshot for:**
- UI elements rendered correctly
- No error messages visible
- Expected content displayed
- Layout looks correct

**Snapshot UI for interaction:**
```bash
xcodebuildmcp ui-automation snapshot-ui --simulator-id [uuid]
```

**Interact with UI elements:**
```bash
xcodebuildmcp ui-automation tap --simulator-id [uuid] --x [x] --y [y]
xcodebuildmcp ui-automation type-text --simulator-id [uuid] --text "[input]"
```

Look for:
- Crashes
- Exceptions
- Error-level log messages
- Failed network requests

</test_screens>

### 6. Human Verification (When Required)

<human_verification>

Pause for human input when testing touches:

| Flow Type | What to Ask |
|-----------|-------------|
| Sign in with Apple | "Please complete Sign in with Apple on the simulator" |
| Push notifications | "Send a test push and confirm it appears" |
| In-app purchases | "Complete a sandbox purchase" |
| Camera/Photos | "Grant permissions and verify camera works" |
| Location | "Allow location access and verify map updates" |

Use AskUserQuestion:
```markdown
**Human Verification Needed**

This test requires [flow type]. Please:
1. [Action to take on simulator]
2. [What to verify]

Did it work correctly?
1. Yes - continue testing
2. No - describe the issue
```

</human_verification>

### 7. Handle Failures

<failure_handling>

When a test fails:

1. **Document the failure:**
   - Take screenshot of error state
   - Capture console logs
   - Note reproduction steps

2. **Ask user how to proceed:**
   ```markdown
   **Test Failed: [screen/feature]**

   Issue: [description]
   Logs: [relevant error messages]

   How to proceed?
   1. Fix now - I'll help debug and fix
   2. Create todo - Add to todos/ for later
   3. Skip - Continue testing other screens
   ```

3. **If "Fix now":**
   - Investigate the issue in code
   - Propose a fix
   - Rebuild and retest

4. **If "Create todo":**
   - Create `{id}-pending-p1-xcode-{description}.md`
   - Continue testing

</failure_handling>

### 8. Test Summary

<test_summary>

After all tests complete, present summary:

```markdown
## Xcode Test Results

**Project:** [project name]
**Scheme:** [scheme name]
**Simulator:** [simulator name]

### Build: Pass / Fail

### Screens Tested: [count]

| Screen | Status | Notes |
|--------|--------|-------|
| Launch | Pass | |
| Home | Pass | |
| Settings | Fail | Crash on tap |
| Profile | Skip | Requires login |

### Console Errors: [count]
- [List any errors found]

### Human Verifications: [count]
- Sign in with Apple: Confirmed
- Push notifications: Confirmed

### Failures: [count]
- Settings screen - crash on navigation

### Created Todos: [count]
- `006-pending-p1-xcode-settings-crash.md`

### Result: [PASS / FAIL / PARTIAL]
```

</test_summary>

### 9. Cleanup

<cleanup>

After testing:

**Stop log capture:**
```bash
xcodebuildmcp logging stop-sim-log-cap --log-session-id [session_id]
```

**Check daemon status if needed:**
```bash
xcodebuildmcp daemon status
```

</cleanup>

## Quick Usage Examples

```bash
# Test with default scheme
/xcode-test

# Test specific scheme
/xcode-test MyApp-Debug

# Test after making changes
/xcode-test current
```

## Integration with /workflows:review

When reviewing PRs that touch iOS code, the `/workflows:review` command can spawn this as a subagent:

```
Task general-purpose("Run /xcode-test for scheme [name]. Build, install on simulator, test key screens, check for crashes.")
```
