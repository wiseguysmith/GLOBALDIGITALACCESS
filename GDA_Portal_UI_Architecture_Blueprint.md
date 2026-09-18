# Global Digital Access (GDA)
## Portal UI Architecture & Open-Source Reference Blueprint

**Document type:** Product / UX / Technical Implementation Specification  
**Status:** Architecture baseline for implementation  
**Audience:** GDA product leadership, UI/UX designers, Claude, Codex, software engineers, technical partners  
**Primary objective:** Define, with implementation-level clarity, the portal experience GDA should build and identify which open-source projects may be safely used as code foundations versus visual/workflow references only.

---

# 1. Executive Decision

GDA should **not** fork a complete third-party financial, CRM, crypto, or data-room platform and attempt to reshape it into GDA. The portal should be a purpose-built institutional workflow application with GDA's own domain model, permissions, readiness gates, and visual identity.

The recommended direction is:

- **Application shell / navigation patterns:** shadcn-admin (MIT) as a practical reference and optional starter shell.
- **Enterprise component language:** Twenty UI (`twenty-ui`) where independently licensed MIT components are useful.
- **CRUD and resource architecture:** Refine (MIT), if it fits the existing GDA repository after technical audit.
- **Charts / analytics:** Tremor (Apache-2.0), selectively.
- **Backend / authentication / storage candidate:** Supabase (Apache-2.0) if consistent with the established GDA platform architecture.
- **Document/data-room UX inspiration:** Papermark; treat as reference unless a legal review confirms the exact code path can be reused under acceptable terms.
- **Access-control / security UX inspiration:** Infisical; use MIT-licensed portions only where beneficial, never copy enterprise-only code.
- **Financial-product visual inspiration:** Midday; reference the UX only unless GDA obtains a compatible commercial license or counsel approves the exact use.
- **Signing workflow reference:** Documenso; reference the UX/process unless GDA intentionally accepts AGPL obligations or obtains licensing.
- **Admin/workflow reference:** Supabase Studio and Appwrite Console.

**Core principle:** GDA should own its product architecture and domain logic. Open-source projects should accelerate implementation, not define what GDA becomes.

---

# 2. Product Definition

GDA is not a generic tokenization dashboard and not merely a CRM. The portal is the operating system for moving an asset/project from initial evaluation through documented readiness, professional validation, structuring, partner coordination, and technical activation.

The platform must make one question obvious at all times:

> **What is this project, where is it in the readiness process, what is missing, who is responsible, and what must happen before it can move forward?**

The user interface must therefore privilege:

1. **State** — current lifecycle stage and readiness status.
2. **Evidence** — the documents, data, approvals, and professional work supporting that state.
3. **Responsibility** — who owns the next action.
4. **Gating** — what is blocked and why.
5. **Auditability** — who changed, approved, rejected, uploaded, or validated what and when.
6. **Jurisdiction + instrument awareness** — requirements vary by legal context and instrument, not simply by asset class.
7. **Controlled handoff** — technical activation is unavailable until the applicable readiness, legal, compliance, and economic requirements have been satisfied.

---

# 3. GDA Portal Design Principles

## 3.1 Institutional, not "crypto"

The portal must look like serious financial infrastructure. Avoid neon gradients, coin imagery, token price widgets, animated blockchain backgrounds, oversized Web3 terminology, and decorative dashboards that do not help users make a decision.

The desired visual character is:

- Deep navy / charcoal foundations.
- Silver / cool-neutral accents.
- White or near-white work surfaces where document-heavy work benefits from contrast.
- Restrained use of status color.
- Dense but readable information architecture.
- Strong typography and hierarchy.
- Minimal animation except for state changes and progress.
- Desktop-first professional workflow with excellent responsive behavior.

## 3.2 Workflow before analytics

A chart should exist only when it helps a user answer a concrete operational question. The project workspace, readiness requirements, documents, approvals, blockers, and next actions are more important than dashboard vanity metrics.

## 3.3 Progressive disclosure

Do not show every discipline to every role. An asset owner should not see internal legal deliberations. An investor should not see draft structuring notes. A technical provider should not be handed a project before the handoff gate opens.

## 3.4 Evidence-backed status

No major status may be represented as complete merely because a user clicked a checkbox. Completion should point to supporting evidence, a responsible person, and where required, a professional validation event.

## 3.5 Explainability

Every blocked stage should provide a human-readable reason. Example:

> Technical activation is locked. Two legal requirements and one economic validation remain incomplete.

## 3.6 Auditability by default

