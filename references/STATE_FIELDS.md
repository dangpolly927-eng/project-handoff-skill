# State field meanings

Use this only when initializing or repairing `.continuity/CURRENT_STATE.json`.

- `project_name`: Stable project name.
- `current_phase`: Current broad phase, such as MVP foundation, messaging, testing, release preparation.
- `active_step_id`: Stable ID from MASTER_PLAN.
- `active_step_goal`: Human-readable goal copied from the plan.
- `last_verified_completed_step_id`: Most recent step proven complete.
- `completed_this_session`: Concrete verified outcomes from this session.
- `changed_files`: Files changed in the latest session/harness run.
- `tests_run`: Tests/checks actually performed.
- `test_results`: Results of those checks; do not claim tests that were not run.
- `unresolved_issues`: Known problems that do not fully block continuation.
- `blockers`: Problems that prevent the next planned action.
- `latest_checkpoint`: Git commit/hash/tag or other recoverable checkpoint.
- `next_action`: One precise action a fresh session can start immediately.
- `next_action_success_check`: How the next session knows that action is done.
- `remaining_high_level_steps`: Short list of the major remaining steps, preserving original order where applicable.
- `user_decision_required`: Boolean. `false` means the next session should continue without asking for reconfirmation.
- `pending_user_decision`: Exactly one question if user_decision_required is true; otherwise empty.
- `why_user_must_decide`: Why the agent cannot safely/legitimately decide it; otherwise empty.
- `do_not_change`: Product/technical constraints the next session must preserve.
- `harness_last_result`: Structured summary of the most recent coding-harness result.
- `handoff_timestamp`: Time the state was last verified/updated.
