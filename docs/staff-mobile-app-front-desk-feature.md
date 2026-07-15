# Front Desk on Mobile — Feature Deep-Dive

**Product:** Atrium Staff (Hotel / Resort Operations Mobile App)
**Feature area:** F1 — Mobile Front Desk
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-14
**Status:** Draft v1.0
**Related:** [Staff Mobile App — Feature Scope](staff-mobile-app-feature-scope.md) · [Competitive Feature Research](../HOTEL-~1.MD)

---

## 1. Executive Summary

The **Mobile Front Desk** turns any staff smartphone or tablet into a full check-in/check-out and folio terminal that works anywhere on property — the lobby, the porte-cochère, a poolside cabana, an airport shuttle, or an overflow event desk. It untethers the reception function from the fixed desk, PC, and printer that have defined hotel arrivals for decades.

For a property, this is not a convenience feature — it is a lever on the three numbers that matter most: **guest satisfaction** (speed and warmth of arrival), **revenue** (upgrade and add-on capture at the highest-intent moment), and **labor cost** (fewer staff-hours per arrival, fewer terminals, less queue-driven overstaffing). Vendors who have shipped mobile front desk — Stayntouch, Shiji, IDS Next FX Front Desk, Infor HMS, Sabre SynXis Property Hub — position it as the single biggest operational differentiator of a modern PMS.

This document defines the feature's scope, quantifies its business case, and maps it to the specific operations it improves.

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What staff can do from the device |
|---|------------|-----------------------------------|
| 1 | **Mobile check-in** | Look up a reservation, verify ID, capture signature, assign/confirm room, encode the key — standing next to the guest, not behind a desk. |
| 2 | **Mobile check-out** | Present the folio, take payment, email the receipt, release the room to housekeeping — in the lobby or at the door. |
| 3 | **Charge posting & folio management** | Post room charges, incidentals, F&B, spa, or minibar; split, transfer, and adjust folios; view real-time balances. |
| 4 | **Payments on the go** | Capture card (tap-to-pay / reader), process deposits, authorizations, and settlements; local gateways (bKash, Nagad, SSLCommerz) for the BD market. |
| 5 | **Room assignment & upgrades** | See live availability, assign or reassign rooms, apply upgrades and rate changes in a few taps. |
| 6 | **Key encoding** | Encode physical keycards via mobile encoder, or issue a digital/mobile key to the guest's phone. |
| 7 | **Arrivals / departures / in-house board** | Search and filter today's arrivals, departures, and stayovers; see status at a glance. |
| 8 | **Guest profile at hand** | View preferences, loyalty tier, VIP flags, notes, and stay history before the guest reaches the desk. |
| 9 | **Offline mode with auto-sync** | Keep checking guests in during Wi-Fi dead zones or outages; queue and sync on reconnect. |

### 2.2 In scope (this release)
Reservation lookup, mobile check-in/out, room assignment/upgrade, folio & charge posting, payment capture, key issuance, arrivals/departures board, guest-profile view, offline queueing, RBAC.

### 2.3 Out of scope (this release)
Full revenue management, channel-manager pricing, group-block contracting, night-audit close (remains a back-office batch), and guest self-service check-in (that lives in the guest-facing app — see Future).

### 2.4 Key assumptions
- The PMS exposes a real-time reservations/folio API, or a middleware sync layer bridges it.
- A certified lock integration (Salto, ASSA ABLOY, dormakaba, or OpenKey) is available for mobile/BLE keys.
- A mobile payment path exists (P2PE reader, tap-to-pay, or hosted payment link) that meets PCI scope.

---

## 3. Capability Deep-Dive — The Ten Front-Desk Features

The competitive research (Part B §1, *Front Desk on Mobile*) identified ten capabilities that define a best-in-class mobile front desk. Each is elaborated below: **what it is**, **how it works in Atrium**, **why it matters operationally**, and the **revenue/cost angle**. Vendor precedents are noted so we know the bar to clear.

### 3.1 Mobile check-in / check-out, upgrades, charge posting, folios & key encoding
*Precedent: Stayntouch, Smart Software (BD).*

**What it is.** The complete reception transaction — the whole arrival-to-departure lifecycle — executed on a handheld instead of a fixed workstation.

