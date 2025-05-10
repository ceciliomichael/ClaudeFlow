# /memory Details

**Purpose**: To create compact, interconnected context records for efficient transfer between sessions, minimizing AI context overload.

**Execution Steps**:

1.  **Analyze Session Context**: 
    * Check the session logs folder (`.session/logs/session/`)
    * Read the latest `summary.md` from this folder (summarizes all logs) if it exists
    * Check for `.session/plan/plan.md` using `read_file`
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
        * Overall completion estimate based on `plan.md` marked phases
        * Represent phase completion status using Markdown checkboxes (`[ ]` or `[x]`)
        * List of phases from `plan.md` with status indicated by Markdown checkboxes (`[ ]` or `[x]`)
        * Next incomplete phase highlighted or noted
        * Overall completion estimate (e.g., "3/5 phases complete")
    
    * `.session/memory/decisions.md` (≤80 lines):
        * Chronological list of key decisions
        * Only critical architectural/design choices
        * Reasoning (extremely brief)
        * Impact on development direction
    
    * `.session/memory/tasks.md` (≤50 lines):
        * List of current pending tasks (use bullet points)
        * Organized by priority/category (optional headings)
        * Status indicators
        * Use Markdown checkboxes (`[ ]` for pending, `[x]` for complete) for status
        * Dependencies between tasks (optional, e.g., `depends on #task_id`)
    
    * `.session/memory/active_files.md` (≤50 lines):
        * List of most recently accessed/modified project files
        * Files critical to current implementation phase (e.g., if a plan is active, files relevant to the next incomplete phase)
        * Primary source files that define core functionality being discussed or modified
        * Configuration files necessary for project operation if they are currently relevant
        * Paths should be relative to workspace root
        * Brief description of each file's purpose (single line)
        * Status (stable/being modified/pending implementation)
        * **IMPORTANT**: Since `/recall` will read ALL files in this list, be highly selective about which files to include. Keep the total list to 10-15 files maximum to avoid overwhelming the context window.
        * Priority ranking (high/medium/low) for attention during commands - ALL files will be loaded regardless of priority:
            *   **High Priority Criteria** (3-5 files max):
                * Files currently being edited as part of the active plan phase
                * Files containing core functionality being actively modified
                * Configuration files that control the immediate task at hand
                * Files with critical bugs being addressed
                * Files explicitly mentioned in the next plan phase
            *   **Medium Priority Criteria** (4-6 files max):
                * Files that directly interact with high priority files
                * Files that will be modified in upcoming plan phases
                * Files needed to understand the current implementation context
                * Files that define key interfaces/APIs being used
                * Recently modified files that are stable but important for context
            *   **Low Priority Criteria** (3-4 files max):
                * Utility files needed for broader context
                * Files that provide supporting functionality
                * Stable files that offer important context but aren't being modified
                * Template or example files relevant to current work
                * Documentation files that explain relevant concepts

4.  **Create Context Map File**: Create/update `.session/memory/context_map.md` (≤70 lines):
    * Visual ASCII/markdown relationship map
    * Use **Mermaid diagram** syntax (e.g., `graph TD; A-->B;`) for the relationship map
    * Shows connections between project components (modules, classes, key functions)
    * Indicates dependency flow (e.g., data flow, control flow)
    * Optionally, maps decision impacts to components (using annotations or labels)

5.  **Optimize for Token Efficiency**:
    * Use bullet points, not paragraphs (except within Mermaid labels if needed)
    * Prefer tables for structured information
    * Use hierarchy (headings) to organize information
    * Eliminate redundancy across files
    * Use consistent terminology/abbreviations
    * **Critically important**: Never exceed line limits for any file

6.  **Coordinate with Other Commands**:
    * If a plan exists at `.session/plan/plan.md`, ensure `.session/memory/plan_status.md` accurately reflects its current state
    * Make sure file references in `.session/memory/active_files.md` use the exact same paths as referenced in other commands
    * Align status indicators with checkboxes in plans
    * Maintain consistent naming conventions across all files (e.g., consistently use `plan_status.md` rather than `current_plan_status.md`)

7.  **Confirmation**: Notify the user that memory has been updated with efficient, interconnected context snapshots that will be available to future commands through `/recall`.

**Goal**: Create a network of small, focused memory files that together provide complete contextual understanding when loaded, while individually remaining under token limits to avoid context overload. This network should facilitate efficient context persistence between commands and sessions. 