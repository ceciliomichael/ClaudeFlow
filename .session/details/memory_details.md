# /memory Details

**Purpose**: To create compact, interconnected context records for efficient transfer between sessions, minimizing AI context overload.

**Execution Steps**:

1.  **Analyze Session Context**: 
    * Check the session logs folder (`.session/logs/session/`)
    * Read the latest `summary.md` from this folder (summarizes all logs)
    * Check for `.session/plan/plan.md`
    * Review recent interactions for key insights

2.  **Create/Update Memory Index**: Create or update `.session/memory/index.md` with:
    * Current session number
    * Session status (active/completed)
    * Top-level summary (1-2 sentences max)
    * Links to all other memory files with single-line descriptions
    * **Limit**: Keep under 30 lines total

3.  **Generate Context Snapshots**: Create or update these focused memory files:
    * `.session/memory/project_state.md` (≤70 lines):
        * Current project structure (key directories/files only)
        * Implementation status (% complete estimate)
        * Core technologies/frameworks in use
        * Architecture overview (bullet points only)
    
    * `.session/memory/plan_status.md` (≤50 lines):
        * Reference to full plan: `.session/plan/plan.md` (if exists)
        * Next incomplete phase from `plan.md` (or completion status)
        * Overall completion estimate based on `plan.md` marked phases.
        * Represent phase completion status using Markdown checkboxes (`[ ]` or `[x]`).
        * List of phases from `plan.md` with status indicated by Markdown checkboxes (`[ ]` or `[x]`).
        * Next incomplete phase highlighted or noted.
        * Overall completion estimate (e.g., "3/5 phases complete").
    
    * `.session/memory/decisions.md` (≤80 lines):
        * Chronological list of key decisions
        * Only critical architectural/design choices
        * Reasoning (extremely brief)
        * Impact on development direction
    
    * `.session/memory/tasks.md` (≤50 lines):
        * List of current pending tasks (use bullet points).
        * Organized by priority/category (optional headings).
        * Status indicators
        * Use Markdown checkboxes (`[ ]` for pending, `[x]` for complete) for status.
        * Dependencies between tasks (optional, e.g., `depends on #task_id`).
    
    * `.session/memory/active_files.md` (≤50 lines):
        * List of most recently accessed/modified project files
        * Files critical to current implementation phase
        * Primary source files that define core functionality
        * Configuration files necessary for project operation
        * Paths should be relative to workspace root
        * Brief description of each file's purpose (single line)
        * Status (stable/being modified/pending implementation)
        * Priority ranking (high/medium/low) for loading during recall

4.  **Create Context Map File**: Create/update `.session/memory/context_map.md` (≤70 lines):
    * Visual ASCII/markdown relationship map
    * Use **Mermaid diagram** syntax (e.g., `graph TD; A-->B;`) for the relationship map.
    * Shows connections between project components (modules, classes, key functions).
    * Indicates dependency flow (e.g., data flow, control flow).
    * Optionally, maps decision impacts to components (using annotations or labels).

5.  **Optimize for Token Efficiency**:
    * Use bullet points, not paragraphs (except within Mermaid labels if needed).
    * Prefer tables for structured information
    * Use hierarchy (headings) to organize information
    * Eliminate redundancy across files
    * Use consistent terminology/abbreviations
    * **Critically important**: Never exceed line limits for any file

6.  **Confirmation**: Notify the user that memory has been updated with efficient, interconnected context snapshots.

**Goal**: Create a network of small, focused memory files that together provide complete contextual understanding when loaded, while individually remaining under token limits to avoid context overload. This network should facilitate efficient context persistence between commands and sessions. 