The UI should surface immutable or append-only audit events for critical changes, not hide them in server logs.

---

# 4. Open-Source Reference Matrix

The implementation team must treat licensing as an engineering requirement. A repository being visible on GitHub does **not** automatically mean GDA may copy any portion of it into a proprietary commercial product.

| Project | Primary GDA value | Current license position reviewed | GDA treatment |
|---|---|---|---|
| `satnaing/shadcn-admin` | App shell, navigation, command UI, tables, forms, responsive layout | MIT | **ADOPT / ADAPT** |
| `twentyhq/twenty` — `twenty-ui` package only | Enterprise objects, tables, detail views, component patterns | Twenty states `twenty-ui` is MIT; most of full repo is AGPL with enterprise exceptions | **ADOPT SELECTIVELY**; verify file/package license before copying |
| `refinedev/refine` | CRUD/resource architecture, access control, data fetching, routing abstraction | MIT | **EVALUATE FOR ADOPTION** |
| `tremorlabs/tremor` | Charts, metrics, compact analytics | Apache-2.0 | **ADOPT SELECTIVELY** |
| `supabase/supabase` / Studio | Backend candidate plus strong admin/workspace patterns | Apache-2.0 | **EVALUATE / ADOPT where architecture permits** |
| `appwrite/console` | Guided admin workflows, settings hierarchy | BSD-3-Clause | **REFERENCE / SELECTIVE ADOPTION** |
| `Infisical/infisical` | Roles, secrets/security UI, audit-minded UX | MIT outside enterprise (`ee`) areas; enterprise areas separately licensed | **REFERENCE / USE MIT-ONLY PORTIONS** |
| `papermark/papermark` | Data-room, document, investor-access UX | Repository licensing requires careful path-level review; advanced/enterprise features may have separate terms | **REFERENCE FIRST** |
| `midday-ai/midday` | Premium modern finance visual language | Repository currently presents AGPL/commercial-use licensing language that warrants caution | **VISUAL REFERENCE ONLY** without licensing approval |
| `documenso/documenso` | Document signing and status workflows | AGPL core; enterprise features licensed separately | **REFERENCE** unless licensing strategy intentionally allows reuse |

### License rule for Claude/Codex/engineers

Before copying code from any external repository:

1. Identify the exact source file and package.
2. Read the license applicable to **that exact path/package**.
3. Record the origin in GDA's third-party notices / provenance ledger.
4. Do not copy AGPL, source-available, enterprise, or commercially restricted code into GDA unless the project owner has explicitly approved the licensing consequence.
5. When uncertain, recreate the interaction pattern from first principles rather than copying implementation code.

---

# 5. Recommended Technical UI Foundation

This section describes the preferred target. It is **not permission to overwrite an existing architecture**. Claude/Codex must inspect the current GDA repository first and preserve compatible existing conventions.

## 5.1 Preferred frontend

- TypeScript.
- React.
- Tailwind CSS.
- shadcn/ui / Radix-style accessible primitives.
- Lucide-style iconography.
- Server-aware routing appropriate to the repository (Next.js if already used or selected; otherwise preserve existing framework).
- Component-first design system rather than page-specific CSS.

## 5.2 Application/resource layer

Evaluate Refine if the current codebase benefits from its resource, auth, access-control, routing, and data-provider abstractions. Do **not** add Refine solely because it appears in this document. It must reduce complexity compared with the existing architecture.

## 5.3 Backend candidate

Supabase is a strong candidate where GDA needs:

- PostgreSQL.
- Auth.
- Row-Level Security.
- Object/file storage.
- Realtime events.
- Edge/server functions.

However, the backend choice belongs to the broader GDA platform architecture. The UI implementation must not silently introduce Supabase if another backend is already established.

## 5.4 Analytics

Use Tremor or equivalent chart primitives for a small number of meaningful analytics. Do not make Tremor the visual identity of the product.

---

# 6. Canonical Information Architecture

## 6.1 Global navigation

Recommended primary navigation:

1. **Home**
2. **Projects**
3. **Readiness**
4. **Documents**
5. **Legal & Compliance**
6. **Economics**
7. **Partners**
8. **Investors**
9. **Activity**
10. **Settings**

Navigation items must be permission-aware. A user should not see inaccessible modules merely to receive an authorization error after clicking them.

## 6.2 Global context switcher

The top bar should support context for:

- Organization / workspace.
- Project.
- Jurisdiction.
- Instrument where relevant.

