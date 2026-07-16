# Housekeeping Management on Mobile — Feature Deep-Dive

**Product:** Atrium Staff (Hotel / Resort Operations Mobile App)
**Feature area:** F3 — Mobile Housekeeping Management
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-14
**Status:** Draft v1.0
**Related:** [Staff Mobile App — Feature Scope](staff-mobile-app-feature-scope.md) · [Front Desk — Deep-Dive](staff-mobile-app-front-desk-feature.md) · [Reservations Management — Deep-Dive](staff-mobile-app-reservations-management-feature.md) · [Competitive Feature Research](../HOTEL-~1.MD)

---

## 1. Executive Summary

**Mobile Housekeeping Management** replaces the printed attendant worksheet, the radio, and the once-a-shift status board with a live, two-way system running on every attendant's and supervisor's handheld. It is the operational engine that turns a **dirty, departed room into clean, inspected, sellable inventory** as fast and reliably as possible — and tells the front desk the instant it happens.

Housekeeping is the largest frontline labor cost in most hotels and the single biggest determinant of two revenue-critical outcomes: **how quickly rooms become sellable** (enabling early check-ins and same-day resells) and **whether the guest's room meets brand standard** (driving review scores and rate power). A mobile housekeeping system attacks both at once — compressing the room-turn cycle while enforcing quality through digital checklists and inspections.

Vendors treat this as a flagship staff-ops capability: Stayntouch, Amadeus HotSOS, IDS Next FX Housekeeping, Infor HMS Housekeeper, Quore, Knowcross, ALICE/Actabl, and Bangladeshi vendors (Smart Software, Pridesys, Mediasoft) all ship it. It is the most mature, most-proven area of the staff app — and the one with the clearest, fastest ROI.

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What staff can do from the device |
|---|------------|-----------------------------------|
| 1 | **Real-time room-status updates** | Set/see Clean / Dirty / Inspected / Out-of-Service, Vacant / Occupied, attendant-in-room — synced live to the PMS. |
| 2 | **Auto assignment & task lists** | Attendants receive an assigned room list; supervisors auto-generate boards by work time, credits, or section. |
| 3 | **Digital checklists & inspection / QA** | Step-by-step cleaning checklists and supervisor inspection forms with photo evidence and brand-standard scoring. |
| 4 | **Supervisor ↔ attendant communication** | Assign tasks dynamically, message, and track cleaning progress in real time — no radios. |
| 5 | **Workload tracking (credits / minutes / area)** | Balance assignments by cleaning credits, minutes, or area, respecting labor rules; multi-language. |
| 6 | **Per-room guest context & DND** | See occupancy, VIP/preferences, and Do-Not-Disturb flags; defer DND rooms and re-queue. |
| 7 | **Minibar, laundry & linen/towel logging** | Log minibar consumption and linen/towel counts from the room; post minibar charges to the folio. |
| 8 | **Instant front-desk "room ready" alert** | The moment a room passes inspection, notify front desk to enable early check-in. |
| 9 | **Mobile task reassignment** | Supervisors rebalance workloads in real time as arrivals, no-shows, and priorities shift. |
| 10 | **Device-flexible** | Runs on staff BYOD or hotel-provided iOS / Android / Windows handhelds and tablets. |

### 2.2 In scope (this release)
Real-time room status, auto/manual assignment and task lists, digital cleaning checklists, supervisor inspection/QA, workload balancing, per-room guest context + DND, minibar/linen logging, room-ready notification, mobile reassignment, offline support.

### 2.3 Out of scope (this release)
Full inventory/procurement and linen-par purchasing, laundry-plant management, deep IoT/BMS sensor integration (occupancy sensors, smart minibars), predictive/AI scheduling, and payroll. (See §11 Future in the [scope doc](staff-mobile-app-feature-scope.md).)

