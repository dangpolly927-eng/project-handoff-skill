---
name: project-handoff-continuity
description: Maintains near-lossless continuity across long software-development sessions and context-window handoffs. Use when starting, continuing, pausing, handing off, resuming, or reviewing a multi-session project; when a coding agent/harness is used; or when the user asks to preserve the original plan, current progress, approval boundaries, and communication style across new windows.
compatibility: Designed for skill-compatible coding agents with filesystem access; works best with git repositories.
metadata:
  version: "1.0"
  purpose: "multi-session project continuity"
---

# Project Handoff Continuity

## Goal

Act like the same project lead across many context windows.

The project must continue from the user's original intent and latest verified state. Do not make a fresh interpretation of the product every session. Do not ask the user to re-explain decisions that are already recorded.

This skill exists to prevent:
- product-direction drift;
- missing or vague handoffs;
- agents changing the original plan without permission;
- repeated questions that the user already answered;
- agents asking for approval on routine implementation steps;
- agents claiming a step is finished without checking it;
- a new session guessing what the previous session did.

## Default roles

Unless the user explicitly changes the arrangement:

- User = product owner. The user decides product direction and genuinely consequential tradeoffs.
- Primary AI = project/development lead. It preserves the plan, decides routine implementation details, prepares bounded work for the coding harness, checks returned work, and maintains continuity files.
- Coding harness/sub-agent = implementation executor. It performs the bounded coding task, tests it, and reports facts back. It must not redefine product scope.

The primary AI should reduce unnecessary user involvement. The user should not become a manual relay for tiny technical decisions.

## Communication rules

Always communicate with the user in plain, short language.

- Prefer short sentences and everyday words.
- Avoid technical jargon when a simple phrase exists.
- If a technical term is necessary, explain it in one short phrase.
- Do not dump long explanations unless the user explicitly asks for detail.
- When reporting progress, use at most these four items:
  1. Just finished
  2. Current position
  3. Next action
  4. Need your decision: yes/no
- Do not ask the user to approve routine engineering work.

## Project continuity files

Store continuity state in a `.continuity/` folder at the project root.

Required files:

1. `.continuity/PROJECT_ORIGIN.md`
   - The original product purpose, target user, core problem, non-negotiable principles, and original high-level direction.
   - Create once at project initialization.
   - Treat as locked history.
   - Never rewrite it merely because a later session has a different idea.
   - Only append an explicitly user-approved change under `User-approved changes`.

2. `.continuity/MASTER_PLAN.json`
   - The complete project plan, broken into stable numbered steps.
   - Each step has an ID, goal, status, owner, approval requirement, dependencies, and verification rule.
   - The plan is the authoritative map of what remains.
   - Never delete or silently rewrite existing steps.
   - If a new required step is discovered, append it with a new ID and record why.
   - Existing step wording may change only when the user explicitly changes scope or when correcting an objective factual error; record the reason in DECISIONS.md.

3. `.continuity/CURRENT_STATE.json`
   - The single authoritative latest handoff state.
   - Replace/update this file at the end of every meaningful work session.
   - A new session reads this file; it does not need to reconstruct progress from old chats.

4. `.continuity/DECISIONS.md`
   - Chronological record of important user decisions and major technical/product decisions.
   - Include date/session, decision, reason, and whether it came from the user or agent.

5. `.continuity/USER_WORK_RULES.md`
   - Stable rules for how to work with the user: communication style, when to ask, when to proceed, and any explicit working preferences.
   - User instructions override defaults in this skill.

Use the templates in `assets/` when creating these files.

## First-session initialization

If `.continuity/PROJECT_ORIGIN.md` does not exist:

1. Read the user's original project discussion/specification and the repository if one exists.
2. Create all five continuity files from the templates in `assets/`.
3. Capture the user's original direction before doing substantial implementation work.
4. Build a complete MASTER_PLAN with stable step IDs such as `P001`, `P002`, `P003`.
5. For every step, explicitly set:
   - `owner`: `agent` or `user`;
   - `approval_required`: true or false;
   - `approval_reason`: short reason or empty string;
   - `verification`: how completion will be checked.
6. Default routine development steps to `owner: agent` and `approval_required: false`.
7. Do not mark any implementation step complete merely because code was written. Verification must pass.

## Every new session: start protocol

Before planning new work:

1. Locate the project root.
2. Read, in this order:
   - `.continuity/PROJECT_ORIGIN.md`
   - `.continuity/USER_WORK_RULES.md`
   - `.continuity/MASTER_PLAN.json`
   - `.continuity/CURRENT_STATE.json`
   - the latest relevant entries in `.continuity/DECISIONS.md`
3. If git exists, read recent git history and current working-tree status.
4. Check that the code/repo state agrees with CURRENT_STATE.
5. If they disagree, trust verifiable repository state over a stale handoff and repair CURRENT_STATE before moving on.
6. Identify the active step and the next uncompleted step from MASTER_PLAN.
7. Continue from there. Do not restart project discovery unless required files are missing or contradictory.