Do not force jurisdiction or instrument into a global selector when the user is already inside a specific project. The project's own metadata is authoritative.

## 6.3 Command/search interface

Provide a command palette / universal search capable of finding:

- Projects.
- Organizations.
- People.
- Requirements.
- Documents.
- Partners.
- Investors.
- Actions.

Search results must preserve permissions.

---

# 7. Canonical GDA Project Lifecycle

Use a lifecycle that reflects GDA's actual role as readiness and transaction coordinator rather than a technology vendor pretending to own every professional decision.

## Stage 0 — Intake / Fit

Purpose: determine whether the opportunity is worth entering the GDA process.

Key UI:
- Asset/project summary.
- Sponsor/owner information.
- Jurisdiction.
- Proposed instrument or financing objective.
- Tokenization/digital-access fit questions.
- Initial red flags.
- Decision: proceed, hold, decline, request more information.

## Stage 1 — Readiness & Evidence

Purpose: establish the factual and documentary foundation.

Key UI:
- Requirement groups.
- Evidence requests.
- Document uploads.
- Missing-data tracker.
- Ownership and due dates.
- Readiness percentage with transparent calculation.

## Stage 2 — Structuring

Purpose: define the proposed legal/economic/operational structure.

Key UI:
- Entity structure.
- Instrument.
- Rights and restrictions.
- Project economics.
- Distribution/raise assumptions.
- Jurisdictional dependencies.
- Open decisions.

## Stage 3 — Professional Validation

Purpose: collect approvals/validations within each professional's scope.

Examples:
- Securities/legal review.
- KYC/KYB/AML readiness.
- Valuation or economics validation.
- Environmental measurement/verification where applicable.
- Tax or accounting review where applicable.

GDA coordinates the workflow. The relevant professional approves or validates matters within that professional's scope.

## Stage 4 — Activation Readiness / Handoff

Purpose: determine whether the project is ready to move to technical implementation.

The UI must show a clear handoff gate:

- Readiness requirements satisfied.
- Legal/compliance requirements satisfied.
- Economics validated where required.
- Required documentation present.
- GDA coordination sign-off recorded.
- Technical provider acceptance pending / accepted / rejected.

A technical provider must be able to reject a proposed implementation as technically irresponsible or infeasible. Technical acceptance does not override legal, compliance, or economic requirements.

## Stage 5 — Technical Activation

Purpose: implementation by the applicable technical provider.

GDA should track:
- Handoff date.
- Technical provider.
- Architecture status.
- Deployment milestones.
- Exceptions / change requests.
- Production activation status.

GDA should **not** present itself as having performed regulated, legal, valuation, or technical work that was actually performed by an external licensed or specialized professional.

## Stage 6 — Investor / Market Access (where applicable)

Purpose: control approved visibility and participation after the project is properly structured and activated.

Potential features:
- Approved investor materials.
- Data room.
- Access requests.
- Suitability / eligibility states.
- Offering or opportunity information appropriate to the legal framework.
- Investor communications and activity.

This stage must be jurisdiction- and instrument-aware. It must not assume every project is a public offering or freely tradeable token.

---

# 8. Portal Home Dashboard

The home screen should answer: **What needs my attention today?**

## Required components

### A. Attention Queue
High-priority items assigned to the current user:
- Missing evidence.
- Validation requests.
- Approval requests.
- Expiring documents.
- Blocked projects.
- Overdue actions.

### B. Project Pipeline
Show count by lifecycle state. Avoid fake financial KPIs if the data is not meaningful.

### C. Recently Updated Projects
Display:
- Project name.
- Jurisdiction.
- Lifecycle stage.
- Readiness percentage.
- Latest event.
- Responsible owner.

### D. Critical Blockers
List blockers with severity, owner, age, and affected stage.

### E. Activity Feed
Permission-filtered recent activity.

### F. Optional portfolio analytics
Only after meaningful data exists:
- Projects by jurisdiction.
- Projects by instrument.
- Project count by readiness band.
- Time-in-stage.
- Completion throughput.

---

# 9. Projects Module

## 9.1 Project list

The default projects view must be a serious data table, not a card wall.

Recommended columns:
- Project.
- Client / sponsor.
- Jurisdiction.
- Asset category.
- Proposed instrument.
- Lifecycle stage.
- Readiness %.
- Blockers.
- Owner.
- Last activity.

Required interactions:
- Sort.
- Filter.
- Saved views.
- Column visibility.
- Search.
- Bulk actions only where safe.
- Export subject to permissions.

