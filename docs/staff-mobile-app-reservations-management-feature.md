# Reservations Management on Mobile — Feature Deep-Dive

**Product:** Atrium Staff (Hotel / Resort Operations Mobile App)
**Feature area:** F2 — Mobile Reservations Management
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-14
**Status:** Draft v1.0
**Related:** [Staff Mobile App — Feature Scope](staff-mobile-app-feature-scope.md) · [Front Desk on Mobile — Deep-Dive](staff-mobile-app-front-desk-feature.md) · [Competitive Feature Research](../HOTEL-~1.MD)

---

## 1. Executive Summary

**Mobile Reservations Management** puts the hotel's forward-looking booking book — the calendar, room blocks, rate plans, and the create/edit/cancel controls — in the pocket of every authorized staff member. Where the [Front Desk feature](staff-mobile-app-front-desk-feature.md) governs *today's* arrivals and departures, Reservations Management governs the **future inventory**: what is sold, held, blocked, or open across days, weeks, and seasons.

For a property, this is the difference between a manager who can only shape occupancy when seated at a back-office PC and one who can adjust a room block, close out a sold-out date, extend a stay, or send a payment link from the lobby floor, a supplier meeting, or home. It is the tool that keeps the **inventory picture accurate in real time** — the single condition on which every other front-office decision (overbooking, upgrades, walk-ins, pricing) depends.

Vendors treat it as core: RoomRaccoon and Cloudbeds ship drag-and-drop mobile calendars, SiteMinder's Little Hotelier pushes instant-booking alerts with mobile create/edit, and Cloudbeds generates guest pay-by-links from the app.

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What staff can do from the device |
|---|------------|-----------------------------------|
| 1 | **Calendar / booking-chart view** | See a color-coded timeline of bookings and room blocks; drag-and-drop to move or extend stays; tap any booking for detail. |
| 2 | **Room assignment & notes** | Assign or reassign specific rooms to reservations; attach notes to a booking or guest (requests, flags, context). |
| 3 | **Create / edit reservations** | Build a new booking, modify dates/rate/occupancy, or cancel — entirely on mobile. |
| 4 | **Room blocks & closures** | Create and manage room blocks (groups, events) and out-of-order / closed-for-maintenance holds. |
| 5 | **Pay-by-Link generation** | Generate a secure payment link for a reservation and send it to the guest to collect deposits or balances. |
| 6 | **Instant booking notifications** | Receive real-time push alerts for new bookings, modifications, and cancellations, with a tap-through to the reservation. |
| 7 | **Availability-aware editing** | Every change validates against live availability so mobile edits never oversell inadvertently. |
| 8 | **Search & filter** | Find any reservation fast by guest, dates, status, channel, or confirmation number. |

### 2.2 In scope (this release)
Calendar/booking-chart view, drag-and-drop stay changes, room assignment + notes, create/edit/cancel reservations, room blocks and closures, pay-by-link, real-time booking notifications, availability validation, reservation search.

### 2.3 Out of scope (this release)
Automated dynamic pricing and full revenue-management optimization, channel-manager rate distribution, group-contract negotiation/billing workflows, and guest-facing self-booking (that lives in the guest app / booking engine — see Future). Manual, tactical rate changes are covered under the [Front Desk deep-dive §3.9](staff-mobile-app-front-desk-feature.md).

### 2.4 Key assumptions
- The PMS exposes real-time reservation **read and write** APIs (not read-only), or a middleware sync layer bridges it.
- A payment provider supports hosted pay-by-link / tokenized collection within PCI scope.
- Where a channel manager is present, availability changes made in-app propagate to it (or are reconciled) to avoid OTA oversell.

---

## 3. Capability Deep-Dive — The Four Reservations Features

The competitive research (Part B §2, *Reservations Management*) identified four defining capabilities. Each is elaborated below — **what it is**, **how it works in Atrium**, **why it matters operationally**, and the **revenue/cost angle** — with vendor precedents noting the bar to clear.

### 3.1 Calendar view of bookings & room blocks
*Precedent: RoomRaccoon, Cloudbeds.*

**What it is.** A visual, color-coded booking chart (calendar / tape chart) showing every reservation and room block across a date range — with drag-and-drop to move a booking, extend a stay, or reassign a room, and tap-through to full detail.

**How it works in Atrium.** Rooms run down one axis, dates across the other. Each reservation is a colored bar (by status: confirmed, in-house, tentative, blocked, out-of-order). The manager drags a bar to a new room or stretches it to extend a stay; the change validates against availability and writes back to the PMS in real time. Tapping a bar opens the reservation.

**Why it matters.** The booking chart is the front office's mental model of inventory made visible. On mobile, that model travels with the manager — they can rebalance the house, spot a gap, or resolve an assignment conflict without returning to a desktop. Drag-and-drop makes edits that are tedious on a PC fast on a touchscreen.

**Revenue / cost angle.** Better visual packing of the house reduces stranded single-night gaps and improves sellable occupancy (revenue); resolving assignment conflicts on the spot avoids the labor and guest friction of desk-bound rework (cost).

