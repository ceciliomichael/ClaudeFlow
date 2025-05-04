# /recall Details

**Purpose**: To efficiently retrieve and present the project's memory context, maintaining relationships between context elements while minimizing token usage.

**Execution Steps**:

1.  **Check for Memory Directory**: Verify if the `.session/memory/` directory exists.

2.  **Start with Index Retrieval**: 
    * First read `.session/memory/index.md` 
    * This provides the top-level overview and maps to all other memory files
    * If missing, check for individual memory files directly

3.  **Selective Memory Loading**:
    * If the user specified a focus area (e.g., `/recall plan` or `/recall decisions`), prioritize loading only the relevant memory file plus the index. For `/recall plan`, load `plan_status.md`, then optionally the full `.session/plan/plan.md` if needed for detail.
    * If no focus specified, load in this order:
        1. `.session/memory/index.md`
        2. `.session/memory/project_state.md`
        3. `.session/memory/plan_status.md` (contains active plan details)
        4. `.session/memory/context_map.md`
        5. `.session/memory/tasks.md`
        6. `.session/memory/decisions.md`
    * Use `should_read_entire_file=True` with `read_file` for each memory file

4.  **Present Unified Context**: 
    * Display information under clear section headers
    * Format as a cohesive briefing, not disconnected file contents
    * Use `plan_status.md` content to show active phase; mention `plan.md` exists for full details.
    * Emphasize connections between elements using the context map
    * Highlight current focus areas (active tasks, next plan phase)

5.  **Adapt to Missing Components**:
    * If any expected memory file is missing, gracefully note its absence
    * Continue with available context information
    * Suggest using `/memory` to refresh the context if files are missing

6.  **Handle No Memory Case**:
    * If no memory files exist, inform the user that no prior context was found
    * Suggest using `/memory` to establish initial context baseline

**Goal**: To present a cohesive view of the project context that preserves relationships between components while keeping token usage efficient. The recall should provide just enough context for the AI to understand the project state without overwhelming it. 