### 2.4 Key assumptions
- The PMS exposes real-time **room-status read/write** and occupancy data, or a middleware sync layer bridges it.
- Attendants have company or BYOD devices; the app degrades gracefully in Wi-Fi dead zones (offline mode).
- Minibar charge posting requires a PMS/POS folio-post path.

---

## 3. Capability Deep-Dive — The Housekeeping Features

The competitive research (Part B §3, *Housekeeping Management*) identified the capabilities below. Each is elaborated — **what it is**, **how it works in Atrium**, **why it matters operationally**, and the **revenue/cost angle** — with vendor precedents noting the bar to clear.

### 3.1 Real-time room-status updates
*Precedent: Stayntouch, Maestro, Hotelogix, IDS Next FX Housekeeping, Amadeus HotSOS, Infor HMS, eZee, Smart Software / Pridesys / Mediasoft (BD).*

**What it is.** Live room-status tracking — Clean / Dirty / Inspected / Out-of-Service, Vacant / Occupied, attendant-in-room — updated from the device and synced two-way with the PMS the instant it changes.

**How it works in Atrium.** An attendant marks a room "in progress" on entry and "clean" on exit; a supervisor marks it "inspected." Every change writes to the PMS immediately, so the front desk, reservations, and other attendants share one live picture. Out-of-service flags remove rooms from the sellable pool in real time.

**Why it matters.** Status lag is the hidden tax on housekeeping: a room clean for 30 minutes but still showing "dirty" is a room that couldn't be sold or assigned. Real-time status collapses that lag to zero and gives every department a single source of truth about the state of the house.

**Revenue / cost angle.** Zero status-lag means faster early check-ins and same-day resells (revenue); it eliminates the radio calls and phone checks staff use today to confirm room state (cost).

### 3.2 Auto room assignment & task lists
*Precedent: Stayntouch, ALICE/Actabl.*

**What it is.** Automatic generation of attendant boards and task lists — drag-and-drop custom assignments, or auto-built by employee work time, section, or credits.

**How it works in Atrium.** At shift start the system generates each attendant's room list from occupancy, checkout/stayover mix, and available work time. Supervisors adjust with drag-and-drop. Each attendant sees only their queue, in priority order (checkout-priority, VIP, early-arrival rooms first).

**Why it matters.** Manual board-building is slow, error-prone, and re-done by hand every morning. Automated assignment distributes work fairly and optimally in seconds, and re-optimizes as the day changes — putting the right rooms in front of the right attendant at the right time.

**Revenue / cost angle.** Sequencing checkout-priority rooms first accelerates sellable-room availability (revenue); automated, balanced boards raise attendant productivity and cut supervisory admin time (cost).

### 3.3 Digital cleaning & inspection checklists / brand-standard QA
*Precedent: WebRezPro, Quore, Knowcross.*

**What it is.** Step-by-step digital cleaning checklists for attendants and structured inspection forms for supervisors — with required steps, photo evidence, deep-clean tracking, and brand-standard scoring.

**How it works in Atrium.** Each room type has a checklist the attendant works through; certain steps require a photo. Supervisors run inspection forms that score the room against brand standards, flag fails with photos, and route rework back to the attendant. Deep-clean and periodic tasks are tracked on their own cadence.

**Why it matters.** Checklists convert "clean" from a subjective judgment into a verifiable standard, and inspections make quality auditable. Photo evidence resolves disputes and protects the property. This is how a brand guarantees consistency across hundreds of rooms and dozens of attendants.

**Revenue / cost angle.** Consistent, standard-meeting rooms drive review scores, rate power, and repeat stays (revenue); catching fails before the guest arrives avoids comps, moves, and complaint-handling labor (cost).

### 3.4 Supervisor ↔ housekeeper communication & progress tracking
*Precedent: IDS Next FX Housekeeping, Amadeus HotSOS.*

**What it is.** Two-way communication and live progress tracking between supervisors and attendants — dynamic task assignment and cleaning-progress visibility from handhelds.

