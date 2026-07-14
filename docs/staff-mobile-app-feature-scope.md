# Feature Scope — Hotel / Resort Staff Mobile App

**Product name (working):** Atrium Staff
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-13
**Status:** Draft v1.0

---

## 1. Overview

### 1.1 Purpose
A mobile-first application that gives hotel and resort operational staff a single tool to receive work, communicate, and complete tasks from anywhere on property. It replaces radios, paper task lists, and desktop-bound PMS terminals for frontline and supervisory roles.

### 1.2 Problem statement
- Frontline staff (housekeeping, maintenance, F&B, front desk, guest services) lack real-time visibility into assignments and guest requests.
- Communication is fragmented across radios, WhatsApp, printed sheets, and desktop PMS.
- Managers have no live view of task status, staff location/availability, or SLA breaches.
- Guest requests get lost between departments, hurting satisfaction scores.

### 1.3 Goals
| Goal | Success metric |
|------|----------------|
| Faster guest request resolution | Median request-to-close time ↓ 40% |
| Fewer missed/dropped tasks | Overdue tasks ↓ 50% |
| Higher staff adoption | 80% of eligible staff active weekly within 90 days |
| Reduce radio/PMS terminal dependence | 60% of task actions performed in app |

### 1.4 Target users (personas)
- **Housekeeping attendant** — receives room-cleaning assignments, updates room status.
- **Maintenance / engineering tech** — receives work orders, logs parts and time.
- **Front desk / guest services agent** — creates and routes guest requests, checks room readiness.
- **F&B / room service staff** — receives orders and delivery tasks.
- **Supervisor / department manager** — assigns work, monitors SLAs, approves timesheets.
- **Duty / general manager** — cross-department dashboard, escalations.

---

## 2. Scope Summary

> **Scope decision (2026-07-14):** the "less necessary" features were **removed** (see [Feature Prioritization](staff-mobile-app-feature-prioritization.md)). Removed here: reporting/analytics/dashboards, shift/attendance, lost & found, and multi-property. Kept: task/work-order (reactive), guest requests, room board, messaging, notifications, offline, RBAC, and audit logging.

### 2.1 In scope (this release)
Task & work-order management (reactive), guest request handling, room status board, staff messaging, notifications, offline support, role-based access, audit logging.

### 2.2 Out of scope (this release)
Guest-facing features, full HR/payroll, procurement/inventory purchasing, POS transaction processing, IoT/BMS device control, **and (removed 2026-07-14): reporting/analytics/dashboards, shift/attendance, lost & found, concierge tasks, preventive maintenance & asset history, and multi-property/cluster support.** (See §9 Future.)

### 2.3 Assumptions
- An existing PMS (e.g., Opera, Cloudbeds, Mews) exposes an API or the app has a middleware sync layer.
- Property provides Wi-Fi coverage; app must degrade gracefully in dead zones.
- Staff use company or BYOD smartphones (iOS 15+ / Android 10+).

---

## 3. Feature Areas

### F1. Authentication & Access
- SSO / email + password login; optional PIN and biometric unlock.
- Role-based access control (RBAC) by department and seniority.
- Multi-property support (staff assigned to one or more properties).
- Auto-logout on inactivity; remote session revoke by admin.

### F2. Task & Work-Order Management
- Personal task list (Today / Upcoming / Overdue) with priority and SLA countdown.
- Task types: room cleaning, inspection, maintenance work order, guest request, delivery, custom.
- Task detail: location (room/area), instructions, photos, checklist, linked guest.
- Actions: accept, start, pause, complete, reassign, add note/photo, flag issue.
- Checklists with required steps and photo evidence.
- Auto and manual assignment; supervisors bulk-assign and rebalance.
- SLA timers with escalation when breached.

### F3. Guest Request Handling
- Create request on behalf of guest (linked to room/reservation).
- Route to correct department automatically by request type.
- Track lifecycle: New → Assigned → In progress → Resolved → Verified.
- Guest preferences / VIP flags surfaced to staff.
- Recurring/standing requests (e.g., extra towels daily).

### F4. Room Status Board (Housekeeping)
- Live room grid: Dirty / Clean / Inspected / Out of Order / Do Not Disturb / Occupied / Vacant.
- Update status from device; two-way sync with PMS.
- Turndown, checkout-priority, and stay-over views.

### F5. Communication & Messaging
- 1:1 and group chat by department, shift, or property.
- Broadcast announcements from managers (read receipts).
- Attach photos, tasks, and room references in messages.
- Quick canned responses and translations for multilingual teams.

