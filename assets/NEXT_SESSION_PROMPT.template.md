# NEXT SESSION PROMPT

Continue this existing project from its latest verified state.

Before doing new work, read these files in order:

1. `.continuity/PROJECT_ORIGIN.md`
2. `.continuity/USER_WORK_RULES.md`
3. `.continuity/MASTER_PLAN.json`
4. `.continuity/CURRENT_STATE.json`
5. `.continuity/PITFALLS.md`
6. `.continuity/SESSION_REVIEW.md`
7. Latest relevant entries in `.continuity/DECISIONS.md`

Then verify the actual repository/git state against `CURRENT_STATE.json`.

Use `CURRENT_STATE.json` as the live resume point. Pay special attention to:

- `active_step_id`
- `active_step_progress`
- `active_step_completed_parts`
- `resume_point`
- `next_action`
- `next_action_success_check`
- `user_decision_required`
- `do_not_change`

Do not ask the user to repeat decisions already recorded. Do not redesign approved scope without permission. Read PITFALLS before repeating a failed or risky approach.

If `user_decision_required` is false, continue the recorded `next_action` automatically.

Communicate according to `USER_WORK_RULES.md`: assume a non-technical user, give the conclusion first, use plain language, and keep output short unless detail is requested.
