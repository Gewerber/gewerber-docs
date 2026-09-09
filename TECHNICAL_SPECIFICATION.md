# 🏗 Gewerber — Technical Specification

Dart‑first open‑core architecture using Serverpod, Flutter, and Jaspr.

---

## 1️⃣ Architecture Overview

Gewerber uses a **single‑language Dart stack**:

- **Backend:** Serverpod
- **Web App:** Flutter Web (`https://app.gewerber.de`)
- **Mobile/Desktop:** Flutter
- **Marketing Site:** Jaspr (`https://gewerber.de`)
- **Database:** PostgreSQL
- **Storage:** Database-backed document storage (S3-compatible storage planned)
- **Open Core:** OSS modules
- **Closed Modules:** banking, tax/ELSTER, employees, subscriptions, AI assistant, multi-currency invoicing, advanced accounting

---

## 2️⃣ System Architecture

### 2.1 Frontend

#### 🌐 Jaspr (Marketing Site)
- SSR for SEO
- Static blog pages
- Hosted at `https://gewerber.de`

#### 🌐 Flutter Web (Application)
- Hosted at `https://app.gewerber.de`
- Modular UI packages
- Offline support *(planned)*
- Responsive design

#### 📱 Flutter Mobile/Desktop
- Shared codebase
- Local persistence (Isar/Hive) *(planned)*
- Background timer service

---

### 2.2 Backend (Serverpod)

#### 🌐 Open Source Endpoints

