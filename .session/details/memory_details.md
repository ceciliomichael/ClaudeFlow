# /memory Details

**Purpose**: To create compact, interconnected context records for efficient transfer between sessions, minimizing AI context overload.

**Execution Steps**:

1.  **Analyze Session Context**: 
    * Check the session logs folder (`.session/logs/session/`)
    * Read the latest `summary.md` from this folder (summarizes all logs)
    * Check for `.session/plan/plan.md` and `.session/plan/active_plan.md`
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
        * Content of `active_plan.md` (shows next phase or completion status)
        * Overall completion estimate based on `plan.md` marked phases.
    
    * `.session/memory/decisions.md` (≤80 lines):
        * Chronological list of key decisions
        * Only critical architectural/design choices
        * Reasoning (extremely brief)
        * Impact on development direction
    
    * `.session/memory/tasks.md` (≤50 lines):
        * Current pending tasks
        * Organized by priority/category
        * Status indicators
        * Dependencies between tasks

4.  **Create Context Map File**: Create/update `.session/memory/context_map.md` (≤70 lines):
    * Visual ASCII/markdown relationship map
    * Shows connections between project components
    * Indicates dependency flow
    * Maps decision impacts to components

5.  **Optimize for Token Efficiency**:
    * Use bullet points, not paragraphs
    * Prefer tables for structured information
    * Use hierarchy (headings) to organize information
    * Eliminate redundancy across files
    * Use consistent terminology/abbreviations
    * **Critically important**: Never exceed line limits for any file

6.  **Confirmation**: Notify the user that memory has been updated with efficient, interconnected context snapshots.

**Goal**: Create a network of small, focused memory files that together provide complete contextual understanding when loaded, while individually remaining under token limits to avoid context overload. 