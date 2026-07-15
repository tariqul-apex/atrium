# Atrium Staff — Executive Summary

**Product:** Atrium Staff (Hotel / Resort Operations Mobile App)
**Prepared for:** Leadership / Stakeholders
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-14
**Status:** Draft v1.0
**Start here:** [Project Overview & Documentation Index](../overview.md)
**Supporting detail:** [Feature Scope](staff-mobile-app-feature-scope.md) · [Navigation & Feature Map](staff-mobile-app-navigation-map.md) · [Competitive Research](../HOTEL-~1.MD) *(draft guideline only, not app scope)*

> The per-feature deep-dive docs are linked from the feature table in §3 below, and cataloged in the [Overview & Index](../overview.md).

---

## 1. The opportunity in one paragraph

Hotels and resorts still run frontline operations on radios, paper worksheets, and desk-bound PMS terminals — while global software leaders have moved reception, housekeeping, maintenance, and revenue control onto staff mobile devices. **Atrium Staff** is a mobile-first operations app that puts the whole property in every staff member's hand. It attacks the three numbers that decide a hotel's economics at once: **guest satisfaction**, **revenue capture**, and **labor + operating cost**. Critically for our target market, **almost no Bangladeshi vendor offers a genuine, feature-complete staff mobile app** with offline mode, local payment rails, and integrated resort/revenue tooling — an open lane Atrium can own locally while meeting the global standard.

---

## 2. Why now

- **Guest expectations shifted.** Contactless arrivals, instant service, and personalization are now baseline, not premium.
- **Labor is scarce and expensive.** Doing more with fewer staff-hours is a survival requirement, not an optimization.
- **The desk is being designed out.** Lobbies, resorts, and cluster operating models assume mobile, not fixed terminals.
- **A local gap is wide open.** Global vendors ship this; BD vendors largely don't. First-mover advantage is available now.

---

## 3. The product: seven feature areas, one operating system

| # | Feature area | What it does | Primary economic lever |
|---|--------------|--------------|------------------------|
| F1 | [Front Desk](staff-mobile-app-front-desk-feature.md) | Check-in/out, folio, payment, key — anywhere on property | Upsell at arrival + labor per arrival |
| F2 | [Reservations](staff-mobile-app-reservations-management-feature.md) | Mobile calendar, blocks, create/edit, deposits (pay-by-link) | No-show reduction + occupancy |
| F3 | [Housekeeping](staff-mobile-app-housekeeping-management-feature.md) | Real-time room status, assignment, QA checklists | Faster sellable rooms + labor productivity |
| F4 | [Maintenance (reactive)](staff-mobile-app-maintenance-work-orders-feature.md) | Work orders — log/assign/close with photos | Downtime reduction |
| F5 | [Task & Comms](staff-mobile-app-task-management-communication-feature.md) | Cross-department routing, messaging | Accountability + coordination cost |
| F6 | [Notifications](staff-mobile-app-notifications-alerts-feature.md) | Real-time, filtered, priority alerts | Timeliness of every action |
| F8 | [POS (F&B core)](staff-mobile-app-pos-multi-outlet-feature.md) | Tableside/poolside F&B ordering, charge-to-room | Resort ancillary revenue |

### 3A. The cost-and-control & assurance layer (F9–F16)

Beyond the guest-facing loop, eight features attack **manual operational cost** directly and give ownership visibility. See [Feature Scope §3A](staff-mobile-app-feature-scope.md) and [Prioritization](staff-mobile-app-feature-prioritization.md).

| # | Feature area | What it does | Primary economic lever |
|---|--------------|--------------|------------------------|
| F9 | [Labor Management](staff-mobile-app-labor-management-feature.md) | Shift scheduling + geofenced clock-in/attendance | **Labor cost** — right-size staffing, cut overtime |
| F10 | [Preventive Maintenance & Assets](staff-mobile-app-preventive-maintenance-assets-feature.md) | Scheduled PM + asset register (extends F4) | Downtime & asset-life protection |
| F11 | [Management Analytics / Owner KPIs](staff-mobile-app-management-analytics-owner-kpis-feature.md) | GM/owner view: labor-cost %, productivity, trend | **Owner visibility** — waste made cuttable |
| F12 | [Inventory & Par-Stock](staff-mobile-app-inventory-par-stock-feature.md) | Minibar/linen/amenity/F&B counts + reorder alerts | Margin — shrinkage & waste |
| F13 | [Inspections & Compliance](staff-mobile-app-inspections-compliance-feature.md) | H&S/HACCP/fire/brand audits, digital evidence | Liability & rework |
| F14 | [Guest Feedback & Recovery](staff-mobile-app-guest-feedback-service-recovery-feature.md) | In-stay feedback + service-recovery workflow | Retention / review scores |
| F15 | [SOP / Training Knowledge Base](staff-mobile-app-sop-training-knowledge-base-feature.md) | In-app SOPs & onboarding | Ramp time & error cost |
| F16 | [Lost & Found & Staff Safety](staff-mobile-app-lost-found-staff-safety-feature.md) | Digital L&F + lone-worker SOS | Liability & compliance |