Saved view examples:
- My active projects.
- Awaiting legal validation.
- Missing evidence.
- Ready for handoff.
- Costa Rica.
- Environmental/natural assets.

## 9.2 New project wizard

The wizard should be staged, saveable, and resumable.

Step sequence:
1. Project identity.
2. Sponsor / organization.
3. Jurisdiction.
4. Asset / opportunity type.
5. Objective / capital need.
6. Proposed or unknown instrument.
7. Initial fit questions.
8. Document starter pack.
9. Internal owner.
10. Review and create.

Do not require users to know a legal instrument before initial evaluation if it has not yet been determined.

---

# 10. Project Workspace — Primary Screen

This is the most important interface in the platform.

## 10.1 Header

Show:
- Project name.
- Short ID.
- Sponsor/organization.
- Jurisdiction.
- Asset category.
- Current instrument or "TBD".
- Lifecycle stage.
- Readiness percentage.
- Primary project owner.
- Risk/blocker indicator.

## 10.2 Lifecycle rail

Display a horizontal or vertical stage rail:

`Intake → Readiness → Structuring → Validation → Handoff → Activation → Access`

Each stage shows:
- Not started.
- In progress.
- Blocked.
- Awaiting review.
- Complete.
- Not applicable.

Clicking a stage opens its requirements and history.

## 10.3 "Next best actions"

Place this high on the screen. Examples:
- Upload cadastral plan.
- Assign securities counsel.
- Resolve beneficial-owner information.
- Obtain economics validation.
- Review technical provider rejection note.

This should be rule-driven where possible, not AI-generated guesswork.

## 10.4 Project tabs

Recommended tabs:
- Overview.
- Readiness.
- Structure.
- Documents.
- Legal & Compliance.
- Economics.
- Partners.
- Investors (permission/stage dependent).
- Activity.

---

# 11. Readiness Engine UI

The readiness interface is a core GDA differentiator.

## 11.1 Requirement anatomy

Every readiness requirement should support:
- Requirement ID.
- Title.
- Plain-language description.
- Jurisdiction.
- Instrument applicability.
- Category.
- Required / conditional / optional.
- Status.
- Responsible role/person.
- Supporting evidence.
- Reviewer / validator where required.
- Due date.
- Blocking effect.
- Dependencies.
- Notes.
- Audit history.

## 11.2 Requirement statuses

Use a controlled enum:
- `NOT_STARTED`
- `IN_PROGRESS`
- `AWAITING_EVIDENCE`
- `AWAITING_REVIEW`
- `SATISFIED`
- `REJECTED`
- `BLOCKED`
- `NOT_APPLICABLE`

Do not create ad-hoc strings in the UI.

## 11.3 Readiness calculation

The UI must disclose how the readiness score is derived. Avoid a mysterious "AI readiness score."

Recommended approach:
- Calculate completion from applicable requirements.
- Separate **completion** from **blocking status**.
- A project may display 92% complete and still be blocked by one critical legal requirement.

Example:
- Overall completion: 92%.
- Critical blockers: 1.
- Professional validations pending: 2.
- Handoff: locked.

## 11.4 Requirement detail drawer

Use a right-side drawer or dedicated page containing:
- Requirement explanation.
- Why it applies.
- Evidence.
- Comments.
- History.
- Assigned party.
- Validation action.

This interaction pattern can take inspiration from enterprise object-detail interfaces such as Twenty and Supabase Studio without copying restricted code.

---

# 12. Documents & Data Room

Documents are evidence, not just attachments.

## 12.1 Document metadata

Every document should support:
- Name.
- Type/category.
- Project.
- Jurisdiction relevance.
- Requirement links.
- Version.
- Uploaded by.
- Upload date.
- Expiration date where relevant.
- Verification/review status.
- Visibility classification.
- Hash/checksum if implemented.

## 12.2 Visibility classifications

Suggested:
- Internal GDA.
- Project team.
- Professional partners.
- Approved investor data room.
- Restricted legal.
- Restricted compliance.

Do not equate project membership with permission to every document.

## 12.3 Data-room UX

Reference Papermark-style concepts:
- Folder hierarchy.
- Access-controlled links or users.
- View audit events.
- Version control.
- Expiration.
- Approved investor collection.

But GDA should implement its own permissions and workflow instead of importing licensing risk unnecessarily.

---

# 13. Legal & Compliance Workspace

This area should be a structured workspace, not a giant textarea.

