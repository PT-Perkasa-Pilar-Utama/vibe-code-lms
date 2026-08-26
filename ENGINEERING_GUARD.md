# Engineering Bootstrap + Architecture Guard

> **Purpose:** Convert the current prototype/repository into the declared production tech-stack shape, establish project architecture and implementation plans, create the project-specific engineering guard, then STOP before feature implementation begins.
> **Scope:** Bootstrap, architecture planning, repository structure, agent instructions, and local quality guard only.
> **Execution boundary:** This document is NOT authorization to execute the implementation plans, finish product features, or run the full product E2E suite. Page/command 8 is the separate implementation handoff.

## 1. Tech Stack Contract Gate - Mandatory

Before generating or modifying any engineering guard, the agent MUST have an explicit **Tech Stack Contract**. The agent must not infer the authoritative stack from repository contents and proceed on its own.

Minimum required fields:

```text
OS / target environment:
Frontend:
Backend / server:
Database:
Observability:
```

`Observability` may be `none`. If the guard must support more than one operating system, declare the target explicitly, for example `Linux + macOS` or `POSIX-compatible`.

Examples of sufficiently specific contracts:

```text
OS / target environment: Linux
Frontend: Next.js
Backend / server: NestJS
Database: PostgreSQL
Observability: OpenTelemetry
```

```text
OS / target environment: Linux
Frontend: HTML + SCSS + JavaScript
Backend / server: Go
Database: SQLite
Observability: none
```

### Hard stop rule

If any required field is missing, ambiguous, or only names a runtime/general library rather than an actionable project stack, the agent MUST stop immediately.

While this gate is closed, the agent may only report what information is missing. It MUST NOT:

- create the guard script,
- create or modify project files,
- install packages or tools,
- configure or weaken linters,
- add hooks,
- mutate `AGENTS.md` or `.agents/`,
- generate architecture/configuration artifacts,
- infer a stack from `package.json`, source files, lockfiles, or repository naming and treat that inference as user authorization.

Examples that are **not sufficiently specific** on their own:

```text
Frontend: React
Backend: Node.js
```

The agent must request the missing framework/server/environment details and wait.

### Authority rule

The Tech Stack Contract must come from the current user instruction or an explicitly authoritative project specification. Repository inspection is evidence for **implementation details**, not permission to choose the stack.

## 2. Repository Inspection - Only After the Stack Gate Opens

Once the Tech Stack Contract is complete, inspect the project before generating the guard. Use repository evidence to resolve implementation details such as:

- package manager / lockfile,
- framework version and conventions,
- existing linter and formatter,
- type checker/compiler,
- test runner,
- build tooling,
- migration/database tooling,
- logging library,
- observability instrumentation,
- monorepo/workspace structure,
- existing Git hooks,
- existing CI quality commands only as optional reference; CI must not be required for the guard to work locally.

Do not replace an existing valid project tool merely because another tool is personally preferred. Adapt the guard to the declared stack and the project that actually exists.

If repository evidence conflicts with the explicit Tech Stack Contract, stop and ask for clarification rather than silently choosing one side.

## 2A. Bootstrap the Repository Into the Declared Stack

After the Tech Stack Contract is complete and repository inspection is finished, transform the repository from its current prototype/delivery shape into the declared stack's normal project structure.

This is the point where a temporary single-file prototype, static HTML demo, or other throwaway delivery shape stops being authoritative. The declared Tech Stack Contract and the stack's established conventions become authoritative for project structure.

Bootstrap work MAY include, when required by the declared stack:

- initializing the framework/runtime project structure,
- creating the package/workspace/module configuration required by the stack,
- installing only the dependencies required for the declared baseline stack and guard,
- creating framework-standard source, app, route, component, server, database, test, config, and asset directories as applicable,
- moving or adapting prototype assets/content into appropriate stack locations,
- creating the minimum application shell/entry point needed to prove the stack boots,
- configuring formatter, linter, type checker/compiler, test runner, build tooling, database/migration tooling, logger, and observability baseline when applicable,
- reconciling `AGENTS.md`, `.agents/`, architecture notes, and implementation plans with the bootstrapped stack.

The bootstrap MUST preserve the existing prototype/PRD/design as implementation reference, but it MUST NOT remain artificially constrained to one authored HTML file when the declared stack uses a real framework/project structure.

### Bootstrap-only boundary

Bootstrap work is intentionally narrower than implementation.