**How it works in Atrium.** Supervisors see each attendant's progress (rooms done / in progress / remaining) in real time and message or assign directly to a device — no radios. Attendants flag issues (maintenance needed, room not ready to clean) straight back to the supervisor.

**Why it matters.** Radios are broadcast, lossy, and leave no record. In-app communication is targeted, logged, and tied to specific rooms and tasks — so nothing is misheard or forgotten, and supervisors always know where the shift stands.

**Revenue / cost angle.** Live progress visibility lets supervisors promise accurate room-ready times to the front desk (revenue via early check-ins); targeted comms cut the wasted motion and miscommunication of radio coordination (cost).

### 3.5 Workload tracking by credits / minutes / area
*Precedent: Amadeus HotSOS.*

**What it is.** Assigning and balancing work by cleaning **credits**, **minutes**, or **area**, respecting local labor rules — with multi-language support for diverse teams.

**How it works in Atrium.** Each task type carries a credit/minute value (a checkout clean costs more than a stayover touch-up). Boards are balanced so no attendant is overloaded or idle, and assignments respect contractual limits. The UI is available in the attendant's language.

**Why it matters.** Fair, rules-aware workload distribution is both an efficiency lever and a labor-relations necessity. Measuring work in credits/minutes also gives managers the data to staff correctly and to justify headcount.

**Revenue / cost angle.** Optimal balancing extracts more productive room-turns per labor hour (cost); measurable workloads enable right-sized staffing that avoids both overstaffing and service-hurting understaffing (cost + revenue protection).

### 3.6 Per-room guest details & Do-Not-Disturb flags
*Precedent: Infor HMS Housekeeper, protel.*

**What it is.** Surfacing per-room occupancy, guest preferences/VIP status, and Do-Not-Disturb flags so attendants clean the right rooms the right way at the right time.

**How it works in Atrium.** Each room on the attendant's list shows whether it's occupied, checking out, a stayover, VIP, or flagged DND. DND rooms are deferred and auto-re-queued for a later attempt; VIP rooms surface preference notes (pillow type, amenities) so the clean is personalized.

**Why it matters.** Cleaning a DND room is a guest-experience failure; skipping a checkout is a revenue failure. Context on every room prevents both, and lets the property tailor service to VIPs without a separate briefing.

**Revenue / cost angle.** Personalized VIP servicing and respected privacy lift satisfaction and loyalty (revenue); smart DND deferral avoids wasted trips and re-attempts (cost).

### 3.7 Minibar consumption & linen / towel / laundry tracking
*Precedent: Quore, protel.*

**What it is.** Logging minibar consumption (with folio posting) and linen/towel/laundry counts directly from the room.

**How it works in Atrium.** The attendant records minibar items consumed; the charge posts to the guest folio before checkout. Linen and towel counts feed par-level tracking and flag replenishment needs.

**Why it matters.** Minibar charges logged at checkout time are frequently missed on paper, leaking revenue. Digital capture at the point of observation closes that leak and feeds linen management with real consumption data.

**Revenue / cost angle.** Captures minibar revenue that deferred/paper logging loses (revenue); accurate linen tracking reduces over-ordering and loss (cost).

### 3.8 Instant front-desk notification when a room is ready
*Precedent: Stayntouch.*

**What it is.** An automatic alert to the front desk the moment a room passes inspection and becomes sellable.

**How it works in Atrium.** When a supervisor marks a room "inspected/ready," the [front desk](staff-mobile-app-front-desk-feature.md) receives an instant notification and the room flips to sellable — enabling an early check-in or a same-day assignment without anyone making a call.

**Why it matters.** The gap between "room ready" and "front desk knows" is where early-check-in opportunities die. Closing it in real time directly converts housekeeping speed into guest-satisfaction and revenue.

**Revenue / cost angle.** Directly enables more early check-ins and faster resells on high-occupancy days (revenue); removes the back-and-forth calls between housekeeping and front desk (cost).

### 3.9 Mobile task reassignment
*Precedent: ALICE/Actabl.*

