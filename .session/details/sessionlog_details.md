# /sessionlog Details

**Purpose**: To create sequentially numbered log entries within a single session folder and maintain an up-to-date summary for the session.

**Execution Steps**:

1.  **Determine Log Target**: 
    * Check if the `.session/logs/session/` directory exists. If not, create it.
    * Check if the action being logged is explicitly a 'bug fix' (when called from the `/fix` command or specifically labeled as a bug fix).
    * If it's a bug fix:
        * List all existing log files in `.session/logs/session/` using `list_dir`.
        * Identify the *latest* existing log file (`log<N>.md`) by finding the highest N value.
        * This latest file will be the target for updating with the bug fix information.
    * If it's not a bug fix:
        * List all existing log files in `.session/logs/session/` using `list_dir`.
        * Determine the next log number (N+1) by finding the highest existing N value and adding 1.
        * If no logs exist yet, start with N=1.
        * The new file will be named `log<N+1>.md`.

2.  **Construct Log Filename**: 
    * Based on step 1, determine the full path for the target log file.
    * For bug fixes: Use the path to the latest existing log file.
    * For new entries: Use `.session/logs/session/log<N+1>.md` with the newly calculated number.

3.  **Get Timestamp**: 
    * Record the current date for this specific log entry/update using PowerShell's `Get-Date -Format "yyyy-MM-dd"` command.
    * This MUST be done before gathering detailed log information.

4.  **Analyze Current State**: 
    * Gather information relevant to the specific event being logged:
        * For a standard `/sessionlog` call: Analyze user requests and actions since the last log.
        * For a `/plan` or `/act` call: Record details about the specific phase executed.
        * For a `/fix` call: Capture the bug description, root cause, and resolution.
        * For a `/create` call: Document what was created based on the pending plan.
    * Focus on activities *since the last log entry* or the specific fix.

5.  **Gather Information for Log Entry**: Collect details like:
    * Files created/modified (with brief descriptions)
    * Summary of code changes/actions/fix details
    * Relevant decisions or status updates
    * Plan status (if applicable)
    * Pending tasks identified
    * Challenges encountered and how they were addressed

6.  **Create or Update Log File**: 
    * If creating a new file (not a bug fix):
        * Create a new file (e.g., `.session/logs/session/log<N+1>.md`).
        * Include a header with the log number and timestamp (from step 3).
        * Add all the gathered information in a structured format.
    * If updating for a bug fix:
        * Read the existing content of the latest log file.
        * *Append* the new timestamp (from step 3) and bug fix details to that file.
        * Use a clear separator and "BUG FIX" header to distinguish the appended content.

7.  **Update Session Summary**: 
    * Read *all* `log*.md` files within the `.session/logs/session/` folder.
    * Generate a concise, bulleted summary covering the key events/changes across all logs.
    * Organize by log number with timestamps.
    * Include bug fixes with special notation.
    * Overwrite or create the `.session/logs/session/summary.md` file with this summary.

8.  **Confirmation**: 
    * Notify the user that the log entry has been created or updated.
    * For new logs: Specify the created log file name (`log<N+1>.md`).
    * For bug fix updates: Specify which log was updated with the fix details.
    * Confirm that the session summary (`summary.md`) has been updated with the latest information.

**Goal**: To maintain structured logs with individual entries and an aggregated summary, facilitating easier review of the session's progress and actions taken. 