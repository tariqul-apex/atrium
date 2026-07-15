# Task Management & Internal Communication on Mobile — Feature Deep-Dive

**Product:** Atrium Staff (Hotel / Resort Operations Mobile App)
**Feature area:** F5 — Mobile Task Management & Internal Communication
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-14
**Status:** Draft v1.1
**Related:** [Staff Mobile App — Feature Scope](staff-mobile-app-feature-scope.md) · [Maintenance & Work Orders — Deep-Dive](staff-mobile-app-maintenance-work-orders-feature.md) · [Competitive Feature Research](../HOTEL-~1.MD)

---

## 1. Executive Summary

**Mobile Task Management & Internal Communication** is the connective tissue of the whole staff app — the layer that routes work between departments, carries messages between back-office and field staff, and gives supervisors a live view of who is doing what. It replaces radios, WhatsApp groups, printed task sheets, and shouted handoffs with a single, logged, cross-department system.

Every other feature area — [front desk](staff-mobile-app-front-desk-feature.md), [housekeeping](staff-mobile-app-housekeeping-management-feature.md), [maintenance](staff-mobile-app-maintenance-work-orders-feature.md) — generates tasks and requests that must reach the right person and be tracked to completion. This feature is where they converge. Its value is **accountability and speed**: nothing gets lost, everything is owned, and supervisors can see and fix bottlenecks in real time.

Vendors position a unified operations platform as the difference between a collection of tools and an actual operating system for the property: ALICE/Actabl (unified ops, cross-department requests), Amadeus HotSOS (two-way desktop↔mobile messaging), and IDS Next FX Guest Service (assignment & monitoring).

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What staff can do from the device |
|---|------------|-----------------------------------|
| 1 | **Cross-department service requests** | Send and receive tasks between any departments, anywhere on property. |
| 2 | **Two-way staff messaging (desktop ↔ mobile)** | Real-time chat between back-office and field staff; 1:1, group, department, shift. |
| 3 | **Guest-service assignment & monitoring** | Assign guest requests and track their progress from a mobile app. |
| 4 | **Unified ops accountability** | One system connecting all departments for messaging, tasking, and ownership. |
| 5 | **Broadcasts & announcements** | Managers push announcements with read receipts. |
| 6 | **Task attachments & context** | Attach photos, rooms, guests, and tasks to messages and requests. |

### 2.2 In scope (this release)
Cross-department task routing, two-way staff messaging (1:1/group/department/shift), guest-service assignment & monitoring, broadcasts with read receipts, task context/attachments, offline support, RBAC.

### 2.3 Out of scope (this release)
Full HR/scheduling suite, external guest-facing messaging (that lives in the guest app), payroll, and social/non-work chat governance beyond basic moderation. (See §9.)

### 2.4 Key assumptions
- Staff directory and department/role structure are available (from HR/PMS or maintained in-app).
- Push infrastructure (APNs/FCM) is in place.
- Guest/room references resolve from the PMS for task context.

---

## 3. Capability Deep-Dive — The Task & Communication Features

Each capability is elaborated — **what it is**, **how it works in Atrium**, **why it matters operationally**, and the **revenue/cost angle** — with vendor precedents noting the bar to clear.

### 3.1 Cross-department service requests
*Precedent: ALICE/Actabl.*

**What it is.** Sending and receiving tasks between any departments — front desk to housekeeping, guest services to engineering — from anywhere on property.

**How it works in Atrium.** A staff member creates a request (extra towels, a repair, a wheelchair) and routes it to the owning department by type; it lands as a tracked task with an owner, priority, and SLA. Status is visible to both sides until closed.

**Why it matters.** Guest needs rarely belong to one department; the handoff between departments is where things get lost. Structured cross-department routing guarantees the request reaches the right team and is tracked to completion instead of dying in a radio call.

**Revenue / cost angle.** Reliable fulfillment protects satisfaction and loyalty (revenue); eliminates the dropped-handoff rework and duplicate requests of informal channels (cost).

### 3.2 Two-way desktop ↔ mobile staff messaging
*Precedent: Amadeus HotSOS.*

**What it is.** Real-time messaging between back-office (desktop) staff and field (mobile) staff — 1:1, group, by department or shift — with work context attached.

**How it works in Atrium.** A manager at a desk and an attendant on the floor exchange messages in the same threads; messages can carry a room, guest, task, or photo. Everything is logged and searchable — unlike radio or personal WhatsApp.

**Why it matters.** Radios are lossy and record nothing; personal WhatsApp is unmanaged and a compliance risk. A purpose-built, logged channel keeps communication targeted, contextual, and auditable, and keeps work conversations on company infrastructure.

**Revenue / cost angle.** Faster, clearer coordination speeds service (revenue via satisfaction); logged comms reduce miscommunication rework and off-platform compliance risk (cost).

### 3.3 Guest-service assignment & monitoring
*Precedent: IDS Next FX Guest Service.*

**What it is.** Managers and supervisors assign guest requests to staff and monitor their progress to completion from a mobile app.

