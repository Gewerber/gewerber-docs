# 🗺 Gewerber Roadmap

A long-term development plan for the Gewerber open-core platform.

The roadmap is divided into an **Open Source (OSS)** track — focused on core functionality, community adoption, and transparency — and a **Commercial** track for advanced modules.

> This is the **public, community-facing** roadmap and the source of truth for OSS deliverables. Internal business timelines and commercial strategy are maintained privately and are not published here.

---

## ✅ Current Status

- **Phase 1 / Core Platform (backend)** — done in `gewerber-backend-core`: Serverpod backend with JWT auth (email IdP), business profile & onboarding, multi-tenancy (tenant resolver), audit log, entitlement scaffold, mail service and GoBD-safe number sequences.
- **Phase 1 / Invoicing Core (backend)** — done: Customer & Invoice CRUD, invoice items, VAT/Kleinunternehmer §19 logic (tax rule engine), invoice templates, PDF generation, payment recording & status (`paid`/`partiallyPaid`), recurring invoices and reminders with scheduled background jobs (materialize recurring, mark overdue, send reminder emails via SMTP), CSV/JSON export.
- **Phase 1 / Time Tracking Core (backend)** — done: projects & tasks, timer, manual entries, rounding rules, reports and time-to-invoice billing.
- **Phase 1 / Basic Accounting Core (backend)** — done: income & expense transactions, categories, receipt upload (document storage), P&L report, CSV export.
- **Phase 1 / Guidance System (backend)** — done: tips, checklists and per-user progress.
- **Backend audit & GDPR hardening (2026‑08)** — passed a full security and quality audit: ownership checks on all business resources, invoice status guards, transactional payment recording with audit trail, upload validation, pagination caps and query optimizations. GDPR self-service shipped: account deletion with anonymization (Art. 17) and full data export (Art. 20).
- **Flutter app (`gewerber-app`)** — one codebase for web, mobile and desktop at `https://app.gewerber.de`, localized DE/EN/RU/TR; dashboard, auth & onboarding and settings are in place, and the UIs of all OSS modules are complete: responsive navigation shell, invoicing (template editor & prefill, recurring schedules, payment history), time-to-invoice billing incl. selected entries with € estimates, receipt upload & documents, transaction editing, guidance, profile management and account deletion with anonymization.
- **Marketing site (`gewerber-website`)** — Jaspr SSR site live at `https://gewerber.de` with blog, FAQ, pricing and this roadmap.
- **Commercial modules** — `gewerber-backend-commercial` repository scaffolded as a Serverpod module (`commercial` nickname) with a public waitlist; placeholder areas for banking (PSD2), tax (ELSTER) and advanced accounting. No commercial business logic implemented yet.

---

## 🌟 Vision

Gewerber aims to become the most friendly, modern, and modular platform for Einzelunternehmer, Kleingewerbe, and micro‑business owners in Germany.

The long-term vision includes:

- A fully modular Dart ecosystem (**Serverpod** + **Flutter** + **Jaspr**)
- A trusted open‑core foundation
- A powerful SaaS offering with advanced features
- A community-driven extension ecosystem
- A guidance-first UX that simplifies German bureaucracy

---

## 🎯 Guiding Principles

- **Open Core:** essential modules are open-source; advanced modules are commercial
- **Modularity:** every feature is a standalone package
- **Single-language stack:** Dart everywhere
- **Developer-first:** easy to extend, fork, and contribute
- **User-first:** simple, friendly, non‑bureaucratic UX
- **Compliance-ready:** German tax and business rules respected

---

## 📊 Roadmap Overview

The roadmap is divided into three phases. Each phase contains both OSS and Commercial tracks.

- **Phase 1** — OSS Core + MVP SaaS
- **Phase 2** — Growth & Commercial Modules
- **Phase 3** — Expansion & Automation

---

## 🟢 Phase 1 — OSS Core + MVP SaaS

### 🌐 Open Source Deliverables

#### 🏗 Core Platform
- Serverpod backend skeleton ✅
- Flutter Web app at `https://app.gewerber.de` ✅
- Jaspr marketing site at `https://gewerber.de` ✅
- Multi-language UI (DE/EN/RU/TR) ✅
- Basic auth & business profile ✅ (JWT auth, business profile, multi-tenancy)

#### 💰 Invoicing Core
- Invoice creation ✅
- PDF generation ✅
- Kleinunternehmer §19 logic ✅
- VAT logic ✅ (standard/reduced, reverse charge, zero)
- CSV/JSON export ✅
- Invoice templates ✅
- Recurring invoices ✅
- Payment recording & status ✅
- Reminders ✅ (scheduled sending via SMTP)

#### ⏱ Time Tracking Core
- Projects & tasks ✅
- Timer ✅
- Manual entries ✅
- Rounding rules ✅
- Reports ✅

#### 📊 Basic Accounting Core
- Income & expense tracking ✅
- Receipt upload ✅
- Categorization ✅
- Basic P&L ✅

#### 📖 Guidance System
- Tooltips ✅
- Checklists ✅
- **"What is this?"** popups
- Blog integration (planned)

#### 🎨 UI Kit
- Forms
- Tables
- Cards
- Layouts

### 🔒 Commercial Track
- Subscription system (Stripe, feature gating, Pro/Business tiers)

---

## 🟡 Phase 2 — Growth & Commercial Modules

### 🌐 Open Source Deliverables
- More invoice templates and accounting categories
- Membership management (invite/remove members, roles)
- Automatic dunning (Mahnung)
- Korrekturrechnung / credit notes
- E-invoicing support (XRechnung/ZUGFeRD) — B2B receiving already mandatory, issuing phased in 2027–2028 (mandatory from 2027 for businesses with annual revenue above €800,000; from 2028 for all)
- Time tracking analytics
- Multi‑business support (OSS)
- Community plugin system
- CLI tools and plugin architecture
- Improved documentation and example deployments

### 🔒 Commercial Track
- Banking module (PSD2, transaction import, reconciliation)
- Advanced accounting (VAT reporting, depreciation, ELSTER export)
- Mobile apps (iOS, Android)
- Chrome extension

---

## 🔵 Phase 3 — Expansion & Automation

### 🌐 Open Source Deliverables
- Advanced reporting
- Custom invoice templates
- Community-driven modules and localization
- Plugin marketplace (OSS)

### 🔒 Commercial Track
- Employees & payroll
- AI assistant
- Full ELSTER integration
- International expansion

---

## 👥 Community Roadmap (Open Source)

### 🤝 Community Contributions
- New invoice templates
- New accounting categories
- Localization (DE/EN/RU + others)
- UI kit components
- Documentation improvements
- Plugins and integrations

### 🏛 Community Governance
- RFC process
- Maintainer roles
- Community meetings
- Open discussions

---

## 🔢 Versioning Strategy

- **OSS Core:** Semantic Versioning (SemVer)
- **Commercial Modules:** SaaS versioning (feature-based)
- **Breaking changes:** documented via migration guides

---

## 📚 Related Documents

- [Technical Specification](TECHNICAL_SPECIFICATION.md)
- [Contributing Guide](https://github.com/Gewerber/.github/blob/main/CONTRIBUTING.md)
- [Code of Conduct](https://github.com/Gewerber/.github/blob/main/CODE_OF_CONDUCT.md)
- [License (MIT)](https://github.com/Gewerber/.github/blob/main/LICENSE.md)

---

## 💬 Feedback

To propose roadmap changes, open an issue or start a discussion.