Sections may include:
- Jurisdiction profile.
- Applicable framework.
- Entity structure.
- Instrument classification/status.
- Offering/distribution pathway.
- KYC/KYB requirements.
- AML/sanctions requirements.
- Investor eligibility requirements.
- Transfer restrictions.
- Required agreements.
- Counsel assignments.
- Open legal questions.
- Legal validation events.

The platform must distinguish:
- **GDA workflow status** from
- **Professional legal conclusion**.

Never display "GDA legal approved" when the actual event is "Validated by counsel [name/organization] within assigned scope."

---

# 14. Economics Workspace

The economics workspace tracks structure and evidence around the project's economic model.

Potential components:
- Capital target.
- Use of proceeds.
- Valuation basis.
- Revenue/cash-flow assumptions.
- Ownership or participation rights.
- Distribution waterfall.
- Fees.
- Scenario model references.
- Independent/professional validation status.
- Supporting model/document versions.

GDA may coordinate and model workflow logic, but where the business process requires a licensed or qualified professional to validate economics/value, the UI must identify that professional validation separately.

---

# 15. Partners Workspace

Purpose: manage the professional network involved in each transaction/project.

Partner categories may include:
- Legal counsel.
- Compliance / AML.
- Valuation / economics.
- Environmental verifier.
- Engineering / technical diligence.
- Tokenization / technical provider.
- Custody.
- Broker/dealer / placement / regulated distribution party where applicable.
- Accounting / tax.

Partner record:
- Organization.
- Contacts.
- Jurisdictions.
- Capabilities.
- Engagement status.
- Scope.
- Assigned projects.
- Required deliverables.
- Validation authority/scope.
- Conflicts / restrictions notes.

---

# 16. Investor Workspace

This module should be enabled only when it reflects an actual, legally permitted workflow.

Potential capabilities:
- Investor/entity profile.
- KYC/KYB status.
- Eligibility / accreditation / sophistication state as applicable.
- Jurisdiction.
- Project access.
- Data-room permissions.
- Interest / allocation records.
- Required disclosures.
- Agreements.
- Activity history.

Do not assume the same investor onboarding requirements across the USA, El Salvador, Costa Rica, Panama, Colombia, and Brazil. Requirements must be driven by the jurisdiction/instrument rules library.

---

# 17. Roles & Permission Model

The UI and backend authorization must share the same source of truth. Frontend hiding alone is not security.

Recommended baseline roles:

## GDA Admin
System configuration, role management, workspace administration.

## GDA Project / Readiness Coordinator
Coordinates projects, requirements, assignments, evidence, partner activity, and handoff workflow.

## Project Owner / Issuer
Provides project information, uploads requested evidence, responds to requests, sees approved workflow state.

## Legal Professional
Accesses assigned legal workspaces, documents, questions, and validation actions within scope.

## Compliance Professional
Accesses assigned KYC/KYB/AML and monitoring-related tasks/data within scope.

## Economics / Valuation Professional
Accesses economic model, supporting evidence, and validation actions within scope.

## Environmental / Technical Specialist
Accesses assigned environmental, engineering, verification, or domain evidence.

## Technical Activation Provider
Receives projects only after handoff rules permit access; may accept, request changes, or reject technical implementation.

## Investor
Sees only approved investor-facing projects, documents, disclosures, and own records.

## Read-only Auditor / Reviewer
Time-bound, scoped, non-editing access to approved content.

### Authorization dimensions

Permissions should support more than role alone:
- Workspace.
- Project.
- Module.
- Object.
- Action.
- Document classification.
- Lifecycle stage.
- Jurisdiction where required.

---

# 18. Audit & Activity System

Critical actions must generate events such as:
- Project created.
- Requirement assigned.
- Evidence uploaded.
- Evidence replaced/versioned.
- Requirement satisfied.
- Requirement rejected.
- Professional validation submitted.
- Handoff gate opened.
- Technical provider accepted/rejected.
- Investor access granted/revoked.
- Document permission changed.
- Role changed.

Event structure should contain:
- Actor.
- Timestamp.
- Action.
- Object type/id.
- Project.
- Before/after values where appropriate.
- Source/IP/device metadata if policy requires.
- Human-readable summary.

The activity view should support filtering and export according to authorization.

---

# 19. Visual Design System

## 19.1 Brand direction

GDA visual language:
- Premium financial infrastructure.
- Deep navy primary field.
- Silver/cool gray identity accent.
- Neutral high-contrast work surfaces.
- Strong typography.
- Subtle borders rather than heavy card shadows.
- Small radius or medium radius; avoid overly playful rounded-everything styling.

