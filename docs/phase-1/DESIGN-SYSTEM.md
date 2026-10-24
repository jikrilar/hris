# Professional HRIS — Phase 1 Design System

## 1. Document Information & Status

| Attribute | Value |
| --- | --- |
| Product | Professional HRIS — Phase 1 MVP |
| Canonical path | `docs/phase-1/DESIGN-SYSTEM.md` |
| Version | 1.0 — Approved Design System Baseline |
| Status | **Approved Design System Baseline** |
| Date | 23 October 2026 (Asia/Jakarta) |
| Initial interface language | English |
| Theme | Light-first; light UI only in the initial MVP design set |
| Audience | Project owner, pen.dev designer/agent, Codex CLI developer, reviewer and tester |
| Output | This specification only; no Vue code, stylesheet, `.pen` asset, screenshot or task backlog is produced |
| Approval | Approved by the project owner on 23 October 2026 (Asia/Jakarta); individual screen approvals remain bound to their identified revisions |

This document selects one visual and interaction system for all six MVP business modules. It specifies reusable foundations, workflow-aware views, responsive behavior and a concrete design-to-code handoff. Values and examples are design contracts, not new business policies. Implement components only when a bounded feature needs them; the inventory is not an instruction to scaffold unused abstractions.

## 2. Source Documents & Design Authority

The complete available canonical files were inspected, including all source sections, requirements/workflows, schema table dictionaries and state-bearing columns. Their content and fingerprints were checked directly for this task.

| Authority | Source / verified status | SHA-256 |
| --- | --- | --- |
| 1 — Product | [PRD.md](PRD.md), v1.0 Approved Requirements Baseline; all 21 ODs APPROVED; 28 locked business decisions; 136 requirement IDs; WF-01–WF-18; AC-01–AC-48 | `aad0e06d15a7c8b92629a2ed04e174bfaac2b5acbc7a4bcac3678a14d03dc494` |
| 2 — Engineering | [ARCHITECTURE.md](ARCHITECTURE.md), v1.0 Technical Architecture Baseline; modular monolith; ADR-001–ADR-015 | `77c424ff082417f574cce93f01725a5b522df0234b80ee4228e5e4f6937429b2` |
| 3 — Supporting persistence | [DATABASE-SCHEMA.md](DATABASE-SCHEMA.md), v1.0 Approved Database Schema Baseline; 90 selected tables; bounded attendance clarification accepted by the owner in G-000/T-0003 on 24 October 2026 | `0891deb5d59d96094e654467c5e6e94bf9962a4bfebfa7401ef71a28bff2d441` |
| 4 — Visual/interaction | This Design System, approved | Cannot override higher authorities |
| 5 — Screen composition | Future owner-approved pen.dev frame at an identified revision | Cannot authorize a feature, disclosure, workflow shortcut or architecture change |
| 6 — Implementation | Codex-maintained Vue/Inertia application | Must reconcile design references with existing reviewed components and backend contracts |

Exactly six business modules remain: **Identity, Workforce, Scheduling, Attendance, Leave and Analytics**. Shared notification/security/retention/operations/demo interfaces support these modules. They are not a seventh business module. No payroll, recruitment, performance management, benefits, multi-company tenancy, public registration, impersonation or artificial role switch is added.

The attendance projection clarification in Schema §11.5 was accepted by the owner through G-000/T-0003 on 24 October 2026: completeness includes inconsistent; the arrival scalar and independent server early-departure projection preserve simultaneous late AND early departure. Typed server values use retained evaluation/policy/required-segment versions and NULL for unevaluable comparisons. PRD lifecycle naming takes precedence over technical processing aliases. This reference refresh does not approve a screen or demonstrate implementation.

### 2.1 Verified capability references

Checked against primary documentation on 23 October 2026. These references establish tool capabilities, not installed versions or completed implementation.

