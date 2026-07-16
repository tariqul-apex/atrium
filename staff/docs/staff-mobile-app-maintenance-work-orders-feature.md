# Maintenance & Work Orders on Mobile — Feature Deep-Dive

**Product:** Atrium Staff (Hotel / Resort Operations Mobile App)
**Feature area:** F4 — Mobile Maintenance & Work Orders
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-15
**Status:** Draft v1.1
**Related:** [Staff Mobile App — Feature Scope](staff-mobile-app-feature-scope.md) · [Housekeeping — Deep-Dive](staff-mobile-app-housekeeping-management-feature.md) · [Competitive Feature Research](../HOTEL-~1.MD)

---

## 1. Executive Summary

**Mobile Maintenance & Work Orders** gives engineering and facilities staff a handheld system to log, assign, track, and close repairs from anywhere on property. It replaces the maintenance logbook, the whiteboard, and the radio with a photo-rich, request-to-completion workflow, and keeps every out-of-order room accounted for.

Maintenance sits at the intersection of **guest experience** and **revenue**: a broken AC is a comp or a bad review; a room stuck out-of-order too long is lost inventory. A mobile work-order system shortens the time from "something's broken" to "it's fixed."

Vendors treat this as a core staff-ops pillar: ALICE/Actabl, Maestro, Quore, protel, WebRezPro (maintenance alarms), Amadeus HotSOS, Knowcross, and Smart Software (BD) all ship it.

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What staff can do from the device |
|---|------------|-----------------------------------|
| 1 | **Work-order creation & tracking** | Log a repair with notes and photos; assign it; track it request-to-completion across shifts. |
| 2 | **Service orders & space inspections** | Generate service orders and run guestroom / public-area inspections via role-based access. |
| 3 | **Guest-request & incident management** | Log, route, and track guest-reported issues and incidents to resolution. |
| 4 | **Out-of-order / out-of-service control** | Flag rooms/areas OOO for repair (sync to PMS) and return them to service on completion. |
| 5 | **Photo & note evidence** | Attach before/after photos and notes for accountability and warranty/insurance records. |
| 6 | **Priority & SLA tracking** | Prioritize by severity; escalate overdue work orders. |

### 2.2 In scope (this release)
Work-order create/assign/track/close, photo+note evidence, guest-request/incident logging, service orders & space inspections, OOO control with PMS sync, priority/SLA, offline support, RBAC.

### 2.3 Out of scope (this release)
Full CMMS/EAM procurement and parts-inventory purchasing, capital-project management, energy/BMS automation, and vendor-contract management. (Integrate with a CMMS where one exists — see §9.)

### 2.4 Key assumptions
- OOO room status syncs to the PMS (real-time write) or via middleware.
- Technicians have devices; app degrades gracefully offline.

---

## 3. Capability Deep-Dive — The Maintenance Features

Each capability is elaborated — **what it is**, **how it works in Atrium**, **why it matters operationally**, and the **revenue/cost angle** — with vendor precedents noting the bar to clear.

### 3.1 Work-order creation & tracking (notes + photos, request-to-completion)
*Precedent: ALICE/Actabl, Maestro work-order module, Quore, protel, WebRezPro maintenance alarms, Smart Software (BD).*

**What it is.** Logging a repair request with notes and photos, assigning it to a technician, and tracking its full lifecycle — New → Assigned → In progress → Completed → Verified — across shifts.

**How it works in Atrium.** Anyone (front desk, housekeeping, a guest via staff) logs an issue against a room/area with a photo. It routes to engineering, a tech accepts, logs time/parts and before/after photos, and closes it; the originator is notified. Open orders carry across shift handovers with full history intact.

**Why it matters.** Paper logs and radios lose work orders between shifts; nothing is auditable and nothing is measurable. A tracked, photo-rich workflow guarantees issues aren't dropped, shows exactly what was done, and creates a record for warranty, insurance, and dispute resolution.

**Revenue / cost angle.** Faster repairs return OOO rooms to sellable inventory sooner (revenue) and prevent guest-facing failures that trigger comps and bad reviews (revenue protected); accountability cuts the rework and duplicated effort of lost tickets (cost).

### 3.2 Service orders & space inspections on mobile (role-based)
*Precedent: Amadeus HotSOS.*

**What it is.** Generating service orders and running structured guestroom and public-area inspections from the device, gated by role-based access.

**How it works in Atrium.** Supervisors run inspection forms on rooms and public spaces (lobby, pool, corridors), flagging deficiencies that auto-generate service orders. RBAC ensures only authorized roles create, assign, or close certain order types.

**Why it matters.** Proactive inspection catches problems before guests do and turns findings directly into tracked work — closing the loop between "spotted an issue" and "someone's fixing it" without paper or verbal handoffs.

**Revenue / cost angle.** Proactive fixes protect the guest experience and asset condition (revenue); auto-generated orders remove manual re-entry (cost).