## 19.2 Status colors

Use semantic colors consistently:
- Neutral = not started.
- Blue = active/in progress.
- Amber = attention/pending.
- Red = blocked/rejected.
- Green = satisfied/complete.
- Purple or separate controlled accent = external validation if useful.

Do not use color as the only status indicator. Always include icon/text.

## 19.3 Density

GDA is an expert workflow application. Default density can be tighter than a consumer app while retaining legibility.

Provide:
- Table density toggle if useful.
- Sticky table headers.
- Consistent filters.
- Detail drawers.
- Keyboard-accessible actions.

## 19.4 Typography

Use a professional sans-serif already licensed/available in the project. Do not introduce a proprietary font dependency without approval.

## 19.5 Responsive behavior

Primary target: desktop/laptop.
Secondary: tablet.
Mobile must support reviewing status, tasks, approvals, and uploads, but full structuring tables do not need to pretend they are ideal on a phone.

---

# 20. Component Source Map

This section tells an implementation agent where to look for patterns.

| GDA Component | Primary reference | Secondary reference | Build guidance |
|---|---|---|---|
| Sidebar + responsive shell | shadcn-admin | Supabase Studio | Recreate in GDA brand system |
| Command palette | shadcn-admin | Twenty | Permission-filtered search/actions |
| Project table | Twenty UI | shadcn-admin | Dense, saved views, filters |
| Project detail header | Twenty | Supabase Studio | GDA domain-specific |
| Right-side detail drawer | Twenty / Supabase Studio | Infisical | Use for requirement/document detail |
| Readiness progress | GDA custom | Tremor | Custom logic; Tremor only for primitives |
| Lifecycle rail | GDA custom | Plane-style workflow reference | Do not import external workflow semantics |
| Requirement checklist | GDA custom | project-management patterns | Must support evidence + validation |
| Document room | GDA custom | Papermark reference | Build permissions internally |
| Access control UI | GDA custom | Infisical | Backend-enforced |
| Analytics charts | Tremor | — | Minimal and meaningful |
| Settings | Supabase Studio / Appwrite Console | shadcn-admin | Hierarchical settings UX |
| Signing status | GDA integration layer | Documenso reference | Prefer API/integration over copying AGPL code |

---

# 21. Canonical Routes

Exact route syntax may be adapted to the framework, but semantic organization should remain stable.

```text
/app
/app/home
/app/projects
/app/projects/new
/app/projects/:projectId
/app/projects/:projectId/readiness
/app/projects/:projectId/structure
/app/projects/:projectId/documents
/app/projects/:projectId/legal
/app/projects/:projectId/economics
/app/projects/:projectId/partners
/app/projects/:projectId/investors
/app/projects/:projectId/activity
/app/readiness
/app/documents
/app/legal
/app/economics
/app/partners
/app/investors
/app/activity
/app/settings
/app/settings/organization
/app/settings/members
/app/settings/roles
/app/settings/jurisdictions
/app/settings/instruments
/app/settings/integrations
/app/settings/audit
```

Global module routes are portfolio/queue views; project routes are project-specific workspaces.

---

# 22. Suggested Core UI Types / Enums

These are implementation guidance, not a mandate to duplicate existing domain types.

```ts
export type LifecycleStage =
  | 'INTAKE'
  | 'READINESS'
  | 'STRUCTURING'
  | 'VALIDATION'
  | 'HANDOFF'
  | 'ACTIVATION'
  | 'ACCESS';

export type RequirementStatus =
  | 'NOT_STARTED'
  | 'IN_PROGRESS'
  | 'AWAITING_EVIDENCE'
  | 'AWAITING_REVIEW'
  | 'SATISFIED'
  | 'REJECTED'
  | 'BLOCKED'
  | 'NOT_APPLICABLE';

export type ValidationStatus =
  | 'NOT_REQUIRED'
  | 'NOT_REQUESTED'
  | 'REQUESTED'
  | 'IN_REVIEW'
  | 'VALIDATED'
  | 'DECLINED'
  | 'SUPERSEDED';

export type HandoffStatus =
  | 'LOCKED'
  | 'ELIGIBLE'
  | 'SUBMITTED'
  | 'ACCEPTED'
  | 'CHANGES_REQUESTED'
  | 'REJECTED';
```

Do not use `APPROVED` as a vague universal state. Approval/validation must identify **what** was approved and **by whom / within what scope**.

---

# 23. Empty, Loading, Error, and Permission States

Claude/Codex must implement these deliberately. A polished enterprise portal cannot treat them as afterthoughts.