The agent MUST NOT during this command:

- execute the implementation plans it creates,
- implement the full product or complete feature slices,
- continue through all `.agents` plans,
- perform the full PRD-defined Playwright E2E suite,
- attempt to complete every user journey or production feature,
- treat a successful bootstrap as permission to keep coding until the application is finished.

Only the minimum verification needed to prove the bootstrap is healthy is allowed, for example:

- dependency/install sanity,
- formatter/lint/type/compile checks,
- guard checks,
- build/package sanity,
- framework start/boot sanity or one root-route smoke check when needed.

Do NOT run the product's full E2E completion gate at this stage. Full implementation and full E2E belong to the later implementation command after this bootstrap handoff.

## 2B. Create Implementation Plans Before Stopping

Before the bootstrap command ends, inspect the authoritative PRD/design/docs and create or reconcile concrete implementation plans under the project's `.agents/` planning structure.

The plans should describe the work that the implementation agent will execute later, including dependencies, sequencing, acceptance criteria, migrations, testing expectations, and relevant architecture boundaries.

Planning is required; plan execution is forbidden in this phase.

The bootstrap phase should leave the repository in a state where the next implementation command can simply read `AGENTS.md`, understand the repository rules and plans, then execute them.

## 3. Guard Contract

After the stack gate is open, repository inspection is complete, and the project has been bootstrapped into the declared stack, create or reconcile a project-local pre-commit guard appropriate for that stack **and target operating system/environment**.

The guard MUST be runnable locally without GitHub Actions, another hosted CI service, or access to a remote pipeline.

Prefer a small project-local executable entry point appropriate for the declared operating system:

```text
Linux/macOS/POSIX  -> ./scripts/pre-commit-guard
Windows            -> ./scripts/pre-commit-guard.ps1
```

A framework-native or task-runner wrapper is also acceptable when it is more idiomatic, as long as there is still one canonical local command that developers and agents can run before committing.

Use the declared target OS/environment to choose the implementation. For example:

- Linux/macOS/POSIX: shell or Bash script when appropriate,
- Windows: PowerShell or another native project-supported command,
- cross-platform projects: a portable task runner or thin OS-specific wrappers around the same checks.

Do not introduce GitHub Actions, GitLab CI, Jenkins, or another CI system merely to enforce this guard. If the repository already has CI, it may call the same local guard as an additional verification layer, but CI is optional and must not be the primary implementation.

The exact implementation may vary by stack, but **the policy below is mandatory**. The guard exits non-zero on any violation. A commit is not allowed until the local guard passes completely.

The generated guard must use commands and path semantics compatible with the declared target OS/environment. If portability is required, avoid OS-specific assumptions unless guarded/documented.

The guard itself must be documented in `AGENTS.md` and, when substantial, in `.agents/`.

## 4. Engineering Principles - Mandatory

All authored production code must follow:

1. Established industry and framework best practices.
2. **DRY** - avoid duplicated knowledge and repeated logic.
3. **SOLID** - keep responsibilities, abstractions, and dependencies well structured.
4. **KISS** - prefer the simplest correct design.
5. **YAGNI** - do not add speculative abstractions or features.
6. **Clean Architecture** - dependencies point inward; domain/business rules do not depend on delivery/infrastructure details.
7. Prefer reusable logic and reusable components over copy-paste variants.
8. Keep modules/files focused and bounded in size.

Do not apply these as dogma when a framework has an idiomatic equivalent. Preserve the intent and document the framework-specific mapping.

## 5. Architecture Boundaries

### Frontend / UI

Prefer this dependency direction, adapted to the framework:

```text
UI primitives
-> reusable components
-> layouts / composition
-> pages / routes / screens
-> application orchestration
```

Rules:

- Pages/routes compose; they should not become giant implementation files.
- Shared UI behavior belongs in reusable components/hooks/composables/services as appropriate.
- Business/domain logic must not be buried inside rendering templates.
- Do not duplicate formatting, validation, API, state, or interaction logic across pages.

### Backend

Prefer this dependency direction, adapted to the framework:

```text
entities/domain
-> repositories/interfaces
-> use cases / services
-> handlers / controllers
-> routers / modules / delivery
-> infrastructure implementations
```

Rules:

- Delivery code must not own business rules.
- Persistence details must not leak into domain/use-case logic.
- Repository interfaces and service/use-case boundaries must remain explicit where they add value.
- Avoid ceremonial layers that add no value; KISS/YAGNI still apply.