**How it works in Atrium.** The agent opens a reservation, verifies the guest's ID and captures a signature on-screen, assigns or confirms the room, offers an upgrade, posts any arrival charges (deposit, early check-in fee), encodes the key, and hands it over — all in one continuous flow, standing beside the guest. At departure the same device presents the folio, settles the balance, emails the receipt, and releases the room to housekeeping.

**Why it matters.** This is the core of the feature: it collapses a multi-station, multi-step desk workflow (look up on PC → walk to encoder → walk to printer → walk to POS) into a single device and a single conversation. It removes the physical desk as the bottleneck and lets check-in happen in parallel, anywhere.

**Revenue / cost angle.** Drives the upsell-at-arrival moment (revenue) and cuts seconds-to-minutes of walking and app-switching per transaction across thousands of arrivals (cost). Folio posting on the spot captures incidentals that deferred posting loses.

### 3.2 Real-time availability & reservations
*Precedent: IDS Next FX Front Desk, Shiji.*

**What it is.** A live, always-current view of room availability and the ability to view, create, or modify reservations from anywhere on property.

**How it works in Atrium.** Availability reads and reservation writes hit the PMS in real time (or through the sync layer), so two roving agents never double-sell the same room.

**Why it matters.** Roving check-in only works if availability is trustworthy at every device simultaneously — every agent sees the same live picture and can create or modify a booking on the spot instead of walking back to a terminal.

**Revenue / cost angle.** Trustworthy real-time availability lets agents capture walk-in and phone demand anywhere without oversell (revenue); removes back-to-the-desk lookups (cost).

### 3.3 Single-device PMS + POS + payments
*Precedent: Shiji.*

**What it is.** One device and one login that spans property management, point-of-sale (dining, spa, activities), and payment capture — no separate terminals or apps per function.

**How it works in Atrium.** From the same guest context an agent can post a room charge, ring up a poolside F&B item or spa treatment, apply an upsell, and take payment — routing each to the correct outlet and to the guest's folio, without leaving the app.

**Why it matters.** It removes the silos between reception, F&B, and payments that force staff to learn and juggle multiple systems and force guests to transact in multiple places. Especially powerful in resorts where the "front desk" is really the whole property.

**Revenue / cost angle.** More charge-capture points and easier upsell across outlets (revenue); one system to license, integrate, train on, and maintain instead of three (cost).

### 3.4 Next-arriving-guest view
*Precedent: Infor HMS.*

**What it is.** A prioritized, forward-looking queue of who is arriving next, with their preferences, messages, and notes editable inline.

**How it works in Atrium.** The board sorts upcoming arrivals by ETA/priority and surfaces each guest's preferences (high floor, feather-free, late arrival), open messages, and internal notes. The agent can prep the room assignment, note a special request, or flag a VIP before the guest walks in.

**Why it matters.** It shifts reception from reactive ("next in line, how can I help?") to anticipatory ("Welcome back, Mr. Rahman — your usual high floor is ready"). Preparation happens in the quiet minutes before arrival, not under queue pressure.

**Revenue / cost angle.** Personalization lifts satisfaction, loyalty, and direct-rebooking (revenue); pre-staged assignments smooth peak throughput so fewer agents are needed at the crunch (cost).

### 3.5 Guest-centric mobile profiles
*Precedent: Sabre SynXis Property Hub.*

**What it is.** Full guest profiles — preferences, loyalty tier, stay history, spend, VIP and service flags — accessible on any device, with the workflow organized *around the guest* rather than around rooms or reservations.

**How it works in Atrium.** Any authorized agent, on any device, pulls the complete profile in one tap. Because the model is guest-centric, a new hire can serve a returning guest well without knowing the property's legacy reservation quirks — the profile tells the story.

**Why it matters.** Consistent, personal service regardless of which agent or device serves the guest. It also underpins **fast onboarding** — the profile-first design means less institutional knowledge is required to give good service.

**Revenue / cost angle.** Recognition and consistency drive loyalty and premium rate tolerance (revenue); dramatically lowers training/ramp cost in a high-turnover labor market (cost).

### 3.6 Mobile / no-download web PMS access
*Precedent: WebRezPro, RMS Cloud staff portal, Mediasoft Pro-Inn, Pridesys.*