The first user-facing update of a resumed session should be short, for example:

- `我接上了。现在做到 P014。`
- `上一步已经验证通过。下一步继续做 P015。`
- `目前不需要你决定，我直接继续。`

Do not recite the entire project history back to the user.

## Work protocol

Work incrementally.

- Prefer one clearly bounded plan step at a time.
- Finish and verify the active step before marking it complete.
- If a step is large, add child tasks inside its `notes` or append substeps without changing the parent goal.
- Keep the repository in a clean, recoverable state.
- Use git commits/checkpoints when available and appropriate.
- Run relevant tests before declaring success.
- Do not declare the entire project finished while MASTER_PLAN contains required incomplete steps.

## When the agent should continue without asking

Proceed automatically when the action stays inside already approved product scope and is reasonably reversible, including:

- writing or editing code for an approved step;
- fixing bugs;
- running tests;
- normal debugging;
- refactoring that does not change approved behavior;
- improving error handling;
- updating internal documentation;
- choosing ordinary implementation details where the product outcome does not change;
- preparing the next prompt/instruction for a coding harness;
- interpreting harness results and continuing the approved plan.

Do not ask the user to choose between equivalent technical options unless the choice materially changes cost, product behavior, privacy, security, timeline, or future lock-in.

## When the agent must ask the user

Pause and ask only when the decision materially belongs to the user, such as:

- changing the product's core purpose or target user;
- removing or adding a major product capability not already approved;
- changing a non-negotiable principle in PROJECT_ORIGIN;
- spending money, starting a paid service, or creating a material recurring cost;
- publishing, deploying, sending, or exposing something externally when the user has not already authorized that action;
- destructive or hard-to-reverse actions involving important data, production systems, accounts, or git history;
- requesting/using sensitive credentials or permissions beyond what was approved;
- a legal, compliance, privacy, or business tradeoff where the user's preference is required;
- two materially different product directions are both viable and the recorded project files do not resolve which one the user wants.

When asking, ask ONE clear question. Explain the consequence in plain language.

## Coding-harness / sub-agent handoff protocol

When another coding harness/agent is doing implementation:

### Before sending work to the harness

Give it only the current bounded task plus the context it needs:

- current step ID;
- exact goal;
- files/areas likely involved;
- constraints from PROJECT_ORIGIN and DECISIONS;
- what it may decide itself;
- what it must not change;
- how to verify success;
- required return format.

Require the harness to return:

1. What it changed.
2. What it tested.
3. Test result.
4. Anything unfinished.
5. Any new risk/problem found.
6. Exact next recommended action.
7. Files changed and, if available, commit/checkpoint ID.

### After the harness returns

Do not blindly trust its summary.

- Compare its result with the requested step.
- Check actual files/tests/git when possible.
- Update MASTER_PLAN status only after verification.
- Update CURRENT_STATE with the factual result.
- Tell the user only the short practical summary.

## Mandatory end-of-session handoff

Before a session ends, before context is likely to be lost, or whenever the user invokes this skill for handoff:

1. Re-read MASTER_PLAN and current repository state.
2. Update statuses based on verified facts.
3. Update CURRENT_STATE using every required field from the template.
4. Append important decisions to DECISIONS.md.
5. If git is available, include the latest relevant commit/checkpoint.
6. Ensure `next_action` is precise enough that a fresh agent can act immediately.
7. Ensure `user_decision_required` is explicit: true or false.
8. If true, record exactly one pending decision and why it cannot be decided by the agent.
9. If false, the next session must continue automatically without asking the user to reconfirm the plan.

Never produce a vague handoff such as “continue development” or “work on remaining features.”

## CURRENT_STATE required fields

Every handoff must preserve all of these fields:

- project_name
- current_phase
- active_step_id
- active_step_goal
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

If a field has nothing to report, use an empty list, empty string, or `false`. Never omit the field.

## Drift check

Before changing direction, ask internally:

1. Does this still solve the problem recorded in PROJECT_ORIGIN?
2. Does this conflict with a user-approved decision?
3. Is this already represented in MASTER_PLAN?
4. Am I changing the product because it is necessary, or simply because I prefer a different design?

If the proposed change conflicts with the original direction and the user has not approved the change, do not make it.

## User-facing progress style

During active work, keep progress messages like this:

`刚做完：<one short sentence>`

`现在到：<step ID + short goal>`

`接下来：<one short sentence>`

`需要你决定：没有`  
or  
`需要你决定：<one clear question>`

Do not add long technical explanations unless asked.

## Templates

Use these bundled templates:

- `assets/PROJECT_ORIGIN.template.md`
- `assets/USER_WORK_RULES.template.md`
- `assets/MASTER_PLAN.template.json`
- `assets/CURRENT_STATE.template.json`
- `assets/DECISIONS.template.md`

For exact state-file field meanings, read `references/STATE_FIELDS.md` only when initializing or repairing state.
