# /sessionlog Details

**Purpose**: To create sequentially numbered log entries within a single session folder and maintain an up-to-date summary for the session.

**Execution Steps**:

1.  **Determine Log Target**: Check if the action being logged is explicitly a 'bug fix'. If so, identify the *latest* existing log file (`log<N>.md`) in `.session/logs/session/` as the target for updating. Otherwise, determine the next log number (N+1) for creating a *new* file (`log<N+1>.md`).
2.  **Construct Log Filename**: Based on step 1, determine the full path for the target log file (either the latest existing for a bug fix update, or the new file name).
3.  **Analyze Current State**: Gather information relevant to the specific event being logged (e.g., user request for `/sessionlog`, actions taken by `/act`, bug fix details). Focus on activities *since the last log entry* or the specific fix.
4.  **Get Timestamp**: Record the current date and time for this specific log entry/update.
5.  **Gather Information for Log Entry**: Collect details like:
    *   Files created/modified.
    *   Summary of code changes/actions/fix details.
    *   Relevant decisions or status updates.
    *   Plan status (if applicable).
    *   Pending tasks identified.
6.  **Create or Update Log File**: If creating a new file (not a bug fix), create it (e.g., `.session/logs/session/log<N+1>.md`) with the timestamp and gathered information. If updating for a bug fix, *append* the new timestamp and bug fix details to the latest existing log file identified in step 1. Ensure the session directory exists.
7.  **Update Session Summary**: Read *all* `log*.md` files within the `.session/logs/session/` folder. Generate a concise, bulleted summary covering the key events/changes across all logs. Overwrite the `.session/logs/session/summary.md` file with this summary.
8.  **Confirmation**: Notify the user that the log entry (`log<N+1>.md` or updated `log<N>.md`) and session summary (`summary.md`) have been created/updated within the session folder.

**Goal**: To maintain structured logs with individual entries and an aggregated summary, facilitating easier review. 