## 6. File Size / Complexity Guard

No authored source file may become a dumping ground.

The agent must determine stack-appropriate file-size and complexity limits and encode them into the guard. If the repository has no established limit, use these conservative defaults until a better project-specific rule is documented:

- UI component / page / route: **250 effective lines** maximum.
- Backend application/source module: **350 effective lines** maximum.
- Utility/shared module: **250 effective lines** maximum.
- Test/spec file: **500 effective lines** maximum.

Generated/vendor code is outside this authored-code policy and must be identified structurally, not through ad-hoc per-file bypass comments.

If a source file exceeds its limit, refactor it before commit. Do not silence the check.

The guard should also use the language/framework's supported complexity checks when available.

## 7. Strict Lint & Static Analysis - Zero Warning Policy

Configure the project's standard linter/static-analysis tooling to strict mode.

Mandatory policy:

- **0 lint errors.**
- **0 lint warnings.**
- **0 type-check/static-analysis warnings where the tool supports warning severity.**
- Do not commit with warnings because they are "harmless."
- Do not use inline disable/ignore/suppress directives to bypass authored-code violations.
- Do not weaken or disable a rule merely to make the guard pass.
- Do not add broad exclude patterns for authored source code.
- Fix the root cause.

Legitimate generated/vendor/tool-owned sources may be excluded through normal project configuration when they are clearly non-authored code. Record such structural exclusions in project documentation.

## 8. Build, Type & Test Gate

Before commit, run the strongest relevant checks supported by the stack:

- formatter/check-format,
- strict lint,
- type checking / compilation,
- unit tests,
- relevant integration tests,
- build/package validation where applicable.

Any failure blocks the commit.

## 9. Logging - No `console.*`

Use an industry-standard structured logger appropriate to the stack.

Mandatory policy:

- Production/application code must not use `console.log`, `console.error`, `console.warn`, print debugging, or equivalent ad-hoc console output.
- Logs are structured and machine-queryable.
- Use standard severity levels.
- Include stable event/context fields instead of concatenated free-form blobs.
- Never log secrets, credentials, raw tokens, or sensitive personal data.

The guard must detect forbidden console/debug logging patterns in authored source code.

## 10. End-to-End Traceability - Frontend to Database

A request/action must be traceable across the complete path when the architecture supports it:

```text
Frontend action/request
-> HTTP/API boundary
-> backend handler/controller
-> use case/service
-> repository
-> database operation
```

Use standard distributed-tracing concepts and tooling appropriate to the stack. Prefer W3C Trace Context / OpenTelemetry-compatible propagation when available.

At minimum:

- propagate a trace/correlation identifier across boundaries,
- include trace/correlation identifiers in structured logs,
- preserve context through async/background work where applicable,
- make database/query spans or equivalent operation context observable where supported,
- do not generate unrelated IDs independently at every layer.

The goal is that an operator can follow one user action/request from frontend through backend to persistence using logs/traces.

## 11. HTTP Standards

HTTP behavior must follow current relevant RFC semantics rather than project-invented conventions.

At minimum:

- Use HTTP methods according to their defined semantics and idempotency expectations.
- Use status codes according to HTTP Semantics (RFC 9110 and successors).
- Use headers, caching, content negotiation, redirects, and conditional requests according to relevant standards when applicable.
- Use a consistent standardized API error representation. Prefer RFC 9457 Problem Details for HTTP APIs when suitable.
- Do not return `200 OK` for failures merely to simplify clients.

The agent must map these rules to the framework/router used by the project.

## 12. 5xx Error Safety

Internal server failures must not expose implementation details to clients.

For 5xx responses:

- never expose stack traces,
- never expose SQL/database errors,
- never expose internal paths, dependency messages, infrastructure details, secrets, or raw exception text,
- return a safe generic client-facing error representation,
- include only a safe trace/correlation identifier that support/operators can use,
- write the full diagnostic context to the structured server logs/traces.

Expected/validated 4xx errors may include safe actionable detail for the client.

## 13. Security & Vulnerability Gate

The pre-commit guard must run the strongest locally practical security checks for the stack, including where supported:

- dependency vulnerability audit,
- static security analysis / SAST,
- secret scanning,
- unsafe dependency/config checks,
- framework-specific security checks.