**What it is.** Real-time reassignment of rooms and tasks between attendants as conditions change — without radios or returning to a desk.

**How it works in Atrium.** When an attendant falls behind, calls in sick, or a rush of early arrivals hits, the supervisor drags rooms to another attendant and the change lands instantly on the affected devices.

**Why it matters.** The morning plan never survives contact with reality. Live reassignment lets supervisors respond to sickness, VIP rushes, and priority changes in seconds, keeping the whole shift optimized rather than locked to a stale board.

**Revenue / cost angle.** Keeps priority rooms moving despite disruptions (revenue via availability); avoids the idle time and bottlenecks of a rigid, manually-rebuilt plan (cost).

### 3.10 Replaces printed attendant worksheets · device-flexible
*Precedent: protel, WebRezPro (paperless); Maestro (device-flexible).*

**What it is.** A fully digital, paperless housekeeping operation that runs on staff or hotel-provided devices (iOS / Android / Windows handhelds, tablets).

**How it works in Atrium.** No printed worksheets: every board, checklist, and status lives in the app and syncs in real time. The app runs on whatever devices the property standardizes on.

**Why it matters.** Paper worksheets are stale the moment they print, can't sync, and generate manual re-entry. Going paperless removes that entire error-prone loop; device flexibility keeps hardware cost and rollout friction low.

**Revenue / cost angle.** Eliminates printing and manual status re-entry (cost); paperless real-time sync accelerates room turns (revenue via faster sellable rooms).

---

## 4. Why This Matters

### 4.1 Housekeeping speed *is* sellable inventory
On a high-occupancy day, every minute shaved off the room-turn cycle is a minute closer to an early check-in or a same-day resell. Real-time status + instant room-ready alerts turn housekeeping throughput directly into revenue.

### 4.2 Housekeeping quality *is* the guest's core product
The room is what the guest actually paid for. Digital checklists and inspections make "clean to standard" verifiable and consistent across every attendant and room — the foundation of review scores and rate power.

### 4.3 It is the biggest frontline labor cost — so efficiency compounds
Housekeeping is typically the largest line of frontline labor. Auto-assignment, workload balancing, and live reassignment extract more productive room-turns per paid hour, and the savings recur every single shift.

### 4.4 A competitive opening in the BD market
Housekeeping modules exist among BD vendors, but few combine real-time PMS sync, digital QA checklists, offline mode, and folio-posting minibar capture in one mobile tool. Atrium can lead locally here.

---

## 5. Operations It Improves

| Operation today | Pain | With Mobile Housekeeping |
|-----------------|------|--------------------------|
| **Morning board build** | Supervisor hand-builds and prints boards | Auto-generated, balanced boards in seconds. |
| **Room-status reporting** | Radio calls / periodic desk updates; status lag | Live two-way status the instant a room changes. |
| **Front-desk room-ready check** | Phone/radio back-and-forth | Automatic "room ready" alert on inspection pass. |
| **Quality control** | Subjective, inconsistent, no record | Digital checklists + scored inspections with photos. |
| **Mid-shift disruption** | Rigid paper plan, hard to rebalance | Drag-and-drop live reassignment. |
| **DND / occupied rooms** | Missed or wrongly-entered rooms | Per-room context; smart DND deferral and re-queue. |
| **Minibar at checkout** | Charges missed on paper | Logged in-room, posted to folio. |
| **Wi-Fi dead zones / outages** | Fixed board or app stalls | Offline mode with auto-sync. |

---

## 6. How It Increases Revenue

1. **More early check-ins and same-day resells.** Real-time status + instant room-ready alerts convert housekeeping speed into sellable inventory exactly when demand is highest.
2. **Higher review scores → rate power.** Enforced, photo-verified brand standards make rooms consistently guest-ready, lifting scores that support pricing and direct bookings.
3. **Recovered minibar revenue.** In-room minibar logging with folio posting captures charges that paper worksheets routinely miss.
4. **VIP and preference fulfillment.** Per-room guest context lets attendants personalize service, strengthening loyalty and repeat/direct stays.