### 3B. Cross-cutting enabler and resort extension (F17–F18)

Two further features round out the package: **F17 NFC & Barcode Scanning** — a cross-cutting scan control (barcode/QR/NFC) surfaced inside POS and inventory that speeds quick-POS rings and stock counts and cuts wrong-item errors; and **F18 Events, Activities & Banquet Management** — structures weddings, banquets, parties, and resort activities, staffs them with **custom event-role crews** (whose hours write back to F9 labor), and captures event F&B and activity ancillary revenue.

These are not seventeen separate tools — they are one system on a shared spine (RBAC, offline sync, real-time PMS integration, one guest/room/folio model). Housekeeping and maintenance produce the room state front desk sells; task management routes the work; notifications make it timely; POS captures ancillary spend; **labor management staffs it all to demand, preventive maintenance protects the assets, and the owner KPI view proves the savings.** The Home tab is a lightweight operational launcher.

---

## 4. The business case, rolled up

### 4.1 How it increases revenue
- **Upsell at the moment of highest intent** (F1) — upgrades and add-ons offered on-device at arrival.
- **Recovered no-show revenue** (F2) — pay-by-link deposits convert lost nights into secured revenue.
- **More sellable inventory, sooner** (F3, F4) — faster room turns and repairs enable early check-ins and same-day resells.
- **Resort ancillary capture** (F8) — frictionless charge-to-room and tableside upsell lift F&B spend.
- **Loyalty and direct bookings** (all) — better, faster, personalized service lifts review scores and repeat/direct stays, reducing OTA commission.

### 4.2 How it reduces operating cost

**This is the primary objective: cut manual operational cost — manpower, time, and error.** The summary levers:

- **Fewer staff-hours per unit of work** — parallel mobile check-in, automated housekeeping assignment, tableside efficiency.
- **Less hardware** — tablets replace fixed terminals, printers, and per-outlet POS.
- **Radios and paper eliminated** — logged, contextual mobile comms and paperless workflows remove a whole error-prone loop.
- **Fewer costly failures** — fast reactive repairs, QA inspections, and SLA tracking cut comps, walks, rework, and downtime.
- **Right-sized staffing** — housekeeping workload data drives labor to actual demand.

#### Must-have features ranked by manual cost eliminated

Grounded in the local + international competitive analysis ([Competitive Research](../HOTEL-~1.MD)), each must-have area is justified here by the **manual process it removes** — not just the ROI it adds.

| Must-have | Manual cost it removes today | Manpower / time saved | Local + intl basis |
|-----------|------------------------------|-----------------------|--------------------|
| **F3 Housekeeping** | Paper worksheets, radio calls, supervisor walk-arounds, manual room-status relay to the desk | Biggest frontline labor line; auto-assignment + real-time status cut supervisor hours and speed room turns | Universal (Stayntouch, HotSOS, Quore) + BD (Smart Software) |
| **F1 Front Desk** | Desk-queue staffing, one-guest-at-a-time terminal, printer/encoder hardware | Parallel/roving check-in; one device replaces PC + encoder + printer + POS | Universal (Shiji single-device, Stayntouch) + BD (Smart Software) |
| **F6 Notifications** | Radio chatter, "did you get it?" follow-ups, manual dispatch | Makes every action timely with no human relaying it | Cloudbeds, Little Hotelier |
| **F5 Task & Comms** | Radios, WhatsApp, printed lists, lost cross-department requests | Removes the verbal-relay + re-entry loop; auto-routes work | ALICE/Actabl unified ops |
| **F2 Reservations** | Desk-bound rebooking, manual deposit chasing, no-show write-offs | Pay-by-link deposits automate no-show recovery | SiteMinder, Cloudbeds |
| **F4 Maintenance** | Paper work orders, phone-tag on repairs, manual asset logs | Faster broken→fixed protects OOO inventory | Quore, HotSOS, Knowcross |
| **F8 F&B POS** *(resort)* | Paper chits, runner trips to a terminal, manual charge-posting | Tableside order + charge-to-room removes re-entry | Agilysys, Smart Software |
| **F9 Labor Management** | Manual rotas, paper timesheets, over-staffing, buddy-punching, overtime leakage | Roster-to-demand + geofenced clock-in cut the **largest controllable cost line** | HotSOS workload-by-credits, ALICE labor analytics |
| **F10 Preventive Maintenance** | Paper PM logs, run-to-failure repairs, long OOO downtime | Scheduled PM prevents costly failures; extends asset life | Quore, ALICE PM & asset history |
| **F11 Owner KPIs** | Manual spreadsheets to compile labor %, productivity, occupancy | Automated BI makes waste visible and cuttable for ownership | Hotelogix mobile KPIs, Stayntouch role dashboards |
| **F12 Inventory & Par-Stock** | Manual stock counts, shrinkage, emergency purchasing | Par alerts cut waste; recovers minibar charges | Quore/protel minibar & stock tracking |
| **F13 Inspections & Compliance** | Paper audit binders, manual compliance evidence | Digital templates cut audit labor and liability | WebRezPro/Quore/Knowcross inspections |

