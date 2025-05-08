# /recall Details

**Purpose**: To efficiently retrieve and present the project's memory context and relevant project files, maintaining relationships between context elements while minimizing token usage and ensuring persistence of context for subsequent commands.

**Execution Steps**:

1.  **Start with Index Retrieval**:
    * Attempt to read `.session/memory/index.md` using `read_file`.
    * This provides the top-level overview and maps to other memory files.
    * If `index.md` is missing or reading fails (indicating no memory context exists), proceed directly to Step 5 (Handle No Memory Case).

2.  **Selective Memory Loading**:
    * Based on the content of `index.md` (if successfully read) and any user-specified focus:
    * If the user specified a focus area (e.g., `/recall plan` or `/recall decisions`), prioritize loading only the relevant memory file mentioned in `index.md`, plus the index itself. For `/recall plan`, load `plan_status.md` (or the file indicated by the index for plan status).
    * If no focus specified, load the primary memory files listed in `index.md` in the recommended order (e.g., project state, plan status, context map, tasks, decisions). Attempt to read each file listed.
    * Use `should_read_entire_file=True` with `read_file` for each memory file to be loaded.

3.  **Load Critical Project Files**:
    * Based on the retrieved context, identify and load critical project files that would be needed by subsequent commands:
    * If plan information is present in memory, load `.session/plan/plan.md` if it exists.
    * Load any key project source files referenced in `project_state.md` (up to 5 most important files).
    * If project uses specific configuration files identified in memory, load those as well.
    * Keep track of all loaded files in a virtual state to avoid redundant reads during subsequent commands.
    * Parse important information about project structure, current implementation state, and planned actions.
    * If an `active_files.md` file exists in the memory directory, use it as a guide for which files to load.

4.  **Present Unified Context**:
    * Display the retrieved information under clear section headers based on the loaded files.
    * Format as a cohesive briefing, not disconnected file contents.
    * Use `plan_status.md` content (or equivalent) to show active phase; mention `plan.md` exists for full details if relevant.
    * Emphasize connections between elements using the context map (if loaded).
    * Highlight current focus areas (active tasks, next plan phase) based on loaded context.
    * Clearly communicate that context has been established for subsequent commands.

5.  **Persist Context for Subsequent Commands**:
    * Store all loaded file content in working memory to avoid redundant reads during subsequent commands.
    * When `/act`, `/create`, or other action commands are subsequently invoked, utilize the already-loaded content.
    * Prioritize previously loaded files over fresh reads when information is needed.
    * Maintain an internal context map linking file paths to their loaded content for quick access.

6.  **Adapt to Missing Components**:
    * If `index.md` was present but any *other* expected memory file listed within it is missing or fails to load, gracefully note its absence within the unified context presentation.
    * Continue presenting the available context information.
    * Suggest using `/memory` to refresh the context if critical files seem missing or outdated.

7.  **Handle No Memory Case**:
    * If the initial attempt to read `index.md` failed (Step 1), inform the user that no prior context was found.
    * Suggest using `/memory` to establish an initial context baseline.

**Goal**: To present a cohesive view of the project context that persists between commands, preserving relationships between components while keeping token usage efficient. The recall should provide enough context for subsequent commands to execute without redundant file reading. 