| Ref | Primary reference / bounded use |
| --- | --- |
| P01 | [pen.dev .pen files](https://docs.pen.dev/core-concepts/pen-files): local JSON-based documents, saving and Git workflow; edit through the editor/MCP/CLI rather than hand-editing serialized IDs |
| P02 | [pen.dev variables](https://docs.pen.dev/core-concepts/variables) and [format](https://docs.pen.dev/for-developers/the-pen-format): document variables and reusable component/instance representation |
| P03 | [pen.dev AI integration](https://docs.pen.dev/getting-started/ai-integration): an external MCP client connects to a running editor host; docs identify the server as `pencil` |
| P04 | [pen.dev design ↔ code](https://docs.pen.dev/design-and-code/design-to-code) and [import/export](https://docs.pen.dev/core-concepts/import-and-export): agent-assisted design/code workflows and export options; no guaranteed production Vue compiler or automatic bidirectional sync is assumed |
| T01 | [Tailwind theme variables](https://tailwindcss.com/docs/theme): CSS-first theme namespaces and `@theme inline` when aliasing variables |
| T02 | [shadcn-vue theming](https://www.shadcn-vue.com/docs/theming) and [Sonner](https://www.shadcn-vue.com/docs/components/sonner): semantic variables and the Vue feedback primitive; no React package/source is selected |
| T03 | [Apache ECharts accessibility](https://echarts.apache.org/handbook/en/best-practices/aria/): opt-in ARIA descriptions and decal support; explicit module registration is needed with modular imports |
| F01 | [Inter](https://rsms.me/inter/): selected font family; retain its distribution license when self-hosting |
| A01 | [WCAG 2.2](https://www.w3.org/TR/WCAG22/), [text contrast](https://www.w3.org/WAI/WCAG22/Understanding/contrast-minimum.html), [non-text contrast](https://www.w3.org/WAI/WCAG22/Understanding/non-text-contrast.html), [target size](https://www.w3.org/WAI/WCAG22/Understanding/target-size-minimum.html), [unobscured focus](https://www.w3.org/WAI/WCAG22/Understanding/focus-not-obscured-minimum.html) |

The locked frontend remains Vue 3 + strict TypeScript + Inertia, Tailwind CSS 4, shadcn-vue over the selected primitives, TanStack Vue Table, Apache ECharts and VeeValidate+Zod 3. Laravel remains authoritative for permissions, fields, time, schedule, leave economics and metrics. No separate frontend API platform, Pinia server-state mirror or new UI framework is introduced.

## 3. Design Goals & Principles

| Principle | Concrete design rule |
| --- | --- |
| Calm operational clarity | Neutral canvas, one indigo action accent, quiet surfaces and short functional motion |
| Useful density | Readable 14px table/body type,48px minimum table rows, no giant hero area or oversized KPI wall |
| Data before decoration | One clear page header and toolbar; tables/detail sections need not be wrapped in nested cards |
| Consequences before commitment | Preview scope, dates, affected work and exact economic consequences inside high-impact forms |
| Current access, historical meaning | Separate current scope summary from historical context labels; omit unauthorized data from payloads |
| Honest state | Upload, validation, committed event, stage decision, reconciliation freshness and delivery are distinct |
| One component language | Reuse shadcn-vue primitives and the application patterns below; a module never invents another button/table system |
| Accessible by design | Labels, focus, keyboard paths, text alternatives and status wording accompany visual treatment |
| Efficient solo maintenance | One design file, one token source, purpose-specific typed DTOs and incremental component reuse |

## 4. Product Visual Identity

Select a clean enterprise workspace with **indigo accents, slate neutrals and Inter**. Indigo marks primary action, focused/selected navigation and active controls; status feedback retains its own semantic palette. White content areas sit on a faint slate canvas. Use typography, dividers and aligned sections before adding elevation.

The wordmark is plain “Professional HRIS” in the selected sans family,18/24px weight 600; a compact initial mark may use “PH” as text, not a new illustrated logo requirement. No gradient logo, decorative illustration, glass blur, oversized rounded cards, hero inside admin pages, ornamental charts or automatic rainbow departments. Cards use 8px radius, buttons/inputs 6px, dialogs 12px. Borders are 1px; elevation is reserved for overlays and sticky separation. Light mode is the only initial set; do not add a theme toggle or incomplete dark surfaces. Semantic tokens leave room for a reviewed future theme without making it MVP scope.

## 5. Color System & Semantic Tokens

HEX values below are the canonical light palette for both pen.dev and CSS; there is no independent color list in page designs. Values are complete CSS colors, not HSL channels. shadcn variables map through Tailwind 4 `@theme inline` aliases `--color-<name>: var(--<name>)`. Custom semantic variables use the same pattern. A utility such as `bg-primary` resolves the selected token, never a nearby hardcoded hex.

| CSS semantic token | Light value | Use / shadcn mapping |
| --- | --- | --- |
| `--primary` | `#4338CA` | Primary action/focus/active brand; text/bg paired with primary-foreground |
| `--primary-foreground` | `#FFFFFF` | Text/icons on primary |
| `--primary-hover` | `#3730A3` | Primary pointer hover |
| `--primary-active` | `#312E81` | Primary pressed |
| `--secondary` | `#F1F5F9` | Quiet secondary button/surface |
| `--secondary-foreground` | `#0F172A` | Secondary text |
| `--accent` | `#EEF2FF` | Selected control/navigation surface |
| `--accent-foreground` | `#3730A3` | Selected text/icon |
| `--background` | `#F8FAFC` | Application canvas |
| `--foreground` | `#0F172A` | Primary text |
| `--sidebar` | `#F1F5F9` | Navigation surface |
| `--sidebar-foreground` | `#334155` | Navigation text |
| `--header` | `#FFFFFF` | Header surface |
| `--card` | `#FFFFFF` | Bounded content surface |
| `--card-foreground` | `#0F172A` | Card text |
| `--surface-elevated` | `#FFFFFF` | Dialog/sheet surface |
| `--popover` | `#FFFFFF` | Menu/popover surface |
| `--popover-foreground` | `#0F172A` | Popover text |
| `--muted` | `#F1F5F9` | Muted section/code-free inset |
| `--muted-foreground` | `#475569` | Helper/secondary metadata on light surfaces |
| `--surface-hover` | `#F1F5F9` | Neutral hover row |
| `--surface-selected` | `#EEF2FF` | Selected row/cell |
| `--text-secondary` | `#334155` | Secondary content |
| `--text-disabled` | `#64748B` | Inactive control text only; never explanatory body copy |
| `--text-inverse` | `#FFFFFF` | Text on dark/solid semantics |
| `--link` | `#4338CA` | Inline links with underline on content, focus/hover |
| `--border` | `#CBD5E1` | Decorative divider/card border; not sufficient alone to identify a control |
| `--border-subtle` | `#E2E8F0` | Nonessential internal divider |
| `--border-strong` | `#64748B` | Meaningful boundary, selected outline |
| `--input` | `#64748B` | Visible input/checkbox boundary |
| `--ring` | `#4338CA` | 2px focus ring |
| `--ring-offset` | `#FFFFFF` | 2px gap around ring |
| `--overlay` | `rgb(15 23 42 / 0.40)` | Modal scrim; no backdrop blur |
| `--success` | `#166534` | Success text/icon or solid feedback |
| `--success-foreground` | `#FFFFFF` | Text on success solid |
| `--success-subtle` | `#F0FDF4` | Success badge/banner background |
| `--success-border` | `#15803D` | Meaningful success boundary |
| `--warning` | `#92400E` | Warning text/icon or solid |
| `--warning-foreground` | `#FFFFFF` | Text on warning solid |
| `--warning-subtle` | `#FFFBEB` | Pending/attention badge background |
| `--warning-border` | `#B45309` | Warning boundary |
| `--destructive` | `#B91C1C` | Error/destructive text or solid action |
| `--destructive-foreground` | `#FFFFFF` | Text on destructive solid |
| `--destructive-hover` | `#991B1B` | Destructive hover |
| `--destructive-active` | `#7F1D1D` | Destructive pressed |
| `--destructive-subtle` | `#FEF2F2` | Rejected/error banner background |
| `--destructive-border` | `#DC2626` | Error boundary |
| `--info` | `#1D4ED8` | Informational/reconciliation text/icon |
| `--info-foreground` | `#FFFFFF` | Text on info solid |
| `--info-subtle` | `#EFF6FF` | Info badge/banner background |
| `--info-border` | `#2563EB` | Info boundary |
| `--neutral` | `#334155` | Neutral state text/icon |
| `--neutral-foreground` | `#FFFFFF` | Text on neutral solid |
| `--neutral-subtle` | `#F1F5F9` | Draft/cancelled/nonworking background |
| `--neutral-border` | `#64748B` | Meaningful neutral boundary |
| `--sidebar-primary` | `#4338CA` | Sidebar selected indicator |
| `--sidebar-primary-foreground` | `#FFFFFF` | Only when sidebar primary solid is used |
| `--sidebar-accent` | `#EEF2FF` | Selected sidebar row |
| `--sidebar-accent-foreground` | `#3730A3` | Selected sidebar label/icon |
| `--sidebar-border` | `#CBD5E1` | Sidebar separator |
| `--sidebar-ring` | `#4338CA` | Sidebar keyboard focus |
| `--chart-attendance` | `#166534` | Attendance metric |
| `--chart-late` | `#B45309` | Lateness metric |
| `--chart-absence` | `#B91C1C` | Absence metric |
| `--chart-leave-approved` | `#4338CA` | Leave metric category, solid |
| `--chart-leave-pending` | `#B45309` | Pending leave category, diagonal decal/outline; not approved consumption |
| `--chart-1` | `#4338CA` | Stable categorical series 1 |
| `--chart-2` | `#0F766E` | Categorical series 2 |
| `--chart-3` | `#1D4ED8` | Categorical series 3 |
| `--chart-4` | `#B45309` | Categorical series 4 |
| `--chart-5` | `#7C3AED` | Categorical series 5 |
| `--chart-6` | `#475569` | Categorical series 6 |
| `--chart-grid` | `#E2E8F0` | Decorative grid only |
| `--chart-axis` | `#475569` | Axis labels/ticks |

Secondary/ghost controls inherit surface-hover; destructive controls use their dedicated hover/active values. Links underline on hover/focus and retain visible affordance in prose. No color conveys status alone: label + icon/pattern are required. The generic chart 1–6 aliases are for compatible categorical comparisons, not substitutions for attendance/absence semantics. Colors of different adjacent series need labels/patterns, not an assumption that hue alone distinguishes them. An “Approved” badge is always success; the indigo leave metric category is a data series, not a contradictory approval-status badge.

### 5.1 Verified light-palette contrast pairs

Ratios below were calculated from the specified sRGB values using relative luminance; values are displayed to2 decimals. Acceptance uses the unrounded ratio. These token-pair checks do not certify complete screens, opacity overlays, charts or all states.

| Foreground / background | Ratio | Intended treatment |
| --- | --- | --- |
| primary-foreground / primary | 7.90:1 | Normal-size primary-button text |
| primary / surface-selected | 7.07:1 | Selected/navigation text and ring |
| foreground / background | 17.06:1 | Main body |
| text-secondary / card | 10.35:1 | Secondary body |
| muted-foreground / muted | 6.92:1 | Helper/meta text |
| input / card | 4.76:1 | Meaningful control boundary; minimum 3:1 target |
| success / success-subtle | 6.81:1 | Badge/banner text |
| warning / warning-subtle | 6.84:1 | Badge/banner text |
| destructive / destructive-subtle | 5.91:1 | Error text |
| info / info-subtle | 6.16:1 | Information text |
| neutral / neutral-subtle | 9.45:1 | Neutral status |

text-disabled on muted is4.34:1 and is limited to genuinely inactive controls; explain why an action is unavailable in normal muted-foreground text nearby. Never lower an entire form/permission error to reduced opacity. Default/subtle borders are decorative: use input/border-strong for a boundary needed to recognize an active control. Recheck all composed pairs and focus outlines during UI QA.

## 6. Typography

Use self-hosted Inter variable WOFF2 with weights 400/500/600; `font-display: swap`. Stack: `Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif`. Keep the font license with the later asset. No external font request is needed in the app/demo. No additional heading/display family. Browser text remains selectable; do not rasterize labels.

| Token / use | Size / line height | Weight / tracking | Adaptation |
| --- | --- | --- | --- |
| type-page | 24/32px | 600 / −0.02em | Phone 22/28px; one h1 per view |
| type-section | 18/26px | 600 / −0.01em | Sections, dialog title; no all-caps headings |
| type-subsection | 16/24px | 600 / 0 | Named groups |
| type-body | 14/20px | 400 / 0 | Default administration text and table cells |
| type-label | 14/20px | 500 / 0 | Field/control labels; table headers 500 |
| type-control | 14/20px | 500 / 0 | Button/select text; mobile editable inputs 16/24px |
| type-helper | 13/18px | 400 / 0 | Help/error text; errors not smaller than help |
| type-caption | 12/16px | 400 / 0 | Nonessential metadata only; never sole status/recovery guidance |
| type-kpi | 28/34px | 600 / −0.02em | Phone 24/32px; comparable unit/basis directly below |
| type-number | Inherit size/line | Inherit weight; tabular-nums | Counts, times, rates, day units, table amounts |

Use rem equivalents at the browser's16px root; do not lock the root to suppress zoom. Body paragraphs max 72ch, helper text max 64ch where feasible. Long names wrap; table primary identifiers may use two lines with full text available through detail. Tooltip is not the only way to read an essential name.

Initial content formatting uses English (en-GB style): “23 Oct 2026”,24-hour “09:05”, comma-separated counts and periods for decimal fractions. Dates in native/data controls retain unambiguous ISO values behind localized labels. Show timezone as “Asia/Jakarta (UTC+07:00)” on scheduling/capture/report context; include both dates across midnight and daylight-saving offsets when relevant. Do not silently reinterpret location time as the browser's timezone. Leave balances show 2 decimals (“1.50 days”); authorized exact ledger inspection shows 4 (“1.5000 days”). The browser formats server values and never recomputes entitlement/percentages. N/A stays text, not 0%. Percent display precision comes from the shared server formatting contract; use 1 decimal for report rates unless a supplied precision is more explicit, and never round before classification.

## 7. Spacing, Sizing, Radius & Elevation

Base rhythm 4px. Scale tokens `space-0=0`, `space-1=4`, `space-2=8`, `space-3=12`, `space-4=16`, `space-5=20`, `space-6=24`, `space-8=32`, `space-10=40`, `space-12=48`, `space-16=64` (px). A2px `space-half` is only for optical icon/text adjustment. Preserve Tailwind's4px spacing basis; do not introduce a second spacing multiplier.

| Token / element | Desktop / laptop | Phone / coarse pointer |
| --- | --- | --- |
| page-gutter | 24px; 32px at≥1536 | 16px |
| section-gap | 24px | 24px |
| card-padding | 20px | 16px |
| field-gap | 20px vertical; 24px groups/columns | 20px; stacked |
| toolbar-gap | 8px controls /16px groups | 8px; wrap with one filter entry |
| sidebar-expanded / rail | 240px /72px | Modal drawer min(280px, viewport−48px) |
| header-height | 64px | 56px; demo band adds natural height |
| button-sm / default / large | 32 /36 /44px | Active touch target≥44px; attendance primary 52px |
| icon-button | 32px box; 16/18px glyph | 44px box; 20px glyph |
| input/select | 40px min height; 12px horizontal padding | 44px; 16px editable text |
| checkbox/radio visual | 18px; full associated label row clickable | 20px visual; 44px label hit area |
| table row | 48px minimum; 12px horizontal/vertical within available row | 56px minimum; content can wrap taller |
| table header | 44px minimum; 14px label | Same; scroll region retained |
| badge | 24px min; 6px horizontal; 13/18px text | Same, allow wrap outside narrow table cell |
| small / standard / wide dialog | 440 /640 /880px maximum | calc(100vw−32px); long forms become full-height sheet |
| popover | min 240px, max 360px by content | ≤calc(100vw−32px) |
| detail aside | 320px | Stacks after main summary |
| form max-width | 880px including context; simple auth form 400px | Full available width |
| main content max | 1600px, centered within main area | Full width minus gutters |

One table density is selected; no density preference/toggle is required. Rich records may increase row height; do not squeeze labels to force 48px. Read-only metadata uses typography hierarchy rather than a second dense table engine.

| Token | Value / use |
| --- | --- |
| radius-xs / sm / md / lg / xl / full | 2px /4px /6px /8px /12px /9999px; md controls, lg cards, xl overlays, full avatar/status dot only |
| --radius | 8px shadcn base; explicitly map radius-sm4, radius-md6, radius-lg8, radius-xl12 once in theme customization |
| elevation-0 | none; ordinary cards/tables |
| elevation-1 | `0 1px 2px rgb(15 23 42 / .06)`; sticky separation only when needed |
| elevation-2 | `0 4px 16px rgb(15 23 42 / .10)`; popover/menu |
| elevation-3 | `0 12px 32px rgb(15 23 42 / .16)`; dialog/sheet |
| z-base / sticky / dropdown / overlay / modal / modal-popover / toast | 0 /20 /40 /50 /60 /70 /80 |

Popover/datepicker launched inside a modal remains in its focus/portal layer above that modal, not trapped behind the scrim. Avoid nested dialogs: replace content/close the preceding overlay. Sticky bars must reserve real layout space and safe-area padding so focus/errors are not obscured.

## 8. Icons, Motion & Feedback

Use Lucide line icons consistently,2px stroke,16px inline/table,18px navigation,20px standalone. Use the corresponding Vue icon imports, not React. Decorative icons are aria-hidden; icon-only controls have explicit names (“Open actions for Maya Putri”). Status icons accompany readable text. Do not use emoji as operational status or switch icon family between modules.

| Interaction | Timing / behavior |
| --- | --- |
| hover/pressed/focus | 120ms color/border transition; focus ring appears immediately and stays visible |
| menu/popover | 120ms opacity +≤4px translation; no spring/bounce |
| dialog/sheet | 160ms fade/≤8px travel; restore focus on close |
| sidebar rail | 160ms width; labels do not remain interactive offscreen |
| server submission | Immediate text+spinner in stable-width button; no success animation before commit |
| skeleton | Static layout-matched placeholders; optional subtle opacity pulse disabled for reduced motion |
| data refresh | Keep prior coherent authorized result visible with “Updating”; announce completion once |
| toast | Confirmation after commit; 5s ordinary success plus persistent page result; failures requiring action remain inline |
| reduced motion | Remove translation, pulsing, animated chart entrances and sidebar width transition; retain instant state/labels |

Use at most one page-level status announcement per transition. A spinner does not prove upload completion, commit or approval. Toasts are supplementary: leave stage result, clock timestamp and export status remain visible in the owning panel. Do not auto-dismiss a destructive failure, stale conflict, lost-response recovery or permission-change notice.

## 9. Responsive Layout System

Tailwind-compatible breakpoints: base<640, sm640, md768, lg1024, xl1280, 2xl1536 CSSpx. Breakpoints are layout thresholds, not device detection. Support 320px viewport/reflow and 200% zoom without page-wide horizontal overflow; tabular/calendar regions may have named horizontal scrolling while surrounding content reflows.

| Range | Navigation/content | Tables/forms/charts |
| --- | --- | --- |
| base/sm phone |56px header, drawer; 16px gutter; titles/actions stack | One column; 44px targets, mobile inputs 16px; table region scrolls with primary identity/status/action retained; calendar defaults agenda |
| md tablet | Drawer until lg; 24px gutter; two KPI columns | Filter Sheet, two form fields only when actual container≥640px; no narrow detail aside |
| lg laptop |240px sidebar with optional 72px rail; 64px header; 24px gutter | Full table toolbar wraps; forms center; 2 KPI columns if content constrained |
| xl standard desktop | Sidebar, main max 1600; detail main+320px aside when actual content≥960px | Up to4 meaningful KPI columns; calendar week; forms may group two columns |
| 2xl large desktop |32px gutters; centered max-width | Wider tables, never stretched prose; no extra decorative cards just to fill space |

Layout patterns: **L1 Authentication** centered 400px form with concise product/demo info; **L2 Overview** page header+scope/date context+KPI strip+purposeful chart/detail sections; **L3 Data management** header+toolbar+table+pagination; **L4 Detail** identity/summary+tabs or sections+context aside; **L5 Form** header+grouped fields+impact summary+action footer; **L6 Attendance focus** one occurrence/policy context+capture/review+commit/recovery panel; **L7 Shared information/operations** contextual notice+read-only evidence or controlled configuration panels. These reuse one application and shared components.

On phones, detail aside follows the essential summary before long history only if it contains the next required action; otherwise follows main content. Form action footer may be sticky with bottom safe-area and reserved padding. Reconciliation/approval actions never float over evidence or error text. Attendance capture supports portrait/landscape, permission/error recovery and system camera constraints without locking orientation. No feature is declared desktop-only; complex admin matrices scroll in a labeled region and use focused per-row/permission editing rather than a shrunken desktop canvas.

## 10. Application Shell & Navigation

The sidebar starts with the text wordmark, grouped activity links and bottom account/context area. Expanded rows 40px (44px touch), icon 18px, label 14/20px; selected row uses sidebar-accent + sidebar-accent-foreground +2px primary indicator and `aria-current=page`. Collapse is a real button with accessible expanded state; rail icons retain names/tooltips available on focus and hover. Persist only the harmless expanded/rail preference, never roles/permissions or private drafts, in local UI preference storage.

The header contains menu toggle, compact breadcrumb/current context, notification entry and user menu. There is no unapproved global search/command palette. Page-level search stays in the relevant toolbar. Breadcrumbs are links to authorized ancestors; phone shows Back + current title rather than truncating an unreadable chain. One h1, optional one-sentence description, scope/timezone/freshness context and one primary action form PageHeader; extra secondary actions move to a labeled menu after the two most useful visible controls.

User menu: “My account”, authenticated password change, and “Sign out”; no persona/role switch, user impersonation or dark-mode/language control is added. Notification entry exposes only own unread count/items. Bell remains 44px touch and has a text name including authorized unread count; no global HR pending count is reused. Mobile sidebar is a shadcn Sheet with focus trap, close button, Escape, scrollable links and focus restoration; route change closes it. Groups with no allowed destination are absent. Parent grouping is not a selectable dead link.

Page body scroll is the default. Table header may stick inside its own scroll region; keep focus visible beneath header/footer via scroll padding. No marketing footer inside admin workflows; small auth/demo disclosure appears in L1/L7 only. At an unknown/denied direct target, use the server's consistent 403/404 outcome and generic guidance without revealing whether a hidden employee exists.

## 11. Role-Aware Information Architecture

Navigation is generated from server-provided current capabilities and allowed destinations. Persona labels below describe default grants, never route guards. Multi-role users get the union of destinations, while each view/action/data field independently uses the exact permission set and scope intersection. No implicit Employee inheritance, broad-role scope shortcut or artificial role switching. With no employee link, self/team employee sets are empty; show an account context explanation, not fake balances/clock buttons.

Notation: `∧` = all listed permissions required for that content; `∨` = an allowed branch. A/T/S/G/O retain PRD §8 semantics. Read-only source panels use their read permission; actions use the specific action capability and business guards. A create-only actor is not silently required to gain broad list access: standalone forms render authorized input/reference DTOs, return safe committed results, and link to detail only if current read access exists. Optional photo/history/reason/balance panels are omitted from payload and layout if their extra read grant is missing. The matrix does not grant permissions or add a new backend policy.

| Destination | Purpose | Exact permission boundary | Default personas (descriptive) | Scope / restriction | Screen IDs | PRD trace |
| --- | --- | --- | --- | --- | --- | --- |
| Workspace / Overview | Role/scope relevant metrics | dashboard.view ∧ each shown source-read permission | SA, HR, M, E; custom as granted | Intersect metric source sets; no global workforce counts with self-only access | UI-DASH-001 | AUTH-003–AUTH-006; DASH-001–DASH-005 |
| Workspace / My schedule | Own resolved work | shift.view for own employee target | All linked personas as granted | Self target must be in exact permission set; all can include self; no assumed Employee role | UI-SHIFT-001 | SHIFT-002, SHIFT-005; AUTH-003 |
| Workspace / Clock attendance | Own valid in/out | attendance.clock (S) | Linked eligible personas | Current eligibility/occurrence/time/leave; not permission alone | UI-ATT-001 | ATT-001–ATT-005, ATT-013 |
| Workspace / My attendance | Own history/correction | attendance.view for own target; correction action attendance.correction.request | Linked personas | Photo separately attendance.photo.view; corrections independently reviewed | UI-ATT-002, UI-ATT-003, UI-ATT-004 | ATT-007–ATT-010, ATT-012 |
| Workspace / My leave | Own requests/balances | leave.view for request panel ∨ leave.balance.view for balance panel ∨ leave.request for standalone create | Linked personas as granted | Each panel has its own permission/target; balances are not implied by request grant | UI-LEAVE-001, UI-LEAVE-002 | LEAVE-002, LEAVE-004–LEAVE-006, LEAVE-015 |
| People / Employees | Authorized directory/detail | employee.view; create employee.create; changes employee.update/employment.manage | SA, HR, M; self link for E | All/team/self exact set; sensitive/history/export separately scoped | UI-EMP-001–UI-EMP-006 | EMP-001–EMP-008; AUTH-004 |
| People / Organization | Reference structure/reporting context | organization.view; employee.view for employee reporting nodes; manage organization.manage | All reference readers; SA/HR edit defaults | G reference read never returns private employee lists; reporting nodes scoped | UI-EMP-007 | ORG-001–ORG-004; AUTH-004 |
| Work / Schedules | Team/company planning | shift.view; publish shift.assign; override shift.override | SA, HR; M read | Same calendar component as My schedule; authorized target scopes and impact action | UI-SHIFT-001, UI-SHIFT-003, UI-SHIFT-005 | SHIFT-002–SHIFT-009 |
| Work / Shift templates | Reusable shift definitions | shift.template.manage (G) | SA, HR | Configuration projection only, not employee schedule access | UI-SHIFT-002 | SHIFT-001; OD-05 |
| Work / Holidays & overrides | Calendar/individual exceptions | calendar.view; calendar.manage for shared calendar writes; shift.override for individual target | All shared-calendar readers; SA/HR edit defaults | Reference holiday does not disclose employee list; individual preview adds shift.view/target access | UI-SHIFT-004, UI-SHIFT-005 | SHIFT-003, SHIFT-005–SHIFT-007 |
| Attendance / Records | Permitted attendance review | attendance.view; attendance.export additionally for CSV | SA, HR, M; own view E | Source scope before count; photos/reason fields separately granted | UI-ATT-002, UI-ATT-003 | ATT-006–ATT-012; DASH-007 |
| Attendance / Corrections | Independent correction decisions | attendance.view ∧ attendance.correction.review; attendance.resolve for HR initiation/resolution | SA, HR defaults | Current target intersection; exclude submitter/affected employee; stale version recheck | UI-ATT-004, UI-ATT-005 | ATT-008–ATT-011; AUTH-008 |
| Attendance / Reconciliation & exceptions | Missing/conflicted work review | attendance.view plus attendance.reconcile or attendance.resolve for respective action | SA, HR | Owned HR exceptions stay visible as permitted; no fake close/grace | UI-ATT-006 | ATT-007, ATT-010–ATT-011; EMP-006 |
| Attendance / Policy | Per-field location/shift policy | attendance.policy.manage (G) | SA, HR | Only authorized config references; not unrestricted employee records | UI-ATT-007 | ATT-004–ATT-005; OD-07–OD-09 |
| Leave / Approvals | Assigned stage work | leave.view ∧ leave.approve; current assignment; leave.reassign for HR intervention | SA, HR, M | Read/approve set intersection and independence; unassigned broad-scope actor cannot decide | UI-LEAVE-003, UI-LEAVE-004 | LEAVE-007–LEAVE-009, LEAVE-011; AUTH-008 |
| Leave / Cancellations | Independent approved-leave cancellation review | leave.view ∧ leave.cancel.review | SA, HR | Original approved leave remains effective while pending | UI-LEAVE-005 | LEAVE-010, LEAVE-013; OD-13 |
| Leave / Balances & ledger | Permitted entitlement evidence | leave.balance.view; leave.balance.adjust for adjustment | SA/HR all; M/E self defaults | No editable ledger rows; sensitive reason needs its own permission | UI-LEAVE-006 | LEAVE-002–LEAVE-003, LEAVE-014–LEAVE-015 |
| Leave / Policies & types | Eligibility and effective leave rules | leave.policy.manage (G) | SA, HR | Config references only; employee override selection remains authorized | UI-LEAVE-007 | LEAVE-001–LEAVE-003; OD-11–OD-12 |
| Leave / Workflows | Ordered selectors/fallbacks/exception config | leave.workflow.manage (G) | SA default | HR intervention does not confer workflow configuration; no skip/self-approval settings | UI-LEAVE-008 | LEAVE-006–LEAVE-007, LEAVE-011; OD-14 |
| Leave / Delegation | Dated existing-authority delegate | leave.delegation.manage for allowed target; task/source reads where shown | SA, HR, M defaults | No new permissions, no chains/self, current approval scope and prior actors checked | UI-LEAVE-009 | LEAVE-011; AUTH-008 |
| Leave / Entitlement processing | Due grant/accrual/carry/expiry preview/run | leave.balance.process (G) | SA, HR | Safe command status; employee rows/details shown only with their own read rights | UI-LEAVE-010 | LEAVE-003; NTF-006 |
| Reports / Explore | Scoped details/chart drill | dashboard.view plus relevant employee/attendance/leave/balance reads for selected report | SA, HR, M; permitted self reports E | Source intersection and shared token/cutoff/metric basis; permitted report tabs only | UI-DASH-002 | DASH-002–DASH-006, DASH-009 |
| Reports / Exports | Owned queued CSV results | Current relevant source read ∧ employee.export / attendance.export / leave.export / analytics.export branch | SA, HR; M team for supported branches | Only own jobs; fresh authority/fieldmask/generation for all bytes; no generic export.view key | UI-DASH-003 | DASH-007–DASH-008; DATA-010 |
| Account / Notifications | Own center/read state | notification.view (O); notification.update (O) for mark read | All authenticated owners | No role/delegate can widen notification ownership | UI-SHARED-001 | NTF-002, NTF-005 |
| Administration / Accounts | Authorized account lifecycle | account.view; account.invite/deactivate/reactivate for respective target action | SA, HR; own menu for M/E | All/team/self account set; role.assign separately; employee panel adds employee.view | UI-AUTH-005, UI-AUTH-006 | AUTH-001, AUTH-005, AUTH-007 |
| Administration / Roles & access | Private role/grant administration | role.view; role.manage edit; role.assign assignment | SA; custom explicit grants | No implicit superuser bypass; final-admin/self-escalation guards; public demo denied | UI-AUTH-007 | AUTH-002–AUTH-006, AUTH-008 |
| Administration / Audit | Read-only material evidence | audit.view (G) | SA; custom explicit grant | No normal audit edit/delete; redact secret/sensitive payloads | UI-SHARED-002 | SEC-005–SEC-006, SEC-012 |
| Administration / Retention | Category validation/holds/controlled cleanup | retention.manage (G) + sensitive-action reauthentication | SA; custom explicit grant | Candidate ages≠approval; current policy version/holds/execution checks remain | UI-SHARED-003 | SEC-007–SEC-009; OD-16, OD-21 |
| Administration / Operations | Safe health/failure/recovery evidence | operations.view (G); operations.retry to retry; domain cap when command requires; demo.reset only authorized operator path | SA/private operator | No public reset/backup/operator mutation; never approve business stages from job UI | UI-SHARED-004 | NTF-006; NFR-012, NFR-015–NFR-016 |
| Demo / About this demo | Synthetic guide/read-only permission comparison | Explicit isolated public-demo projection; not role.view/manage/assign | Public synthetic personas/visitors | No visitor camera/GPS, secret token/admin grant or active role switch | UI-SHARED-005 | SEC-011; OD-20; AC-40, AC-47–AC-48 |

SA=Super Admin, HR=HR Admin, M=Manager, E=Employee. Public demo guard removes/denies privileged administration even when a default private persona has broad grants. Explanatory role comparison is a separate sanitized destination. For self-only users, suppress duplicate “Work / Schedules” and “Attendance / Records” aliases; My schedule/history uses the same UI screen ID with an explicit own-target context. Multi-role planners may retain both personal and operational shortcuts with distinct labels and one shared component.

Landing selection is server-driven: permitted Overview, otherwise the first relevant authorized workspace destination, otherwise My account with an account-link explanation. Do not require dashboard.view for login, clock or a standalone permitted form. Scope chips say “My records”, “Current team” or “All permitted records”; they are descriptions/filter choices within current grants, never a client scope switch that broadens access. Reference comboboxes receive only server-authorized label/value DTOs; a G capability does not fetch raw employee/account tables.

## 12. Component Foundations

Prefer reviewed shadcn-vue/Reka primitives under `components/ui`. The following are one chosen convention per control, not alternative visual kits. Shared active controls support default, hover, focus-visible, pressed/selected, disabled and applicable invalid/processing states. Read-only is readable/copyable and distinct from disabled. Use native semantics before adding ARIA; retain the primitive's keyboard/focus behavior when styling it. Do not reimplement datepicker/dialog/select focus machinery with div/click handlers.

| Foundation | Purpose | Variants / size | States | Behavior / anti-pattern | Accessibility | Implementation |
| --- | --- | --- | --- | --- | --- | --- |
| Button | Commit or navigate by explicit verb | primary, secondary-outline, ghost, destructive, link; 32/36/44px | processing replaces label with verb+ellipsis, disabled retains explanation; invalid belongs to form | One primary per action region; button for command, link for route; no pointer-only or optimistic success | Name action/target; native disabled prevents repeat; 44px touch | shadcn Button |
| IconButton | Secondary compact action | ghost/outline; 32px desktop,44px touch | hover/focus/pressed/disabled; loading if actual command | Use labeled actions menu/edit/close, not an unlabeled primary action; not hover-only | Accessible name; tooltip on focus and hover; icon aria-hidden | Button size=icon + Tooltip |
| Input | Short scalar value | default/read-only/invalid; 40/44px | empty, filled, focus, disabled, read-only, invalid, processing form | Persistent label; examples in help, not placeholder-only label; mobile 16px text | label-for/id, describedby helper/error, aria-invalid | Input + Field/Label |
| Textarea | Reason or bounded notes | same input border; min 112px, vertical resize | input states, character guidance when a server limit exists | Human reason, no rich-text editor or made-up limit; do not expose hidden sensitive value | Label and error; multiline keyboard natural | Textarea + Field |
| Select | Small fixed option set | single select; 40/44px trigger | placeholder, selected, open, focus, unavailable, invalid | Only approved option values; no invented status or empty-string=real-category | Arrow/Enter/Escape behavior; option labels; no trap | Select |
| Combobox | Search authorized reference options | single searchable; 40/44px; bounded popover | idle, searching, empty, error, selected, invalid | Server-filtered authorized options; distinguish no matches from denied; no entire private roster download | Combobox/listbox naming, active option and keyboard; selected text persists | Combobox/Command + Popover |
| Checkbox | Independent option or explicit page selection | 18px control+clickable label; 20px touch visual | checked, unchecked, indeterminate, invalid, disabled | No prechecked consent for destructive action; selection affects only specified loaded rows | Label hit area, mixed state, Space toggle | Checkbox |
| RadioGroup | One mutually exclusive choice | stacked labels;≥32px row/44px touch | selected, unselected, focus, disabled, invalid | Full/first-half/second-half or policy mode; never mutually exclusive switches | Group legend/description, arrow navigation | RadioGroup |
| Switch | Binary reversible setting | compact with full label and help | on, off, focus, disabled, saving/error if immediate | Only genuine binary config. Effective policy form uses explicit Save, not hidden immediate economic writes | Accessible checked state; text does not change meaning ambiguously | Switch |
| DatePicker | Local business date/range | 40/44px trigger; desktop popover/mobile sheet | unset, selected, invalid, disabled date, open | Typeable unambiguous date+calendar; server timezone stated. Disabled dates reflect known capability/policy, server revalidates | Keyboard calendar, month/year nav, labeled range endpoints | Popover/Sheet + Calendar + Input |
| Calendar | Date selection foundation | single/range with clear start/end | today, selected, range, unavailable, focus | Not an employee shift engine; operational schedule uses ShiftCalendar pattern | Day full-date labels; current/selected distinct; no color-only meaning | Calendar/RangeCalendar |
| Dialog | Short focused form/detail | 440/640/880px max; 12px radius | opening, open, submitting, validation error, stale | Title/description, close, content, action row. Long complex forms use page/sheet; no nested dialogs | Trap focus, visible close, Escape unless commit critical, restore trigger; initial focus on title/first safe field | Dialog |
| AlertDialog | Review high-impact consequences | standard 640px max; cancel then explicit destructive action | idle, reauthentication needed, submitting, rejected/error | Use for final meaningful commitment after preview, not an extra business approval stage; default focus Cancel | Title/description associated; Escape Cancel; no vague OK | AlertDialog |
| Sheet | Mobile nav/filter/short detail | right side max 440px; nav left 280px; phone full-height | open, editing, applying, error, closing | Mobile filters use Apply/Clear. Do not layer another sheet over it; close/replace | Dialog focus behavior, close label, scroll/footer safe-area | Sheet; no separate drawer library |
| Popover | Light contextual option panel | 240–360px width; match anchor when useful | open, closed, focus, loading/empty options | Noncritical short context only; no destructive complex form in hover panel | Keyboard trigger, Escape, collision handling; correct modal portal | Popover |
| Tooltip | Supplementary short hint | max 240px, helper type; short delay 300ms | focus/hover visible, Escape dismiss | Never sole label, essential consequence, validation error or permission explanation | Available on keyboard; does not steal focus | Tooltip |
| Tabs | Related views of same authorized resource | underline tabs,40px/44px min target | selected, focus, disabled only when meaningful | URL query state for resource tabs; omit unauthorized tabs; no nested tab labyrinth | Tablist/tabpanel association, arrows; links for route navigation | Tabs |
| DropdownMenu | Less common row/page actions | text+optional 16px icon; min 40/44px items | open, focus, disabled known business guard | Keep key row action/detail link visible; destructive items separated, not default. Never hide primary only on hover | Keyboard items/roles, typeahead, Escape/focus restore | DropdownMenu |
| Badge | Short state label | subtle+icon+text; 24px min; semantic 5 families | Legitimate business/freshness/processing states only | Do not make every metadata label a badge; clickable filter uses button semantics | Color plus text/icon; no status conveyed by dot alone | Badge styled through StatusBadge |
| Avatar | Identity orientation | 32px table,40px summary,64px detail | initials fallback, loading image, unavailable | Initials are sufficient; no new employee photo-upload feature. Camera evidence is never profile avatar | Adjacent name prevents duplicate announcements; decorative avatar alt empty | Avatar |
| Separator | Group related content | 1px decorative border; 16/24px surrounding gap | static | Use semantic headings/fieldset, not divider-only structure | Decorative separator hidden or correct orientation when meaningful | Separator |
| Skeleton | Preserve expected layout while first loading | neutral blocks matching rows/heading/cards | initial load only; no success/failed imitation | Show 5 visible table rows or matching first viewport, not count of hidden data; no endless animated blank screen | aria-busy on region, one loading label; skeletons hidden from SR | Skeleton |
| Toast | Supplementary committed confirmation | one Sonner family, lower-right desktop/above safe footer phone | success/info; actionable failures remain inline | 5s ordinary success; no critical content only in toast; no fake saved/approved before commit | Polite announcement, pause on interaction; not stacked duplicate alerts | shadcn-vue Sonner |
| InlineAlert | Persistent contextual outcome/recovery | info/success/warning/destructive/neutral | loading recovery, stale, blocked, denied, failure | Title+one clear reason+next permitted action; no raw exception, token, GPS or SQL text | status for informational change, alert for new blocking error; no repeated announcement each poll | shadcn Alert |
| Pagination | Server-paged traversal | Previous/Next, current page,25/50/100 size | loading, first/last, empty, server error | Authorized total only; no invented all-pages bulk selection; page reset on filters | nav label/current page; controls keyboard/touch | Pagination + Select |
| Breadcrumb | Authorized location context | 14px desktop; phone back+current | current, shortened known parents | Never render links to forbidden ancestors or raw IDs as names | nav landmark/label, ordered links, current page | Breadcrumb |

## 13. Application Component Patterns

These 30 patterns compose foundations; they are not another primitive library. Names are recommended PascalCase Vue component names. Shared components belong in components/forms/tables/charts or layouts; domain-only patterns stay in features/module. Each important pattern below fixes visual structure, state, behavior, accessibility, responsive treatment and source ownership. Do not implement a general workflow renderer from this inventory.

### 13.1 PageHeader

| Attribute | Contract |
| --- | --- |
| Component | PageHeader |
| Purpose | Orient the task and next action |
| Visual structure | Breadcrumb, h1, optional short description; ScopeSummary/context row; primary then secondary actions |
| Variants | list/detail/form; no alternative hero style |
| States | loading title shell; permission-limited actions; current context |
| Behavior | One clear primary action; preserve current authorized filters on return |
| Accessibility | One h1, meaningful action names; title focus after navigation |
| Responsive | Stack title/actions on phone; wrap description, no clipped name |
| Implementation mapping | Layout composition + Button/Breadcrumb |
| Source reference | AUTH-004; NFR-009; Architecture 11 |

### 13.2 KpiCard

| Attribute | Contract |
| --- | --- |
| Component | KpiCard |
| Purpose | Explain one decision-relevant metric |
| Visual structure | Label, server value/unit, basis/cutoff, freshness and optional authorized detail link |
| Variants | number/rate/N/A; one bordered 8px surface |
| States | loading, complete, provisional, stale, N/A, error |
| Behavior | Do not calculate/animate counts; no comparison without server compatible period basis |
| Accessibility | Readable label/value/basis; link separately keyboard reachable |
| Responsive | 2 columns tablet,1–2phone by legibility; up to4 desktop |
| Implementation mapping | Card + ReportBasisPanel |
| Source reference | DASH-003–DASH-006; AC-31–AC-32 |

### 13.3 StatComparison

| Attribute | Contract |
| --- | --- |
| Component | StatComparison |
| Purpose | Explain a supported change on comparable bases |
| Visual structure | Current value, explicit comparison interval, signed delta with unit |
| Variants | server-supplied delta or unavailable; inline, not another card |
| States | available, incompatible/N/A, loading |
| Behavior | Only show if server provides comparable authorized series; up arrow is not automatically positive |
| Accessibility | Text says increased/decreased and basis, no red/green-only judgment |
| Responsive | Wrap below KPI, no tiny sparkline requirement |
| Implementation mapping | Text + icon, optional within KpiCard |
| Source reference | DASH-004–DASH-005; no new comparison metric required |

### 13.4 FilterBar

| Attribute | Contract |
| --- | --- |
| Component | FilterBar |
| Purpose | Narrow authorized results predictably |
| Visual structure | Scoped search, date, department/location/status controls; active FilterChips; Clear |
| Variants | table/report/calendar; same control family |
| States | initial, editing draft, applying, applied, invalid, error |
| Behavior | Explicit Apply for multi-control filters; Enter/search debounced 300ms; commit URL and reset page 1 |
| Accessibility | Named filter region, labels, apply announces authorized update |
| Responsive | Phone Filters button→Sheet; applied chips remain visible |
| Implementation mapping | Input/Select/Combobox/DatePicker/Sheet |
| Source reference | DASH-002, DASH-009; EMP-008; AUTH-004 |

### 13.5 AdvancedDataTable

| Attribute | Contract |
| --- | --- |
| Component | AdvancedDataTable |
| Purpose | Read authorized records with bounded server work |
| Visual structure | TableToolbar, semantic header/rows, named scroll region, empty/error layer, pagination |
| Variants | domain columns; same 48px row treatment |
| States | loading, ready, no records, filtered zero, error, stale access |
| Behavior | TanStack manual sorting/filtering/paging; only server field descriptors; detail link in primary column |
| Accessibility | caption, th scope, aria-sort, named row actions; no whole-row click trap |
| Responsive | Region scroll; identity/status/action remain; secondary columns only removed with equivalent detail |
| Implementation mapping | TanStack Vue Table + shadcn Table |
| Source reference | DASH-009; NFR-007; AUTH-004 |

### 13.6 TableToolbar

| Attribute | Contract |
| --- | --- |
| Component | TableToolbar |
| Purpose | Group actions without competing headers |
| Visual structure | Search left; filter/sort context; authorized export/action right; selected count only if valid bulk workflow |
| Variants | with/without supported selection |
| States | searching/applying; exporting queued; selection page-scoped |
| Behavior | No export of browser-loaded rows; all CSV queued; no generic bulk delete |
| Accessibility | Controls labeled, selection count announced once |
| Responsive | Stack search then action row; export stays explicit button/menu |
| Implementation mapping | FilterBar/SearchInput/Button |
| Source reference | DASH-007–DASH-009; DATA-010 |

### 13.7 SearchInput

| Attribute | Contract |
| --- | --- |
| Component | SearchInput |
| Purpose | Search the current permitted entity set |
| Visual structure | Search icon, persistent or visually hidden label, query, clear control |
| Variants | directory/reference/table; 40/44px |
| States | idle, querying, clear, no matches, error |
| Behavior | 300ms debounce or Enter; cancel superseded requests; no global unapproved search |
| Accessibility | Name what is searched, clear label; results count polite after response |
| Responsive | Full width phone; no icon-only collapsed input |
| Implementation mapping | Input + IconButton |
| Source reference | EMP-008; DASH-009; AUTH-004 |

### 13.8 FilterChips

| Attribute | Contract |
| --- | --- |
| Component | FilterChips |
| Purpose | Make applied query visible |
| Visual structure | Label:value, remove button, Clear filters |
| Variants | Applied only, separate from draft controls |
| States | active, removing/applying, restricted option removed |
| Behavior | Remove narrows/resets appropriately, never broadens beyond current permission; retain timezone/cutoff context |
| Accessibility | Button says Remove department filter, not x |
| Responsive | Wrap; never horizontal page overflow |
| Implementation mapping | Button/badge-style composition |
| Source reference | DASH-002; AUTH-004–AUTH-006 |

### 13.9 StatusBadge

| Attribute | Contract |
| --- | --- |
| Component | StatusBadge |
| Purpose | Consistent entity-appropriate state |
| Visual structure | Small icon + canonical English state label |
| Variants | success/warning/destructive/info/neutral; §18 mappings |
| States | Only legitimate states; separate dimensions |
| Behavior | Requires explicit entity and dimension; no one giant status precedence hiding missing-out |
| Accessibility | Readable full label, icon decorative; tooltip supplementary |
| Responsive | Allow wrap or labeled detail, no unexplained letter abbreviations |
| Implementation mapping | Badge + Lucide |
| Source reference | PRD 14; ATT-007, ATT-010; DASH-005 |

### 13.10 DetailSummary

| Attribute | Contract |
| --- | --- |
| Component | DetailSummary |
| Purpose | Summarize one authorized resource |
| Visual structure | Title/identity; key-value rows; dates/timezone; current status/action; sensitive groups omitted |
| Variants | employee/request/occurrence/export |
| States | loading, ready, missing optional, restricted, error |
| Behavior | Only explicit DTO fields; missing≠hidden≠zero. Do not render hidden property names/values in DOM |
| Accessibility | Description list, meaningful headings; copyable permitted reference only |
| Responsive | 2 columns at wide containers, stacked phone |
| Implementation mapping | Semantic dl + StatusBadge |
| Source reference | AUTH-004; EMP-008; LEAVE-015 |

### 13.11 ActivityTimeline

| Attribute | Contract |
| --- | --- |
| Component | ActivityTimeline |
| Purpose | Read dated effective and recorded events |
| Visual structure | Ordered event label, actor if permitted, effective date/time, recorded-at when different, reason if granted |
| Variants | employment/approval/evaluation history |
| States | loading, empty, recorded correction, superseded assertion |
| Behavior | Display original and later revision; no frontend reconstruction from current values |
| Accessibility | Ordered list with textual times/version labels |
| Responsive | One-column chronological list; no horizontal timeline |
| Implementation mapping | List + StatusBadge |
| Source reference | EMP-002, EMP-004; DATA-006; AC-08–AC-09 |

### 13.12 AuditHistoryViewer

| Attribute | Contract |
| --- | --- |
| Component | AuditHistoryViewer |
| Purpose | Inspect protected immutable changes |
| Visual structure | FilterBar, safe event table, detail before/after diff and correlation reference |
| Variants | authorized audit; safe local history projection |
| States | ready, redacted field, no events, error |
| Behavior | Read-only; no edit/delete; never print credentials/raw reasons/photos/GPS |
| Accessibility | Semantic before/after labels and text diff, not color-only |
| Responsive | Mobile stacked diff; table scroll if necessary |
| Implementation mapping | AdvancedDataTable + Dialog/detail section |
| Source reference | SEC-005–SEC-006, SEC-012 |

### 13.13 ApprovalStepper

| Attribute | Contract |
| --- | --- |
| Component | ApprovalStepper |
| Purpose | Understand ordered stage progress |
| Visual structure | Vertical ordered stages with title, status, actual actor/time if allowed, current assignment/fallback label |
| Variants | request/cancellation review history as supplied |
| States | waiting, pending, blocked, approved, rejected, closed |
| Behavior | Current stage only; blocked is not skipped; distinguish configured selector from actual decision-maker |
| Accessibility | ol with text current/completed state; no clickable future-stage decision |
| Responsive | Vertical on all sizes; consistent order, wraps names |
| Implementation mapping | Ordered list + StatusBadge |
| Source reference | LEAVE-007, LEAVE-011; AUTH-008; OD-14 |

### 13.14 ApprovalDecisionPanel

| Attribute | Contract |
| --- | --- |
| Component | ApprovalDecisionPanel |
| Purpose | Make one authorized independent decision |
| Visual structure | Request summary and period charge, current stage, evidence/context, reason field where needed; explicit decision buttons |
| Variants | leave stage/correction/cancellation with domain copy |
| States | eligible, unassigned, self-conflict, prior-actor conflict, stale, blocked, submitting, committed |
| Behavior | Server eligibility reasons; no role override; stage 1 toast is not final leave approval; stale needs fresh review |
| Accessibility | Field legend, error summary, clear labels Approve stage/Approve correction; focus safe on review |
| Responsive | Desktop 320px context aside if room; phone after summary with reserved footer |
| Implementation mapping | Form primitives + AlertDialog where high-impact |
| Source reference | AUTH-008; ATT-009; LEAVE-007–LEAVE-010; AC-20, AC-23–AC-28, AC-46 |

### 13.15 EmployeeProfileSummary

| Attribute | Contract |
| --- | --- |
| Component | EmployeeProfileSummary |
| Purpose | Keep employee identity distinct from login/employment |
| Visual structure | Initial avatar, name/code, employment type, lifecycle, location; optional separate account status |
| Variants | directory/detail/decision compact |
| States | planned/probation/active/ended; account invited/active/disabled or no account |
| Behavior | No account is legitimate; rehire keeps code/history, sensitive contacts omitted separately |
| Accessibility | Clear labeled independent states, no single Active ambiguity |
| Responsive | Stack lifecycle/account rows phone; no oversized hero |
| Implementation mapping | Avatar + DetailSummary |
| Source reference | EMP-001–EMP-004, EMP-007–EMP-008; OD-03–OD-04 |

### 13.16 OrganizationHierarchyViewer

| Attribute | Contract |
| --- | --- |
| Component | OrganizationHierarchyViewer |
| Purpose | Navigate authorized department or reporting structure |
| Visual structure | Separate Department structure / Reporting lines context; indented expandable list, detail panel |
| Variants | catalog hierarchy / scoped reporting |
| States | collapsed/expanded, loading subtree, no children, restricted, invalid proposed cycle |
| Behavior | No drag reparent; edit via effective-date preview. G catalog read does not expose private employee tree |
| Accessibility | Disclosure buttons+lists with aria-expanded; keyboard controls; no canvas-only graph |
| Responsive | Scrollable indentation bounded; open detail below on phone |
| Implementation mapping | Disclosure/Collapsible + detail/form |
| Source reference | ORG-001–ORG-004; AUTH-006; EMP-005 |

### 13.17 ShiftCalendar

| Attribute | Contract |
| --- | --- |
| Component | ShiftCalendar |
| Purpose | Read working obligations and exceptions |
| Visual structure | Date navigation, timezone/scope, day/week/month/agenda view choices, legend, occurrence labels with times and next-day marker |
| Variants | agenda phone; week desktop; month for overview |
| States | working, nonworking, holiday, special work, unassigned, conflicted, protected |
| Behavior | No drag/resizing shortcut. Click permitted occurrence→detail/explicit change preview; weekends follow resolver |
| Accessibility | Named date/cell, text legend and list equivalent; no color-only grid |
| Responsive | Phone agenda default, full labels; desktop week grid with scroll within region |
| Implementation mapping | Application calendar built from typed resolved DTOs, not new calendar library |
| Source reference | SHIFT-001–SHIFT-009; ATT-002; LEAVE-004 |

### 13.18 AttendanceClockPanel

| Attribute | Contract |
| --- | --- |
| Component | AttendanceClockPanel |
| Purpose | Complete one own clock action honestly |
| Visual structure | Current occurrence start/end/date/timezone, expected remaining work, policy cue, PhotoCapturePanel/GeoFeedback, action/result/recovery |
| Variants | private genuine / explicitly isolated demo projection |
| States | not started, open, closed; uploading, validating, unknown result, committed, rejected, unsent |
| Behavior | Server time/result authoritative; recover same intent after lost response; retake new intent; no post-end grace |
| Accessibility | State live region once, descriptive controls; committed time persistent |
| Responsive | Centered max 640px; 44/52px touch controls; no small dense clock dashboard |
| Implementation mapping | features/attendance composition; useClockSubmission |
| Source reference | ATT-001–ATT-005, ATT-013; AC-14–AC-18, AC-43, AC-47–AC-48 |

### 13.19 PhotoCapturePanel

| Attribute | Contract |
| --- | --- |
| Component | PhotoCapturePanel |
| Purpose | Capture/review a genuine private photo |
| Visual structure | Live preview, Take photo, review image, Retake, Use photo; clear permission/device guidance |
| Variants | private genuine only; demo replaces with labeled server fixture preview |
| States | permission prompt, active camera, captured, reviewing, denied, unavailable, invalid upload, stopped |
| Behavior | No file-upload bypass when camera required; no synthetic fallback. Stop tracks/revoke URLs on close; never show photos to unauthorized viewers |
| Accessibility | Real buttons, image description without exposing personal data, persistent error recovery |
| Responsive | Fit full width, aspect ratio preserved; do not crop evidence to decorative avatar |
| Implementation mapping | features/attendance; browser APIs only eligible private HTTPS |
| Source reference | ATT-001–ATT-002, ATT-012; OD-09, OD-20 |

### 13.20 GeoValidationFeedback

| Attribute | Contract |
| --- | --- |
| Component | GeoValidationFeedback |
| Purpose | Explain location policy/outcome without identity claims |
| Visual structure | Policy requirement/mode label, status icon/text, one next action; configured location name if permitted |
| Variants | disabled/optional/required; Standard/Strict |
| States | denied, unavailable, insufficient accuracy, outside, strict uncertainty, valid, checking |
| Behavior | No map/raw GPS default. Standard/Strict server-selected; no client mode toggle/manual coordinates. Validated means policy check only |
| Accessibility | Specific text/error, not a green identity-verification tick alone |
| Responsive | Short readable panel; Retry location or correction path as authorized |
| Implementation mapping | Alert + Text, same attendance feature |
| Source reference | ATT-004–ATT-005; OD-08; AC-17–AC-18 |

### 13.21 LeaveBalanceSummary

| Attribute | Contract |
| --- | --- |
| Component | LeaveBalanceSummary |
| Purpose | Distinguish real posted/reserved/available from forecast |
| Visual structure | Employee/type/year context; Available primary; Posted and Reserved labeled; validity/expiry note; forecast separate |
| Variants | own/authorized admin; exact ledger drill |
| States | loading, positive, zero, insufficient, expired availability, N/A non-balance, error |
| Behavior | Server quantities 2 decimals; period-specific. Forecast never added into available; original lineage visible only in ledger detail |
| Accessibility | Units in each label; description of Posted−Reserved, no color-only deficit |
| Responsive | Stack by year/type; never sum unrelated years into one giant balance |
| Implementation mapping | DetailSummary + numeric text |
| Source reference | LEAVE-002–LEAVE-005; OD-12; AC-22, AC-44–AC-45 |

### 13.22 LeaveRequestForm

| Attribute | Contract |
| --- | --- |
| Component | LeaveRequestForm |
| Purpose | Submit shift-aware leave with all-or-none funding |
| Visual structure | Type/date/full-or-half choice; occurrence/covered-break preview; item period charges; allowed balance panel; routing preview; reason; submit |
| Variants | draft/new/resubmit-stale; self only ordinary request |
| States | calculating, included/excluded/unresolved, conflict, insufficient posted, no first route, submitting, committed |
| Behavior | No invented leave units in browser; first-route failure reserves nothing. Stale acceptance explicitly re-previews/adjusts all reserves; no direct ledger edit |
| Accessibility | Labels, fieldset for half, error summary linked per item/year |
| Responsive | Single-column phone with expandable item details, sticky Submit after reserved padding |
| Implementation mapping | VeeValidate/Zod + DatePicker + domain preview |
| Source reference | LEAVE-004–LEAVE-006, LEAVE-012–LEAVE-015; AC-21–AC-22, AC-42, AC-44–AC-45 |

### 13.23 NotificationCenter

| Attribute | Contract |
| --- | --- |
| Component | NotificationCenter |
| Purpose | Read only owned notification work |
| Visual structure | Unread/all filter, items with safe title/time/read state, mark read, current authorized target link |
| Variants | header preview and full owner center |
| States | loading, empty, unread/read, mark-read pending, inaccessible target, error |
| Behavior | Owner-only count; idempotent read; no sensitive reasons or stale-data preview from old scope |
| Accessibility | List headings/times, item link distinct from mark read, count announced on response |
| Responsive | Header opens sheet/route; full-width phone list |
| Implementation mapping | Dropdown/Sheet + owner list |
| Source reference | NTF-002, NTF-005; AUTH-006 |

### 13.24 ExportStatusPanel

| Attribute | Contract |
| --- | --- |
| Component | ExportStatusPanel |
| Purpose | Explain queued report and safe availability |
| Visual structure | Report name/filter/timezone/cutoff/field context, status, progress if supplied, generated time, expiry, Download/Retry where allowed |
| Variants | inline request confirmation and owner export list |
| States | queued, processing, ready, failed, cancelled, expired; stale context/access separate |
| Behavior | All CSV queued; no fake percentage or preview of private partial. Ready still reauthorizes; stale token asks rebuild |
| Accessibility | Text status and bounded polite updates; Download named with report/time |
| Responsive | List cards only for this job-summary pattern if narrow; same data/state contract |
| Implementation mapping | Progress/Alert/Button; features/analytics |
| Source reference | DASH-007–DASH-009; DATA-010; AC-34 |

### 13.25 EmptyState

| Attribute | Contract |
| --- | --- |
| Component | EmptyState |
| Purpose | Explain a valid empty result |
| Visual structure | Small relevant icon, specific heading, scope/filter sentence, permitted next action |
| Variants | first use / zero matches / genuinely no data |
| States | only authorized successful response |
| Behavior | No employees found is not permission denied; Clear filters or Create only when allowed |
| Accessibility | Heading+plain text, no decorative illustration dependency |
| Responsive | Centered within actual region, modest vertical space |
| Implementation mapping | Text + Button |
| Source reference | AUTH-004; DASH-009; NFR-009 |

### 13.26 ErrorState

| Attribute | Contract |
| --- | --- |
| Component | ErrorState |
| Purpose | Recover a failed view/command safely |
| Visual structure | Short title, safe reason, Retry/return action, optional safe incident reference |
| Variants | network / validation summary / business conflict / stale / service unavailable |
| States | blocking or inline; unknown result separate |
| Behavior | Do not retry a clock with new key when outcome unknown; do not expose SQL/storage/provider secrets |
| Accessibility | New blocking error alert, focus summary after submit; not repeated on poll |
| Responsive | Wrap text, full-width action on phone when needed |
| Implementation mapping | Alert + Button |
| Source reference | ATT-013; DATA-004–DATA-007; NFR-011 |

### 13.27 PermissionDeniedState

| Attribute | Contract |
| --- | --- |
| Component | PermissionDeniedState |
| Purpose | Explain unavailable access without existence leak |
| Visual structure | Generic not available/permission-changed message and safe navigation |
| Variants | 403/404 consistent with backend policy |
| States | initial denied / access revoked during view |
| Behavior | Remove unauthorized data/controls; no blurred field/photo, hidden JSON or client-only guard |
| Accessibility | Readable heading, focus moved after revocation, safe destination link |
| Responsive | Same page-level treatment all sizes |
| Implementation mapping | ErrorState variant |
| Source reference | AUTH-004–AUTH-006; SEC-001, SEC-003 |

### 13.28 ScopeSummary

| Attribute | Contract |
| --- | --- |
| Component | ScopeSummary |
| Purpose | Expose active query context without role switching |
| Visual structure | Current target set label, employee link if allowed, filters, as-of/timezone |
| Variants | own / current team / all permitted |
| States | stable, updating access, narrowed/revoked |
| Behavior | Labels describe server access; no client All toggle creates wider scope. Historical group label is separate |
| Accessibility | Text context, filter removals labeled; no tooltips-only scope rule |
| Responsive | Wrap above content; do not hide on phone |
| Implementation mapping | Shared text/context pattern |
| Source reference | AUTH-003–AUTH-006; RULE-008; DASH-002 |

### 13.29 TemporalImpactPreview

| Attribute | Contract |
| --- | --- |
| Component | TemporalImpactPreview |
| Purpose | Review an effective change and its affected work |
| Visual structure | Original/proposed dates/context, effective timezone, affected schedule/leave/attendance/approvals, blockers and explicit permitted resolution |
| Variants | employment / schedule / calendar |
| States | previewing, valid, blocked protected history, stale, applying, committed |
| Behavior | Recheck at commit; reason required where approved. Never silently extend employment, rewrite protected occurrences or reprice pending leave |
| Accessibility | Comparison headings Original/Proposed and textual consequences; no color-only diff |
| Responsive | Stack original then proposed; blockers before submit; long preview uses page not tiny dialog |
| Implementation mapping | DetailSummary/Alert/AdvancedDataTable + domain form |
| Source reference | EMP-002–EMP-006; SHIFT-006–SHIFT-007; DATA-005; AC-12, AC-43 |

### 13.30 ReportBasisPanel

| Attribute | Contract |
| --- | --- |
| Component | ReportBasisPanel |
| Purpose | Explain a coherent metric/detail/export version |
| Visual structure | Filters, source basis, timezone/as-of/cutoff, units/numerator/denominator, coverage, evaluated-at and freshness; no raw hash required to ordinary user |
| Variants | compact inline metadata / expanded explanation |
| States | provisional, reconciled, incomplete, stale/rebuilding, N/A |
| Behavior | Same server-issued context for chart/detail/export; source mismatch refresh visibly; never frontend recompute |
| Accessibility | Readable structured text/table; accessible Show data link/button |
| Responsive | Compact wraps above chart, expanded disclosure stays available phone |
| Implementation mapping | features/analytics + shared chart wrapper |
| Source reference | DASH-004–DASH-008; DATA-010; AC-30–AC-34 |

## 14. Data Table Standards

TanStack Vue Table uses manualPagination/manualSorting/manualFiltering over Laravel's projected rows, authorized totals and field descriptors. URL query preserves allowlisted search/sort/filter/date/timezone/report context; browser Back restores the committed query. Default page 25, selectable 25/50/100, maximum 100. A filter change resets page 1; changing page never discards active scope/timezone. No “load every record then filter in Vue”, client total of hidden rows or generic all-company summary.

| Element | Selected convention |
| --- | --- |
| Column hierarchy | Primary identity/detail link, status/context/date, relevant measures, optional secondary metadata, persistent Actions |
| Widths | Identity 240px preferred/min 180; date/time 160; state 144; numeric 112; actions 64; selection 44 only where supported. Table min-width depends on selected authorized columns, not a fixed desktop screenshot. |
| Alignment | Text left, day units/rates/counts right with tabular numerals; dates/times left; action icon centered in64px column |
| Labels/sort | Plain labels, no vague State 1; clickable header is button with ascending/descending indicator and aria-sort; default server order explicit in caption/context |
| Search | Named field; 300ms debounce/Enter; Clear; only scoped server search options; pending old results marked Updating |
| Filters | Committed chips plus one Clear filters control; draft edits applied together; invalid dates not sent as silently swapped range |
| Pagination/counts | “1–25 of87 permitted records” only from authorized total; if total withheld, Previous/Next and supplied has_more. Never invent a hidden total. |
| Selection/bulk | Only approved atomic schedule assignment/publication or explicitly supported domain action gets selection. Select-all means loaded page unless backend defines a scoped selection contract. No generic bulk deletion/invitation/approval. |
| Row actions | Primary name/detail link always visible; actions menu permanently discoverable; no hover-only approve/delete buttons |
| Sensitive columns | Server excludes unauthorized column descriptor/value. Hiding with CSS, tooltips, collapsed rows or downloads does not protect data. |
| Export | Explicit “Export CSV” with current source/export capability; uses same query/context and queues a job; never exports only the 25 client rows unless that is an explicit user filter |
| Loading/error | First load matching skeleton; refresh retain coherent authorized rows + busy label; failed refresh announces error/retry and does not label old rows fresh |
| Empty | Distinguish no records yet / no matches / no eligible metric data / N/A denominator / denied access |
| Mobile | Semantic table in named horizontal scroll region with visible scroll affordance; primary identity/status/actions priority, optional secondary columns omitted only with equivalent permitted detail. No page-wide scrolling or wholesale custom card app. |

Keep identity column sticky only when it does not consume most of a 320px viewport or obscure focus; prefer normal horizontal scroll on the smallest widths. Long names may wrap two lines and detail reveals full text. Missing value displays “Not provided”, N/A or a specific unknown label from server, never a dash interpreted as zero. Charts/exports follow the same row scope. Sorting/filter toggles are not optimistic authorization or business state changes.

## 15. Form & Validation Standards

VeeValidate owns form state/errors; Zod 3 input schemas via the established adapter describe shape. Initialize every field and cross-field dependency explicitly. Laravel repeats input and all domain/current-scope/version checks. Inertia/router carries submissions; do not introduce an independent form store or duplicate native/Inertia/VeeValidate state ownership.

Persistent label 14px500 above field, required marker with a form legend, concise helper below, then inline error 13px. Placeholder is an example only. Group fields with headings/fieldset/legend; no floating-label-only design. Validate first on blur after touch, then while correcting invalid fields; submit validates the whole form. Show server 422 errors on their fields and a linked summary for multiple/offscreen failures. Keep entered nonsensitive values on recoverable validation; never keep photos/GPS/tokens/private reasons in localStorage. No new universal autosave or server draft feature is introduced; an approved leave draft reserves nothing.

Dependent options come from authorized DTOs; clearing a parent explicitly clears an invalid child and explains why. Do not silently switch location/timezone, employee, entitlement period, workflow or half-day choice. Date/time fields show date,24-hour time, IANA timezone and overnight next-day label as applicable; effective local 00:00 boundaries and both dates remain understandable. Preview changes are drafts until a confirmed server commit. Input precision and min/max limits mirror server-supplied rules; no invented annual entitlement or maximum report range.

On Submit: mark busy immediately, retain layout/action label, disable duplicate submission and await committed result. Known validation rejection allows correction; uncertain network outcome enters status recovery with the same intent. A retake or deliberate edited payload is a new intent. On stale 409, keep a safe proposed-value summary, fetch current version, show Original/Current/Proposed and require explicit refreshed review; do not auto-overwrite or endlessly replay. On permission change, remove restricted data/actions and explain the edit cannot proceed; do not keep sending old grants. Unsaved navigation confirmation is client discard protection, not a business approval step. Credential expiration uses normal authentication/reauth flow; submitting old photos/commands after login is not automatic.

| Outcome class | Presentation / recovery |
| --- | --- |
| Form/input 422 | Inline field errors + summary; focus first invalid input/summary; preserve permitted values |
| Business-rule rejection | Persistent contextual alert with affected permitted occurrence/year/stage and authorized next step; no false success |
|403/404 permission/hidden target | Consistent generic unavailable state; no hidden record existence, values or count; safe navigation |
| Stale 409/version conflict | Explain newer context; refresh/review/re-preview; never blind overwrite |
| Transient lock/service failure with known rollback | Safe retry guidance and same intent as server contract allows; no partial displayed effect |
| Lost response/unknown commit | “Checking whether your clock was recorded”; status recovery, not a second fresh clock |
|419/authentication expired | Explain session expiry; authenticate; revalidate current context before continuing safe draft |
| Committed success | Persistent entity/time/stage-specific result plus optional toast; reset only the form whose result is confirmed |

### 15.1 High-impact confirmation patterns

| Action | Required visible consequences and exact commitment | Guard preserved |
| --- | --- | --- |
| Termination | Employee, current/proposed local boundary+timezone, open overnight intersections/future shifts/leave/reserves/assigned approvals, reason; “Apply employment ending” | Preview+pre-application recheck; no extension, grace or fake close; owned HR resolution |
| Role/security change | Account/role, explicit added/removed permission scopes, current impact, reauth when required; “Save access changes” | Final-admin/self-escalation, next-check invalidation, public demo denial |
| Correction approval | Original evidence, proposed factual values/unknown-end resolution, affected work/leave/evaluation, reason, reviewer eligibility; “Approve correction” | Independent reviewer/stale conflict; original event/photo retained |
| Balance adjustment | Type/period, signed 4 decimal units, current posted/reserved/available from server, resulting preview and reason; “Post adjustment” | Cannot erase active reserve, no direct ledger editing or forecast credit |
| Approved cancellation | Exact covered segments and originally consumed buckets/periods, eligible units, expired-return explanation; “Approve cancellation” | Original leave effective pending; one linked compensation, independent HR review |
| Protected shift change | Original/proposed occurrence, protected references and stale pending calculations, blockers; “Publish changes” only if allowed | No override-history escape hatch; batch atomic; explicit requester reacceptance |
| Retention/deletion | Exact category/version/validation state, candidate scope/action, current holds/dependencies and last preview; “Run approved cleanup” only eligible | Candidate age≠authority; final current recheck/off-host control; archive≠purge; public demo denied |

Use a single meaningful review+confirmation, Cancel as safe initial focus and action-specific verb. Reauthentication within 15 minutes follows OD-02/SEC-009; it is not a new reviewer/owner signoff. Never add a second approval workflow or typed magic phrase to ordinary safe actions. A disabled high-impact action for known business blockers shows explanation in normal text; unauthorized actions are absent and backend still denies forged commands.

## 16. Module-Specific Interaction Patterns

### 16.1 Identity — WF-01–WF-03

Login is invitation-based with no Sign up link. Activation token states accepted/expired/revoked/used are explicit and do not activate an already disabled account. Expired invitation says “This invitation has expired. Ask your HR administrator to send a new invitation.” The public user cannot resend to an arbitrary address; authorized account UI does resend. Password reset acknowledgement does not reveal whether an email exists. Password minimum 12,48h invitations,60min reset,5 attempts/min account/IP and 30min idle/8h absolute session are existing server rules. Reauthentication is a compact password dialog for a known sensitive action; after 15min require it again per server. “Account disabled” never offers clock-out-only login.

Roles/grants use one read/edit matrix: permission rows grouped by module, exact allowed scope checkboxes all/team/self or global/owner classification, clear default versus effective current grants, no scalar role-wide scope. Multiple roles have a read-only effective-permission preview from server. Public demo shows only sanitized explanatory comparison, no live editor or role-switch privilege.

### 16.2 Workforce — WF-04–WF-06

Directory shows permitted employee code/name, lifecycle, type, department/position/location and separately account presence where granted. No account is a normal value; do not show it as employment error. Profile Overview and Employment history are separate authorized regions. History requires employee.view∩employee.history.view; sensitive details require its separate intersection. Employee self-service changes credentials only; no unsupported self-edit contact/employment form.

Employment change form names the event (promotion/transfer/reporting/location/renewal), effective local date/timezone, Original/Proposed context and required reason. Current organization and “Context at shift start” are labeled independently. Organizational-root manager can be absent; do not invent a required fake manager to satisfy approval routing. Department/reporting graphs remain distinct.

Offboarding prominently shows a 22:00–06:00 session intersected by00:00 termination, dependent future work and owned resolution. HR may explicitly audit boundary adjustment or use independent factual correction. At unchanged boundary access ends 00:00; no “Finish clock-out first” permission grace, automatic employment extension, guessed 06:00 event or automatic post-end absence. Rehire reuses employee identity with a new period; show previous unresolved session with its original period, and account reactivation is a separate authorized action.

### 16.3 Scheduling — WF-07–WF-08

One ShiftCalendar uses server-resolved occurrences; provide agenda/week/month choice, with agenda on phone. Working item: time + location/label; nonworking: “Non-working”; holiday: named holiday; explicit special: “Special working day”; unresolved: “Unassigned schedule”; conflict: warning icon+“Schedule conflict”. A Saturday can be ordinary work. Overnight label shows “22:00,31 Dec →06:00,1 Jan; shift date 31 Dec” rather than moving its report/year automatically.

Template form explicitly captures start/end and “Ends next day”, breaks and validity; rotation shows cycle+anchor, not a guessed Monday reset. Assignment selection can include multiple employees but preview/commit remains one atomic approved batch. Conflict preview identifies authorized employee/date/source and protected history. A protected change cannot be forced through a UI checkbox. Pending leave becomes “Schedule changed—review calculation”; requester explicitly accepts/resubmits recalculated units/reserves. The 90-day preview horizon is labeled a display/materialization horizon, never claimed as the entire validation scope or a maximum user request range. No calendar drag/drop shortcut skips reason/impact validation.

### 16.4 Attendance — WF-09–WF-11

The clock panel first shows current eligible occurrence, local start-date attribution, expected remaining work and server policy. Private valid capture proceeds camera permission→capture→review/retake→submit with location evidence as required→server validation→committed result. Separate photo is required for out. Full approved leave blocks covered clock-in; pending leave alone does not block valid clocking but may create a visible approval conflict. Clock-out finds the original open occurrence after midnight, subject to current account/employment eligibility and allowed window.

| Submission state | Exact treatment / permitted next step |
| --- | --- |
| Genuinely unsent | “Your clock has not been sent.” Keep private in-memory review only; Send if still eligible. A local camera capture is not attendance. |
| Uploading | Progress only if real upload bytes are known; “Uploading photo…”; no committed timestamp/success badge |
| Received/validating | “Photo received. Checking your attendance…”; no fake 100% business completion |
| Committed | “Clock-in recorded at 09:02,23 Oct 2026 (Asia/Jakarta).” Show original server result and own permitted evidence link |
| Validation failed, known no event | Specific approved error and retry/retake/correction guidance; do not show attendance saved |
| Response lost/unknown | “We could not confirm the result. Checking this attempt…”; poll/recover same intent, no automatic new attempt |
| Existing matching success | Show recovered original committed result/time, not a second success event |
| Missing-out / ended access | “Clock-out missing” and owned HR correction path; ending employment never authorizes out-only access or fabricated closure |

Location feedback differentiates disabled (not collected/required), optional unavailable (not a failure solely due to coordinates), permission denied, unavailable, insufficient accuracy, outside geofence, Strict uncertainty rejection and policy check passed. Standard/Strict and radius/accuracy policy are server-selected; no employee toggle, map drag or manual coordinate fields. Use “Location check passed” plus “This does not verify identity.” Strict rejection: “Your location uncertainty extends beyond the permitted area. Retry with better accuracy or contact HR about a correction.” Don't leak precise coordinates or show a fake identity-verification seal.

History/detail show separate expectation, presence, punctuality flags (late and early departure can coexist), completeness, exception and reconciliation. Valid in+missing out is Present / Clock-out missing, not Absent. Unknown/conflicted/unassigned is not a finalized absence. An out timestamp shortly after second-half leave starts does not prove work across all leave. Correction review shows original/proposed/current values and independent reviewer guard; unknown factual end remains unknown and reviewed, not silently 06:00.

### 16.5 Leave — WF-12–WF-16

Balance labels Posted / Reserved / Available have exact day units and year/type; forecast is separate “Expected accrual—not yet available”. Policy source is employee override > location > company; no department layer. Entitlement year is1 January–31 December in company timezone, with each charge attributed by its preserved local shift-start date. Example: Posted 12.00, Reserved 2.00, Available 10.00. Full working day 1.00 and half 0.50 depend on actual resolved working segments excluding breaks, not fixed morning/afternoon clock times. Preview shows each local shift start date, first/second-half intervals, excluded nonworking dates, period charge and applicable bucket funding. Technical half-microsecond representation is not shown to HR users; time labels explain first/second-half coverage while the backend preserves exact precision.

Zero included work, unresolved schedule, overlap/known attendance conflict, forbidden self-service backdate or insufficient posted entitlement blocks submission with its authorized item/year. A future 0.50 forecast cannot fund a 0.50 request; do not offer borrowing/negative balance toggles. Cross-year request shows separate 2026/2027 totals and all-or-none funding; no combined surplus. Carry/expiry/adjustment/reversal history is read-only with linked source, not editable balance cells.

Submission shows route preview. No eligible independent first approver means “No eligible first-stage approver is configured. Nothing was reserved. Contact HR.” It does not require employee to add a fake manager. Later missing candidate shows “Approval blocked” plus “Your reservation remains held”; only authorized reassignment/configuration actions appear. ApprovalStepper shows actual decisions, current stage and waiting stages. An approval stage is not a final Approved leave until the final atomic result. Exclude requester and previous actual approvers by default; a configured reasoned repeat-actor exception is visible in the submitted snapshot/history, not an ad hoc Approve anyway button. Delegation does not grant authority and cannot chain/self-delegate.

Pending rejection/cancel releases reservation once without a consumption reversal message. Approved cancellation remains Pending cancellation while original leave stays Approved and effective; only successful independent cancellation approval changes it and shows actual linked reversal/expiry result. Historical partial HR resolution uses explicit eligible units/original consumption and reason, not the ordinary self-service backdate form. Expired restored units must say “Returned units expired” rather than “Available balance increased”. Reminders 24h/escalation 48h are informational work signals and never auto-approve.

### 16.6 Analytics — WF-17–WF-18

Overview selects permitted source panels and coherent server metadata. Provisional today, reconciled history, incomplete coverage, stale/rebuilding, pending reconciliation and N/A are visible independent dimensions. Show last evaluated time and selected timezone/as-of/cutoff. Workforce distribution uses effective as-of organization, historical attendance uses occurrence context, both under current access. No-account employees remain workforce entities when in scope.

Rates use the backend contract and server numerator/denominator, never frontend average of daily percentages. Full leave is excused, unknown/ongoing/conflict excluded with coverage, missing-out present/incomplete, zero denominator N/A. Golden fixture 60/40/50 and leave 7.5 remain unchanged. Annual leave utilization keeps receiving year/type and cutoff; future planned usage and forecast accrual separate; expiry is disclosed without shrinking the granted denominator. A selected monthly report window never relabels annual grant as monthly. Out-of-range integrity conditions are surfaced, not clamped to0–100%.

Click chart/card uses the same server report token/filter basis to detail/export; changed source shows “This report has changed. Refresh to continue” rather than silently navigating another revision. All CSV queues. Ready download checks current access; revoked, expired, incomplete or retired-generation outputs are unavailable. Display real row progress only when supplied, never a fake time estimate. The 24h artifact candidate does not authorize source-HR deletion.

### 16.7 Shared operational interfaces

Notifications show only safe owned text and reauthorized targets. Audit is read-only with safe diffs. Retention configuration shows candidate duration separately from exact current category/version validation and approval; holds/dependencies block cleanup even after preview. Operations shows safe task/health/failure evidence and authorized retries, never a UI that edits ledger entries, forces approval or bypasses protected history. Backup/restore are automatic operational procedures/evidence; no unapproved generic “Run arbitrary command” or “Upload database” UI. Public demo removes privileged controls and backend denies forged access. Technical job publication, execution, mail uncertainty and business outcome remain separate so “Queued” never implies “Delivered” or “Approved”.

## 17. Screen Inventory & UI Specification Matrix

There are **44 MVP screens/major views**: Identity 7, Workforce 7, Scheduling 5, Attendance 7, Leave 10, Analytics 3 and Shared supporting views 5. These count major workflow views, including a reusable impact-review view; a tab/dialog/error/mobile/state variant is not counted again. They are not 44 mandatory routes or separate page components. All 18 approved workflows are represented. UI IDs identify design references, not new business requirement IDs.

All views inherit token/type/layout, common loading/empty/error/permission/stale rules, current server authorization and field omission. The contracts below add the actual required regions, actions, data and responsive differences needed for pen.dev; a generic wireframe or one screen per persistence table is insufficient. “Permitted” always means current server-provided capability and target eligibility. SA/HR/M/E are descriptive defaults, and custom/multi-role actors use the same views.

| Screen ID | Major view | Owner | Layout | Intended users | Permission boundary | PRD workflows/references |
| --- | --- | --- | --- | --- | --- | --- |
| UI-AUTH-001 | Sign in | Identity | L1 | Invited/active account users; public synthetic personas | Authentication route; no configurable registration grant | AUTH-001; SEC-002; WF-02; AC-02–AC-03 |
| UI-AUTH-002 | Accept invitation | Identity | L1 | Invited employee account | Valid server-resolved single-use invitation; no public self-registration | AUTH-001, AUTH-007; SEC-002; WF-01; AC-01–AC-02 |
| UI-AUTH-003 | Request password reset | Identity | L1 | Existing or unknown email submitter | Authentication broker; rate limits, neutral acknowledgement | AUTH-001; SEC-002; WF-02; AC-02 |
| UI-AUTH-004 | Set new password | Identity | L1 | Valid reset recipient | Valid broker token and current account safeguards | AUTH-001; SEC-002; WF-02; AC-02–AC-03 |
| UI-AUTH-005 | My account | Identity | L4 | All authenticated owners, including no employee link | Own authenticated credentials; account.view for account data; no acting as another user | AUTH-001, AUTH-007; SEC-002, SEC-009; WF-02; OD-04 |
| UI-AUTH-006 | Accounts and invitations | Identity | L3 + L5 sheet | SA/HR defaults; custom account readers/operators | account.view target set; account.invite/deactivate/reactivate per action; role.assign only for role assignment | AUTH-001, AUTH-005–AUTH-007; EMP-003; WF-01, WF-03; AC-01–AC-03, AC-40 |
| UI-AUTH-007 | Roles and effective access | Identity | L3 + L4 | SA/private security operator; custom explicit grants | role.view; role.manage for definitions; role.assign + authorized target reads for assignment | AUTH-002–AUTH-006, AUTH-008; WF-03; AC-04–AC-06, AC-40, AC-46 |
| UI-EMP-001 | Employee directory | Workforce | L3 | SA/HR/M; self-only reader gets own permitted projection | employee.view; employee.create for entry action; employee.export + read for CSV | EMP-001, EMP-003, EMP-007–EMP-008; AUTH-004; WF-04–WF-05; AC-05–AC-08, AC-33 |
| UI-EMP-002 | Employee profile and employment history | Workforce | L4 | Current permitted readers; SA/HR edit defaults | employee.view; sensitive/history intersections; update/employment.manage for respective action | EMP-001–EMP-008; ORG-002; WF-04–WF-06; AC-08–AC-09, AC-43 |
| UI-EMP-003 | Create or edit employee | Workforce | L5 | SA/HR; custom create/update actor | employee.create for new; employee.update and authorized read/projection for existing target | EMP-001, EMP-003, EMP-005, EMP-007; OD-04; WF-04; AC-01, AC-07 |
| UI-EMP-004 | Effective employment change | Workforce | L5 | SA/HR; authorized employment manager | employment.manage for target, employee.view for shown context; graph actions remain authorized | EMP-002, EMP-005; ORG-002–ORG-004; DATA-005; WF-05; AC-06–AC-08 |
| UI-EMP-005 | Offboarding review | Workforce | L5 + TemporalImpactPreview | SA/HR authorized for employee | employment.manage target + employee.view; related attendance/leave actions separately checked | EMP-003–EMP-006; ATT-002, ATT-007–ATT-010; OD-03; WF-06; AC-43 |
| UI-EMP-006 | Rehire employee | Workforce | L5 | SA/HR authorized for existing ended employee | employment.manage target; employee.view; account.reactivate separately for login | EMP-002–EMP-006; AUTH-001; WF-06; AC-09, AC-43 |
| UI-EMP-007 | Organization workspace | Workforce | L3/L4 | Reference readers; SA/HR managers; scoped employee readers | organization.view; organization.manage edit; employee.view for reporting-person nodes | ORG-001–ORG-004; EMP-005; WF-05; AC-07–AC-08, AC-42 |
| UI-SHIFT-001 | Resolved schedule calendar | Scheduling | L3 with ShiftCalendar | All permitted own/team/company readers; SA/HR planners | shift.view target set; assign/override actions separate; calendar reference content as allowed | SHIFT-002–SHIFT-009; RULE-002, RULE-009; WF-07–WF-08; AC-10–AC-12, AC-16 |
| UI-SHIFT-002 | Shift templates | Scheduling | L3+L5 | SA/HR; custom global configuration grant | shift.template.manage (G) | SHIFT-001, SHIFT-006–SHIFT-007; OD-05; WF-07; AC-12 |
| UI-SHIFT-003 | Assign weekly, rotation or dated schedules | Scheduling | L5 | SA/HR; permitted scheduler | shift.assign target set; shown records require authorized schedule/employee projections | SHIFT-002, SHIFT-004, SHIFT-006, SHIFT-008–SHIFT-009; WF-07; AC-11–AC-12 |
| UI-SHIFT-004 | Holidays and individual overrides | Scheduling | L3+L5 | Calendar readers; SA/HR editors; custom authorized operator | calendar.view/manage for shared calendar; shift.override for employee target; required source reads | SHIFT-003, SHIFT-005–SHIFT-008; OD-06; WF-08; AC-10–AC-12 |
| UI-SHIFT-005 | Schedule impact review | Scheduling | L5 focused major review view | Authorized actor applying schedule/calendar changes | Original command capability plus authorized impact projections; no extra approval permission | SHIFT-004, SHIFT-006–SHIFT-009; LEAVE-012; DATA-005; WF-07–WF-08; AC-11–AC-12 |
| UI-ATT-001 | Clock attendance | Attendance | L6 | Linked eligible account holder; isolated public demo persona | attendance.clock own target; current employment/account/schedule/leave/window guards | ATT-001–ATT-005, ATT-012–ATT-013; WF-09–WF-10; AC-13–AC-18, AC-29, AC-43, AC-47–AC-48 |
| UI-ATT-002 | Attendance history and records | Attendance | L3 | All scoped readers; SA/HR/M operational; own self-service | attendance.view target set; attendance.export+read for CSV | ATT-006–ATT-010; DASH-005, DASH-009; WF-10–WF-11; AC-16, AC-19, AC-30, AC-33 |
| UI-ATT-003 | Attendance detail and evidence | Attendance | L4 | Authorized source readers; photo readers subset | attendance.view; attendance.photo.view intersection for private evidence; sensitive/history projections as authorized | ATT-002, ATT-006–ATT-012; DATA-006; WF-11; AC-19–AC-21, AC-30, AC-43 |
| UI-ATT-004 | Request or initiate attendance correction | Attendance | L5 | Employee own requester; authorized HR attendance.resolve actor | attendance.correction.request own target OR attendance.resolve authorized target; source data read separately | ATT-008–ATT-009, ATT-011; EMP-006; WF-11; AC-20, AC-43 |
| UI-ATT-005 | Correction review inbox and decision | Attendance | L3 inbox + L4 selected review | Independent authorized reviewer; SA/HR defaults | attendance.view ∩ attendance.correction.review on target; no submitter/affected self-review | AUTH-008; ATT-009–ATT-011; DATA-005; WF-11; AC-20, AC-30, AC-43 |
| UI-ATT-006 | Reconciliation and owned exceptions | Attendance | L3/L4 | SA/HR; authorized attendance operator | attendance.view + attendance.reconcile/attendance.resolve for relevant command | ATT-007, ATT-010–ATT-011; EMP-006; NTF-006; WF-06, WF-11; AC-19, AC-29–AC-30, AC-43 |
| UI-ATT-007 | Attendance policy configuration | Attendance | L5 | SA/HR; explicit global policy manager | attendance.policy.manage (G); authorized configuration references only | ATT-004–ATT-006; OD-07–OD-09; WF-09–WF-10 policy context; AC-15, AC-17–AC-18 |
| UI-LEAVE-001 | My leave workspace | Leave | L3/L2 compact | Linked own-target users as granted | Separate leave.view / leave.balance.view / leave.request capabilities per panel | LEAVE-002, LEAVE-004–LEAVE-006, LEAVE-010; WF-13, WF-16; AC-22, AC-44–AC-45 |
| UI-LEAVE-002 | Leave request and shift-aware preview | Leave | L5 | Own-target requester; no unapproved HR backdate shortcut | leave.request (S); optional balance/reason/source details independently authorized | LEAVE-004–LEAVE-006, LEAVE-012–LEAVE-015; WF-13; AC-21–AC-22, AC-42, AC-44–AC-45 |
| UI-LEAVE-003 | Leave request detail and history | Leave | L4 | Current permitted requester/reviewer/HR reader | leave.view target; leave.sensitive.view reason; approve/cancel/reassign action capabilities separately | LEAVE-007–LEAVE-013, LEAVE-015; WF-14–WF-16; AC-23–AC-26, AC-42, AC-46 |
| UI-LEAVE-004 | Assigned approval inbox and stage review | Leave | L3 inbox + L4 decision | Assigned eligible approver; SA/HR/M defaults | leave.view ∩ leave.approve + current assignment/delegation/independence | AUTH-008; LEAVE-007–LEAVE-009, LEAVE-011; WF-14–WF-15; AC-23–AC-24, AC-28, AC-42, AC-46 |
| UI-LEAVE-005 | Cancellation and historical resolution review | Leave | L3/L4 | Independent authorized HR reviewer | leave.view ∩ leave.cancel.review; further balance/conflict action grants as required by existing workflow | LEAVE-010, LEAVE-013; OD-12–OD-13; WF-16; AC-25–AC-26, AC-45 |
| UI-LEAVE-006 | Balances and immutable ledger | Leave | L3/L4 | Permitted self/team/all balance reader; HR adjustment actor | leave.balance.view target; leave.balance.adjust for signed posting; sensitive reasons separately masked | LEAVE-002–LEAVE-003, LEAVE-014–LEAVE-015; WF-12, WF-16; AC-22, AC-26–AC-27, AC-45 |
| UI-LEAVE-007 | Leave types and effective policies | Leave | L3+L5 | SA/HR; explicit global policy manager | leave.policy.manage (G); authorized override/reference options | LEAVE-001–LEAVE-003, LEAVE-012; OD-11–OD-12; WF-12; AC-27, AC-44–AC-45 |
| UI-LEAVE-008 | Approval workflow configuration | Leave | L5 | SA/private explicit workflow configurator | leave.workflow.manage (G); no implied HR intervention grant | LEAVE-006–LEAVE-007, LEAVE-011; AUTH-008; OD-14; WF-13–WF-15; AC-42, AC-46 |
| UI-LEAVE-009 | Dated delegation | Leave | L3+L5 | M own default; SA/HR scoped delegation managers | leave.delegation.manage target; current existing read/approve authority, no new grant | LEAVE-011; AUTH-008; WF-15; AC-28, AC-46 |
| UI-LEAVE-010 | Entitlement processing | Leave | L7/L5 | SA/HR; global authorized balance processor | leave.balance.process (G); employee detail reads independently required | LEAVE-002–LEAVE-003, LEAVE-014; NTF-006; WF-12; AC-27, AC-44–AC-45 |
| UI-DASH-001 | Overview dashboard | Analytics | L2 | SA/HR/M/E according to current grants | dashboard.view ∩ relevant source reads and field sets | DASH-001–DASH-006; AUTH-004; WF-17; AC-30–AC-33, AC-41, AC-45 |
| UI-DASH-002 | Report explorer and drill-down | Analytics | L3/L4 | Permitted source/report reader | dashboard.view plus relevant source-read intersection; export additionally branch-specific | DASH-002–DASH-009; DATA-010; WF-17–WF-18; AC-30–AC-34, AC-45 |
| UI-DASH-003 | My CSV exports | Analytics | L3/L7 | Current authorized export initiator | Owner AND current relevant read/export/field/scope/generation checks; no generic new permission | DASH-007–DASH-009; DATA-010; WF-18; AC-33–AC-35 |
| UI-SHARED-001 | Notifications | Shared | L7/list | All authenticated owners | notification.view (O); notification.update (O) | NTF-002, NTF-005; AUTH-006; WF-01–WF-18 subsequent work; AC-06, AC-33, AC-35 |
| UI-SHARED-002 | Audit records | Shared | L3/L4 | SA/private explicitly authorized audit reader | audit.view (G); redacted safe projection | SEC-005–SEC-006, SEC-010, SEC-012; WF-03–WF-16 evidence; AC-36 |
| UI-SHARED-003 | Retention policies, holds and cleanup | Shared | L7/L5 | Private authorized retention operator | retention.manage (G); current reauthentication; existing exact policy validation/approval | SEC-007–SEC-009; DATA-006; OD-16, OD-21; AC-37 |
| UI-SHARED-004 | Operations health and failures | Shared | L7/L3 | Private authorized operator | operations.view; operations.retry for permitted task; domain action permission and reauth where required; demo.reset scheduler/operator only | NTF-003, NTF-006; NFR-011–NFR-012, NFR-015–NFR-016; OD-18, OD-20; AC-35, AC-38, AC-40 |
| UI-SHARED-005 | About this public demo | Shared | L7 or L1 information panel | Public synthetic HR/Manager/Employee visitors | Explicit synthetic deployment projection, no role.view/manage/assign grant | SEC-011; NFR-014; OD-20; AC-40, AC-47–AC-48 |

### 17.1 UI-AUTH-001 — Sign in

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-AUTH-001 |
| Screen name | Sign in |
| Module | Identity |
| Primary users | Invited/active account users; public synthetic personas |
| Permission boundary | Authentication route; no configurable registration grant |
| Purpose | Start an eligible session |
| Main UI regions | Text wordmark; email/password; sign-in action; reset link; neutral auth error; isolated-demo notice/link when applicable |
| Layout | L1 |
| Primary action | Sign in |
| Secondary actions | Reset password; About this demo only on synthetic deployment |
| Data requirements | Safe auth outcome, password/rate-limit guidance, demo-mode projection; no account-existence enumeration |
| Interaction states | idle/submitting/invalid credentials/throttled/disabled/session-expired return |
| Responsive considerations | 400px max form; 16px gutters,44px controls; no decorative split-screen image required |
| PRD references | AUTH-001; SEC-002; WF-02; AC-02–AC-03 |

### 17.2 UI-AUTH-002 — Accept invitation

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-AUTH-002 |
| Screen name | Accept invitation |
| Module | Identity |
| Primary users | Invited employee account |
| Permission boundary | Valid server-resolved single-use invitation; no public self-registration |
| Purpose | Activate one invited account |
| Main UI regions | Invitation context limited to intended recipient; password/confirmation; 12-character help; activation result; expired/revoked/used state |
| Layout | L1 |
| Primary action | Activate account |
| Secondary actions | Return to sign in; ask authorized HR to resend if invalid |
| Data requirements | Validated invitation state/expiry and permitted recipient display; never plaintext token in props/history/debug |
| Interaction states | valid/submitting/accepted/expired/revoked/already used/ineligible disabled |
| Responsive considerations | Single form, persistent token failure text; no mobile-only alternate onboarding |
| PRD references | AUTH-001, AUTH-007; SEC-002; WF-01; AC-01–AC-02 |

### 17.3 UI-AUTH-003 — Request password reset

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-AUTH-003 |
| Screen name | Request password reset |
| Module | Identity |
| Primary users | Existing or unknown email submitter |
| Permission boundary | Authentication broker; rate limits, neutral acknowledgement |
| Purpose | Request a reset safely |
| Main UI regions | Email field; minimum guidance; send link action; nonenumerating confirmation |
| Layout | L1 |
| Primary action | Send reset link |
| Secondary actions | Return to sign in |
| Data requirements | Safe broker/rate-limit response; no existence or disabled-status disclosure |
| Interaction states | idle/submitting/generic acknowledgement/throttled/network error |
| Responsive considerations | Full-width primary phone; generic confirmation wraps |
| PRD references | AUTH-001; SEC-002; WF-02; AC-02 |

### 17.4 UI-AUTH-004 — Set new password

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-AUTH-004 |
| Screen name | Set new password |
| Module | Identity |
| Primary users | Valid reset recipient |
| Permission boundary | Valid broker token and current account safeguards |
| Purpose | Reset password without reactivating disabled access |
| Main UI regions | Password/confirmation; expiry guidance; committed result; invalid-link recovery |
| Layout | L1 |
| Primary action | Reset password |
| Secondary actions | Request another reset; sign in after confirmed result |
| Data requirements | Safe token state and password errors; session invalidation result, not token/hash |
| Interaction states | valid/submitting/expired/used/invalid/confirmed/disabled remains disabled |
| Responsive considerations | Same auth field sizing and focus/errors |
| PRD references | AUTH-001; SEC-002; WF-02; AC-02–AC-03 |

### 17.5 UI-AUTH-005 — My account

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-AUTH-005 |
| Screen name | My account |
| Module | Identity |
| Primary users | All authenticated owners, including no employee link |
| Permission boundary | Own authenticated credentials; account.view for account data; no acting as another user |
| Purpose | Understand own account and change credentials |
| Main UI regions | Name/email/account status; optional permitted employee link; change-password form; session/reauth guidance; sign out |
| Layout | L4 |
| Primary action | Change password |
| Secondary actions | Open own employee profile when permitted; sign out |
| Data requirements | Safe own account DTO, link availability, server password/reauth requirements; no role-catalog/security secrets |
| Interaction states | loading/ready/no employee link/validation error/submitting/confirmed/session revoked |
| Responsive considerations | One column phone; no employee-contact edit fields |
| PRD references | AUTH-001, AUTH-007; SEC-002, SEC-009; WF-02; OD-04 |

### 17.6 UI-AUTH-006 — Accounts and invitations

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-AUTH-006 |
| Screen name | Accounts and invitations |
| Module | Identity |
| Primary users | SA/HR defaults; custom account readers/operators |
| Permission boundary | account.view target set; account.invite/deactivate/reactivate per action; role.assign only for role assignment |
| Purpose | Manage optional login lifecycle separately from employment |
| Main UI regions | Account table with separate employee/lifecycle/account states; authorized employee invitation form; invitation history/status; controlled disable/reactivate and role-assignment panel |
| Layout | L3 + L5 sheet |
| Primary action | Invite selected employee (if allowed) |
| Secondary actions | Resend current invitation; disable/reactivate; assign roles only with its grant |
| Data requirements | Scoped account list/count, optional employee summary only with read, invitation issuance/expiry, action eligibility and version |
| Interaction states | loading/empty/no matches/invited/active/disabled/expired invite/duplicate link/stale/last-admin blocker/public-demo denied |
| Responsive considerations | Scrollable priority account/status/action columns; lifecycle action opens full-width sheet phone |
| PRD references | AUTH-001, AUTH-005–AUTH-007; EMP-003; WF-01, WF-03; AC-01–AC-03, AC-40 |

### 17.7 UI-AUTH-007 — Roles and effective access

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-AUTH-007 |
| Screen name | Roles and effective access |
| Module | Identity |
| Primary users | SA/private security operator; custom explicit grants |
| Permission boundary | role.view; role.manage for definitions; role.assign + authorized target reads for assignment |
| Purpose | Configure exact grants without role inheritance |
| Main UI regions | Role list; selected role name/description; grouped permission/scope matrix; effective-access preview; reasoned changed-grant review; separate assignment region |
| Layout | L3 + L4 |
| Primary action | Save role changes (if allowed) |
| Secondary actions | Create role; review effective access; assign to permitted target |
| Data requirements | Frozen permission catalog/allowed scope pairs, selected direct grants and server effective preview; versions/last-admin/self-escalation eligibility |
| Interaction states | read-only/editing/invalid scope/multi-role union/stale/forbidden escalation/public demo unavailable |
| Responsive considerations | Matrix named horizontal region; phone edit one permission row in Sheet with allowed scope checks |
| PRD references | AUTH-002–AUTH-006, AUTH-008; WF-03; AC-04–AC-06, AC-40, AC-46 |

### 17.8 UI-EMP-001 — Employee directory

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-EMP-001 |
| Screen name | Employee directory |
| Module | Workforce |
| Primary users | SA/HR/M; self-only reader gets own permitted projection |
| Permission boundary | employee.view; employee.create for entry action; employee.export + read for CSV |
| Purpose | Find and inspect employees within current scope |
| Main UI regions | PageHeader/ScopeSummary; name/code search; lifecycle/type/department/location filters; identity-first table; authorized count/pagination |
| Layout | L3 |
| Primary action | Add employee if allowed, otherwise open detail |
| Secondary actions | Export CSV; clear filters; switch permitted query context |
| Data requirements | Scoped employee rows, independent lifecycle/type/account labels, authorized field descriptors, action capabilities |
| Interaction states | loading/no employees/no matches/read-only/revoked/error/export queued |
| Responsive considerations | Identity/status/action priority; horizontal table region; filters Sheet phone |
| PRD references | EMP-001, EMP-003, EMP-007–EMP-008; AUTH-004; WF-04–WF-05; AC-05–AC-08, AC-33 |

### 17.9 UI-EMP-002 — Employee profile and employment history

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-EMP-002 |
| Screen name | Employee profile and employment history |
| Module | Workforce |
| Primary users | Current permitted readers; SA/HR edit defaults |
| Permission boundary | employee.view; sensitive/history intersections; update/employment.manage for respective action |
| Purpose | Read current identity and preserved periods/context |
| Main UI regions | EmployeeProfileSummary; Overview; permitted private section; Employment history/timeline; current vs historical label; separate account link/status; next authorized actions |
| Layout | L4 |
| Primary action | Start permitted employment change or read profile |
| Secondary actions | Edit allowed fields; invite/open account; view original-period attendance/leave when granted |
| Data requirements | Purpose-specific current and historical DTOs, recorded/effective dates, period IDs rendered human labels, restricted data absent |
| Interaction states | loading/ready/no account/planned/active/ended/rehire history/history unavailable/stale |
| Responsive considerations | Summary then tabs/sections; timeline vertical; no oversized hero or hidden sensitive mobile content |
| PRD references | EMP-001–EMP-008; ORG-002; WF-04–WF-06; AC-08–AC-09, AC-43 |

### 17.10 UI-EMP-003 — Create or edit employee

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-EMP-003 |
| Screen name | Create or edit employee |
| Module | Workforce |
| Primary users | SA/HR; custom create/update actor |
| Permission boundary | employee.create for new; employee.update and authorized read/projection for existing target |
| Purpose | Capture approved employee fields without requiring login |
| Main UI regions | Identity/code/name; start/type/lifecycle; activation organization/position/level/location; optional root manager; permitted contacts/notes; optional separately authorized invitation after creation |
| Layout | L5 |
| Primary action | Create employee / Save employee changes |
| Secondary actions | Cancel; review dependent context when material change requires employment workflow |
| Data requirements | Approved field schema, authorized catalogs/options, required activation flags, version; no salary/bank/national-ID/medical fields |
| Interaction states | editing/validation/required activation missing/duplicate code/stale/submitting/committed |
| Responsive considerations | Single column until form container≥640px; related pairs grouped; persistent errors |
| PRD references | EMP-001, EMP-003, EMP-005, EMP-007; OD-04; WF-04; AC-01, AC-07 |

### 17.11 UI-EMP-004 — Effective employment change

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-EMP-004 |
| Screen name | Effective employment change |
| Module | Workforce |
| Primary users | SA/HR; authorized employment manager |
| Permission boundary | employment.manage for target, employee.view for shown context; graph actions remain authorized |
| Purpose | Record promotion/transfer/reporting/location/renewal with impact |
| Main UI regions | Change-kind heading; current context; proposed fields/effective local boundary+timezone; reason; TemporalImpactPreview including future relationships and dependent work |
| Layout | L5 |
| Primary action | Review then apply change |
| Secondary actions | Revise proposal; cancel; open permitted blocker detail |
| Data requirements | Original/current/proposed context, server graph/date validation, future effect and expected version |
| Interaction states | editing/previewing/valid/cycle/overlap/dependency conflict/stale/applying/committed |
| Responsive considerations | Original then proposed stacked phone; blockers before action footer |
| PRD references | EMP-002, EMP-005; ORG-002–ORG-004; DATA-005; WF-05; AC-06–AC-08 |

### 17.12 UI-EMP-005 — Offboarding review

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-EMP-005 |
| Screen name | Offboarding review |
| Module | Workforce |
| Primary users | SA/HR authorized for employee |
| Permission boundary | employment.manage target + employee.view; related attendance/leave actions separately checked |
| Purpose | End employment at explicit boundary and own dependent conflicts |
| Main UI regions | Employee/period; current/proposed boundary; owned overnight session intersection; future shifts/pending leave/reserves/assigned approvals; reason; resolution choices; final current recheck |
| Layout | L5 + TemporalImpactPreview |
| Primary action | Apply employment ending when eligible |
| Secondary actions | Explicit reasoned boundary revision; authorized independent correction path; cancel preview |
| Data requirements | Scoped dependent impact projections, no post-end grace option, account-ending consequence and HR exception ownership |
| Interaction states | previewing/conflicted/stale/resolution planned/applying/ended/unresolved owned exception |
| Responsive considerations | Blockers and exact overnight dates visible before final button; do not collapse essential warning on phone |
| PRD references | EMP-003–EMP-006; ATT-002, ATT-007–ATT-010; OD-03; WF-06; AC-43 |

### 17.13 UI-EMP-006 — Rehire employee

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-EMP-006 |
| Screen name | Rehire employee |
| Module | Workforce |
| Primary users | SA/HR authorized for existing ended employee |
| Permission boundary | employment.manage target; employee.view; account.reactivate separately for login |
| Purpose | Create a new period while retaining identity/history |
| Main UI regions | Stable code/name; previous periods and unresolved-session notice; new start/type/context; activation requirements; independent account state/reactivation summary |
| Layout | L5 |
| Primary action | Create new employment period |
| Secondary actions | Return to profile; authorized account reactivation separately |
| Data requirements | Existing identity/period history, new-period inputs, nonoverlap/current versions, retained balances/access metadata only permitted |
| Interaction states | editing/overlapping period/missing activation context/stale/committed/account still disabled |
| Responsive considerations | Stack prior-period summary before new form; no wizard implying a new employee code |
| PRD references | EMP-002–EMP-006; AUTH-001; WF-06; AC-09, AC-43 |

### 17.14 UI-EMP-007 — Organization workspace

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-EMP-007 |
| Screen name | Organization workspace |
| Module | Workforce |
| Primary users | Reference readers; SA/HR managers; scoped employee readers |
| Permission boundary | organization.view; organization.manage edit; employee.view for reporting-person nodes |
| Purpose | Maintain reference structure and inspect distinct reporting relationships |
| Main UI regions | Company/timezone context; Department structure / Reporting lines tabs; position/level/location reference panels; selected-node details and effective-dated edit form |
| Layout | L3/L4 |
| Primary action | Add/edit permitted organization reference |
| Secondary actions | Expand scoped tree; open allowed employee; retire reference with protection check |
| Data requirements | Authorized catalog tree, timezone/location metadata, scoped reporting node projection, referenced-record retirement blockers |
| Interaction states | loading/no nodes/expanded subtree/read-only/cycle/reference-in-use/stale |
| Responsive considerations | Indented disclosure list+below detail phone, no canvas-only org chart or drag reparent |
| PRD references | ORG-001–ORG-004; EMP-005; WF-05; AC-07–AC-08, AC-42 |

### 17.15 UI-SHIFT-001 — Resolved schedule calendar

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-SHIFT-001 |
| Screen name | Resolved schedule calendar |
| Module | Scheduling |
| Primary users | All permitted own/team/company readers; SA/HR planners |
| Permission boundary | shift.view target set; assign/override actions separate; calendar reference content as allowed |
| Purpose | Understand actual resolved work including overnight/special dates |
| Main UI regions | Scope/date/timezone controls; agenda/week/month; legend; source/resolution details; protected/history marker; allowed planning action |
| Layout | L3 with ShiftCalendar |
| Primary action | View occurrence / Assign schedule if allowed |
| Secondary actions | Previous/Next/Today; choose view; open impact preview |
| Data requirements | Server-resolved occurrences/work intervals/holiday classification/unassigned/conflict; original period/context when allowed |
| Interaction states | loading/working/nonworking/holiday/special/unassigned/conflict/protected |
| Responsive considerations | Phone agenda; desktop week; labeled table/list equivalent; no weekend=off shortcut |
| PRD references | SHIFT-002–SHIFT-009; RULE-002, RULE-009; WF-07–WF-08; AC-10–AC-12, AC-16 |

### 17.16 UI-SHIFT-002 — Shift templates

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-SHIFT-002 |
| Screen name | Shift templates |
| Module | Scheduling |
| Primary users | SA/HR; custom global configuration grant |
| Permission boundary | shift.template.manage (G) |
| Purpose | Define valid local times, overnight end and breaks |
| Main UI regions | Template list/name; selected immutable revision context; start/end and next-day choice; ordered break form; working-duration preview; retirement/source impact notice |
| Layout | L3+L5 |
| Primary action | Save template revision |
| Secondary actions | Create template; retire eligible template; review impacts |
| Data requirements | Authorized template definitions/revisions, server validity/duration/break/DST validation and protected reference impact |
| Interaction states | editing/invalid interval/break overlap/DST resolution required/referenced/stale/committed |
| Responsive considerations | Form sections stacked; breaks simple ordered rows with labeled add/remove, no drag-only control |
| PRD references | SHIFT-001, SHIFT-006–SHIFT-007; OD-05; WF-07; AC-12 |

### 17.17 UI-SHIFT-003 — Assign weekly, rotation or dated schedules

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-SHIFT-003 |
| Screen name | Assign weekly, rotation or dated schedules |
| Module | Scheduling |
| Primary users | SA/HR; permitted scheduler |
| Permission boundary | shift.assign target set; shown records require authorized schedule/employee projections |
| Purpose | Publish approved recurring/date assignment batch |
| Main UI regions | Authorized employee selection; weekly/rotation/dated radio; pattern/template; anchor; effective range/location; reason; batch impact summary |
| Layout | L5 |
| Primary action | Preview assignments then Publish eligible batch |
| Secondary actions | Edit selection/anchor/range; cancel |
| Data requirements | Authorized target options, pattern definition, exact effective range, conflict/protection results and expected versions |
| Interaction states | editing/previewing/overlap/future conflict/unassigned configuration/stale/batch rejected/committed |
| Responsive considerations | Selection table scrolls phone; preview separate full view; no partial-success checkbox |
| PRD references | SHIFT-002, SHIFT-004, SHIFT-006, SHIFT-008–SHIFT-009; WF-07; AC-11–AC-12 |

### 17.18 UI-SHIFT-004 — Holidays and individual overrides

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-SHIFT-004 |
| Screen name | Holidays and individual overrides |
| Module | Scheduling |
| Primary users | Calendar readers; SA/HR editors; custom authorized operator |
| Permission boundary | calendar.view/manage for shared calendar; shift.override for employee target; required source reads |
| Purpose | Set named shared exceptions or explicit special work |
| Main UI regions | Company/location calendar list; individual override context; date/layer/type; working template if needed; reason; precedence explanation and impact preview |
| Layout | L3+L5 |
| Primary action | Save authorized holiday/override after preview |
| Secondary actions | View replaced context; cancel; open affected permitted occurrence |
| Data requirements | Approved calendar layers/labels, resolved precedence, authorized target/reference options, conflicts/protected dependencies |
| Interaction states | read-only/editing/equal-priority conflict/invalid work result/protected/stale/committed |
| Responsive considerations | Phone date list + form sheet/page; no separate full calendar engine |
| PRD references | SHIFT-003, SHIFT-005–SHIFT-008; OD-06; WF-08; AC-10–AC-12 |

### 17.19 UI-SHIFT-005 — Schedule impact review

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-SHIFT-005 |
| Screen name | Schedule impact review |
| Module | Scheduling |
| Primary users | Authorized actor applying schedule/calendar changes |
| Permission boundary | Original command capability plus authorized impact projections; no extra approval permission |
| Purpose | Review all affected work before atomic publication |
| Main UI regions | Original/proposed effective dates/timezone; authorized affected employees/dates; overlap blockers; attendance/approved leave references; stale-pending-leave warning; notifications consequence |
| Layout | L5 focused major review view |
| Primary action | Publish changes only if server eligible |
| Secondary actions | Back to proposal; open permitted blocker; cancel |
| Data requirements | Versioned preview, target summary, full affected-range validation outcome, protected blockers; no raw DB keys |
| Interaction states | previewing/valid/blocked/stale/applying/whole batch failed/committed |
| Responsive considerations | Stack comparisons; blockers above table and action; horizontal region for detailed affected rows |
| PRD references | SHIFT-004, SHIFT-006–SHIFT-009; LEAVE-012; DATA-005; WF-07–WF-08; AC-11–AC-12 |

### 17.20 UI-ATT-001 — Clock attendance

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-ATT-001 |
| Screen name | Clock attendance |
| Module | Attendance |
| Primary users | Linked eligible account holder; isolated public demo persona |
| Permission boundary | attendance.clock own target; current employment/account/schedule/leave/window guards |
| Purpose | Record genuine private clock or clearly simulated demo command |
| Main UI regions | Occurrence/date/timezone/policy; current session; PhotoCapturePanel + GeoValidationFeedback private, synthetic fixture explanation demo; Submit/result/recovery |
| Layout | L6 |
| Primary action | Clock in / Clock out (current permitted action) |
| Secondary actions | Retake before send; retry same intent/status; authorized correction route |
| Data requirements | Own resolved occurrence/work segments, server-selected evidence mode, windows, command recovery DTO, committed server time; no raw GPS in props |
| Interaction states | camera/location states; unsent/uploading/validating/committed/known rejection/unknown result/ended/no schedule/on leave |
| Responsive considerations | Phone 52px primary; portrait/landscape preview; no public camera/GPS API invocation |
| PRD references | ATT-001–ATT-005, ATT-012–ATT-013; WF-09–WF-10; AC-13–AC-18, AC-29, AC-43, AC-47–AC-48 |

### 17.21 UI-ATT-002 — Attendance history and records

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-ATT-002 |
| Screen name | Attendance history and records |
| Module | Attendance |
| Primary users | All scoped readers; SA/HR/M operational; own self-service |
| Permission boundary | attendance.view target set; attendance.export+read for CSV |
| Purpose | Read independent attendance dimensions in scope |
| Main UI regions | Scope/date/department/location/status filters; identity/date/shift table; presence+completeness+punctuality/freshness columns; authorized total/detail links |
| Layout | L3 |
| Primary action | Open record |
| Secondary actions | Export CSV if granted; request correction for own permitted occurrence |
| Data requirements | Scoped authorized rows with local shift-start attribution, dimensions, safe actions and freshness |
| Interaction states | loading/no records/no matches/present missing-out/provisional/reconciled/conflict/stale/error |
| Responsive considerations | Identity/date/presence/incomplete priority; detailed dimensions remain in linked detail, not hidden data transfer |
| PRD references | ATT-006–ATT-010; DASH-005, DASH-009; WF-10–WF-11; AC-16, AC-19, AC-30, AC-33 |

### 17.22 UI-ATT-003 — Attendance detail and evidence

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-ATT-003 |
| Screen name | Attendance detail and evidence |
| Module | Attendance |
| Primary users | Authorized source readers; photo readers subset |
| Permission boundary | attendance.view; attendance.photo.view intersection for private evidence; sensitive/history projections as authorized |
| Purpose | Inspect original events and current reviewed evaluation |
| Main UI regions | Occurrence start/end/date/timezone and historical context; six independent classification dimensions; original in/out evidence slots; correction/evaluation timeline; required/leave segments; next action |
| Layout | L4 |
| Primary action | Request correction / Review permitted issue |
| Secondary actions | Open authorized private photo; inspect previous evaluation; return to filtered list |
| Data requirements | Scoped event/evaluation/required-work DTO, original receipt times, private photo endpoint only if permitted, no storage path/raw GPS |
| Interaction states | loading/open/closed/missing-in/missing-out/inconsistent/conflicted/reviewed unknown/provisional/reconciled/photo unavailable |
| Responsive considerations | Image fits uncropped contain; dimensions/timeline stacked; no blurred unauthorized thumbnail |
| PRD references | ATT-002, ATT-006–ATT-012; DATA-006; WF-11; AC-19–AC-21, AC-30, AC-43 |

### 17.23 UI-ATT-004 — Request or initiate attendance correction

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-ATT-004 |
| Screen name | Request or initiate attendance correction |
| Module | Attendance |
| Primary users | Employee own requester; authorized HR attendance.resolve actor |
| Permission boundary | attendance.correction.request own target OR attendance.resolve authorized target; source data read separately |
| Purpose | Propose factual correction with reason without rewriting evidence |
| Main UI regions | Original/current context; proposed in/out or explicit unknown-end resolution; reason; work/leave/termination warnings; independent review explanation |
| Layout | L5 |
| Primary action | Submit correction request |
| Secondary actions | Cancel; revisit original record; revise stale proposal |
| Data requirements | Authorized original/effective values and permitted proposed fields, existing pending correction/version, eligibility/conflict preview |
| Interaction states | editing/already pending/invalid proposed value/stale/offboarded employee/no account/submitting/pending |
| Responsive considerations | Stack Original/Proposed; phone datetime fields separate; preserve safe draft on 422 |
| PRD references | ATT-008–ATT-009, ATT-011; EMP-006; WF-11; AC-20, AC-43 |

### 17.24 UI-ATT-005 — Correction review inbox and decision

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-ATT-005 |
| Screen name | Correction review inbox and decision |
| Module | Attendance |
| Primary users | Independent authorized reviewer; SA/HR defaults |
| Permission boundary | attendance.view ∩ attendance.correction.review on target; no submitter/affected self-review |
| Purpose | Decide one correction using original evidence and current context |
| Main UI regions | Pending queue; selected record original/proposed/current; permitted evidence; conflict/termination context; reason; ApprovalDecisionPanel; history |
| Layout | L3 inbox + L4 selected review |
| Primary action | Approve correction / Reject correction |
| Secondary actions | Refresh stale context; return to queue; open permitted HR resolution |
| Data requirements | Scoped correction, expected/current version, reviewer eligibility, original evidence fields, server consequence preview |
| Interaction states | loading/empty/self-ineligible/concurrent decision/stale/conflict/eligible/submitting/approved/rejected |
| Responsive considerations | Inbox→full detail phone; no decision in tiny table icon; evidence above final action |
| PRD references | AUTH-008; ATT-009–ATT-011; DATA-005; WF-11; AC-20, AC-30, AC-43 |

### 17.25 UI-ATT-006 — Reconciliation and owned exceptions

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-ATT-006 |
| Screen name | Reconciliation and owned exceptions |
| Module | Attendance |
| Primary users | SA/HR; authorized attendance operator |
| Permission boundary | attendance.view + attendance.reconcile/attendance.resolve for relevant command |
| Purpose | Resolve known incomplete/conflicted work without fabricated events |
| Main UI regions | Due/exception filters; owned HR cases including no-account/offboarded; independent dimensions; last evaluation; permitted reconciliation/resolution actions |
| Layout | L3/L4 |
| Primary action | Review owned issue / Reconcile an eligible occurrence |
| Secondary actions | Open source record/correction; retry eligible failed reconcile |
| Data requirements | Scoped due/evaluable state, unknown coverage, ownership, original period and approved resolution options |
| Interaction states | missing-in/missing-out/unassigned/ongoing/conflicted/termination-review/reviewed unresolved/processing |
| Responsive considerations | Phone exception list/detail; never hide missing evidence behind an all-green completion banner |
| PRD references | ATT-007, ATT-010–ATT-011; EMP-006; NTF-006; WF-06, WF-11; AC-19, AC-29–AC-30, AC-43 |

### 17.26 UI-ATT-007 — Attendance policy configuration

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-ATT-007 |
| Screen name | Attendance policy configuration |
| Module | Attendance |
| Primary users | SA/HR; explicit global policy manager |
| Permission boundary | attendance.policy.manage (G); authorized configuration references only |
| Purpose | Configure per-field inheritance and genuine location rules |
| Main UI regions | Company/location/shift layer and effective range; inherited/source labels per field; early/grace/latest/out settings; disabled/optional/required location; Standard/Strict and center/radius/accuracy; resolved preview |
| Layout | L5 |
| Primary action | Save policy revision |
| Secondary actions | Preview resolved values; revert unsaved edits; view source layer |
| Data requirements | Allowed configuration DTO, per-field unset/inherit versus disabled, configured geofence not captured employee coordinates, impact/version |
| Interaction states | inherited/explicit/invalid incomplete geofence/invalid dates/stale/previewing/committed |
| Responsive considerations | Grouped fields stacked; inheritance labels always adjacent; no employee-facing bypass switch |
| PRD references | ATT-004–ATT-006; OD-07–OD-09; WF-09–WF-10 policy context; AC-15, AC-17–AC-18 |

### 17.27 UI-LEAVE-001 — My leave workspace

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-LEAVE-001 |
| Screen name | My leave workspace |
| Module | Leave |
| Primary users | Linked own-target users as granted |
| Permission boundary | Separate leave.view / leave.balance.view / leave.request capabilities per panel |
| Purpose | Track own leave and eligible posted entitlement |
| Main UI regions | Own period/type balance summary when granted; request list/status/current stage; forecast separate; next permitted request/cancel action |
| Layout | L3/L2 compact |
| Primary action | Request leave if allowed |
| Secondary actions | Open request; request cancellation when eligible; view own ledger if granted |
| Data requirements | Separate authorized balance/request projections,2 decimal day units, period/expiry, safe route/action eligibility |
| Interaction states | loading/no requests/no balance grant/zero posted/forecast only/pending/blocked/cancel pending/error |
| Responsive considerations | Balances stack by type/year; compact request list uses table region/detail link |
| PRD references | LEAVE-002, LEAVE-004–LEAVE-006, LEAVE-010; WF-13, WF-16; AC-22, AC-44–AC-45 |

### 17.28 UI-LEAVE-002 — Leave request and shift-aware preview

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-LEAVE-002 |
| Screen name | Leave request and shift-aware preview |
| Module | Leave |
| Primary users | Own-target requester; no unapproved HR backdate shortcut |
| Permission boundary | leave.request (S); optional balance/reason/source details independently authorized |
| Purpose | Submit or explicitly resubmit an exact eligible request |
| Main UI regions | Type/dates/full-or-half; actual occurrence segments/break exclusions; per-start-year charge; posted funding result; route preview; reason; stale acceptance notice |
| Layout | L5 |
| Primary action | Submit leave / Accept revised calculation and resubmit |
| Secondary actions | Cancel; adjust dates; continue reviewing the draft calculation |
| Data requirements | Server calculation items/covered segments/exclusions/year-specific funding, valid first route, safe policy rules and expected version; forecast excluded |
| Interaction states | draft/calculating/no working items/unresolved/overlap/work conflict/insufficient posted/no first approver/stale/submitting/pending |
| Responsive considerations | Phone expand each item/year, never merge cross-year totals; submit follows full preview |
| PRD references | LEAVE-004–LEAVE-006, LEAVE-012–LEAVE-015; WF-13; AC-21–AC-22, AC-42, AC-44–AC-45 |

### 17.29 UI-LEAVE-003 — Leave request detail and history

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-LEAVE-003 |
| Screen name | Leave request detail and history |
| Module | Leave |
| Primary users | Current permitted requester/reviewer/HR reader |
| Permission boundary | leave.view target; leave.sensitive.view reason; approve/cancel/reassign action capabilities separately |
| Purpose | Understand current request, preserved calculation and staged decisions |
| Main UI regions | Request status vs cancellation status; exact dates/segments/type/period charge; balance allocation if permitted; ApprovalStepper; permitted reason; original/changed history; safe actions |
| Layout | L4 |
| Primary action | Act on current assigned stage or request cancellation if permitted |
| Secondary actions | Refresh stale context; view allowed ledger; return to filtered queue |
| Data requirements | Request/calculation/workflow snapshot, actual actors/current assignments, sensitivity mask, current eligibility, original lineage labels |
| Interaction states | draft/pending/blocked/approved/rejected/cancelled/cancel pending/stale/revoked |
| Responsive considerations | Vertical stepper; summary before decision panel; no unauthorized reason placeholder value |
| PRD references | LEAVE-007–LEAVE-013, LEAVE-015; WF-14–WF-16; AC-23–AC-26, AC-42, AC-46 |

### 17.30 UI-LEAVE-004 — Assigned approval inbox and stage review

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-LEAVE-004 |
| Screen name | Assigned approval inbox and stage review |
| Module | Leave |
| Primary users | Assigned eligible approver; SA/HR/M defaults |
| Permission boundary | leave.view ∩ leave.approve + current assignment/delegation/independence |
| Purpose | Make one ordered decision with current authority |
| Main UI regions | Assigned-only queue; scope; current stage; calculation and conflict context; prior actual actors; optional snapshot exception notice; ApprovalDecisionPanel |
| Layout | L3 inbox + L4 decision |
| Primary action | Approve stage / Approve leave at final stage / Reject request |
| Secondary actions | Refresh/review; authorized reassign for undecided stage; view history |
| Data requirements | Server stage number/final flag, current candidate eligibility/exclusion reasons, exact item/period reservation validation, request version |
| Interaction states | empty/unassigned/self/prior-actor conflict/delegate expired/blocked/stale/intermediate approved/final approved/rejected |
| Responsive considerations | Inbox to full review phone; no all-stages bulk approval or cascade button |
| PRD references | AUTH-008; LEAVE-007–LEAVE-009, LEAVE-011; WF-14–WF-15; AC-23–AC-24, AC-28, AC-42, AC-46 |

### 17.31 UI-LEAVE-005 — Cancellation and historical resolution review

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-LEAVE-005 |
| Screen name | Cancellation and historical resolution review |
| Module | Leave |
| Primary users | Independent authorized HR reviewer |
| Permission boundary | leave.view ∩ leave.cancel.review; further balance/conflict action grants as required by existing workflow |
| Purpose | Review cancellation/reversal without prematurely releasing leave |
| Main UI regions | Pending cancellation queue; original Approved leave; whole-future or authorized historical resolution kind; exact consumed-source portions; attendance/time conflict; returned-expired warning; reason |
| Layout | L3/L4 |
| Primary action | Approve cancellation / Reject cancellation |
| Secondary actions | Open original request/evidence; refresh; adjust only permitted resolution proposal |
| Data requirements | Server cancellable segments/units/original debit lineage/expiry outcome and independent reviewer eligibility; no invented refund sum |
| Interaction states | pending original still approved/not eligible/already decided/stale/eligible/committed reversal/expired return |
| Responsive considerations | Stack original and proposed effects; final action label remains visible after full evidence |
| PRD references | LEAVE-010, LEAVE-013; OD-12–OD-13; WF-16; AC-25–AC-26, AC-45 |

### 17.32 UI-LEAVE-006 — Balances and immutable ledger

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-LEAVE-006 |
| Screen name | Balances and immutable ledger |
| Module | Leave |
| Primary users | Permitted self/team/all balance reader; HR adjustment actor |
| Permission boundary | leave.balance.view target; leave.balance.adjust for signed posting; sensitive reasons separately masked |
| Purpose | Inspect period-specific entitlement and reasoned compensations |
| Main UI regions | Employee/type/year context; Posted/Reserved/Available; separate forecast/expiry; immutable ledger with kind/sign/4 decimal exact detail; retained carry/reversal links; optional adjustment form |
| Layout | L3/L4 |
| Primary action | View ledger / Post reasoned adjustment if allowed |
| Secondary actions | Filter period/type; inspect source linkage; open authorized request |
| Data requirements | Authorized fixed-point strings, posted/reserved/available from backend, source/receiving period/root lineage summaries, adjustment preview/version |
| Interaction states | loading/no entitlement/active reserves/expired/reversed/insufficient adjustment/stale/committed |
| Responsive considerations | Balance numbers stack; ledger numeric columns right and horizontal scroll; no inline editable ledger cells |
| PRD references | LEAVE-002–LEAVE-003, LEAVE-014–LEAVE-015; WF-12, WF-16; AC-22, AC-26–AC-27, AC-45 |

### 17.33 UI-LEAVE-007 — Leave types and effective policies

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-LEAVE-007 |
| Screen name | Leave types and effective policies |
| Module | Leave |
| Primary users | SA/HR; explicit global policy manager |
| Permission boundary | leave.policy.manage (G); authorized override/reference options |
| Purpose | Configure approved eligibility/grant/accrual/carry/expiry parameters |
| Main UI regions | Type/policy list; employee/location/company precedence; effective dates; annual grant OR monthly accrual; amount/proration; employment-type eligibility; explicit carry cap+expiry; workflow reference and source preview |
| Layout | L3+L5 |
| Primary action | Save policy revision |
| Secondary actions | Create/retire configured type; compare effective source; cancel |
| Data requirements | Allowed catalogs/effective versions, exact units, safe configuration reference DTO, material-impact validation; no assumed statutory amount |
| Interaction states | editing/inherit/configured/missing carry config/conflicting policy/stale/previewing/committed |
| Responsive considerations | Group economic fields and plain-language preview; no huge side-by-side matrix on phone |
| PRD references | LEAVE-001–LEAVE-003, LEAVE-012; OD-11–OD-12; WF-12; AC-27, AC-44–AC-45 |

### 17.34 UI-LEAVE-008 — Approval workflow configuration

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-LEAVE-008 |
| Screen name | Approval workflow configuration |
| Module | Leave |
| Primary users | SA/private explicit workflow configurator |
| Permission boundary | leave.workflow.manage (G); no implied HR intervention grant |
| Purpose | Configure ordered selectors/fallback and explicit independence exception |
| Main UI regions | Definition/revision; ordered stage rows; direct manager/named account/eligible role pool; configured root fallback/triggers; default distinct actual actors; explicit repeat-actor exception+reason; submitted-snapshot notice |
| Layout | L5 |
| Primary action | Save workflow revision |
| Secondary actions | Add/remove/reorder stage with buttons; preview allowed routing; cancel |
| Data requirements | Authorized selectors/reference options, stage order, configured fallback triggers/exception and safe validation, definition version |
| Interaction states | draft/invalid selector/no required steps/missing exception reason/stale/committed; existing request snapshot read-only |
| Responsive considerations | Vertical stage cards/list on all sizes; keyboard Move up/down, no drag-only ordering |
| PRD references | LEAVE-006–LEAVE-007, LEAVE-011; AUTH-008; OD-14; WF-13–WF-15; AC-42, AC-46 |

### 17.35 UI-LEAVE-009 — Dated delegation

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-LEAVE-009 |
| Screen name | Dated delegation |
| Module | Leave |
| Primary users | M own default; SA/HR scoped delegation managers |
| Permission boundary | leave.delegation.manage target; current existing read/approve authority, no new grant |
| Purpose | Route eligible undecided work for an explicit date range |
| Main UI regions | Current delegations; delegator/delegate authorized selection; inclusive company-local dates/timezone; reason; eligible-work/independence guidance; revoke action |
| Layout | L3+L5 |
| Primary action | Save delegation |
| Secondary actions | Revoke allowed delegation; open permitted pending work |
| Data requirements | Current eligible candidates, company timezone/date-to-UTC meaning, source authority, no-chain/self/prior-stage blockers, version |
| Interaction states | editing/no eligible delegate/self/chain/expired/revoked/stale/saved |
| Responsive considerations | Date fields stacked phone; eligibility explanation remains inline |
| PRD references | LEAVE-011; AUTH-008; WF-15; AC-28, AC-46 |

### 17.36 UI-LEAVE-010 — Entitlement processing

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-LEAVE-010 |
| Screen name | Entitlement processing |
| Module | Leave |
| Primary users | SA/HR; global authorized balance processor |
| Permission boundary | leave.balance.process (G); employee detail reads independently required |
| Purpose | Preview/run due retry-safe grant/accrual/carry/expiry |
| Main UI regions | Due cutoff/year/type; annual/monthly mode context; posted vs forecast explanation; safe batch preview and economic warnings; command progress/outcome; link to permitted detailed results |
| Layout | L7/L5 |
| Primary action | Process eligible due entitlement |
| Secondary actions | Refresh preview; retry known rolled-back/failed command as authorized; inspect allowed ledger |
| Data requirements | Due closed-period server calculations, safe processing status/effect outcome and allowed row projection; no future credit inputs |
| Interaction states | previewing/not due/blocked reserves/stale/processing/committed/failed/retry prior outcome |
| Responsive considerations | Single-column task summary phone; detailed rows only if authorized, not required for global command |
| PRD references | LEAVE-002–LEAVE-003, LEAVE-014; NTF-006; WF-12; AC-27, AC-44–AC-45 |

### 17.37 UI-DASH-001 — Overview dashboard

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-DASH-001 |
| Screen name | Overview dashboard |
| Module | Analytics |
| Primary users | SA/HR/M/E according to current grants |
| Permission boundary | dashboard.view ∩ relevant source reads and field sets |
| Purpose | See meaningful scope-relevant work and reporting coverage |
| Main UI regions | PageHeader/ScopeSummary; date/department/location/timezone filters; permitted KPI strip; attendance trend/late/absence; leave utilization by compatible year/type; workforce distribution; ReportBasisPanel; authorized next-work links |
| Layout | L2 |
| Primary action | Drill into an authorized measure |
| Secondary actions | Apply filters; Show data; request CSV when granted |
| Data requirements | Server metrics/series/units/numerators/denominators, source token, as-of/cutoff/freshness/coverage, authorized persona tasks; no unrestricted aggregate props |
| Interaction states | loading/partial permitted panels/provisional/reconciled/incomplete/N/A/stale-rebuilding/error |
| Responsive considerations | 1–2 KPI phone, one chart per row; full accessible values/table; no dashboard full of unrelated cards |
| PRD references | DASH-001–DASH-006; AUTH-004; WF-17; AC-30–AC-33, AC-41, AC-45 |

### 17.38 UI-DASH-002 — Report explorer and drill-down

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-DASH-002 |
| Screen name | Report explorer and drill-down |
| Module | Analytics |
| Primary users | Permitted source/report reader |
| Permission boundary | dashboard.view plus relevant source-read intersection; export additionally branch-specific |
| Purpose | Inspect the same coherent metric source behind a selected view |
| Main UI regions | Report kind tabs only as allowed; canonical filters/token/timezone/as-of/cutoff; basis/coverage; relevant chart and detail table; source consistency/stale message |
| Layout | L3/L4 |
| Primary action | Inspect permitted detail / Refresh changed report |
| Secondary actions | Show tabular values; export same context; return to originating view |
| Data requirements | Identical server query/token/metric/source metadata and scoped rows/counts; compatible year/type groups retained |
| Interaction states | loading/no applicable work/N/A/unknown coverage/provisional/reconciled/stale token/rebuilding/revoked |
| Responsive considerations | Filters Sheet; table scroll; comparison groups stack instead of merged incompatible chart |
| PRD references | DASH-002–DASH-009; DATA-010; WF-17–WF-18; AC-30–AC-34, AC-45 |

### 17.39 UI-DASH-003 — My CSV exports

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-DASH-003 |
| Screen name | My CSV exports |
| Module | Analytics |
| Primary users | Current authorized export initiator |
| Permission boundary | Owner AND current relevant read/export/field/scope/generation checks; no generic new permission |
| Purpose | Track queued coherent private reports and retrieve valid outputs |
| Main UI regions | Owner-only job list; ExportStatusPanel detail with filters/type/timezone/cutoff/source basis; state/retry/failure/expiry; protected Download |
| Layout | L3/L7 |
| Primary action | Download ready authorized CSV |
| Secondary actions | Retry/rebuild where server permits; return to report; refresh status |
| Data requirements | Safe owned job projection, real progress if known, ready artifact availability and current authorization outcome; no private key/public URL |
| Interaction states | queued/processing/ready/failed/cancelled/expired/stale report/access changed/partial unavailable |
| Responsive considerations | Phone bounded job summaries; no fake progress percent; 44px download/action target |
| PRD references | DASH-007–DASH-009; DATA-010; WF-18; AC-33–AC-35 |

### 17.40 UI-SHARED-001 — Notifications

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-SHARED-001 |
| Screen name | Notifications |
| Module | Shared |
| Primary users | All authenticated owners |
| Permission boundary | notification.view (O); notification.update (O) |
| Purpose | Read own safe work notifications and current targets |
| Main UI regions | Unread/All tabs; safe event/title/time; read state; mark read; current authorized target links |
| Layout | L7/list |
| Primary action | Open notification target |
| Secondary actions | Mark an owned item read; refresh |
| Data requirements | Only own notifications/unread count and safe template content; target permission checked again |
| Interaction states | loading/empty/unread/read/mark-read pending/link no longer available/error |
| Responsive considerations | Full-width list and 44px actions phone; no split tiny preview dependency |
| PRD references | NTF-002, NTF-005; AUTH-006; WF-01–WF-18 subsequent work; AC-06, AC-33, AC-35 |

### 17.41 UI-SHARED-002 — Audit records

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-SHARED-002 |
| Screen name | Audit records |
| Module | Shared |
| Primary users | SA/private explicitly authorized audit reader |
| Permission boundary | audit.view (G); redacted safe projection |
| Purpose | Inspect immutable material activity |
| Main UI regions | Allowed filters/date/action/actor; event table; safe before/after diff; effective/recorded distinction; reason/correlation where authorized |
| Layout | L3/L4 |
| Primary action | Open event detail |
| Secondary actions | Filter/clear; return to source only if permitted |
| Data requirements | Sanitized audit DTO with no credentials/hash/private bytes/raw GPS; no edit/delete action |
| Interaction states | loading/no matches/partial redaction/source unavailable/error |
| Responsive considerations | Mobile stacked labeled diff after table detail; no color-only change indicators |
| PRD references | SEC-005–SEC-006, SEC-010, SEC-012; WF-03–WF-16 evidence; AC-36 |

### 17.42 UI-SHARED-003 — Retention policies, holds and cleanup

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-SHARED-003 |
| Screen name | Retention policies, holds and cleanup |
| Module | Shared |
| Primary users | Private authorized retention operator |
| Permission boundary | retention.manage (G); current reauthentication; existing exact policy validation/approval |
| Purpose | Manage category-specific authority without age-only purge |
| Main UI regions | Category/version; candidate duration versus validation/approval status; material policy diff; hold/dispute controls; dependency preview; archive/purge distinction; execution/audit outcome; current recheck notice |
| Layout | L7/L5 |
| Primary action | Record existing validation/approve exact policy or Run eligible cleanup, as current state allows |
| Secondary actions | Suspend/withdraw; add/release authorized hold with reason; refresh preview; inspect safe outcome |
| Data requirements | Safe control/version/validation/hold/dependency DTOs and execution eligibility; underlying HR fields only if separately authorized |
| Interaction states | unapproved/pending validation/approved/suspended/held/blocked/stale preview/running/archived/purged/failed/public demo denied |
| Responsive considerations | Single-column risk summary and named sections; required warning/current policy never clipped in phone dialog |
| PRD references | SEC-007–SEC-009; DATA-006; OD-16, OD-21; AC-37 |

### 17.43 UI-SHARED-004 — Operations health and failures

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-SHARED-004 |
| Screen name | Operations health and failures |
| Module | Shared |
| Primary users | Private authorized operator |
| Permission boundary | operations.view; operations.retry for permitted task; domain action permission and reauth where required; demo.reset scheduler/operator only |
| Purpose | Observe durable background work and safe recovery evidence |
| Main UI regions | Protected dependency health; due/lag/failed-task list; publication vs completion and mail uncertain/exhausted state; backup/restore observations; safe incident detail; retry eligibility; restricted reset context |
| Layout | L7/L3 |
| Primary action | Retry authorized failed obligation when safe |
| Secondary actions | Inspect last success/restore evidence; refresh; authorized private reset only existing operator procedure |
| Data requirements | Sanitized health/attempt/checkpoint/control metadata; no arbitrary SQL/shell/secret/file path or private email content |
| Interaction states | healthy/degraded/queued/running/failed/exhausted/uncertain/quarantined/stale generation/public demo denied |
| Responsive considerations | Stack meaningful health rows; failures table scrolls; no new infrastructure dashboard framework |
| PRD references | NTF-003, NTF-006; NFR-011–NFR-012, NFR-015–NFR-016; OD-18, OD-20; AC-35, AC-38, AC-40 |

### 17.44 UI-SHARED-005 — About this public demo

| Attribute | Specification |
| --- | --- |
| Screen ID | UI-SHARED-005 |
| Screen name | About this public demo |
| Module | Shared |
| Primary users | Public synthetic HR/Manager/Employee visitors |
| Permission boundary | Explicit synthetic deployment projection, no role.view/manage/assign grant |
| Purpose | Understand demonstration limits and choose a documented demo account |
| Main UI regions | Synthetic-data disclosure; allowed persona account guidance; read-only permission comparison; simulated-photo/location explanation; restricted administration notice; nightly Asia/Jakarta reset/session message |
| Layout | L7 or L1 information panel |
| Primary action | Open sign in for documented demo account |
| Secondary actions | Return to app; inspect sanitized comparison; read simulated result explanation |
| Data requirements | Fixed safe synthetic persona guidance and explanatory permission DTO; no unrestricted SA credentials or production tokens |
| Interaction states | public guide/read-only comparison/simulation notice/reset maintenance/session invalidated |
| Responsive considerations | Readable phone headings/stacked comparison or named scroll region; never real camera/GPS permission prompt |
| PRD references | SEC-011; NFR-014; OD-20; AC-40, AC-47–AC-48 |

## 18. Status & State Language

Use explicit entity+dimension DTOs. Do not bind every raw database status string to a global badge or invent a transition because a color exists. Subtle badge uses the semantic foreground/subtle background; icon 16px,13/18px text, min 24px. A16px status icon is decorative when adjacent text already names it. Icons below are Lucide names or a closest reviewed equivalent in the pinned set; full wording is authoritative.

Separate **business state** (request Approved), **evaluation dimensions** (Present, Late, Clock-out missing), **freshness** (Provisional/Reconciled/Stale) and **operational processing** (Uploading/Processing/Ready). Business approval does not mean reconciled, a stage decision does not approve the whole request, and Ready is not authorization to download. Detail views show the independent labeled dimensions; tables show primary presence/status with material exception/completeness and freshness in its own column/context.

| State category | Legitimate entity / dimension | Display + accessible wording | Semantic family | Icon | Contextual behavior |
| --- | --- | --- | --- | --- | --- |
| Draft | Leave request/form | Draft | neutral | FileText | Continue editing; no reservation |
| Waiting | Future approval stage | Waiting | neutral | Circle | No decision action; previous stage must complete |
| Pending | Current leave/correction/cancellation decision | Pending approval / Pending correction / Pending cancellation | warning | Clock3 | Open permitted current review; original approved leave remains effective during cancellation |
| Blocked | Current approval route | Approval blocked | warning | CircleAlert | Authorized HR reassignment/configuration only; reservation held |
| Approved | Leave/correction/cancellation/policy where valid | Approved (name entity) | success | CircleCheck | View committed result/history; do not imply all other entities changed |
| Rejected | Reviewed request/proposal | Rejected | destructive | CircleX | Show permitted reason and eligible next action |
| Cancelled | Terminated request/job | Cancelled | neutral | CircleSlash | No further action on cancelled state; new intent only if allowed |
| Active | Account or employment | Account active / Employment active | success | CircleCheck | Keep state domains separate |
| Invited | Account | Invited | info | Mail | Authorized resend/revoke lifecycle only |
| Disabled | Account | Account disabled | neutral | LockKeyhole | Authorized reactivation separately; no clock-only grace |
| Planned | Employment | Planned | info | CalendarClock | Show effective start, not current work eligibility |
| Probation | Employment | Probation | info | UserRound | Work eligibility per current server policy |
| Ended | Employment period | Employment ended | neutral | Archive | Rehire/new period separately; no old period rewrite |
| Completed | Completed operational task or factual session | Completed / Session closed | success | CircleCheck | Name task; session closed only factual known out/correction |
| Incomplete | Attendance completeness | Clock-in missing / Clock-out missing / Inconsistent record | warning | CircleAlert | Show exact missing/inconsistent dimension and independent presence; correction path |
| Late | Punctuality | Late | warning | Clock3 | Server duration/basis; can coexist with Early departure |
| Early departure | Punctuality | Early departure | warning | LogOut | Separate server flag/duration; do not overwrite Late |
| Present | Presence | Present | success | UserCheck | Completeness/freshness remain independent |
| Absent | Finalized presence only | Absent | destructive | UserX | Only after valid finalization; unknown/unreconciled row says Awaiting reconciliation |
| Pending presence | Nonevaluable/unfinalized attendance | Awaiting reconciliation / Presence unknown | info | Clock3 | Show coverage/reason; do not label Absent or0% |
| On leave | Approved fully excused expectation | On leave | info | CalendarCheck | Not absent; partial leave shows covered segments plus remaining-work dimensions |
| Non-working | Resolved no work | Non-working | neutral | CalendarOff | No automatic absence; weekend alone is not this state |
| Unassigned | Missing schedule resolution | Unassigned schedule | warning | CalendarDays | Authorized schedule setup/review; not a day off |
| Conflicted | Unresolved work/schedule/leave evidence | Conflict needs review | warning | TriangleAlert | Open permitted explicit resolution; no silent finalize |
| Provisional | Current reporting evaluation | Provisional | info | Clock3 | Show evaluated time/coverage; not an approval state |
| Reconciled | Reporting/evaluation freshness | Reconciled | success | CheckCheck | Indicates evaluated version, not perfect completeness |
| Pending reconciliation | Revised source awaiting evaluation | Pending reconciliation | info | RefreshCw | Retain last coherent result visibly stale/incomplete as supplied |
| Stale | Report/calculation/source context | Needs refresh / Calculation changed | warning | RefreshCw | Refresh or requester re-preview/resubmit; no silent reprice |
| Rebuilding | Active fact/report refresh | Updating report | info | RefreshCw | One live status; do not animate falsely for completed request |
| Expired | Invitation/reset/artifact | Invitation expired / Reset link expired / Export expired | neutral | ClockAlert | Entity-specific resend/request/rebuild; no source-HR deletion implication |
| Queued | Export/notification/background obligation | Queued | info | ListTodo | Publication/processing not complete/delivered |
| Processing | Export/business job execution | Processing | info | LoaderCircle | Real progress only; no automatic success at 100% |
| Ready | Complete export artifact | Ready to download | success | FileCheck2 | Current owner/source/export permission check still required |
| Failed | Known terminal or retryable execution outcome | Export failed / Delivery failed / Processing failed | destructive | CircleX | Reason+safe Retry where allowed; do not repeat business effect |
| Uncertain | External provider ACK or unknown clock result | Delivery uncertain / Checking clock result | warning | CircleHelp | Review/recover existing intent; not blind send again |
| N/A | Zero/incompatible denominator | N/A — no eligible work / N/A — no entitlement basis | neutral | Minus | Show basis, not 0% or empty chart |
| Synthetic | Demo evidence dimension | Simulated attendance | info | FlaskConical | Always present on fixture result/evidence; does not establish real identity |

Never present “Active” without labeling account versus employment when both exist. Default table row prioritizes one business/presence label, then the material completeness/exception label; details expose all dimensions. “Pending approval”, “Pending cancellation” and “Pending reconciliation” have different purposes even if they share a visual family. Canonical job display uses PRD queued→processing→ready/failed/cancelled→expired; architecture adapter `running` maps to Processing, while stale/access/source is a separate outcome, not an invented new lifecycle. Approval blocked is an unresolved current stage, never a skipped completed stage.

## 19. Dashboard & Data Visualization Standards

Use Apache ECharts via the existing typed wrapper; do not adopt shadcn's separate chart engine or another chart library. CSS semantic chart tokens resolve to actual colors in the wrapper. Register only needed ESM chart/components/renderers, including AriaComponent for descriptions/decal support [T03]; configure options for the pinned ECharts version and verify actual assistive output. ARIA labels alone are not an interactive/table alternative.

Every chart has a short title, unit, timeframe/timezone/as-of/cutoff where relevant, server basis/coverage/freshness, persistent legend with labels, and a nearby “Show data” disclosure or authorized detail link. Legend toggling only changes display, not a newly calculated KPI. Tooltip shows allowed label/value/unit and basis; it is available through an equivalent keyboard-focusable data table/link. No essential action is possible only by clicking a canvas point. ECharts click emits server-issued report token+allowlisted dimension; Laravel validates current access/source before detail/export.

| Required insight | Selected chart / layout | Encoding / alternative |
| --- | --- | --- |
| Attendance trend | 2-series line, attendance and absence; height 280px desktop/240px phone | Semantic green/red, solid vs dashed with symbols, direct named legend; table date/rate/eligible basis/coverage. Never average day percentages in Vue. |
| Lateness | Vertical bar by period for server late count, with server late-rate text/basis | Amber; explicit count versus percent unit. Present eligible occurrence denominator is server-supplied. |
| Absence rate | KPI and matched trend/drill, not a competing formula/card palette | Red with label; excluded unknown/full-leave coverage visible. No finalized Absent label for unknown rows. |
| Annual leave utilization | Horizontal bars per compatible receiving year/type plus consumed/granted text | Indigo realized category; forecast/planned and expired units separately labeled, pending leave amber/diagonal only in its own explicitly non-realized context. No reserved/forecast in available or realized numerator. |
| Workforce distribution | Horizontal categorical bars with exact counts and as-of context | Stable chart 1–6 assignment by server category key; labels/table for every category. For many groups prefer scrollable chart+complete authorized table, not an invented client Other/topN sum. |
| Calendar attendance | Date grid with text/icon markers and per-date occurrence list | Distinguish presence/completeness/leave/nonwork/conflict; multiple occurrences on one date remain distinct. Phone agenda list is equivalent. |
| Leave periods / multiple series | Separate receiving-year/type groups or small multiples | Same units/compatible denominator only; no merged monthly entitlement assumption or pie suggesting utilization must sum 100%. |

Use 2px lines with 4–6px symbols, minimal grid, no3D, gradient fill, decorative donut, misleading truncated count bars or smoothed trend implying unobserved values. Rate axes 0–100 apply only to valid bounded attendance/late data; if server reports an integrity anomaly, show actual value and warning/table rather than clamp/clip it. Leave utilization may expose>100%/negative anomalies explicitly with an appropriate scale, not silently cap them. Missing observations are gaps/N/A, not interpolated zero. Plot labels use 12px minimum, muted-foreground; body/basis 13px. On narrow views reduce tick frequency, never drop the text data alternative or essential basis.

KPI strip contains only useful permitted measures for the actor. Up to4 cards per desktop row; no invented comparison arrows/sparklines if the backend has no comparable prior basis. All source metrics and display strings come from Laravel. Format only; never recompute r, sum rounded weights, merge incompatible leave years, infer attendance from browser clock or use two-decimal displayed leave in arithmetic. Golden display fixture values remain 60% attendance,40% absence,50% late and 1.5/20=7.5% leave utilization; different presentation precision does not change the underlying result.

## 20. Accessibility Standards

Target practical **WCAG 2.2 AA** behavior where applicable [A01]; this document does not certify conformance. Automated checks, keyboard/manual/screen-reader review and actual supported-browser/device tests remain necessary. Use the selected primitives as a starting point, not a guarantee after composition.

| Area | Project contract / verification |
| --- | --- |
| Text/meaningful graphics | Normal text≥4.5:1, large text≥3:1, meaningful control/graphical boundaries≥3:1 against adjacent color; ratios unrounded. Check all actual theme/state pairs. Decorative grid/divider can be lighter but cannot be sole control/state cue. |
| Keyboard | All actions reachable in logical DOM order; visible label/name; native buttons/links, no positive tabindex. Arrow semantics for tabs/listbox/calendar, Escape closes temporary layers, Return/Space as appropriate. No keyboard trap except expected modal containment. |
| Focus |2px ring +2px offset; focused target remains visible under sticky header/footer/drawer. Route change focuses h1/main or an appropriate restored target; close returns to trigger. Project ring styling is not a claim of additional formal certification. |
| Touch | Prefer≥44px for touch actions, especially capture/nav/submit; desktop active targets≥24px or legitimate WCAG spacing exception. Do not shrink mobile click targets to16px glyph size. |
| Forms | Real labels and fieldset/legend; helper/error describedby, aria-invalid when invalid; summary links to errors; do not identify required/error only by color. Preserve browser password-manager/autofill/paste behavior. |
| Dynamic status | Polite status for load/commit/read count; assertive blocking error only when new; no screen-reader flood each poll/timer. Progress indeterminate if no real percentage. |
| Dialogs/sheets | Named title/description, modal focus containment, safe initial focus, visible close and Escape/cancel where safe; no nested focus traps or click-outside-only dismissal. |
| Tables | Semantic table/caption/header scope and aria-sort; independent links/buttons; labeled scroll region keyboard accessible; authorized counts only; no whole-row click that blocks text selection. |
| Charts/calendar | Meaningful description and complete authorized text/table/list alternative; stable labeled patterns/icons; equivalent keyboard links for drill-down. Canvas hover is supplementary. |
| Reflow/zoom |320px viewport and 200% zoom: shell/forms/messages reflow without page-wide horizontal scroll; complex tables/calendar have named scroll regions. Test browser text-spacing overrides without clipping/overlap. |
| Motion/time | Respect reduced-motion; no blinking/auto-moving decorative content. Existing session/time windows remain backend rules; explain expiry/reauth/recovery without inventing an extension. |
| Evidence/privacy | Describe image purpose, not an invented identity claim. Unauthorized image not requested at all, no blurred real photo. Demo browser never requests camera/GPS. |
| Error comprehension | Name the problem and safe next action; distinguish denied, unknown, no-data and N/A; provide correction path for genuine device failure where authorized. |

Forms must support keyboard and screen reader, including activation/reset/password managers. Do not block paste or introduce puzzles/memory tasks. Preserve OD-20's initial client matrix: current/previous-stable Chrome, Firefox and Edge desktop; current Android Chrome and iOS Safari, verified at implementation/release. Test real genuine capture permission dialogs privately on these supported clients; deterministic Playwright mocks alone do not prove real camera support. Unsupported genuine capture has the documented correction path. Read-only demo simulation cannot replace that validation.

## 21. Public Demo UX

The isolated public demo is a complete-looking product using synthetic people and server-selected synthetic photo/location evidence. Use a compact info band, normal typography and one “About this demo” link, not a modal disclaimer on every click. Copy: **“Demo environment · Synthetic employee data. Attendance uses simulated photo and location evidence.”** Persist a “Simulated attendance” label in clock panel, committed fixture result/evidence and relevant reporting detail.

Sign-in guidance lists only approved constrained synthetic HR/Manager/Employee accounts as provided by deployment. No public Super Admin credentials. The explanatory permission comparison is read-only and visually uses the same table tokens; clicking a persona row does not change the signed-in role/grants. It needs no role.view/manage/assign grant. Privileged invitations/security/retention/operator/backup/reset controls are absent from navigation and backend rejects direct forged access. Explain “This action is available only in the private administrator demonstration” only in the explanatory guide/known restricted context, not as a disabled misleading admin app shell.

Public capture never calls getUserMedia/navigator.geolocation, never accepts visitor photo/GPS and never offers a production validation toggle or fixture chooser. The server chooses the fixture; unknown/client-forged fixture requests fail. Simulation preserves normal time/schedule/permission/leave/duplicate guards. Genuine non-demo capture has no “Use sample photo” fallback after camera/GPS failure.

Guide states “The demo refreshes nightly in Asia/Jakarta. Sessions and exports from the previous dataset expire.” Show an exact next reset only if supplied by server. During authorized maintenance, display a calm unavailable page; after reset “The demo has been refreshed. Sign in again.” Do not resurrect prior command/draft/file in the new generation. Outbound mail remains sandboxed with no visitor-selected destination/public token inbox. No reset button for public personas; night reset is existing operator/scheduler behavior, not a visitor workflow.

## 22. UI Content & Microcopy

Use professional English, sentence case, concrete nouns and action verbs. Page titles name the task/resource; buttons say “Create employee”, “Publish schedule”, “Submit leave” or “Approve stage”, not “Execute”, “OK”, “Proceed anyway” or “Submit data”. Labels are concise; help explains format/consequence; errors state problem+next action without blame. Name a timezone/year/unit when ambiguity matters. Never expose internal schema/lease/hashes/SQL/storage path details to ordinary HR/employee flows; a safe incident reference is enough for support.

| Context | Recommended content | Avoid |
| --- | --- | --- |
| Empty directory | “No employees match these filters. Clear filters to see your permitted records.” | “No employees exist” after a filtered/denied response |
| No account link | “Your account is not linked to an employee record. Contact HR if you need employee self-service.” |0.00 balance or inactive employment inferred from absent link |
| Field error | “Choose an effective date.” / “End time must follow the selected start.” | “Invalid input” with no field/action |
| Shift overnight | “Shift starting 31 Dec 2026 ·22:00–06:00 next day ·Asia/Jakarta” | Recording it as1 Jan solely because clock-out happened then |
| Upload pending | “Uploading photo…” → “Photo received. Checking attendance…” | “Attendance saved” before commit |
| Unknown clock result | “We could not confirm the result. Check this attempt before trying a new clock.” | “Failed—Clock in again” when commit is unknown |
| Clock committed | “Clock-in recorded at 09:02 on 23 Oct 2026 (Asia/Jakarta).” | Client-local current time presented as official evidence |
| Location | “Location check passed.” / “Location accuracy is insufficient. Retry or contact HR about a correction.” | “Identity verified by GPS” |
| Missing out | “Present ·Clock-out missing. A correction is required to establish a factual end.” | “Absent” or guessed shift-end timestamp |
| Termination intersection | “This open shift crosses the employment ending at 00:00. Review the boundary or arrange independent attendance resolution.” | “Access extended until you clock out” |
| Future forecast only | “No eligible posted entitlement is available for 2027. Expected accrual cannot fund this request.” | “Balance available soon—submit anyway” |
| Intermediate approval | “Stage 1 approved. The request is waiting for its next review.” | “Leave approved” before final atomic approval |
| Blocked approval | “No eligible independent approver is available for this stage. The reservation remains held.” | “Stage skipped” or automatic reuse of prior approver |
| Pending cancellation | “Cancellation requested. The approved leave remains effective until the cancellation is approved.” | “Balance restored” while cancellation is pending |
| Expired return | “Cancellation approved. Returned units have expired and are not available to request.” | “Available balance increased” without checking expiry |
| Stale proposal | “The schedule changed after this preview. Review the updated calculation before resubmitting.” | Silent replacement of dates/units/grants |
| N/A metric | “N/A — no eligible work in this period.” / “N/A — no entitlement basis.” |0% from missing denominator |
| Access changed | “Your access changed. This view can no longer be opened. Return to your workspace.” | Hidden employee name/detail in denial text |
| Export queued | “Your CSV export is queued. You can track it in My CSV exports.” | “Downloaded” or fake completed percentage |
| Export no longer valid | “This export is no longer available with your current access. Create a new export from a permitted view.” | A public link that bypasses revocation |
| Retention blocked | “Cleanup is blocked by a current hold or policy check. Refresh the preview before continuing.” | “Older than 90days, safe to delete” |
| Demo | “Simulated attendance ·Server-provided synthetic evidence. Your camera and location are not requested.” | Claim that fixture success proves genuine capture/geofence performance |

Do not blame the employee, claim legal compliance, promise exact queue completion time or describe a declined request as a system crash. A broad-role user with a blocked own approval sees the same separation-of-duty explanation. Confirmation text identifies the affected resource/dates/units and final action, while ordinary success stays short and persistent in context.

## 23. Frontend Implementation Conventions

Respect the architecture's existing names/versions/adapter families; this document does not upgrade packages. Use Vue SFC script setup, strict TypeScript, explicit props/emits and purpose-specific server DTOs. Pages orchestrate layout/feature composition. Domain business logic remains in Laravel services/workflows; Vue owns local form/capture/dialog/filter presentation state. No Pinia server-state clone, unrestricted local filtering, metrics/leave calculator or separate frontend backend.

Preserve the selected source families: Vue^3.5/TypeScript 5.x, Inertia adapters^3.0, Tailwind CSS 4.1.x, shadcn-vue source with Reka UI 2.x, TanStack Vue Table 8.x, ECharts 6.x, vee-validate 4.15.1 + @vee-validate/zod 4.15.1 + Zod≥3.24<4. Current docs are capability references; they do not authorize adopting Tailwind 4.3, VeeValidate 5 beta, Zod 4 or another chart adapter. Resolve compatible exact patches/lockfiles during the implementation process already specified by the architecture.

| Location | Ownership / rule |
| --- | --- |
| `resources/js/pages/{identity,workforce,scheduling,attendance,leave,analytics}/` | Inertia page composition and authorized page props; no duplicated domain formulas |
| `resources/js/features/{identity,workforce,scheduling,attendance,leave,analytics}/` | Domain-specific forms/clock/approval/calendar/report patterns; keep module ownership |
| `resources/js/components/ui/` | Reviewed shadcn-vue primitives; style/behavior customization once, no one-off competing primitives |
| `resources/js/components/forms/` | Field/error/confirmation composition shared by real repeated forms |
| `resources/js/components/tables/` | AdvancedDataTable/toolbar/manual server-query helpers |
| `resources/js/components/charts/` | Typed ECharts lifecycle/theme/accessibility wrapper; not business calculator |
| `resources/js/layouts/` | L1–L7 shared shells/composition, responsive navigation and focus regions |
| `resources/js/composables/` | Existing useAuthorizedTable/useClockSubmission/useCaptureSession/useReportFilters equivalents; typed local behavior |
| `resources/js/types/` | DTO/field/capability/metric/context types; restricted field absent, not a required property carrying hidden data |
| `resources/js/lib/` | Formatting/class/token helpers; no duplicate source authority |

Shared supporting pages fit their owning feature/shared composition under this structure; do not create a conflicting top-level UI architecture or a seventh business-module directory solely for this document. Inspect actual existing files and aliases before choosing the final file path; names here are contracts, not a scaffold instruction.

CSS uses global semantic variables and a single Tailwind 4 theme bridge. Tokens above map to shadcn names plus custom `--color-*` aliases. Use utility classes for layout/spacing/responsive/state, component variants for coherent states, and scoped styles only for genuine layout/renderer needs (calendar/chart/capture), consuming the same tokens. No arbitrary hex/font/shadow/radius in a feature, inline per-page palette, legacy Tailwind 3 HSL-channel wrapper or separate tailwind.config theme source. Do not copy React registry code.

Illustrative token bridge fragment only (not a full stylesheet or emitted implementation):

```css
:root {
  --background: #F8FAFC;
  --foreground: #0F172A;
  --primary: #4338CA;
  --primary-foreground: #FFFFFF;
  --radius: 8px;
}
@theme inline {
  --color-background: var(--background);
  --color-foreground: var(--foreground);
  --color-primary: var(--primary);
  --color-primary-foreground: var(--primary-foreground);
}
```

Apply the same bridge to every listed color token; font/typography/radius/shadow/z-index namespaces are registered once with their chosen values. Use base→sm/md/lg/xl/2xl responsive utility order and feature layout containers rather than scattered pixel queries. ECharts receives resolved token values through its wrapper; pen.dev variable naming maps mechanically to semantic CSS tokens. Theme customization must preserve reviewed primitive interaction/focus and update design references when materially changed; do not rewrite installed primitives per screen.

Typed server presentation contracts include: current capabilities/allowed destinations/field descriptors; scoped rows/count or has_more; expected version; local occurrence date/timezone and independent classification flags; decimal/raw+formatted values and unit; stage number/final flag/assignment eligibility; command intent recovery state/result; report token/source basis/coverage/cutoff/freshness; current private artifact availability. Precision-bearing economic display strings are rounded by the backend; Vue may format labels/grouping, not run binary-float rounding or arithmetic on displayed leave. No raw ORM model serialization, source permission expansion, client-computed lateness/available balance or authority inferred from role name.

Submit via existing Inertia/router/forms and small same-origin clock/status endpoints described in architecture, not a new API application. Cleanup camera tracks/object URLs/listeners/ResizeObserver/ECharts instances on unmount; do not keep private previews after navigation or reset. Scope change revokes affected rows/fields/cache/view actions on next server check; a private thumbnail is not fetched without permission. No data in hidden DOM, tooltips, JSON props, chart tooltip, email or CSV exceeds the authorized projection.

## 24. pen.dev Design File Organization

Recommended initial asset, to be authored later: **`design/phase-1/professional-hris.pen`**. Use one file for this solo-developer MVP to keep document variables, reusable components and 44 screen references together. Do not create a design file per page/table or depend on unverified cross-file library sync. Group top-level frames/regions with the following names; these are project naming conventions, not a claim that pen.dev has Figma-style pages/variant sets.

| Group prefix | Contents |
| --- | --- |
| `00 Foundations` | Color variables/swatches, type scale, spacing/radius/elevation, icons and semantic state legend |
| `01 Components` | Reviewed reusable foundation/application patterns and important state variants, with token bindings |
| `02 Shell` | Expanded/rail/drawer shells, PageHeader, breadcrumbs, mobile/filter/modal patterns |
| `03 Identity` | UI-AUTH references |
| `04 Workforce` | UI-EMP references |
| `05 Scheduling` | UI-SHIFT references |
| `06 Attendance` | UI-ATT references; private genuine vs explicitly synthetic states clearly named |
| `07 Leave` | UI-LEAVE references; funding/approval/cancellation variants |
| `08 Analytics` | UI-DASH references; basis/coverage/freshness and same-token detail |
| `09 Shared` | UI-SHARED reference views and controlled operations/demo notices |

Naming contracts:

| Object | Name convention / example |
| --- | --- |
| Semantic variable | `color/primary` ↔ CSS `--primary`; `space/6` ↔24px; `radius/md` ↔6px; `type/body` ↔14/20/400. Use typed number/color/string values the tool supports; typography can be named style/reference annotation if not one native object. |
| Reusable component | `Component/Button/Primary/Default`, `Component/StatusBadge/Warning`, `Pattern/ApprovalDecisionPanel/Blocked` |
| Screen frame | `UI-LEAVE-004 / laptop-1280 / pending / team / r01` |
| State variant | Stable readable names: `loading`, `empty`, `validation-error`, `permission-changed`, `stale`, `blocked`, `committed`, `unknown-result`; entity-specific states from §18 |
| Evidence mode | `private-genuine` or `public-synthetic`; not a user-selectable toggle |
| Reference viewport | `phone-390`, `tablet-768`, `laptop-1280`, `desktop-1440`; verify 320px/reflow and required intermediate boundaries in implementation QA |
| Approval annotation | Within the file/frame note: screen ID, revision, state Draft/In review/Approved/Superseded, owner decision/date, source IDs, responsive/state coverage, approved commit/ref and intentional differences |

Frames are visual references at selected sizes, not fixed CSS widths/heights for every viewport. Do not hardcode canvas absolute coordinates into Vue layout. Reuse component instances/variables supported by the actual editor; exact variant names above are our convention, not a promise of a native variant API. Keep IDs/references intact through editor/MCP operations. `.pen` is JSON-based and Git-friendly [P01], but manual JSON conflict edits can corrupt references; save/close before branch change, preserve conflicting versions, reapply changes in the editor and inspect affected frames. Keep later fonts/legitimate assets at stable relative repository paths. No real HR data, photo/GPS, credentials or token is allowed in design fixtures.

Use editor/MCP/CLI capabilities actually present in the installed edition [P02–P04]. The verified docs describe document variables/reusable instances, an external MCP connection to a running host, and agent-assisted code workflows; they do not establish automatic CSS synchronization, production-ready Vue generation or universally live-linked libraries. Current import/export docs include visual/HTML routes, but exported markup is reference material, not our application architecture. Recheck installed version/tool availability before future design execution; no `.pen` file or export is generated now.

## 25. pen.dev → Codex Handoff Workflow

### 25.1 Sequence and representative views

1. Read current approved sources and this approved design system; reconcile actual installed primitives and tokens. Product behavior outranks all canvas edits.
2. Populate semantic variables/type/spacing and 26 foundation controls; develop only the shared patterns needed by the first views, not 30 unused components at once.
3. Design Shell expanded/rail/drawer, headers/toolbars/empty/error/focus and layout L1–L7 references.
4. Design representative flow in this order: UI-AUTH-001 Sign in → UI-DASH-001 HR overview → UI-EMP-001 directory → UI-EMP-002 profile/history → UI-SHIFT-001 schedule → UI-ATT-001 private capture and synthetic variant → UI-LEAVE-002 request → UI-LEAVE-004 assigned review → UI-DASH-002 report/drill. Use synthetic, permission-valid fixtures.
5. Review token/component consistency, current-scope/field omission, actual workflow states and phone variants before filling the remaining views. Include high-risk offboard/protected history/forecast-only/blocked-root/unknown-clock cases, not just happy-path empty dashboards.
6. Owner reviews identified screen revision and important states; record Draft→In review→Approved, with previous approved frame Superseded only after a reviewed replacement. A designer/agent never infers approval from a generated preview.
7. Pass the concrete screen contract below to a bounded future implementation task, with source/architecture/visual references and permitted scope. No TASK.md or task list is created here.
8. Codex inspects current components/layouts/types, maps reuse, implements through Vue/Inertia/backend contracts, validates business/accessibility behavior and compares against the exact approved reference.
9. Review intentional deviations (content expansion, keyboard behavior, browser text rendering or corrected source meaning). Update design/implementation reference together when materially accepted; do not silently treat an implementation accident as new design authority.

### 25.2 Lightweight approval/version rules

**Draft:** editable proposal, not implementation authority. **In review:** identified revision and viewport/state list ready for owner. **Approved:** explicit owner decision bound to file/frame revision and saved Git reference where available. **Superseded:** retained old reference with replacement identified; never delete historical approval context casually. This is a single-owner review record, not a new committee or product approval workflow.

One screen approval covers only the named responsive/state variants. It cannot waive missing backend permission tests or approve different economics, stage routing, retention policy, legal compliance or real-data use. If a visual proposal would change product behavior, identify the source conflict and resolve through the existing product process before adopting it; no forced confirmation is needed for ordinary reversible implementation details already covered by approved scope.

### 25.3 Required Codex handoff record

Record these values in the future bounded task/context and the `.pen` frame annotation; do not create another handoff document now.

| Required field | Concrete content |
| --- | --- |
| Screen identity | One of the 44 UI IDs plus named state/target/evidence-mode variants |
| Design source | `design/phase-1/professional-hris.pen`, exact frame path/node reference if provided by tools, approved local revision and saved Git commit/version |
| Approval evidence | Owner decision/date and covered viewport/state list; currently no approved `.pen` reference is asserted by this document |
| Product trace | Exact existing PRD requirement IDs, WF and relevant AC; no invented product requirement |
| Engineering trace | Architecture 8,11,12 and relevant module/clock/leave/report/export sections/ADRs; supporting schema projection version |
| Reuse inventory | Existing component filenames/variants inspected; which of the 26 foundations/30 patterns are reused versus a justified new composition |
| Layout | L1–L7 pattern, actual content max/columns/gutters/sidebar/header and below-fold/scroll behavior, phone adaptation |
| Tokens | Semantic names/type/spacing/radius/elevation/icon sizes; no independent local palette |
| Data contract | Typed authorized fields, nullable/missing meaning, display precision/units/timezone, scope/count/fieldmask and current capability props |
| Action contract | Existing permission names/target sets, version/intent/recovery outcome, server validation and committed-result semantics |
| States | Initial loading/valid empty/filtered empty/error/denied/stale plus entity-specific blocked/unknown/incomplete/N/A/reset cases |
| Accessibility | Heading/labels, keyboard path, focus initial/restoration/scroll, live announcements, chart/table alternatives, touch/reduced-motion |
| Visual acceptance | The applicable VQA labels below, fixed viewport/fixture/fonts and meaningful structural tolerances; intentional differences explicitly recorded |
| Validation | Required existing Pest/MySQL/Playwright/security/business tests plus deterministic visual comparison; design similarity alone is insufficient |

External pen.dev MCP connection requires its running desktop/IDE host and enabled client as documented [P03]; use inspected current tools, not guessed method names. Generated HTML/React/inline absolute styles must be translated to maintainable Vue/shadcn-vue semantic components and current DTOs. Do not paste unrestricted model JSON, replace the authorizer or compute leave/KPIs in Vue to make a screenshot match. No live cross-file sync/export/plugin feature is required for this chosen single-file handoff.

## 26. Visual QA & UI Acceptance Criteria

VQA labels are design verification references, not new business requirement IDs. They complement PRD AC-01–AC-48 and existing QA gates. A visually matching but unauthorized/incorrect workflow is a failed implementation.

| ID | Area | Observable acceptance | Product trace |
| --- | --- | --- | --- |
| VQA-01 | Token integrity | No independent feature hex/font/radius/shadow; actual computed theme values match §5–§8 and reviewed frame. | NFR-009, NFR-013; Architecture 11 |
| VQA-02 | Structure and alignment | Correct authorized shell/header/toolbar/sections, primary action and content width at 390/768/1280/1440; repeated edges follow 4px rhythm. | QA-003; NFR-009 |
| VQA-03 | Typography/data readability | Inter loaded or reviewed fallback, size/weight/line-height hierarchy; names/units/errors not clipped; tabular measures and 2/4 decimal day strings correct. | LEAVE-002; NFR-009 |
| VQA-04 | Contrast/focus | Actual text/control/meaningful graphic pairs pass target ratios; visible unobscured focus in all tested states, no opacity-only errors. | NFR-009; AC-41 |
| VQA-05 | Keyboard/dialog behavior | Navigate shell/tabs/table/calendar/forms/decisions with keyboard, open/close/restore focus correctly; Escape and no nested focus trap. | NFR-009; QA-003; AC-41 |
| VQA-06 | Responsive/reflow | 320px and 200% zoom usable; no page overflow except named table/calendar regions; 44px touch capture/nav; sticky bars do not cover fields/errors. | NFR-009–NFR-010; AC-41 |
| VQA-07 | Table server behavior | 25default/100max, allowlisted sort/filter/pagination/search, authorized totals/columns and URL Back; no all-record browser filtering or hover-only actions. | DASH-009; NFR-007; AUTH-004 |
| VQA-08 | Form error classes | Client/server field error, business rejection,403/404,409, transient rollback and unknown result have distinct copy/recovery; preserve safe input, no silent overwrite. | DATA-004–DATA-008; QA-002–QA-003 |
| VQA-09 | Current authorization | Multi-role exact scope unions/intersections; hidden fields absent from props/DOM/tooltips/CSV/photo requests; direct target and changed hierarchy do not leak. | AUTH-003–AUTH-006; AC-04–AC-06, AC-33–AC-34 |
| VQA-10 | Temporal meaning | Employment type/lifecycle/account separate; historical context/timezone/overnight start date retained; offboard unchanged boundary no grace/fabricated out; rehire old period preserved. | EMP-002–EMP-006; SHIFT-007; AC-08–AC-09, AC-16, AC-43 |
| VQA-11 | Attendance commit/recovery | Unsent/upload/validation/known rejection/unknown/committed states visually distinct; same intent recovery, retake new; persistent original server result only after commit. | ATT-001–ATT-005, ATT-013; AC-13–AC-18 |
| VQA-12 | Evaluation dimensions | Late+early departure can coexist; present+missing-out stays present/incomplete; unknown/unassigned/conflicted not finalized absence; exact reviewed unknown end visible. | ATT-006–ATT-010; PRD 14.1; AC-19–AC-21, AC-43 |
| VQA-13 | Leave precision/funding | Posted/reserved/available/year and forecast clearly separate; first/second halves exclude breaks; cross-year all-or-none preview and expired return copy; immutable ledger. | LEAVE-002–LEAVE-006, LEAVE-010, LEAVE-014; AC-21–AC-22, AC-26–AC-27, AC-44–AC-45 |
| VQA-14 | Independent ordered decisions | Assigned/current stage only; requester/prior-actor/delegate blockers; explicit snapshotted exception display; root fallback/blocked stage retains meaning; no cascade or stage 1=Leave approved toast. | AUTH-008; LEAVE-007–LEAVE-011; AC-20, AC-23–AC-28, AC-42, AC-46 |
| VQA-15 | Charts and metric truth | Typed series/basis/cutoff/freshness/N/A/coverage; 60/40/50 and 7.5 fixture matches detail/CSV; labeled patterns, complete Show data and keyboard drill. | DASH-002–DASH-006; AC-30–AC-32, AC-41, AC-45 |
| VQA-16 | Export privacy/coherence | All CSV queued; owner-only status and ready authorization, same source token, stale/revoked/expired/partial safely unavailable; no fake progress/public URL. | DASH-007–DASH-009; DATA-010; AC-33–AC-34 |
| VQA-17 | Shared controls | Owner notification center/read only; read-only audit; retention exact version/hold/execution blockers and archive≠purge; failures/mail uncertainty not business rollback. | NTF-001–NTF-006; SEC-005–SEC-009, SEC-012; AC-35–AC-38 |
| VQA-18 | Public/genuine separation | Synthetic labels, sanitized read-only role comparison, backend denied privileged writes, zero visitor camera/GPS calls; real environment forged fixture/capture fallback rejected. | SEC-011; OD-20; AC-40, AC-47–AC-48 |
| VQA-19 | Reduced motion and loading | No motion-dependent meaning; reduced-motion removes transforms/pulse/chart entrance; empty/error not an endless skeleton; one useful status announcement. | NFR-009; QA-003 |
| VQA-20 | Approved-reference fidelity and gates | Exact approved frame/revision used; purposeful deviations documented; relevant business/accessibility tests pass; no benchmark/compliance claim from screenshots. | QA-004–QA-008; REL-001–REL-008; AC-39, AC-41 |

Compare implementation and approved pen.dev frame at fixed viewport, fonts, language, timezone, authorized synthetic fixture and explicit state. First compare region hierarchy, actions/content/field omission and responsive behavior; then spacing/type/control size/color/borders. Repeated alignment/spacing should follow the specified token values, not ad hoc “looks close”. Allow normal subpixel font antialiasing/browser-native picker/camera differences; investigate meaningful geometry/wrapping or missing controls. Do not distort accessible text/layout to chase pixel identity.

For stable views, use deterministic Playwright screenshot comparison with fixed clock/fixtures/fonts and reduced motion; mask only truly nondeterministic safe metadata, never private-field leaks or workflow states. Baseline changes require review and the approved design reference, not blanket snapshot updates after failures. Test keyboard/focus/forms/permissions separately; a screenshot cannot prove server-side scope, clock commit, ledger correctness, file privacy, current export revocation, benchmark targets or legal validation. Private real-device capture and release benchmark/restore exercises remain their source gates. No screenshot is generated in this specification task.

## 27. Design Decision Register

DSD entries are approved design approaches, not new Operational Decisions or product requirements. They bind token/component/screen design consistently until a reviewed replacement is recorded.

| Decision | Question | Selected approach | Alternatives considered | Rationale | UX impact | Implementation impact | Source references |
| --- | --- | --- | --- | --- | --- | --- | --- |
| DSD-001 | Visual identity | Light enterprise workspace, indigo/slate, subtle borders, functional density | Decorative dashboard kit; dark-first; cloned commercial identity | Calm precision suitable for HR without ornamental clutter | Clear hierarchy and reliable operational tone | One theme/tokens; no dark toggle/extra assets | NFR-009, NFR-013; Architecture 11,26 |
| DSD-002 | Color semantics | Complete semantic light CSS variables, explicit foreground/subtle/border pairs; separate chart metrics/status | Raw page hexes; one status palette per module | Consistent action/focus and accessible state meaning | No color-only state, selected vs success separate | Tailwind 4 @theme inline + shadcn token bridge once | NFR-009; AUTH-004; Architecture 11 |
| DSD-003 | Typography | Self-hosted Inter 400/500/600; 14px body/table; 24px page; 16px mobile inputs | Multiple display fonts; tiny dense cells; oversized KPI type | Maintainable readable hierarchy and stable numeric alignment | Scannable data and legible mobile forms | One font stack/type scale and tabular numeral convention | NFR-009–NFR-010; Architecture 11 |
| DSD-004 | Shell/layout | One capability-aware shell; 240/72sidebar, drawer<1024, L1–L7 compositions | Four role apps; one card grid for every screen | Shared navigation and reusable task-appropriate layouts | Personal and admin activities clear without role switching | Existing layouts/pages/features; no extra app shell framework | AUTH-003; DASH-001; Architecture 8,11; ADR-014 |
| DSD-005 | Responsive behavior | Mobile agenda and stacked forms; named table scrolling; 44px touch targets | Desktop canvas shrink; rewrite every table as custom cards | Preserves information/interaction with minimal maintenance | Self-service capture/request accessible on phone, admin remains usable | Tailwind breakpoints + a small set of actual-container thresholds | NFR-009–NFR-010; AC-41; Architecture 11 |
| DSD-006 | Tables | One 48px row convention, server TanStack queries 25/100, explicit toolbar/actions | All rows local; hover-only actions; multiple density engines | Correct access/counts and predictable review | Stable sort/filter/Back and clear row identity | Reuse one typed wrapper; no generic unapproved bulk actions | DASH-009; NFR-007; AUTH-004; Architecture 11 |
| DSD-007 | Forms | VeeValidate+Zod 3 input shape, Laravel authority; distinct error/recovery classes | Two form stores; browser business calculator; generic error toast | Preserves drafts without hiding current business rejection | Field guidance and meaningful stale/high-impact review | Same Inertia adapters/current versions; no new approval workflow | DATA-004–DATA-008; QA-002–QA-003; Architecture 11 |
| DSD-008 | State language | Entity+independent dimension labels; committed/stage/freshness/processing separated | Single green/red status; raw schema enum shown everywhere | Avoids false absence/approval/success certainty | Present missing-out and stale approved leave understood | Typed server projection; accepted Schema §11.5 late/early and inconsistent completeness contract | ATT-006–ATT-010; DASH-005; PRD 14.1; Architecture 15,20 |
| DSD-009 | Charts | Purposeful ECharts lines/bars/calendar with basis/coverage/Show data; one backend metric | Decorative donuts; frontend averages; chart-only hover drill | Truthful exact ratios and accessible analysis | N/A/unknown/planned versus realized explicit | Typed wrapper/resolved tokens/ARIA module; same token to drill/CSV | DASH-002–DASH-008; AC-30–AC-34; ADR-011–ADR-012 |
| DSD-010 | Role-aware IA | Server capability union destinations, exact per-view/field intersections; no role switch | Hardcoded SuperAdmin/HR layouts; broadest role scope globally | Preserves dynamic grants/custom roles and current-team revocation | Relevant navigation without fake inaccessible admin links | Laravel authorizer/page props; G/O not employee data shortcuts | AUTH-002–AUTH-008; OD-01, OD-14; ADR-003 |
| DSD-011 | Attendance UX | Focused genuine capture + explicit server-synthetic variant, same-intent lost-response recovery | Save on upload; browser timestamp; sample fallback after capture failure | Protects evidence/commit meaning and public privacy | Clear camera/GPS failures and factual correction path | Existing capture/clock composables; no public browser evidence calls | ATT-001–ATT-005, ATT-012–ATT-013; OD-03, OD-08–OD-10, OD-20 |
| DSD-012 | pen.dev handoff | One grouped .pen file; semantic variables/reusable instances; owner-approved frame/rev+typed contract | File per table; raw generated markup; assumed automatic sync | Concrete reproducible reference with low solo-maintenance cost | Consistent responsive/state review rather than static happy path | Git/editor/MCP supported workflow; inspect reuse, Vue composition and visual/business QA | QA-003, QA-006–QA-007; Architecture 26; ADR-014 |
| DSD-013 | Leave quantity presentation | Period/type-specific Posted/Reserved/Available and distinct forecast; 2 decimal summary/4 decimal ledger | One editable balance; borrowed forecast; combined-year surplus | Preserves exact economics without exposing implementation ticks | User sees why a future/cross-year request is unfunded | Server formatted strings/item preview; immutable ledger display | LEAVE-002–LEAVE-006, LEAVE-010, LEAVE-014; OD-11–OD-13; ADR-010 |
| DSD-014 | Historical/privacy/control clarity | Current scope separate from historical context; safe evidence/retention/demo/operations projections | Blurring hidden fields; age-only purge; fixture-as-real proof | Trustful operational information without scope/approval confusion | Clear real-data/synthetic and archived/purged/held meaning | Current backend checks, omitted fields, private endpoints and existing control gates | EMP-002–EMP-006; SEC-003–SEC-012; OD-16, OD-18, OD-20–OD-21; ADR-004, ADR-013, ADR-015 |

## 28. Design-to-PRD Traceability Matrix

Coverage here means a visual/interaction contract exists, not that a database constraint, backend workflow, security guarantee, performance target or release gate has been implemented. UI IDs and component/VQA references below point to this file; original PRD requirement/WF/AC identifiers remain unchanged.

| Existing requirement IDs | Screens / views | Components / contract | Visible or interaction coverage | Verification boundary |
| --- | --- | --- | --- | --- |
| AUTH-001 | UI-AUTH-001–UI-AUTH-006 | Form foundations, ErrorState | Invitation/activation/login/reset/disable independent from employment; exact server errors | VQA-08, VQA-10; backend AC-01–AC-03 |
| AUTH-002 | UI-AUTH-007; UI-SHARED-005 | AdvancedDataTable, ScopeSummary | Dynamic exact grant matrix privately; public sanitized comparison only | VQA-09, VQA-18; AC-40 |
| AUTH-003 | All authorized destinations; UI-AUTH-007 | ScopeSummary | Same-permission role union and required-set intersection; no role switch/inheritance | VQA-09; AC-04–AC-05 |
| AUTH-004 | All 44 major views | AdvancedDataTable, PermissionDeniedState | Authorized fields/rows/counts/props/links/files only; UI hiding never security | VQA-07, VQA-09, VQA-16; AC-33 |
| AUTH-005 | UI-AUTH-006–UI-AUTH-007 | ApprovalDecisionPanel-style consequence review, InlineAlert | Explicit access diff, reauth and server final-admin/self-escalation rejection | VQA-08–VQA-09; backend guard tests |
| AUTH-006 | All scoped views; UI-DASH-003; UI-SHARED-001 | ScopeSummary, PermissionDeniedState | Current graph/grants at subsequent reads and exports, not old manager access | VQA-09, VQA-16; AC-06, AC-34 |
| AUTH-007 | UI-AUTH-002, UI-AUTH-006; UI-EMP-003 | EmployeeProfileSummary | Optional unique employee account, invite state and retry outcomes | VQA-08; backend AC-01–AC-02 |
| AUTH-008 | UI-ATT-005; UI-LEAVE-004–UI-LEAVE-005, UI-LEAVE-008–UI-LEAVE-009 | ApprovalStepper, ApprovalDecisionPanel | Current assignment/read/approve, requester and earlier-actor independence; explicit snapshot exception only | VQA-14; AC-20, AC-23, AC-28, AC-42, AC-46 |
| ORG-001 | UI-EMP-007 | OrganizationHierarchyViewer | Company/departments/position/level/location as reference workspace, not pages per table | VQA-02, VQA-09; AC-07 |
| ORG-002 | UI-EMP-004, UI-EMP-007 | OrganizationHierarchyViewer, ActivityTimeline | Separate effective reporting graph, current team versus old context | VQA-10; AC-06–AC-08 |
| ORG-003 | UI-EMP-007 | TemporalImpactPreview | Retire referenced catalog safely; visible in-use blocker, no destructive shortcut | VQA-08; backend history tests |
| ORG-004 | UI-EMP-007; UI-SHIFT-001; UI-ATT-007 | DetailSummary, DatePicker | Location IANA timezone and attendance-policy context clearly labeled | VQA-10–VQA-11; AC-15–AC-17 |
| EMP-001 | UI-EMP-001, UI-EMP-003 | EmployeeProfileSummary | Employee creation without required login | VQA-09–VQA-10; AC-01 |
| EMP-002 | UI-EMP-002, UI-EMP-004–UI-EMP-006 | ActivityTimeline, TemporalImpactPreview | Dated hire/promotion/transfer/reporting/renewal/offboard/rehire | VQA-09–VQA-10; AC-07–AC-09 |
| EMP-003 | UI-EMP-002; UI-AUTH-006 | EmployeeProfileSummary | Type, lifecycle and account state independently labeled | VQA-09–VQA-10; AC-09, AC-43 |
| EMP-004 | UI-EMP-006 | ActivityTimeline | New employment period, same identity/old unresolved session | VQA-09–VQA-10; AC-09, AC-43 |
| EMP-005 | UI-EMP-003–UI-EMP-007 | TemporalImpactPreview | Date/cycle/overlap/stale errors without overwrite | VQA-09–VQA-10; AC-07 |
| EMP-006 | UI-EMP-005; UI-ATT-006 | TemporalImpactPreview | Boundary/dependent work and owned HR exception, no grace/fake closure | VQA-09–VQA-10; AC-43 |
| EMP-007 | UI-EMP-002–UI-EMP-004 | DetailSummary, ActivityTimeline | Only permitted fields and reasoned before/after history | VQA-09–VQA-10; AC-08, AC-33 |
| EMP-008 | UI-EMP-001; UI-DASH-003 | AdvancedDataTable, ExportStatusPanel | Scoped filters/fields/counts and queued private employee CSV | VQA-09–VQA-10; AC-33–AC-34 |
| SHIFT-001 | UI-SHIFT-002 | ShiftCalendar, TemporalImpactPreview | Explicit local/overnight/break template and retirement | VQA-08, VQA-10; AC-12 |
| SHIFT-002 | UI-SHIFT-001, UI-SHIFT-003 | ShiftCalendar, TemporalImpactPreview | Weekly/anchored rotation/dated assignment | VQA-08, VQA-10; AC-10–AC-11 |
| SHIFT-003 | UI-SHIFT-004–UI-SHIFT-005 | ShiftCalendar, TemporalImpactPreview | Reasoned individual/special replacement context | VQA-08, VQA-10; AC-10, AC-12 |
| SHIFT-004 | UI-SHIFT-003, UI-SHIFT-005 | ShiftCalendar, TemporalImpactPreview | Overnight overlap and atomic rejected batch preview | VQA-08, VQA-10; AC-11 |
| SHIFT-005 | UI-SHIFT-001, UI-SHIFT-004 | ShiftCalendar, TemporalImpactPreview | Holiday hierarchy and explicit ordinary working weekend | VQA-08, VQA-10; AC-10 |
| SHIFT-006 | UI-SHIFT-005 | ShiftCalendar, TemporalImpactPreview | Current impact preview before publish | VQA-08, VQA-10; AC-12 |
| SHIFT-007 | UI-SHIFT-005; UI-ATT-003; UI-LEAVE-003 | ShiftCalendar, TemporalImpactPreview | Protected reference and historical context, stale pending calculation | VQA-08, VQA-10; AC-12 |
| SHIFT-008 | UI-SHIFT-005; UI-SHARED-001 | ShiftCalendar, TemporalImpactPreview | Committed publication then safe notification | VQA-08, VQA-10; AC-35 |
| SHIFT-009 | UI-SHIFT-003, UI-SHIFT-005 | ShiftCalendar, TemporalImpactPreview | 90d preview not correctness/max-range limit; on-demand validation outcome | VQA-08, VQA-10; AC-11–AC-12 |
| ATT-001 | UI-ATT-001 | AttendanceClockPanel, PhotoCapturePanel | Own eligible genuine in or guarded labeled synthetic projection | VQA-09, VQA-11–VQA-12, VQA-18; AC-13, AC-47–AC-48 |
| ATT-002 | UI-ATT-001, UI-ATT-003 | AttendanceClockPanel | Separate out evidence, original overnight occurrence, fresh eligibility | VQA-09, VQA-11–VQA-12, VQA-18; AC-16, AC-43 |
| ATT-003 | UI-ATT-001 | AttendanceClockPanel | Disable duplicate UI action, recover same committed intent | VQA-09, VQA-11–VQA-12, VQA-18; AC-14 |
| ATT-004 | UI-ATT-001, UI-ATT-007 | GeoValidationFeedback | Resolved per-field policy and exact server rejection reason | VQA-09, VQA-11–VQA-12, VQA-18; AC-15, AC-17 |
| ATT-005 | UI-ATT-001, UI-ATT-007 | GeoValidationFeedback | Disabled/optional/required, Standard/Strict failures, no identity/bypass claim | VQA-09, VQA-11–VQA-12, VQA-18; AC-17–AC-18 |
| ATT-006 | UI-ATT-002–UI-ATT-003 | StatusBadge, DetailSummary | Expected remaining-work segments, Late AND Early departure flags from server | VQA-09, VQA-11–VQA-12, VQA-18; AC-15, AC-21 |
| ATT-007 | UI-ATT-002–UI-ATT-003, UI-ATT-006 | StatusBadge, ErrorState | No in/missing out/unassigned/overnight inconsistency distinct | VQA-09, VQA-11–VQA-12, VQA-18; AC-19, AC-43 |
| ATT-008 | UI-ATT-004 | Form patterns, ActivityTimeline | Proposed factual correction with reason and original evidence | VQA-09, VQA-11–VQA-12, VQA-18; AC-20 |
| ATT-009 | UI-ATT-005 | ApprovalDecisionPanel | Independent reviewer, stale/concurrent decision, no self-review | VQA-09, VQA-11–VQA-12, VQA-18; AC-20 |
| ATT-010 | UI-ATT-006; UI-DASH-001 | ReportBasisPanel | Versioned reconciliation/pending refresh, present incomplete | VQA-09, VQA-11–VQA-12, VQA-18; AC-19, AC-30 |
| ATT-011 | UI-ATT-003, UI-ATT-006; UI-LEAVE-004–UI-LEAVE-005 | TemporalImpactPreview | Explicit known/uncertain work-leave conflict resolution | VQA-09, VQA-11–VQA-12, VQA-18; AC-21, AC-29, AC-43 |
| ATT-012 | UI-ATT-001, UI-ATT-003 | PhotoCapturePanel | Private authorized evidence, not public key/blurred hidden image | VQA-09, VQA-11–VQA-12, VQA-18; AC-13, AC-17, AC-33 |
| ATT-013 | UI-ATT-001 | AttendanceClockPanel, ErrorState | Unsent/upload/received/unknown/committed/recovery distinguishable | VQA-09, VQA-11–VQA-12, VQA-18; AC-14 |
| LEAVE-001 | UI-LEAVE-007 | Form/DetailSummary | Policy/type/eligibility/proration/carry/expiry explicit, no statutory guess | VQA-09, VQA-13–VQA-14; AC-27 |
| LEAVE-002 | UI-LEAVE-001, UI-LEAVE-006 | LeaveBalanceSummary | Posted/reserved/available per year/type, immutable ledger | VQA-09, VQA-13–VQA-14; AC-22, AC-44–AC-45 |
| LEAVE-003 | UI-LEAVE-010 | ErrorState, LeaveBalanceSummary | Due closed-period cumulative posting; retry-safe visible result | VQA-09, VQA-13–VQA-14; AC-27, AC-44 |
| LEAVE-004 | UI-LEAVE-002 | LeaveRequestForm, ShiftCalendar | Full/half actual work/break/local shift-start/year preview | VQA-09, VQA-13–VQA-14; AC-21, AC-45 |
| LEAVE-005 | UI-LEAVE-002 | LeaveRequestForm | Eligibility/overlap/posted capacity conflict, no forecast borrowing | VQA-09, VQA-13–VQA-14; AC-22, AC-44–AC-45 |
| LEAVE-006 | UI-LEAVE-002 | LeaveRequestForm, ApprovalStepper | First valid route + all-period reserve result, failure reserves nothing | VQA-09, VQA-13–VQA-14; AC-22, AC-42 |
| LEAVE-007 | UI-LEAVE-004 | ApprovalStepper, ApprovalDecisionPanel | Current ordered independent assigned stage only | VQA-09, VQA-13–VQA-14; AC-23, AC-28, AC-46 |
| LEAVE-008 | UI-LEAVE-004 | ApprovalDecisionPanel | Final stage versus intermediate result and consumed reservation | VQA-09, VQA-13–VQA-14; AC-24 |
| LEAVE-009 | UI-LEAVE-004 | ApprovalDecisionPanel | Reject reason/close pending stages/release once message | VQA-09, VQA-13–VQA-14; AC-25 |
| LEAVE-010 | UI-LEAVE-003, UI-LEAVE-005 | ApprovalDecisionPanel, LeaveBalanceSummary | Pending cancellation≠restored; independent linked reversal/expiry | VQA-09, VQA-13–VQA-14; AC-25–AC-26 |
| LEAVE-011 | UI-LEAVE-004, UI-LEAVE-008–UI-LEAVE-009 | ApprovalStepper | Dated delegation/remind/escalate/reassign cannot create authority | VQA-09, VQA-13–VQA-14; AC-28, AC-42, AC-46 |
| LEAVE-012 | UI-LEAVE-002–UI-LEAVE-003 | ActivityTimeline, LeaveRequestForm | Frozen submitted/approved context; explicit stale reacceptance | VQA-09, VQA-13–VQA-14; AC-12, AC-45 |
| LEAVE-013 | UI-LEAVE-003, UI-LEAVE-005; UI-ATT-006 | TemporalImpactPreview | No conflicting worked/excused assertion or guessed segment | VQA-09, VQA-13–VQA-14; AC-21, AC-29 |
| LEAVE-014 | UI-LEAVE-006 | LeaveBalanceSummary | Signed reasoned adjustment preview; no ledger row edit | VQA-09, VQA-13–VQA-14; AC-22, AC-26–AC-27 |
| LEAVE-015 | UI-LEAVE-001–UI-LEAVE-005 | DetailSummary, AdvancedDataTable | Restricted reason omitted from all props/rows/alerts/exports | VQA-09, VQA-13–VQA-14; AC-33 |
| DASH-001 | UI-DASH-001 | KpiCard, ReportBasisPanel, AdvancedDataTable, ExportStatusPanel | Current capability/source-intersection relevant panels, no role bypass | VQA-07, VQA-15–VQA-16; AC-04–AC-06 |
| DASH-002 | UI-DASH-001–UI-DASH-002 | KpiCard, ReportBasisPanel, AdvancedDataTable, ExportStatusPanel | Shared authorized filters/token/drill | VQA-07, VQA-15–VQA-16; AC-33 |
| DASH-003 | UI-DASH-001–UI-DASH-002 | KpiCard, ReportBasisPanel, AdvancedDataTable, ExportStatusPanel | Required KPI/attendance/late/leave/workforce/calendar patterns | VQA-07, VQA-15–VQA-16; AC-31, AC-41 |
| DASH-004 | UI-DASH-001–UI-DASH-003 | KpiCard, ReportBasisPanel, AdvancedDataTable, ExportStatusPanel | Backend exact formulas/start-date/receiving-year/N/A basis | VQA-07, VQA-15–VQA-16; AC-31–AC-32, AC-45 |
| DASH-005 | UI-DASH-001–UI-DASH-002 | KpiCard, ReportBasisPanel, AdvancedDataTable, ExportStatusPanel | Independent provisional/reconciled/stale/incomplete metadata | VQA-07, VQA-15–VQA-16; AC-19, AC-30–AC-32 |
| DASH-006 | UI-DASH-001–UI-DASH-002 | KpiCard, ReportBasisPanel, AdvancedDataTable, ExportStatusPanel | Authorized correction leads visible refreshed coherent version | VQA-07, VQA-15–VQA-16; AC-30 |
| DASH-007 | UI-DASH-003 | KpiCard, ReportBasisPanel, AdvancedDataTable, ExportStatusPanel | Permitted employee/attendance/leave/analytics CSV metadata | VQA-07, VQA-15–VQA-16; AC-33–AC-34 |
| DASH-008 | UI-DASH-003 | KpiCard, ReportBasisPanel, AdvancedDataTable, ExportStatusPanel | All CSV queued; private complete artifact/current access only | VQA-07, VQA-15–VQA-16; AC-34 |
| DASH-009 | UI-DASH-002; shared tables | KpiCard, ReportBasisPanel, AdvancedDataTable, ExportStatusPanel | Server bounded queries 25/100, variable 30d default not invented cap | VQA-07, VQA-15–VQA-16; AC-34, AC-39 |
| RULE-001–RULE-004 | UI-EMP-002; UI-SHIFT-005; UI-ATT-003; UI-LEAVE-003 | TemporalImpactPreview, ActivityTimeline | Effective context/shared resolver/protected references/conflict wording | VQA-10, VQA-12–VQA-14; backend invariant tests |
| RULE-005–RULE-007 | UI-LEAVE-002–UI-LEAVE-006; UI-ATT-005 | LeaveBalanceSummary, ApprovalDecisionPanel | Reserve/independence/atomic result truth, no UI bypass | VQA-11, VQA-13–VQA-14; backend transaction/race tests |
| RULE-008–RULE-010 | UI-ATT-001; UI-DASH-001–UI-DASH-003 | ScopeSummary, AttendanceClockPanel, ReportBasisPanel | Current scope, authoritative receipt/local start date, identified retry | VQA-09–VQA-11, VQA-16 |
| NTF-001, NTF-003–NTF-004, NTF-006 | UI-SHARED-004; UI-LEAVE-004; UI-DASH-003 | ExportStatusPanel, ErrorState | Business commit≠mail/job completion; due reminders/failures/uncertainty visible | VQA-17; backend outbox/delivery tests |
| NTF-002, NTF-005 | UI-SHARED-001 | NotificationCenter | Owner-only count/read and current target privacy | VQA-09, VQA-17; AC-35 |
| SEC-001–SEC-004 | All views; UI-ATT-001, UI-ATT-003; UI-DASH-003 | PermissionDeniedState, PhotoCapturePanel | Authentication/CSRF-safe adapter use, omitted private payload/evidence, neutral token errors | VQA-08–VQA-09, VQA-11, VQA-16; backend security tests |
| SEC-005–SEC-006, SEC-012 | UI-SHARED-002–UI-SHARED-004 | AuditHistoryViewer, ErrorState | Immutable sanitized audit and independent downstream failures; UI cannot prove SQL atomicity | VQA-17; AC-35–AC-36 |
| SEC-007–SEC-009 | UI-SHARED-003–UI-SHARED-004; high-impact forms | TemporalImpactPreview, AlertDialog | Category/version/validation/holds/restore evidence and required reauth; no extra business approval | VQA-08, VQA-17; AC-37–AC-38 |
| SEC-010–SEC-011 | All fixtures; UI-ATT-001; UI-SHARED-005 | PhotoCapturePanel, PermissionDeniedState | No secrets/visitor evidence; synthetic guard and restricted public administration | VQA-09, VQA-18; AC-40, AC-47–AC-48 |
| DATA-001–DATA-003, DATA-006, DATA-009 | Critical forms; UI-ATT-001; UI-LEAVE-002–UI-LEAVE-006 | TemporalImpactPreview, AttendanceClockPanel | UI only confirms committed consistent outcome; preserves originals and staged-file failure meaning | VQA-10–VQA-14; MySQL/atomicity/file tests required |
| DATA-004–DATA-005, DATA-007–DATA-008 | All submitting views | ErrorState, Form patterns | Intent/stale/rollback/error recovery; no blind replay or frontend invariant claim | VQA-08, VQA-11; concurrency/validation tests required |
| DATA-010 | UI-DASH-003 | ExportStatusPanel | Current auth/source recheck across queued processing/download; incomplete hidden | VQA-09, VQA-16; AC-34 |
| NFR-001–NFR-008 | UI-DASH-001–UI-DASH-003; data/capture views | AdvancedDataTable, ReportBasisPanel | Server work, useful loading and async outputs; no screenshot proof of measured target | VQA-07, VQA-11, VQA-15–VQA-16; approved benchmark still required |
| NFR-009–NFR-010 | All views, especially UI-ATT-001 and UI-LEAVE-002 | All foundations; L1–L7 | Responsive keyboard/focus/touch/capture/text alternatives and supported-browser tests | VQA-02–VQA-06, VQA-19; AC-41 |
| NFR-011–NFR-012, NFR-015–NFR-016 | UI-SHARED-004; UI-ATT-001; UI-DASH-003 | ErrorState, ExportStatusPanel | Safe errors/incident references/health/recovery visibility, not new monitoring platform | VQA-08, VQA-11, VQA-16–VQA-17 |
| NFR-013–NFR-014 | Shared shell/components; UI-SHARED-005 | Reusable pattern inventory | Existing stack/directory boundaries; synthetic deployment presentation | VQA-01, VQA-18, VQA-20; deployment tests |
| QA-001–QA-008 | All 44 views and associated backend workflows | VQA-01–VQA-20; §25 handoff | Design coverage/visual+interaction tests complement unit/MySQL/security/benchmark/SIT/UAT gates | Implementation/release evidence remains mandatory; not satisfied by this file |
| REL-001–REL-008 | All six modules and shared support | §25–§30 conformance | Approved scope and quality-gated stable Phase 1; no Phase 2 UI or auto-release | No release completion or approval claimed |

### 28.1 Complete workflow coverage

| Workflow | Design views |
| --- | --- |
| WF-01 | UI-AUTH-002, UI-AUTH-006; optional invitation after UI-EMP-003 |
| WF-02 | UI-AUTH-001, UI-AUTH-003–UI-AUTH-005 |
| WF-03 | UI-AUTH-006–UI-AUTH-007; UI-SHARED-005 sanitized projection |
| WF-04 | UI-EMP-001–UI-EMP-003 |
| WF-05 | UI-EMP-002, UI-EMP-004, UI-EMP-007 |
| WF-06 | UI-EMP-005–UI-EMP-006; UI-ATT-006 |
| WF-07 | UI-SHIFT-001–UI-SHIFT-003, UI-SHIFT-005 |
| WF-08 | UI-SHIFT-001, UI-SHIFT-004–UI-SHIFT-005 |
| WF-09 | UI-ATT-001 |
| WF-10 | UI-ATT-001–UI-ATT-003 |
| WF-11 | UI-ATT-003–UI-ATT-006; refreshed UI-DASH-001–UI-DASH-002 |
| WF-12 | UI-LEAVE-006–UI-LEAVE-007, UI-LEAVE-010 |
| WF-13 | UI-LEAVE-001–UI-LEAVE-003, UI-LEAVE-008 configuration |
| WF-14 | UI-LEAVE-003–UI-LEAVE-004 |
| WF-15 | UI-LEAVE-004, UI-LEAVE-008–UI-LEAVE-009; UI-SHARED-001 |
| WF-16 | UI-LEAVE-003–UI-LEAVE-006 |
| WF-17 | UI-DASH-001–UI-DASH-002 |
| WF-18 | UI-DASH-002–UI-DASH-003; UI-SHARED-001 |

### 28.2 Locked business and operational decision preservation

| Locked decisions | Preserved area | Design contract |
| --- | --- | --- |
| BR-01, BR-02, BR-26 | Exact scoped role/nav/field algebra | §10–§11, §23; DSD-010 |
| BR-03, BR-04, BR-22 | Stable employee/account separation and effective historical context | §13, §16.2, §17 Workforce; DSD-014 |
| BR-05, BR-09, BR-23, BR-24, BR-25 | One resolved schedule, protected history, attendance/leave segment conflict and calendar hierarchy | §16.3–§16.5; DSD-008, DSD-011, DSD-013 |
| BR-06, BR-12, BR-20 | Private evidence/current security and isolated synthetic demo | §16.4, §20–§23; DSD-011, DSD-014 |
| BR-07, BR-10, BR-11 | Configured exact leave economics and independent ordered routing/delegation | §15–§16, §18; DSD-013 |
| BR-08, BR-17, BR-27 | Interactive scoped coherent metrics, provisional/reconciled coverage | §14, §19, §23; DSD-006, DSD-009 |
| BR-13, BR-14, BR-15, BR-16 | Owned notifications, committed-state truth, retention/restore controls and operational recovery | §16.7, §17 Shared, §21; DSD-014 |
| BR-18, BR-19, BR-21, BR-28 | Risk-based quality gates, bounded handoff, unchanged performance/release claims | §25–§30; DSD-012 |

| Approved OD | Preserved design meaning |
| --- | --- |
| OD-01 — APPROVED | Permission/nav/field projections §11; DSD-010 |
| OD-02 — APPROVED | Auth/session/reauth §15–§16.1; no new account rules |
| OD-03 — APPROVED | Offboard/rehire/unknown factual end §16.2, §16.4; UI-EMP-005–UI-EMP-006 |
| OD-04 — APPROVED | Required approved fields/optional root manager/account §16.2; UI-EMP-003 |
| OD-05 — APPROVED | Date/timezone/overnight/half-open meaning §6, §16.2–§16.5 |
| OD-06 — APPROVED | Precedence/protected/stale/horizon §16.3; UI-SHIFT-005 |
| OD-07 — APPROVED | Resolved windows/grace/presence-completeness §16.4; UI-ATT-007 |
| OD-08 — APPROVED | GPS Standard/Strict error distinctions §16.4; no identity/manual/bypass claim |
| OD-09 — APPROVED | Genuine photo capture/review/validation §16.4; PhotoCapturePanel |
| OD-10 — APPROVED | Independent reasoned correction §15, §16.4; UI-ATT-004–UI-ATT-005 |
| OD-11 — APPROVED | Shift-aware full/half units/excluded breaks §16.5; LeaveRequestForm |
| OD-12 — APPROVED | Posted/reserved/available/forecast/year/expiry lineage §16.5; DSD-013 |
| OD-13 — APPROVED | Approved cancellation stays effective pending review §15, §16.5 |
| OD-14 — APPROVED | Root fallback/distinct actual actor/explicit snapshot exception §16.5; ApprovalStepper |
| OD-15 — APPROVED | Reminder/escalation/nontransactional delivery semantics §16.5, §16.7 |
| OD-16 — APPROVED | Candidate duration≠exact-version purge authority/current holds §16.7; UI-SHARED-003 |
| OD-17 — APPROVED | Independent evaluation dimensions/coherent metric basis §18–§19; DSD-008–DSD-009 |
| OD-18 — APPROVED | Recovery evidence/private controls §16.7; UI-SHARED-004; no new restore UI feature |
| OD-19 — APPROVED | Async all-CSV/25–100 pages/30d default/no invented hard range cap §14, §19 |
| OD-20 — APPROVED | English, client matrix, synthetic no visitor evidence/public mutation guard §20–§21 |
| OD-21 — APPROVED | No regulatory claim or unverified statutory amount; real-data validation remains source gate §16.7, §30 |

## 29. Design System Conformance Checklist

This is a specification consistency audit of the approved design system; it does not establish implemented UI or change the approval status of source documents.

| Check | Result / requirement |
| --- | --- |
| Source authority | Approved PRD > owner-accepted architecture > approved schema > design/frame. §2 fingerprints are refreshed for the accepted G-000/T-0003 attendance clarification; original source/design approvals remain preserved. |
| Six modules / scope | Identity, Workforce, Scheduling, Attendance, Leave, Analytics only; shared support views not another business module; no Phase 2 features. |
| Locked decisions |28 BR and 21 APPROVED OD references preserved; all 18 workflows mapped; no new business rule or arbitrary role bypass. |
| Screens |44 unique major-view IDs with full 14-attribute contracts; state/responsive/modal variants not inflated into routes; no page per database table. |
| Tokens | Complete semantic color mapping, type/spacing/dimensions/radii/elevation/z-index and motion; primary/feedback contrast calculated; final composed UI still requires checks. |
| Reuse |26 foundation controls +30 application patterns; meaningful variants/states/accessibility/responsive/implementation/source contracts; no premature code scaffold. |
| Scope / navigation | Server capabilities, exact-permission union/intersection, current graph, G/O separation, omitted restricted fields, no artificial role switch. |
| Status / workflow truth | Committed clock≠upload; stage≠whole leave approval; pending cancellation≠restored; presence/completeness/freshness distinct; unknown≠Absent/0%. |
| Sensitive / historical data | Current authorization separate from original period/context, private photo/reason omission, no raw model/GPS/token/key in ordinary UI. |
| Table / form compatibility | TanStack manual 25/100 server behavior, VeeValidate 4/Zod 3 and Laravel authority, typed DTO, current version/intent/recovery. |
| Charts | ECharts typed backend metrics/source basis; N/A/coverage, text/table equivalent, keyboard detail and shared export token; no frontend HR formulas. |
| Responsive / accessibility | Tailwind-compatible base→2xl, explicit phone/tablet/laptop/desktop,320px/200% reflow, touch/focus/labels/reduced-motion patterns; no formal WCAG certification claimed. |
| Demo | Server synthetic labels, no visitor capture/location, no public privileged administration, read-only comparison no grants, reset/session/generation messaging. |
| pen.dev capability / workflow | Primary docs checked; single grouped future .pen, variables/reusable instances, frame/rev/Git/owner approval annotations; no assumed automatic sync/code compiler. |
| Codex readiness | Approved reference/data/action/state/accessibility/visual-test handoff explicit; inspect reuse before code; no alternative framework/API/store architecture. |
| Database alignment | Authorized purpose-specific DTOs, no raw persistence views; accepted Schema §11.5 projects late/early independently and persists inconsistent completeness; unknown end remains explicit. Runtime conformance is still required. |
| Deliverable boundary | This Markdown file only; no PRD/architecture/schema edit, TASK.md, Vue source, `.pen`, image or screenshot produced. |
| Approval / evidence | Approved Design System Baseline; project owner approval recorded on 23 October 2026 (Asia/Jakarta); no individual screen approval, package compatibility run, WCAG certification or performance target claimed. |

## 30. Risks, Limitations & Future Extensions

| Item | Source / impact | Smallest recommended treatment |
| --- | --- | --- |
| Attendance projection conformance | Owner accepted the G-000/T-0002 proposal through T-0003 on 24 October 2026. Schema §11.5 now includes inconsistent completeness; punctuality remains the arrival scalar, with independently derived late/early server flags. | Use each retained evaluation's effective bounds, resolved policy and exact required segments; preserve simultaneous Late AND Early departure, Present + missing-out, reviewed unknown end and NULL/N/A. Keep ck_ae_01 and event guards; never fabricate closure, infer work through excused leave or calculate HR classifications in Vue. Historical/report output must use the same referenced evaluation/source version. Verify runtime conformance at the named later persistence/evaluator/reporting gates; document acceptance alone proves no software behavior. |
| Technical processing aliases | Architecture 20.3 uses running/stale aliases while PRD 14 names export processing and keeps ready/failed/cancelled/expired lifecycle. | Display Processing for running; stale source/access as a separate freshness/recovery outcome. Backend adapter follows PRD; no new state transition invented by design. |
| Screen approval evidence | The design system is approved; no owner-approved .pen frame/revision exists as an output of this task. | Proceed to screen authoring. Record screen approval on a concrete later revision before claiming visual conformance to it; do not ask for extra approvals on already authorized routine work. |
| Current tool versus pinned app versions | Current pen.dev/shadcn/Tailwind docs may cover more than the locked app versions. | Use compatible variable/layout primitives; preserve Architecture 6 version families and check actual installed registry/MCP capabilities at execution. No automatic sync/export/production-Vue guarantee. |
| Complex grids/matrices on small screens | HR configuration/approval evidence can overflow a narrow device or produce hidden controls. | Labeled region scroll, focused row editing, stacked detail/action and explicit phone QA; never remove mandatory evidence/action solely to fit a frame. |
| Status explosion | A single row can carry work expectation, presence, punctuality, completeness, conflict and freshness. | Table prioritizes primary+material exception with separate freshness; detail renders all labeled dimensions; no precedence rule that overwrites truth. |
| Long/dynamic/translated content | Fixed synthetic names/labels can conceal clipping and dependency shifts. | Test long names, empty/unknown/hidden fields, many scope-valid categories, zoom and actual errors; initial English only. |
| Genuine device behavior | Camera/GPS permissions/Strict uncertainty/codec/ingress timing cannot be proven by a canvas or public fixture. | Private supported-device/backend tests and source benchmark gates; correction recovery, no sample fallback. |
| Real-data privacy/retention | UI consistency or approved requirements do not validate current Indonesian employment/privacy law or approve category purge. | OD-21 validation, current category/version/holds and secure deployment remain existing gates; no statutory amount/compliance module added. |
| Performance/accessibility completion | Screens/token ratios are not a runtime benchmark or complete assistive audit. | Playwright/manual keyboard/screen reader/real-device checks and approved reproducible backend/dashboard/capture benchmark; failed required checks remain incomplete. |

No unresolved product choice requires reopening the approved six-module behavior. The attendance projection clarification is accepted in Schema §11.5; implementation must still verify the version-bound server contract and its independent dimensions. This specification is ready for **pen.dev screen design using the approved design system**, and supplies references for **subsequent bounded TASK.md planning**; no task backlog, code or design asset is generated here. Dark mode, localization, extra density modes and larger design-file partitioning are possible later reviewed extensions, not mandatory initial features or visible empty controls.

## 31. Glossary

| Term | Meaning |
| --- | --- |
| Capability | Current server-authorized view/action/field eligibility; not a role-name shortcut |
| Scope | Exact permission's permitted employee set: all/team/self; global/owner remain distinct |
| Current team | Current direct/indirect reporting tree under source rules, not historical manager relationship |
| Historical context | Preserved employment/organization/schedule/policy at the original effective time |
| Major view | Workflow-oriented screen or substantial review workspace, not every tab/modal/state/table |
| Foundation | Reviewed shadcn-vue primitive/control with token and interaction conventions |
| Application pattern | Shared/domain composition that solves an HR interaction need, implemented only when used |
| Semantic token | Named color/size/type/spacing meaning, shared between CSS and design references |
| Occurrence | One employee's concrete applicable shift/assignment/slot, attributed to its local start date |
| Committed | Server-confirmed durable successful business outcome, not uploaded/queued/optimistic UI |
| Unknown result | Request may have committed; recover the same intent before a new action |
| Provisional / reconciled | Evaluation freshness states; neither alone implies complete evidence or approval |
| Posted / reserved / available | Actual signed entitlement / active pending allocation / posted minus reserved, per eligible bucket/period |
| Forecast | Expected future entitlement, visibly separate and not available for reservations |
| N/A | No applicable/compatible denominator, never fabricated zero |
| Source/report context | Coherent server metric/query/cutoff/fingerprint reference used by view/detail/export; not an access grant |
| Synthetic evidence | Server-selected demo fixture labeled simulated; never visitor photo/GPS or proof of genuine capture |
| Approved design reference | Explicit owner-approved file/frame/revision and covered states/viewports; cannot override PRD |
| VQA / DSD / UI IDs | Design verification, design-decision and screen-reference identifiers; not new business requirements |