Two cross-cutting must-haves make these savings real in the local market: **offline-first** (keeps work running through the Wi-Fi dead zones and outages common on BD properties) and **local payment rails** (bKash / Nagad / SSLCommerz), neither of which global vendors serve — see §5.

**The core cost lever:** **F3 + F1 + F6** — the operational spine. It attacks the two largest manual-labor lines directly: housekeeping coordination and desk staffing.

**Pinpoint by design:** role-based login shows each person only their job's screens (least privilege / least clutter — see [Role-Based Design](staff-mobile-app-role-based-design.md)). That itself lowers cost: no training on irrelevant screens, no wrong-role errors, and each role's landing screen is their most frequent task.

### 4.3 Illustrative economics (single 150-room property)
*Figures illustrative — validate against real property data before external use.*

| Lever | Illustrative annual impact |
|-------|----------------------------|
| Check-in upsell (F1) | ~$128K incremental (8% attach × $40 × ~110 arrivals/day) |
| No-show recovery (F2) | ~$120–130K protected (deposits on ~half of 5% no-shows) |
| Housekeeping/maintenance | Recurring labor savings + recovered minibar + avoided comps/downtime |
| POS ancillary (F8, resort) | +5–10% F&B check average, compounding per cover |

The point is not the precise numbers — it is that **each area targets a distinct, recurring lever**, and they stack. Even conservatively, the combined revenue upside and cost savings materially exceed the build-and-run cost.

### 4.4 Per-feature summary — cost reduced + owner revenue increased

Every feature included so far, on both dimensions the owner cares about: what it takes **out** of the cost line, and what it puts **into** the revenue line.

| # | Feature | How it reduces operational cost | How it increases owner revenue |
|---|---------|---------------------------------|--------------------------------|
| **F1** | Front Desk | Parallel/roving check-in cuts desk-queue staffing; one device replaces PC + encoder + printer + POS | Upsell (upgrades, add-ons) at arrival — the moment of highest intent; faster room-ready → more early check-ins sold |
| **F2** | Reservations | Manage inventory from anywhere; no desk-bound rebooking or manual deposit chasing | Pay-by-link deposits recover no-show revenue; better packing lifts occupancy |
| **F3** | Housekeeping | Auto-assignment + real-time status replace paper worksheets, radios, and supervisor walk-arounds — the biggest frontline labor line | Faster room turns = sellable inventory sooner; recovered minibar charges |
| **F4** | Maintenance | Digital work orders end paper, phone-tag, and manual asset logs; QA/SLA tracking cuts rework and downtime | Faster broken→fixed returns OOO rooms to sale sooner; protects review scores and comps avoided |
| **F5** | Task & Comms | Replaces radios, WhatsApp, and printed lists; auto-routes cross-department work with accountability | Indirect — faster, reliable service lifts guest satisfaction, reviews, and repeat/direct stays |
| **F6** | Notifications | Ends radio chatter and "did you get it?" follow-ups; every action becomes timely with no human relay | React to bookings/cancellations/messages in minutes — captured revenue and recovered inventory |
| **F8** | F&B POS *(resort)* | Tableside/poolside ordering ends paper chits, runner trips, and manual charge-posting | Charge-to-room + tableside upsell lift F&B check average and ancillary spend per cover |
| **F9** | Labor Management | Roster-to-demand + geofenced clock-in cut over-staffing, overtime, and manual rotas/timesheets — the largest controllable cost | Frees manager hours; disciplined labor-cost % protects margin |
| **F10** | Preventive Maintenance & Assets | Scheduled PM ends run-to-failure repairs, paper PM logs, and long OOO downtime | Returns/keeps rooms sellable; defers capex by extending asset life |
| **F11** | Management Analytics / Owner KPIs | Auto-compiles labor %, productivity, and SLA health — no manual spreadsheets | Gives owner/GM ("authority") the numbers to act on the whole savings stack |
| **F12** | Inventory & Par-Stock | Device counts + reorder alerts cut shrinkage, waste, and emergency buys | Protects F&B/minibar margin; recovers minibar charges fully |
| **F13** | Inspections & Compliance | Digital templates replace paper audit binders and manual evidence-gathering | Protects licence/brand; avoids fines and rework |
| **F14** | Guest Feedback & Recovery | Catches issues in-stay → fewer comps/refunds handled manually at the desk | Protects review scores → direct/repeat bookings, less OTA commission |
| **F15** | SOP / Training Knowledge Base | Cuts new-staff ramp time and trainer hours; fewer errors | Indirect — consistency and retention |
| **F16** | Lost & Found & Staff Safety | Ends paper L&F logbooks; lone-worker SOS meets safety compliance | Indirect — reduced liability |
| **F17** | NFC & Barcode Scanning | Removes lookup labor in POS/inventory counts; cuts wrong-item and receiving errors | Faster quick-POS throughput; captured mini-mart/gift retail |
| **F18** | Events, Activities & Banquet | Cuts event coordination labor; role-based crews avoid over-calling and event overtime | Captures high-margin banquet, event F&B, and activity ancillary revenue |

