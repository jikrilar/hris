# AGENTS.md — Professional HRIS

## 1. Project Context

Build the single-company Professional HRIS Phase 1 MVP as a modular monolith.
Keep exactly six business modules: Identity, Workforce, Scheduling, Attendance, Leave and Analytics.
Treat audit, notifications, files, exports, retention, recovery and demo operations as supporting infrastructure.
Exclude Phase 2 functionality, speculative extension scaffolding and unapproved product features.

Use Laravel/PHP 8.4, MySQL 8.4/InnoDB, Vue 3, strict TypeScript, Inertia.js, Tailwind CSS 4, shadcn-vue, TanStack Vue Table, Apache ECharts and VeeValidate/Zod.
Use Docker Compose, Pest, Playwright and GitHub Actions for the specified engineering workflow; use pen.dev for design and Codex CLI for authorized execution.
Take version families and compatible supporting dependencies from Architecture §6; take resolved patches/digests from verified lockfiles and images after bootstrap.
Do not assume application code, manifests, lockfiles, test runners, CI or design tools already exist.

## 2. Sources of Truth

Resolve these paths relative to the repository root and verify current versions, approval evidence and relevant source fingerprints.

| Work or decision | Required authority |
| --- | --- |
| Product scope, business rules, permission semantics, workflows and acceptance | `docs/phase-1/PRD.md` |
| Module ownership, dependency direction, technical design, transactions and security | `docs/phase-1/ARCHITECTURE.md` |
| Relational types, constraints, history, invariants and migration order | `docs/phase-1/DATABASE-SCHEMA.md` |
| Tokens, components, interaction, responsive behavior and accessibility | `docs/phase-1/DESIGN-SYSTEM.md` |
| Screen-specific visual composition | Approved frame/state/viewport/revision in `design/phase-1/professional-hris.pen` |
| Task authority, sequence, dependencies, branches, commits and execution gates | `docs/phase-1/TASK.md` |
| Agent behavior and operational discipline | This root `AGENTS.md` |

Use TASK §23 and Architecture §24 together for testing/release gates; use Design System §24–§26 for design handoff.
Keep each source within its authority. AGENTS and canvas references cannot redefine business, architecture or persistence contracts.
Report contradictions with exact source IDs/sections and use TASK §26 change control; do not invent a compromise or silently weaken a contract.
Preserve stable BR/OD/requirement/WF/AC/ADR/INV/DBD/UI IDs and existing approval history.

## 3. Agent Scope and Operating Principles

- Execute only the task or group explicitly selected by the project owner; do not select an arbitrary next task.
- Treat a selected task instruction as authorization for its documented bounded work and exact task commit once its gates pass.
- Continue within an explicitly authorized group in its documented order only; stop at the selected scope.
- Apply existing authorization without repeatedly asking about routine implementation choices; verify separate merge, publishing, deployment and release authority before those actions.
- Inspect existing instructions, implementation, components and tests before creating another solution. Reuse established conventions and focused abstractions.
- Keep changes within the task's allowed files. Avoid unrelated refactoring, formatting, dependency additions and generated scaffolding.
- Preserve unrelated user work and private data. Make no claim of approval, completion or passing checks without actual evidence.
- Keep one root instruction file. Create nested agent guidance only through a separately authorized task with a concrete need.

## 4. Scoped Context Loading

Follow TASK §24.1. Do not repeatedly load all 280 task specifications or entire source documents into active context.

1. Read applicable repository instructions and TASK global rules (§1–§9, §23–§28); retain a compact, referenced working summary.
2. Locate the exact task anchor using `rg -n`; read its complete block through the next task/group/top-level section boundary.
3. Read the parent group's metadata, acceptance, ordered inventory, exact branch, Git command blocks, PR requirements and merge gates wherever located.
4. Read direct dependency entries and relevant §11–§12 rows; verify actual commits, prerequisite outcomes and preceding group merge/main evidence.
5. Read every active-task subtask, allowed file, acceptance criterion, check, Definition of Done and exact planned commit subject.
6. Read the cited current source passages, relevant §20–§22 traceability rows, approved design references and existing owned contracts/code/tests.
7. Expand into neighboring or cross-module sections whenever authorization, history, data integrity, transactions or an ambiguity requires it.

