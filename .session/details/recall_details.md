# /recall Details

**Purpose**: To efficiently retrieve and present the project's memory context, maintaining relationships between context elements while minimizing token usage.

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

3.  **Present Unified Context**:
    * Display the retrieved information under clear section headers based on the loaded files.
    * Format as a cohesive briefing, not disconnected file contents.
    * Use `plan_status.md` content (or equivalent) to show active phase; mention `plan.md` exists for full details if relevant.
    * Emphasize connections between elements using the context map (if loaded).
    * Highlight current focus areas (active tasks, next plan phase) based on loaded context.

4.  **Adapt to Missing Components**:
    * If `index.md` was present but any *other* expected memory file listed within it is missing or fails to load, gracefully note its absence within the unified context presentation.
    * Continue presenting the available context information.
    * Suggest using `/memory` to refresh the context if critical files seem missing or outdated.

5.  **Handle No Memory Case**:
    * If the initial attempt to read `index.md` failed (Step 1), inform the user that no prior context was found.
    * Suggest using `/memory` to establish an initial context baseline.

**Goal**: To present a cohesive view of the project context that preserves relationships between components while keeping token usage efficient. The recall should provide just enough context for the AI to understand the project state without overwhelming it. 