**Cross-cutting multipliers:** *offline-first* keeps all of the above earning and saving through outages/dead zones; *local payment rails* (bKash/Nagad/SSLCommerz) capture payments global vendors can't; *role-based login* (least privilege / least clutter) removes training and wrong-role error cost. See §5.

---

## 5. Our differentiation (especially in Bangladesh)

- **Offline-first** — keeps arrivals, housekeeping, and payments running through the Wi-Fi dead zones and outages common locally.
- **Local payment rails** — bKash, Nagad, SSLCommerz across front desk, reservations, and POS — which global vendors don't serve.
- **Integrated resort tooling on mobile** — F&B POS with charge-to-room in one app, rare among local vendors.
- **Bilingual, localization-ready** — Bangla/English UI, local VAT, multi-language frontline teams.
- **Global-standard, locally-priced** — matches what Oracle, Mews, Shiji, and Stayntouch offer, tuned to the BD market.

---

## 6. One complete package

Atrium Staff ships as **one complete package** — every feature delivered together, not staged. The features group into themes that together cover the guest-facing operating loop and turn the same platform into a manual-cost-reduction engine.

| Theme | Focus | Areas |
|-------|-------|-------|
| **Operational spine** | Core operating value | Auth/RBAC, **F1 Front Desk**, **F3 Housekeeping** |
| **Flow & timeliness** | Keep inventory accurate, work timely | **F2 Reservations**, **F4 Maintenance**, **F6 Notifications** |
| **Coordinate** | Accountability glue | **F5 Task Management & Comms** |
| **Resort upside** | Highest-value resort features | **F8 F&B POS**, **F18 Events, Activities & Banquet** |
| **Cost & control** | Attack the biggest manual-cost lines | **F9 Labor Management**, **F10 Preventive Maintenance**, **F11 Owner KPIs** |
| **Assurance** | Protect margin and reduce liability | **F12 Inventory & Par-Stock**, **F13 Inspections & Compliance** |
| **Experience & enablement** | Retention, onboarding, safety | **F14 Guest Feedback**, **F15 SOP/Training**, **F16 Lost & Found & Safety** |
| **Cross-cutting enabler** | Faster capture in POS & inventory | **F17 NFC & Barcode Scanning** |

The guest-facing operating loop and the **manual-cost-reduction engine** — labor, assets, inventory, and compliance — are delivered as one, with the owner KPI view (F11) making the savings visible. A single-property pilot validates before chain rollout.

---

## 7. Key risks and how we manage them

| Risk | Mitigation |
|------|------------|
| PMS integration depth (real-time writes) | Middleware sync layer; graceful degradation |
| Connectivity gaps on property | Offline-first architecture with auto-sync |
| Staff adoption | Simple, multilingual, task-based UX; on-shift training; manager buy-in |
| Payment / PCI scope | Tokenized/P2PE flows; hosted links; certified gateways |
| Scope creep | Integrate specialist systems (RMS, CMMS, BI); don't rebuild them |

---

## 8. The decisions we need

1. **Launch market & model** — BD-first single-property pilot, then chain? (Recommended.)
2. **Launch PMS(es)** — and do they support real-time writes, not just reads?
3. **Payment + channel partners** — which local gateways and channel manager are mandatory for v1?
4. **Device strategy** — company-issued vs. BYOD, and MDM approach.
5. **Segment priority** — city hotel (F1–F6) vs. resort (adds F8 F&B POS and F18 Events/Activities) as the beachhead.

---

## 9. Bottom line

> **Atrium Staff turns frontline operations from a fixed-cost, desk-bound process into a mobile, real-time, revenue-generating system — matching the global standard while owning a wide-open local market. It increases revenue at every guest touchpoint, cuts labor and operating cost across every department, and pays back from day one.**
