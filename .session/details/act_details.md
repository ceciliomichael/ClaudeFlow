# Act Command Details

This file provides detailed operational instructions for the `/act` command.

## /act

**Purpose**: To find and execute the *next* incomplete phase (`[ ]`) from `.session/plan/plan.md`.

**Execution Steps**:

1.  **Find Next Incomplete Phase**: Read `.session/plan/plan.md`. Scan through the phases sequentially. Identify the *first* phase that starts with an incomplete Markdown checkbox (`[ ]`). Store its title and details.
2.  **Handle Completion**: If no phase starting with `[ ]` is found, inform the user the plan is complete and stop. Recommend running `/plan` again if they wish to archive this completed plan and start a new one.
3.  **Check for UI Work & Load Guidelines**: Analyze the details of the *target phase* identified in step 1. Determine if it involves UI work (check category tags like `(ui)` or UI-related content). **If UI work is identified, you MUST read `.session/details/aesthetic_details.md`, internalize its guidelines, and prepare for UNYIELDING adherence. Application of these guidelines during execution is NON-NEGOTIABLE.**
4.  **Execute Phase (from Full Plan)**: 
    *   **Dependency Installation (if any for this phase)**: If the current phase details include new external dependencies:
        *   Provide a clear explanation in chat for why each dependency is needed.
        *   Use `run_terminal_cmd` to execute the planned installation command (e.g., `npm install [package_name]`). Install one package per command unless the plan specifies installing multiple related packages together.
        *   Await user approval and successful execution of the command before proceeding.
    *   Perform file creations/modifications based *only* on the details for the target phase. Use `edit_file`. **If UI work was identified in step 3, you MUST flawlessly and completely apply the aesthetic guidelines from `.session/details/aesthetic_details.md` during execution. NO EXCUSES.** Ensure changes leverage existing functions, variables, and modules whenever practical to maintain consistency and reduce redundancy.
5.  **Mark Phase Complete (in Full Plan)**: Update `.session/plan/plan.md` to mark the target phase (identified in step 1) as complete by changing its checkbox from `[ ]` to `[x]`.
6.  **Log Phase Execution**: Follow the procedure in `.session/details/sessionlog_details.md` to log the execution of the completed phase. **This is the only logging action performed by `/act`.**
7.  **Confirmation**: Report back to the user, confirming which phase (`[x] ...`) was executed and that its status is updated in `.session/plan/plan.md`. Mention if subsequent phases remain, and if this was the final phase, suggest using `/plan [description]` for a new plan or archiving this completed plan. **No memory files are updated by the `/act` command.**

**Goal**: To find the next incomplete phase (`[ ]`) in the main plan, execute it, update its status (`[x]`) in `plan.md`, and log this specific action. Memory updates are handled separately by the `/memory` command. 