### F6. Notifications & Alerts
- Push notifications for new assignments, escalations, SLA warnings, broadcasts.
- Configurable per-user notification preferences and quiet hours.
- Critical alerts (emergency, VIP arrival) that bypass Do Not Disturb.

### F7. Shift, Attendance & Availability — ❌ REMOVED FROM SCOPE
*(Removed 2026-07-14. Availability/break status may return later; not built in v1.)*

### F8. Supervisor / Manager Tools
- Live department **view**: staff status, open tasks, SLA health *(operational monitoring, not analytics/reporting)*.
- Assign, reassign, and prioritize tasks; workload balancing.
- Approvals: escalated requests. *(Timesheet/swap approvals removed with shift/attendance.)*
- *(Reporting/daily-summary exports removed from scope.)*

### F9. Offline Support
- Cached task lists, checklists, and room board available offline.
- Queue actions locally; auto-sync on reconnect with conflict resolution.
- Clear offline/sync status indicators.

### F10. Audit log (foundation)
- Activity/audit log per task (who did what, when) — a shared foundation, kept.
- *(Reporting: task-completion / SLA / response-time metrics and exportable summaries were **removed from scope** with F7 Dashboards & Reporting.)*

---

## 4. Non-Functional Requirements

| Category | Requirement |
|----------|-------------|
| Platforms | iOS 15+, Android 10+ (native or React Native/Flutter) |
| Performance | Task list loads < 2s on 4G; actions feel instant (optimistic UI) |
| Offline | Core read + action queueing without connectivity |
| Reliability | 99.9% backend uptime; no data loss on sync conflicts |
| Security | Encrypted at rest & in transit (TLS 1.2+), RBAC, audit logging |
| Privacy | Guest PII minimized and access-logged; GDPR/CCPA aligned |
| Accessibility | WCAG 2.1 AA where applicable; large-tap targets for gloved/on-the-go use |
| Localization | Multi-language UI; RTL support |
| Scalability | Single-property scope for v1 (multi-property/cluster out of scope); 1000s of concurrent staff |

---

## 5. Integrations

- **PMS** (Opera, Mews, Cloudbeds, etc.) — reservations, room status, guest profiles.
- **POS / F&B** — room service and charge posting (read/link only this release).
- **Maintenance / CMMS** — work-order sync (optional).
- **SSO / Identity** (Okta, Azure AD) — authentication.
- **Push** — APNs / FCM.
- **Analytics** — event tracking for adoption metrics.

---

## 6. Key User Flows

1. **Housekeeping cleans a room** — receive assignment → open checklist → complete steps + photos → mark Clean → PMS updates → supervisor inspects.
2. **Guest calls front desk for extra towels** — agent logs request → auto-routes to housekeeping → attendant accepts → delivers → resolves → guest profile updated.
3. **Maintenance work order** — supervisor creates order → assigns tech → tech logs time/parts/photos → completes → room returns to service.
4. **SLA escalation** — request unaddressed past threshold → escalates to supervisor → reassigned → alert cleared.

---

## 7. Data Model (high level)
`User`, `Role`, `Property`, `Department`, `Shift`, `Room`, `Reservation/Guest`, `Task/WorkOrder`, `GuestRequest`, `Checklist`, `Message`, `Notification`, `AuditEvent`.

---

## 8. Release Plan (phased)

| Phase | Scope |
|-------|-------|
| **MVP** | Auth/RBAC, Task management (F2), Guest requests (F3), Room board (F4), Push (F6), basic offline (F9) |
| **Phase 2** | Messaging (F5), Supervisor tools (F8, monitoring + assignment) |
| **Phase 3** | Advanced offline conflict handling, multilingual, deeper PMS/POS integration |

---

## 9. Future Considerations
Guest-facing companion app, IoT/BMS controls (HVAC, locks), predictive scheduling/AI task routing, voice/hands-free mode, wearables, inventory & procurement, full HR/payroll, gamification and recognition.

---

## 10. Open Questions
- Which PMS(es) must be supported at launch, and is a real-time API available?
- BYOD vs. company-issued devices — MDM requirements?
- Is geofenced clock-in acceptable to staff/labor agreements?
- Single property or chain rollout for v1?
- Data residency / compliance regions?

---

## 11. Risks
| Risk | Mitigation |
|------|------------|
| Poor Wi-Fi coverage on property | Strong offline mode; sync indicators |
| Low staff adoption | Simple UX, multilingual, on-shift training, manager buy-in |
| PMS integration limits | Middleware sync layer; graceful fallback |
| Guest data exposure | Minimize PII, RBAC, audit trails |
