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
- `auth_endpoint.dart`
- `business_endpoint.dart`
- `invoice_endpoint.dart`
- `time_endpoint.dart`
- `accounting_endpoint.dart`
- `guidance_endpoint.dart`
- `document_endpoint.dart`

#### 🔒 Closed Endpoints
Closed modules (banking, tax/ELSTER, employees, subscriptions, AI assistant) are implemented in private repositories and are not part of the public codebase.

---

### 2.3 Database Schema (PostgreSQL)

#### 🌐 Entities (Open Source)
- User
- Business
- Invoice
- Client
- TimeEntry
- Project
- Expense
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

#### 🌐 Open Source
- Invoice creation
- PDF generation
- VAT/Kleinunternehmer logic
- Recurring invoices
- Reminders
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
**🔒 Closed** — PSD2 banking integration, transaction import, and reconciliation. Implemented in a private repository; see the [Roadmap](ROADMAP.md) for the public overview.

---

### 3.6 Employees Module
**🔒 Closed** — employee profiles, per-employee time tracking, and payroll export. Implemented in a private repository; see the [Roadmap](ROADMAP.md) for the public overview.

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