Track loaded references/fingerprints, the selected task/group, branch, dependency evidence, applicable gates and blockers in session context.
Refresh changed or affected slices after a revision, task switch or context loss; use current source inspection rather than memory or summaries as authority.
Read a truncated task/source passage completely before relying on it. Do not replace a detailed task with an inventory row.
Perform any comprehensive audit explicitly required by the task; scoped reading limits repeated loading, not verification depth.

## 5. Task Execution Lifecycle

Preserve Phase → Group → Task → Subtask and the approved 7 phases, 33 groups, 280 tasks and 2,383 subtasks until a deliberate reviewed plan amendment.
Keep one active group and one active task. Do not execute tasks in parallel or skip a predecessor.

- **Before changes:** identify task/group/authority; inspect repository status, exact branch/base, source versions, allowed files, existing implementation and task-specific tests.
- Verify all required predecessor commits and evidence. At a group boundary, verify the previous PR actually merged and integrated main passed required checks.
- Verify current source/design approvals, real backend collaborators, test resources and progressive tooling availability; establish a bounded approach.
- **During changes:** execute subtasks in order, reuse existing solutions, implement only authorized behavior and add meaningful applicable tests.
- Preserve module, authorization, transaction, historical/economic and private-evidence contracts; do not expose successful production stubs for missing collaborators.
- **After changes:** run all applicable gates, inspect results and acceptance, review the bounded diff and staged files, and check for secrets/private data.
- Record real task status/evidence in the same atomic task commit under TASK §6/§9/§27; report limitations and the next eligible task without starting it.
- Leave failed or unverified required work incomplete. On a blocker, stop affected work and use §15 below.

Keep G-000 mandatory: verify Architecture owner-acceptance evidence, review the attendance state contract and refresh the necessary Schema references through its exact tasks.
Do not infer Architecture acceptance, resolved late/early/inconsistent attendance representation or approved screen revisions from plan approval.
If no initialized owner-selected repository/main exists, report the execution-entry prerequisite before branching; do not initialize a repository or create a hidden genesis commit.

## 6. Git and Commit Protocol

- Protect `main`. Branch each group from updated, verified main only after its predecessor group PR has merged.
- Use the exact group branch and exact task-specific Conventional Commit subject in TASK; do not invent or rename them.
- Keep one group = one branch; one task = one coherent atomic commit; all subtasks/tests/tracking changes belong to that task commit.
- Prepare exactly one PR per group under the authorized scope. Integrate through a reviewed merge commit preserving task commits; do not squash.
- Treat the merge commit as an integration record outside the 280 task commits. Verify integrated main before the next group branch.
- Obtain the stated CI/owner review and actual action authorization before merge/deployment/release. Group authorization does not silently grant these actions.
- Never combine tasks, add status-only commits or amend accepted commits to insert their own SHA. Record resulting SHAs in PR/execution metadata.

Inspect these commands in the actual repository before editing or committing; inspect relevant configuration and the verified remote without exposing credentials:

```sh
git status --short --branch
git branch --show-current
git log -1 --oneline
git diff --check
git diff --stat
git diff --cached
```

Stage only intended files; preserve unrelated modifications and branch history. Follow the selected group's commands for updating main and opening its exact branch.
Never silently run `git reset --hard`, `git clean -fd`, destructive checkout/branch deletion, force push or history rewriting; require explicit authorization for that destructive operation.
Save, verify and close the `.pen` asset before branch changes; resolve design conflicts through the supported editor while preserving identifiers.

Treat AGENTS as a pre-execution instruction artifact, not another planned task commit or completion of G-000/T-0001.
Include it in an owner-approved initial repository baseline only when that baseline is authorized.
If task history already exists, adopt it through the approved Git/change-control process; do not create an out-of-band implementation commit, renumber tasks or modify protected main directly.

## 7. Architecture and Module Boundaries

- Follow Architecture §8–§10's module import DAG and public contracts. Keep authoritative state in its owning module.
- Use immutable typed DTOs across module boundaries; never return mutable Eloquent models or cross-owned save/delete relationships.
- Keep Application workflows above module contracts. Modules must not import workflows, HTTP adapters or jobs.
- Keep Attendance and Leave independent; coordinate their shared conflicts through a real application workflow using owned read/command contracts.
- Keep Shared free of module-implementation imports; bind ports through service providers and avoid recursive authorization/context reads.
- Invoke Analytics invalidation through the committing workflow; source modules must not import Analytics implementations.
- Avoid a generic repository per model, interface per class, god service, workflow engine, event broker or modular framework.

