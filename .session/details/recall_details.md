# /recall Details

**Purpose**: To efficiently retrieve and present the project's memory context and relevant project files, maintaining relationships between context elements while minimizing token usage and ensuring persistence of context for subsequent commands.

**Execution Steps**:

1.  **Start with Index Retrieval**:
    * Attempt to read `.session/memory/index.md` using `read_file`.
    * This provides the top-level overview and maps to other memory files.
    * If `index.md` is missing or reading fails (indicating no memory context exists), proceed directly to Step 5 (Handle No Memory Case).

2.  **Selective Memory Loading Based on Focus Parameter**:
    * If the user specified a focus area using the optional `[focus]` parameter:
        * `/recall plan`: Load only `index.md` and `plan_status.md`
        * `/recall project`: Load only `index.md` and `project_state.md`
        * `/recall decisions`: Load only `index.md` and `decisions.md`
        * `/recall tasks`: Load only `index.md` and `tasks.md`
        * `/recall files`: Load only `index.md` and `active_files.md`
        * `/recall map`: Load only `index.md` and `context_map.md`
    * If no focus specified, load the primary memory files listed in `index.md` in this order:
        1. `project_state.md` 
        2. `plan_status.md`
        3. `active_files.md`
        4. `tasks.md`
        5. `decisions.md`
        6. `context_map.md`
    * Use `should_read_entire_file=True` with `read_file` for each memory file to be loaded. Keep track of which memory files were successfully loaded.

3.  **MANDATORY: Load Critical Project Files & Active Files Content**:
    * This step is CRITICAL to avoid redundant file reading in subsequent commands like `/act`.
    * Based on the retrieved context from successfully loaded memory files:
    * **Plan File**: If `plan_status.md` (or `index.md` indicating plan context) was loaded and references `.session/plan/plan.md`, then you MUST use `read_file` (with `should_read_entire_file=True`) to load the content of `.session/plan/plan.md` if it exists.
    * **Active Files Content**: If `active_files.md` was successfully loaded (either via focus or general loading):
        * Parse `active_files.md` to identify the list of file paths and their priorities.
        * You MUST read ALL files listed in `active_files.md` regardless of priority using `read_file` (with `should_read_entire_file=True`).
        * There is NO limit to how many active files should be loaded - ALL files in the active files list must be read to ensure commands like `/act` do not need to re-read them.
        * Store the content of these successfully read files in memory.
    * **Configuration Files**: If `project_state.md` (or other memory files) identify specific critical configuration files, you MUST use `read_file` to load their content.
    * All file content loaded in this step (plan, active files, configurations) MUST be explicitly stored in memory to be available for subsequent commands without re-reading.

4.  **Present Unified Context**:
    * Display the retrieved information under clear section headers based on the loaded memory files and project files.
    * Format as a cohesive briefing, not disconnected file contents.
    * If using a focus parameter, emphasize that aspect of the context in your presentation.
    * Use `plan_status.md` content (if loaded) to show active phase; mention if `.session/plan/plan.md` content was also loaded.
    * If `active_files.md` content was loaded, summarize which files were read and briefly mention their status if available in `active_files.md`.
    * Emphasize connections between elements using the context map (if loaded).
    * Highlight current focus areas (active tasks, next plan phase) based on loaded context.
    * Explicitly state which specific memory files and project files (including those from `active_files.md`) were loaded to establish context.

5.  **MANDATORY: Persist All Loaded Content for Subsequent Commands**:
    * This is a CRITICAL step to prevent redundant file reading by subsequent commands.
    * Store ALL loaded file content (both memory files and ALL project/active files) in working memory.
    * Commands like `/act`, `/create`, or other action commands MUST utilize this already-loaded content instead of re-reading the same files.
    * The complete content of ALL files from `active_files.md` MUST be stored and available to subsequent commands.
    * If a file's content changes during a session, it should be re-read only when modified.
    * This persistence MUST last for the duration of the current chat session.
    * Maintain an internal context map linking file paths to their loaded content for quick access.
    * For implementation: Store each file's content with its path as the key to enable rapid retrieval by other commands.

6.  **Adapt to Missing Components**:
    * If `index.md` was present but any *other* expected memory file listed within it is missing or fails to load, gracefully note its absence within the unified context presentation.
    * If a file listed in `active_files.md` cannot be read, note this as well.
    * Continue presenting the available context information.
    * Suggest using `/memory` to refresh the context if critical files seem missing or outdated.

7.  **Handle No Memory Case**:
    * If the initial attempt to read `index.md` failed (Step 1), inform the user that no prior context was found.
    * Suggest using `/memory` to establish an initial context baseline.
    * Offer to explore the current project state through directory listings and file exploration.

**Goal**: To present a cohesive view of the project context that persists between commands, including the content of ALL key project files identified in memory (especially ALL files in `active_files.md`), preserving relationships between components while keeping token usage efficient. The `/recall` command MUST load sufficient context so that subsequent commands like `/act` can execute without redundant file reading. 