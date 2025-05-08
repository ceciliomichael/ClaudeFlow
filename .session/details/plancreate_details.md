# /plancreate Command - Implementation Instructions

**Purpose**: To generate a detailed, single-phase implementation plan based on the user's description and display it *only* in the chat for review and subsequent execution by the `/create` command. This command does *not* execute the plan or create/modify any files.

When the user invokes the `/plancreate [description]` command, follow these steps precisely:

## Step 1: Analyze Request and Determine Approach

1.  **Parse User Description**: Thoroughly analyze the user's `[description]` to understand the core requirements, scope, and desired outcome of the creation task.
2.  **Identify UI/UX Work**: Determine if the request involves creating or modifying any user interface or user experience elements.
3.  **Aesthetic Guidelines Check (for UI)**: If UI/UX work is identified:
    *   You MUST run `read_file .session/details/aesthetic_details.md` (or recall its content if already loaded).
    *   Internalize relevant aesthetic guidelines. The plan generated MUST account for these guidelines.
4.  **Contextual Analysis for Plan Sophistication**: Before formulating the plan strategy, you MUST attempt to gather existing project context to ensure the plan is sophisticated and relevant. To do this:
    *   First, check if the `.session/memory/` directory exists and contains any files using `list_dir .session/memory/`.
    *   If the directory exists and is not empty, then proceed to read the following files from it using `read_file` for each (if they exist):
        *   `project_state.md`
        *   `current_plan_status.md`
        *   `active_files.md`
        *   `context_map.md`
    *   **Study Existing Files**: Whether or not memory files exist, you MUST study the project's existing files using `list_dir`, `read_file`, and `codebase_search` extensively. This thorough examination of the codebase is CRITICAL for creating a plan that maintains project coherence, follows established patterns, and integrates seamlessly with existing components.
    *   Synthesize information from these files if they were read. This context should be used to inform the plan strategy, avoid redundant suggestions if similar work is already described or underway, and ensure the new plan complements the existing project state.
    *   If the `.session/memory/` directory does not exist or is empty, skip reading memory files and proceed, noting that no prior context was available to inform the plan.
5.  **Determine Request Complexity**: You MUST assess the complexity of the user's request to ensure the single-phase plan is appropriately detailed:
    *   **Low Complexity**: Simple, self-contained tasks (e.g., creating a single utility function, a small component with no external dependencies, minor configuration changes).
    *   **Medium Complexity**: Tasks involving a few interconnected parts or simple integrations (e.g., a new feature with a few functions/components, basic API endpoint setup, integration with an existing simple module).
    *   **High Complexity**: Tasks that, while intended for a single phase here, might touch upon multiple aspects or require careful orchestration of steps (e.g., a more involved component with several internal states, setup of a new data model with basic CRUD operations, initial scaffolding for a new section of an application).
    *   **Base your assessment on**: Scope of the request, number of distinct elements to create/modify, and potential for internal dependencies within the single phase.
    *   **The goal is to ensure the single-phase plan is comprehensive enough for the assessed complexity.** While `/plancreate` is for a single phase, high complexity tasks require more detailed steps *within* that phase.
6.  **Formulate Plan Strategy**: Outline the necessary steps for a single-phase implementation. Consider potential edge cases, dependencies, and required configurations that should be part of the plan. This strategy MUST be informed by the contextual analysis and complexity assessment performed in the previous steps.

## Step 2: Generate and Display Intelligent, Detailed One-Phase Plan in Chat

1.  **Generate Plan**: Create a high-quality, comprehensive, single-phase implementation plan. This plan MUST be:
    *   **Intelligent**: Demonstrate a clear understanding of the problem, propose appropriate technologies/approaches, and provide rationale if decisions are non-obvious.
    *   **Detailed**: Break down the task into specific, actionable steps. Include intended file names, function names, key logic points, configuration settings, etc.
    *   **Thorough**: Address all aspects of the user's request within the single phase.
    *   **Aesthetics-Aware (if UI)**: If UI is involved, the plan steps must reflect the application of guidelines from `.session/details/aesthetic_details.md`.
2.  **Display Plan in Chat**:
    *   Present the *entire plan directly in the chat*.
    *   **Crucially, do NOT create any `plan.md` file or any other project files at this stage.**
    *   Use clear formatting for readability:
        *   **Title**: Brief, descriptive title for the creation task.
        *   **Overview**: Concise summary of what will be created.
        *   **Approach/Rationale**: Brief explanation of the chosen approach.
        *   **Dependencies**: List any required dependencies with installation instructions (e.g., `npm install [package]`, `pip install [package]`). **DO NOT** plan to modify package management files directly.
        *   **Implementation Steps**: A detailed, numbered list of actions.
        *   **UI Considerations (if applicable)**: Explicitly state that aesthetic guidelines from `.session/details/aesthetic_details.md` will be applied during actual implementation by the `/create` command.
        *   **Success Criteria**: Measurable criteria for successful implementation.
3.  **Inform User**: After displaying the plan, notify the user that the plan is ready and can be executed by using the `/create` command.

## Step 3: Await Further Instruction

1.  **Do NOT proceed with implementation.**
2.  The responsibility for initiating execution lies with a subsequent `/create` command.
3.  The generated plan displayed in the chat is considered the "pending plan" for the `/create` command.

## Notes:

*   This command is purely for plan generation and display in chat.
*   No files are created or modified by `/plancreate`.
*   No logging via `/sessionlog` occurs for `/plancreate`.
*   The aesthetic guidelines from `.session/details/aesthetic_details.md` are to be considered during plan generation for UI tasks, ensuring the plan itself is sound for producing high-quality UI. 
*   **Dependency Management**: Plans should NEVER include steps to directly modify package management files (e.g., `package.json`, `requirements.txt`, `Cargo.toml`, etc.). Instead, they should include explicit installation commands for the user to run (e.g., `npm install [package]`, `pip install [package]`) with clear explanations for why each dependency is needed. 