## 8. Backend Conventions

- Keep controllers thin; use Form Requests for input shape, Laravel Policies and the selected scoped authorizer for access, focused Actions for use cases and domain services for calculations.
- Let application workflows own cross-module transactions. Make module Actions join the same transaction without an independent commit or remote side effect.
- Keep Queries scoped and read-only; project explicitly authorized DTO fields before Inertia/JSON/CSV serialization.
- Revalidate permissions and mutable domain facts at the committing boundary; an early Form Request authorization check is insufficient.
- Make Laravel authoritative for eligibility, hierarchy, schedules, attendance, leave economics, approval transitions, metrics and sensitive fields.
- Pass time/context explicitly to deterministic calculations. Carry actor, generation, correlation, intent and expected version where required.
- Use the approved typed outcomes and 422/stale/auth/retry semantics; return success only for a committed outcome and support authenticated intent recovery.
- Keep jobs/commands as adapters around owned workflows/queries, not alternate business implementations.

## 9. Database and Migration Safety

- Follow Schema §5/§20/§22–§24 and the exact §23.1 table dependency order, including Laravel-native driver contracts.
- Preserve declared MySQL types, precision, collation, FK/unique/CHECK/generated-column definitions and stable coordination anchors.
- Distinguish database-enforced constraints from transactional application guards; do not weaken either for convenience.
- Preserve effective-dated context, UTC/business-local meaning, immutable original evidence, economic lineage, source revisions and dataset generations.
- Use exact four-decimal leave quantities and prescribed rational cumulative arithmetic; never store or calculate authoritative balances with binary floats.
- Retain protected history and references; do not introduce cascading deletion, guessed historical backfills or destructive rollback of committed evidence/economics.
- For a schema incompatibility, identify exact tables/constraint IDs, verify the selected MySQL capability, prepare the smallest correction and follow source review before dependent changes.
- Before `migrate:fresh`, `db:wipe`, truncation, reset, purge, restore or load tests, verify explicit disposable environment/database/dataset/private-volume identity. Abort on unknown or real-data targets.
- Run retention/recovery only through their authorized workflow and current approval/hold/control-journal gates; a preview or approved schema is not deletion authorization.

## 10. Authorization and Security

- Enforce the PRD's dynamic multi-role RBAC: union employee sets only within the same permission; intersect all distinct required permissions.
- Resolve `all`, `team` and `self` exactly; use current reporting relationships for access and historical context only for business interpretation.
- Apply separate sensitive-field permissions to the same target. Allow no implicit role hierarchy or unconditional Super Admin bypass.
- Authorize commands, rows, counts, search/facets, aggregates, Inertia props, files, exports and stage decisions before disclosure or mutation.
- Treat capability flags and hidden controls as presentation only. Omit restricted fields from payloads, tooltips, charts, logs and exports.
- Recheck current authority for asynchronous exports at execution/chunks, publication and download; never use a historical report snapshot as current authority.
- Enforce private-file ownership, state and generation with protected routes; do not bypass revocation through public or long-lived URLs.
- Preserve session/CSRF/invitation/reauthentication and account/employment eligibility contracts; do not expose public registration or a demo privilege switch.
- Keep credentials, tokens, raw GPS, personal evidence and prohibited sensitive payloads out of logs, fixtures, designs, Git and reports.
- Use synthetic development/test fixtures. Preserve OD-21's separate validation gate before real-data use and OD-16's category/version approvals and holds.

## 11. Concurrency and Business Invariants

