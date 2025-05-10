# Plan Command Details

This file provides detailed operational instructions for the `/plan` command.

## /plan [description]

**Purpose**: To generate a new implementation plan (`plan.md`), automatically archiving any previously completed plan. The plan is NOT automatically executed.

**Execution Steps**:

0.  **Check for Existing Plan & Archive if Completed**: 
    *   Check if `.session/plan/plan.md` exists using `read_file`.
    *   If it exists:
        *   Read the file content.
        *   Determine if *all* phases within it are marked complete (check for `[x]` checkboxes in all phase headings).
        *   **If all phases ARE complete**: 
            *   First, ensure the `.session/plan/old/` directory exists. Create it if needed.
            *   Determine the current date string: Use PowerShell command `Get-Date -Format 'yyyyMMdd'`.
            *   List existing archived plans for today using `list_dir .session/plan/old/` to find files matching pattern `plan-<date_string>-*.md`.
            *   Calculate the next sequence number (`NNNN`, e.g., 0001, 0002) based on the existing files found. If none exist for today, start with `0001`. Ensure it's zero-padded to 4 digits.
            *   Use PowerShell's `Move-Item -Path ".session\\plan\\plan.md" -Destination ".session\\plan\\old\\plan-$(Get-Date -Format 'yyyyMMdd')-<NNNN>.md"` command to move the file, embedding the date command and inserting the calculated sequence number `<NNNN>`.
            *   Inform the user that the completed plan was archived using the new naming scheme.
        *   **If any phase is NOT complete**: 
            *   Abort the `/plan` command.
            *   Notify the user that an *active, incomplete* plan exists at `.session/plan/plan.md`. Instruct them to either complete it using `/act` or manually archive/delete it before creating a new plan.
            *   Stop execution here.
    *   If `.session/plan/plan.md` does not exist or read_file fails, continue to step 1.

1.  **MANDATORY Pre-Planning Analysis**: Before generating *any* plan, you MUST perform a deep and thorough analysis of the current project state. This is non-negotiable. 
    *   **Study Existing Structure, Dependencies, and Content**: Use `list_dir`, `codebase_search`, and especially `read_file` extensively. The objective is not only to understand the existing project (directory layout, key files, core logic, patterns, installed libraries/dependencies) but also to **proactively read and internalize the complete content of ALL files deemed necessary for the execution of ANY phase of the planned tasks by the `/act` command.** This includes files that will be modified or referenced. This comprehensive upfront file reading during the planning phase is CRITICAL to ensure `/act` can proceed directly to implementation with minimal to no further reading of these pre-existing files. This step MUST be performed thoroughly to ensure compatibility, coherence, maintain project patterns, and avoid redundant dependencies.
    *   **Identify Integration Points**: Determine how the planned work will integrate with existing components, functions, and data structures.
    *   **Leverage Existing Code & Libraries**: Actively look for opportunities to reuse existing functions, modules, configurations, or **currently integrated libraries** to avoid redundancy, maintain consistency, and reduce the introduction of new external dependencies. Prioritize solutions using existing assets.
    *   **Determine Project Complexity**: You MUST assess the complexity of the project to guide the number of phases required:
        *   **Low Complexity** (2-3 phases): Simple features, minimal dependencies, limited scope, few files affected, straightforward integration.
        *   **Medium Complexity** (4-6 phases): Multi-component features, moderate dependencies, specialized knowledge areas, several files affected, requires coordination between parts.
        *   **High Complexity** (7+ phases): System-wide changes, complex state management, multiple knowledge domains, significant architectural impact, many files affected, extensive testing requirements.
        *   **Base your assessment on**: Scope breadth, technical difficulty, integration complexity, domain knowledge required, number of affected components, and testing complexity.
        *   **Each phase must have a clear, distinct purpose**: Do not artificially inflate or reduce the number of phases. The structure should naturally reflect the project's true complexity.
    *   **Consult Context**: 
        *   Review the latest log summary (`.session/logs/session/summary.md`) if it exists.
        *   Attempt to gather context from memory files: 
            *   First, check if the `.session/memory/` directory exists and contains any files using `list_dir .session/memory/`.
            *   If the directory exists and is not empty, then read relevant memory files (e.g., `project_state.md`, `plan_status.md`, `active_files.md`, `context_map.md`) using `read_file` for each (if they exist). 
            *   Alternatively, you can execute the `/recall` command first to load this context if it hasn't already been loaded.
            *   Synthesize information from these files if they were read. 
            *   If the `.session/memory/` directory does not exist or is empty, note that no prior memory context was available.
        *   Ensure the plan aligns with prior decisions and the synthesized context from logs and memory files.
    *   **Failure Consequence**: Skipping or performing a superficial analysis WILL lead to incorrect, inefficient, or conflicting plans that damage the project. Comprehensive upfront analysis is mandatory.

