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

1.  **MANDATORY Pre-Planning Analysis**: Before generating *any* plan, you MUST perform a deep and thorough analysis of the current project state. This is non-negotiable. 
    *   **Study Existing Structure**: Use `list_dir`, `read_file`, and `codebase_search` extensively to fully understand the existing directory layout, key files, core logic, and established patterns. **This is the most critical step - extensive study of existing files MUST precede any plan creation to ensure compatibility, coherence and maintain project patterns.**
    *   **Identify Integration Points**: Determine how the planned work will integrate with existing components, functions, and data structures.
    *   **Leverage Existing Code**: Actively look for opportunities to reuse existing functions, modules, or configurations to avoid redundancy and maintain consistency.
    *   **Determine Project Complexity**: You MUST assess the complexity of the project to guide the number of phases required:
        *   **Low Complexity** (2-3 phases): Simple features, minimal dependencies, limited scope, few files affected, straightforward integration.
        *   **Medium Complexity** (4-6 phases): Multi-component features, moderate dependencies, specialized knowledge areas, several files affected, requires coordination between parts.
        *   **High Complexity** (7+ phases): System-wide changes, complex state management, multiple knowledge domains, significant architectural impact, many files affected, extensive testing requirements.
        *   **Base your assessment on**: Scope breadth, technical difficulty, integration complexity, domain knowledge required, number of affected components, and testing complexity.
        *   **Each phase must have a clear, distinct purpose**: Do not artificially inflate or reduce the number of phases. The structure should naturally reflect the project's true complexity.
    *   **Consult Context**: 
        *   Review the latest log summary (`.session/logs/session/summary.md`).
        *   Attempt to gather context from memory files: 
            *   First, check if the `.session/memory/` directory exists and contains any files using `list_dir .session/memory/`.
            *   If the directory exists and is not empty, then read relevant memory files (e.g., `project_state.md`, `current_plan_status.md`, `active_files.md`, `context_map.md`) using `read_file` for each (if they exist). This can also be done via `/recall` if more structured retrieval is needed and its loaded context is preserved as per its rules.
            *   Synthesize information from these files if they were read. 
            *   If the `.session/memory/` directory does not exist or is empty, note that no prior memory context was available.
        *   Ensure the plan aligns with prior decisions and the synthesized context from logs and memory files.
    *   **Failure Consequence**: Skipping or performing a superficial analysis WILL lead to incorrect, inefficient, or conflicting plans that damage the project. Comprehensive upfront analysis is mandatory.

2.  **Create New Plan File**: Create the file `.session/plan/plan.md`. The `edit_file` tool will create the `.session/plan/` directory if needed.
3.  **Structure the New Plan**: Organize the plan based on the user's `[description]` into logical, numbered phases within the new `.session/plan/plan.md`. Each phase represents a distinct step and should start with an incomplete Markdown checkbox (`[ ]`).
4.  **Detail Each Phase with Category Tags**: For every phase in `.session/plan/plan.md`, specify:
    *   **Phase Title with Checkbox and Category Tags**: Start with `[ ]` followed by the title and appropriate category markers (e.g., `## [ ] Phase 1: Initial Setup (config) (structure)`).
    *   Files: List all files to be created or modified.
    *   Purpose/Functionality: Briefly describe the role of each file or the nature of the changes.
    *   Dependencies: Note any dependencies between files or phases. **Crucially, identify and leverage existing codebase dependencies where possible before introducing new ones.**
        *   **Dependency Management**: When new external dependencies are needed, DO NOT plan to directly modify package management files (e.g., `package.json`, `requirements.txt`). Instead, include explicit installation commands at the start of the relevant phase (e.g., `npm install [package]`, `pip install [package]`) with clear explanations for why each dependency is needed.
    *   Implementation Notes: Add crucial details.
    *   **UI Category**: If the phase involves any UI work, explicitly tag it with a `(ui)` category marker.
5.  **Check Phase 1 for UI Work & Load Guidelines**: Read the newly created `.session/plan/plan.md`. Analyze Phase 1 to determine if it involves UI work (check for `(ui)` category tag or UI-related content). **If UI work is identified, you MUST read `.session/details/aesthetic_details.md` and internalize its guidelines.**
6.  **Execute Phase 1 (from New Plan)**: Identify the details for **Phase 1**. Perform file creations/modifications based *only* on the details for Phase 1. Use `edit_file`. **If UI work was identified in step 5, you MUST apply the aesthetic guidelines from `.session/details/aesthetic_details.md` during execution.**
7.  **Mark Phase 1 Complete (in New Plan)**: Update `.session/plan/plan.md` to mark Phase 1 as complete by changing its checkbox from `[ ]` to `[x]`.
8.  **Log Phase 1 Execution**: Follow the procedure in `.session/details/sessionlog_details.md` to log the execution of Phase 1.
9.  **Confirmation**: Inform the user that the new plan is in `.session/plan/plan.md` and Phase 1 (`[x] ...`) was executed.

**Goal**: To establish a new plan, execute its first step, and ensure any previously completed plan is archived.

## /act

**Purpose**: To find and execute the *next* incomplete phase (`[ ]`) from `.session/plan/plan.md`.

**Execution Steps**:

1.  **Find Next Incomplete Phase**: Read `.session/plan/plan.md`. Scan through the phases sequentially. Identify the *first* phase that starts with an incomplete Markdown checkbox (`[ ]`). Store its title and details.
2.  **Handle Completion**: If no phase starting with `[ ]` is found, inform the user the plan is complete and stop. Recommend running `/plan` again if they wish to archive this completed plan and start a new one.
3.  **Check for UI Work & Load Guidelines**: Analyze the details of the *target phase* identified in step 1. Determine if it involves UI work (check category tags like `(ui)` or UI-related content). **If UI work is identified, you MUST read `.session/details/aesthetic_details.md` and internalize its guidelines.**
4.  **Execute Phase (from Full Plan)**: Perform file creations/modifications based *only* on the details for the target phase. Use `edit_file`. **If UI work was identified in step 3, you MUST apply the aesthetic guidelines from `.session/details/aesthetic_details.md` during execution.** Ensure changes leverage existing functions, variables, and modules whenever practical to maintain consistency and reduce redundancy.
5.  **Dependency Management**: 
    *   **DO NOT directly modify** package management files (e.g., `package.json`, `requirements.txt`, `Cargo.toml`, etc.).
    *   Instead, when new dependencies are needed, output instructions in the chat like `npm install [package]`, `pip install [package]`, etc.
    *   Be specific about versions when necessary: `npm install [package]@[version]`.
    *   Group related dependencies in a single command when possible: `npm install [package1] [package2] [package3]`.
    *   Provide clear explanations for why each dependency is needed.
6.  **Mark Phase Complete (in Full Plan)**: Update `.session/plan/plan.md` to mark the target phase (identified in step 1) as complete by changing its checkbox from `[ ]` to `[x]`.
7.  **Log Phase Execution**: Follow the procedure in `.session/details/sessionlog_details.md` to log the execution of the completed phase.
8.  **Confirmation**: Report back to the user, confirming which phase (`[x] ...`) was executed and that its status is updated in `.session/plan/plan.md`. Mention if subsequent phases remain.

**Goal**: To find the next incomplete phase (`[ ]`) in the main plan, execute it, and update its status (`[x]`) in the main plan. 