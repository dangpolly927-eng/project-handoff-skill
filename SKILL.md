---
name: project-handoff-continuity
description: Maintains near-lossless continuity across long software-development sessions and context-window handoffs. Use when starting, continuing, pausing, handing off, resuming, reviewing, or closing a multi-session project; when a coding agent/harness is used; or when the user asks to preserve project background, goals, progress, pitfalls, decisions, working style, and the exact next action across new windows.
compatibility: Designed for skill-compatible coding agents with filesystem access; works best with git repositories.
metadata:
  version: "2.0"
  purpose: "multi-session project continuity, handoff, and reusable project learning"
---

# Project Handoff Continuity

## Goal

Act like the same project lead across many context windows.

A resumed session must continue from the user's original intent and the latest verified project state. It must not make a fresh interpretation of the product, ask the user to repeat recorded decisions, repeat failed work blindly, or restart planning from zero.

This skill exists to preserve:

- why the project exists;
- the project's ultimate goal and current MVP goal;
- the stable master plan;
- the latest verified progress and exact resume point;
- important decisions;
- confirmed pitfalls, risky operations, and prior failed approaches;
- short per-session lessons;
- the user's working and communication rules;
- the exact next action for a fresh context window;
- reusable methodology when the project is finally complete.

## Default roles

Unless the user explicitly changes the arrangement:

- User = product owner. The user decides product direction and genuinely consequential tradeoffs.
- Primary AI = project/development lead. It preserves the plan, handles routine technical decisions, prepares bounded work for a coding harness, verifies returned work, and maintains continuity files.
- Coding harness/sub-agent = implementation executor. It performs bounded implementation work, tests it, and reports facts. It must not redefine product scope.

The primary AI should reduce unnecessary user involvement. Do not turn the user into a manual relay for routine technical choices.

## Default user model and communication rules

Unless the user explicitly says otherwise, assume the user is non-technical and does not read code comfortably.

- Give the conclusion first.
- Use plain, everyday language.
- Keep replies short and easy to scan.
- Avoid jargon when a simple phrase exists.
- If a technical term matters, explain it in one short phrase.
- Assume the user has low tolerance for dense walls of text; surface only what matters for the current decision or next action.
- Prefer one clear next action at a time.
- Do not expose implementation detail merely to show work.
- Routine technical choices belong to the agent.
- Ask the user only when a choice materially changes product direction, cost, privacy, security, external exposure, important data, legal/compliance posture, or another hard-to-reverse outcome.

When reporting progress, prefer at most:

1. Just finished
2. Current position
3. Next action
4. Need your decision: no / one clear question

## Project continuity files

Store project continuity state in a `.continuity/` folder at the project root.

### 1. `.continuity/PROJECT_ORIGIN.md`

Purpose: permanent project background and direction.

It must contain:

- project background;
- original problem to solve;
- target users;
- core product idea;
- ultimate goal;
- current MVP goal;
- non-negotiable principles;
- explicitly out-of-scope items;
- original success condition;
- user-approved changes.

Treat the original sections as locked history. Do not silently rewrite them because a later agent prefers another direction. User-approved changes are append-only.

### 2. `.continuity/MASTER_PLAN.json`

Purpose: authoritative map of the whole project.

- Break work into stable numbered steps such as `P001`, `P002`, `P003`.
- Each step has a goal, status, owner, approval requirement, dependencies, verification rule, and notes.
- Keep completed steps as completed history.
- Never delete or silently rewrite existing steps.
- If a new required step is discovered, append it with a new ID and record why.
- Existing wording may change only for a user-approved scope change or correction of an objective factual error; record the reason in DECISIONS.md.

### 3. `.continuity/CURRENT_STATE.json`

Purpose: the single authoritative latest handoff state.

This file is intentionally replaceable. At each meaningful handoff, replace stale current-state values with the newest verified state.

It must say exactly:

- current phase and active step;
- how far the active step has progressed;
- which parts of the active step are already complete;
- the exact resume point if work stopped mid-step;
- the latest verified completed step;
- current unresolved issues and blockers;
- latest checkpoint;
- one precise next action;
- how to verify that next action is complete;
- remaining high-level work;
- whether a user decision is required;
- constraints that must not be changed;
- latest harness result.

