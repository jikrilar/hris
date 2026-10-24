# Professional HRIS — Phase 1 Product Requirements Document

## 1. Document Information & Status

| Attribute | Value |
| --- | --- |
| Product | Professional HRIS (Human Resource Information System) |
| Phase | Phase 1 — MVP |
| Canonical repository path | `docs/phase-1/PRD.md` |
| Document version | 1.0 — Approved Requirements Baseline |
| Date | 22 October 2026 (Asia/Jakarta) |
| Status | Approved Requirements Baseline |
| Operational approval date | 22 October 2026 |
| Initial operational approval | 15 decisions approved without revision by the product owner on 22 October 2026 |
| Final operational approval | OD-03, OD-08, OD-12, OD-14, OD-16, and OD-20 approved in their revised form by the product owner on 22 October 2026; all 21 Operational Decisions are APPROVED |
| Product decision owner | Project owner / developer |
| Intended readers | Product reviewers, architects, designers, developers, testers, and Codex CLI |
| Source | Existing PRD; product-owner batch approval, targeted revisions, and final approval dated 22 October 2026; BR-01 through BR-28 remain authoritative |
| Implementation status | Not implemented or validated by this document |

### 1.1 Requirement authority

- **CONFIRMED** means a requirement follows the 28 locked business decisions. The word **must** is normative for these requirements.
- **APPROVED** means the product owner approved the operational definition. All OD-01 through OD-21 are binding as of 22 October 2026, including their thresholds, formulas, permissions, defaults, and the final revised definitions of OD-03, OD-08, OD-12, OD-14, OD-16, and OD-20.
- An OD reference provides traceability to an approved operational rule. No Operational Decision in this baseline awaits further owner sign-off.
- **IMPLEMENTATION CHOICE** means architecture or development tasks may choose a mechanism while preserving required outcomes. It does not authorize a product-rule change.

Locked business decisions take precedence over operational definitions. No conflict may be resolved by weakening a locked requirement. This PRD is the Approved Requirements Baseline. Approval does not waive runtime workflow guards, per-category retention-policy validation, testing/release gates, or OD-21's legal/privacy validation before real-data use; it does not establish that implementation, regulatory compliance, or performance targets have been validated.

### 1.2 Change and downstream documentation control

Requirement IDs are stable. Do not renumber existing IDs when adding requirements. Record approved policy decisions in this document before changing dependent architecture, schema, designs, tasks, or tests. Product-owner approval is required for changes to scope, locked rules, or major architecture decisions.

Only this PRD is delivered now. Later documents are `ARCHITECTURE.md`, `DATABASE-SCHEMA.md`, `DESIGN-SYSTEM.md`, `TASK.md`, `SIT.md`, and `UAT.md`, under `docs/phase-1/`. They must reference these requirement IDs. This PRD deliberately does not define a complete schema, migrations, package versions, component implementation, or application directory structure.

## 2. Executive Summary

Professional HRIS is a single-company workforce operations application and a professional Fullstack/Backend Developer portfolio. Its MVP connects identity and authorization, employee history, scheduling, attendance, leave, and operational analytics. It must demonstrate reliable Laravel business logic, MySQL relational integrity, a Vue/Inertia interface, and reproducible quality evidence.

The core product promise is that employee operations remain consistent across modules: a schedule determines attendance and leave calculations; approval does not overspend a leave balance; corrections retain original evidence; and every view, report, background export, and file download respects the same authorization boundaries.

Exactly six primary modules are included. Audit, notifications, queues, retention, recovery, and observability support those modules. Payroll, overtime, employee performance management, and recruitment are Phase 2 extensions. A public demonstration uses synthetic data and restricted administrative capabilities.

This Approved Requirements Baseline preserves all 28 locked business decisions and records final product-owner approval of all 21 Operational Decisions on 22 October 2026. The last six revised definitions are approved without further business changes. Section 24 records approval; subsequent architecture, schema, design, implementation, and testing must conform to this baseline.

## 3. Product Background and Problem Statement

The developer previously maintained an existing employer HRIS, added camera-based attendance, and optimized attendance SQL and indexing. Processing time improved from approximately three seconds to approximately one second in that system's operating conditions. That experience informs the problem selection; it is not evidence of this new product's performance.

Professional HRIS is independently developed. It must not reuse proprietary source code, internal documents, confidential workflows, or real employee data without authorization. Portfolio datasets, examples, screenshots, and the public demo must be synthetic.

Everyday workforce operations create connected failure risks: separate employee and account statuses, changes in reporting lines, shifts crossing midnight, conflicting leave and attendance, delayed approvals, and reports that disagree with underlying records. The application must make these relationships explicit, preserve effective-dated history, and provide authorized resolution instead of silent data changes.

## 4. Product Vision and Objectives

| Objective | Evidence required for MVP |
| --- | --- |
| Deliver usable workforce operations | The six module workflows complete end to end using synthetic employees, including employees without accounts |
| Demonstrate Laravel and relational engineering | Server-side authorization, transactional commands, history preservation, constraints, and meaningful Pest tests |
| Demonstrate modern frontend engineering | Vue 3 + TypeScript through Inertia; validated forms, accessible tables, charts, and clear workflow states |
| Demonstrate correctness under contention | Duplicate clocking, concurrent reservations, concurrent approvals, and schedule conflicts pass concurrency tests |
| Demonstrate measured performance | Reproducible benchmark at the BR-28 workload; targets are reported as measured pass/fail results |
| Demonstrate operational readiness | Docker deployment, functioning demo, monitored jobs, backup/restore evidence, and release checks |
| Preserve extensibility | Phase 2 can consume stable employee, schedule, attendance, leave, permission, and audit concepts without replacing the MVP |

No commercial adoption, revenue, uptime percentage, or unmeasured scaling claim is an MVP success criterion.

## 5. Target Users and Personas

| Persona | Primary needs | Key boundary |
| --- | --- | --- |
| Super Admin | Configure roles, permissions, approval policies, company settings, and sensitive safeguards | Broad permissions do not bypass self-approval, audit, or business invariants |
| HR Admin | Maintain employee history, schedules, attendance exceptions, leave policies, and workforce reporting | Cannot grant itself security administration or bypass approval stages |
| Manager | View permitted team records, review assigned leave, and inspect team attendance | Team means active direct and indirect subordinates, not everyone in the department |
| Employee | Access own profile, schedule, attendance, leave balance, requests, and notifications | Can act on their own records only where the action permits it |

These are default role names, not hardcoded authorization branches. Custom roles reuse the permission catalog. One user may hold multiple roles. A reporting manager may exist as an employee without an active account; that does not make them a valid online approver.

## 6. Goals, Non-Goals, and Scope

### 6.1 Exactly six MVP modules

| Module | Included capabilities |
| --- | --- |
| Authentication & Authorization | Invitations, activation, session login/logout, reset, account deactivation, dynamic roles, per-permission scopes |
| Employee & Organization Management | One company, department hierarchy, positions, locations, reporting lines, separate employee/account identity, effective-dated employment history |
| Shift Management | Templates, weekly recurrence, rotation, dated assignments, individual overrides, holiday calendar, overlap checks, preserved schedule context |
| Attendance Management | Camera photo clock-in/out, server time, configurable geolocation/geofence, policy validation, exceptions, corrections, reconciliation |
| Leave Management | Configurable types and policies, allocation/accrual, ledger/reservations, shift-aware duration, sequential approval, delegation/escalation, cancellation |
| Dashboard & Analytics | Scope-aware KPIs, line/bar/donut views where useful, calendar views, filters, drill-down, consistent CSV reporting |

Supporting capabilities are audit, email/in-app notifications, background jobs and scheduler, secure files, data integrity, retention/recovery, observability, deployment, and testing. They are not additional product modules.

### 6.2 Non-goals and Phase 2 boundaries

- No payroll computation, payslips, tax calculations, deductions, or holiday-work compensation.
- No overtime requests, approvals, rates, or overtime balance calculations. Hours outside a shift may be attendance evidence; they must not be labeled approved overtime or compensation.
- No employee performance KPI targets, appraisal, scoring, or reviews. Dashboard KPIs describe operations.
- No recruitment, applicant records, vacancies, interview pipelines, or ATS.
- No multi-company tenant isolation, subscriptions, billing, or customer onboarding in MVP.
- No facial recognition, biometric matching, continuous location tracking, or claim that a photograph/GPS reading proves identity.
- No microservices requirement, separate frontend deployment, page-by-page REST API, WebSocket requirement, general document-management module, or backup-management product.

## 7. Locked Stack and Architecture Constraints

| Layer | Confirmed technology |
| --- | --- |
| Backend | Laravel |
| Frontend | Vue 3 and TypeScript |
| Integration | Inertia.js |
| Database | MySQL |
| CSS / components | Tailwind CSS / shadcn-vue |
| Tables | TanStack Vue Table |
| Charts | Apache ECharts |
| Forms | VeeValidate + Zod |
| Client state | Vue composables; Pinia only with documented justification |
| Infrastructure | Docker Compose; containerized deployment |
| Backend / E2E testing | Pest / Playwright |
| CI/CD | GitHub Actions |
| Architecture | Modular Monolith |
| Design / coding tools | pen.dev / Codex CLI |

Laravel owns routes, session authentication, authorization, authoritative validation, and business logic. Vue provides the Inertia interface; client validation is supplementary. The application serves one company with multiple departments, positions, organizational levels, branches, and work locations. Package versions and compatibility must be verified when implementation begins; this PRD does not assume specific versions.

Module boundaries must keep business ownership explicit while allowing atomic MySQL transactions where a cross-module invariant requires them. Mechanisms for events, caching, queues, and private storage are architecture choices unless constrained below. No Redis, cloud provider, permission package, or queue driver is mandated here.

## 8. Roles, Permissions, and Data Access Requirements

### 8.1 Scope vocabulary and invariants

`all` means all applicable company records, subject to field visibility and business guards. `team` means employees reachable through the actor's current direct or indirect reporting relationships; it does not include the actor automatically. `self` means records linked to the actor's employee identity. An actor with no employee link has an empty `self` and `team` set. Roles do not inherit from one another.

Historical attendance, leave, and approvals retain their historical organizational context. **Access checks use the active reporting hierarchy at the time of access**, not an employee's old manager. Historical report grouping and current access are different concerns. Removing a role or changing a reporting line must affect subsequent authorization checks, including queued exports and file downloads.

### 8.2 Complete MVP permission catalog and approved default grants

**CONFIRMED:** the following action boundaries must be enforceable. **APPROVED [OD-01]:** the exact permission keys, default grants, grant-only resolution model, and global/owner-only classification below were approved on 22 October 2026. The catalog is frozen for implementation subject to controlled future changes.

Legend: **A** = `all`; **T** = `team`; **S** = `self`; **G** = global configuration capability with no employee scope; **O** = authenticated user's own resource; **—** = no grant. `T+S` is the union of grants for that same permission. A global capability never confers employee-data access. Every row is an explicit grant; no role inherits the Employee role. Super Admin has the entries shown rather than an unbounded bypass. Business guards always apply.

| Permission key | Allowed scope | Super Admin | HR Admin | Manager | Employee |
| --- | --- | --- | --- | --- | --- |
| `account.view` | A/T/S | A | A | S | S |
| `account.invite` | A | A | A | — | — |
| `account.deactivate` | A | A | A | — | — |
| `account.reactivate` | A | A | A | — | — |
| `role.view` | G | G | — | — | — |
| `role.manage` | G | G | — | — | — |
| `role.assign` | G | G | — | — | — |
| `organization.view` | G | G | G | G | G |
| `organization.manage` | G | G | G | — | — |
| `employee.view` | A/T/S | A | A | T+S | S |
| `employee.sensitive.view` | A/T/S | A | A | S | S |
| `employee.create` | A | A | A | — | — |
| `employee.update` | A/T | A | A | — | — |
| `employment.manage` | A/T | A | A | — | — |
| `employee.history.view` | A/T/S | A | A | T+S | S |
| `employee.export` | A/T/S | A | A | — | — |
| `shift.view` | A/T/S | A | A | T+S | S |
| `shift.template.manage` | G | G | G | — | — |
| `shift.assign` | A/T | A | A | — | — |
| `shift.override` | A/T | A | A | — | — |
| `calendar.view` | G | G | G | G | G |
| `calendar.manage` | G | G | G | — | — |
| `attendance.view` | A/T/S | A | A | T+S | S |
| `attendance.photo.view` | A/T/S | A | A | — | S |
| `attendance.clock` | S | S | S | S | S |
| `attendance.correction.request` | S | S | S | S | S |
| `attendance.correction.review` | A/T | A | A | — | — |
| `attendance.resolve` | A/T | A | A | — | — |
| `attendance.policy.manage` | G | G | G | — | — |
| `attendance.reconcile` | A/T | A | A | — | — |
| `attendance.export` | A/T/S | A | A | T | — |
| `leave.view` | A/T/S | A | A | T+S | S |
| `leave.sensitive.view` | A/T/S | A | A | S | S |
| `leave.request` | S | S | S | S | S |
| `leave.cancel.request` | S | S | S | S | S |
| `leave.approve` | A/T | A | A | T | — |
| `leave.cancel.review` | A/T | A | A | — | — |
| `leave.policy.manage` | G | G | G | — | — |
| `leave.balance.view` | A/T/S | A | A | S | S |
| `leave.balance.adjust` | A/T | A | A | — | — |
| `leave.balance.process` | G | G | G | — | — |
| `leave.workflow.manage` | G | G | — | — | — |
| `leave.delegation.manage` | A/T/S | A | A | S | — |
| `leave.reassign` | A/T | A | A | — | — |
| `leave.export` | A/T/S | A | A | T | — |
| `dashboard.view` | A/T/S | A | A | T+S | S |
| `analytics.export` | A/T/S | A | A | T | — |
| `notification.view` | O | O | O | O | O |
| `notification.update` | O | O | O | O | O |
| `audit.view` | G | G | — | — | — |
| `retention.manage` | G | G | — | — | — |
| `operations.view` | G | G | — | — | — |
| `operations.retry` | G | G | — | — | — |
| `demo.reset` | G | G | — | — | — |

Login, logout, activation, password reset, and changing one's own password are authentication flows, not configurable grants that permit acting as another user. An account viewing itself does not expose password hashes, tokens, or another user's account security data. Notification ownership cannot be delegated through custom roles.

Sensitive fields require both the ordinary view permission and the matching sensitive-view permission for that employee. Photos require `attendance.view` and `attendance.photo.view`. History requires `employee.view` and `employee.history.view`. Download/export permission is additional to the relevant read permissions, never a replacement. Organization/calendar reads expose necessary reference data, not embedded private employee lists.

### 8.3 Approved permission resolution algorithm [OD-01]

1. Authenticate an enabled account; apply employment eligibility where required by the action. Load current role grants and current reporting relationships.
2. Resolve the **exact requested permission**. With no grant, deny. A missing grant in one role is not an explicit deny against another role's grant. No explicit-deny or implicit inheritance mechanism is included in MVP.
3. For that permission only, union all granted record sets: `all ∪ team = all`; `team ∪ self` includes the team and actor; `self ∪ self = self`. Never choose one global scope across unrelated permissions.
4. Intersect the result with company boundaries, the requested filters, field permissions, and every additional required permission's record set. Filters may narrow access, never broaden it.
5. Apply immutable workflow guards: no self-approval, valid current stage, valid approver assignment, eligible employment, valid time window, sufficient balance, and nonconflicting state. Super Admin and multiple roles do not override these guards.
6. Enforce the same check in lists, detail routes, mutation commands, counters, charts, search options, files, queued job execution, export generation, and export download. Reject stale writes and record security-relevant denials without sensitive payloads.

Example: `employee.view=all` plus `leave.approve=team` permits company employee viewing and only assigned team leave approvals. It does not permit company-wide leave approval. Adding `leave.view=self` does not make team leave details visible; the reviewer also needs `leave.view=team`. Being a designated delegate does not manufacture missing permissions or expand scope.

Approved OD-14 adds default distinct-stage approver checks without changing this approved permission resolution. Approved OD-20's public read-only role comparison is a sanitized explanatory view of configured permissions, not a grant of `role.view`, `role.manage`, or `role.assign` to demo accounts. The approved matrix above remains unchanged; actual role administration is demonstrated in a private/local owner-controlled environment.

### 8.4 Authorization requirements

| ID | Requirement | Verifiable acceptance |
| --- | --- | --- |
| AUTH-001 | Support invitations, activation, session login/logout, password reset, and account deactivation independently of employee lifecycle. | Expired/used invitations cannot activate; disabled accounts cannot authenticate or continue an existing session. Token/session values follow approved OD-02. Under approved OD-03, an open attendance session never grants post-offboarding login/clock-out access or automatic employment extension. |
| AUTH-002 | Super Admin can create custom roles and configure action permissions with permitted scopes through the UI. | An allowed custom role can be saved and used; an unsupported scope or unknown permission is rejected server-side. Approved OD-20 permits a public read-only comparison only; role mutations remain blocked on the backend in public demo. |
| AUTH-003 | A user may hold multiple roles without implicit inheritance; evaluate permission-specific effective access. | The example in Section 8.3 passes; a broad unrelated grant does not change a narrower grant. The approved algorithm in OD-01 applies. |
| AUTH-004 | Enforce action, record scope, and field visibility on the backend for every access path. | Direct URLs, forged IDs, Inertia payloads, filters, chart drill-downs, private file requests, and exports cannot expose unauthorized records or fields. |
| AUTH-005 | Prevent privilege escalation and preserve administrative recovery. | HR cannot assign roles or edit its own grants; attempts to remove/disable the final active security administrator fail. Safeguards follow OD-01/OD-02. |
| AUTH-006 | Resolve team scope from the active reporting graph and invalidate stale authorization. | After a reporting change or grant removal commits, the next access check reflects it; a queued export cannot reuse stale access. |
| AUTH-007 | An employee's optional login account is unique; invitations and activation are retry-safe. | Concurrent provisioning cannot create two linked accounts; resend invalidates superseded tokens without duplicate invitations for one event. |
| AUTH-008 | Require a valid assignment and current permissions for approval; prohibit self-approval across all role combinations and delegates. | A requester with Super Admin and Manager roles cannot approve their own leave or attendance correction. Approved OD-14 also enforces configured distinct-stage review against actual decision-makers; roles, delegation, and broad scope cannot bypass it. |

## 9. Employee & Organization Management Requirements

### 9.1 Core record and history rules

Department hierarchy and employee reporting hierarchy are separate. Both must be acyclic. A manager may be in another department when company policy permits it; department membership does not automatically grant access. Organizational levels and positions describe structure and do not create implicit roles.

Employee identity survives account changes, termination, and rehire. Employment lifecycle, employment type, and authentication status are separate attributes. Each employment change has an effective date, recorded time, actor, and reason where applicable. Past attendance and approval records must retain the organizational context applicable when the business event occurred. Approved effective-date, termination/attendance intersection, and lifecycle details are in OD-03.

Approved minimum fields and field visibility are in OD-04. No salary, bank account, tax, applicant, or employee appraisal dataset is required in MVP. Do not collect national identifiers or medical detail merely to make the portfolio appear realistic.

