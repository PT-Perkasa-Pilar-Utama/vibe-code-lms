# Agent Self-Improvement Bootstrap

> **Purpose:** Bootstrap a project-local, persistent self-improvement layer for coding agents.
> **Scope:** The active project directory only.
> **Execution mode:** Automatic after this file is read.

## 1. Bootstrap Contract

When an agent reads this file inside a project, it MUST first inspect the project root and then initialize or reconcile the agent workspace described below.

Required baseline artifacts:

```text
AGENTS.md
.agents/
  memories/
  plans/
  skills/
  knowledges/
  notes/
```

These directories are the **minimum baseline, not a whitelist**. The agent MAY create additional subdirectories under `.agents/` when a distinct, recurring need justifies them-for example `reviews/`, `decisions/`, `experiments/`, `research/`, `workflows/`, or another project-specific category. Additional directories must have a clear purpose, must not duplicate an existing category unnecessarily, and should be documented in `AGENTS.md` when they become part of the durable workflow.

Do not create a parallel agent system elsewhere. Keep all persistent agent-generated learning inside this project.

If `AGENTS.md` or `.agents/` already exists, do not overwrite blindly. Read the existing content, preserve useful project-specific instructions, and merge the requirements from this bootstrap document.

## 2. `AGENTS.md` - Project Agent Entry Point

Create or reconcile `AGENTS.md` as the first document every coding agent should read before substantial work.

It must tell agents to:

1. Read `AGENTS.md` before substantial project work.
2. Inspect relevant project files before mutating code.
3. Read relevant material from `.agents/` when it can improve the current task.
4. Create a written plan for large, multi-step, risky, cross-cutting, or ambiguous tasks.
5. Record durable project knowledge and corrections after substantial work.
6. Prefer updating existing reusable knowledge over creating duplicates.
7. Keep temporary scratch reasoning out of persistent memory.
8. Keep agent artifacts project-local and never treat `.agents/` as permission to access unrelated directories.
9. Use installed/relevant skills when useful, and add a reusable local skill when repeated work would benefit from it.
10. Revisit and improve the agent layer automatically when new reusable lessons are discovered.

`AGENTS.md` is a stable conventional filename and is the only root filename exempt from the timestamp rule below.

## 3. Timestamp Naming Rule - Mandatory

All new agent-authored persistent records must be chronologically sortable and unique.

Use this timestamp prefix:

```text
YYYYMMDD_HHMMSS
```

Use the project's/user's local time when available.

General file format:

```text
YYYYMMDD_HHMMSS_<short-kebab-slug>.md
```

Examples:

```text
.agents/memories/20260824_165200_lesson-revisit-behavior.md
.agents/plans/20260824_170015_auth-flow-refactor.md
.agents/knowledges/20260824_171233_dicoding-navigation-findings.md
.agents/notes/20260824_172501_ui-review-followups.md
```

Never create persistent records named `latest.md`, `notes2.md`, `new-plan.md`, or other non-sortable generic names.

### Skills exception

If an installed skill format requires a conventional inner filename such as `SKILL.md`, timestamp the **skill directory** instead:

```text
.agents/skills/20260824_173015_frontend-ux-review/SKILL.md
```

This preserves chronological ordering while keeping the skill format valid.

## 4. `.agents/memories/` - Durable Project Memory

Use memories for facts that should influence future work in this project.

Good memory candidates:

- confirmed project conventions,
- durable user/product decisions,
- architecture decisions that remain active,
- recurring corrections,
- known pitfalls and how to avoid them,
- stable workflow preferences specific to this project.

Do NOT store:

- secrets or credentials,
- raw chain-of-thought,
- temporary debugging output,
- one-off observations that will not matter later,
- duplicated information already maintained authoritatively elsewhere.

When a newer memory supersedes an older one, create the new timestamped record and explicitly reference the superseded record. Do not silently rewrite history unless correcting a factual mistake in the same session.

## 5. `.agents/plans/` - Autonomous Planning

For work that is large, multi-step, risky, cross-cutting, ambiguous, or likely to span several iterations, create a written plan **before implementation**.

