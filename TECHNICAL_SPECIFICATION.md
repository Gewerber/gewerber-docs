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
- **Storage:** S3-compatible
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
- Offline support
- Responsive design

#### 📱 Flutter Mobile/Desktop
- Shared codebase
- Local persistence (Isar/Hive)
- Background timer service

---

### 2.2 Backend (Serverpod)

#### 🌐 Open Source Endpoints
- `auth` — JWT email/password sign-in, refresh
- `business` — business profile & settings (multi-tenant)
- `customer` — customer CRUD
- `invoice` — invoice CRUD, items, status (`draft`/`sent`/`paid`/`partiallyPaid`/`overdue`/`cancelled`)
- `invoiceTemplate` — reusable invoice templates
- `payment` — payment recording & payment status
- `document` — document upload
- `entitlement` — subscription feature gating

Future OSS modules (time tracking, basic accounting, guidance) will add `time`, `accounting`, and `guidance` endpoints.

#### 🔒 Closed Endpoints
Closed modules (banking/PSD2, tax/ELSTER, employees, subscriptions, AI assistant) are implemented in the private `gewerber-backend-commercial` repository (Serverpod module, nickname `commercial`) and are not part of the public codebase. Only a placeholder `commercial.status` health endpoint exists so far.

---

### 2.3 Database Schema (PostgreSQL)

#### 🌐 Entities (Open Source)
- User
- Business
- BusinessSettings
- Invoice
- InvoiceItem
- InvoiceTemplate
- PaymentRecord
- Customer
- Reminder
- Document

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

#### 🌐 Open Source (implemented in `gewerber-backend-core`)
- Invoice creation ✅
- PDF generation
- VAT/Kleinunternehmer §19 logic ✅
- Recurring invoices ✅
- Reminders (model) ✅
- Invoice templates ✅
- Payment recording & status ✅
- Export

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
- Tooltips
- Checklists
- Blog
- Tutorials

#### 🔒 Closed
- AI assistant
- Personalized tax guidance
- Automated compliance checks

---

## 4️⃣ Security
- JWT auth
- Encrypted fields
- GDPR compliance
- Audit logs
- Role‑based access control

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
