# Feature Scope — Hotel / Resort Staff Mobile App

**Product name (working):** Atrium Staff
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-13
**Status:** Draft v1.0
**Start here:** [Project Overview & Documentation Index](../overview.md)

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
- **Supervisor / department manager (line manager)** — assigns work, monitors SLAs, approves reassignments/escalations; builds/publishes rosters (F9); sees the department operational overview. See [Role-Based Design](staff-mobile-app-role-based-design.md).
- **Duty / general manager** — cross-department operational overview, escalations, and the owner/management KPI view (F11).
- **Owner / above-property** — reviews performance KPIs (F11); no operational task involvement.

---

## 2. Scope Summary

### 2.1 In scope
Task & work-order management (reactive), guest request handling, room status board, staff messaging, notifications, offline support, role-based access, audit logging.

### 2.2 Extended features (also in scope — see §3A)
Front-desk & reservations on mobile (F1/F2), F&B POS (F8), and the cost-and-control layer: **Labor Management (F9)**, **Preventive Maintenance & Assets (F10)**, **Owner KPIs (F11)**, **Inventory & Par-Stock (F12)**, **Inspections & Compliance (F13)**, **Guest Feedback (F14)**, **SOP/Training (F15)**, **Lost & Found + Safety (F16)**, the cross-cutting **NFC & Barcode Scanning (F17)** enabler for quick POS and inventory, and **Events, Activities & Banquet Management (F18)** with custom event-role crews. All are delivered as part of the one complete package.

### 2.3 Out of scope (entirely)
Guest-facing app, full HR/payroll, full procurement/purchasing, and IoT/BMS device control. (F9 covers scheduling/attendance but not payroll; F12 covers par-stock but not purchasing.) (See §9 Future.)

### 2.4 Assumptions
- An existing PMS (e.g., Opera, Cloudbeds, Mews) exposes an API or the app has a middleware sync layer.
- Property provides Wi-Fi coverage; app must degrade gracefully in dead zones.
- Staff use company or BYOD smartphones (iOS 15+ / Android 10+).

---

## 3. Feature Areas

### F1. Authentication & Access
- SSO / email + password login; optional PIN and biometric unlock.
- Role-based access control (RBAC) by department and seniority.
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

### F7. Supervisor / Manager Tools
- Live department view: staff status, open tasks, SLA health.
- Assign, reassign, and prioritize tasks; workload balancing.
- Approvals: escalated requests.

### F8. Offline Support
- Cached task lists, checklists, and room board available offline.
- Queue actions locally; auto-sync on reconnect with conflict resolution.
- Clear offline/sync status indicators.

### F9. Audit log (foundation)
- Activity/audit log per task (who did what, when).

> **Numbering note:** §3 above (F1–F9) lists the *capability groups* of the original release. §3A below lists the **product-area features F9–F18** using the same F-numbering scheme as the [Prioritization](staff-mobile-app-feature-prioritization.md), [Navigation Map](staff-mobile-app-navigation-map.md), and [Executive Summary](staff-mobile-app-executive-summary.md) docs (F1 Front Desk … F8 POS, then F9–F18; F7 is unused as a product area). The two schemes coexist for historical reasons; the product-area scheme is canonical across the newer docs.

---

## 3A. Extended feature areas (F9–F18)

These extend the product beyond the guest-facing operating loop (F1–F8): **F9–F13** attack **manual operational cost** and give ownership visibility; **F14–F16** cover experience, enablement, and safety; **F17** adds a cross-cutting scan enabler; **F18** adds events/activities & banquet operations. See [Prioritization](staff-mobile-app-feature-prioritization.md). Each has a full deep-dive doc (linked per feature below).

### F9. Labor Management (scheduling + attendance)
*Deep-dive: [Labor Management](staff-mobile-app-labor-management-feature.md).*
- Shift scheduling / roster builder; publish rosters to staff, shift-swap requests with manager approval.
- **Geofenced clock-in/out** and break tracking on the staff device; timesheet auto-generation.
- Roster-to-demand: suggest staffing from occupancy/arrivals forecast; overtime and coverage alerts.
- Feeds F3 housekeeping workload (credits/minutes) and the manager Overview team status.
- *Cost lever:* right-sizes labor, cuts overtime and buddy-punching, ends manual rotas/timesheets.