## Empty state
Explain what belongs on the page and the next action.

Bad:
> No data.

Good:
> No readiness requirements have been generated for this project yet. Select the jurisdiction and proposed instrument, then generate the applicable requirement set.

## Loading
Use skeletons that resemble the final content. Avoid full-screen spinners for routine page transitions.

## Error
Show:
- Human-readable failure.
- Whether user action can resolve it.
- Retry when safe.
- Correlation/reference ID for support where relevant.

## Unauthorized
Do not reveal restricted object metadata. Provide a neutral access message and a path back.

## Locked
A locked workflow stage must explain prerequisites.

---

# 24. Internationalization

GDA is operating across multiple jurisdictions and should be designed for multilingual delivery from the start.

Baseline:
- English and Spanish architecture-ready.
- No hard-coded strings in components.
- Locale-aware dates and currency.
- Jurisdiction names and legal labels should come from controlled data, not translation guesswork.
- User-entered legal/professional text should never be automatically translated in a way that changes authoritative meaning without explicit workflow.

---

# 25. Security & Privacy UX Requirements

The UI must visibly support security rather than treating it as invisible infrastructure.

Required patterns:
- Session/account management.
- Role display.
- Sensitive document labels.
- Access-grant/revoke confirmation.
- Audit history for permission changes.
- Principle of least privilege.
- Server-side authorization.
- Secure download controls where supported.
- No sensitive values in URLs.
- No secret keys or internal credentials exposed in browser configuration.

Where Supabase is used, Row-Level Security should be designed from the domain model, not bolted on after frontend completion.

---

# 26. Accessibility

Target WCAG 2.2 AA practices where feasible:
- Keyboard navigation.
- Visible focus.
- Proper labels.
- Semantic tables.
- Screen-reader status text.
- Contrast compliant status indicators.
- No color-only signaling.
- Accessible dialogs/drawers.
- Reduced-motion support.

Using Radix/shadcn primitives does not automatically make the finished application accessible; implementation still must be tested.

---

# 27. Implementation Phases

## Phase 0 — Repository Audit

Before writing UI code, Claude/Codex must:
1. Map the existing frontend stack.
2. Identify routing.
3. Identify auth.
4. Identify database/data access.
5. Identify existing design-system components.
6. Identify current project/readiness domain models.
7. Identify tests.
8. Identify deployment target.
9. Identify environment/secrets conventions.
10. Produce a short compatibility report.

**No framework replacement in Phase 0.**

## Phase 1 — Design System Foundation

Deliver:
- Theme tokens.
- Typography.
- Spacing.
- Status system.
- Buttons.
- Inputs.
- Tables.
- Badges.
- Tabs.
- Drawers.
- Dialogs.
- Empty/error/loading states.
- Page shell.

## Phase 2 — App Shell

Deliver:
- Sidebar.
- Top bar.
- Account menu.
- Context navigation.
- Command/search shell.
- Responsive behavior.
- Permission-aware nav rendering.

## Phase 3 — Projects & Project Workspace

Deliver:
- Project list.
- Saved filter architecture.
- New project wizard.
- Project header.
- Lifecycle rail.
- Overview.
- Activity preview.

## Phase 4 — Readiness Engine

Deliver:
- Requirement groups.
- Status management.
- Evidence linking.
- Detail drawer.
- Blockers.
- Readiness calculation display.
- Professional validation flow.

## Phase 5 — Documents & Professional Workspaces

Deliver:
- Documents.
- Visibility/classification.
- Legal/compliance workspace.
- Economics workspace.
- Partner assignments.

## Phase 6 — Handoff & Technical Activation

Deliver:
- Gate eligibility.
- Handoff package.
- Technical provider access.
- Accept / changes requested / reject flow.
- Activation milestones.

## Phase 7 — Investor Access

Only after legal/business requirements are specified:
- Investor profiles.
- Access controls.
- Investor-facing data room.
- Eligibility states.
- Approved project presentation.

---

# 28. Definition of Done for Each Screen

A screen is not done because it "looks good." It is done when:

- It is wired to real domain data or explicit typed fixtures during prototype stage.
- Permission behavior is defined.
- Loading state exists.
- Empty state exists.
- Error state exists.
- Responsive behavior is defined.
- Keyboard interaction works.
- Audit event implications are identified.
- Critical actions require confirmation where appropriate.
- No mock metric is presented as production truth.
- No restricted third-party code has been copied.
- Tests cover essential behavior.

---