Any unresolved high-confidence security finding blocks the commit. Do not suppress a finding without an explicit documented project decision and a real remediation/risk rationale.

The agent should also review common OWASP risks relevant to the changed code rather than relying only on automated scanning.

## 14. Reuse / Duplication Guard

Prefer shared abstractions when behavior or knowledge is truly repeated.

The guard should use available duplication/clone detection or framework analysis when practical. Even when automated clone detection is unavailable, the agent must review changed code for duplicated business logic, validation, API transformations, UI state patterns, and components before commit.

Do not abstract one-off logic prematurely merely to satisfy DRY. DRY applies to repeated knowledge, not superficial textual similarity.

## 15. `.agents` Freshness Gate

After every meaningful task, evaluate and update the project-local `.agents/` self-improvement layer before commit.

The guard/workflow must ensure the agent has considered whether the work produced:

- a durable memory,
- an updated plan,
- reusable knowledge,
- a reusable skill,
- review/follow-up notes,
- a needed update to `AGENTS.md`,
- or another useful agent artifact.

Do not create noise when nothing reusable was learned. When an update is warranted, use the project's timestamp naming rules.

## 16. Required Guard Pipeline

The generated guard should execute the applicable project checks in a deterministic order similar to:

```text
00 verify declared Tech Stack Contract + target OS/environment
01 repository sanity / forbidden bypasses
02 format check
03 strict lint / static analysis - zero warnings
04 type check / compile
05 architecture & dependency-boundary checks
06 file-size / complexity checks
07 reuse / duplication checks
08 unit + relevant integration tests
09 security / dependency / secret scans
10 logging policy checks
11 HTTP / error-contract checks
12 traceability instrumentation checks
13 .agents freshness / workflow verification
14 build/package validation
```

If the stack provides a better ordering, adapt it while preserving all applicable gates.

## 17. No Bypass Policy

The agent must not bypass the guard through:

- `--no-verify`,
- disabling pre-commit hooks,
- force-committing despite guard failure,
- linter ignore/disable comments,
- blanket warning suppression,
- reducing strictness,
- excluding authored code to avoid checks,
- deleting tests/checks,
- replacing a failing check with a weaker one.

If a guard rule is genuinely incorrect for the project, fix the rule transparently, document why, update `AGENTS.md` / `.agents` where appropriate, then rerun the complete guard.

## 18. Commit Rule

Before every commit:

1. Update relevant `.agents` artifacts when warranted.
2. Run the canonical pre-commit guard.
3. Fix every failure and warning.
4. Run the complete guard again from the beginning.
5. Commit only when the full guard exits successfully.

A partially passing guard is a failed guard.

## 19. Bootstrap Completion + Mandatory Stop

This bootstrap is complete only when:

- an explicit Tech Stack Contract was provided before stack/guard mutation,
- the declared target OS/environment is known,
- repository inspection was performed only after the stack gate opened,
- the repository has been transformed into the declared stack's appropriate project structure rather than remaining constrained to a temporary single-file prototype shape,
- the declared stack can install/build/boot at the minimum baseline level required to prove the scaffold is healthy,
- architecture boundaries and repository conventions are documented,
- concrete implementation plans exist under the project `.agents/` planning structure,
- `AGENTS.md` explains how later implementation agents must work in the repository,
- the canonical local guard command/script exists,
- its framework-specific implementation is documented,
- `AGENTS.md` requires the guard before every commit,
- strict linter/static analysis is configured,
- bypass/suppression detection is configured,
- architecture/file-size/reuse policies are encoded as far as practical,
- tests/build/security/logging/HTTP/error/trace checks are integrated as applicable,
- `.agents` update discipline is part of the workflow,
- the guard has been executed successfully against the bootstrapped baseline.

### HARD STOP

Once the conditions above are satisfied, **STOP**.

Do not execute the implementation plans. Do not continue feature development. Do not run the full product Playwright E2E completion suite. Do not attempt to finish the application.

The expected handoff is:

```text
Tech Stack Contract
-> inspect repository
-> bootstrap repository into declared stack
-> define architecture + AGENTS.md
-> create implementation plans under .agents/
-> create/configure local engineering guard
-> verify bootstrap baseline
-> STOP

NEXT COMMAND (separate phase):
Read AGENTS.md, understand and follow all repository rules, then implement the existing plan.
```

Implementation and full E2E verification begin only after the separate implementation handoff.
