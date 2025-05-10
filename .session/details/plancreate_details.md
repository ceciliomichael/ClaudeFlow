# /plancreate Command - Implementation Instructions

**Purpose**: To generate a detailed, single-phase implementation plan based on the user's description and display it *only* in the chat for review and subsequent execution by the `/create` command. This command does *not* execute the plan or create/modify any files.

When the user invokes the `/plancreate [description]` command, follow these steps precisely:

## Step 1: Analyze Request and Determine Approach

1.  **Parse User Description**: Thoroughly analyze the user's `[description]` to understand the core requirements, scope, and desired outcome of the creation task.
2.  **Identify UI/UX Work**: Determine if the request involves creating or modifying any user interface or user experience elements.
3.  **Aesthetic Guidelines Check (for UI)**: If UI/UX work is identified:
    *   You MUST run `read_file .session/details/aesthetic_details.md` to load the aesthetic guidelines.
    *   Internalize relevant aesthetic guidelines. The plan generated MUST explicitly state and ensure that these guidelines will be **flawlessly and completely applied** during implementation by `/create`. This is a NON-NEGOTIABLE part of the plan.
4.  **Contextual Analysis for Plan Sophistication**: Before formulating the plan strategy, you MUST attempt to gather existing project context to ensure the plan is sophisticated and relevant. To do this:
    *   First, check if the `.session/memory/` directory exists and contains any files using `list_dir .session/memory/`.
    *   If the directory exists and is not empty, then proceed to read the following files from it using `read_file` for each (if they exist):
        *   `project_state.md`
        *   `current_plan_status.md` (or `plan_status.md`)
        *   `active_files.md`
        *   `context_map.md`
    *   **Study Existing Files, Dependencies, and Content**: Whether or not memory files exist, you MUST study the project's existing files (using `list_dir`, `codebase_search`, and especially `read_file`) and currently installed libraries/dependencies extensively. The objective is not only to understand the project but also to **proactively read and internalize the complete content of ALL files deemed necessary for the implementation of the single-phase plan by the `/create` command.** This includes files that will be modified or referenced during the task. This comprehensive upfront file reading is CRITICAL for creating a plan that allows `/create` to proceed directly to implementation with minimal to no further reading of these pre-existing files, maintaining project coherence, following established patterns, integrating seamlessly, and avoiding redundant new dependencies if existing ones suffice.
    *   Synthesize information from these files if they were read. This context should be used to inform the plan strategy, avoid redundant suggestions if similar work is already described or underway, and ensure the new plan complements the existing project state.
    *   If the `.session/memory/` directory does not exist or is empty, skip reading memory files and proceed, noting that no prior context was available to inform the plan.
5.  **Determine Request Complexity**: You MUST assess the complexity of the user's request to ensure the single-phase plan is appropriately detailed:
    *   **Low Complexity**: Simple, self-contained tasks (e.g., creating a single utility function, a small component with no external dependencies, minor configuration changes).
    *   **Medium Complexity**: Tasks involving a few interconnected parts or simple integrations (e.g., a new feature with a few functions/components, basic API endpoint setup, integration with an existing simple module).
    *   **High Complexity**: Tasks that, while intended for a single phase here, might touch upon multiple aspects or require careful orchestration of steps (e.g., a more involved component with several internal states, setup of a new data model with basic CRUD operations, initial scaffolding for a new section of an application).
    *   **Base your assessment on**: Scope of the request, number of distinct elements to create/modify, and potential for internal dependencies within the single phase.
    *   **The goal is to ensure the single-phase plan is comprehensive enough for the assessed complexity.** While `/plancreate` is for a single phase, high complexity tasks require more detailed steps *within* that phase.
6.  **UI Design Considerations in Planning**: If the request involves UI work:
    *   The plan generated MUST clearly state that the UI implementation by the `/create` command needs to adhere to principles in `.session/details/aesthetic_details.md`.
    *   If significant UI design is needed, the plan should include a step for "UI Design" which specifies creating a design (visual strategy, component architecture, interaction patterns, accessibility, performance) based on `.session/details/aesthetic_details.md`, before the implementation step.
    *   The AI generating this plan does not need to read `aesthetic_details.md` itself, but it MUST ensure the plan correctly instructs `/create` to do so for UI implementation.