*Core platform & user*
- `auth` / `userProfile` — JWT email/password sign-in, refresh, profile management, email verification (8-digit codes), identity discovery (`userProfile.me` returns the caller's global admin role (if any) and business memberships)
- `business` / `businessSettings` — business profile & settings (multi-tenant)
- `entitlement` — subscription feature gating

*Invoicing*
- `customer` — customer CRUD with offset-based (`listPage`, incl. total count) and keyset cursor (`listCursorPage`) pagination
- `invoice` — invoice CRUD, items, status workflow (`draft`/`sent`/`paid`/`partiallyPaid`/`overdue`/`cancelled`; draft-only editing, deletion restricted to drafts/cancelled), PDF generation, CSV/JSON export, same pagination options
- `invoiceTemplate` — reusable invoice templates (editor + invoice prefill)
- `payment` — payment recording & payment status (transactional, overpayments rejected)
- `recurringSchedule` — create/get/list/update/cancel recurring invoice schedules
- `reminder` — reminders listed via `reminder.list` and sent synchronously on demand via `reminder.send` (SMTP delivery)
- `document` — document upload (MIME type & extension whitelist)

*Time tracking*
- `project` / `task` / `timeEntry` — projects & tasks, timer, manual entries, rounding rules, reports; time-to-invoice via `timeEntry.createInvoice` (whole project or selected entries)

*Accounting*
- `accounting` — income/expense transactions (editable), categories, receipt upload, P&L report, CSV export

*Dashboard*
- `dashboard.getSummary` — aggregated dashboard summary in a single request: current-month KPIs (income/expense/profit, tracked-time minutes incl. rounding rules from BusinessSettings), monthly income/expense/profit trend (`trendMonths` 1–12, default 6), recent invoices/transactions/time entries feeds (`recentLimit` ≤ 50, default 5; project/task names resolved server-side) and a receivables summary (open & overdue invoice counts/totals, top debtors (`debtorLimit` ≤ 50, default 10), overdue invoice list (`overdueLimit` ≤ 100, default 20)); returns `DashboardSummary` (`generatedAt`, `asOf`, `trendFrom`/`trendTo`, `kpis`, `monthlyTrend`, recent lists, `receivables` with `debtors` and `overdueInvoices`); money values are integer cents (`*Cents`; `remaining` = max(0, total − payments)); read-only — `requireLogin`, member role sufficient, tenant-scoped via `TenantResolver` (foreign `businessId` → `ForbiddenException`); v1 semantics: month buckets in UTC, half-open trend windows `[monthStart, nextMonthStart)`, open invoices = status `sent`/`partiallyPaid`/`overdue`, credit notes excluded (compensation is a planned follow-up), open-invoice scan capped at the 500 oldest by due date, optional `asOf` parameter (default: now) as a test escape hatch; table-less DTOs only (no database migration), implemented in `modules/dashboard/` with ~12 constant indexed queries and no N+1

*Guidance*
- `guidance` — tips, checklists and per-user progress

*GDPR self-service*
- `userProfile.deleteMyAccount` — account deletion with anonymization of personal references (GDPR Art. 17)
- `userProfile.exportMyData` — full data export as a downloadable archive (GDPR Art. 20)

*Administration*
- `adminStats` / `adminUsers` / `adminBusinesses` / `adminInvoices` / `adminAudit` / `adminGuidance` — global administration surface consumed by the open-source `gewerber-mcp` integration tooling (see Admin API below)

#### 🔒 Closed Endpoints
Closed modules (banking/PSD2, tax/ELSTER, employees, subscriptions, AI assistant) are implemented in the private `gewerber-backend-commercial` repository (Serverpod module, nickname `commercial`) and are not part of the public codebase. Implemented so far: the public `waitlist.join` endpoint used by the marketing site and a closed subscription & billing module (feature plans, checkout, promo codes — details intentionally out of scope here). OSS builds resolve the module against the public stub packages in `gewerber-backend-stubs` (identical API surface, no business logic); the real module is injected locally via gitignored `pubspec_overrides.yaml` and in release builds via token. Closed app features follow the same pattern through the `AppFeature` contract of `gewerber-app` and are composed in the private `gewerber-app-commercial` repository.

#### 🛡️ Admin API

The `modules/admin` endpoints — `adminStats`, `adminUsers`, `adminBusinesses`, `adminInvoices`, `adminAudit`, `adminGuidance` — form a global administration surface consumed by the open-source [`gewerber-mcp`](https://github.com/Gewerber/gewerber-mcp) server — an MCP integration surface positioned as open integration tooling (**not** an AI assistant) that serves platform staff through these admin endpoints (including a generic commercial-administration surface). The MCP server operates purely through these Serverpod endpoints; it has **no direct database access**. Admin authorization is independent of business membership.

*Role model*
- Global `admin_user` allowlist table with two roles: `moderator` (read-only) < `admin` (may mutate).
- The caller's role is resolved from the database on **every request** (`AdminRoleResolver` behind the shared `AdminEndpoint.requireAdmin` base guard); unauthenticated, non-admin or below-minimum-role calls fail with a typed `ForbiddenException` — revocation therefore takes effect immediately.
- Role management is out-of-band by design: since only admins could manage admins through the API, the first role is bootstrapped with [`grant_admin.sql`](https://github.com/Gewerber/gewerber-backend/blob/main/gewerber_backend_server/tool/grant_admin.sql) — registered users only, upsert per user (re-running promotes/demotes), default role `moderator`, audited as `admin.roleGranted` / `admin.roleChanged` with actor NULL. There are no grant/revoke endpoints.

*Endpoints*

| Endpoint | Method | Key parameters | Min role |
|---|---|---|---|
| `adminStats` | `statsOverview` | platform-wide counters | moderator |
| `adminUsers` | `usersSearch` | `query` (email substring), `limit`, `cursor` | moderator |
| | `usersGet` | `userId` — full dossier (profile, memberships, auth status) | moderator |
| | `usersVerifyEmail` | `userId` — read-only verification-state compliance check (audited) | admin |
| | `usersBan` | `userId`, required `reason`, **`confirm`** | admin |
| | `usersUnban` | `userId`, **`confirm`** | admin |
| `adminBusinesses` | `businessesSearch` | `query` (name substring), `limit`, `cursor` | moderator |
| | `businessesGet` | `businessId` — incl. all memberships | moderator |
| | `membershipsSetRole` | `membershipId`, `role`, **`confirm`** | admin |
| `adminInvoices` | `invoicesList` | optional filters `businessId`, `status`, inclusive date range `from`/`to`; `limit`, `cursor` | moderator |
| | `invoicesGet` | `invoiceId` | moderator |
| | `invoiceCancelAdmin` | `invoiceId`, required `reason`, **`confirm`** | admin |
| `adminAudit` | `auditQuery` | optional `actorUserId`, exact-match `action`, `since`, `limit` | moderator |
| `adminGuidance` | `guidanceTipsList` | effective tips (curated content + admin overrides) | moderator |
| | `guidanceTipUpsert` | unique `topic`, `title`, `body`, **`confirm`** | admin |

All mutations require the explicit `confirm: true` flag (missing/false → typed `ValidationException`); bans and invoice cancellation additionally require a non-empty `reason`, which is stored in the audit trail. `invoiceCancelAdmin` cancels only open invoices (`sent`/`partiallyPaid`/`overdue`) — drafts belong to their owners and paid invoices are immutable (GoBD), so both are rejected.

*Safety guarantees*
- **Confirm guard** — destructive methods take an explicit `confirm` parameter; the MCP client must send `confirm: true` deliberately.
- **Audit trail** — mutations record an `audit_entry` row with action prefix `admin.*` and the acting admin as actor; membership-role changes and admin invoice cancellations commit the audit entry **transactionally** with the change, while ban/unban and guidance tip upsert commit it in a separate step (cross-module atomicity not available — see Known limitations).
- **Atomic last-owner guard** — demoting the last owner of a business is refused (typed conflict error).
- **Bans are reversible flags** — banning blocks the user on the auth level (`AuthUser.blocked`) and purges refresh tokens immediately; no user data is deleted, and unban restores sign-in.
- **Typed errors** — failures throw generated serializable exceptions (`NotFound`, `Validation`, `Forbidden`, `Conflict`) that clients catch by type.
- **Keyset pagination** — search/list methods use opaque cursors, default limit 50 and hard cap 200 rows per page; `auditQuery` is the exception: it has no opaque cursor — it pages newest-first via the `since` timestamp of the oldest returned entry, filtered inclusively (`>=`), so the boundary entry repeats on the next page and clients must skip already-seen rows.

*Known limitations*
- Access JWTs issued before a ban remain valid until they expire; refresh tokens are revoked, so affected sessions cannot be renewed.
- Ban/unban side effects live in the auth module: block flag, token purge/connection revocation and the audit entry commit separately rather than in one cross-module transaction — documented residual risk; guidance tip upsert likewise commits its audit entry in a separate step for the same reason.

---

### 2.3 Database Schema (PostgreSQL)

#### 🌐 Entities (Open Source)
- User
- Business
- BusinessSettings
- Invoice *(recurring schedules are modelled as fields on the invoice)*
- InvoiceItem
- InvoiceTemplate
- PaymentRecord
- Customer
- Reminder
- Document
- Project / Task / TimeEntry
- AccountingTransaction
- UserGuidanceProgress
- Dashboard *(read-only summary DTOs, no database tables)*: DashboardSummary, MonthlyTrendPoint, DashboardKpis, RecentTimeEntry, ReceivablesSummary, DebtorSummary

#### 🔒 Entities (Closed)
Data models for closed modules live in private repositories and are not part of the public schema.

---

## 3️⃣ Module Architecture

### 3.1 User & Business Module
- Auth (JWT)
- Business profile
- Multi‑language
- Multi‑business (future)

---

### 3.2 Invoicing Module

#### 🌐 Open Source (implemented in `gewerber-backend`)
- Invoice creation ✅
- PDF generation ✅
- VAT/Kleinunternehmer §19 logic ✅
- Recurring invoices ✅
- Reminders ✅ (on-demand sending via SMTP)
- Invoice templates ✅
- Payment recording & status ✅
- CSV/JSON export ✅
- Pagination (offset & keyset cursor) ✅

#### 🔒 Closed
- Online payments
- Multi-currency invoicing
- Banking reconciliation

---

### 3.3 Time Tracking Module
- Projects & tasks
- Timer
- Manual entries
- Rounding rules
- Reports
- Convert to invoice

---

### 3.4 Accounting Module

#### 🌐 Open Source
- Income/expense tracking
- Receipt linking (files uploaded via the document module, linked by `receiptDocumentId`)
- Categorization
- Basic P&L
- Export

#### 🔒 Closed
- ELSTER API
- Automated tax estimation
- VAT reporting
- Depreciation

---

### 3.5 Banking Module
**🔒 Closed** — PSD2 banking integration, transaction import, and reconciliation. Implemented in the private `gewerber-backend-commercial` repository; see the [Roadmap](ROADMAP.md) for the public overview.

---

### 3.6 Employees Module
**🔒 Closed** — employee profiles, per-employee time tracking, and payroll export. Implemented in the private `gewerber-backend-commercial` repository; see the [Roadmap](ROADMAP.md) for the public overview.

---

### 3.7 Guidance Module

#### 🌐 Open Source
- Tooltips ✅
- Checklists ✅
- **"What is this?"** popups *(planned)*
- Blog integration *(planned)*
- Tutorials *(planned)*

#### 🔒 Closed
- AI assistant
- Personalized tax guidance
- Automated compliance checks

---

### 3.8 E-Invoicing (E-Rechnung) — Planned OSS Feature

Structured e-invoicing is being phased in for German B2B by law: businesses must already be able to **receive** structured e-invoices, while the obligation to **issue** them applies from 2027–2028 depending on prior-year turnover.

Gewerber will add **XRechnung/ZUGFeRD e-invoice export** to the open-source invoicing module, built on the existing invoice data model (items, VAT rates, Kleinunternehmer §19 handling), so self-hosted users can stay compliant without a commercial tier.

- Planned deliverable: standards-compliant export (XRechnung CII, ZUGFeRD hybrid PDF/XML) from invoices created in Gewerber
- Timeline aligned with the statutory issuing deadlines (2027–2028); see the [Roadmap](ROADMAP.md) (Phase 2)
- E-invoice receiving/validation and tax-filing integrations are out of scope for the OSS core (see closed modules)

---

## 4️⃣ Security
- JWT auth
- Resource ownership checks on all business data (IDOR protection)
- Invoice status guards (draft-only editing, restricted deletion)
- Upload validation (MIME type & extension whitelist)
- GDPR compliance — self-service account deletion with anonymization (Art. 17) and full data export (Art. 20)
- Audit logs written transactionally alongside the business changes they record
- Role‑based access control
- Encrypted fields *(planned)*

---

## 5️⃣ Open Core Strategy

### 🌐 Open Source
- Core platform
- Invoicing (no payments)
- Time tracking
- Basic accounting
- Guidance
- UI kit

### 🔒 Closed
- Banking
- Tax/ELSTER
- Employees
- Subscriptions
- AI assistant
- Multi-currency invoicing
- Advanced accounting

---

## 6️⃣ Deployment

### ☁️ SaaS Deployment
- Multi‑tenant
- Feature gating via subscription flags
- Hosted at `https://app.gewerber.de`

### 🐳 OSS Deployment
- Single‑tenant
- Docker compose templates
- Community‑maintained

---

## 7️⃣ Roadmap

See the [Roadmap](ROADMAP.md) for development phases and deliverables.

---

## 📚 Related Documents

- [Roadmap](ROADMAP.md)
- [Contributing Guide](https://github.com/Gewerber/.github/blob/main/CONTRIBUTING.md)
- [Organization Structure](https://github.com/Gewerber/.github/blob/main/ORGANIZATION.md)
- [License (MIT)](https://github.com/Gewerber/.github/blob/main/LICENSE.md)