### F10. Preventive Maintenance & Asset Register
*Deep-dive: [Preventive Maintenance & Assets](staff-mobile-app-preventive-maintenance-assets-feature.md).*
- Asset register with location, warranty, and service history.
- Scheduled/recurring PM tasks (by time or meter reading); auto-generate work orders.
- Extends **F4** (reactive) with planned maintenance; PM and reactive share one work-order model.
- *Cost lever:* prevents costly failures and long OOO downtime; extends asset life.

### F11. Management Analytics / Owner KPIs
*Deep-dive: [Management Analytics / Owner KPIs](staff-mobile-app-management-analytics-owner-kpis-feature.md).*
- **GM/owner-only** performance view: labor-cost %, productivity (rooms/attendant-hour), SLA health, task throughput, occupancy & ancillary trend.
- Date-range and department filters; export/share for ownership.
- Distinct from the *operational* manager Overview (live status, no analytics) — this is business intelligence.
- *Cost lever:* makes waste visible and cuttable; gives ownership ("authority") the numbers to act.

### F12. Inventory & Par-Stock
*Deep-dive: [Inventory & Par-Stock](staff-mobile-app-inventory-par-stock-feature.md).*
- Par-level tracking for minibar, linen, guest amenities, and F&B consumables.
- Count on device; low-stock and reorder alerts; consumption logged from the room/outlet.
- Touchpoints in F3 (minibar restock) and F8 (F&B stock); lighter than full procurement (still out of scope).
- *Cost lever:* cuts shrinkage, waste, and emergency purchasing; recovers minibar charges.

### F13. Digital Inspections & Compliance
*Deep-dive: [Digital Inspections & Compliance](staff-mobile-app-inspections-compliance-feature.md).*
- Configurable inspection templates: H&S, food-safety/HACCP, fire/safety rounds, brand-standard audits.
- Scheduled and ad-hoc inspections with photo evidence, scoring, and pass/fail; corrective-action tasks auto-raised (F5).
- Generalizes F3's QA checklist engine to any compliance domain.
- *Cost lever:* replaces paper audit binders, automates compliance evidence, lowers liability/fine risk.

### F14. Guest Feedback & Service Recovery
*Deep-dive: [Guest Feedback & Service Recovery](staff-mobile-app-guest-feedback-service-recovery-feature.md).*
- In-stay feedback / NPS capture (staff-assisted or link); negative-signal alerts routed to the duty manager (F6).
- Complaint/glitch log with service-recovery workflow and resolution tracking.
- *Revenue lever:* catch and fix problems before checkout → fewer comps, stronger reviews, repeat/direct stays.

### F15. SOP / Training Knowledge Base
*Deep-dive: [SOP / Training Knowledge Base](staff-mobile-app-sop-training-knowledge-base-feature.md).*
- In-app searchable SOPs, how-to guides, and short training content by role/department.
- New-hire onboarding checklists; "learn on shift"; version-controlled content.
- *Cost lever:* cuts new-staff ramp time and trainer hours (high hospitality turnover); fewer errors.

### F16. Lost & Found & Staff Safety
*Deep-dive: [Lost & Found & Staff Safety](staff-mobile-app-lost-found-staff-safety-feature.md).*
- Digital lost-and-found register: log found items with photo/location, match to guest, track return/disposal.
- **Lone-worker safety:** SOS/panic button (global) for solo staff; alert with location to security/duty manager.
- *Cost lever:* ends paper logbooks; meets staff-safety compliance and reduces liability.

### F17. NFC & Barcode Scanning (cross-cutting enabler)
*Deep-dive: [NFC & Barcode Scanning](staff-mobile-app-nfc-barcode-scanning-feature.md).*
- One scan control (barcode / QR / NFC) reused across features: **scan-to-order** (quick POS, F8), **scan-to-count** (Inventory, F12), **scan-to-identify** assets/rooms (F10/F3/F4), **proof-of-presence** for rounds/PM/inspections.
- Tag/label provisioning (print QR/barcode, write NFC) + a code registry; offline scan resolution; manual-entry fallback.
- Surfaced *inside* other features' screens — not a tab of its own.
- *Cost/revenue lever:* speeds quick POS and inventory capture; cuts wrong-item and lookup errors.