2.  **UI Design Considerations in Planning**: For ANY plan phase that will involve UI work:
    *   The plan MUST identify such phases clearly (e.g., using an `(ui)` tag).
    *   The plan phase description MUST state that the UI implementation during `/act` execution must adhere to the principles in `.session/details/aesthetic_details.md`.
    *   If significant UI design is needed for a phase, the plan for that phase should include a sub-task for "UI Design" which specifies creating a design (visual strategy, component architecture, interaction patterns, accessibility, performance) based on `.session/details/aesthetic_details.md`. This design will then be implemented in a subsequent sub-task of that phase.
    *   The AI generating the plan does not need to read `aesthetic_details.md` itself, but it MUST ensure the plan correctly instructs `/act` to do so for UI phases.

3.  **Create New Plan File**: Create the file `.session/plan/plan.md`. The `edit_file` tool will create the `.session/plan/` directory if needed.

4.  **Structure the New Plan**: Organize the plan based on the user's `[description]` into logical, numbered phases within the new `.session/plan/plan.md`. Each phase represents a distinct step and should start with an incomplete Markdown checkbox (`[ ]`).

5.  **Detail Each Phase with Category Tags**: For every phase in `.session/plan/plan.md`, specify:
    *   **Phase Title with Checkbox and Category Tags**: Start with `[ ]` followed by the title and appropriate category markers (e.g., `## [ ] Phase 1: Initial Setup (config) (structure)`).
    *   **Use These Standard Category Tags**:
        * `(ui)` - User interface work. **CRITICAL: If this tag is present, the plan phase MUST clearly state that implementation via `/act` requires adherence to `.session/details/aesthetic_details.md`. If complex, a dedicated design sub-task referencing the guide should precede implementation sub-tasks within this phase.**
        * `(data)` - Data structures or database
        * `(api)` - API endpoints or interfaces
        * `(logic)` - Core business logic
        * `(auth)` - Authentication or authorization
        * `(testing)` - Test implementation
        * `(config)` - Configuration work
        * `(structure)` - Project structure changes
        * `(perf)` - Performance optimization
        * Add others as appropriate
    *   **Files**: List all files to be created or modified.
    *   **Purpose/Functionality**: Briefly describe the role of each file or the nature of the changes.
    *   **Dependencies**: Note any dependencies between files or phases. **Crucially, identify and leverage existing codebase dependencies and installed libraries where possible before introducing new ones. Thoroughly check if an existing library can fulfill the requirement.**
        *   **Dependency Management**: Only when new external dependencies are deemed essential after checking existing resources:
            *   Identify the appropriate package installation command (e.g., `npm install [package]`, `pip install [package]`, `cargo add [package]`).
            *   The plan should state that these dependencies will be installed using `run_terminal_cmd` when the phase is executed by `/act`.
            *   A clear explanation for why each *new* dependency is needed (especially if similar functionalities might exist in current libraries) MUST be included in the plan and provided in chat before running the command during `/act`.
    *   **Implementation Notes**: Add crucial details.
    *   **UI Category**: If the phase involves any UI work:
        *   Explicitly tag it with a `(ui)` category marker.
        *   Ensure the phase description clearly mandates that `/act` must apply `.session/details/aesthetic_details.md` during implementation.
        *   If complex, include sub-tasks for "UI Design (following aesthetic_details.md)" and then "UI Implementation".

6.  **Confirmation**: Inform the user that the new plan has been created in `.session/plan/plan.md`. Advise them to use `/act` to begin executing the first phase of the plan. **No session log is created, and no memory files are updated by the `/plan` command itself.**

**Goal**: To establish a new plan with world-class UI design principles and ensure any previously completed plan is archived. Execution of the plan phases and subsequent logging/memory updates are handled by the `/act` and `/memory` commands respectively. 