**APPROVED [OD-03]:** offboarding impact review occurs when scheduled and again immediately before application. Identify open attendance, an ongoing shift intersecting the proposed employment end, future shifts, pending leave/reservations, and assigned approval responsibilities. HR may explicitly adjust the effective boundary with reason and preserved original dates, or use the independently reviewed attendance-resolution path. If unresolved when the unchanged boundary arrives, employment ends and account access is disabled as scheduled; retain an owned, visible attendance exception for HR resolution. Do not extend eligibility, leave the account enabled for clock-out, or fabricate evidence. A reviewed factual attendance correction does not itself reactivate the account or revise employment dates. Reconciliation retains the original schedule context and requires explicit HR resolution of applicable work expectations; do not silently classify post-termination scheduled time as an employee absence or early-departure penalty.

| ID | Requirement | Verifiable acceptance |
| --- | --- | --- |
| ORG-001 | Maintain one company, hierarchical departments/subdepartments, positions, organizational levels, and work locations/branches. | Create and revise reference records; reject department cycles, invalid references, and duplicate identifiers under approved uniqueness rules. |
| ORG-002 | Maintain effective-dated manager-subordinate relationships, including permitted cross-department reporting. | Direct and indirect team members resolve correctly; self-management and reporting cycles are rejected, including future-dated cycles. |
| ORG-003 | Preserve referenced organization records instead of destructive deletion. | A department/location/position used by history can be retired; historical records remain readable and new invalid assignments are blocked. |
| ORG-004 | Associate locations with a timezone and configurable attendance/geofence context. | The same UTC instant displays in the applicable location time; transferring an employee does not alter an old shift's timezone. Time model follows OD-05. |
| EMP-001 | Create an employee without requiring a user account and assign applicable organization information. | A valid employee saves without email/login; required-field and duplicate-identity checks follow OD-04. |
| EMP-002 | Support hiring, probation, activation, promotion, transfer, manager/location change, contract renewal, offboarding, and rehire as effective-dated employment changes. | Each operation produces a dated history entry; a historical query retrieves the old assignment. Exact states follow OD-03. |
| EMP-003 | Separate employment lifecycle, employment type, and account state. | Changing employment type does not log out the user by itself; offboarding applies the approved account-access rule without erasing history. |
| EMP-004 | Preserve employee identity and distinct employment periods on rehire. | Rehire reuses the employee record, creates the new employment period, and does not duplicate, merge, or rewrite old attendance or balances silently; any unresolved prior-period attendance remains attached to that period. |
| EMP-005 | Validate dates and concurrent employment changes. | Invalid effective ranges, manager cycles, and conflicting simultaneous changes are rejected or returned as a stale-state conflict, not silently overwritten. |
| EMP-006 | Make offboarding reconcile dependent access and work items. | Approved OD-03 requires pre-application checks of active/overnight attendance, future shifts, pending leave/reservations, and assigned approvals. HR explicitly adjusts the boundary or initiates independently reviewed resolution; an unresolved conflict does not delay deactivation automatically. Preserve original employment dates/evidence and audit every adjustment/correction. |
| EMP-007 | Limit employee updates to authorized fields and preserve material before/after history. | Forged sensitive/organizational fields from an unauthorized account fail; masked fields are absent from serialized payloads and CSVs. |
| EMP-008 | Restrict employee lists and exports to permitted records, fields, and filters. | Counts, pagination totals, search suggestions, and export rows all apply the same access predicate. |

## 10. Shift Management Requirements

### 10.1 Schedule resolution and calendar precedence

A **shift occurrence** is one employee's concrete work interval with a start date, start/end instants, timezone, work location, and applicable policy context. A shift crossing midnight is one occurrence attributed to its local start date. Clock-out on the following date must resolve the original occurrence. A non-working day is a deliberate calendar/schedule result; an unassigned schedule is missing operational configuration, not automatically a day off or an absence.

The confirmed calendar priority is **authorized special assignment → location exception → company calendar → regular schedule**. An explicit special working-day assignment can override a holiday. Saturday/Sunday have no universal non-working meaning.

**APPROVED [OD-06]:** resolve the regular schedule using an explicit dated base assignment, otherwise its rotating pattern, otherwise its weekly pattern. Individual overrides intended to override a holiday must be explicitly marked as authorized special assignments. A calendar layer either explicitly declares working/non-working and its shift source, or falls through. Same-priority conflicting results are rejected rather than selected by creation time. A working-day exception without a resolvable template is invalid. Validate the resulting intervals, not only the template dates.

Historical protection applies when attendance, an approved leave calculation, or a finalized report references an occurrence. Future schedule changes are versioned; changes affecting pending leave require explicit revalidation under OD-11. Referenced history cannot be silently regenerated from a newer recurring template.

| ID | Requirement | Verifiable acceptance |
| --- | --- | --- |
| SHIFT-001 | Create and retire reusable shift templates with valid local start/end time and explicit overnight semantics. | A 22:00–06:00 template resolves to the next calendar day; ambiguous zero-duration/end-day combinations fail. Duration limits follow OD-05. |
| SHIFT-002 | Support weekly recurring schedules, rotating patterns with an anchor date, and date-specific assignments. | The same employee/date resolves deterministically across repeated reads; weekly days off and rotation boundaries produce expected occurrences. |
| SHIFT-003 | Support authorized individual-date overrides and special working days, retaining the reason and replaced context. | An override is visible to permitted users; original and replacement contexts can be inspected through history. |
| SHIFT-004 | Reject overlapping effective employee work intervals, including cross-midnight and concurrent assignment writes. | A 22:00–06:00 shift conflicts with a 05:00–13:00 next-day shift; only one conflicting concurrent change can commit. Adjacent interval behavior follows OD-05. |
| SHIFT-005 | Apply company holidays, location exceptions, weekly days off, and authorized special assignments in the defined hierarchy; retain applicable holiday classification. | A normal Saturday shift remains working; a location holiday overrides the company result unless an authorized special assignment supersedes it. Authorized work on a holiday retains a holiday-work marker on attendance/report detail; no payroll compensation is calculated. |
| SHIFT-006 | Preview assignment/calendar impacts before committing a change affecting employees or referenced occurrences. | Affected date range, employees, pending/approved leave, recorded attendance, and employment-ending intersections [approved OD-03] are identified; protected conflicts cannot be overwritten or employment extended merely by confirming the preview. |
| SHIFT-007 | Preserve schedule history and freeze the context referenced by attendance and approved leave. | Editing a template does not change prior lateness, leave debit, location, or overnight start date; corrections create an audited revision. |
| SHIFT-008 | Notify affected account holders after committed shift changes. | One change event yields deduplicated notifications; employees without accounts remain scheduled and are shown as having no delivery destination. |
| SHIFT-009 | Validate recurrence publication over its effective range and revalidate newly materialized future occurrences. | A calendar or future pattern change cannot introduce an unchecked overlap outside the initial preview horizon. Exact preview horizon follows OD-06. |

## 11. Attendance Management Requirements

### 11.1 Policy resolution

**CONFIRMED:** HR can configure company defaults and authorized location/shift overrides; all listed attendance rules in BR-23 must be configurable. The effective policy must be explainable for an occurrence and preserved with its evaluation.

**APPROVED [OD-07]:** resolve each policy field independently as `shift override > work-location override > company default`. An unset value inherits; an explicit disabled value does not. Store/display the source and effective version. An individual special assignment may select a shift/location but does not create a fourth attendance-policy layer. A shift edit must not silently replace a prior attendance evaluation's policy.

| Policy field | Required behavior | Governing decision |
| --- | --- | --- |
| Early clock-in / latest clock-in | Define allowed interval around the expected work start/end | OD-07 numerical windows and boundary semantics |
| Late grace | Separate permitted grace from late classification and late-minute reporting | OD-07 |
| Clock-out window / early departure | Validate against the resolved occurrence and approved leave segments | OD-07 |
| Missing clock-out | Flag incomplete attendance; define when it is eligible for reconciliation | OD-07 / OD-17 |
| Absence finalization | Wait until the relevant occurrence/window closes; never classify an ongoing overnight shift absent at midnight | OD-07 / OD-17 |
| Unassigned shift | Detect configuration gap and provide an authorized resolution path | OD-07 |
| Geolocation / geofence | Per-location enforcement, allowed distance and GPS-accuracy handling | OD-08 |
| Overnight rules | Resolve occurrence by its interval/window, not device calendar date | OD-05 / OD-07 |

### 11.2 Photo, time, and location contract

Clock-in and clock-out each require a newly camera-captured photo. The browser must explain camera use before capture, allow review/retake, and show clear submission success or failure. Client timestamps, coordinates, MIME labels, and employee IDs are untrusted. A server timestamp is authoritative for the event; exact receipt boundary is approved in OD-09. Attendance is an online workflow; unsent photos must not later be backdated as successful clock events.

When geolocation enforcement is disabled, absence of coordinates must not block a valid photo-based submission. When enforcement is enabled, the location and accuracy checks apply at both clock-in and clock-out. Precise failure behavior is OD-08. Photographs and location are evidence, not biometric identity verification; no face matching or continuous tracking is included.

### 11.3 Geolocation validation [OD-08]

Select enforcement and validation mode from the occurrence's applicable server-side work-location policy. Initial configured radius is **200 m** and maximum acceptable reported accuracy is **100 m**. Enforcement remains disabled until configured and approved; once mandatory, a client flag, permission failure, or user-entered coordinate cannot disable it. Browser location readings are uncertain, potentially spoofable evidence, not proof of physical presence.

Let `d` be the backend-calculated distance to the configured center, `a` the reported accuracy, `r` the radius, and `a_max` the accuracy threshold. Require finite valid coordinates, finite `d >= 0`, and finite `0 <= a <= a_max`. Compare unrounded values; the displayed rounded distance is not authoritative.

| Mode | Exact acceptance rule after other attendance checks | Security / usability trade-off |
| --- | --- | --- |
| Standard — initial default | `d <= r` and `a <= a_max`, with valid evidence | Fewer false rejections near the boundary; the uncertainty region may extend outside the fence. Passing is not proof of presence. |
| Strict uncertainty-aware | `d + a <= r` and `a <= a_max`, with valid evidence | More conservative about uncertainty; rejects more otherwise in-radius readings. The location must explicitly select this mode. |

Equality at either configured threshold is accepted; an unrounded value above it is not. For `r=200`, `a_max=100`, `d=200`, `a=100`, Standard accepts and Strict rejects as uncertainty crossing the fence. For `d=100`, `a=100`, Strict accepts; `a=100.01` fails both modes regardless of distance. Even Strict mode does not prove real-world presence or authenticate the GPS source.

Return distinct actionable outcomes for permission denied, unavailable/timeout, invalid reading, accuracy exceeding threshold, measured distance outside radius, and in-radius uncertainty failing Strict mode. Offer retry or authorized correction/resolution; do not accept a manual location claim as replacement evidence. Do not persist precise coordinates by default: retain only the minimal validation result/mode and necessary non-precise evidence. Any justified precise-coordinate retention requires an approved category policy and purpose under approved OD-16 and OD-21; exclude raw coordinates from logs and exports.

Under approved OD-20, an explicitly isolated synthetic-demo environment uses server-selected/validated fixtures instead of requesting visitors' real camera/GPS. It preserves attendance business guards, labels simulation, and cannot enable that path in real-data mode. Genuine capture and geofence behavior remain subject to private production-like tests.

| ID | Requirement | Verifiable acceptance |
| --- | --- | --- |
| ATT-001 | Accept clock-in only for the authenticated eligible employee, with camera photo, server time, and a resolvable permitted occurrence. | Forged employee IDs, missing/invalid photos, disabled accounts, ineligible employment, and invalid windows fail without creating successful attendance. |
| ATT-002 | Accept clock-out with a separate camera photo linked to the employee's open occurrence. | A next-day overnight clock-out closes the correct occurrence when account/employment eligibility still applies; no valid open record means no fabricated clock-in. If offboarding has become effective, deny ordinary clock-out and route the retained open session to independent HR correction/resolution under approved OD-03, not an access exception. |
| ATT-003 | Prevent duplicate clock-in/out under double-clicks, retries, and concurrency. | Identical retry returns the same business outcome; conflicting submissions cannot create two successful events for the same action/occurrence. |
| ATT-004 | Enforce resolved attendance policy and expose actionable validation reasons. | Tests cover just before, exactly on, and just after approved window and grace boundaries; browser validation cannot override a server rejection. |
| ATT-005 | Enforce location-specific geolocation/geofence settings without treating GPS as identity proof. | Approved OD-08 defines Standard and Strict mode, inclusive thresholds, server-authoritative comparison, and distinct denied/unavailable/invalid/inaccurate/outside/uncertain errors. Mandatory enforcement cannot be disabled by the client; retry/correction is offered and precise-location retention is minimized. |
| ATT-006 | Calculate lateness and early departure from the occurrence's applicable expected work segments, excluding approved leave. | First-half leave moves the expected arrival to the remaining segment; full-shift approved leave has no expected attendance and no lateness. |
| ATT-007 | Detect no clock-in, missing clock-out, unassigned schedule, and inconsistent overnight records separately. | Unassigned schedules are visible exceptions; missing clock-out is not automatically a fully absent shift; open overnight work is not finalized early. Approved OD-03 adds a visible termination-intersection exception with HR ownership; it neither fabricates closure nor extends employment. |
| ATT-008 | Permit an employee to request a correction with proposed values and a reason; retain original evidence. | A correction request does not directly change attendance; duplicate pending/conflicting corrections are detected under OD-10. HR may initiate for an employee without access under approved OD-10. |
| ATT-009 | Authorized reviewers approve/reject corrections, with no self-approval and auditable reasoning. | An approval atomically records the decision and corrected evaluation; rejected requests preserve the previous effective result. Under OD-10, both the submitting actor and affected employee are excluded from review. |
| ATT-010 | Reconcile eligible occurrences and reprocess authorized corrections while retaining prior evaluation history. | Reconciliation is idempotent; an approved correction invalidates affected aggregates and leads to a traceable new evaluation version. |
| ATT-011 | Detect attendance/leave/schedule conflicts and prevent contradictory final states. | A clock-in overlapping approved leave is rejected; leave final approval cannot overlap recorded work without explicit resolution. Section 15 governs resolution. |
| ATT-012 | Securely store attendance evidence and prevent orphaned or public uploads. | Guessing photo URLs fails; failed submissions do not leave indefinitely accessible orphan photos; retention follows SEC-007. |
| ATT-013 | Present clock submission progress, retry guidance, and committed result after an interrupted response. | A network timeout followed by retry/status refresh does not create another event; the user can distinguish committed attendance from an unsent attempt. |

## 12. Leave Management Requirements

### 12.1 Policy, eligibility, and schedule calculation

Leave types define applicable eligibility, whether a balance is charged, and allocation/accrual/carry-over/expiry rules. Entitlement values are company policy subject to legal review, not hardcoded statutory assumptions. Annual allocation and monthly accrual must both be supported as configurable methods. Their combination, if configured, must explicitly prevent unintended duplicate entitlement.

Calculate leave against actual resolved shift occurrences, not the number of calendar dates or an assumed Monday–Friday week. A requested date for overnight work identifies the **shift start date**. Store a calculation preview with included occurrences, full/half-day selection, excluded non-working days, balance impact, and any unresolved scheduling issue. Approved requests preserve that schedule/policy context; later schedule edits do not silently alter debits.

**APPROVED [OD-11]:** full-day leave covers one occurrence; a half day covers either its first or second half of scheduled working time, excluding configured non-working breaks. Charge one or one-half day irrespective of shift length. Block requests with unresolved work schedules and zero eligible duration. Pending requests are revalidated before final approval; changed calculations require the requester's explicit acceptance/resubmission, not a silent increase in reservation.

**APPROVED [OD-12]:** each employee has one effective leave policy per type/date, resolved as employee override > location > company; reject conflicting same-priority policies. Charge and entitlement-period selection follow each occurrence's shift-start date, including overnight shifts, using its preserved timezone/date and the configured company leave-year boundary. Split cross-period requests before reserving, not by the calendar date of clock-out.

### 12.2 Ledger and reservation invariants

**CONFIRMED:** leave balances require a transaction ledger and pending requests reserve applicable balances. Allocation/accrual, adjustment, approval, rejection, cancellation, carry-over, and expiration must be represented consistently and remain auditable. No UI directly overwrites a balance total.

The transactional reservation/debit guarantees are confirmed; cancellation and reversal behavior is **APPROVED [OD-13]**. The detailed accounting, precision, and cross-period rules below are **APPROVED [OD-12]** and preserve those guarantees:

- `posted balance(b) = sum(posted signed economic ledger entries for bucket b)`. Credits include allocation/accrual, carry-in, positive adjustment, and linked consumption reversal; debits include consumption, expiration, carry-out, and negative adjustment. A paired carry-out/carry-in transfers entitlement without making the same units spendable in both buckets.
- `available balance = posted balance - active reservations`. Active reservations are commitments, not consumption debits.
- Submitting a chargeable request atomically reserves its required units. Intermediate approvals retain the reservation. Final approval atomically releases the reservation and posts consumption for the same amount.
- Rejection or cancellation before final approval releases the reservation exactly once and posts no consumption reversal.
- Approved cancellation posts a linked consumption reversal for approved cancellable units exactly once. It must not release an already released reservation again. Expired/carry-over bucket rules determine whether restored entitlement is usable; cancellation must not manufacture fresh entitlement.
- A non-balance-consuming leave type has approval and schedule rules but no fake reservation or consumption debit.
- Adjustments and corrections use linked ledger entries with actor, reason, effective date, and recording time. Existing entries are not edited/deleted to make totals agree.

Reconcile posted, reserved, and available amounts per bucket before aggregating; a positive total in one period cannot conceal an overspent/ineligible bucket in another. Neither an adjustment nor a policy edit may erase or silently invalidate an active reservation. Offboarding treatment is governed by approved OD-03; reversal eligibility remains governed by approved OD-13 and the approved bucket details of OD-12.

#### Deterministic accrual and precision [approved OD-12]

Retain four-decimal day-unit ledger precision and round to nearest with **half up**; display two decimals without changing calculations. Retain the original annual/new-hire proration and month-end-in-arrears accrual basis. For one employee/policy-version/leave-year accrual scope, let `C(k)` be the four-decimal rounded sum of the **unrounded** eligible monthly amounts through due period `k`: each contribution is `annual amount / 12 × eligible calendar days in month / calendar days in month`. Do not round each month's contribution before summing.

Under lock, post only `C(k) - signed accrual already posted for that scope`, then record the processed-through period. A retry or older job whose period has already been processed is a no-op; a later due job may catch up prior due periods once. Future/unclosed months are excluded. A negative difference caused by an authorized historical policy/eligibility correction requires an explicit audited compensating adjustment with reservation checks; it is not an automatic scheduler clawback. Repeated or out-of-order execution must never duplicate credits or reverse a newer valid accrual. For an illustrative 1-day annual amount over 12 fully eligible months, cumulative credits total exactly **1.0000**, not **0.9996**.

#### Future requests, period splitting, and bucket allocation [approved OD-12]

Expected future accrual is a forecast, not posted entitlement or spendable balance. A future request may reserve only actually posted entitlement whose leave type, employee, validity interval, and receiving entitlement period cover the relevant shift-start date. If sufficient eligible posted units do not exist, reject with the affected date/period and an explanation; do not borrow unearned accrual or create placeholder credits.

