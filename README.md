# Project Handoff Skill

A reusable continuity and handoff skill for long AI-assisted software projects.

Its purpose is to keep a project moving in the same direction across many chat/context windows while preserving the latest verified state, avoiding repeated mistakes, and handing the next agent an exact resume point.

## What v2 preserves

- Project background and original problem
- Ultimate goal and current MVP goal
- Full project plan
- Latest verified progress
- Exact mid-step resume point
- Important decisions
- Confirmed pitfalls, high-risk operations, and useful failed attempts
- Short chronological session reviews in one file
- User communication and decision-boundary rules
- A ready-to-use next-session prompt
- Final reusable methodology when the project is complete

## Main file

`SKILL.md`

## Continuity files created inside a project

The `assets/` folder contains templates for a `.continuity/` folder:

- `PROJECT_ORIGIN.md` — background, ultimate goal, MVP goal, locked original direction
- `MASTER_PLAN.json` — stable project roadmap and verified step status
- `CURRENT_STATE.json` — replaceable latest state and exact next action
- `DECISIONS.md` — append-only decision history
- `PITFALLS.md` — append-only pitfalls, failed attempts, and high-risk lessons
- `SESSION_REVIEW.md` — one compact append-only file with a short section per session
- `USER_WORK_RULES.md` — communication, safety, and decision rules
- `NEXT_SESSION_PROMPT.md` — replaceable bootstrap prompt for the next window
- `FINAL_REVIEW.md` — reusable methodology distilled after project completion

## Core rule

Current state is replaced with the newest verified facts. Historical learning is preserved.

A new session reads the durable continuity files, verifies repository/git reality, checks prior pitfalls, finds the exact resume point, and continues the current `next_action` without forcing the user to explain the project again.

## Typical use

At project start:

> Use project-handoff-continuity to initialize this project.

When continuing in a new context window:

> Use project-handoff-continuity and continue from the latest verified state.

Before switching windows:

> Use project-handoff-continuity to update the handoff and next-session prompt.

When the project is complete:

> Use project-handoff-continuity to create the final reusable methodology review.