**What it is.** Front- and back-office operations that run in a phone or tablet browser — no app install, no per-device provisioning.

**How it works in Atrium.** Alongside the native app, a responsive web client exposes the core front-desk functions through any modern mobile browser. A manager, a seasonal hire, or a borrowed device can be productive in seconds via a URL and login.

**Why it matters.** Zero-install access removes an adoption and IT barrier: no app-store approvals, no MDM push, no device lock-in. It's the fastest path to "try it on any device you already have," which matters for pilots, surge staffing, and BYOD-heavy small properties.

**Revenue / cost angle.** Lowers rollout friction and time-to-value (revenue via faster adoption); avoids per-device provisioning and hardware standardization cost (cost).

### 3.7 Offline mode with auto-sync
*Precedent: Smart Software (BD).*

**What it is.** The ability to keep checking guests in and out — reading data and queuing actions — with no internet, syncing automatically when connectivity returns.

**How it works in Atrium.** The app caches the arrivals/departures board, guest profiles, room status, and folios locally. Actions taken offline (check-in, status change, charge posting) are queued and replayed on reconnect, with conflict resolution to reconcile against any changes made elsewhere. Clear indicators show what is pending vs. synced.

**Why it matters.** Property Wi-Fi has dead zones (far villas, basements, gardens) and outages happen. Without offline mode, the "roam anywhere" promise breaks exactly where coverage is weakest — and an outage stops all arrivals. This is a hard differentiator in the BD market where connectivity is uneven.

**Revenue / cost angle.** Keeps arrivals and payments flowing during outages (revenue protected); eliminates the paper-fallback, comp, and reconciliation labor an outage otherwise triggers (cost).

### 3.8 Arrivals / departures / in-house views with guest & reservation search
*Precedent: Cloudbeds, Hotelogix, eZee.*

**What it is.** The operational board of the day — today's arrivals, today's departures, and everyone currently in-house — with fast search by guest name or reservation.

**How it works in Atrium.** Filterable, sortable lists give each agent an at-a-glance picture of the day's workload and the current house. Search jumps straight to a specific guest or booking; tapping a row opens the full detail and available actions.

**Why it matters.** This is the front desk's daily situational awareness — the map every other action starts from. On mobile it means any agent, anywhere, shares the same live picture instead of crowding one screen behind the desk.

**Revenue / cost angle.** Faster lookups shorten every interaction (cost); visibility into pending departures/arrivals helps orchestrate early check-ins and room turns (revenue via inventory).

### 3.9 Rate & rack-rate changes (seasonal / dynamic)
*Precedent: Hotelogix.*

**What it is.** The ability to adjust room rates and rack rates — for seasons, events, or demand spikes — in a few taps from the device.

**How it works in Atrium.** Authorized managers (RBAC-gated) update rates on the fly: raise rates for a sudden high-demand event, apply a seasonal rack change, or override a walk-in rate at the point of sale. Changes propagate to the availability and quoting the agents see.

**Why it matters.** Front-office managers rarely sit at a desk during the busy hours when pricing decisions matter most. Mobile rate control means the person who sees the lobby filling up can act on it immediately, rather than waiting to get back to a terminal.

**Revenue / cost angle.** Timely demand-based pricing is directly revenue-accretive — capturing rate on high-demand nights and protecting occupancy on soft ones. This is manual, tactical rate control at the point of sale.

### 3.10 Process guest payments on the go
*Precedent: Stayntouch, Hotelogix, SiteMinder (Little Hotelier).*

**What it is.** Taking payment — deposits, balances, incidentals — anywhere on property, by any supported method.

**How it works in Atrium.** The agent captures payment via tap-to-pay, a paired P2PE card reader, or a hosted pay-by-link, plus local rails (bKash, Nagad, SSLCommerz) for the BD market. Authorizations, deposits, and settlements post straight to the folio; the receipt is emailed. PCI scope is controlled by keeping card data tokenized and off the device.

**Why it matters.** Payment is the step that most often forces a guest back to a fixed desk. Closing it on the same device completes the "everything at the point of guest contact" promise and removes the last tether to the terminal.

**Revenue / cost angle.** On-the-spot settlement reduces uncollected balances and disputes and captures incidentals immediately (revenue protected); removes dedicated payment-terminal hardware and the walk-to-POS step (cost). Local gateways are a decisive advantage in the BD market that global vendors largely don't serve.