### 3.2 Assign rooms & add notes to reservations / guests
*Precedent: Cloudbeds, eZee.*

**What it is.** Assigning (or reassigning) a specific room to a reservation and attaching free-text notes — special requests, VIP context, operational flags — to the booking or the guest profile.

**How it works in Atrium.** From a reservation the agent picks an available room matching the room type and any preferences, or reassigns to resolve a conflict. Notes captured here surface downstream — to the [front desk at arrival](staff-mobile-app-front-desk-feature.md), to housekeeping, and on the guest profile — so context follows the guest.

**Why it matters.** Pre-assignment lets the property honor preferences (high floor, connecting rooms, quiet side) and stage the house before arrivals hit. Notes turn scattered tribal knowledge into structured, shareable context that every department can act on.

**Revenue / cost angle.** Honored preferences and remembered context lift satisfaction, loyalty, and direct rebooking (revenue); pre-staged assignments smooth peak check-in and cut miscommunication-driven rework and room moves (cost).

### 3.3 Create / edit reservations & room closures on mobile (with instant booking push)
*Precedent: SiteMinder (Little Hotelier).*

**What it is.** Building a new reservation, modifying an existing one (dates, rate, occupancy, cancel), and creating room closures — all from the device — plus real-time push notifications the instant a booking arrives or changes.

**How it works in Atrium.** A guided mobile flow captures a new booking end-to-end; edits and cancellations validate against availability and rate rules. Room closures (out-of-order, maintenance, owner-use holds) remove rooms from the sellable pool. A new OTA/direct booking fires an instant push so staff act on demand as it lands, not hours later.

**Why it matters.** Demand and disruptions don't wait for someone to be at a desk. A phone booking, a walk-in, a maintenance issue that must close a room, an OTA reservation at 2am — mobile create/edit plus instant alerts means the property responds immediately, keeping the book accurate minute to minute.

**Revenue / cost angle.** Capturing phone/walk-in bookings anywhere and reacting to OTA demand instantly protects revenue; accurate, timely closures prevent selling an unusable room (avoiding walk/comp costs). Fewer missed or late-entered bookings means fewer costly reconciliation and oversell incidents (cost).

### 3.4 Generate Pay-by-Link and send to guests
*Precedent: Cloudbeds.*

**What it is.** Creating a secure hosted payment link tied to a reservation and sending it to the guest (email/SMS/WhatsApp) to collect a deposit or balance without handling the card directly.

**How it works in Atrium.** From a reservation the agent generates a pay-by-link for a set amount; the guest pays on a hosted page and the payment posts to the folio automatically. For the BD market, local rails (bKash, Nagad, SSLCommerz) are supported alongside cards. Because the card is captured on the hosted page, device PCI scope stays minimal.

**Why it matters.** It closes the gap between *making* a reservation and *securing* it. Deposits reduce no-shows; balances can be collected pre-arrival to speed check-in. Doing it from mobile means the person taking the booking can secure payment in the same conversation.

**Revenue / cost angle.** Deposit collection cuts no-show revenue loss and secures cancellations-policy revenue (revenue protected); pre-collected balances speed arrivals and reduce front-desk payment time (cost). Local gateways are a decisive edge in the BD market global vendors don't serve.

---

## 4. Why This Matters

### 4.1 Inventory accuracy is the foundation of every front-office decision
Overbooking limits, upgrade offers, walk-in acceptance, and pricing all assume the booking book is correct *right now*. Mobile reservations management keeps it correct in real time, from anywhere — closing the lag between a change happening and the system reflecting it.

### 4.2 Demand and disruption are not desk-bound
Bookings arrive around the clock across channels; rooms go out of order without notice; guests call to extend. The staff member who can act the moment something changes — not when they next reach a PC — keeps occupancy optimized and the house honest.

### 4.3 It secures revenue before it can leak
Pay-by-link deposits and timely closures stop the two biggest silent revenue leaks: no-shows and selling rooms that can't be occupied. Both are addressable only if the booking book is manageable in the moment.

### 4.4 A competitive opening in the BD market
Global leaders ship mobile calendars, instant-booking alerts, and pay-by-link; most Bangladeshi vendors do not offer a true drag-and-drop mobile reservations tool with local payment rails. Atrium can lead locally here, paired with the front-desk advantage.

---

## 5. Operations It Improves

| Operation today | Pain | With Mobile Reservations Management |
|-----------------|------|-------------------------------------|
| **Reacting to new OTA/phone bookings** | Bookings sit unseen until someone checks the PC | Instant push alerts; act on demand as it lands. |
| **Rebalancing the house** | Assignment conflicts fixed only at a back-office terminal | Drag-and-drop on the booking chart from anywhere. |
| **Extending a stay / date change** | Guest waits while agent goes to a desk | Edit in a few taps at the point of contact. |
| **Group & event blocks** | Blocks managed by one person on one screen | Create/adjust room blocks on mobile, on the floor. |
| **Taking a room out of service** | Delay before closure entered → risk of selling it | Close the room instantly the moment the issue is found. |
| **Securing a booking** | Deposit collection deferred → no-show risk | Send a pay-by-link in the same conversation. |
| **Honoring guest preferences** | Requests lost between booking and arrival | Pre-assign rooms and attach notes that follow the guest. |
| **Manager off-site / off-desk** | Cannot shape inventory until back at a PC | Full reservations control from phone or tablet, anywhere. |

