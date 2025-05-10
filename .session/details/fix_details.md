# /fix Command - Implementation Instructions

When the user invokes the `/fix [issue_description]` command, follow these steps to diagnose and resolve issues:

## Step 1: Analyze the Issue
1. **Parse Issue & Gather Context**:
   - Analyze the user's description to understand the problem type, affected components, and symptoms
   - Use `read_file`, `codebase_search`, or `grep_search` to examine relevant code
   - Ask clarifying questions if needed

2. **Find Root Cause**:
   - Identify the most likely causes (logic, syntax, runtime errors, etc.)
   - Determine which files need modification

## Step 2: Create and Display Fix Plan in Chat
1. **Create a Fix Plan with**:
   - **Issue Summary**: Brief restatement of the problem
   - **Root Cause**: Technical explanation of what's causing the issue
   - **Fix Actions**: Specific steps to resolve the issue (files to modify, changes to make)
   - **Verification Method**: How to confirm the fix works

2. **IMPORTANT: Display the entire plan in CHAT (not as a file)**:
   - Use clear formatting with headers
   - Be specific about files and changes

## Step 3: Execute the Fix
1. **Implement Changes**:
   - Make precise edits using `edit_file`
   - Maintain consistent code style
   - Install dependencies if needed (with appropriate `run_terminal_cmd` usage)

2. **If UI Work Involved**:
   - Read `.session/details/aesthetic_details.md`
   - Explicitly acknowledge applying aesthetic guidelines
   - Ensure all UI work follows the mandatory standards

## Step 4: Log the Fix
1. **Use `/sessionlog` Procedure with Bug Fix Flag**:
   - Update the most recent log file (not create a new one)
   - Clearly mark as "BUG FIX"
   - Include issue, cause, solution, and files changed

## Step 5: Update Memory Files (If They Exist)
1. **Check and Update**:
   - Update `.session/memory/decisions.md` to document the fix
   - Modify `.session/memory/project_state.md` if significant changes were made
   - Add to `.session/memory/tasks.md` with completed status

## Step 6: Provide Confirmation
1. **Summarize the Fix**:
   - What was changed and why
   - How to verify it works
   - Any future prevention tips

Remember: The `/fix` command requires diagnosis followed by precise, targeted implementation. Display the fix plan in chat before execution, and only update existing log files - never create new ones. 