Split full/half-day charges by occurrence and entitlement period, then allocate across eligible buckets in earliest-expiry order; ties use valid-from date, then immutable bucket identifier. A non-expiring eligible bucket sorts after expiring buckets. Do not reserve expired/not-yet-valid/ineligible units or directly spend a prior-year bucket in a new year without an authorized posted carry-over. Preserve the original grant and every carry-over link. A shift starting on 31 December and ending on 1 January belongs to the starting period; a separate shift starting on 1 January belongs to the next period.

Lock all involved bucket capacities and conflict state; reserve the entire request or none. Concurrent requests cannot overspend any bucket. Final approval consumes the validated allocation and releases its reservation atomically; changed eligibility requires explicit revalidation/reallocation, not silent substitution. Carry-over transfers only eligible unreserved remainder; valid reservations remain tied to their original lineage. Expiry protects valid reservations for covered dates; releasing/reversing units that are no longer spendable posts the required linked expiry effect without making those units newly available. Ledger economic entries are immutable; corrections use linked compensating entries.

### 12.3 Sequential approval and exception routing

Super Admin configures the ordered stages, including manager and HR-type stages. All required stages must complete before final approval. The requester cannot approve their own request at any stage. Route assignment, permission/scope, and stage validity are independent checks.

An approver must have an enabled account, necessary permissions and data scope, valid assignment/delegation, and no self-approval conflict. A manager without an account, an offboarded approver, a revoked role, or a requester matching the HR approver cannot decide. The required stage must still be filled by an eligible independent actor; do not skip it or auto-approve.

**APPROVED [OD-14]:** snapshot the workflow definition at submission; resolve assignees when stages open and revalidate at decision. For an organizational-root requester with no direct manager, use an explicitly configured alternative eligible account/pool for that same required stage. The alternative needs read/approval permissions, scope, and authority under the configured workflow. This does not require a fabricated manager assignment or change OD-04's approved employee-field rules. A disabled/ineligible manager may use a fallback only where its trigger is explicitly configured; otherwise HR must reassign. If no eligible independent first-stage approver exists, reject submission without a reservation; if a later stage has no candidate, block it visibly, preserve the pending reservation, and alert HR.

Distinct actual decision-makers across sequential stages are the default. Exclude previous approving actors from later stages and revalidate delegates against the same history. An explicit workflow exception allowing repeated approvers requires `leave.workflow.manage`, a recorded reason, and audit of configuration and every use; include it in the submitted workflow snapshot. Do not infer an exception from multiple roles, broad scope, a shared role pool, delegation, or Super Admin access. No exception permits self-approval, skipped stages, or cascading decisions. If independent review becomes impossible, block and require authorized reassignment rather than reusing an approver automatically. Reassignment affects only undecided stages and preserves prior decisions. HR intervention does not confer workflow-configuration rights; initial route/fallback changes still require an actor with `leave.workflow.manage`.

Temporary delegation has a start and end date and delegates only eligible work. A delegate must already have adequate permission/scope; delegation does not grant a general role. Escalation sends notifications or requests authorized reassignment; it never approves the request. Exact authority, reminder intervals, and escalation recipients are OD-14.

| ID | Requirement | Verifiable acceptance |
| --- | --- | --- |
| LEAVE-001 | Configure leave types, eligibility, annual allocation, monthly accrual, proration, carry-over, and expiry without assumed statutory entitlements. | Approved policy fixtures produce deterministic entitlement; incompatible duplicate allocation methods or overlapping policy versions fail. |
| LEAVE-002 | Maintain an auditable ledger and derived posted/reserved/available balances per employee/type/period. | Totals reconcile per bucket to signed entries and active reservations; editing a display cannot bypass an adjustment. Approved OD-12 preserves carry-over/reversal lineage and separates posted entitlement from future expected accrual. |
| LEAVE-003 | Allocate/accrue/expire balances through retry-safe processing. | Approved OD-12 posts only the difference from authoritative cumulative four-decimal half-up accrual; retries/older jobs cannot duplicate credits or undo newer due postings. Backfills retain effective/recording dates and require authorized adjustment for a negative correction. |
| LEAVE-004 | Preview full-day and half-day leave against actual schedules, holidays, and overnight start dates. | Non-working dates contribute no charge where policy excludes them; two unequal-length shifts still follow approved leave-unit rules. |
| LEAVE-005 | Validate eligibility, overlaps, pending reservations, and balances at submission. | Overlapping pending/approved segments fail. Approved OD-12 splits by shift-start date/period and permits only eligible posted buckets; missing future entitlement is explicitly rejected and concurrent reservations cannot overspend any bucket. Disjoint half days follow approved OD-11. |
| LEAVE-006 | Reserve balance atomically with request submission and valid route creation. | The request, all period/bucket reservations, and first-stage state commit together or none does. Approved OD-14 fills a missing-manager stage with an explicitly configured eligible independent fallback; without one, initial submission fails with no reservation. |
| LEAVE-007 | Enforce ordered multi-level decisions and authorization on each decision. | Stage two cannot decide while stage one is pending; stale, duplicate, unassigned, out-of-scope, and self-approval commands fail safely. Approved OD-14 requires distinct actors by default; a permitted repeated-actor exception must be explicit, authorized, snapshotted, and audited. |
| LEAVE-008 | Final approval converts the reservation to consumption atomically after current conflict/eligibility validation. | A request can neither remain reserved and fully debited nor become approved without its required debit; an attendance race cannot produce conflicting approval. |
| LEAVE-009 | Rejection terminates undecided stages, records the reason, and releases reservations exactly once. | A concurrent approval/rejection has one winner; repeat rejection does not add balance. |
| LEAVE-010 | Support requester cancellation and authorized review of approved cancellations. | Pending cancellation releases reservations; approved cancellation restores only approved cancellable units using linked reversals under OD-13. |
| LEAVE-011 | Support temporary delegation, reminders, escalation, and HR reassignment with audit. | Expired/self delegates cannot decide; reminders are deduplicated; escalation never approves. Approved OD-14 requires scope, stage assignment, and distinct-actor checks after delegation/reassignment; its existing 24-hour reminder/48-hour escalation intervals are retained. |
| LEAVE-012 | Preserve approved schedule, policy, calculation, approver, and organizational context. | A later shift transfer or policy revision does not silently change a past leave debit, approval record, or historical grouping. |
| LEAVE-013 | Resolve leave/attendance conflicts through explicit authorized actions. | Approval cannot overlap effective recorded work; conflict resolution links any cancellation, ledger reversal, attendance correction, and audit events. |
| LEAVE-014 | Permit authorized balance adjustments with validation and reason. | An adjustment cannot erase a reservation or linked debit; approved OD-12 rejects reductions that overspend a bucket or invalidate reservations until explicit audited resolution/reallocation occurs. Use compensating entries, preserving immutable economic meaning. |
| LEAVE-015 | Protect leave reasons and sensitive detail independently of summary visibility. | A manager permitted to see dates/type/status cannot retrieve hidden medical or private reasons through page payloads, notifications, or exports. |

## 13. End-to-End Workflows

These workflows specify business order and failure outcomes, not UI screen layouts. Notification delivery follows commit. A validation, authorization, stale-state, or concurrency failure leaves the previous committed business state intact.

| Workflow | Actor and preconditions | Required sequence | Completion and failure behavior |
| --- | --- | --- | --- |
| WF-01 Invitation and activation | Authorized provisioner; employee exists; suitable unique email | Select employee → verify optional account link → issue single-use invitation → queue email → recipient sets password and activates → link account | Account linked once; expired/revoked/reused token fails. Email failure is retryable without duplicating employee/account. |
| WF-02 Login, logout, and reset | Account exists; activation/access rules met | Authenticate → regenerate session → enter scoped landing view; logout invalidates session; reset issues limited token → set password → invalidate token | Disabled account remains blocked; reset responses avoid account enumeration; reset does not reactivate an offboarded account. |
| WF-03 Roles and effective access | Super Admin/security-authorized account in private/local owner-controlled environment | Create/edit role → choose known actions and valid scopes → preview grants → assign one or more roles → commit/audit → invalidate authorization cache | The next server check uses new grants; forbidden self-escalation/final-admin removal is rejected. Public demo shows only the sanitized read-only role comparison and backend-enforced mutation denial [approved OD-20]. |
| WF-04 Hire and provision | HR with employee/employment permissions | Create employee → validate required fields → assign organization/manager/location/type → set effective employment start → activate/probation per policy → optionally invite | Employee can remain without login; incomplete activation is a visible validation failure, not an implicit active employee. |
| WF-05 Employment change | HR; employee exists; effective date valid | Select promotion/transfer/reporting/location/renewal event → preview dependent schedules/routes/access → validate graph and dates → commit dated history → notify where required | History preserved; present access changes only when effective; conflicting backdated changes enter controlled resolution. |
| WF-06 Offboard and rehire | HR; effective employment action authorized | Schedule ending → preview open sessions/ongoing overnight intersections, future shifts, pending leave/reservations and assigned approvals → alert HR → explicitly adjust effective boundary or plan independent attendance resolution → recheck immediately before application → at unchanged/authorized boundary end employment and disable access → resolve owned exceptions; on rehire reuse identity/new period and separately authorize account reactivation | Approved OD-03 permits no automatic employment extension, clock-out access grace, guessed evidence, or abandoned exception. Audit original/revised dates and corrections; old attendance remains in its original period. Bucket effects follow approved OD-12 and approved OD-13. |
| WF-07 Templates and recurring scheduling | HR/scheduler with required permissions | Define valid template → configure weekly/rotation anchor → select employees/effective range → resolve calendars → preview overlaps/dependencies → commit → queue notifications | Conflicting employees/dates are identified; atomic batch behavior is OD-06. Historical referenced occurrences remain protected. |
| WF-08 Override and holiday update | Calendar/override-authorized actor | Add company holiday/location exception/special assignment or date override → resolve precedence → preview affected shifts, leave, attendance → validate → commit version → notify | Invalid working-day result or protected history conflict blocks that change. Existing evidence is not rewritten. |
| WF-09 Clock-in | Enabled employee; eligible employment and schedule | Load resolved occurrence/policy → genuine camera capture/retake and required GPS, or explicitly isolated server-controlled demo fixtures → submit → backend selects mode and validates/locks → save evidence and event/evaluation → return committed result | Duplicate retry resolves prior outcome. Approved OD-08 provides specific GPS errors and Standard/Strict boundary checks; approved OD-20 labels simulated evidence and forbids client activation of synthetic mode outside demo. |
| WF-10 Clock-out | Enabled employee with eligible open occurrence | Recheck employment/account eligibility → capture new photo and required GPS (or isolated demo fixture) → locate original shift even after midnight → backend validates mode/policy and locks → record clock-out → evaluate | No open shift/expired window requires correction. If termination has become effective, deny ordinary clock-out and retain an HR-owned exception under approved OD-03; never enable the account, extend eligibility, or guess a timestamp to close it. |
| WF-11 Attendance correction and reconciliation | Employee requester, or HR initiation under approved OD-10, including an offboarded employee; independent authorized reviewer | View original record/termination conflict → propose documented correction and reason → reviewer examines evidence, employment boundary and conflicts → approve/reject → atomically apply reviewed revision → re-reconcile and refresh aggregates → notify | Original dates/photos/timestamps remain. A factual reviewed end does not reactivate access or extend employment; if actual end cannot be established, preserve missing evidence with an explicit reviewed resolution, not a fabricated clock-out [approved OD-03]. |
| WF-12 Configure leave and grant balances | Authorized policy/balance actor | Define effective policy → validate allocation/accrual/proration/carry-over/expiry → preview due cumulative entitlement → lock scope/buckets → post only cumulative difference and processed period → reconcile signed ledger and reservations | Approved OD-12 makes retries/older jobs no-ops, preserves four-decimal calculation versus two-decimal display, and prohibits future expected accrual credits or duplicate carry-over. |
| WF-13 Submit leave and reserve | Employee with eligibility | Select dates/full/half segments/type → split by shift-start entitlement period → preview eligible posted buckets → resolve required stage and configured missing-manager fallback → validate overlaps → lock buckets/occurrences → reserve all segments/create stages → commit/notify | Approved OD-12 rejects missing/expired/ineligible or unearned future entitlement with no partial reservation. Approved OD-14 rejects an invalid first-stage route without reserving; a fallback fills rather than skips the stage. |
| WF-14 Sequential review | Assigned approver with current scope, permissions, and required independence | Open request → validate current stage/actor history and any authorized snapshotted exception → decide under lock → open next stage after checking eligible distinct/fallback candidates → final stage revalidates schedules, conflicts and bucket allocation, then debits/releases → notify | Approved OD-14 blocks self/reused/unassigned/out-of-order/stale actors as applicable; missing independent next approver blocks visibly with HR intervention, not auto-approval. Approved OD-12 preserves original bucket lineage and atomic debit/release. |
| WF-15 Delegate, remind, escalate, reassign | Eligible delegator/HR; pending stage | Configure dates → validate delegate permissions/scope and prior-stage actor history → route eligible undecided work → remind/escalate at retained intervals → HR may reassign with reason → revalidate at decision | Approved OD-14 prevents delegation/reassignment from bypassing independence or self-approval. Expired/revoked authority fails; escalation never decides; prior decisions and exception use remain auditable. |
| WF-16 Reject or cancel leave | Requester or assigned reviewer; cancellable state | Rejection closes remaining stages and releases reservation; pending cancellation releases it; approved cancellation request stays under review → HR validates attendance/time/ledger → approves linked reversal or rejects → re-reconcile | No double restoration; unresolved/used leave requires authorized resolution. Approved leave remains effective until cancellation commits. |
| WF-17 Dashboard and drill-down | Authenticated user with dashboard and relevant source permissions | Load scoped persona view → apply date/department/location filters → show metric basis/freshness → select chart/card/calendar segment → open identically filtered authorized detail | Scope limits every query and counter. A provisional or incomplete denominator is labeled; no unauthorized record count is disclosed. |
| WF-18 CSV export | User with export and source-read permissions | Choose permitted report/filter → validate → create bounded export request → queue export under approved OD-19 → recheck current access → generate safe CSV → notify completion → authorize download → clean up under its approved temporary-artifact policy | Revocation or hierarchy change prevents stale delivery; partial/failed exports are not downloadable. Approved OD-16 keeps temporary artifact cleanup separate from deletion of real source HR records. |

## 14. State Transitions and Lifecycle Definitions

The invariants and supported business events are confirmed. Invitation/account security follows approved OD-02; corrections, cancellations, and evaluations follow approved OD-10/OD-13/OD-17. Employment-ending behavior, bucket transitions, approval independence/fallback, retention eligibility, and demo reset safeguards follow approved OD-03/OD-12/OD-14/OD-16/OD-20. State transitions require current state/version validation in the same transaction as their economic or attendance effects.

| Entity | States and legal transitions | Guards / side effects |
| --- | --- | --- |
| Invitation | pending → accepted / expired / revoked | Single use; resend revokes the old pending token; acceptance cannot link a second account. |
| User account | invited → active → disabled; disabled → active by authorized reactivation | No state conflation; disabled sessions invalidated. An open attendance session cannot create a temporary active/clock-out-only state [approved OD-03]. |
| Employment period | planned → probation or active; probation → active or ended; active → ended | Recheck intersecting sessions before the effective boundary; any adjustment is explicit/audited. An unresolved session never delays ending automatically [approved OD-03]. |
| Employee rehire | ended prior period + new planned/probation/active period on same identity | Promotion/transfer/renewal are dated events, not new employee identities; no overlapping active periods under OD-03. |
| Attendance session | not started → open → closed through valid clock-out or approved factual correction | Ending employment/account access does not close the session. Preserve an HR-owned termination exception; if factual end is unknown, retain incompleteness and explicit reviewed resolution without fabricating an event [approved OD-03]. |
| Attendance evaluation | provisional → reconciled; reconciled → pending re-reconciliation → reconciled with new version | Completeness and presence classifications are separate dimensions; corrections retain previous versions. |
| Attendance correction | pending → approved / rejected | Only independent authorized reviewer decides; approval applies one effective revision; stale pending requests require revalidation. |
| Leave request | draft → pending approval → approved / rejected; draft or pending approval → cancelled; approved → cancelled only after approved cancellation | Draft reserves nothing; pending reserves; approved consumes; transitions have exactly-once ledger effects. |
| Approval stage | waiting → pending → approved / rejected; waiting/pending → closed when request terminates | One current stage; missing-manager fallback fills it. No independent candidate means blocked, not skipped/approved. Revalidate prior actors/delegates and any explicit exception [approved OD-14]. |
| Leave reservation | created active → consumed on final approval / released on rejection or cancellation | No effect before full request commit; period/bucket lineage retained. Reallocation and expiry require explicit atomic validation, never silent movement or deletion [approved OD-12]. |
| Retention-category policy | unapproved → pending validation → approved; approved → suspended or pending validation after material change | Destructive real-HR cleanup requires current category/version approval and legal/policy validation; holds and preview checks still apply [approved OD-16]. |
| Approved-leave cancellation request | pending → approved / rejected | Original leave stays approved and effective while pending; approval atomically reverses approved cancellable consumption. |
| Export job | queued → processing → ready / failed / cancelled; ready → expired | Only complete artifacts become ready; download requires current authorization; reset/revocation can cancel stale work. |
| Notification delivery | pending → sent or retryable failure → sent / exhausted | Channel retries do not repeat the business event or duplicate notification-center items. |

### 14.1 Attendance classification dimensions

**APPROVED [OD-17]:** separate (1) scheduled expectation, (2) presence, (3) punctuality, (4) completeness, (5) exception flag, and (6) reconciliation state. This prevents one status from incorrectly implying another.

- Expectation: working, fully excused leave, non-working, or schedule unresolved.
- Presence on remaining required work: present, absent after finalization, or pending/unknown.
- Punctuality: on time, late, early departure, or not evaluable; late and early departure may coexist.
- Completeness: complete, missing clock-in, missing clock-out, or inconsistent.
- Exceptions: unresolved cross-module conflict, missing policy/schedule, invalid evidence, or correction under review.

A valid clock-in with missing clock-out is present but incomplete; lateness may still be evaluable. A fully approved leave occurrence is excused, not absent. Non-working dates are excluded from required attendance. An unresolved conflict is not silently finalized as absence. Exact metric eligibility is in Section 19.

## 15. Business Rules, Cross-Module Integration, and Conflict Resolution