# 29. Anti-Patterns — Do Not Build

1. A crypto exchange-looking dashboard.
2. Token price charts on the home page unless a specific product requirement later demands them.
3. A card-only project list.
4. A universal `APPROVED` badge with no scope.
5. A progress percentage that ignores critical blockers.
6. A role dropdown that is only enforced in frontend code.
7. A technical activation button available before readiness gates.
8. A giant single project form containing every jurisdiction's requirements.
9. A document folder with no evidence links or visibility classification.
10. A separate copy of the same project information in every module.
11. Copying an AGPL/enterprise UI file because "it's on GitHub."
12. Generating legal conclusions with AI and displaying them as professional validation.
13. Making users choose a legal instrument before the structuring process has determined one.
14. Showing external partners internal data outside their assignment.
15. Building investor UX before the jurisdiction/instrument rules are defined.

---

# 30. Instructions to Claude / Codex

When this document is provided as implementation context, follow these rules:

1. Treat the GDA domain model and workflow as authoritative over any third-party template.
2. Do not redesign the lifecycle without documenting the conflict and obtaining approval.
3. Do not introduce a new major framework without first auditing the existing repository.
4. Prefer composition of small permissively licensed components over copying full applications.
5. Preserve license notices where required.
6. Maintain a `THIRD_PARTY_NOTICES.md` or equivalent provenance record for copied/adapted code.
7. For every reused external component, record repository, exact path/package, license, and modifications.
8. Never copy code from `ee`, enterprise, commercial, or restricted directories without explicit authorization.
9. Treat Papermark, Midday, Documenso, and other non-permissive areas as **design/workflow references by default**.
10. Ensure backend authorization mirrors frontend permissions.
11. Do not create fake compliance, approval, certification, or legal states.
12. Use deterministic rules for readiness calculations; do not hide gating logic behind an LLM.
13. AI may assist users with explanation, extraction, or drafting, but must not silently change authoritative project status.
14. Build core interfaces against stable typed contracts.
15. Ask for clarification only when a decision materially changes legal/business behavior; otherwise follow this specification and existing repository conventions.

---

# 31. Recommended First Prototype

The first high-fidelity GDA prototype should include exactly these connected flows:

### Flow A — Create a project
Create project → jurisdiction + opportunity → initial fit → project workspace.

### Flow B — Readiness
Open project → see lifecycle/readiness → open requirement → upload/link evidence → assign reviewer → mark awaiting review → validator resolves requirement.

### Flow C — Blocked handoff
Project at high completion → handoff remains locked → UI explains critical missing legal/economic requirement.

### Flow D — Successful handoff
All applicable gates satisfied → GDA submits handoff → technical provider accepts → activation stage opens.

### Flow E — Technical rejection
Technical provider rejects or requests changes → project returns to actionable state without changing prior legal/compliance validations unless the change invalidates them.

These five flows are more valuable than building 30 disconnected polished screens.

---

# 32. Source References Reviewed

The following references were checked for the architectural/licensing conclusions in this document. Licensing may change; engineering must re-check before reuse.

1. Twenty repository license — https://github.com/twentyhq/twenty/blob/main/LICENSE
2. shadcn-admin — https://github.com/satnaing/shadcn-admin
3. Tremor — https://github.com/tremorlabs/tremor
4. Refine — https://github.com/refinedev/refine
5. Supabase — https://github.com/supabase/supabase
6. Supabase Studio README — https://github.com/supabase/supabase/blob/master/apps/studio/README.md
7. Appwrite Console — https://github.com/appwrite/console
8. Infisical — https://github.com/Infisical/infisical
9. Papermark — https://github.com/papermark/papermark
10. Midday — https://github.com/midday-ai/midday
11. Documenso — https://github.com/documenso/documenso

---

# 33. Final Architecture Position

GDA's portal should become a **readiness-centered institutional workflow platform**, not a reskinned open-source dashboard.

The implementation strategy is:

**Use permissive open-source code for commodity UI and platform acceleration.**  
**Use specialized products as UX references where their licensing or domain assumptions do not fit.**  
**Keep GDA's readiness engine, jurisdiction/instrument rules, evidence model, validation model, permissions, handoff logic, and product identity proprietary to GDA.**

The portal should make complex transactions easier to understand without pretending that complexity does not exist. A project owner should know what is missing. A professional should know what requires their validation. GDA should know what can move forward. A technical provider should receive a clean, controlled handoff. An investor should see only what has been approved for investor access.

That is the product architecture this blueprint is intended to enforce.