---

## 4. Why This Matters

### 4.1 The arrival is the highest-stakes moment of the stay
First impressions are formed in the first few minutes. A queue at the desk is the single most common friction point guests cite. Mobile front desk **eliminates the queue as a physical constraint** — staff walk the line, greet arrivals by name, and complete check-in conversationally. The desk stops being a barrier between staff and guest.

### 4.2 It is where up-sell intent peaks
The guest is present, committed, and their room is not yet fixed. An agent holding a device that surfaces "Suite available for +$40" at exactly this moment converts far better than any pre-arrival email. **The point of highest revenue intent is exactly where the fixed desk has historically been weakest** (agents rarely leave the terminal to pitch).

### 4.3 It future-proofs the property
Contactless expectations, staffing shortages, and lobby-redesign trends (removing the desk entirely) all point the same direction. Properties without mobile front desk are locked into a fixed-cost, fixed-location model competitors are abandoning.

### 4.4 It is a competitive table-stake in the mid/upper market and an opening in BD
Global leaders ship it; most Bangladeshi vendors do not offer a genuine mobile front desk with offline mode and local payment rails. Atrium can lead locally on exactly this feature.

---

## 5. Operations It Improves

| Operation today | Pain | With Mobile Front Desk |
|-----------------|------|------------------------|
| **Peak-hour arrivals** | Long queues, guest frustration, agents rushed | Staff roam the lobby/queue and check in in parallel — throughput scales with people, not terminals. |
| **Group & event check-in** | A coach of 40 arrives; the desk collapses | Multiple agents check in the group in the ballroom foyer or coach itself. |
| **Late-night / low-staff shifts** | One agent, one desk = bottleneck | A single roving agent handles arrivals plus other duties without being desk-bound. |
| **VIP / loyalty arrivals** | Impersonal, guest waits like everyone else | Agent meets the VIP at the car with profile and key already prepared. |
| **Room-not-ready situations** | Guest waits at desk with nothing to do | Agent takes payment and details on a lobby sofa, texts the guest when the room is ready. |
| **Check-out crush (morning)** | Line at 8–10am, missed breakfast/transport | Express mobile check-out at the door or over coffee; folio emailed instantly. |
| **Resort sprawl** | Guest walks to a distant reception building | Check-in happens at the villa, pool, or activity desk. |
| **Wi-Fi / power outage** | Fixed PMS terminal is dead | Offline mode keeps arrivals moving; syncs when service returns. |
| **New-hire onboarding** | Complex desktop PMS training | Guided, task-based mobile UX shortens time-to-productive. |

---

## 6. How It Increases Revenue

1. **Upsell capture at check-in.** Surfacing room upgrades, early check-in, late checkout, breakfast, parking, and package add-ons on the device at the moment of arrival is the single biggest revenue driver. Industry upsell platforms report uplifts ranging from double-digit percentages to multiples on ancillary revenue when the offer lands at high-intent moments. Even a conservative capture — e.g., **5–10% of arrivals taking a $30–50 upgrade** — compounds across every arrival, every day.
2. **Higher ADR through frictionless upgrades.** Because assigning a better room is a two-tap action, agents actually make the offer instead of avoiding a clunky desktop flow — lifting average daily rate.
3. **Recovered walk-ins and overflow demand.** Roving agents clear queues faster, so fewer impatient walk-ins leave; event/overflow desks capture demand a single fixed desk would turn away.
4. **Faster room turns → more sellable inventory.** Instant "room released / room ready" sync between front desk and housekeeping enables **more same-day early check-ins**, effectively increasing usable inventory on high-occupancy nights.
5. **Better guest satisfaction → higher rate power and repeat/direct bookings.** Smoother arrivals lift review scores and loyalty, which support pricing and reduce OTA dependence (and OTA commission).
6. **Add-on and incidental posting on the spot.** Easy folio posting captures charges (late checkout, minibar, services) that get missed when posting is deferred.

> **Illustrative model:** A 150-room hotel at 75% occupancy sees ~110 arrivals/day. If mobile upsell converts 8% at an average $40 uplift, that is ~$350/day ≈ **$128,000/year** in incremental revenue from upsell alone — before counting ADR lift, recovered walk-ins, and early-check-in inventory gains. *(Figures illustrative; validate against property data.)*