> **Illustrative model:** If faster, transparent room-turns let a 150-room hotel grant even 3 additional early check-ins per day that would otherwise be declined — and each represents a satisfaction/loyalty gain plus occasional paid early-check-in fees — the annual value in retained loyalty, review lift, and fees is material, on top of recovered minibar revenue. *(Figures illustrative; validate against property data.)*

---

## 7. How It Reduces Operational Cost

1. **Higher productivity per labor hour.** Auto-assignment, credit/minute balancing, and live reassignment extract more room-turns from the same paid hours — the largest recurring saving.
2. **Eliminated paper and re-entry.** No printed worksheets, no manual status transcription at the desk — removing a whole error-prone clerical loop.
3. **Less coordination overhead.** In-app targeted communication replaces radios and phone tag between attendants, supervisors, and front desk.
4. **Fewer quality-failure costs.** Catching standard fails at inspection (not from a guest complaint) avoids comps, room moves, and complaint-handling labor.
5. **Right-sized staffing.** Workload data (credits/minutes) enables accurate staffing that avoids both costly overstaffing and service-damaging understaffing.
6. **Lower linen/supply waste.** Accurate linen/towel tracking curbs over-ordering and loss.
7. **Outage resilience.** Offline mode avoids the paper-fallback and reconciliation labor an outage otherwise triggers.

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Faster room turns | Median checkout-to-ready time | ↓ 30% |
| Zero status lag | Time from room clean to PMS "ready" | < 1 minute |
| More early check-ins | Early check-ins enabled per day | ↑ measurable |
| Quality consistency | Inspection pass rate / brand-standard score | ↑ vs. baseline |
| Labor efficiency | Rooms cleaned per attendant-hour | ↑ 10–15% |
| Revenue capture | Minibar charges captured vs. estimate | ↑ vs. baseline |
| Coordination | Radio/phone room-status calls | ↓ near zero |
| Reliability | Status updates completed during connectivity loss | 100% via offline |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| PMS status sync | Real-time room-status read/write required | Middleware sync layer; queue + reconcile |
| Minibar posting | Needs a folio-post path (PMS/POS) | Integrate folio API; fallback to charge queue |
| Wi-Fi dead zones | Coverage gaps in far rooms/floors | Robust offline mode with clear sync status |
| Device strategy | BYOD vs. hotel-provided handhelds | Support both; MDM/kiosk mode |
| Adoption | Attendants used to paper; language diversity | Simple, icon-driven, multilingual UX; on-shift training |
| Labor rules | Credit/minute limits vary by contract/region | Configurable workload rules per property |
| Photo storage | Checklist/inspection photos add data volume | Compression, retention policy, secure storage |

---

## 10. Scope delivered

Atrium Staff ships as one complete package. This feature delivers:

- Real-time room status
- Auto/manual assignment + task lists
- Per-room guest context + DND
- Instant room-ready alert
- Digital cleaning checklists + supervisor inspection/QA with photos
- Supervisor↔attendant messaging + progress tracking
- Mobile reassignment
- Workload balancing by credits/minutes
- Minibar/linen logging + folio posting
- Multilingual UI
- Offline support with advanced conflict resolution
- RBAC

---

## 11. Open Questions

- Does the launch PMS expose real-time room-status writes and occupancy data?
- Is minibar folio-posting available via the PMS/POS API at launch?
- Company-provided handhelds or BYOD — and which OS mix (iOS / Android / Windows)?
- Are cleaning credits/minutes standardized, or must they be configurable per property/contract?
- What are the photo-evidence retention and privacy requirements (rooms may show guest belongings)?

---

## 12. One-Line Business Case

> **Mobile Housekeeping Management turns the hotel's biggest frontline labor cost into its fastest revenue lever — compressing every room-turn, enforcing brand-standard quality, and telling the front desk the instant a room is sellable, all from the attendant's hand.**