**How it works in Atrium.** Incoming guest requests are assigned (or auto-assigned) to available staff; the supervisor sees a live board of open requests, owners, elapsed time, and SLA status, and can reassign or escalate.

**Why it matters.** Assignment without monitoring is just delegation; monitoring is what guarantees follow-through. A live board turns guest service into a managed queue with visible accountability and SLA enforcement.

**Revenue / cost angle.** SLA-managed guest service lifts satisfaction and review scores (revenue); live monitoring lets one supervisor oversee more staff effectively (cost).

### 3.4 Unified ops accountability layer
*Precedent: ALICE/Actabl.*

**What it is.** A single platform connecting all departments for messaging, tasking, and accountability — one shared model of staff, rooms, guests, and departments.

**How it works in Atrium.** All task and message streams live in one system so a request, its owner, its SLA, and its history are always visible to the right people.

**Why it matters.** Point tools create silos; a unified platform creates accountability. Supervisors can see where work sits, who owns it, and what's overdue — and act — without a separate reporting tool.

**Revenue / cost angle.** Live accountability speeds service and reduces dropped work across every department (revenue + cost).

---

## 4. Why This Matters

### 4.1 It's the layer that makes everything else accountable
Front desk, housekeeping, and maintenance all generate work that crosses departments. This feature is where that work is owned and tracked — without it, the other features are islands.

### 4.2 Radios and WhatsApp are the status quo it replaces
The default today is lossy, unrecorded, and off-platform. A purpose-built channel is faster, auditable, and compliant — and it keeps guest and staff data on company infrastructure.

### 4.3 A competitive opening in the BD market
Few BD vendors offer a true unified cross-department ops platform. Atrium can lead locally.

---

## 5. Operations It Improves

| Operation today | Pain | With Task & Comms |
|-----------------|------|-------------------|
| **Cross-department handoffs** | Lost in radio/verbal calls | Tracked requests with owner + SLA. |
| **Back-office ↔ field comms** | Radios, personal WhatsApp | Logged, contextual, on-platform messaging. |
| **Guest requests** | No visibility after assignment | Live monitored queue with escalation. |
| **Manager oversight** | No live view of who owns what | Real-time operational visibility (not reports). |
| **Announcements** | Missed, no confirmation | Broadcasts with read receipts. |

---

## 6. How It Increases Revenue

1. **Faster, more reliable guest service.** SLA-managed, monitored requests raise satisfaction and review scores that support rate and direct bookings.
2. **Nothing falls through the cracks.** Reliable cross-department fulfillment protects the guest experience that drives loyalty and repeat stays.
3. **Faster coordination.** On-platform messaging speeds every multi-department interaction the guest touches.

---

## 7. How It Reduces Operational Cost

1. **Less coordination overhead.** Purpose-built messaging replaces radios and phone tag across departments.
2. **Fewer dropped-handoff reworks.** Tracked routing eliminates duplicated and forgotten requests.
3. **Wider supervisory span.** Live monitoring lets one supervisor oversee more staff effectively.
4. **Compliance risk reduction.** On-platform, logged communication reduces the exposure of unmanaged personal-app use.

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Reliable fulfillment | Cross-department requests closed within SLA | ≥ 90% |
| Faster response | Median request acknowledge/response time | ↓ 40% |
| Coordination shift | Radio/WhatsApp use for work coordination | ↓ near zero |
| Supervisory efficiency | Staff overseen per supervisor | ↑ measurable |
| Adoption | Staff active in-app weekly | ≥ 80% |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| Staff directory | Needs accurate roles/departments | Sync from HR/PMS; in-app maintenance |
| Push reliability | Alerts must be dependable | APNs/FCM + in-app inbox fallback |
| Adoption vs. WhatsApp | Staff habituated to personal apps | Superior UX, manager mandate, on-shift training |
| Notification fatigue | Too many alerts → ignored | Smart routing, quiet hours, priority tiers |
| Data governance | Guest data in messages | RBAC, retention policy, audit log |
| Offline zones | Field staff lose connectivity | Offline queue with auto-sync |

---

## 10. Scope delivered

Atrium Staff ships as one complete package. This feature delivers:

- Cross-department task routing
- Two-way messaging (1:1/group/department)
- Broadcasts with read receipts
- Task context/attachments
- RBAC
- Offline support
- Guest-service assignment & monitoring board
- Escalation/SLA
- Unified accountability view
- Advanced routing

---

## 11. Open Questions

- Source of truth for the staff directory and department/role structure?
- Is off-platform messaging (WhatsApp) to be actively restricted, or just offered an alternative?
- What notification-governance rules (quiet hours, priority bypass) are required?
- Retention and audit requirements for staff messages containing guest data?
- Single property confirmed for v1?

---

## 12. One-Line Business Case

> **Task Management & Internal Communication is the accountability layer that makes the whole operation visible and owned — replacing radios and WhatsApp with one logged, cross-department system where nothing gets lost and supervisors can see and fix bottlenecks in real time.**
