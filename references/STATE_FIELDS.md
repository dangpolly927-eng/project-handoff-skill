# State field meanings

Use this only when initializing or repairing `.continuity/CURRENT_STATE.json`.

- `project_name`: Stable project name.
- `current_phase`: Current broad phase, such as MVP foundation, messaging, testing, or release preparation.
- `active_step_id`: Stable ID from MASTER_PLAN.
- `active_step_goal`: Human-readable goal copied from the plan.
- `active_step_progress`: Short current progress description, such as `about 60%` or `core flow complete, error handling remaining`. This is live state and may be replaced at the next handoff.
- `active_step_completed_parts`: Concrete sub-parts of the active step already verified complete.
- `resume_point`: Exact place where a fresh agent should resume if the active step stopped halfway.
- `last_verified_completed_step_id`: Most recent full plan step proven complete.
- `completed_this_session`: Concrete verified outcomes from the latest session only.
- `changed_files`: Files changed in the latest session/harness run.
- `tests_run`: Tests/checks actually performed.
- `test_results`: Results of those checks; do not claim tests that were not run.
- `unresolved_issues`: Known problems that do not fully block continuation.
- `blockers`: Problems that prevent the next planned action.
- `latest_checkpoint`: Git commit/hash/tag or other recoverable checkpoint.
- `next_action`: One precise action a fresh session can start immediately. Never write only `continue development`.
- `next_action_success_check`: How the next session knows that action is done.
- `remaining_high_level_steps`: Short list of major remaining steps in original order where applicable.
- `user_decision_required`: Boolean. `false` means the next session should continue without asking for reconfirmation.
- `pending_user_decision`: Exactly one question if `user_decision_required` is true; otherwise empty.
- `why_user_must_decide`: Why the agent cannot safely or legitimately decide it; otherwise empty.
- `do_not_change`: Product/technical/safety constraints the next session must preserve.
- `harness_last_result.task_sent`: Most recent bounded task sent to the coding harness.
- `harness_last_result.what_changed`: Factual changed areas reported/verified.
- `harness_last_result.what_tested`: Checks performed.
- `harness_last_result.result`: Overall harness result.
- `harness_last_result.unfinished`: Remaining parts of the harness task.
- `harness_last_result.stopping_point`: Exact point where harness work stopped.
- `harness_last_result.failed_attempts`: Useful failed approaches from the latest harness run; classify persistent lessons into PITFALLS.md.
- `harness_last_result.new_risks`: Newly discovered risks/problems.
- `harness_last_result.recommended_next_action`: Harness recommendation; the primary agent must verify it before treating it as authoritative.
- `handoff_timestamp`: Time the state was last verified/updated.

## Replacement rule

`CURRENT_STATE.json` stores the latest live state. Replace stale progress, stale resume points, and stale next actions with the newest verified facts. Preserve historical learning in MASTER_PLAN, SESSION_REVIEW, DECISIONS, and PITFALLS instead of keeping obsolete live state here.
