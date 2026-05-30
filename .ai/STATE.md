# LLM Active State (The Working Memory)

This file tracks the *exact* current micro-task and state of the project.
When a new model session begins, it MUST read this file to synchronize context.

## Session Metadata
- **Last Agent ID / Model:** [Which model last touched this file, e.g., GPT-4o, Claude 3.5 Sonnet, Gemini 1.5 Pro]
- **Current Git Branch:** [Name of the active branch, e.g., feature/onboarding-flow]

## Current Micro-Goal
- [Brief description of the current feature, bug, or refactoring task]

## Current Status
- **Status:** [IN_PROGRESS | COMPLETED | BLOCKED]
- **Step:** [What exact step are we on right now? e.g., "fixing type errors in user_service"]

## Last Known State / Blockers
- [YYYY-MM-DD] [Record of the last successful action or current blocking error/traceback]
- **Blocker:** [None | Description of the blocker preventing progress]

## Attempted Solutions (Do Not Repeat)
- [List of failed approaches, exact commands, or code structures already tried in this session to prevent the next LLM from repeating mistakes]

## Rejected Approaches
If an approach fails twice, log it in the table below. For permanent architectural rejections that apply beyond the current task, migrate the entry to `wiki/rejected-approaches.md`.

| Date       | Approach Tried                  | Why It Failed                        | Permanent? |
|------------|---------------------------------|--------------------------------------|------------|
| YYYY-MM-DD | [Description of the approach]   | [Exact error or reason for failure]  | Yes/No     |

## Next Steps for Hand-off
- [Explicit instructions left for the next LLM taking over if this session drops or hits a token limit. E.g., "I finished step 2, but the tests are failing. Next agent, please look at the User model mapping."]