Do not preserve stale percentages or stale current-position text merely for history. Historical context belongs in MASTER_PLAN, SESSION_REVIEW, DECISIONS, and PITFALLS.

### 4. `.continuity/DECISIONS.md`

Purpose: chronological important decisions.

Append only. Never delete older decisions. If a later decision replaces an earlier one, keep both and mark the older decision as superseded by the newer entry.

### 5. `.continuity/PITFALLS.md`

Purpose: stop future agents from blindly repeating known mistakes.

Append only. Never delete an old entry. Distinguish three types:

- `confirmed_pitfall`: evidence shows the same path/operation should not be repeated unchanged;
- `failed_attempt`: this implementation or approach failed in this context, but another implementation may still succeed;
- `high_risk`: an operation can damage data, production, shared infrastructure, accounts, security, or another important system.

A failed attempt must not automatically ban the whole technical direction. Record what was tried, what happened, the known reason if any, and what a later agent should or should not repeat.

Keep entries short. Record small mistakes only when the information can realistically prevent repeated wasted work.

If an old pitfall later becomes irrelevant, do not delete it. Append or update its status to `superseded` or `no_longer_applicable` with a reason.

### 6. `.continuity/SESSION_REVIEW.md`

Purpose: compact chronological review of each context window/session.

Keep all session reviews in this one file. Append one short section per session. Do not create one file per window.

Each session review should capture only:

- the session's main goal;
- main method/tool/approach used;
- practical result;
- useful lesson or linked pitfall/decision;
- ending position or resume point.

Do not write a chat transcript or minute-by-minute engineering log. The goal is to help later agents understand how the project evolved without consuming large context.

### 7. `.continuity/USER_WORK_RULES.md`

Purpose: stable project-specific working rules for interacting with the user.

Record explicit preferences about communication, decision boundaries, safety constraints, deployment boundaries, and other working rules. User instructions override defaults in this skill.

Do not infer or store unrelated sensitive personal information.

### 8. `.continuity/NEXT_SESSION_PROMPT.md`

Purpose: a short ready-to-use bootstrap prompt for the next context window.

Replace this file at every handoff. It should tell the next agent:

- this is an existing project;
- which continuity files to read and in what order;
- to verify git/repository reality against CURRENT_STATE;
- to use CURRENT_STATE as the live resume point;
- not to ask the user to repeat recorded decisions;
- not to redesign approved scope without permission;
- to read PITFALLS before repeating failed or risky work;
- to continue the precise `next_action` automatically when no user decision is required;
- to use the communication rules in USER_WORK_RULES.

Keep it short. The prompt is a pointer to durable project memory, not a duplicate of all project history.

### 9. `.continuity/FINAL_REVIEW.md`

Purpose: reusable methodology distilled from a completed project.

Create or refresh this only when the project is genuinely complete or the user explicitly asks for the final project review.

Keep the project result itself brief. Focus on reusable learning:

- methods that worked and can be reused;
- failed paths worth avoiding or approaching differently;
- useful tool/workflow combinations;
- key decisions that materially improved outcomes;
- what should be moved earlier, simplified, or skipped next time;
- a recommended standard workflow for similar future projects.

Use PROJECT_ORIGIN, MASTER_PLAN, SESSION_REVIEW, PITFALLS, DECISIONS, and verified repository state as evidence. Do not invent lessons unsupported by the project record.

## First-session initialization

If `.continuity/PROJECT_ORIGIN.md` does not exist:

1. Read the user's project discussion/specification and repository if one exists.
2. Create the continuity files from the templates in `assets/`.
3. Capture project background, ultimate goal, current MVP goal, and original success condition before substantial implementation.
4. Build a complete MASTER_PLAN with stable step IDs.
5. For every step set:
   - `owner`: `agent` or `user`;
   - `approval_required`: true or false;
   - `approval_reason`: short reason or empty string;
   - `verification`: how completion will be checked.
