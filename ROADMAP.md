# 🗺 Gewerber Roadmap

A long-term development plan for the Gewerber open-core platform.

The roadmap is divided into an **Open Source (OSS)** track — focused on core functionality, community adoption, and transparency — and a **Commercial** track for advanced modules.

> This is the **public, community-facing** roadmap and the source of truth for OSS deliverables. Internal business timelines and commercial strategy are maintained privately and are not published here.

---

## ✅ Current Status

- **Phase 1 / Invoicing Core (backend)** — implemented in `gewerber-backend-core`: Customer & Invoice CRUD, invoice items, VAT/Kleinunternehmer §19 logic, GoBD-compliant invoice numbering, invoice templates, payment recording & status (`paid`/`partiallyPaid`), recurring invoices, reminders model, migration and integration tests.
- **Phase 1 / Core Platform (backend)** — Serverpod backend running with auth (JWT), business profile, and multi-tenancy (tenant resolver).
- **Commercial modules** — `gewerber-backend-commercial` repository scaffolded as a Serverpod module (`commercial` nickname) with placeholder areas for banking (PSD2), tax (ELSTER), and advanced accounting. No commercial business logic implemented yet.

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
- Flutter Web app at `https://app.gewerber.de`
- Jaspr marketing site at `https://gewerber.de`
- Multi‑language (EN/RU/DE)
- Basic auth & business profile ✅ (JWT auth, business profile, multi-tenancy)

#### 💰 Invoicing Core
- Invoice creation ✅
- PDF generation
- Kleinunternehmer §19 logic ✅
- VAT logic ✅ (standard/reduced, reverse charge, zero)
- CSV/JSON export
- Invoice templates ✅
- Recurring invoices ✅
- Payment recording & status ✅
- Reminders (model) ✅

#### ⏱ Time Tracking Core
- Projects & tasks
- Timer
- Manual entries
- Rounding rules
- Reports

#### 📊 Basic Accounting Core
- Income & expense tracking
- Receipt upload
- Categorization
- Basic P&L

#### 📖 Guidance System
- Tooltips
- Checklists
- **"What is this?"** popups
- Blog integration

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