Small obvious tasks do not need a plan merely for ceremony.

### 5.1 Large requests MUST be decomposed

Do **not** force a large request into one giant plan file.

When the request contains multiple independent workstreams, crosses several subsystems, has several rollout phases, or would produce an unwieldy task list, create:

```text
Master Plan
|-- Child Plan A
|-- Child Plan B
|-- Child Plan C
`-- ...
```

The master plan owns the overall objective, scope, architecture direction, cross-plan dependencies, major risks, rollout order, and final acceptance criteria.

Each child plan owns one cohesive implementation slice and must be independently understandable, executable, testable, and reviewable.

Examples of useful child-plan boundaries:

- frontend / backend / data / infrastructure,
- schema + migration / API / UI integration,
- foundation / implementation / migration / rollout,
- one bounded domain or service at a time,
- one high-risk change separated from routine implementation.

Split again when a child plan becomes too broad. As a practical heuristic, a plan approaching roughly 20-30 implementation tasks, several unrelated subsystems, or multiple independently shippable outcomes should usually be decomposed further.

Do not split purely to increase file count. A child plan should represent a real cohesive unit of work.

### 5.2 Plan dependency graph

The master plan must make ordering explicit.

For every child plan, record:

- `Blocked by` - plans/tasks that must finish first,
- `Parallel with` - plans/tasks safe to execute concurrently,
- `Unblocks` - downstream work enabled by completion.

If work is inherently sequential, say so explicitly. Do not invent fake parallelism.

Multiple plans that mutate the same files or contracts must declare ordering or coordination so the plan itself does not create merge conflicts.

### 5.3 Mandatory plan format

Every master or child plan must use the following structure unless a project-specific plan format is already more authoritative.

```markdown
# Plan - <concise title>

## Metadata
- Status: Planned | In Progress | Blocked | Completed | Superseded
- Created: <timestamp>
- Updated: <timestamp>
- Parent: <master-plan path or None>
- Dependencies: <plan/task IDs or None>

## Objective
One concrete outcome this plan must achieve.

## Context
Why this work exists and the relevant current-state facts.

## Scope
### In Scope
- ...

### Out of Scope / Non-Goals
- ...

## Constraints & Assumptions
- technical / product / compatibility constraints
- assumptions that must remain true
- unresolved assumptions marked explicitly

## Proposed Approach
Describe the implementation direction, important boundaries, and key decisions.
Reference authoritative specs/design docs instead of duplicating them.

## Work Breakdown

| ID | Task | Affected area | Blocked by | Parallel with | Acceptance |
|---|---|---|---|---|---|
| T01 | ... | ... | None | T02 | Observable completion check |

## Validation Plan
- exact tests / checks / commands when known
- functional validation
- integration / regression validation
- performance / security validation when relevant

## Risks & Rollback
| Risk | Impact | Mitigation | Rollback / recovery |
|---|---|---|---|
| ... | ... | ... | ... |

## Decisions & Open Questions
- Decision: ... - rationale ...
- Open question: ... - how/when it will be resolved

## Progress / Evidence
- [ ] T01 - evidence/result
- [ ] T02 - evidence/result