### F18. Events, Activities & Banquet Management
*Deep-dive: [Events, Activities & Banquet Management](staff-mobile-app-events-activities-feature.md).*
- Create events/activities (weddings, banquets, parties, resort activities); **run-of-show / function sheet**; space/resource/AV booking with conflict detection.
- **Custom event-role** crews (Captain, Bar Lead, Setup, Host, Guide…) each with a per-role checklist — an **overlay on base RBAC**; crew assignment **writes back to the F9 roster** (an event within a shift adds no extra hours; only hours beyond it flag as overtime).
- Activity slots with capacity + assigned guide + folio-charged sign-ups (F8); live event-day board; F&B/AV charge roll-up.
- *Revenue lever:* captures high-margin event & activity ancillary revenue; cuts coordination and staffing chaos.

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
| Scalability | Single-property scope for v1; 1000s of concurrent staff |

---

## 5. Integrations

- **PMS** (Opera, Mews, Cloudbeds, etc.) — reservations, room status, guest profiles.
- **POS / F&B** — room service and charge posting (read/link only this release).
- **Maintenance / CMMS** — work-order sync, including preventive schedules & asset history (F10) (optional).
- **SSO / Identity** (Okta, Azure AD) — authentication.
- **Push** — APNs / FCM.
- **Analytics** — event tracking for adoption metrics; KPI/BI feed for the owner view (F11).
- **HR / T&A** (optional) — export timesheets from F9 to payroll; F9 does not run payroll itself.
- **Location** — device geofencing for clock-in/out (F9).

---

## 6. Key User Flows

1. **Housekeeping cleans a room** — receive assignment → open checklist → complete steps + photos → mark Clean → PMS updates → supervisor inspects.
2. **Guest calls front desk for extra towels** — agent logs request → auto-routes to housekeeping → attendant accepts → delivers → resolves → guest profile updated.
3. **Maintenance work order** — supervisor creates order → assigns tech → tech logs time/parts/photos → completes → room returns to service.
4. **SLA escalation** — request unaddressed past threshold → escalates to supervisor → reassigned → alert cleared.

---

## 7. Data Model (high level)
`User`, `Role`, `Property`, `Department`, `Shift`, `Room`, `Reservation/Guest`, `Task/WorkOrder`, `GuestRequest`, `Checklist`, `Message`, `Notification`, `AuditEvent`.

**Added by F9–F18:** `Roster/ShiftAssignment`, `TimeEntry` (F9); `Asset`, `PMSchedule` (F10); `KpiSnapshot` (F11); `InventoryItem`, `ParLevel`, `StockCount` (F12); `InspectionTemplate`, `Inspection` (F13); `Feedback/Glitch`, `RecoveryAction` (F14); `KbArticle` (F15); `LostFoundItem`, `SafetyAlert` (F16); `CodeTag` (barcode/QR/NFC → item/asset/room/bin registry), `ScanEvent` (F17); `Event/Activity`, `RunOfShow/EventSegment`, `EventRole` (custom crew role), `CrewAssignment` (staff↔event↔role overlay), `ActivitySlot`, `ActivityBooking` (F18).

---

## 8. Delivery

Atrium Staff is delivered as **one complete package** — all capability groups in §3 and all product-area features (F1–F18, including the extended features in §3A) ship together, not staged across releases. The **relative weighting** of each feature — which carry the most productivity, revenue, and cost impact — is in the [Prioritization](staff-mobile-app-feature-prioritization.md); how the features map onto the app shell is in the [Navigation Map](staff-mobile-app-navigation-map.md).

---

## 9. Future Considerations
Guest-facing companion app, IoT/BMS controls (HVAC, locks), AI task routing & demand-based auto-scheduling (building on F9), voice/hands-free mode, wearables, full procurement/purchasing (building on F12), full HR/payroll (building on F9), gamification and recognition.

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
