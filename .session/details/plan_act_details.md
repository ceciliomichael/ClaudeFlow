# Plan and Act Command Details

This file provides detailed operational instructions for specific custom commands.

## /plan [description]

**Purpose**: To generate a new implementation plan (`plan.md`) and execute its first phase, automatically archiving any previously completed plan.

**Execution Steps**:

0.  **Check for Existing Plan & Archive if Completed**: 
    *   Check if `.session/plan/plan.md` exists.
    *   If it exists:
        *   Read the file.
        *   Determine if *all* phases within it are marked complete (`[x]` checkboxes).
        *   **If all phases ARE complete**: 
            *   Determine the current date string: Use `Get-Date -Format 'yyyyMMdd'`.
            *   List existing archived plans for today: Use `list_dir` on `.session/plan/old/` to find files matching `plan-<date_string>-*.md`.
            *   Calculate the next sequence number (`NNNN`, e.g., 0001, 0002) based on the existing files found. If none exist for today, start with `0001`. Ensure it's zero-padded to 4 digits.
            *   Use PowerShell's `Move-Item -Path ".session\plan\plan.md" -Destination ".session\plan\old\plan-$(Get-Date -Format 'yyyyMMdd')-<NNNN>.md"` command to move the file, embedding the date command and inserting the calculated sequence number `<NNNN>`.
            *   Inform the user that the completed plan was archived using the new naming scheme.
        *   **If any phase is NOT complete**: 
            *   Abort the `/plan` command.
            *   Notify the user that an *active, incomplete* plan exists at `.session/plan/plan.md`. Instruct them to either complete it using `/act` or manually archive/delete it before creating a new plan.
            *   Stop execution here.
    *   If `.session/plan/plan.md` does not exist, continue to step 1.

1.  **Analyze Current State**: Thoroughly examine the current project status, directory structure, and existing files. Review the latest log summary in `.session/logs/session/summary.md` if available.
2.  **Create New Plan File**: Create the file `.session/plan/plan.md`. The `edit_file` tool will create the `.session/plan/` directory if needed.
3.  **Structure the New Plan**: Organize the plan based on the user's `[description]` into logical, numbered phases within the new `.session/plan/plan.md`. Each phase represents a distinct step and should start with an incomplete Markdown checkbox (`[ ]`).
4.  **Detail Each Phase with Category Tags**: For every phase in `.session/plan/plan.md`, specify:
    *   **Phase Title with Checkbox and Category Tags**: Start with `[ ]` followed by the title and appropriate category markers (e.g., `## [ ] Phase 1: Initial Setup (config) (structure)`).
    *   Files: List all files to be created or modified.
    *   Purpose/Functionality: Briefly describe the role of each file or the nature of the changes.
    *   Dependencies: Note any dependencies between files or phases. **Crucially, identify and leverage existing codebase dependencies where possible before introducing new ones.**
    *   Implementation Notes: Add crucial details.
5.  **Execute Phase 1 (from New Plan)**: Read `.session/plan/plan.md`. Identify the details for **Phase 1**. Perform file creations/modifications based *only* on the details for Phase 1. Use `edit_file`.
6.  **Mark Phase 1 Complete (in New Plan)**: Update `.session/plan/plan.md` to mark Phase 1 as complete by changing its checkbox from `[ ]` to `[x]`.
7.  **Log Phase 1 Execution**: Follow the procedure in `.session/details/sessionlog_details.md` to log the execution of Phase 1.
8.  **Confirmation**: Inform the user that the new plan is in `.session/plan/plan.md` and Phase 1 (`[x] ...`) was executed.

**Goal**: To establish a new plan, execute its first step, and ensure any previously completed plan is archived.

## /act

**Purpose**: To find and execute the *next* incomplete phase (`[ ]`) from `.session/plan/plan.md`.

**Execution Steps**:

1.  **Find Next Incomplete Phase**: Read `.session/plan/plan.md`. Scan through the phases sequentially. Identify the *first* phase that starts with an incomplete Markdown checkbox (`[ ]`). Store its title and details.
2.  **Handle Completion**: If no phase starting with `[ ]` is found, inform the user the plan is complete and stop. Recommend running `/plan` again if they wish to archive this completed plan and start a new one.
3.  **Check for UI Work & Apply Advanced Guidelines**: Analyze the details of the *target phase* identified in step 1. If it involves UI work (check category tags like `(ui)`), read `.session/details/aesthetic_details.md` and adhere to its principles during execution.
4.  **Execute Phase (from Full Plan)**: Perform file creations/modifications based *only* on the details for the target phase. Use `edit_file`, applying advanced UI guidelines if needed. **Ensure changes leverage existing functions, variables, and modules whenever practical to maintain consistency and reduce redundancy.**
5.  **Mark Phase Complete (in Full Plan)**: Update `.session/plan/plan.md` to mark the target phase (identified in step 1) as complete by changing its checkbox from `[ ]` to `[x]`.
6.  **Log Phase Execution**: Follow the procedure in `.session/details/sessionlog_details.md` to log the execution of the completed phase.
7.  **Confirmation**: Report back to the user, confirming which phase (`[x] ...`) was executed and that its status is updated in `.session/plan/plan.md`. Mention if subsequent phases remain.

**Goal**: To find the next incomplete phase (`[ ]`) in the main plan, execute it, and update its status (`[x]`) in the main plan. 