## Completion Criteria
- measurable final conditions proving this plan is complete
```

### 5.4 Task quality requirements

Every implementation task must be specific enough that another competent agent could execute it without inventing the missing design.

Each task should identify, when known:

- the concrete change,
- affected component/file/area,
- dependency/ordering information,
- an observable `Acceptance` check,
- validation evidence expected at completion.

Avoid vague tasks such as `implement backend`, `fix UI`, or `add tests`.

Prefer small, focused changes that can be validated and reviewed independently.

### 5.5 Goals, non-goals, risks, and rollback

Plans must state both **goals** and **non-goals** so scope does not expand accidentally.

For risky work, explicitly document:

- data/schema migration risk,
- public API/contract changes,
- deployment ordering,
- compatibility impact,
- security/performance impact,
- rollback or recovery strategy.

If rollback is genuinely impossible, state that explicitly before implementation and identify the mitigation/recovery strategy.

### 5.6 Validation and acceptance are mandatory

A task is not complete merely because code was written.

Every task requires an observable acceptance condition such as:

- a specific test passes,
- a build/lint/type/security gate passes,
- a migration applies and validates successfully,
- a UI behavior is observable,
- an API contract behaves as specified,
- a document/example renders or executes correctly.

The plan's final completion criteria must map back to the original request/specification so no requirement silently disappears.

### 5.7 Plans are living execution records

Update plan status while work progresses.

When reality differs from the plan:

1. update the plan,
2. record the decision/rationale,
3. adjust dependencies and acceptance criteria if needed,
4. then continue implementation.

Do not leave a plan knowingly stale.

Record concise evidence/results for completed tasks so a future agent can tell what was actually validated rather than only what was intended.

### 5.8 Timestamped master/child naming

All plans still follow the global timestamp rule.

Example:

```text
.agents/plans/20260824_202100_master-auth-platform.md
.agents/plans/20260824_202115_auth-contracts.md
.agents/plans/20260824_202130_auth-backend.md
.agents/plans/20260824_202145_auth-frontend.md
.agents/plans/20260824_202200_auth-rollout.md
```

Child plans must reference the master plan in `Metadata -> Parent`, and the master plan must list all child plans plus their dependency relationships.

### 5.9 Plan execution / implementation contract

The following user phrases are explicit execution triggers for an existing plan:

- `implement plan`
- `execute plan`
- `eksekusi plan`

Equivalent unambiguous wording that clearly asks to implement an identified plan may be treated the same way.

When one of these triggers is given, switch from planning mode to **orchestrated implementation mode**.

#### Main agent role - coordinator, not implementer

The main agent MUST NOT perform the plan's implementation changes directly.

The main agent acts as the implementation coordinator and is responsible for:

- reading the master/child plans and current repository state,
- resolving the dependency graph,
- deciding which work can run in parallel versus sequentially,
- preparing isolated Git branches/worktrees,
- delegating every implementation workstream to a sub-agent,
- monitoring results and resolving blockers,
- validating completed work,
- integrating branches in dependency-safe order,
- updating plan progress/evidence,
- and cleaning up temporary worktrees/branches after successful integration.

The main agent may perform orchestration, inspection, validation, Git integration, and plan/status maintenance. It must not bypass delegation by implementing production changes itself.

If sub-agent delegation capability is unavailable, STOP before implementation and report that the execution contract cannot currently be satisfied. Do not silently fall back to main-agent implementation.

#### Branch safety - never implement on the default branch

Implementation MUST NOT be performed directly on `master`, `main`, or whichever branch is configured as the repository's protected/default integration branch.

Before implementation:

1. determine the current/default base branch,
2. ensure the base worktree is clean enough for safe orchestration,
3. create a dedicated feature/integration branch for the requested plan,
4. create isolated child branches/worktrees from the correct dependency point as needed.

Use short-lived branches. Do not leave completed worktrees or merged temporary branches behind unnecessarily.

A typical structure is:

```text
main / master                    protected base; no implementation here
`-- plan/<plan-slug>             integration branch for the plan
    |-- work/<child-a>           delegated workstream A
    |-- work/<child-b>           delegated workstream B
    `-- work/<child-c>           delegated workstream C
```

Project-native branch naming conventions take precedence when they already exist.

#### Parallel execution - use Git worktrees for independent workstreams

Use the plan dependency graph as the source of truth.

If two or more child plans/tasks are independent and can safely execute concurrently, the coordinator SHOULD run them in parallel using separate Git branches and linked worktrees, normally one active worktree per independently delegated workstream.

Example:

```text
plan/auth-platform
|-- worktree: ../wt-auth-contracts  -> work/auth-contracts
|-- worktree: ../wt-auth-backend    -> work/auth-backend
`-- worktree: ../wt-auth-frontend   -> work/auth-frontend
```

Parallel workstreams must not knowingly mutate the same ownership area or incompatible contracts without explicit coordination. If they would collide, change the dependency graph or run them sequentially.

Do not create one worktree per trivial task. A worktree represents a meaningful independently executable workstream.