| ID | Business invariant / integration contract | Validation and resolution |
| --- | --- | --- |
| RULE-001 | Employee and organization history provide the effective context for scheduling, eligibility, approval routing, and reporting. | Reject cycles/invalid intervals; distinguish preserved history from current access. Approved OD-03 requires explicit handling of termination/active-shift intersections without automatic eligibility extension or continued account access. |
| RULE-002 | Scheduling and calendar resolution determine work obligations used by attendance and leave. | Resolve one deterministic occurrence per applicable assignment; reject overlaps and unresolved contradictory calendar results. |
| RULE-003 | Attendance and approved leave cannot assert worked and excused time over the same effective segment. | Recheck under transaction at clocking, final approval, correction, and cancellation; use explicit resolution paths below. |
| RULE-004 | Protected history is not recalculated from mutable current configuration. | Referenced schedules, policies, approvals, and evaluations retain versions; authorized revisions link old/new context. |
| RULE-005 | A pending leave reservation is not spent twice or erased by an adjustment. | Approved OD-12 locks and reconciles every eligible bucket/period; future expected accrual is not available and cross-period requests are all-or-nothing. |
| RULE-006 | No account/role combination bypasses no-self-approval or stage order. | Compare requester/actor and prior-stage identities including delegates; approved OD-14 requires distinct review by default and explicit audited authority for any configured repeated-actor exception. |
| RULE-007 | A balance, approval, attendance revision, and required audit outcome commit consistently. | Roll back failed critical mutations; notifications and expensive aggregates follow commit. |
| RULE-008 | Scope-aware analytics and exports cannot widen source permissions. | Intersect report permission, each source-read permission, field permissions, and current employee scope. |
| RULE-009 | Server time determines clock events; location timezone determines occurrence date. | Ignore client time as authority; retain original instants/timezone and report overnight work on its start date. |
| RULE-010 | Every retryable business command has an unambiguous retry outcome. | Same idempotency intent returns prior result; same key with a different payload is rejected; technical retries do not duplicate side effects. |

### 15.1 Controlled exception paths

| Conflict / edge case | Immediate behavior | Authorized resolution and preserved evidence |
| --- | --- | --- |
| Clock-in during full approved leave or its covered half | Reject conflicting clock-in; explain leave/segment conflict | Employee requests cancellation; independent HR review commits any cancellation/reversal, then a valid new clock attempt may succeed. Past work uses audited correction after leave resolution. |
| Leave request overlaps existing attendance | Show conflicting occurrence; block final approval, and under OD-11 block submission for known overlap | HR validates actual work; either leave dates/segments are resubmitted or erroneous attendance is corrected. No automatic deletion. |
| Attendance arrives while leave is pending | Pending leave is not yet excused time; detect conflict and warn/block approval | Under OD-11 allow otherwise valid attendance, mark leave conflict, and require request cancellation/resubmission or attendance correction before final approval. |
| Final leave approval races with clock-in | Serialize shared employee/occurrence conflict checks | One compatible state wins; the other command returns a conflict and applies no partial debit or attendance. |
| Shift/calendar edit affects recorded attendance or approved leave | Block silent replacement; present affected records | Use an explicit HR resolution with reason, linked schedule revision and any required correction/cancellation. Keep original evidence/calculation. |
| Schedule changed while leave is pending | Mark calculation stale and stop final approval | Recalculate and require requester acceptance/resubmission [OD-11]; adjust reservation atomically, releasing the old reservation exactly once. |
| Missing clock-out | Mark incomplete; no guessed clock-out | Employee or HR-initiated correction → independent review → re-reconcile. Offboarded employees use HR initiation without reopening access. If evidence cannot establish a factual end, record a reviewed incomplete resolution; presence is not automatically absence. |
| No clock-in after closed work window | Await finalization rules; check leave and schedule validity | Reconcile to absence only if work was required, no excusing leave applies, and no blocking conflict exists. |
| Unassigned shift / unknown calendar | Reject unvalidated clocking under OD-07; show operational exception | HR assigns a legitimate occurrence or special assignment. Any historical adjustment is audited, not backdated silently. |
| Overnight shift across month/year/holiday boundary | Keep one occurrence under local start date and frozen calendar context | End-date clock-out closes the original session; approved leave/ledger split follows approved start-date/leave-period rules. |
| No direct manager, or approver absent/ineligible/self | Resolve an explicitly configured eligible fallback for the same stage where applicable [approved OD-14]; otherwise block/notify HR | An alternative must satisfy scope, permissions, authority, and independence. Invalid first route means no submitted reservation; a later blocked stage preserves pending state. HR reassigns undecided stages; never skip/auto-approve. |
| Same actor would decide multiple sequential stages | Apply default distinct-stage guard, including after delegation/reassignment [approved OD-14] | Use another eligible independent actor, or an authorized reasoned exception already in the workflow snapshot with audit of use. Never infer an exception from roles or bypass self-approval. |
| Delegation expires during an open stage | Revalidate at decision time and reject expired authority | Return to eligible original approver or authorized reassignment; delegation history remains. |
| Two simultaneous correction reviews/approvals | Compare state/version under lock | Exactly one valid transition; loser refreshes. No duplicated revision or ledger side effect. |
| Camera denied/unavailable or invalid photo | No successful clock event | Explain permission/retake/retry; if unable to clock, submit correction for review. No unapproved photo-free clock bypass. |
| GPS denied, inaccurate, outside, uncertain, or unavailable | Backend applies configured Standard/Strict mode and returns a specific actionable outcome [approved OD-08] | Retry or independent correction/resolution; never silently disable mandatory enforcement, accept manual coordinate claims, or persist precise evidence without its approved policy. |
| Offboarding intersects active/overnight attendance or pending work | Recheck and present conflict before applying; disable access at the effective boundary if HR has not explicitly changed it [approved OD-03] | Audit any boundary adjustment; otherwise retain an HR-owned session exception for independent resolution, without extending eligibility, enabling clock-out access, or fabricating events. Preserve original dates and reservation lineage. |
| Future/cross-period request lacks eligible posted entitlement | Reject full reservation with date/period explanation [approved OD-12] | Wait for actual posting or authorized entitlement change; do not borrow forecast accrual, use an expired bucket, or reserve only the funded part silently. |
| Holiday published after approval/reconciliation | Existing protected context remains until reviewed | Preview impacts; HR explicitly corrects attendance/leave if policy requires. Audit changed grouping/aggregates. |
| No eligible records / zero denominator | Display N/A for percentage and an explanatory empty state | Do not show 0% as a substitute for missing eligibility or insufficient data. |
| Demo reset while jobs are running | Cancel or invalidate jobs tied to old demo data | Reset is atomic from user perspective; old exports, sessions, and notifications cannot attach to new employees [OD-20]. |
| Synthetic fixture requested in real-data mode | Fail closed regardless of request flags, IDs, roles, or capture failure [approved OD-20] | Correct the explicitly isolated environment configuration; do not permit a production fallback or treat simulated evidence as genuine. |

**APPROVED half-day evidence interpretation [OD-11]:** an allowed early/late boundary clock event does not assert that every elapsed minute was worked. Non-working breaks and excused segments remain distinct. A permitted clock-out transmitted just after the beginning of second-half leave may close the earlier work session without inventing work during leave; retain its actual timestamp. A new clock-in inside an approved leave segment is still rejected. Final approval of leave overlapping an open attendance session must wait for sufficient clock-out/correction evidence to establish non-overlap. A claim of actual work during approved leave requires explicit leave resolution before attendance correction. This convention prevents camera/upload latency from silently changing leave coverage.

### 15.2 Cross-module ownership

| Source owner | Consumers | Required information boundary |
| --- | --- | --- |
| Authentication/authorization | All modules, files, jobs | Current identity, grant sets, scope predicates, field restrictions, account eligibility |
| Employee/organization | Shift, attendance, leave, dashboard | Stable identity; effective employment, manager, department, location, type |
| Shift | Attendance, leave, reporting | Resolved occurrence, timezone/start date, working segments, calendar and policy versions |
| Attendance | Leave conflict checks, dashboard, future Phase 2 | Original events/evidence, effective correction, completeness, reconciled evaluation/version |
| Leave | Attendance, dashboard, future Phase 2 | Approved excused segments, reservations, ledger effects, route/decision state |
| Dashboard/reporting | UI, CSV | Shared metric definitions, authorized filters, source/evaluation versions and freshness |

These are conceptual contracts; they do not prescribe tables, REST endpoints, or event payload implementations.

## 16. Notifications and Background Processing

Email and in-app notifications are required. The notification center includes unread count, a chronological list, an accessible detail/link, and mark-as-read. No WebSocket channel is required. Updates may use navigation, refresh, or bounded polling selected during design.

| Event | Required recipient(s) | Channels and payload boundary |
| --- | --- | --- |
| Account invitation | Invited email; authorized provisioner sees delivery state | Email carries limited activation link; delivery failure does not create a second account. Before activation no in-app inbox is available. |
| New leave / next stage opened | Current eligible approver or delegate | Email + in-app; minimal requester/date/type context; no sensitive reason in email. |
| Stage decision / final approval / rejection | Requester; newly active approver where applicable | Email + in-app; outcome and safe authorized deep link. |
| Cancellation requested / decided | Assigned cancellation reviewer / requester | Email + in-app; no premature balance-restored message before commit. |
| Shift changed | Affected employees with active accounts | Email + in-app; changed dates and authorized schedule link. |
| Attendance correction decided | Requester | Email + in-app; result and authorized detail link. |
| Approval reminder / escalation / blocked routing | Current eligible approver; configured escalation recipient / authorized HR | Email + in-app; stage age and required action, never an automatic decision. |
| Export ready / failed | Requesting account | Email + in-app; link requires login/current authorization; no CSV attachment. |
| Exhausted jobs / critical errors | Configured operator recipient | Operational alert; no photo, token, personal reason, or raw sensitive payload. |

An unprovisioned employee has no notification center; the system must not invent a login account to deliver a shift notification. Missing email/addressability is reported to authorized HR without rolling back scheduling or employment operations. Exact routine notification controls and intervals are OD-15.

| ID | Requirement | Verifiable acceptance |
| --- | --- | --- |
| NTF-001 | Queue email and in-app delivery after commit using a recoverable delivery obligation. | Rolled-back operations send no notification; committed operations survive dispatch/delivery failure and are retried without repeating business mutations. |
| NTF-002 | Provide an owner-only notification center, unread count, and idempotent mark-as-read. | User A cannot list/read/update user B's notifications; repeated read actions preserve the same unread total. |
| NTF-003 | Retry delivery with bounded backoff, deduplication, and exhausted-failure visibility. | Replaying a job creates one center item per recipient/event and no repeated email for a successfully acknowledged channel; provider ambiguity is documented [OD-15]. |
| NTF-004 | Schedule reminders/escalation only for currently pending, eligible stages. | Closed stages stop reminders; expired delegates stop receiving new tasks; one reminder interval does not create duplicate notifications. |
| NTF-005 | Recheck sensitive links and recipient eligibility without disclosing hidden data in messages. | A transferred manager's old link denies access; email contains no raw reason, photo URL, or protected export content. |
| NTF-006 | Monitor/retry scheduled accrual, expiration, reconciliation, export, cleanup, backup, and demo-reset work. | A failed job is visible to authorized operators; retry resumes safely; overlapping scheduler invocations do not double-apply economic effects. |

Scheduled operational jobs use a constrained system actor under approved policies and are auditable; this is not a blanket authorization bypass for user commands. A user-requested export retains its requesting identity and current-access checks even when a worker executes it.

Email delivery is at-least-once infrastructure behavior, not a guarantee of exactly-once receipt by an external provider. Business-level deduplication and delivery tracking are required; ambiguous provider acknowledgement must be handled and documented rather than claiming impossible certainty.

## 17. Security, Privacy, Audit, and Recovery

### 17.1 Security and audit requirements

| ID | Requirement | Verifiable acceptance |
| --- | --- | --- |
| SEC-001 | Enforce backend authentication, action/scope/field authorization, CSRF protection, safe rendering, and validated inputs. | Unauthenticated mutations, forged IDs, mass-assigned protected fields, CSRF requests, and rendered script payloads cannot produce unauthorized effects. |
| SEC-002 | Use secure password storage and HTTPS session configuration with controlled expiry and rate limits. | Cookies have secure production flags; login/reset are rate limited; disabled accounts lose sessions; reset/invitation secrets are not recoverable from logs. Numeric rules follow OD-02. |
| SEC-003 | Keep photos and any supported documents private; validate content and authorize each read. | Public/static URL guessing fails; unsafe/oversized/non-image files fail; temporary links cannot be used as permanent unauthenticated access. |
| SEC-004 | Minimize and separately protect personal, location, and leave-reason data across payloads, errors, logs, CSVs, and email. | Hidden fields are absent, not merely concealed in Vue; audit/log inspection shows no password/token/secret, raw image, or unnecessary precise GPS/reason. Approved OD-08 avoids storing precise coordinates by default; any justified retention needs a validated category policy under approved OD-16 and approved OD-21. |
| SEC-005 | Audit material authentication/account, role, employee/employment, schedule/override, correction, approval/delegation, balance, retention, and sensitive administrative actions. | Each committed material mutation records actor or system origin, action, target, recorded/effective time as applicable, reason, correlation ID, and safe relevant before/after references. |
| SEC-006 | Restrict audit access and prevent ordinary UI editing/deletion of audit entries. | Only granted audit access can query logs; HR/Manager/Employee defaults cannot edit evidence; scheduled retention is separately authorized and itself recorded. |
| SEC-007 | Configure retention by category and run controlled cleanup with dependency/hold checks. | Approved OD-16 keeps durations as company-policy candidates. Destructive real-HR cleanup needs current category/version approval plus legal/policy validation; preview and execution recheck preserve reservations, lineage, references, disputes, evidence, and holds. Material policy/cleanup actions are audited; unapproved categories are never automatically purged. |
| SEC-008 | Automatically back up database and required private files to secure storage and validate restoration. | Restore reestablishes employee history, ledger totals, evidence links, and access controls. Approved OD-18's frequency, 30-day retention target, and recovery objectives remain unchanged; approved OD-16 aligns real-data backup deletion/expiry with validated retention, holds, and deletion obligations before exposure. |
| SEC-009 | Require safeguards for high-impact security, retention, and administrative actions. | Password reauthentication/confirmation and final-admin protection follow OD-01/OD-02; demo users cannot change secrets, security roles, or retention. |
| SEC-010 | Keep secrets out of source control, client bundles, screenshots, CI artifacts, and logs. | Production uses environment configuration; error pages omit stack/secrets; documented secret handling and redaction checks pass. |
| SEC-011 | Limit public-demo exposure to synthetic data and guard privileged operations and outbound email. | Approved OD-20 requires server-selected/validated labeled fixtures only in explicitly isolated synthetic mode, no visitor camera/GPS collection, and fail-closed real-data safeguards. Provide sanitized read-only role comparison without public security grants; reject role/security/retention/privileged writes on the backend. Sandbox email/invitations and invalidate sessions, exports, and jobs on reset; never publish unrestricted Super Admin credentials. |
| SEC-012 | Bound audit/log volume and preserve committed business changes if a downstream logging service fails. | Essential audit data is durable with the mutation; remote log/alert failure is isolated and observable. Routine reads do not create unbounded sensitive audit payloads. |

Critical business audit evidence must be durably recorded with its mutation. If that local evidence cannot be persisted, the critical mutation fails rather than committing without an audit trail. Remote log aggregation and notification delivery remain after-commit/non-blocking concerns. This distinction does not require a separate logging infrastructure product.

### 17.2 Retention and recovery boundaries

Categories must include employee information, structured attendance, attendance photos, leave requests/ledger, audit logs, operational logs, generated exports, backups, and any justified precise-location evidence. Archive and delete are separate actions. Approved OD-16 requires approval/validation per category and policy version before destructive real-HR cleanup; candidate durations, a mechanism approval, or a PRD signature are not legal validation or deletion approval. Material policy changes invalidate the affected approval until revalidated. Previews and execution must protect active reservations, economic lineage/references, disputes, required evidence, and holds. Any controlled tombstone/privacy treatment requires validated policy and legal review.

Synthetic reset and temporary derived-export cleanup have separately approved operational policies and cannot cascade into source HR deletion. Backup cadence/recovery targets remain approved under OD-18; actual expiration/deletion of real-data backup copies must align with validated retention/hold/deletion obligations. Suspended or unapproved real-data category policies cannot be bypassed through backup cleanup.

Backup implementation may be infrastructure scripts/jobs plus documentation. It is not a user-facing backup manager. Restoration must prevent old outbound jobs from sending duplicate invitations/notifications and must reapply deletion/retention controls before an environment is exposed. A backup being created is insufficient evidence that it is restorable.

### 17.3 Indonesian legal review boundary

The original draft recorded official Indonesian regulatory listings for **Law No. 27 of 2022 on Personal Data Protection** and **Government Regulation No. 35 of 2021** concerning fixed-term employment, outsourcing, working/rest time, and termination. These remain legal-review starting points, not an exhaustive or verified-current legal interpretation. The draft's listing-title checks were dated 22 October 2026; full legal text/requirements were not verified, so no article-level obligation, entitlement, retention duration, or amendment status is asserted here.

