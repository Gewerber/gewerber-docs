# 🗺 Gewerber Roadmap

A long-term development plan for the Gewerber open-core platform.

The roadmap covers the **Open Source (OSS)** track — core functionality, community adoption, and transparency.

> This is the **public, community-facing** roadmap and the source of truth for OSS deliverables. Internal business timelines and commercial strategy are maintained privately and are not published here.

---

## ✅ Current Status

- **Phase 1 / Core Platform (backend)** — done in `gewerber-backend`: Serverpod backend with JWT auth (email IdP), business profile & onboarding, multi-tenancy (tenant resolver), audit log, entitlement scaffold, mail service and GoBD-safe number sequences.
- **Phase 1 / Invoicing Core (backend)** — done: Customer & Invoice CRUD, invoice items, VAT/Kleinunternehmer §19 logic (tax rule engine), invoice templates, PDF generation, payment recording & status (`paid`/`partiallyPaid`), recurring invoices and reminders (background jobs materialize recurring invoices and mark overdue; reminder emails sent on demand via SMTP), CSV/JSON export.
- **Phase 1 / Time Tracking Core (backend)** — done: projects & tasks, timer, manual entries, rounding rules, reports and time-to-invoice billing.
- **Phase 1 / Basic Accounting Core (backend)** — done: income & expense transactions, categories, receipt upload (document storage), P&L report, CSV export.
- **Phase 1 / Guidance System (backend)** — done: tips, checklists and per-user progress.
- **Backend audit & GDPR hardening (2026‑08)** — passed a full security and quality audit: ownership checks on all business resources, invoice status guards, transactional payment recording with audit trail, upload validation, pagination caps and query optimizations. GDPR self-service shipped: account deletion with anonymization (Art. 17) and full data export (Art. 20).
- **Flutter app (`gewerber-app`)** — one codebase for web, mobile and desktop at `https://app.gewerber.de`, localized DE/EN/RU/TR; dashboard (v2: KPIs, monthly trends, recent activity and receivables via the server-side `dashboard.getSummary` endpoint), auth & onboarding and settings are in place, and the UIs of all OSS modules are complete: responsive navigation shell, invoicing (template editor & prefill, recurring schedules, payment history), time-to-invoice billing incl. selected entries with € estimates, receipt upload & documents, transaction editing, guidance, profile management and account deletion with anonymization. Android packaging groundwork is in place; Play Store internal testing has not launched yet.
- **Marketing site (`gewerber-website`)** — Jaspr SSR site live at `https://gewerber.de` with blog, FAQ, pricing and this roadmap.
- **Open-core restructure (2026‑08)** — complete: OSS builds resolve the commercial Serverpod module against the public placeholder packages in `gewerber-backend-stubs`; insiders override locally via gitignored `pubspec_overrides.yaml`, and release CI/Docker injects the real private module with a token.
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

The roadmap is divided into three phases of open-source deliverables.

- **Phase 1** — OSS Core + MVP SaaS
- **Phase 2** — Growth & Maturity
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
- Reminders ✅ (on-demand sending via SMTP)

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

---

## 🟡 Phase 2 — Growth & Maturity

### 🌐 Open Source Deliverables
- More invoice templates and accounting categories
- Membership management (invite/remove members, roles)
- Automatic dunning (Mahnung)
- Korrekturrechnung / credit notes
- E-invoicing support (XRechnung/ZUGFeRD) — receiving structured e-invoices already mandatory; issuing obligation from 2027 for businesses whose **prior-year total turnover** exceeded €800,000, from 2028 for all (exemptions: Kleinunternehmer under §19 UStG, invoices ≤ €250 under §34a UStDV); accepted formats must be **EN 16931**-conformant — XRechnung and ZUGFeRD are the common German syntaxes, Peppol-BIS and Factur-X are also accepted
- Time tracking analytics
- Multi‑business support (OSS)
- Community plugin system
- CLI tools and plugin architecture
- Improved documentation and example deployments

---

## 🔵 Phase 3 — Expansion & Automation

### 🌐 Open Source Deliverables
- Advanced reporting
- Custom invoice templates
- Community-driven modules and localization
- Plugin marketplace (OSS)

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
