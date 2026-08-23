# TODO — gewerber-docs

Синхронизация публичной документации с фактическим состоянием кода (аудит 2026-08).
Правило: docs не должны ни отставать, ни забегать вперёд.

---

## TECHNICAL_SPECIFICATION.md

- [x] §1 «Storage: S3-compatible» → фактически DatabaseStorage; S3 помечен как planned
- [x] §4 «Encrypted fields» → помечен planned
- [x] §2.1 Flutter Web «Offline support» / Mobile «Isar/Hive» → помечены planned
- [x] §2.2 Endpoints: добавлены реализованные модули time_tracking, accounting, guidance,
      documents, userProfile (+ invoiceTemplate, recurringSchedule, reminder),
      пагинация listPage/listCursorPage, deleteMyAccount/exportMyData,
      поштучный timeEntry.createInvoice
- [x] §2.3 Entities: добавлены Project/Task/TimeEntry, AccountingTransaction,
      UserGuidanceProgress; пометка, что recurring — поля на invoice
- [x] §3.7 Guidance «Blog integration» → помечена planned (+ popups/tutorials planned)
- [x] Добавлен раздел 3.8 о E-Rechnung/XRechnung экспорте как планируемой OSS-фиче
      инвойсинга (приём B2B уже обязателен, выставление — 2027–2028)
- [x] §3.2: PDF generation и CSV/JSON export получили ✅ (давно реализованы),
      Reminders актуализированы (SMTP-отправка)

## ROADMAP.md

- [x] «Reminders (model) ✅» → «Reminders ✅ (scheduled sending via SMTP)»
      + уточнение в Current Status
- [x] Phase 1 Time Tracking / Accounting пункты уже были ✅ (подтверждено аудитом);
      Guidance: Blog integration → planned
- [x] Phase 2 OSS: добавлены membership management (invite/remove/roles),
      automatic dunning (Mahnung), Korrekturrechnung / credit notes;
      в строку E-invoicing добавлен контекст дедлайнов (2027–2028)

## Общее

- [x] После закрытия P0/P1 бэкенда обновлён «Current Status» вверху ROADMAP.md:
      UI всех модулей app готовы, аудит-харденинг (безопасность/GDPR/производительность)
- [ ] Проверять ссылки на репо при переименованиях (правило абсолютных URL соблюдено)