- Use one MySQL connection and READ COMMITTED business transaction; use separate read-only REPEATABLE READ reporting snapshots with fresh authorization reads.
- Follow Architecture §17 and Schema §22: company fence → sorted principal rows → sorted employee anchors → receipt → Scheduling → Attendance → Leave request/stage → Leave scopes/buckets → evidence/fact/audit/outbox/result.
- Select SHARE/X company-fence mode before locking; never upgrade it in place. Recheck facts under locks and restart with the complete set if affected resources expand.
- Lock existing employee anchors before absence/overlap/capacity checks. An empty result, missing row or disabled button does not serialize competing inserts.
- Bind idempotency to canonical intent, actor/target and generation; reject changed-payload reuse. Preserve expected versions and uniqueness of business/economic effects.
- Retry the entire transaction only under the bounded source policy, with the same intent and fresh facts; resolve uncertain outcomes before replaying effects.
- Commit critical business changes, economic effects, audit, source-dirty marker, outbox obligation and result atomically as specified.
- Keep raster decoding, network calls, CSV formatting and large file operations outside business locks. Filesystem/SMTP delivery is not atomic with MySQL.
- Distinguish committed business results, queue publication and completed delivery; preserve leased/idempotent outbox recovery and visible delivery failures.
- Preserve genuine attendance evidence and trusted receipt timing before decode/command work. Do not fabricate clock-out, extend offboarding boundaries or erase unknown/inconsistent classifications.
- Reserve all required leave periods/buckets atomically from posted availability; preserve exact rounding, original expiry/carry/reversal lineage and once-only economic effects.
- Enforce stage order, actual-actor independence, current delegation and only documented exceptions; never permit requester self-approval or cascading multi-stage decisions.
- Protect schedule publication against all affected protected history/conflicts. Enforce cross-module invariants synchronously, not through eventual repair events.
- Keep one authoritative metric calculator and versioned facts with source/access fingerprints; preserve provisional/reconciled/N/A meaning and report/drill/CSV consistency.
- Keep synthetic demo operation behind both deployment and dataset guards; public visitors must not supply real camera/GPS evidence or trigger privileged administration.

## 12. Frontend Conventions

- Use Vue SFCs with Composition API/script setup, strict TypeScript, explicit props/emits and typed server DTOs; compose Inertia pages from existing layouts/features.
- Keep UI-only drafts, dialogs, camera previews and interactions local; reuse composables. Do not add a parallel API application or global state library without reviewed need.
- Reuse reviewed shadcn-vue/Reka primitives and shared form/table/chart wrappers; do not rebuild a competing component system.
- Use Design System semantic indigo/slate tokens, Inter typography, approved light MVP layouts and status/accessibility conventions; avoid per-module palettes or arbitrary styles.
- Use TanStack's approved manual server pagination/sorting/filtering. Never fetch unrestricted history for client filtering or let hidden columns leak fields.
- Render ECharts from backend series/metrics and report tokens; never recompute authoritative KPIs, leave balances or permission sets in Vue.
- Keep VeeValidate and its Zod adapter/version family aligned with Architecture §6; use client validation for feedback and always honor Laravel validation/domain outcomes.
- Preserve recoverable drafts and distinct loading/empty/denied/stale/blocked/unknown/committed states; retain non-color indicators, keyboard/focus and responsive/reflow behavior.
- Reuse the same intent for a network retry; use a new intent for a genuine retake. Release camera tracks, object URLs, listeners and chart observers on navigation/unmount.
- Never call visitor camera/geolocation APIs on public synthetic pages. Keep business eligibility and genuine/private evidence rules server-authoritative.

## 13. pen.dev Design Workflow

Use `design/phase-1/professional-hris.pen` as the single canonical asset under Design System §24; preserve its foundation/component/shell/module groups and all 44 screen IDs.

1. Verify the selected design task/group, relevant UI IDs, source rules, tokens and existing frames/components.
2. Inspect the actual supported pen.dev editor/MCP/CLI and required running host; do not guess tool methods or installed capabilities.
3. Author only the selected frames/states/viewports, reusing variables and foundations and preserving established identifiers.
4. Save and reopen the asset; inspect affected frames and perform the required visual, responsive, keyboard/state and accessibility review.
5. Record the exact file/frame or actual node ID, saved revision/Git reference, covered states/viewports, source trace and actual owner decision.
6. Before Vue implementation, verify the required concrete approved frame/revision and real backend contracts; map existing reusable components and typed props first.

Keep Draft/In review/Approved/Superseded evidence truthful. Approval of this file or the Design System does not approve a screen.
Do not hand-edit serialized object IDs, mechanically repair JSON conflicts, duplicate reusable tokens or infer acceptance from a screenshot.
Treat exports/generated HTML/React as reference material; translate into maintainable Vue/Inertia components rather than assuming production-ready Vue export or copying canvas coordinates.
Keep design asset changes inside the selected group branch and task commit. Record material approved deviations through the established handoff/change process.

