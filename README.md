> **Note**: ClaudeFlow is designed for use within AI-integrated IDEs (like Cursor). While the principles and structure can be adapted for other AI models, the system, commands, and workflows were primarily developed and tested with **Claude 3.7**, which is recommended for the optimal experience. See the `vibe-coding-guide.md` for tips on leveraging this system for structured AI-driven development (vibe coding).

# ClaudeFlow System

A sophisticated project management and execution system built around Claude AI, designed for seamless context retention, efficient planning, and methodical implementation of complex software projects.

## Overview

ClaudeFlow enhances Claude's capabilities with structured workflows, persistent memory, and methodical execution. Using custom commands and a well-defined file structure within the `.session` directory, it maintains project state, tracks progress, and ensures continuity between development sessions.

## Custom Commands

ClaudeFlow utilizes slash commands to trigger specific actions:

| Command | Description |
|---------|-------------|
| `/plan [description]` | Analyzes the request, creates a multi-phase plan (`.session/plan/plan.md`), archives the old one, and immediately executes Phase 1. |
| `/act` | Finds the next incomplete phase (`[ ]`) in `plan.md`, executes it, and marks it as complete (`[x]`). |
| `/sessionlog [optional: bug fix note]` | Logs recent actions to `.session/logs/session/`. If "bug fix" is mentioned, appends to the latest log; otherwise, creates a new log and updates `summary.md`. |
| `/memory` | Captures the current project state into interconnected memory files within `.session/memory/` (uses Markdown checkboxes for task/plan status). |
| `/recall [focus?]` | Loads and presents the saved context from memory, optionally focused on a specific area (e.g., `/recall plan`). |

## Directory Structure

The system relies on the following structure within the `.session` directory:

```
.session/
├── details/              # Detailed instructions for *how* commands execute
│   ├── plan_act_details.md     # Logic for /plan and /act (includes archiving)
│   ├── sessionlog_details.md # Logic for /sessionlog
│   ├── memory_details.md       # Logic for /memory (uses checkboxes, Mermaid maps)
│   ├── recall_details.md       # Logic for /recall
│   └── aesthetic_details.md    # Guidelines for advanced UI generation
├── logs/
│   └── session/          # Contains logs for the current session
│       ├── log1.md       # Individual, sequential log entries
│       ├── log2.md
│       └── summary.md    # Aggregated summary of all logs in this session
├── memory/               # Stores context snapshots for persistence
│   ├── index.md          # Overview and links to other memory files
│   ├── project_state.md  # High-level project status, structure, tech
│   ├── plan_status.md    # Current plan phases (with [ ]/[x] status)
│   ├── decisions.md      # Log of key architectural/design decisions
│   ├── tasks.md          # List of pending tasks (with [ ]/[x] status)
│   └── context_map.md    # Visual Mermaid diagram of component relationships
└── plan/                 # Manages the implementation plan
    └── plan.md           # The complete, multi-phase implementation plan
    ├── plan.md           # The *active* multi-phase implementation plan (using [ ]/[x])
    └── old/              # Archive for completed plans (e.g., plan-YYYYMMDDHHMMSS.md)
```

## Core Systems

### Planning System (`.session/plan/`)
Provides a structured approach for complex projects:
*   `/plan` creates `plan.md` (full plan) and executes Phase 1 directly from its details.
*   `/plan` creates a new `plan.md` (using Markdown checkboxes `[ ]`/`[x]` for phases) and executes Phase 1. If a fully completed `plan.md` already exists, it's automatically archived to `.session/plan/old/` using a timestamped filename before the new plan is created.
*   `/act` finds the next incomplete phase in `plan.md`, executes it based on its details, and updates its status in `plan.md`.
*   `/act` finds the next incomplete phase (`[ ]`) in `plan.md`, executes it, and updates its checkbox status (`[x]`).
*   Phases in `plan.md` include category tags (e.g., `(ui)`, `(api)`) for clarity.