### 3.3 Guest-request & incident management
*Precedent: Knowcross, Quore.*

**What it is.** Logging, routing, and tracking guest-reported issues and operational incidents to resolution — with accountability and history.

**How it works in Atrium.** A guest complaint (no hot water, noisy AC) is logged against the room, routed to the right department, tracked to closure, and the guest followed up. Incidents (safety, damage) are recorded with photos and an audit trail. Integrates with the [Task Management](staff-mobile-app-task-management-communication-feature.md) layer for cross-department routing.

**Why it matters.** Guest issues that fall through the cracks are the fastest path to a bad review. Structured routing and tracking ensure every reported problem is owned, resolved, and closed with the guest — turning a complaint into a recovery.

**Revenue / cost angle.** Fast, tracked service recovery protects reviews, loyalty, and rate power (revenue); incident records reduce liability and dispute cost (cost).

---

## 4. Why This Matters

### 4.1 Downtime is lost inventory and lost guests
An out-of-order room earns nothing; a broken amenity earns a comp and a bad review. Shortening repair cycles directly recovers both inventory and satisfaction.

### 4.2 Fast reactive repair protects revenue and experience
The faster a reported fault is fixed, the sooner an OOO room returns to sale and the fewer guests hit a broken amenity. A mobile work-order system is what makes request-to-completion fast on the floor.

### 4.3 Accountability is only real if it's tracked
Photos, timestamps, and assignment history turn maintenance from a black box into a measurable, auditable operation — essential for warranty, insurance, compliance, and dispute resolution.

### 4.4 A competitive opening in the BD market
Few BD vendors offer photo-rich mobile work orders with PMS-synced OOO control. Atrium can lead locally.

---

## 5. Operations It Improves

| Operation today | Pain | With Mobile Maintenance |
|-----------------|------|-------------------------|
| **Logging a repair** | Radio/logbook; easily lost | Photo work order created on the spot, routed instantly. |
| **Shift handover** | Open issues forgotten | Orders carry across shifts with full history. |
| **OOO rooms** | Rooms stuck OOO too long | Tracked repairs + PMS sync return rooms to sale fast. |
| **Guest complaints** | Fall through the cracks | Logged, routed, tracked, and closed with the guest. |
| **Inspections** | Paper, no follow-through | Digital inspections auto-generate service orders. |

---

## 6. How It Increases Revenue

1. **Faster OOO recovery.** Tracked, prioritized repairs return rooms to sellable inventory sooner — especially valuable on high-occupancy days.
2. **Protected reviews and rate power.** Fast fixes to guest-facing issues prevent the comps and negative reviews that erode pricing and loyalty.
3. **Service recovery.** Structured guest-issue handling turns complaints into recoveries that retain guests and direct rebookings.

> **Illustrative model:** If faster repair cycles reduce average OOO room-days by even 1 room-day/week at a $120 ADR, that is ~$6,200/year per recovered room-day trend — before counting avoided comps and protected reviews. *(Figures illustrative; validate against property data.)*

---

## 7. How It Reduces Operational Cost

1. **No lost tickets / rework.** Tracked orders eliminate the duplicated and forgotten work that paper logs and radios produce.
2. **Lower liability and dispute cost.** Incident records and photo evidence reduce risk exposure and dispute effort.
3. **Efficient technician routing.** Prioritized, location-tagged orders cut wasted travel and idle time.

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Faster repairs | Median work-order resolution time | ↓ 40% |
| Less downtime | Average OOO room-days | ↓ measurable |
| Guest-issue recovery | Guest-reported issues closed within SLA | ≥ 90% |
| Accountability | Work orders with photo evidence | ≥ target % |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| PMS OOO sync | Real-time room-status write needed | Middleware sync; queue + reconcile |
| CMMS overlap | Property may run a CMMS | Integrate/sync rather than duplicate |
| Photo storage | Volume + retention | Compression, retention policy, secure storage |
| Offline zones | Plant rooms/basements lack Wi-Fi | Offline mode with auto-sync |
| Adoption | Techs used to logbooks | Simple UX, on-shift training |

---

## 10. Scope delivered

Atrium Staff ships as one complete package. This feature delivers:

- Work-order create/assign/track/close with photos + notes
- OOO control + PMS sync
- Priority/SLA
- Offline support
- RBAC
- Guest-request/incident management
- Mobile inspections auto-generating service orders
- Compliance logging
- CMMS integration

---

## 11. Open Questions

- Does the launch PMS support real-time OOO status writes?
- Is there an existing CMMS to integrate with, or is Atrium the system of record?
- What compliance logs are mandatory (pool, fire, lift) in the target market?
- What photo-retention and evidentiary requirements apply?

---

## 12. One-Line Business Case

> **Mobile Maintenance & Work Orders shortens the time from broken to fixed — protecting inventory and guest experience with a photo-rich, tracked, request-to-completion workflow from the technician's hand.**