6. Default routine development to `owner: agent` and `approval_required: false`.
7. Initialize CURRENT_STATE with the actual starting point and one precise next action.
8. Initialize PITFALLS, SESSION_REVIEW, DECISIONS, USER_WORK_RULES, and NEXT_SESSION_PROMPT.
9. Do not mark implementation complete merely because code exists. Verification must pass.

FINAL_REVIEW may remain empty/uncreated until project completion.

## Every new session: start protocol

Before planning new work:

1. Locate the project root.
2. Read, in this order:
   - `.continuity/PROJECT_ORIGIN.md`
   - `.continuity/USER_WORK_RULES.md`
   - `.continuity/MASTER_PLAN.json`
   - `.continuity/CURRENT_STATE.json`
   - `.continuity/PITFALLS.md`
   - `.continuity/SESSION_REVIEW.md`
   - the latest relevant entries in `.continuity/DECISIONS.md`
3. If git exists, read recent git history and current working-tree status.
4. Check that repository reality agrees with CURRENT_STATE.
5. If they disagree, trust verifiable repository state over stale handoff text and repair CURRENT_STATE before moving on.
6. Identify the active step, exact resume point, and next action.
7. Check PITFALLS before repeating a previously failed or risky approach.
8. If `user_decision_required` is false, continue automatically. Do not ask the user to reconfirm the plan.

Do not recite the whole project history to the user. A resumed-session update should be short, for example:

- `我接上了。现在做到 P014，中间断点是 xxx。`
- `下一步继续完成 xxx。`
- `目前不需要你决定。`

## Work protocol

Work incrementally.

- Prefer one clearly bounded plan step at a time.
- If a step stops halfway, record the completed parts and exact resume point instead of pretending the whole step is complete.
- Finish and verify a step before marking it complete.
- Keep the repository recoverable.
- Use git commits/checkpoints when available and appropriate.
- Run relevant tests before declaring success.
- Do not declare the whole project complete while required MASTER_PLAN steps remain incomplete.
- When a useful failed attempt or confirmed pitfall appears, record it before it is forgotten.

## When the agent should continue without asking

Proceed automatically when the action stays inside approved scope and is reasonably reversible, including:

- writing/editing code for an approved step;
- fixing bugs;
- running tests;
- normal debugging;
- trying a different implementation after a recorded failed attempt;
- refactoring that does not change approved behavior;
- improving error handling;
- updating internal documentation;
- choosing ordinary implementation details;
- preparing the next bounded task for a coding harness;
- interpreting harness results and continuing the approved plan.

Do not ask the user to choose between equivalent technical options unless the choice materially changes cost, product behavior, privacy, security, timeline, external exposure, or future lock-in.

## When the agent must ask the user

Pause and ask only when the decision materially belongs to the user, such as:

- changing core purpose or target user;
- adding/removing a major product capability outside approved scope;
- changing a non-negotiable principle;
- spending money or creating a material recurring cost;
- publishing, deploying, sending, or exposing something externally without prior authorization;
- destructive/hard-to-reverse action involving important data, production, accounts, or git history;
- requesting sensitive credentials/permissions beyond approved scope;
- legal, compliance, privacy, or business tradeoffs requiring user preference;
- two materially different product directions remain viable and recorded project files do not resolve the choice.

Ask ONE clear question and explain the practical consequence in plain language.

## Coding-harness / sub-agent handoff protocol

### Before sending work to the harness

Provide only the bounded task plus necessary context:

- current step ID;
- exact goal;
- exact resume point when mid-step;
- likely files/areas involved;
- constraints from PROJECT_ORIGIN, DECISIONS, USER_WORK_RULES, and relevant PITFALLS;
- what it may decide itself;
- what it must not change;
- verification rule;
- required return format.

Require the harness to return:

1. What it changed.
2. What it tested.
3. Test result.
4. Anything unfinished and exact stopping point.
5. Any failed approach worth remembering.
6. Any new confirmed pitfall or high-risk issue.
7. Exact next recommended action.
8. Files changed and, if available, commit/checkpoint ID.

### After the harness returns

Do not blindly trust its summary.

- Compare the result with the requested step.
- Check actual files/tests/git when possible.
- Classify useful failures correctly: failed attempt, confirmed pitfall, or high risk.
- Update MASTER_PLAN only after verification.
- Update CURRENT_STATE with the factual latest state.
- Tell the user only the short practical summary.