### Logging System (`.session/logs/session/`)
Maintains a chronological record:
*   `/sessionlog`, `/plan`, and `/act` create sequential `log<N>.md` files.
*   Bug fixes *append* to the latest log file instead of creating a new one.
*   `summary.md` is updated with every new log entry, providing a concise overview of the session.

### Memory System (`.session/memory/`)
Enables efficient context transfer between sessions:
*   `/memory` captures the current state into multiple, focused files (project state, plan status, decisions, tasks, context map) with strict line limits for efficiency.
*   `/memory` captures the current state. `plan_status.md` and `tasks.md` use Markdown checkboxes (`[ ]`/`[x]`) for status. `context_map.md` uses Mermaid syntax.
*   `/recall` loads this structured context, presenting a cohesive briefing. `/recall [focus]` loads specific areas.

## Core Configuration and Guidelines

### `rules.md` (Primary Configuration)
This file acts as the main instruction set for the AI, defining:
*   The AI's persona and objectives.
*   Available tools and constraints (including limited exceptions for specific PowerShell commands for plan archiving).
*   The list and basic descriptions of custom commands.
*   Fundamental operational rules (file storage, command execution, detail reading).
*   Consequences for rule violations.
*   **Intended Use**: Can serve as a `.cursorrules` file or similar mechanism.

### Detail Files (`.session/details/`)
These files provide the mandatory, step-by-step execution logic for each custom command (`plan_act_details.md`, `sessionlog_details.md`, etc.). The AI *must* read the relevant detail file before executing any custom command.

### Aesthetic Guidelines (`.session/details/aesthetic_details.md`)
Contains mandatory guidelines for generating **advanced, aesthetically superior UI**. This file *must* be consulted by the AI (specifically during the `/act` command execution) whenever a plan phase involves UI creation or modification. The goal is modern, engaging, performant, and accessible interfaces, utilizing techniques like glassmorphism, advanced typography, microinteractions, and mobile-first responsive design.

## Recommended Workflow

Based on practical testing, the following workflow provides the most reliable results:

1.  **Initial Plan Creation**: Start with `/plan [description]`. This creates the plan (archiving any previously completed one) and executes the first phase (`[x] Phase 1...`).
2.  **Execute Subsequent Phases**: Use `/act` repeatedly to execute the next available phase (`[ ] Phase N...`) until all phases are marked (`[x]`).
3.  **Context Management**:
    *   **Between Sessions**: Use `/memory`, start a new chat session, use `/recall`, then continue with `/act` or `/plan`.
    *   **Within a Session**: Continue using `/act`, `/memory`, `/recall` as needed.
4.  **Bug Fixing**: Use `/sessionlog` to append fix details to the latest log.
5.  **Starting a New Plan**: Simply use `/plan [new description]`. The system will automatically archive the current `plan.md` if it's fully completed (`[x]` on all phases) before creating the new one.

## Extending ClaudeFlow: Adding Custom Commands

Follow these steps to add new commands:
1.  **Define**: Choose a name (`/command`), define purpose.
2.  **Create Detail File**: Write precise execution steps in `.session/details/command_details.md`.
3.  **Update `rules.md`**: Add the command to the list, link to the detail file, update general rules/consequences if needed.
4.  **Consider Integration**: Does it need logging? Affect memory? Interact with plans?
5.  **Update `README.md`**: Add command to the table, update workflow if needed.

## Best Practices

- Plan with modular phases and clear category tags.
- Log significant changes/decisions.
- Use `/memory` and `/recall` diligently between sessions.
- Review the session `summary.md` periodically.

## System Rules (Summary)
- Commands start with `/`.
- Commands are matched exactly (case-sensitive).
- Files stored only within `.session` subdirectories.
- Files created *only* upon explicit command invocation.
- Detail files *must* be read directly (`read_file`) before command execution (no `list_dir` checks).
- State maintained via memory system. 

---
*Note: The ClaudeFlow system workflow and commands were developed and tested primarily using Claude 3.7 within the Cursor IDE environment.* 