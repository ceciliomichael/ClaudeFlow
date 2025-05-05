# /create Command - Implementation Instructions

When the user invokes the `/create [description]` command, follow these steps precisely:

## Step 1: Analyze Request and Determine Approach

1. Parse the user's description thoroughly to achieve a deep understanding of what needs to be created, including explicit and implicit requirements.
2. Check if the request involves UI generation. If it does, you MUST:
   - Run `read_file .session/details/aesthetic_details.md`
   - Internalize and prepare to apply *all* relevant aesthetic guidelines.
3. Determine the necessary steps for implementation, considering potential edge cases, dependencies, and required configurations.

## Step 2: Generate and Display Intelligent, Detailed One-Phase Plan

1. **Generate a high-quality, comprehensive implementation plan.** This plan MUST be:
   - **Intelligent**: Demonstrate understanding of the problem, choose appropriate technologies/approaches, and provide clear rationale for decisions.
   - **Detailed**: Break down the task into specific, actionable steps. Include file names, function names, key logic points, configuration settings, and necessary commands (excluding forbidden `run_terminal_cmd` usage).
   - **Thorough**: Address all aspects of the user's request. Consider potential challenges and how they will be handled.
2. This plan should include all necessary steps for completion in a single, cohesive phase.
3. **Display the entire plan directly in the chat (NOT as a file).**
4. Format the plan clearly and logically for easy understanding:
   - **Title**: Brief, descriptive title for the creation task.
   - **Overview**: Concise summary of what will be created and the chosen approach.
   - **Rationale**: Brief explanation of *why* this approach/structure was chosen (if non-obvious).
   - **Implementation Steps**: A detailed, numbered list of specific actions (e.g., "Create file `src/types/models.ts` with interface definitions for domain models", "Add type guards to `src/types/guards.ts` for runtime type checking", "Define API response types in `src/types/api.ts` to match backend schema")
   - **UI Considerations (If Applicable)**: Explicitly state how `aesthetic_details.md` guidelines will be applied.
   - **Success Criteria**: Clear, measurable criteria to verify successful implementation.

## Step 3: Execute Implementation Immediately

1. Begin implementing the plan immediately after displaying it
2. Use the appropriate tools to:
   - Create new files where needed
   - Modify existing files if required
   - Install dependencies if necessary
3. Execute each implementation step in order, without waiting for additional user confirmation
4. If UI elements are involved, ensure they follow the aesthetic guidelines precisely

## Step 4: Log the Creation Process

1. Create a new session log using the `/sessionlog` procedure
2. Document what was created, implementation details, and any decisions made
3. Update the session summary file to reflect this creation activity

## Step 5: Provide Completion Confirmation

1. After implementation is complete, provide a concise confirmation
2. Include:
   - What was created
   - How to use the created elements (if applicable)
   - Any next steps the user might want to consider

## Notes on UI Generation

When the request involves UI elements of any kind (even minor components):
1. The aesthetic guidelines in `.session/details/aesthetic_details.md` MUST be followed
2. UI should be sophisticated, polished, and visually engaging
3. Focus on creating UIs that are:
   - Visually advanced (beyond basic functionality)
   - Highly interactive with appropriate microinteractions
   - Optimized for performance
   - Fully accessible
   - Responsive across devices
   - Aligned with modern design principles

Remember: The `/create` command is designed for direct, immediate implementation without the planning phases of `/plan` and `/act`. Execution should be efficient, comprehensive, and follow all aesthetic guidelines when UI is involved. 