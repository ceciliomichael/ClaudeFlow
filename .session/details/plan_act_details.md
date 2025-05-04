# Plan and Act Command Details

This file provides detailed operational instructions for specific custom commands.

## /plan [description]

**Purpose**: To generate a full implementation plan (`plan.md`) and execute Phase 1 based on its details.

**Execution Steps**:

1.  **Analyze Current State**: Thoroughly examine the current project status, directory structure, and existing files. Review the latest log summary in `.session/logs/session/summary.md` if available.
2.  **Create/Update Full Plan File**: Create or overwrite the file `.session/plan/plan.md`. The `edit_file` tool will create the `.session/plan/` directory if needed.
3.  **Structure the Full Plan**: Organize the plan into logical, numbered phases within `.session/plan/plan.md`. Each phase represents a distinct step.
4.  **Detail Each Phase with Category Tags**: For every phase in `.session/plan/plan.md`, specify:
    *   **Phase Title with Category Tags**: Include appropriate category markers in parentheses for each phase (e.g., `## Phase 1: Initial Setup (config) (structure)` or `## Phase 3: User Authentication (firebase) (auth)`). Common categories might include:
        * `(api)` - API integration work
        * `(ui)` - User interface components
        * `(mock)` - Mock data or services
        * `(db)` - Database operations
        * `(auth)` - Authentication
        * `(test)` - Testing components
        * `(localstorage)` - Local storage implementation
        * `(firebase)` - Firebase specific work
        * `(config)` - Configuration setup
        * `(deploy)` - Deployment tasks
        * Create appropriate custom tags as needed for the specific project
    *   **Files**: List all files to be created or modified. Include their full intended path (e.g., `src/components/NewComponent.js`).
    *   **Purpose/Functionality**: Briefly describe the role of each file or the nature of the changes within it.
    *   **Dependencies**: Note any dependencies between files or phases (e.g., "Phase 2 requires the utility function created in Phase 1").
    *   **Implementation Notes**: Add any crucial details or considerations for the implementation of that phase.
5.  **Execute Phase 1 (from Full Plan)**: Read `.session/plan/plan.md`. Identify the details for **Phase 1**. Perform file creations/modifications based *only* on the details identified for Phase 1. Use the `edit_file` tool.
6.  **Mark Phase 1 Complete (in Full Plan)**: Update `.session/plan/plan.md` to mark Phase 1 as complete (e.g., add `[COMPLETED]` to its title).
7.  **Log Phase 1 Execution**: Follow the procedure in `.session/details/sessionlog_details.md` to log the execution of Phase 1, creating `log<N+1>.md` and updating `summary.md` in `.session/logs/session/`.
8.  **Confirmation**: Inform the user that the full plan is in `.session/plan/plan.md` and Phase 1 was executed.

**Goal**: To establish a full plan and execute the first step, leaving subsequent steps for the `/act` command.

## /act [phase_number?] - *Note: phase_number argument is ignored, acts on the next incomplete phase in plan.md*

**Purpose**: To find and execute the *next* incomplete phase from `.session/plan/plan.md`.

**Execution Steps**:

1.  **Find Next Incomplete Phase**: Read `.session/plan/plan.md`. Scan through the phases sequentially. Identify the *first* phase that is *not* marked as complete (e.g., does not have `[COMPLETED]` in its title). Store its title and details.
2.  **Handle Completion**: If no incomplete phase is found, inform the user the plan is complete and stop.
3.  **Check for UI Work & Apply Advanced Guidelines**: Analyze the details of the *target phase* identified in step 1 (files, purpose, category tags like `(ui)`). If the phase involves UI work, read `.session/details/aesthetic_details.md` and adhere to its principles during execution.
4.  **Execute Phase (from Full Plan)**: Perform file creations/modifications based *only* on the details identified for the target phase in step 1. Use the `edit_file` tool, applying advanced UI guidelines if needed.
5.  **Mark Phase Complete (in Full Plan)**: Update `.session/plan/plan.md` to mark the target phase (identified in step 1) as complete (e.g., add `[COMPLETED]` to its title).
6.  **Log Phase Execution**: Follow the procedure in `.session/details/sessionlog_details.md` to log the execution of the completed phase (identified in step 1), creating `log<N+1>.md` and updating `summary.md` in `.session/logs/session/`.
7.  **Confirmation**: Report back to the user, confirming which phase (use title from step 1) was executed and that its status is updated in `.session/plan/plan.md`. Mention if subsequent phases remain.

**Goal**: To find the next incomplete phase in the main plan, execute it, and update its status in the main plan. 