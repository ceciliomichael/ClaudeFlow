# /fix Command - Implementation Instructions

When the user invokes the `/fix [issue_description]` command, follow these steps precisely to diagnose and resolve bugs, errors, or implementation issues:

## Step 1: Analyze and Diagnose the Issue

1. **Parse the Issue Description**: Analyze the user's [issue_description] thoroughly to fully understand:
   - The nature of the problem (bug, error, unexpected behavior, performance issue, etc.)
   - Affected components, files, or functionalities
   - Error messages or symptoms reported
   - Expected vs. actual behavior

2. **Gather Context**:
   - If context is unclear, ask targeted questions to better understand the issue
   - Examine relevant files/code using appropriate tools (read_file, codebase_search, grep_search)
   - Check for similar patterns or known issues in the codebase

3. **Root Cause Analysis**:
   - Identify the most likely root cause(s) of the issue
   - Determine whether the issue is due to:
     - Logic errors (incorrect algorithm implementation)
     - Syntax errors (improper language usage)
     - Runtime errors (exceptions, variable scope issues)
     - Integration problems (component incompatibility)
     - Configuration issues (incorrect settings)
     - Missing components or dependencies

## Step 2: Generate and Display Fix Plan

1. **Create Diagnostic Report and Fix Strategy**: Develop a comprehensive plan that includes:
   - **Issue Summary**: Clear restatement of the problem with technical specificity
   - **Root Cause Analysis**: Explanation of the underlying issue(s) identified
   - **Fix Approach**: Strategy for resolving the problem with specific changes
   - **Verification Method**: How the fix will be tested/confirmed to work

2. **Display the Fix Plan in Chat**: Present the entire plan directly in the chat (NOT as a file) with:
   - **Title**: Brief, descriptive title for the fix
   - **Issue Summary**: Concise problem statement
   - **Diagnosis**: Technical explanation of what's causing the issue
   - **Fix Actions**: Ordered, specific steps that will be taken to resolve the issue:
     - Files to be modified
     - Code changes to be made
     - Configuration updates needed
     - Dependencies to be added/updated (if applicable)
   - **Verification**: How the success of the fix will be determined

## Step 3: Execute the Fix Immediately

1. **Implement the Fix**:
   - Make precise code/configuration changes using `edit_file`
   - Modify only what's necessary to resolve the issue
   - Ensure changes maintain code style consistency
   - Add comments explaining the fix if the change isn't self-explanatory

2. **Minimize Side Effects**:
   - Ensure fix doesn't introduce new issues
   - Validate that changes don't break existing functionality
   - Maintain backwards compatibility where appropriate

## Step 4: Log the Fix Process

1. Create a new session log using the `/sessionlog` procedure
2. Document the issue that was fixed, diagnosis details, and implementation
3. Add a "bug fix" tag to the log for proper categorization
4. Update the session summary file to reflect this fix activity

## Step 5: Provide Verification and Completion Confirmation

1. **Explain the Fix**:
   - Summarize what was changed and why
   - Explain how the fix addresses the root cause
   - Provide any necessary user instructions if the fix requires additional steps

2. **Suggest Testing**:
   - Advise the user on how to verify the fix works
   - Suggest specific test cases if applicable
   - Note any limitations or edge cases to be aware of

3. **Future Prevention**:
   - If appropriate, briefly suggest how similar issues might be prevented in the future
   - Recommend any best practices that would help avoid recurrence

**Remember**: The `/fix` command is specialized for surgical, targeted problem resolution. It should focus on accurate diagnosis followed by precise implementation of only what's needed to resolve the specific issue. 