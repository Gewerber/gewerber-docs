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
- **Closed Modules:** banking, tax/ELSTER, employees, subscriptions, AI assistant

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
- `auth` / `userProfile` — JWT email/password sign-in, refresh, profile management, email verification (8-digit codes)
- `business` / `businessSettings` — business profile & settings (multi-tenant)
- `entitlement` — subscription feature gating

*Invoicing*
- `customer` — customer CRUD with offset-based (`listPage`, incl. total count) and keyset cursor (`listCursorPage`) pagination
- `invoice` — invoice CRUD, items, status workflow (`draft`/`sent`/`paid`/`partiallyPaid`/`overdue`/`cancelled`; draft-only editing, deletion restricted to drafts/cancelled), PDF generation, CSV/JSON export, same pagination options
- `invoiceTemplate` — reusable invoice templates (editor + invoice prefill)
- `payment` — payment recording & payment status (transactional, overpayments rejected)
- `recurringSchedule` — create/get/list/update/cancel recurring invoice schedules
- `reminder` — reminders with scheduled sending via SMTP
- `document` — document upload (MIME type & extension whitelist)

*Time tracking*
- `project` / `task` / `timeEntry` — projects & tasks, timer, manual entries, rounding rules, reports; time-to-invoice via `timeEntry.createInvoice` (whole project or selected entries)

*Accounting*
- `accounting` — income/expense transactions (editable), categories, receipt upload, P&L report, CSV export

*Guidance*
- `guidance` — tips, checklists and per-user progress

*GDPR self-service*
- `userProfile.deleteMyAccount` — account deletion with anonymization of personal references (GDPR Art. 17)
- `userProfile.exportMyData` — full data export as a downloadable archive (GDPR Art. 20)

#### 🔒 Closed Endpoints
Closed modules (banking/PSD2, tax/ELSTER, employees, subscriptions, AI assistant) are implemented in the private `gewerber-backend-commercial` repository (Serverpod module, nickname `commercial`) and are not part of the public codebase. Implemented so far: a placeholder `commercial.status` health endpoint and the public `waitlist.join` endpoint used by the marketing site. OSS builds resolve the module against the public stub packages in `gewerber-backend--stubs` (identical API surface, no business logic); the real module is injected locally via gitignored `pubspec_overrides.yaml` and in release builds via token. Closed app features follow the same pattern through the `AppFeature` contract of `gewerber-app` and are composed in the private `gewerber-app-commercial` repository.

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
- Reminders ✅ (scheduled sending via SMTP)
- Invoice templates ✅
- Payment recording & status ✅
- CSV/JSON export ✅
- Pagination (offset & keyset cursor) ✅

#### 🔒 Closed
- Online payments (Stripe)
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
- Receipt upload
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