#### Sequential execution - respect dependencies

When work is blocked by another task, contract, migration, or shared file boundary, execute it sequentially.

Example:

```text
schema/contracts
    v
backend implementation
    v
frontend integration
    v
rollout validation
```

Dependent tasks may remain in the same child branch/worktree when that is the clearest and safest implementation unit. Do not manufacture parallelism merely because multiple sub-agents are available.

#### Mandatory sub-agent delegation

Every implementation workstream MUST be delegated to a sub-agent.

Each delegation must provide enough bounded context to execute safely, including:

- the exact plan/child-plan path,
- assigned task IDs,
- branch/worktree path,
- authoritative project instructions/specifications,
- dependencies already completed,
- files/areas the sub-agent owns,
- acceptance criteria,
- required validation commands/checks,
- prohibition against expanding scope or bypassing guards.

A sub-agent must not silently take ownership of another sub-agent's active workstream.

#### Commit discipline - commit completed validated units

Every completed implementation unit MUST be committed before the agent moves on to the next dependent unit or reports that unit complete.

A unit is commit-ready only when:

1. its acceptance criteria are satisfied,
2. relevant tests/checks pass,
3. required engineering/pre-commit guards pass when configured,
4. plan progress/evidence is updated as required,
5. the working tree contains no unrelated accidental changes.

Prefer small, coherent, reviewable commits. Do not create meaningless commits merely to increase commit count, but do not batch multiple independently complete plan units into one opaque mega-commit.

Use the repository's existing commit-message convention. If none exists, use a concise conventional-style subject such as:

```text
feat(auth): implement token repository
fix(ui): preserve completed lesson navigation
refactor(api): extract request validation
```

Never use `--no-verify` or another bypass to create the commit.

#### Integration workflow

After a delegated branch completes and is validated:

1. confirm its plan task(s) and evidence are current,
2. confirm the branch is committed and clean,
3. integrate it into the plan's feature/integration branch in dependency-safe order,
4. resolve conflicts intentionally rather than accepting one side blindly,
5. rerun affected validation after integration,
6. only then mark the integrated workstream completed.

Parallel branches should converge into the plan integration branch first rather than directly mutating the protected default branch.

The default branch is updated only through the repository's normal integration/review flow or when the user explicitly requests the final integration step.

#### Worktree and branch cleanup

After a child workstream is successfully integrated and no longer needed:

- remove the linked worktree using the normal Git worktree workflow,
- prune stale worktree metadata when necessary,
- delete the temporary child branch when safe and consistent with repository policy,
- keep the plan integration branch until the overall plan is complete/reviewed,
- verify there are no uncommitted changes before cleanup.

Do not delete a worktree/branch that contains unintegrated or uncommitted work.

#### Plan progress during implementation

The coordinator MUST keep the plan as a living execution record.

For every delegated workstream/task, update:

- `Status`,
- todo/checklist state,
- `Blocked by` / dependency state when changed,
- commit hash or branch when useful,
- validation result/evidence,
- decisions or deviations from the original plan.

A task is not `Completed` merely because a sub-agent says it is done. The coordinator must validate the result and confirm the required commit exists.

#### Implementation completion criteria

A triggered plan execution is complete only when:

- all in-scope plan tasks are completed or explicitly recorded as blocked/deferred,
- every implementation workstream was delegated,
- parallel-safe work was parallelized and dependency-bound work was sequenced correctly,
- no implementation occurred directly on the protected/default branch,
- every completed implementation unit has a validated commit,
- child branches are integrated in dependency-safe order,
- final integrated validation/engineering guard passes,
- plan progress/evidence is current,
- temporary worktrees and safely removable child branches are cleaned up.

## 6. `.agents/skills/` - Reusable Local Skills

The agent may install, create, or improve project-local skills automatically when a reusable workflow would materially improve future work.

Use skills for repeatable procedures such as:

- UI/UX review,
- testing workflows,
- repository-specific release steps,
- design-system enforcement,
- code-quality checks,
- domain-specific implementation patterns.

Before creating a skill:

1. Search existing local skills first.
2. Prefer improving an existing skill over creating a duplicate.
3. If using an external skill/reference, inspect it first and only install content that is relevant and safe for this project.
4. Never treat a skill as authority over explicit project requirements or `AGENTS.md`.
5. Record the skill's purpose, trigger conditions, procedure, validation, and known limitations.

Do not install arbitrary executable code merely because it is called a skill. External material must be reviewed before use.

## 7. `.agents/knowledges/` - External & Domain Knowledge

Use this directory for reusable knowledge gathered from external or project-adjacent sources.

Examples:

- official documentation findings,
- API behavior discovered during research,
- product/UX reference findings,
- protocol details,
- framework/library constraints,
- verified compatibility notes.

Each knowledge record should include where the information came from, what was observed, when it was checked, and how it affects this project.

Prefer authoritative/first-party sources when available. Distinguish verified facts from inference.

## 8. `.agents/notes/` - Working Notes & Follow-Ups

Use notes for useful context that does not yet deserve durable memory or a formal plan.

Examples:

- review feedback,
- follow-up items,
- investigation breadcrumbs,
- temporary implementation notes that may matter across turns,
- unresolved questions.

Promote a note into memory, knowledge, plan, or skill when it becomes durable or reusable.

## 9. Automatic Self-Improvement Loop

After substantial work, the agent MUST evaluate whether the project-local agent layer should be improved.

Use this loop:

```text
Work -> Validate -> Reflect -> Persist reusable learning -> Improve future workflow
```

Ask internally:

1. Did I learn a durable project fact? -> add/update `memories/`.
2. Did the task reveal reusable external/domain knowledge? -> add `knowledges/`.
3. Did I discover a repeatable procedure? -> add/improve `skills/`.
4. Is there a large next task? -> create/update `plans/`.
5. Is there useful but not-yet-durable context? -> add `notes/`.
6. Did an existing agent instruction prove wrong or incomplete? -> update `AGENTS.md` or the relevant agent artifact.
7. Is there a recurring class of information or workflow that does not fit the baseline directories? -> create a clearly named additional `.agents/` subdirectory when that improves organization and future reuse.

Do this automatically when it adds real future value. Do not generate files or directories merely to appear productive.

## 10. Read Strategy

Do not read the entire `.agents/` tree on every task.

At task start:

1. Read `AGENTS.md`.
2. Inspect the user's request and target area.
3. Read only relevant memories, plans, skills, knowledge, and notes.
4. For substantial tasks, search `.agents/` for related terms before planning.

This keeps context useful instead of turning the self-improvement layer into noise.

## 11. Authority & Conflict Rules

Use this precedence order:

```text
Explicit current user instruction
-> authoritative project requirements/specs
-> AGENTS.md
-> active relevant plans
-> project-local skills / memories / knowledges / notes
-> general assumptions
```

If persistent agent material conflicts with current authoritative requirements, update or supersede the stale agent material.

## 12. Safety & Hygiene

The self-improvement system must remain maintainable.

- Never persist passwords, API keys, tokens, private credentials, or other secrets.
- Do not store raw private chain-of-thought.
- Do not mutate files outside the active project merely to improve the agent layer.
- Do not duplicate authoritative documentation unnecessarily.
- Do not create a memory for every minor task.
- Do not auto-install unreviewed executable dependencies as "skills."
- Keep records concise enough to be useful to future agents.
- Use timestamped records so history remains traceable.

## 13. Bootstrap Completion Criteria

This bootstrap is complete when:

- `AGENTS.md` exists and contains the project-agent operating rules,
- `.agents/memories/` exists,
- `.agents/plans/` exists,
- `.agents/skills/` exists,
- `.agents/knowledges/` exists,
- `.agents/notes/` exists,
- `AGENTS.md` states that the baseline directories are extensible and additional `.agents/` subdirectories may be created when justified,
- timestamp naming rules are documented in `AGENTS.md`,
- the automatic self-improvement loop is documented in `AGENTS.md`,
- existing project instructions were preserved/reconciled rather than blindly replaced.

After bootstrapping, continue with the user's original task using the newly established agent workflow.