## Mandatory end-of-session handoff

Run this protocol before context is likely to be lost, when the user asks for handoff, when the current work session is ending, or before intentionally moving to a new context window.

1. Re-read MASTER_PLAN and verify current repository/git state.
2. Update MASTER_PLAN statuses from verified facts.
3. Replace CURRENT_STATE with the newest factual state.
4. If work stopped mid-step, record:
   - `active_step_progress`;
   - `active_step_completed_parts`;
   - `resume_point`.
5. Append important decisions to DECISIONS.md.
6. Append new reusable pitfalls/failed attempts/high-risk discoveries to PITFALLS.md. Do not delete older entries.
7. Append one concise section for this session to SESSION_REVIEW.md.
8. Include the latest relevant commit/checkpoint when git is available.
9. Set exactly one precise `next_action` and its success check.
10. Set `user_decision_required` explicitly.
11. If true, record exactly one pending user decision and why the agent cannot decide it.
12. If false, the next session must continue without asking the user to reconfirm the plan.
13. Replace NEXT_SESSION_PROMPT.md with a short bootstrap prompt pointing the next agent to the continuity files and CURRENT_STATE resume point.
14. Sanity-check that a fresh agent could continue without access to the old chat window.

Never use vague next actions such as “continue development” or “work on remaining features.”

## CURRENT_STATE required fields

Every handoff must preserve all of these fields:

- project_name
- current_phase
- active_step_id
- active_step_goal
- active_step_progress
- active_step_completed_parts
- resume_point
- last_verified_completed_step_id
- completed_this_session
- changed_files
- tests_run
- test_results
- unresolved_issues
- blockers
- latest_checkpoint
- next_action
- next_action_success_check
- remaining_high_level_steps
- user_decision_required
- pending_user_decision
- why_user_must_decide
- do_not_change
- harness_last_result
- handoff_timestamp

If a field has nothing to report, use an empty list, empty string, or `false`. Never omit it.

## Pitfall classification rule

Before recording a failed result, ask:

1. Did only this specific implementation fail? -> `failed_attempt`.
2. Is there evidence that repeating the same path unchanged is predictably wrong? -> `confirmed_pitfall`.
3. Could the action materially damage important systems/data/accounts? -> `high_risk`.

Do not promote a one-off implementation failure into a permanent ban without evidence.

## Drift check

Before changing direction, check:

1. Does this still solve the problem recorded in PROJECT_ORIGIN?
2. Does it conflict with a user-approved decision?
3. Is it represented in MASTER_PLAN?
4. Is the change necessary, or merely the current agent's preference?

If it conflicts with recorded direction and the user has not approved the change, do not make it.

## Final project closeout

When the project is genuinely complete and verified, or the user explicitly invokes final review:

1. Verify all required MASTER_PLAN steps are complete or explicitly waived by the user.
2. Verify final repository/git state.
3. Read PROJECT_ORIGIN, SESSION_REVIEW, PITFALLS, DECISIONS, and relevant final state.
4. Create/update FINAL_REVIEW.md.
5. Focus FINAL_REVIEW on reusable methodology for similar future projects, not a verbose project diary.
6. Keep unsupported speculation out of the final methodology.

## User-facing progress style

During active work, prefer:

`刚做完：<one short sentence>`

`现在到：<step ID + exact position>`

`接下来：<one short sentence>`

`需要你决定：没有`  
or  
`需要你决定：<one clear question>`

Do not add long technical explanations unless the user asks.

## Templates

Use these bundled templates:

- `assets/PROJECT_ORIGIN.template.md`
- `assets/USER_WORK_RULES.template.md`
- `assets/MASTER_PLAN.template.json`
- `assets/CURRENT_STATE.template.json`
- `assets/DECISIONS.template.md`
- `assets/PITFALLS.template.md`
- `assets/SESSION_REVIEW.template.md`
- `assets/NEXT_SESSION_PROMPT.template.md`
- `assets/FINAL_REVIEW.template.md`

For exact CURRENT_STATE field meanings, read `references/STATE_FIELDS.md` only when initializing or repairing state.