---

## 6. How It Increases Revenue

1. **No-show reduction via deposits.** Pay-by-link deposit collection at the point of booking converts otherwise-lost no-show nights into secured revenue or enforceable cancellation charges.
2. **Faster demand response.** Instant booking alerts and mobile create/edit let staff capture phone and walk-in demand immediately and react to OTA surges — fewer bookings lost to slow handling.
3. **Better occupancy through visual packing.** A drag-and-drop booking chart helps managers eliminate stranded single-night gaps and fit more stays into the same inventory, lifting sellable occupancy.
4. **Preference-driven loyalty and direct bookings.** Pre-assignment and persistent notes deliver the personalization that raises satisfaction, repeat stays, and direct (lower-commission) rebooking.
5. **Protected inventory integrity.** Timely closures and availability-validated edits prevent the oversells and forced walks that cost both revenue and reputation.

> **Illustrative model:** A 150-room hotel at 75% occupancy with a 5% no-show rate loses ~5–6 room-nights/day to no-shows. If pay-by-link deposits recover even half of that at a $120 ADR, that is ~$330–$360/day ≈ **$120,000–$130,000/year** in protected revenue — before counting occupancy gains from better packing and faster demand response. *(Figures illustrative; validate against property data.)*

---

## 7. How It Reduces Operational Cost

1. **Fewer oversell incidents.** Availability-validated mobile edits and timely closures reduce the costly walks, comps, and last-minute relocations that oversells trigger.
2. **Less back-office desk time.** Managers shape inventory from the floor instead of walking back to a PC for every change — reclaiming supervisory hours for guest-facing work.
3. **Lower reconciliation labor.** Real-time, in-the-moment entry means fewer late or missed bookings to chase and reconcile at night audit.
4. **Reduced payment-handling time.** Pre-collected balances via pay-by-link shorten check-in and cut cash/card handling at the desk.
5. **Fewer miscommunication reworks.** Structured notes and pre-assignment reduce the room moves, duplicate requests, and guest complaints that consume staff time.
6. **Leaner cluster staffing.** Multi-property-aware reservations let a chain run centralized/cluster reservations coverage with fewer dedicated terminals and staff.

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Reduce no-shows | No-show rate on deposit-eligible bookings | ↓ 50% |
| Faster demand response | Median time from booking arrival to staff acknowledgement | < 5 minutes |
| Higher occupancy | Sellable-gap nights (stranded singles) | ↓ measurable |
| Inventory integrity | Oversell / walk incidents | → near zero |
| Preference fulfillment | Reservations pre-assigned before arrival | ≥ 80% |
| Deposit capture | Reservations with deposit collected via pay-by-link | ≥ target % |
| Manager mobility | Reservation actions performed in-app (vs. desktop) | ≥ 50% |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| PMS write API | Create/edit/cancel needs real-time writes, not read-only | Middleware sync layer; queue + reconcile |
| Channel-manager sync | In-app availability changes must reach OTAs | Two-way CM integration; reconciliation job |
| Oversell risk | Concurrent edits from multiple devices | Server-side availability locking / validation |
| Payment / PCI | Pay-by-link must keep card off-device | Hosted payment page; tokenization |
| Local gateways | bKash/Nagad/SSLCommerz support for BD | Prioritize local-rail integration |
| Drag-and-drop UX on mobile | Precision on small screens | Snap-to-grid, confirm-before-commit, undo |
| Staff permissions | Rate/closure/cancel changes are sensitive | RBAC gating + audit log of every change |

---

## 10. Release Phasing

| Phase | Scope |
|-------|-------|
| **MVP** | Calendar/booking-chart view (read + tap-through), reservation search, room assignment + notes, create/edit/cancel reservations, instant booking push, RBAC + audit. |
| **Phase 2** | Drag-and-drop stay changes with availability validation, room blocks & closures, pay-by-link (cards + local gateways). |
| **Phase 3** | Multi-property reservations switching, channel-manager two-way sync, advanced conflict resolution, group-block tooling. |

---

## 11. Open Questions

- Does the launch PMS support real-time reservation **writes** and availability locking, or read-only?
- Is a channel manager in scope for v1, and can in-app changes propagate to it to avoid OTA oversell?
- Which pay-by-link provider and local gateways (bKash / Nagad / SSLCommerz) for the BD market?
- How granular should RBAC be — who may cancel, close rooms, or change rates?
- Single-property pilot or chain rollout, and is multi-property reservations needed at launch?

---

## 12. One-Line Business Case

> **Mobile Reservations Management keeps the booking book accurate and actionable from anywhere — turning every manager into a real-time inventory controller who can capture demand, secure deposits, and protect occupancy without ever returning to a desk.**