Sources: [Law No. 27 of 2022 — official registry](https://peraturan.go.id/id/uu-no-27-tahun-2022); [Government Regulation No. 35 of 2021 — official registry](https://peraturan.go.id/id/pp-no-35-tahun-2021).

**No statutory leave amount, probation rule, working-hour limit, consent requirement, or retention duration has been verified as an approved requirement in this PRD.** Operational values are company/demo policy or explicitly pending candidates, not verified statutory requirements. **APPROVED OD-21 requires validation before real employee data is used; its approval does not mean compliance is verified, certified, fully identified, or exempt from legal review.** Validate current employment/personal-data obligations, lawful processing/notice, access/correction/deletion procedures, storage/provider arrangements, and retention exceptions before real-data use. Real-data commercialization/legal operations are not additional MVP modules.

## 18. Data Integrity and Concurrency Requirements

### 18.1 Transaction and command guarantees

| ID | Requirement | Verifiable acceptance |
| --- | --- | --- |
| DATA-001 | Use MySQL transactions for critical state changes and their required ledger/audit effects. | Injecting a failure between request, reservation, decision, debit, and audit writes leaves either the prior state or the complete valid new state. |
| DATA-002 | Protect uniqueness, references, and valid economic state using database constraints and row-level locking where required. | Concurrency tests run on MySQL and prove no duplicate successful clock action, duplicate linked account, or double economic posting. |
| DATA-003 | Lock/revalidate relevant employee-occurrence conflict state and balance state consistently. | Concurrent leave/attendance and schedule-assignment conflicts have one valid business winner. Approved OD-03 protects clocking/offboarding boundaries; approved OD-12 locks every cross-period bucket; approved OD-14 rechecks actual-actor independence. No phantom precheck permits contradictory commits. |
| DATA-004 | Make critical commands idempotent across network and worker retries. | Identical intent replay returns the same result; key reuse with altered payload fails; reservation/debit/reversal/accrual/reminder/export effects are not duplicated. |
| DATA-005 | Detect stale updates to employment, schedules, policies, corrections, and approvals. | A second writer cannot unknowingly overwrite the first writer's business decision; UI receives refresh/review guidance. |
| DATA-006 | Preserve original events, policy context, history, and linked compensating entries. | Corrections retain original employment boundaries, photos/timestamps, and prior evaluation. Approved OD-12 preserves immutable economic meaning and original entitlement/carry-over lineage; approved reversal rules do not edit original debits. Retention remains subject to approved OD-16 approval/holds. |
| DATA-007 | Recover from transient lock/deadlock failures without partial effects or unbounded retries. | Bounded retry either commits once or returns an actionable conflict/error; downstream notifications do not duplicate. |
| DATA-008 | Validate all dates, identities, references, enum values, and precision on the server. | Invalid effective ranges, unsupported half-day values, negative duration, invalid geofence configuration, and forged scope values fail before business commitment. |
| DATA-009 | Stage private uploads safely across nontransactional file storage and MySQL. | A failed DB transaction does not expose a successful attendance photo; orphan cleanup and missing-file alerts cover storage/DB interruption paths. |
| DATA-010 | Recheck authorization and source consistency throughout asynchronous exports. | Job generation and download both evaluate current access; failed/stale generation never exposes a partial or overly broad artifact. |

The architecture must document a shared lock strategy for employee/time-interval conflicts, including cases where no attendance row exists yet. Locking only a nonexistent row or relying solely on a frontend precheck is insufficient. The choice of lock-anchor record, constraint shape, idempotency storage, and deadlock order belongs in architecture/schema documents, not this PRD.

### 18.2 Required race outcomes

| Concurrent operations | Required outcome |
| --- | --- |
| Two clock-ins for one employee/occurrence | At most one successful clock-in; retry can retrieve its outcome |
| Two overlapping shift assignments | At most one conflicting assignment accepted; no published overlap |
| Two leave requests spending the last available unit | Only valid reservations within available balance commit |
| Two reviewers decide the same stage | One state transition; loser sees stale/current state |
| Last-stage approval versus clock-in | No committed overlap between approved excused work and attendance |
| Final approval versus cancellation | One valid request transition with one economic outcome |
| Cancellation retry versus expiry/accrual job | Correct original entitlement bucket, one reversal, no resurrected expired entitlement |
| Attendance correction versus reconciliation | One coherent effective evaluation version; outdated aggregate rebuild cannot replace a newer version |
| Hierarchy/role change versus CSV generation | No artifact delivered under obsolete grants; stale job is rebuilt safely or cancelled |
| Clock-out/clock-in versus effective offboarding | Revalidate access/eligibility at the business boundary; no automatic grace or extension, and unresolved original attendance remains an HR-owned exception [approved OD-03] |
| Multiple accrual periods/retries versus reservations | Cumulative postings advance once through due periods; older jobs do not claw back newer credits and bucket availability remains valid [approved OD-12] |
| Cleanup preview versus approval withdrawal/new hold | Recheck current category/version approval and holds before deletion; a stale preview cannot authorize purge [approved OD-16] |

## 19. Dashboard Metrics and Reporting Requirements

### 19.1 Dashboard behavior

Default persona views must prioritize relevant tasks: Super Admin/HR see authorized operational workforce summaries and exceptions; Manager sees permitted team activity and assigned approvals; Employee sees own schedule, attendance, and leave. Persona changes presentation, not backend permission rules. A custom role receives only permitted capabilities and data. No dashboard response may include hidden source data for the client to discard.

Use KPI cards and interactive line/bar/donut charts where the data supports those representations: attendance trend as a time series, lateness by permitted grouping, leave utilization by chargeable type, and workforce distribution by department/location. A calendar view supports day-level attendance review and drill-down where useful. Provide accessible text/table equivalents; an empty dataset does not render invented chart values.

### 19.2 Approved metric contract [OD-17]

The requirement to define and consistently use these metrics is **CONFIRMED**. The denominator conventions and reconciliation timing are **APPROVED [OD-17]**. Ledger precision, accrual posting, and entitlement-bucket attribution follow approved OD-12; the metric formulas remain unchanged. Do not substitute a different formula in a chart, detail list, or CSV.

#### Common occurrence basis

For each authorized, eligible occurrence `o` in the filtered local shift-start-date range:

- `W(o)` = total scheduled working minutes in its protected schedule context, excluding configured non-working breaks; `W > 0`.
- `L(o)` = scheduled minutes excused by approved, effective, non-overlapping leave segments, including non-balance-consuming leave types; `0 <= L <= W`.
- `R(o) = W(o) - L(o)` = remaining attendance obligation.
- `r(o) = R(o) / W(o)` = required shift-equivalent weight. A full ordinary shift has weight 1; half-day leave normally leaves weight 0.5.
- `P(o)` = 1 if a valid effective clock-in establishes presence in the remaining required segment, otherwise 0 after absence can be finalized. A missing clock-out does not erase valid presence; it is counted separately as incomplete.
- `Late(o)` = 1 when presence exists and arrival exceeds the approved grace threshold relative to the remaining segment's expected start; otherwise 0.

Fully excused leave (`R=0`) and non-working days are excluded from attendance/late denominators. Unassigned schedules and unresolved blocking conflicts are excluded and shown as exception counts. Historical metrics use reconciled evaluations. Required but not yet evaluable occurrences are pending, not absent. These are occurrence-presence measures, **not the percentage of minutes actually worked**.

| Metric | Approved exact formula / basis | Exclusions and display |
| --- | --- | --- |
| Attendance Rate | `100 × sum(r × P) / sum(r)` over evaluable required occurrences | Full leave/non-working/unknown excluded; missing clock-out remains present and separately flagged; zero denominator → N/A |
| Absence Rate | `100 × sum(r × (1-P)) / sum(r)` over the same cohort | Only finalized/evaluable no-presence is absent; same exclusions; zero denominator → N/A |
| Late Rate | `100 × count(present occurrences with Late=1) / count(present occurrences with R>0)` | Count occurrences, not leave-weighted units or late minutes; zero present occurrences → N/A |
| Late arrival statistics | Count of late occurrences; total/mean late minutes from expected remaining-segment start once grace is exceeded [OD-07] | Show evaluable count; do not infer overtime or salary deductions |
| Leave Utilization | Per chargeable leave type and leave year: `100 × net approved consumed units through cutoff / net granted entitlement through cutoff` | Entitlement-based, not submitted/pending requests divided by employee count; details below |
| Workforce Distribution | Distinct employment-eligible employees at the chosen as-of date, grouped by effective department/location/position/type | Default includes probation + active [OD-03]; one employee once per grouping; no account requirement |
| Attendance Trends | Per start-date bucket: present equivalents `sum(r×P)`, absent equivalents `sum(r×(1-P))`, excused leave equivalents `sum(L/W)`; rates use the same definitions | Unknown/pending counts separate; use ratio of summed weights for a period, never mean daily percentages |

**Leave Utilization denominator details [OD-12/OD-17]:**

1. The tile/chart is explicitly labeled **Leave-year entitlement utilization — through [cutoff]**. The selected end date determines the cutoff, capped at today for realized reporting; future approved leave is shown separately as planned consumption.
2. For each employee/type/receiving leave year, net granted entitlement is allocations + accruals + inbound carry-over + signed entitlement adjustments, effective by cutoff. Exclude reservations, consumption debits/reversals, and expiration debits from this denominator: expiration reduces spendable balance but does not erase the fact that entitlement was granted. Show expired units separately. Attribute carry-over to its receiving year and retain original lineage; do not also count the original prior-year grant in the receiving-year denominator.
3. The numerator is approved consumption for occurrences from that leave year's start through cutoff, less linked approved cancellation/correction reversals for those occurrences. Include full and half-day units; exclude pending/rejected/cancelled consumption and future planned occurrences.
4. The date filter's start does not turn this annual entitlement measure into a monthly entitlement allocation. Show the leave year/basis beside it. For a range touching multiple leave years, show separate per-year results rather than a misleading combined percentage. Other date-filtered leave counts may show activity strictly within the selected range and must be labeled differently.
5. Group percentages use sums of numerators and denominators over the same authorized employees/type/year, never the average of individual percentages. Do not combine incompatible leave units/types. Non-balance-consuming types display approved duration/count, with utilization N/A.
6. Zero entitlement → N/A. Positive consumed units with zero/negative entitlement, negative entitlement, or utilization above 100% is a visible integrity/policy exception, never clamped to hide it. Report carry-over/adjustment effects so reviewers can explain a denominator change; report expiry separately without inflating utilization. For example, a 10-day grant with 5 consumed and 5 expired remains 50% utilization.

Approved OD-12 supplies the source-accounting details without changing these approved formulas: use actual posted entitlement, not future expected accrual; retain four-decimal economic inputs independently of UI rounding; split consumption by shift-start period and receiving bucket lineage; do not double-count original and carried grants. Cumulative accrual corrections and linked reversals must feed the same auditable source versions used by dashboard/drill-down/CSV.

The owner approved this annual entitlement-utilization convention in OD-17. Entitlement consumption and scheduled-work absence remain different measures; no formula change is introduced by this revision.

#### Worked acceptance fixture [OD-17]

Four reconciled occurrences are in scope: A works an 8-hour shift and is present; B has first-half leave on an 8-hour shift and is present for the rest; C has a 12-hour shift and is absent; D has full-day approved leave on an 8-hour shift. One of A/B is late.

Required equivalents = `1 + 0.5 + 1 = 2.5`; present equivalents = `1.5`. Attendance Rate = **60%**; Absence Rate = **40%**; Late Rate = **50%** (one of two present occurrences). Approved leave totals **1.5 days**. If the authorized type/year's net granted entitlement is 20 days and this is its only consumption, Leave Utilization = **7.5%**. D is not counted absent; C's longer shift still represents one shift-equivalent. Missing B's clock-out adds one incomplete record without changing these presence rates.

### 19.3 Provisional, reconciled, and corrected reporting

**CONFIRMED:** current-day data may be provisional; historical reporting uses reconciled records; corrections update aggregates audibly; overnight records belong to their start date; zero-denominator percentages show N/A.

**APPROVED [OD-17]:** reconcile after each occurrence's allowed clock-out window plus a configurable delay. A frequent job processes newly eligible occurrences; a nightly sweep finds missed work. On current-day views, rates use completed/evaluable occurrences and are marked provisional until reconciliation; show ongoing, not-yet-due, incomplete, and unresolved counts separately. An ongoing overnight occurrence remains pending even if its start date is yesterday. A historical period with unresolved/unreconciled entries shows incomplete coverage; it must not be labeled fully finalized.

Corrected evaluations create a new version and invalidate the affected employee/date/group aggregates after commit. Until rebuild finishes, expose stale/reconciling state or compute an authorized fresh result; never silently present old cached totals as current. UI, drill-down, and CSV must share a defined cutoff/version. An export with changing source versions must produce a coherent snapshot or restart/cancel; it must not mix incompatible partial results. Archive audit references to old/new metric inputs rather than making aggregates the source of truth.

Historical department/location grouping uses the effective context for the occurrence start date, or the selected workforce as-of date. Current team authorization is still applied first. Filter options and counts must not leak employees outside permitted scope.

### 19.4 Dashboard and export requirements

| ID | Requirement | Verifiable acceptance |
| --- | --- | --- |
| DASH-001 | Provide role-aware, permission-aware, scope-aware dashboards without hardcoded role bypasses. | Employee sees own data; manager sees current team plus self only if granted; custom-role view contains only permitted source records. |
| DASH-002 | Support date, department, and location filters plus authorized drill-down. | Chart selection, card totals, detail pagination, and CSV use the same filters, scope, dates, and metric basis. |
| DASH-003 | Provide required KPIs, attendance/late/leave/workforce visualizations and useful calendar review. | Charts have labeled axes/units/legend and equivalent accessible values; selected points lead to permitted detail. |
| DASH-004 | Use one consistent metric contract, including half-day weighting, start-date attribution, and N/A rules. | The worked fixture passes in backend calculation, dashboard output, drill-down summaries, and CSV under approved OD-17. |
| DASH-005 | Distinguish provisional, reconciled, stale, and incomplete reporting. | An open overnight shift is not shown as finalized absence; unresolved historical exceptions show coverage/freshness information. |
| DASH-006 | Recalculate affected aggregates after authorized corrections without losing prior history. | A corrected late arrival changes relevant totals after reconciliation; audit identifies its old/new evaluation and aggregate refresh. |
| DASH-007 | Export authorized employee, attendance, leave, and analytics CSVs with metadata and safe values. | Report identifies filters, timezone, cutoff/freshness, units, and generation time; formula-like cell contents cannot execute as spreadsheet formulas. |
| DASH-008 | Queue large exports and securely deliver only complete authorized results. | User can see job status; authorization is rechecked at generation and download; expired or stale artifacts fail safely. Large threshold is OD-19. |
| DASH-009 | Paginate/filter/sort detail data on the server and bound report ranges. | A table does not preload the full employee/attendance dataset; invalid sort fields/filter values and excessive ranges are rejected or constrained. |

## 20. Non-Functional Requirements

### 20.1 Confirmed performance baseline

The following are **targets**, not verified achievements. Prior internship measurements do not establish these results.

| ID | Workload / metric | Required target and measurement boundary |
| --- | --- | --- |
| NFR-001 | Dataset and load | 500 active employees, 250,000+ historical attendance records, 50 concurrent users; include representative hierarchy, schedules, leave, photos, and exceptions |
| NFR-002 | Standard backend requests | P95 ≤ 500 ms, measured at the server ingress through completed response for ordinary authenticated reads/mutations; report each critical endpoint separately |
| NFR-003 | Attendance processing | P95 ≤ 1 second after the complete photo reaches the server through committed business result and response; include validation, image processing/private persistence, MySQL, authorization, and synchronous audit |
| NFR-004 | Dashboard loading | P95 ≤ 3 seconds through usable data and rendered interactive charts in the benchmark browser; record server and browser timings separately |
| NFR-005 | Large CSV exports | Asynchronous background generation; HTTP request enqueues/reports status rather than waiting for full generation; threshold follows OD-19 |
| NFR-006 | Reproducibility | Record dataset generator/seed, sizes/distributions, environment, versions, run commands, concurrency/mix, timings, errors, and raw-result reference |

Photo-upload network time is excluded only from NFR-003's specified timing boundary; measure/report it separately. Queueing email and expensive aggregate refreshes may follow commit. The initial committed attendance result must not be faked before evidence/transaction success. Dashboard endpoint requests belong to NFR-004; exports/uploads and dashboard work are not silently mixed into the ordinary-request statistic. Authentication/validation/locking time remains part of the relevant server timing.

Approved OD-20's public simulated evidence is not proof of genuine capture/geofence behavior or production attendance performance. Benchmark the real validated photo-processing path in a production-like non-demo environment; do not meet NFR-003 by bypassing it with the public fixture path.

**APPROVED benchmark environment/method [OD-19]:** 2 vCPU, 4 GiB RAM, SSD-backed storage; app, MySQL, and worker resource allocations documented on one benchmark host; independent load generator. Use a recorded browser/network profile for dashboard measurements (approved profile: 50 ms RTT and 10 Mbps). Generate at least 250,000 attendance records for 500 active employees plus historical employees, multi-level teams, overnight shifts, transfers, holidays, partial leave, and corrected/missing events. Document any synthetic photo reuse and keep realistic payload sizes.

Use 50 concurrent authenticated virtual users with documented pacing, a representative mix (approved 60% standard reads, 20% clock actions on eligible distinct occurrences, 10% leave/approval writes, 10% dashboard), two-minute warm-up and ten-minute measured interval. Ensure fixture replenishment does not turn writes into duplicate-rejection traffic. Report per-endpoint sample counts, P50/P95/P99, successful/failed/conflicting outcomes, and error rates. Run cold-cache behavior separately from the steady-state target; do not hide errors/timeouts or average percentiles. Record exact CPU/memory, Docker/OS/database configuration and application commit. Change these approved environment details only through the product change-control process.

### 20.2 Additional quality attributes

| ID | Requirement | Verifiable acceptance |
| --- | --- | --- |
| NFR-007 | Prevent N+1 queries; index measured critical queries; paginate/filter/sort server-side. | Query inspection and representative load show bounded queries for a paginated list; benchmark evidence supports any added optimization. |
| NFR-008 | Use efficient aggregation/selective caching with authorization-aware keys and invalidation. | Role/hierarchy changes and corrections do not expose stale unauthorized data; rebuild cannot overwrite a newer source version. |
| NFR-009 | Provide responsive, keyboard-operable interfaces with accessible labels, focus, validation, and non-color-only states. | Login, clocking, leave, approvals, filters, and tables can be completed by keyboard where device capture permits; errors identify fields/actions; charts have text equivalents. |
| NFR-010 | Make camera attendance usable on supported mobile and desktop browsers over HTTPS. | The support matrix in approved OD-20 is verified privately against genuine permission/capture/retake/upload/GPS/error/retry behavior; public simulation cannot replace these checks. Unsupported real-capture devices receive the documented correction path. |
| NFR-011 | Provide structured logs, centralized exception handling, correlation IDs, and redaction. | One failed request/job can be traced by correlation ID without exposing credentials, private photos, or sensitive reasons. |
| NFR-012 | Provide protected operational visibility, health checks, failed-job monitoring, and critical alerts. | Dependency failure changes health status and raises a bounded alert; public health response contains no infrastructure secrets. Exact checks/thresholds follow architecture/OD-15. |
| NFR-013 | Keep module boundaries maintainable and critical logic testable. | Business rules execute consistently outside the browser; future tasks do not duplicate leave/attendance calculations in Vue or exports. |
| NFR-014 | Support Docker Compose development and containerized public-demo deployment with secure configuration. | A clean setup starts configured services; HTTPS demo uses synthetic HR/Manager/Employee accounts, labeled server-controlled fixtures, and a read-only permission comparison. Approved OD-20 requires deployment/runtime safeguards rejecting fixture activation in real-data mode; hosting remains open. |
| NFR-015 | Recover safely from interrupted background and external work. | Restarting workers/scheduler does not double-post balances or deliver stale exports; failures are visible and retryable. |
| NFR-016 | Document operational procedures matching the implementation. | Setup, seed/demo credentials, queue/scheduler, deployment, backup/restore, retention, benchmark, and known limitations are reproducible at release. |

No Prometheus/Grafana deployment, uptime SLA, multi-region service, or unlimited scale is required. Lightweight logs, health endpoints, failure records, and operator alerts are sufficient when they satisfy the requirements.

## 21. Testing and Acceptance Criteria

### 21.1 Risk-based test strategy and CI gates

| ID | Requirement | Required evidence |
| --- | --- | --- |
| QA-001 | Use Pest unit tests for deterministic business calculations and state/permission resolution. | Schedule/half-day/ledger/metric/multi-role fixtures cover the approved rules, including cumulative accrual rounding, period eligibility, geofence modes, and distinct-stage approvers. |
| QA-002 | Use Pest feature/integration tests for backend authorization, transactions, files, jobs, and workflow transitions. | Tests use actual backend routes/commands and MySQL for lock/constraint/concurrency behavior; a simplified in-memory database is not concurrency evidence. |
| QA-003 | Use Playwright for critical end-to-end user journeys and accessible interaction states. | Login, leave review, dashboard, and capture success/failure are verified. Test public read-only RBAC, denied direct mutations, no visitor camera/GPS calls, and refused production fixture activation [approved OD-20]. Private production-like real-device checks supplement deterministic mocks. |
| QA-004 | Make required quality checks blocking in GitHub Actions. | Relevant Pest suites, critical integration/concurrency tests, frontend type checks/lint/build, and critical Playwright journeys pass; exact tooling is selected later. |
| QA-005 | Complete critical SIT and UAT scenarios before release. | SIT proves cross-module consistency; UAT records Super Admin/HR/Manager/Employee completion and sign-off. This PRD does not create SIT/UAT documents yet. |
| QA-006 | Keep implementation tasks bounded and traceable for Codex. | Future tasks have unique task IDs, dependencies, permitted scope, referenced requirements/decisions, tests, acceptance criteria, status, and Definition of Done. |
| QA-007 | Treat failed relevant checks as incomplete work and verify material fixes. | No task is marked complete with failing required checks; ignoring a failure cannot waive an approved requirement or mandatory business guard. |
| QA-008 | Validate performance, security, private storage, recovery, and observability with release evidence. | Reproducible benchmark report, authorization/security tests, monitored failure exercise, and successful isolated restore accompany the release. |

Prioritize tests for financial-like leave accounting, scope isolation, original evidence preservation, and concurrency. Do not use an arbitrary line-coverage percentage as a substitute for those assertions. Run deterministic fixture tests in CI; performance and real-device checks may be controlled release jobs/manual checks with recorded evidence. A failure in any required release gate still blocks release.

### 21.2 Explicit scenario acceptance catalog

All scenarios AC-01–AC-48 are binding acceptance criteria under the approved requirements and Operational Decisions. Their workflow, configuration, and data preconditions remain applicable. Every original AC-01–AC-42 identifier is retained.

| Scenario | Given / When / Then | Requirements and test level |
| --- | --- | --- |
| AC-01 | **Given** an employee without an account, **when** two invitation-provision commands race, **then** at most one account link exists and retry returns the existing provisioning outcome. | AUTH-001, AUTH-007; Pest integration/concurrency |
| AC-02 | **Given** expired, revoked, or already accepted invitation/reset tokens, **when** reused, **then** activation/reset fails without changing account access. | AUTH-001, SEC-002; Pest feature |
| AC-03 | **Given** an active session, **when** the account is disabled, **then** its next request and any existing private-file/export access fail. | AUTH-001, AUTH-006; Pest + Playwright |
| AC-04 | **Given** `employee.view=all` and `leave.approve=team`, **when** attempting an assigned out-of-team approval, **then** leave approval fails; unrelated broad scope is not reused. | AUTH-003, AUTH-008; Pest unit/feature [OD-01] |
| AC-05 | **Given** Manager + Employee grants, **when** loading records, **then** exact-permission team/self unions apply and omitted sensitive/photograph grants do not leak those fields. | AUTH-003, AUTH-004; Pest [OD-01] |
| AC-06 | **Given** a current manager has an old employee link, **when** reporting hierarchy changes, **then** subsequent detail, chart, notification link, and export download checks use the new team. | AUTH-006, DATA-010; Pest integration |
| AC-07 | **Given** a department/reporting graph and future assignments, **when** a cycle is introduced, **then** the complete invalid change is rejected and current/future valid history remains. | ORG-001, ORG-002, EMP-005; Pest |
| AC-08 | **Given** a transferred employee, **when** viewing old attendance and current workforce, **then** old organizational context and current effective workforce context remain distinct. | EMP-002, SHIFT-007, DASH-004; Pest |
| AC-09 | **Given** an ended employment period, **when** rehired, **then** identity/history are reused, a new period is recorded, old attendance (including unresolved sessions) remains attached to its original period, and balances/access are not silently reset. | EMP-004, EMP-006; Pest + UAT [OD-03/OD-12] |
| AC-10 | **Given** a normal working Saturday and a company holiday with location exception, **when** an authorized special assignment applies, **then** resolved work follows Section 10 precedence. | SHIFT-002, SHIFT-005; Pest [OD-06] |
| AC-11 | **Given** a 22:00–06:00 assignment, **when** assigning 05:00–13:00 next day or racing equivalent writes, **then** no overlapping interval can commit. | SHIFT-004, DATA-003; MySQL concurrency |
| AC-12 | **Given** attendance or approved leave references a shift, **when** its template/calendar is edited, **then** original evaluations/debits stay unchanged and a conflicting historical change requires explicit resolution. | SHIFT-006, SHIFT-007, LEAVE-012; integration |
| AC-13 | **Given** an eligible employee and allowed window in a private production-like non-demo test, **when** a genuine camera-capture photo is clocked in/out, **then** validated private evidence and server timestamps are stored once and displayed only under authorized access; a public fixture result is not equivalent evidence. | ATT-001, ATT-002, ATT-012; Pest + Playwright [OD-09/OD-20] |
| AC-14 | **Given** duplicate clicks or a lost successful response, **when** retrying the same command, **then** one successful clock event exists and the prior result is recoverable. | ATT-003, ATT-013, DATA-004; concurrency + E2E |
| AC-15 | **Given** clocking at the exact early/grace/latest boundary and one second either side, **when** evaluated, **then** approved inclusive/exclusive and lateness rules apply before display rounding. | ATT-004, ATT-006; Pest [OD-07] |
| AC-16 | **Given** an overnight open occurrence with account/employment eligibility still valid at clock-out, **when** clock-out occurs the following day/month/year, **then** it closes the original occurrence and reports under its local start date. Termination-intersection behavior is covered separately by AC-43. | ATT-002, RULE-009, DASH-004; integration |
| AC-17 | **Given** denied camera, oversized/spoofed image, or mandatory invalid/inaccurate/outside GPS, **when** submitted, **then** no successful attendance is created and a specific recovery message appears. With radius 200 m/accuracy limit 100 m, d=200/a=100 passes Standard but fails Strict uncertainty; d=100/a=100 passes Strict; a=100.01 fails both and d=200.01 fails Standard. Comparisons use unrounded values and client mode/manual-coordinate claims cannot bypass policy. | ATT-005, ATT-012, SEC-003; Pest + E2E [OD-08/OD-09] |
| AC-18 | **Given** location enforcement disabled, **when** GPS is unavailable and valid photo/policy checks pass, **then** absence of coordinates alone does not block clocking. | ATT-005; Pest + E2E |
| AC-19 | **Given** valid clock-in and no clock-out, **when** reconciliation becomes due, **then** presence is retained, incompleteness is visible, and no clock-out is fabricated. | ATT-007, ATT-010; Pest [OD-17] |
| AC-20 | **Given** an attendance correction, **when** its submitter or affected employee attempts self-review, or two reviewers decide simultaneously, **then** self-review fails and only one independent valid decision can apply. | ATT-009, AUTH-008, DATA-005; concurrency [OD-10] |
| AC-21 | **Given** half-day leave on an overnight or ordinary shift, **when** previewing and approving, **then** covered work segments, excluded breaks/days, expected arrival, and 0.5-day charge agree; an otherwise permitted clock-out just after second-half leave begins does not invent worked time in that segment. | LEAVE-004, ATT-006; Pest [OD-11] |
| AC-22 | **Given** one eligible posted day in a bucket, **when** two one-day requests for different non-overlapping dates submit concurrently, **then** at most one reserves successfully; available balance never becomes negative and a different period's surplus cannot fund the deficit. | LEAVE-005, LEAVE-006, DATA-002; MySQL concurrency [OD-12] |
| AC-23 | **Given** two sequential approval stages, **when** stage two acts early or an unassigned/all-scope user acts, **then** the decision fails without advancing any stage or changing the ledger. | LEAVE-007, AUTH-008; integration |
| AC-24 | **Given** a first-stage approval and valid period/bucket reservations, **when** the final stage approves, **then** the validated reservation allocation becomes linked consumption debits atomically and no released reservation remains active or duplicates the debits. | LEAVE-008, DATA-001; integration/fault injection [OD-12] |
| AC-25 | **Given** pending leave, **when** rejected/cancelled and retried, **then** its reservation is released once and no consumption reversal is fabricated. | LEAVE-009, LEAVE-010; Pest |
| AC-26 | **Given** approved cancellable leave, **when** cancellation is approved twice or races expiry/carry-over, **then** only the originally consumed eligible units are reversed once to their retained lineage, no new-year grant is fabricated, and expired units are not made spendable. | LEAVE-010, DATA-004; integration [OD-12/OD-13] |
| AC-27 | **Given** due accrual/carry-over/expiration jobs, **when** retries and out-of-order execution overlap, **then** each scope/bucket has one economic effect and older accrual jobs do not reverse newer postings. For a 1-day annual amount and 12 fully eligible months, cumulative four-decimal half-up postings total 1.0000 rather than 0.9996; two-decimal display does not feed calculation. | LEAVE-002, LEAVE-003; unit + MySQL concurrency [OD-12] |
| AC-28 | **Given** a delegate whose authority expired, lost scope, or conflicts with mandatory distinct-stage review, **when** deciding, **then** the decision fails. Existing 24-hour reminder/48-hour escalation intervals remain; reminders/reassignment never approve and cannot create a self-approval or independence bypass. | LEAVE-011, NTF-004; Pest [OD-14/OD-15] |
| AC-29 | **Given** full approved leave and a clock attempt, or an approval racing clock-in, **when** both paths evaluate the occurrence, **then** no conflicting worked/excused interval can commit. | ATT-011, LEAVE-013, DATA-003; MySQL concurrency |
| AC-30 | **Given** an approved correction to historical lateness, **when** re-reconciled, **then** prior evidence remains, aggregates refresh, and dashboard/drill-down/CSV agree on the new version. | ATT-010, DASH-005, DASH-006; integration |
| AC-31 | **Given** Section 19's four-occurrence fixture, **when** calculated across dashboard/detail/CSV, **then** attendance is 60%, absence 40%, late 50%, and the stated leave fixture is 7.5%. | DASH-004; Pest + E2E [OD-17] |
| AC-32 | **Given** no eligible work, no present occurrences, or zero entitlement, **when** computing the relevant percentage, **then** N/A and its basis are shown without division-by-zero or invented zeros. | DASH-004, DASH-005; Pest + E2E |
| AC-33 | **Given** an out-of-scope filter/record/photo/export ID, **when** requested directly, **then** the server discloses no unauthorized rows, hidden fields, counts, or file bytes. | AUTH-004, SEC-003, DASH-002; negative feature tests |
| AC-34 | **Given** a queued export and subsequently revoked permission/hierarchy, **when** generated/downloaded, **then** current authorization prevents stale data delivery; CSV strings are formula-safe. | DASH-007, DASH-008, DATA-010; integration |
| AC-35 | **Given** notification provider failure after a valid leave/shift commit, **when** jobs retry, **then** business state stays committed and delivery is deduplicated/observable. | NTF-001, NTF-003; integration/fault injection |
| AC-36 | **Given** a critical mutation whose durable audit write fails, **when** executing, **then** the mutation rolls back; if only remote logging fails, committed state survives and operator visibility remains. | SEC-005, SEC-012, DATA-001; fault injection |
| AC-37 | **Given** real-HR cleanup candidates older than candidate durations, an unapproved category/version or a hold/dispute/reservation, **when** cleanup runs, **then** source records/evidence/lineage remain. Withdrawing approval or adding a hold after preview also blocks deletion at execution. Separately approved temporary-export cleanup may remove the artifact without deleting source HR records; policy/material cleanup actions are audited. | SEC-007, SEC-008, DATA-006; integration/operational [OD-16/OD-21] |
| AC-38 | **Given** an automatic database/private-file backup, **when** restored in isolation, **then** sampled ledger totals/history/evidence links/auth checks agree and old outbound jobs do not send unexpectedly. | SEC-008, NFR-016; recovery exercise [OD-18] |
| AC-39 | **Given** the approved 500-employee/250,000+-record/50-user fixture, **when** benchmarked reproducibly on the genuine validated non-demo attendance path, **then** per-endpoint P95 and dashboard targets are measured against the approved thresholds, with failures and environment reported. | NFR-001–NFR-006; release performance [OD-19] |
| AC-40 | **Given** public demo HR/Manager/Employee accounts, **when** viewing the sanitized read-only permission comparison and then directly attempting role/grant/security/retention/invitation/backup/operator/reset mutations, **then** the comparison is readable without security administration grants and backend guards deny privileged writes. Authorized scheduled reset invalidates old sessions, jobs, and exports; full role editing is demonstrated only privately/locally. | AUTH-002, AUTH-005, SEC-011, NFR-014; Pest + Playwright/demo smoke [OD-20] |
| AC-41 | **Given** required forms/charts and validation failures, **when** operated via keyboard or narrow supported viewport, **then** focus/labels/errors and accessible chart values support completion. | NFR-009, NFR-010; Playwright + manual smoke |
| AC-42 | **Given** an organizational-root employee with no direct manager, **when** routing opens a required manager stage, **then** an explicitly configured eligible independent fallback fills that same stage with read/approval scope and authority. Without such a candidate, submission fails with no reservation, or a later stage remains visibly blocked for HR; disabled/self/ineligible candidates never skip/approve the stage. | LEAVE-006, LEAVE-007, LEAVE-011, AUTH-008; integration [OD-14] |
| AC-43 | **Given** a 22:00–06:00 active session and scheduled termination at 00:00, **when** pre-application review detects the intersection, **then** HR sees an owned conflict and may explicitly audit a boundary adjustment or use independent correction. If the boundary is unchanged, account access ends at 00:00 and ordinary clock-out fails; no extension/event is fabricated. Reviewed resolution preserves original dates/evidence and rehire cannot rewrite that old session. | EMP-003–EMP-006, AUTH-001, ATT-002, ATT-007–ATT-010, DATA-003; integration + E2E [OD-03/OD-10] |
| AC-44 | **Given** a future half-day request with zero eligible posted units and 0.5 day only in forecast accrual, **when** reserving, **then** reject with the affected period/reason, no request/reservation debit or fictional credit. An actually posted eligible valid bucket may fund the same request; future/unclosed accrual remains excluded. | LEAVE-002, LEAVE-003, LEAVE-005, LEAVE-006; Pest integration [OD-12] |
| AC-45 | **Given** working occurrences starting 31 December overnight and 1 January, **when** full-day leave spans them, **then** charge one unit to each start-date period using eligible posted buckets in deterministic expiry order. Missing next-period entitlement rejects the whole reservation; concurrent requests cannot overspend any bucket and carry-over/reversal retain lineage without duplicate source/receiving availability. | LEAVE-002, LEAVE-005, LEAVE-006, LEAVE-008, LEAVE-010, DATA-002, DATA-003, DASH-004; MySQL integration/concurrency [OD-12] |
| AC-46 | **Given** an actor who approved stage one and a distinct-review workflow, **when** the actor is selected/delegated/reassigned to a later stage, **then** reject reuse even with multiple roles/Super Admin. A separate workflow fixture may allow reuse only under an explicit authorized reasoned snapshotted exception audited at configuration/use; requester self-approval and cascading decisions still fail. | AUTH-008, LEAVE-007, LEAVE-011, RULE-006; Pest feature/integration [OD-14] |
| AC-47 | **Given** a non-demo or real-data environment, **when** a user/worker supplies demo flags, fixture IDs, headers, roles, or a capture-failure fallback, **then** no synthetic attendance bypass is activated and no invalid event commits. A configuration combining synthetic activation with a real-data dataset is rejected by environment/deployment guards. | AUTH-004, ATT-001, ATT-005, SEC-011, NFR-014; Pest + deployment negative tests [OD-20] |
| AC-48 | **Given** an explicitly isolated synthetic public demo, **when** a visitor completes attendance, **then** only server-selected/validated synthetic image/location evidence is used, simulated labels are visible, no real camera/geolocation API is requested and no visitor evidence is stored. Unknown/client-forged fixtures fail; normal permission/time/schedule/leave/duplicate guards still apply. | ATT-001–ATT-005, SEC-004, SEC-011, QA-003; Pest + Playwright browser-API assertions [OD-20] |

## 22. Quality-Gated MVP Release Criteria

Phase 2 must not begin until the MVP release baseline is established. Release requires all of the following; no performance or security claim is inferred from a passing build.

| ID | Release gate | Pass evidence |
| --- | --- | --- |
| REL-001 | All six modules and confirmed requirements are implemented and accepted. | Requirement-to-test evidence covers every mandatory row; no Phase 2 feature substitutes for incomplete MVP behavior. |
| REL-002 | Maintain the approved requirements baseline without reducing confirmed scope. | Section 24 records final owner approval of all 21 Operational Decisions on 22 October 2026. Any later change to an approved requirement must follow the documented product change-control process before dependent implementation/release. |
| REL-003 | Critical defects are resolved. | No unresolved defect causing unauthorized disclosure/escalation, data loss, incorrect economic state, contradictory attendance/leave, or blocked essential workflow. |
| REL-004 | Required automated checks pass. | Pest, integration/concurrency, Playwright, frontend checks/build, and GitHub Actions required checks pass on the candidate commit. |
| REL-005 | Critical SIT and persona UAT pass. | Recorded cross-module and independent reviewer scenarios, including negative paths and role/scope boundaries, receive owner sign-off. |
| REL-006 | Performance and security are validated. | Approved benchmark meets BR-28 targets on the genuine validated path; private files, scope/field isolation, sessions, redaction, backend demo restrictions, and rejected synthetic-mode activation in real-data environments pass. |
| REL-007 | Deployment and recovery function. | HTTPS synthetic demo, workers/scheduler, health/failure monitoring, secure backup, and isolated restore evidence are complete. Approved OD-16 blocks unvalidated destructive real-HR cleanup; approved OD-21 requires legal/privacy validation before real-data use, without claiming that validation is already complete. |
| REL-008 | Documentation matches implementation and a stable version is established. | Downstream documentation, operational instructions, known limitations, and traceability match the release; create a stable Git baseline such as `v1.0.0`. |

An approved change to a locked target must update this PRD explicitly; reporting a failed target as achieved is not a release exception. Documentation creation in this assignment does not assert any release gate has passed.

## 23. Phase 2 Compatibility and Agentic Delivery

### 23.1 Extension boundaries

| Future module | MVP foundation it may consume | Boundary retained in MVP |
| --- | --- | --- |
| Payroll | Stable employee/employment identity, effective organization/type, reconciled attendance, approved leave | Do not add salary, tax, payroll formulas, or payout flows now |
| Overtime | Shift expectations, original clock evidence, corrected/reconciled attendance | Early/late/extra presence is evidence, not approved overtime or compensation |
| Performance Management | Stable employee/reporting hierarchy and permissions | Operational dashboard rates must not become employee performance scores |
| Recruitment / ATS | Existing identity/account-provisioning boundary for eventual hiring | No applicant/vacancy/pipeline entities or workflows now |

Phase 2 extends the same modular application. Preserve stable business identifiers and conceptual contracts; avoid speculative Phase 2 tables, endpoints, UI, or microservices. Architecture may allow extension without implementing dormant features.

### 23.2 pen.dev and Codex responsibilities

Product documentation defines requirements, policies, permissions, workflows, and acceptance. pen.dev explores layouts, reusable patterns, visual components, screens, and interaction states. Codex implements approved Vue Single File Components, Tailwind/shadcn-vue patterns, Inertia integration, Laravel logic, and tests. Neither tool may redefine business rules through a design or implementation shortcut.

PRD approval is complete. The sequence continues with architecture/schema → design system → pen.dev screens → Codex frontend/backend integration → automated/visual checks → SIT/UAT → stable MVP release. Local technical choices are permitted within approved boundaries. A future `TASK.md` must use unique task IDs, dependencies, clear scope, requirement/OD references, tests, acceptance criteria, Definition of Done, and status tracking. Tasks use this approved baseline; future changes requiring owner approval must block only their affected work until approved.

### 23.3 Readiness and primary risks

Version 1.0 is the Approved Requirements Baseline and the primary product-requirements source for `ARCHITECTURE.md`, `DATABASE-SCHEMA.md`, and subsequent Phase 1 documentation. All 28 locked business decisions and all 21 Operational Decisions govern downstream work; no Operational Decision remains awaiting owner approval. Provider/hosting selection remains explicitly open as specified, and OD-21 still gates real-data use on legal/privacy validation. Approved requirements and architecture readiness do not imply that implementation or MVP release gates have passed.

No locked BR or approved operational behavior needs to be weakened for these revisions. Remaining trade-offs are timely HR handling when offboarding prevents ordinary clock-out; Standard GPS usability versus uncertain boundary evidence; rejection of future leave without posted entitlement; limited eligible independent approvers; storage growth while retention approval/holds block deletion; and fail-closed demo isolation. The approved scope union, history protection, cancellation/metric formulas, and performance targets remain unchanged. The product owner accepted these policy trade-offs with final approval on 22 October 2026; legal compliance and performance achievements are not asserted.

## 24. Operational Decisions & Approval Status

The product owner gave final approval to **all 21 Operational Decisions on 22 October 2026**. The 15 initially approved definitions and the six final revised definitions are preserved without business changes. Every decision below has status **`APPROVED`**. Approval of the PRD/retention mechanism does not itself validate law or authorize real-data deletion. Hosting/provider selection remains open where the approved definition explicitly leaves it open.

### OD-01 — Permission catalog, default grants, and conflict resolution

- **Business context:** Multi-role authorization is locked; exact keys, default role matrix, and grant conflict semantics are approved below.
- **Approved operational definition:** Use Section 8's catalog/matrix and permission-specific grant unions; grant-only rules, no explicit denies/inheritance. Multiple required permissions intersect their record sets. Global capabilities and own-notification actions never elevate employee scope. Approval requires read + approve access + stage assignment. Protect the last active security administrator; HR cannot modify security grants or deactivate a security administrator without security authority. `leave.delegation.manage` scope targets the source approver whose delegation is being changed, not the leave requester.
- **Rationale:** Makes effective access predictable and prevents unrelated broad grants from leaking into other actions.
- **Risk / impact if changed:** Alters policies, UI navigation, query scopes, exports, delegation eligibility, and security tests; resolve before authorization/schema baselining.
- **Status:** `APPROVED`.
- **Approval record:** Approved by the product owner / developer on 22 October 2026.

### OD-02 — Account lifecycle, sessions, tokens, and sensitive actions

- **Business context:** Authentication flows are locked; operational security thresholds are approved below.
- **Approved operational definition:** Invitation expires after 48 hours; reset after 60 minutes; single-use secrets, resend revokes old invitation. Minimum 12-character passwords with no arbitrary composition pattern. Idle session timeout 30 minutes, absolute lifetime 8 hours; no persistent remember-me in MVP. Rate-limit login/reset initially at 5 attempts/minute per relevant account/IP key with safe non-enumerating responses. Require password reauthentication within 15 minutes for role/grant changes, sensitive account access changes, and retention/deletion changes. Invalidate sessions on deactivation and password reset. No public self-registration.
- **Rationale:** Provides bounded access and controlled invitation-only onboarding.
- **Risk / impact if changed:** Changes account UX, attack exposure, rate-limit behavior, and authentication tests; future changes require owner review.
- **Status:** `APPROVED`.
- **Approval record:** Approved by the product owner / developer on 22 October 2026.

### OD-03 — Employment lifecycle, effective dates, and offboarding

- **Business context:** Effective-dated employment and independent account status must remain correct when termination intersects an active or overnight attendance session.
- **Approved operational definition:** Retain planned/probation/active/ended employment periods; probation/active are work-eligible, employment type remains separate, and HR-authorized cross-department reporting remains subject to cycle/eligibility checks. Keep non-overlapping effective intervals with an exclusive end boundary and the location-timezone 00:00 default for date-only events. Before scheduling and immediately before applying offboarding, inspect active attendance/ongoing shift intersections, future shifts, pending leave/reservations, and assigned approval responsibilities. Present conflicts and an owned resolution work item to authorized HR. HR may explicitly adjust the effective boundary with a reason, retained original date/instant, actor, and audit trail; a deliberate boundary adjustment may identify an exact instant. Otherwise retain the scheduled end, disable account access when effective, and use approved OD-10's independently reviewed correction/resolution. An unresolved session never extends eligibility or keeps an account enabled for clock-out. Do not fabricate timestamps/photos or silently close the session; retain incomplete evidence and an explicit reviewed resolution if a factual end cannot be established. Factual attendance correction does not itself revise employment dates or reactivate access. Preserve original schedule expectations and explicitly resolve eligible work segments before reconciliation; never silently penalize scheduled time beyond the employment end. Future changes apply when due; contract expiry flags HR review rather than inventing resignation. Offboarding cancels pending leave/reservations under audited rules and requires explicit resolution of approved future leave and assigned approvals. Rehire retains identity, creates a new employment period, separately authorizes account reactivation, and never rewrites old attendance or automatically resets balances. Backdated changes affecting protected records require impact review and linked corrections.
- **Rationale:** Protects access revocation while giving HR a controlled path for legitimate work evidence at a termination boundary.
- **Risk / impact if changed:** HR may need timely intervention; without an approved boundary adjustment, ordinary clock-out is unavailable after termination. Preserve accountable exception handling instead of automatic extension or guessed closure.
- **Status:** `APPROVED`.
- **Approval record:** Final revised definition approved by the product owner / developer on 22 October 2026.

### OD-04 — Employee mandatory fields and field visibility

- **Business context:** Employee records may exist without login; exact data collection and field exposure are approved below.
- **Approved operational definition:** Require unique employee code, full name, employment start, employment type, and an initial lifecycle state. Before work activation require department, position, level, and location; manager is optional for the organizational root but required where an approval route uses one. Require a unique login email only when provisioning an account; contact phone/email otherwise optional. Basic visibility: employee code, name, work assignment, manager, business status. Sensitive visibility: private contact details, private employment notes, and leave reasons, under separate permission; immutable security secrets are never viewable. Manager receives leave dates/type/status by default, not private reasons/photos. No national ID, bank, salary, or medical document collection by default; no employee self-editing beyond account credentials in MVP.
- **Rationale:** Supports useful records with minimal sensitive data and clear activation validation.
- **Risk / impact if changed:** Alters forms, validation, privacy controls, exports, and possibly real-data obligations.
- **Status:** `APPROVED`.
- **Approval record:** Approved by the product owner / developer on 22 October 2026.

### OD-05 — Timezones, work intervals, and breaks

- **Business context:** Overnight shifts and effective history require consistent temporal boundaries.
- **Approved operational definition:** Store authoritative instants with UTC semantics and retain each occurrence's IANA timezone; synthetic company default Asia/Jakarta, configurable locations. Intervals are start-inclusive/end-exclusive; adjacent work intervals may touch. Support one continuous attendance session per occurrence. Templates have an elapsed interval greater than zero and below 24 hours, positive remaining working duration, and an explicit end day; non-working breaks are configured within the interval, do not overlap, and are excluded from leave/expected-work calculations. A shift crossing a holiday/month/year uses its local start date. Preserve original timezone on transfer. Date changes at DST or invalid local times must be rejected/explicitly resolved if such zones are configured, even though the initial demo uses Indonesian zones.
- **Rationale:** Prevents midnight ambiguity and inconsistent date/interval arithmetic without requiring multi-session attendance.
- **Risk / impact if changed:** Affects schedule keys, overlap locking, half-day segmentation, clock matching, ledger periods, and reporting.
- **Status:** `APPROVED`.
- **Approval record:** Approved by the product owner / developer on 22 October 2026.

### OD-06 — Schedule precedence, publication, and calendar updates

- **Business context:** Calendar priority is locked; base schedule precedence and update effects are approved below.
- **Approved operational definition:** Use Section 10's resolution: explicit authorized special assignment > location exception > company calendar > regular schedule; within regular schedule use dated assignment > anchored rotation > weekly recurrence. Do not resolve equal-priority conflicts by last-write-wins. Batch publication is all-or-nothing after impact preview. Preview 90 days for open-ended recurrence, but revalidate all subsequently materialized occurrences and every affected finite assignment interval. Calendar/template changes apply prospectively unless explicitly reviewed; referenced historical attendance/approved leave is protected. Pending leave is marked stale for recalculation.
- **Rationale:** Preserves the locked calendar hierarchy while making rotation/override behavior deterministic.
- **Risk / impact if changed:** Alters occurrence resolution, calendar UI, batch transactions, long-range validation, and historical protections.
- **Status:** `APPROVED`.
- **Approval record:** Approved by the product owner / developer on 22 October 2026.

### OD-07 — Attendance policy precedence, windows, and classifications

- **Business context:** Configurable attendance policy is locked; exact order, numeric windows, and exception behavior are approved below.
- **Approved operational definition:** Per-field inheritance `shift > location > company`. Early clock-in allowed from 60 minutes before expected remaining-work start; latest clock-in is strictly before remaining-work end. Grace is 5 minutes; later arrival is late and duration is measured from expected start, not after subtracting grace. Clock-out must be later than clock-in and no later than 4 hours after expected end; early departure is allowed but flagged, not hidden or treated as overtime. Evaluate exact seconds before display rounding. Missing clock-out is flagged when its window closes; never auto-insert clock-out. Unassigned schedules block ordinary clocking and require HR assignment/correction. Reconciliation timing is OD-17.
- **Rationale:** Offers usable synthetic defaults while preserving evidence and distinguishing policy violations from processing failures.
- **Risk / impact if changed:** Changes employee outcomes, late/early statistics, overnight matching, correction volume, and boundary tests. Values are company policy, not legal standards.
- **Status:** `APPROVED`.
- **Approval record:** Approved by the product owner / developer on 22 October 2026.

### OD-08 — Geolocation enforcement and uncertainty

- **Business context:** A single conservative uncertainty formula causes avoidable boundary rejections; geofencing needs explicit security/usability modes and privacy limits.
- **Approved operational definition:** Keep work-location enforcement configurable and disabled until configured/approved; optional GPS is collected only if that policy requests it. Initial radius is 200 m and maximum reported accuracy 100 m. Backend-selected Standard mode initially accepts valid finite evidence when unrounded distance `d <= radius` and `0 <= accuracy <= maximum`; equality is accepted. Configurable Strict mode additionally requires `d + accuracy <= radius`. Invalid/negative/non-finite values fail. A client cannot choose a weaker mode or disable mandatory enforcement. A reading at d=200 m, accuracy=100 m passes Standard but fails Strict for a 200 m radius; d=100 m, accuracy=100 m passes Strict. Explain that uncertainty may cross the fence in Standard and that neither mode proves physical presence or prevents GPS spoofing. Use the occurrence's work location for both actions, including overnight clock-out. Return distinct actionable denied, unavailable/timeout, invalid, insufficient-accuracy, outside-radius, and strict-uncertainty errors. Offer retries or authorized correction, never a manual coordinate claim as equivalent evidence. Do not persist precise coordinates by default; retain minimal validation outcome/mode and only necessary non-precise evidence. Any justified precise-coordinate retention needs a validated purpose/category policy under OD-16/OD-21 and must not exceed its approved duration; no raw coordinates in logs/CSV.
- **Rationale:** Makes boundary behavior predictable and usable while retaining server-side policy enforcement and explicit uncertainty.
- **Risk / impact if changed:** Standard accepts some readings whose uncertainty extends outside the radius; Strict causes more false rejections. GPS remains uncertain evidence and requires correction support and retention discipline.
- **Status:** `APPROVED`.
- **Approval record:** Final revised definition approved by the product owner / developer on 22 October 2026.

### OD-09 — Camera, photo validation, and authoritative receipt time

- **Business context:** Camera photos and server timestamps are locked; capture failure and file boundaries are approved below.
- **Approved operational definition:** Require live camera UI capture for each action with retake; do not offer a gallery fallback. Accept decoded JPEG/PNG/WebP up to 5 MiB, shorter dimension at least 480 px and longer dimension at most 4096 px; reject SVG/GIF, invalid bytes, and deceptive MIME/extension combinations. Apply orientation, strip EXIF metadata, and store privately. Use first complete server upload receipt time for a successfully validated idempotent command; preprocessing does not change it. Failed camera/upload creates no clock event and directs the employee to retry/correction. No offline backdating or assertion that a browser can cryptographically prove live identity/capture.
- **Rationale:** Bounds processing, prevents unsafe file handling, and defines the performance/timestamp boundary.
- **Risk / impact if changed:** Affects browser support, privacy, evidence size, late classification, storage cost, and the one-second target.
- **Status:** `APPROVED`.
- **Approval record:** Approved by the product owner / developer on 22 October 2026.

### OD-10 — Attendance correction authority and stale requests

- **Business context:** Authorized corrections and audit are locked; initiation and concurrent pending behavior are approved below.
- **Approved operational definition:** Employees request corrections to their own occurrences; HR may initiate a documented correction for an employee without access using `attendance.resolve`. An independent eligible reviewer must approve; neither the submitting actor nor the affected employee may self-review. Require reason, original context, requested values, and current version; allow one pending correction per occurrence. Do not overwrite originals or delete photos. A stale request requires explicit revalidation before approval. A material cross-module conflict must be resolved before applying the correction.
- **Rationale:** Covers non-account employees and preserves separation of duties without a direct unreviewed editing shortcut.
- **Risk / impact if changed:** Affects correction UI, permission guards, reviewer availability, audit, and concurrency rules.
- **Status:** `APPROVED`.
- **Approval record:** Approved by the product owner / developer on 22 October 2026.

### OD-11 — Leave duration, half days, overlaps, and schedule changes

- **Business context:** Shift-aware full/half-day leave is locked; its precise units and stale-calculation behavior are approved below.
- **Approved operational definition:** Use one full or first/second half of scheduled working time per occurrence, excluding breaks; charge 1 or 0.5 day regardless of shift length. Half boundaries use elapsed scheduled working time, retaining exact precision for odd-duration shifts. Dates refer to shift starts. Exclude resolved non-working days; block unknown schedules, zero-duration requests, known attendance overlaps, overlapping pending/approved leave segments, and backdated employee self-service submission. Disjoint half-day requests may coexist. Apply Section 15.1's boundary-event interpretation: late clock-out evidence alone does not assert work throughout excused time, while approval against an unresolved open session is blocked. Pending leave does not excuse work, so valid attendance may proceed and blocks later conflicting approval. A changed pending schedule requires explicit recalculation and requester acceptance/resubmission before final approval. Approved calculation remains frozen.
- **Rationale:** Defines half-day behavior and prevents silent extra debits or unintended absences.
- **Risk / impact if changed:** Changes request data, schedule segmentation, balance units, conflicts, approvals, and dashboard weighting.
- **Status:** `APPROVED`.
- **Approval record:** Approved by the product owner / developer on 22 October 2026.

### OD-12 — Leave policy precedence, periods, accrual, and precision

- **Business context:** Leave accounting needs deterministic cumulative rounding, earned-versus-expected separation, and bucket lineage across periods and retries.
- **Approved operational definition:** Retain one policy/type/date resolved as employee override > location > company, no department layer, and a 1 January–31 December leave year in company timezone. Annual grant or monthly accrual is explicitly selected, never both implicitly; amounts are company policy, not statutory claims. Keep month-end-in-arrears accrual based on annual amount/12 × eligible calendar-day fraction, and annual new-hire proration based on eligible remaining calendar days/year days. Retain four-decimal ledger day units, nearest-half-up rounding, and two-decimal UI display; approved half-day usage remains exactly 0.5. Compute authoritative cumulative entitlement from unrounded eligible contributions through the due period; post only its rounded cumulative difference from prior signed accrual postings under lock. A processed-through marker makes retry/older jobs no-ops; exclude future/unclosed periods. Negative historical recalculation requires an authorized compensating adjustment, never automatic reservation invalidation. Forecast accrual is not posted or available: future leave can reserve only eligible actually posted units valid for each covered period, otherwise reject explicitly without borrowing or fictional credits. Split charges by occurrence shift-start date and entitlement period, including overnight and cross-year requests. Allocate valid buckets by earliest expiry, then valid-from date and immutable bucket ID; non-expiring buckets follow expiring ones. Reserve all periods atomically or none; lock/reconcile each bucket so one period's surplus cannot hide another's deficit. Preserve original grant/carry-over lineage through paired source carry-out and receiving carry-in, consumption, expiry, and linked reversal. Carry-over remains disabled until cap/expiry is configured; transfer only eligible unreserved remainder. Keep valid reservations against their covered dates protected through expiry; release/reversal of no-longer-spendable units posts a linked expiry effect without creating availability. Final approval consumes the validated allocation and releases reservations atomically. Economic entries are immutable; changes use explicit linked adjustments/reallocation with audit and reservation checks. No negative available balances or implicit unearned borrowing.
- **Rationale:** Prevents drift, double grants, cross-year double spending, and resurrection of expired entitlement while preserving the approved cancellation and metric contracts.
- **Risk / impact if changed:** More precise allocation/reconciliation is required; future leave may be rejected until entitlement is actually posted. Policy changes and backfills need deliberate compensating entries, and approved analytics must consume the same posted ledger lineage.
- **Status:** `APPROVED`.
- **Approval record:** Final revised definition approved by the product owner / developer on 22 October 2026.

### OD-13 — Cancellation, reversal, and balance adjustment

- **Business context:** Cancellation and consistent restoration are locked; authority and timing are approved below.
- **Approved operational definition:** Requester may cancel pending leave before final approval; release the reservation once. Self-service approved cancellation is whole-request and must be requested before its first covered work segment; independent HR reviews it. Approved leave remains effective while cancellation is pending. Historical/partially used leave requires HR resolution identifying affected occurrence units, with linked compensating entries and attendance reconciliation; it is not silently treated as an ordinary future cancellation. Restore to original entitlement lineage and honor expiry; do not create new-year entitlement. Negative adjustments cannot consume reserved units or produce invalid available balance; resolve existing commitments first.
- **Rationale:** Makes reversal behavior auditable and minimizes accidental double restoration.
- **Risk / impact if changed:** Changes request states, cancellation UI, ledger lineage, expiry rules, and cross-module resolution complexity.
- **Status:** `APPROVED`.
- **Approval record:** Approved by the product owner / developer on 22 October 2026.

### OD-14 — Approval routing, delegation authority, and escalation

- **Business context:** Organizational-root employees need a valid first-stage route, and sequential levels need meaningful default separation of duties.
- **Approved operational definition:** Keep direct manager → configured eligible HR approver/pool as the initial workflow, with configurable ordered manager/named-account/permission-bearing-role-pool selectors. Snapshot the workflow definition at submission; resolve each stage on opening and revalidate at decision. Where no direct manager exists, fill that same stage using an explicitly configured eligible independent fallback account/pool with required read/approval permissions, scope, and configured authority. Other ineligible-manager fallbacks require explicit configured triggers or HR reassignment. No eligible first approver means rejected submission without reservation; a later missing eligible approver visibly blocks the stage and alerts HR, preserving pending state/reservation until authorized action. HR intervention does not grant workflow-configuration rights: correcting initial fallback configuration still requires `leave.workflow.manage`. Default to distinct actual decision-makers across sequential stages; exclude prior approving actors and apply the same check to delegates/reassignments. A repeated-actor exception must be explicitly configured by an actor with `leave.workflow.manage`, reasoned, included in the submitted workflow snapshot, and audited at configuration and use; it never permits self-approval, skipped stages, or cascading decisions. Multiple roles/Super Admin/delegation cannot bypass mandatory guards. Retain self-task delegation with `leave.delegation.manage=self` and authorized HR control of allowed source approvers; delegates already need permission/scope, with no chaining or self-delegation. Inclusive delegation dates mean start-date 00:00 through exclusive 00:00 the day after end date in company timezone, stored as explicit instants. Retain reminders after 24 elapsed hours and escalation after 48, at most one reminder/escalation per 24 hours per stage. HR reassigns only undecided stages with reason; escalation never approves, and previous decisions remain preserved.
- **Rationale:** Supports executives/root employees without invented reporting lines and makes independent stages effective rather than merely separate buttons.
- **Risk / impact if changed:** Small teams can lack enough eligible independent actors; they must configure legitimate fallback/explicit exception or await HR intervention. No exception can bypass requester self-approval.
- **Status:** `APPROVED`.
- **Approval record:** Final revised definition approved by the product owner / developer on 22 October 2026.

### OD-15 — Delivery retries, notification timing, and operator alerts

- **Business context:** Both channels, retry/backoff, deduplication, and operational visibility are locked; exact intervals are approved below.
- **Approved operational definition:** Queue both channels with one durable event/recipient/channel identity; target notification-center visibility within one minute under normal operation. Use five total delivery attempts: initial, then 1/5/15/60-minute retry delays; expose exhausted delivery to authorized operators. Revalidate current task recipients before new reminders. Track provider acknowledgements and flag ambiguous sends rather than promising exactly-once external email. Alert on exhausted critical jobs, unavailable database/storage, and persistent scheduler/worker failure; deduplicate repeated alerts. Approval intervals follow OD-14; no notification preference bypass for required workflow tasks.
- **Rationale:** Keeps business commitment independent of messaging while making delivery failures actionable.
- **Risk / impact if changed:** Alters notification load, user expectations, alert volume, and failure tests; thresholds require environment tuning.
- **Status:** `APPROVED`.
- **Approval record:** Approved by the product owner / developer on 22 October 2026.

### OD-16 — Retention, archival, and deletion safeguards

- **Business context:** Operational retention candidates are not validated legal durations or approval to destroy real HR evidence.
- **Approved operational definition:** Preserve candidate company-policy values: attendance photos 90 days; operational logs 30 days; exports 24 hours; audit records 365 days; structured employment/attendance/leave history five years after employment ends. None is a verified statutory requirement. Track approval/validation by data category and policy version; real-HR records, attendance evidence, audit history, leave ledger, and any justified precise-location data cannot be purged before applicable company policy and legal requirements are validated and the current category policy is explicitly approved. Unapproved/pending/suspended categories fail closed. Material duration/scope changes require renewed validation/approval, not reuse of old approval. Preview consequential cleanup; immediately before execution recheck current policy approval, active reservations, ledger lineage/references, unresolved disputes, required evidence and authorized holds. Archival does not imply deletion permission. Audit policy changes, approvals, holds and material cleanup outcomes without exposing sensitive content. Keep routine synthetic-demo reset and temporary derived-export cleanup as separate bounded operational policies; deleting an export must not delete source HR records. Preserve OD-18's approved daily backups, 30-day recovery retention target, and restore objectives; real-data backup expiration/deletion must also honor validated category/hold/deletion obligations and recovery policy. Restore reapplies current retention/deletion controls before exposure. OD-21 real-data legal validation remains mandatory.
- **Rationale:** Enables policy-based operation without treating unverified example durations or a PRD signature as destructive authorization.
- **Risk / impact if changed:** Approval/holds can extend storage beyond candidate periods, increasing cost and review work. Blind cleanup risks privacy/evidence loss; backup retention needs aligned handling rather than an automatic legal exemption.
- **Status:** `APPROVED`.
- **Approval record:** Final revised definition approved by the product owner / developer on 22 October 2026.

### OD-17 — Analytics denominator contract and reconciliation timing

- **Business context:** Hybrid reporting and listed metrics are locked; denominator and finalization conventions are approved below.
- **Approved operational definition:** Use Section 19's weighted attendance/absence, occurrence-based late rate, annual entitlement utilization, workforce as-of basis, coverage labels, and worked fixture. Include probation/active employees for workforce. Use current hierarchy for access and effective history for grouping. Reconcile 15 minutes after an occurrence's latest clock-out window; run eligibility checks every 15 minutes with a nightly recovery sweep. Open/unknown/conflicted occurrences stay provisional/pending. Corrections invalidate affected aggregates immediately after commit; label stale until refreshed. Preserve report cutoff/evaluation version consistency and N/A handling.
- **Rationale:** Gives reproducible reports without penalizing half-day/full-day leave or finalizing overnight shifts prematurely.
- **Risk / impact if changed:** Affects accepted business meaning, aggregate dimensions, dashboard queries, CSV reconciliation, and tests; settle before reporting schema/design.
- **Status:** `APPROVED`.
- **Approval record:** Approved by the product owner / developer on 22 October 2026.

### OD-18 — Backup frequency, recovery objectives, and restore validation

- **Business context:** Automatic secure backups and validated restoration are locked; frequency/recovery objectives are approved below, while provider selection remains open.
- **Approved operational definition:** Daily encrypted database + private-file backup to access-restricted storage separate from the application host; retain 30 days. Target RPO ≤ 24 hours and RTO ≤ 4 hours, subject to a measured restore. Run one isolated restore before MVP release and monthly thereafter; verify ledger/history samples, file links, authorization, retained deletion controls, and suppressed outbound jobs. Keep a practical operator runbook; provider remains open.
- **Rationale:** Covers host failure without creating a backup-management application.
- **Risk / impact if changed:** Alters infrastructure cost, data-loss exposure, recovery effort, and release evidence; targets are not achieved until tested.
- **Status:** `APPROVED`.
- **Approval record:** Approved by the product owner / developer on 22 October 2026.

### OD-19 — Benchmark specification, report bounds, and export threshold

- **Business context:** BR-28 targets are locked; benchmark resources, mix, and operational bounds are approved below.
- **Approved operational definition:** Use Section 20's 2-vCPU/4-GiB environment, 50-user mix, warm-up/measurement windows, browser/network profile, separate cold-cache reporting, and per-endpoint pass evidence. Default tables to 25 rows, cap a page at 100; default operational date range to 30 days with a validated explicit range. Queue all CSV generation for a uniform status/download workflow; classify more than 10,000 rows as large and measure it separately. Set hard report-range/job-size caps only after representative measurement and owner approval, not silently in an agent implementation.
- **Rationale:** Makes timing claims falsifiable and avoids blocking HTTP on unpredictable export sizes.
- **Risk / impact if changed:** Alters comparability, infrastructure needs, query bounds, worker capacity, and frontend status UX. Locked performance targets remain unchanged.
- **Status:** `APPROVED`.
- **Approval record:** Approved by the product owner / developer on 22 October 2026.

### OD-20 — Demo hosting, reset, and supported clients

- **Business context:** The public demo must show attendance and Dynamic RBAC without collecting visitor evidence or exposing security administration.
- **Approved operational definition:** Use an explicitly configured synthetic-demo environment and server-controlled synthetic-only dataset guard. Allow only server-selected/validated camera/location fixtures; label capture/location as simulated and do not claim real-world attendance validation. Never request or persist visitors' real photos or precise GPS. Fixture paths must fail closed outside the explicit synthetic environment, and unsafe synthetic/real-data environment combinations must be rejected; client flags, headers, fixture IDs, roles, and permission failures cannot activate a bypass. Retain all normal authorization, time, duplicate, schedule, leave, and transaction guards when demonstrating fixtures. Test real camera/browser permissions/GPS failures and both geofence modes in private production-like environments. Publish synthetic HR/Manager/Employee accounts without unrestricted Super Admin credentials. Provide a sanitized read-only comparison of configured permission matrix/role settings so visitors can observe role differences; expose no actual security administration or sensitive account assignments and do not change OD-01's default grants. Deny public role/security/retention/privileged mutations server-side, including direct requests; demonstrate full Dynamic RBAC privately/locally under owner control. Sandbox external email and block invitation abuse, backups, and operator actions. Keep nightly Asia/Jakarta reset with old sessions/exports/queued jobs invalidated; owner/operator alone may reset. Keep the initial current/previous-stable Chrome/Firefox/Edge desktop and current Android Chrome/iOS Safari client matrix, verified at implementation, English initial UI, and documented unsupported real-capture correction path. Select hosting only after image/queue/database/backup/HTTPS requirements are evaluated.
- **Rationale:** Demonstrates the core engineering features while separating synthetic evidence from real attendance assurance and restricting public administration.
- **Risk / impact if changed:** Public simulation cannot prove real-device correctness; private production-like tests are still required. Misconfigured environments could become a bypass unless deployment/runtime guards and negative tests fail closed; reset must not deliver stale queued work.
- **Status:** `APPROVED`.
- **Approval record:** Final revised definition approved by the product owner / developer on 22 October 2026.

### OD-21 — Real-data legal and privacy validation

- **Business context:** Independent development and synthetic public data are locked; applicable current legal obligations have not been fully verified.
- **Approved operational definition:** Keep public/demo data synthetic. Before any real-data use, have the owner validate current Indonesian employment and personal-data rules and organization policy using complete primary sources/qualified review; record validated entitlements, processing/notice, sensitive data treatment, retention/holds, provider/storage arrangements, and rights-request procedures. Do not encode unverified statutory amounts or claim regulatory compliance. Required privacy handling can use authorized operational procedures; it does not justify adding an unrelated compliance module.
- **Rationale:** Distinguishes verified legal requirements from configurable portfolio policies.
- **Risk / impact if changed:** Real-data use can materially alter retention, fields, access, working-time/leave policy, and deployment assumptions.
- **Status:** `APPROVED`.
- **Approval record:** Approved by the product owner / developer on 22 October 2026.

## 25. Requirements Traceability Matrix

### 25.1 Coverage of all 28 locked decisions

All 28 BR entries remain confirmed. All referenced ODs supply approved operational definitions; their approval records are authoritative in Section 24. Requirement, workflow, and acceptance references remain unchanged.

| Locked decision | Primary requirement anchors / sections | Acceptance evidence anchors | Operational decisions (status in Section 24) |
| --- | --- | --- | --- |
| BR-01 Dynamic RBAC | AUTH-002, AUTH-005; Section 8 permission catalog | AC-04, AC-05; role administration UAT; AC-40 | OD-01 |
| BR-02 Configurable data scope | AUTH-004, AUTH-006, RULE-008 | AC-06, AC-33, AC-34 | OD-01 |
| BR-03 Hierarchical organization | ORG-001–ORG-004, EMP-002 | AC-07, AC-08, AC-42 | OD-03–OD-05, OD-14 |
| BR-04 Separate employee/user | AUTH-007, EMP-001, EMP-003, EMP-004, EMP-006 | AC-01–AC-03, AC-09; AC-43 | OD-02–OD-04 |
| BR-05 Hybrid shifts | SHIFT-001–SHIFT-004, SHIFT-007, SHIFT-009 | AC-10–AC-12, AC-16; AC-43 | OD-05, OD-06 |
| BR-06 Photo/geolocation attendance | ATT-001–ATT-009, ATT-012, ATT-013 | AC-13–AC-20; AC-43, AC-47, AC-48 | OD-07–OD-10 |
| BR-07 Multi-level leave approval | LEAVE-006–LEAVE-009 | AC-23, AC-24, AC-42; AC-46 | OD-14 |
| BR-08 Interactive analytics | DASH-001–DASH-009; Section 19 | AC-30–AC-34, AC-41 | OD-17, OD-19 |
| BR-09 Controlled conflict resolution | ATT-011, LEAVE-013, RULE-003, RULE-004; Section 15 | AC-12, AC-20, AC-29, AC-30; AC-43 | OD-06, OD-10, OD-11, OD-13 |
| BR-10 Configurable leave policy | LEAVE-001–LEAVE-005, LEAVE-009, LEAVE-010, LEAVE-014 | AC-21, AC-22, AC-25–AC-27; AC-44, AC-45 | OD-11–OD-13, OD-21 |
| BR-11 Delegation/escalation | LEAVE-011, NTF-004 | AC-28, AC-42; AC-46 | OD-14, OD-15 |
| BR-12 Audit/security | SEC-001–SEC-007, SEC-009–SEC-012 | AC-03–AC-06, AC-17, AC-33, AC-36, AC-37, AC-40; AC-47, AC-48 | OD-01, OD-02, OD-04, OD-16, OD-20, OD-21 |
| BR-13 Email + in-app notifications | NTF-001–NTF-006; Section 16 event matrix | AC-28, AC-35 | OD-14, OD-15, OD-20 |
| BR-14 Transactional consistency | DATA-001–DATA-010, RULE-005, RULE-007, RULE-010 | AC-01, AC-11, AC-14, AC-20, AC-22–AC-29, AC-36; AC-43, AC-44, AC-45, AC-46 | OD-05, OD-10–OD-14 |
| BR-15 Retention/recovery | SEC-007, SEC-008; Section 17.2 | AC-37, AC-38 | OD-16, OD-18, OD-21 |
| BR-16 Observability | NFR-011, NFR-012, NTF-006, SEC-004, SEC-012 | AC-35, AC-36, AC-38; health/alert exercise | OD-15 |
| BR-17 Performance-oriented architecture | DASH-008, DASH-009, NFR-007, NFR-008 | AC-34, AC-39; query inspection | OD-19 |
| BR-18 Risk-based tests/CI | QA-001–QA-008 | AC-01–AC-48; required CI checks | Operational policies; status in Section 24 |
| BR-19 Guardrailed agentic work | QA-006, QA-007; Sections 1.2 and 23.2 | Future task audit and requirement-to-check links | No independent scope changes |
| BR-20 Public demo/Docker | SEC-011, NFR-010, NFR-014, NFR-016 | AC-13, AC-17, AC-40, AC-41; deployment smoke; AC-47, AC-48 | OD-20 |
| BR-21 Quality-gated release | REL-001–REL-008 | Completed release evidence and stable Git baseline | All acceptance-critical ODs |
| BR-22 Effective employment history | EMP-002–EMP-006, ORG-002, RULE-001 | AC-07–AC-09, AC-12; AC-43 | OD-03–OD-05 |
| BR-23 Attendance policy | ATT-004, ATT-006, ATT-007, ATT-010; Section 11.1 | AC-15, AC-16, AC-19 | OD-05, OD-07, OD-17 |
| BR-24 Holiday hierarchy | SHIFT-003, SHIFT-005, SHIFT-006, SHIFT-009 | AC-10, AC-12, AC-16 | OD-06 |
| BR-25 Shift-aware leave | LEAVE-004, LEAVE-005, LEAVE-010, LEAVE-012, LEAVE-013 | AC-12, AC-21, AC-26, AC-29 | OD-05, OD-11–OD-13 |
| BR-26 Multi-role + scope | AUTH-003, AUTH-006, AUTH-008; Section 8.3 | AC-04–AC-06, AC-20, AC-23, AC-28; AC-46, AC-47 | OD-01, OD-14 |
| BR-27 Hybrid analytics | ATT-010, DASH-004–DASH-007; Section 19 | AC-19, AC-30–AC-32, AC-34; AC-45 | OD-07, OD-11, OD-12, OD-17 |
| BR-28 Measured baseline | NFR-001–NFR-006, QA-008 | AC-39; raw benchmark/environment evidence | OD-09, OD-19 |

### 25.2 Workflow and module completeness

| MVP module | End-to-end workflows | Primary acceptance clusters |
| --- | --- | --- |
| Authentication & Authorization | WF-01–WF-03 | AC-01–AC-06, AC-33, AC-34, AC-40, AC-46, AC-47 |
| Employee & Organization Management | WF-04–WF-06 | AC-07–AC-09, AC-42, AC-43 |
| Shift Management | WF-07–WF-08 | AC-10–AC-12, AC-16, AC-43 |
| Attendance Management | WF-09–WF-11 | AC-13–AC-20, AC-29, AC-30, AC-43, AC-47, AC-48 |
| Leave Management | WF-12–WF-16 | AC-21–AC-29, AC-42, AC-44–AC-46 |
| Dashboard & Analytics | WF-17–WF-18 | AC-30–AC-34, AC-39, AC-41, AC-45 |

Supporting security, messaging, recovery, demo, and operational requirements are covered by AC-33–AC-41, AC-47–AC-48, and the relevant requirement-specific acceptance columns. This mapping records intended coverage, not executed test results. The document covers 6/6 primary modules and 28/28 locked decisions. All 21 OD IDs are APPROVED, with final approval recorded on 22 October 2026. All original 136 requirement IDs, WF-01–WF-18, and AC-01–AC-48 remain; AC-43–AC-48 retain targeted revision coverage. The whole PRD is the Approved Requirements Baseline.

## 26. Glossary

| Term | Meaning in this PRD |
| --- | --- |
| Account / user | Authentication identity; optional one-to-one employee link, with independent access status |
| Employee identity | Stable workforce identity retained across employment periods and rehire |
| Employment period | Effective-dated interval of employment with its own lifecycle; not a new employee on rehire |
| Employment type | Contractual/work arrangement category, separate from lifecycle and login state |
| Effective time / recorded time | When a business fact applies / when the application recorded it |
| RBAC | Role-based grants of action permissions; no implicit role inheritance |
| Scope | Employee-record set for one permission: all, current team, or self |
| Team | Direct and indirect subordinates in the currently active reporting graph, excluding self unless separately granted |
| Grant union / permission intersection | Combine grants for one permission; then require every necessary permission to cover the target |
| Shift template / occurrence | Reusable local-time schedule definition / one employee's concrete resolved work interval |
| Special assignment | Explicit authorized working/non-working override at the highest calendar priority |
| Overnight shift | One occurrence ending after its start date; reports under its local start date |
| Protected context | Preserved schedule/policy/organization/calculation information referenced by historical business records |
| Clock event | Server-timestamped attendance action with required private photo evidence |
| Correction / resolution | Independently reviewed change to effective evidence interpretation / authorized handling of a cross-module exception |
| Reconciliation | Validation and final evaluation of eligible source records; repeatable and auditable after corrections |
| Provisional / stale / incomplete | Not finalized / newer source exists than the displayed aggregate / unresolved or unreconciled coverage remains |
| Attendance obligation | Scheduled working time remaining after approved excused leave |
| Shift-equivalent | Fraction of one occurrence used by the approved attendance metric; not worked hours or pay |
| Leave unit / bucket | Day-based accounting unit under approved OD-11 / entitlement lineage with approved period and expiry rules under OD-12 |
| Posted / reserved / available | Ledger-derived balance / pending commitment / posted balance minus active reservations |
| Consumption reversal | Linked compensating transaction undoing authorized leave consumption without editing its original debit |
| Leave utilization | Approved annual entitlement-consumption ratio; distinct from scheduled-work absence and pending requests |
| Approval stage | One ordered required decision with valid assignee(s), current permissions, and no self-approval |
| Delegation / reassignment / escalation | Temporary eligible decision authority / explicit change of undecided assignee / overdue notification or intervention request, never automatic approval |
| Idempotency | Repeating the same business intent does not create another economic or operational effect |
| P95 | Value at or below which 95% of measured samples fall; reported with workload and timing boundary |
| RPO / RTO | Approved target maximum recoverable data-loss interval / target time to restore usable service; achievement requires restore evidence |
| SIT / UAT | System Integration Testing / User Acceptance Testing |
| Operational KPI | Workforce/attendance/leave measure; not Phase 2 employee performance scoring |







