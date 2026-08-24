# Project Handoff Skill

A reusable handoff skill for long AI-assisted software projects.

Its purpose is to keep a project moving in the same direction across many chat/context windows.

## What it preserves

- Original product direction
- Full project plan
- Latest verified progress
- Important decisions
- User communication preferences
- Clear boundary between what the agent may decide and what needs user approval
- Structured handoff to coding harnesses/sub-agents

## Main file

`SKILL.md`

## Templates

The `assets/` folder contains templates the agent uses to create a `.continuity/` folder inside an actual project.

The key rule is simple: each new session reads the original direction and the latest state, then continues from the next verified step instead of reconstructing the project from old chats.

## Typical use

At the start of a project:

> Use project-handoff-continuity to initialize this project.

When continuing in a new context window:

> Use project-handoff-continuity and continue from the latest verified state.

Before switching windows:

> Use project-handoff-continuity to create/update the handoff state.