---

## 7. How It Reduces Operational Cost

1. **Fewer staff-hours per arrival.** Parallel, roving check-in raises throughput, so peak periods no longer require piling extra agents at the desk purely to fight the queue — trimming overtime and seasonal over-staffing.
2. **Lower hardware and desk footprint.** Fewer fixed PMS PCs, terminals, and printers to buy, license, power, and maintain. A tablet is a fraction of the cost of a full desk workstation, and lobbies can be redesigned with fewer/no desks (real-estate and fit-out savings).
3. **Reduced check-in labor time.** Combined PMS + payment + key on one device removes the walk-to-terminal, swap-app, print, and re-key steps — cutting seconds-to-minutes per transaction that add up across thousands of arrivals.
4. **Fewer errors and reworks.** Guest profile, room status, and folio in one synced view reduces mis-assignments, double-postings, and billing disputes — each of which costs staff time and goodwill (refunds, comps).
5. **Lower training cost and faster ramp.** Task-based mobile UX is cheaper to train than legacy desktop PMS, reducing onboarding hours and turnover cost in a high-churn labor market.
6. **Resilience cost avoidance.** Offline mode prevents the "the system is down, we can't check anyone in" scenario that otherwise triggers manual paper fallback, guest comps, and reconciliation labor.
7. **Consolidated tooling.** Single-device PMS + POS + payments removes redundant systems and the integration/maintenance overhead of running them separately.

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Faster arrivals | Median check-in time | ↓ 40% vs. desk baseline |
| Shorter queues | Avg. peak-hour lobby wait | < 3 minutes |
| Revenue capture | Upsell attach rate at check-in | ≥ 8% of arrivals |
| ADR lift | Avg. upgrade revenue per occupied room | +$X (property-set) |
| Cost efficiency | Front-desk labor hours per 100 arrivals | ↓ 20% |
| Inventory gain | Same-day early check-ins enabled | ↑ measurable |
| Guest experience | Arrival-experience review sentiment | ↑ vs. baseline |
| Reliability | Check-ins completed during connectivity loss | 100% via offline mode |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| PMS API depth | Real-time folio/room-status writes required | Middleware sync layer; graceful degradation |
| Lock integration | Mobile/keycard encoding needs certified locks | Support both physical-key encoders and digital/mobile keys via certified lock integrations |
| Payment / PCI | Mobile card capture expands PCI scope | Use P2PE readers / tokenized tap-to-pay; hosted links as fallback |
| Wi-Fi dead zones | Property coverage gaps | Robust offline mode with clear sync status |
| Device management | BYOD vs. company devices | MDM policy; kiosk/lockdown mode |
| Staff adoption | Change from familiar desk workflow | Simple UX, on-shift training, supervisor buy-in |
| Payment reconciliation | Mobile + desk payments must reconcile cleanly | Unified settlement/night-audit reporting |

---

## 10. Scope delivered

Atrium Staff ships as one complete package. This feature delivers:

- Reservation lookup and arrivals/departures board
- Mobile check-in/out
- Room assignment
- Folio view + charge posting
- Payment capture (reader + tap-to-pay + local gateways)
- Upgrade/upsell prompts at check-in
- Physical-key encoding
- Digital/mobile key issuance
- Single-device POS posting
- Guest-profile depth
- Offline read, offline write + auto-sync, and advanced conflict resolution
- RBAC

---

## 11. Open Questions

- Which PMS(es) at launch, and do they support real-time folio writes (not just reads)?
- Which lock vendor(s) are in the target properties — physical encoder, BLE, or both?
- What is the acceptable PCI approach for mobile payments (P2PE reader vs. tap-to-pay vs. link)?
- Is upsell/upgrade content managed in Atrium or pulled from the PMS/RMS?
- Single-property pilot or chain rollout for v1, and in which market (BD-first with local gateways)?

---

## 12. One-Line Business Case

> **Mobile Front Desk moves reception to where the guest and the money are — turning every arrival into a faster, warmer, higher-margin transaction while cutting the terminals, queues, and staff-hours the fixed desk demands.**