## 14. Tests and Quality Gates

Consult the active task, TASK §23 and Architecture §24 for exact commands, selectors, progressive activation and task/PR/main/release obligations.
Inspect manifests, scripts, services and runner configuration first. Before bootstrap, report unavailable commands as not yet applicable with their named activation/follow-up task; never invent a pass or waive an already-applicable failure.

Use these representative commands only after the corresponding tooling exists and the selected gate requires them; this is not a mandatory bundle for every task:

```sh
vendor/bin/pint --test
vendor/bin/phpstan analyse --memory-limit=512M
php artisan test
npm run type-check
npm run lint:check
npm run build
npx playwright test
docker compose config --quiet
```

- Run focused meaningful Pest unit/feature/architecture and relevant MySQL/Playwright checks with the implementation; run full suites when the gate requires them, not routinely after every small change.
- Verify module boundaries, scoped field omission, failure/rollback paths and required history/economic invariants as applicable.
- Prove races on real MySQL 8.4 using independent committed connections/processes and deterministic barriers; SQLite, mocks or an invisible outer test transaction are insufficient.
- Exercise real private codecs/volumes and queue/outbox recovery where those boundaries are under test; fakes cannot prove their durability or performance.
- Keep browser visual fixtures deterministic and compare exact approved references. Do not blindly accept snapshots, disable checks or conceal unrelated failures.
- Respect dependency/platform/peer/secret audits and GitHub Actions PR/main/release gates. Do not use forced installs to hide compatibility failures.
- Run benchmark, genuine-device, security, retention, demo-reset and isolated recovery evidence only at their applicable authorized gates; do not claim targets from documentation or mocks.
- Report executed commands and observed outcomes, including failures and genuinely unavailable checks. Preserve sanitized evidence tied to actual code/configuration.

## 15. Change Control and Blockers

- Stop affected work on missing prerequisites, unavailable tools/infrastructure/credentials, failed gates, ambiguous product meaning, unapproved designs or source contradictions.
- Preserve user work; identify task/group, exact source IDs/sections, the blocker category, observed evidence and the smallest concrete resolution.
- State which owner decision is actually missing; do not repeat approval already supplied. Prepare a reviewable proposal before requesting a material contract change.
- Require the documented review for changes to scope/business/permissions, leave accounting, attendance evidence/time, historical meaning, material architecture, destructive persistence, approved design references, significant dependencies or release/security guarantees.
- Choose routine names, helpers and styling/composition details within approved contracts without repeated confirmation.
- Follow TASK §26 for deliberate plan amendments; preserve accepted IDs/history and update affected mappings only through that process.
- Do not silently edit an approved source, weaken a constraint, substitute a successful production stub, conceal an environment problem or skip to an unrelated task.
- Keep the task BLOCKED/incomplete until its actual prerequisite or failure is resolved; internal planning does not replace TASK or authorize extra work.

## 16. Completion Reporting

Report concisely using these fields; do not repeat the whole task checklist:

- **Task:** actual ID and title.
- **Group / branch:** actual group and exact branch.
- **Changes:** concrete results and relevant behavior.
- **Verification:** commands executed, observed outcomes and not-yet-applicable checks with activation/follow-up references.
- **Git:** exact planned/created subject, actual SHA if created and actual PR/merge state where relevant.
- **Risks / blockers:** unresolved issues and limits.
- **Status:** evidence-backed task/group state.
- **Next:** next dependency-eligible task, without automatically starting it.

For design, include `.pen` file/frame/revision and approval coverage; for backend, include critical integrity tests; for group completion, include actual PR/merge/main evidence.
Never report an opened PR as merged, generated output as owner approval or an unexecuted test as passed.

## 17. Final Self-Check

- Verify authorized scope, exact task/group/branch and satisfied dependencies; stop at the selected boundary.
- Verify current source/design authority, module ownership, current authorization and applicable history/data/economic/evidence invariants.
- Verify every task acceptance condition, meaningful applicable tests and actual quality-gate outcomes.
- Review the diff and staged files; preserve user work and exclude secrets, private data and unrelated changes.
- Verify the exact atomic task commit and truthful status/evidence tracking; do not claim a commit, approval or merge that did not occur.
- Report real results, limitations and the next eligible task; leave unresolved required work incomplete.