7.  **Formulate Plan Strategy**: Outline the necessary steps for a single-phase implementation. Consider potential edge cases, dependencies, and required configurations that should be part of the plan. This strategy MUST be informed by the contextual analysis, complexity assessment, and (if applicable) UI design considerations performed in the previous steps.

## Step 2: Generate and Display Intelligent, Detailed One-Phase Plan in Chat

1.  **Generate Plan**: Create a high-quality, comprehensive, single-phase implementation plan. This plan MUST be:
    *   **Intelligent**: Demonstrate a clear understanding of the problem, propose appropriate technologies/approaches, and provide rationale if decisions are non-obvious.
    *   **Detailed**: Break down the task into specific, actionable steps. Include intended file names, function names, key logic points, configuration settings, etc.
    *   **Thorough**: Address all aspects of the user's request within the single phase.
    *   **Design-Driven (if UI)**: If UI is involved, the plan MUST:
        *   Clearly state that implementation by `/create` must adhere to `.session/details/aesthetic_details.md`.
        *   If complex, include a design step referencing the guide before the implementation step.
        *   Specify exact styling approaches, component structures, and interaction patterns that align with the design vision (this may require AI to have some general knowledge of good UI, but not necessarily a full read of the aesthetic guide at this plancreate stage).
2.  **Display Plan in Chat**:
    *   Present the *entire plan directly in the chat*.
    *   Format with a clear title starting with "**PENDING PLAN:**" to make it obvious this is the plan that `/create` will execute.
    *   **Crucially, do NOT create any `plan.md` file or any other project files at this stage.**
    *   Use clear formatting for readability:
        *   **Title**: Brief, descriptive title for the creation task.
        *   **Overview**: Concise summary of what will be created.
        *   **Design Section (if UI)**: The complete design section containing all the detailed design specifications as outlined in Step 1.6.
        *   **Approach/Rationale**: Brief explanation of the chosen approach.
        *   **Dependencies**: If any external dependencies are needed, first explicitly state that a check for existing suitable libraries was performed. If a *new* dependency is still required, list it. The plan should state that these new dependencies will be installed using `run_terminal_cmd` (e.g., `npm install [package]`) when the `/create` command executes the plan. A clear explanation for why each *new* dependency is essential (and why existing libraries are not sufficient) MUST be included.
        *   **Implementation Steps**: A detailed, numbered list of actions.
        *   **UI Considerations (if applicable)**: Explicitly and forcefully state that the UI implementation by the `/create` command **MUST adhere to the principles outlined in `.session/details/aesthetic_details.md`**. Reiterate that this is a non-negotiable requirement for the `/create` step.
        *   **Success Criteria**: Measurable criteria for successful implementation.
3.  **Inform User**: After displaying the plan, explicitly notify the user that:
    *   The plan is ready to be executed with the `/create` command
    *   The plan will remain in chat memory until `/create` is called
    *   They can optionally provide additional implementation context by using `/create [description]`

## Step 3: Await Further Instruction

1.  **Do NOT proceed with implementation.**
2.  The responsibility for initiating execution lies with a subsequent `/create` command.
3.  The generated plan displayed in the chat is considered the "pending plan" for the `/create` command.

## Notes:

*   This command is purely for plan generation and display in chat.
*   No files are created or modified by `/plancreate`.
*   No logging via `/sessionlog` occurs for `/plancreate`.
*   **UI Excellence is MANDATORY for Execution**: For ANY UI-related task, the generated plan MUST clearly state that the `/create` command is responsible for ensuring the aesthetic guidelines from `.session/details/aesthetic_details.md` are **flawlessly and completely applied** during actual implementation. This is a NON-NEGOTIABLE aspect of the execution.
*   **Dependency Management**: Plans should NEVER include steps to directly modify package management files (e.g., `package.json`, `requirements.txt`, `Cargo.toml`, etc.). When planning for new dependencies, **first confirm that no existing library can meet the requirement.** Then, specify that new dependencies will be installed using `run_terminal_cmd` by the `/create` command, along with the appropriate installation command and a strong justification for the new dependency, highlighting why existing options are inadequate.
*   **Plan Persistence**: The plan is ONLY stored in chat history/memory and is NOT written to any file. It will remain accessible to the `/create` command until a new session starts or a new plan is created with `/plancreate`. 