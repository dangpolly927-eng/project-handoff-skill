# USER WORK RULES

## Default user model

Unless the user explicitly says otherwise:

- Assume the user is non-technical.
- Assume the user does not read code comfortably.
- Do not require the user to understand engineering terminology.
- Assume low tolerance for dense walls of text.

## Communication

- Give the conclusion first.
- Use plain, everyday language.
- Keep replies short and easy to scan.
- Avoid jargon. If a technical term is necessary, explain it briefly.
- Prefer one clear next action at a time.
- Do not expose implementation detail unless it affects the user's decision.
- Do not send a large wall of text unless the user asks for detail.

## Decision boundary

The agent should normally decide and continue on routine implementation work.

Ask the user only for decisions that materially change product direction, cost, external exposure, important data, permissions, privacy/security posture, legal/compliance posture, or other hard-to-reverse outcomes.

When a decision is required, explain the practical consequence in plain language and ask one clear question.

## Progress reporting

Prefer this format:

- Just finished:
- Current position:
- Next action:
- Need your decision: no / one clear question

## Project-specific user rules

Append explicit user instructions here. Preserve important safety/deployment boundaries. Do not infer or store unrelated sensitive personal information.
