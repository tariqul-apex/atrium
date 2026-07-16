# Atrium Staff — Feature Prioritization (Productivity & Revenue)

**Product:** Atrium Staff (Hotel / Resort Operations Mobile App)
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-15
**Status:** Draft v2.0
**Related:** [Overview & Index](../overview.md) · [Executive Summary](staff-mobile-app-executive-summary.md) · [Navigation & Feature Map](staff-mobile-app-navigation-map.md)

Atrium Staff ships as **one complete package** — every feature is delivered. This document ranks the feature areas by **staff-productivity impact** and **business-revenue impact** so their **relative weight within that package** is clear (which features carry the most impact), not to stage a rollout.

---

## 1. Scoring matrix

Impact rated **High / Med / Low** for a typical property.

| # | Feature | Staff productivity | Revenue impact | Primary user | Priority |
|---|---------|:---:|:---:|--------------|----------|
| F1 | [Front Desk](staff-mobile-app-front-desk-feature.md) | **High** | **High** | Front desk | 🟢 Core |
| F3 | [Housekeeping](staff-mobile-app-housekeeping-management-feature.md) | **High** | **High** | Housekeeping | 🟢 Core |
| F2 | [Reservations](staff-mobile-app-reservations-management-feature.md) | Med | **High** | Front desk / mgr | 🟢 Core |
| F6 | [Notifications](staff-mobile-app-notifications-alerts-feature.md) | **High** | Med | All | 🟢 Core |
| F5 | [Task Mgmt & Comms](staff-mobile-app-task-management-communication-feature.md) | **High** | Low–Med | All | 🟢 Glue |
| F4 | [Maintenance](staff-mobile-app-maintenance-work-orders-feature.md) | Med | Med *(protection)* | Engineering | 🟡 Supporting |
| F8 | [POS (F&B)](staff-mobile-app-pos-multi-outlet-feature.md) | Med | **High** *(resort)* | F&B / resort | 🟡 Segment |
| F9 | [Labor Management](staff-mobile-app-labor-management-feature.md) (scheduling + attendance) | **High** | Med *(cost)* | Managers / all | 🟠 Cost lever |
| F10 | [Preventive Maintenance & Assets](staff-mobile-app-preventive-maintenance-assets-feature.md) | Med | Med *(protection)* | Engineering | 🟠 Cost lever |
| F11 | [Management Analytics / Owner KPIs](staff-mobile-app-management-analytics-owner-kpis-feature.md) | Med | Med *(visibility)* | GM / owner | 🟠 Cost lever |
| F12 | [Inventory & Par-Stock](staff-mobile-app-inventory-par-stock-feature.md) | Med | Med *(margin)* | HK / F&B / mgr | 🔵 Assurance |
| F13 | [Digital Inspections & Compliance](staff-mobile-app-inspections-compliance-feature.md) | Med | Low *(protection)* | HK / mgr | 🔵 Assurance |
| F14 | [Guest Feedback & Service Recovery](staff-mobile-app-guest-feedback-service-recovery-feature.md) | Low | Med *(retention)* | FD / mgr | 🔵 Assurance |
| F15 | [SOP / Training Knowledge Base](staff-mobile-app-sop-training-knowledge-base-feature.md) | Med | Low | All | ⚪ Enablement |
| F16 | [Lost & Found & Staff Safety](staff-mobile-app-lost-found-staff-safety-feature.md) | Low | Low *(liability)* | All | ⚪ Enablement |
| F17 | [NFC & Barcode Scanning](staff-mobile-app-nfc-barcode-scanning-feature.md) | **High** | Med *(throughput/capture)* | F&B / stores / all | 🟡 Enabler *(amplifies F8/F12/F10)* |
| F18 | [Events, Activities & Banquet Management](staff-mobile-app-events-activities-feature.md) | Med | **High** *(resort/ancillary)* | Events / F&B / mgr | 🟡 Segment *(resort/MICE)* |

---

## 2. The ranking, explained

### The core four — highest, most universal ROI

| Feature | Productivity driver | Revenue driver |
|---------|--------------------|--------------------|
| **F1 Front Desk** | Parallel/roving check-in; one device replaces PC + encoder + printer + POS | Upsell at the moment of highest intent; faster room-ready → more early check-ins |
| **F3 Housekeeping** | Auto-assignment, real-time status; replaces paper worksheets & radios (biggest frontline labor line) | Faster room turns = sellable inventory sooner; recovered minibar charges |
| **F2 Reservations** | Manage inventory anywhere; instant booking alerts | Pay-by-link deposits cut no-shows; better packing lifts occupancy |
| **F6 Notifications** | Productivity multiplier — makes every other feature *timely* | React to bookings/cancellations/messages in minutes, not hours |

### The rest

- **F5 Task Mgmt & Comms** — the accountability glue: replaces radios/WhatsApp and routes cross-department work. High **productivity**; revenue only indirect.
- **F4 Maintenance** — reactive work orders: fast broken-to-fixed protects OOO inventory and guest experience (revenue protection).
- **F8 POS (F&B)** — **high revenue for resorts** (charge-to-room + tableside upsell); most valuable in the resort segment.
- **F18 Events, Activities & Banquet** — **high ancillary revenue for resorts and MICE**: structures weddings, banquets, parties, and resort activities, staffs them with **custom event-role crews**, and captures event F&B/activity fees. Segment-weighted like F8; also cuts the manual coordination that events run on. Precedent: Oracle OPERA S&E, Tripleseat/Event Temple (BEOs), ResortSuite/FareHarbor (activities).

### The cost-and-control layer (F9–F11) — the manual-cost headline

These target *manual operational cost* directly — the product's stated #1 priority — rather than the guest-facing loop.

- **F9 Labor Management** — shift scheduling + geofenced clock-in/attendance. Rosters labor to demand, cutting over-staffing and overtime (the biggest controllable cost line) and ending manual rotas/timesheets. Highest-impact cost lever not previously in scope; validated internationally (HotSOS workload-by-credits, ALICE labor analytics).
- **F10 Preventive Maintenance & Assets** — scheduled PM + asset register, extending reactive F4. Prevents costly failures and long OOO downtime; pure cost protection.
- **F11 Management Analytics / Owner KPIs** — a GM/owner-only view of labor-cost %, productivity (rooms/attendant-hour), SLA health, occupancy/ancillary trend. Makes waste *visible and cuttable* and gives ownership ("authority") the numbers to act on — distinct from the *operational* manager Overview.

### The assurance & enablement layer (F12–F16) — lighter, protective

- **F12 Inventory & Par-Stock** — minibar/linen/amenity/F&B counts with low-stock alerts; cuts shrinkage, waste, emergency buys; recovers minibar charges. Protects margin.
- **F13 Digital Inspections & Compliance** — H&S, food-safety/HACCP, fire rounds, brand-standard audits; replaces paper binders, automates compliance evidence, lowers liability. Generalizes F3's QA checklist engine.
- **F14 Guest Feedback & Service Recovery** — in-stay NPS/complaint capture; catch problems *before* checkout → fewer comps and stronger reviews → repeat/direct bookings.
- **F15 SOP / Training Knowledge Base** — in-app how-tos and onboarding; cuts new-staff ramp and trainer hours (high hospitality turnover).
- **F16 Lost & Found & Staff Safety** — digital L&F logbook + lone-worker SOS panic button; ends paper logs, meets staff-safety compliance.

### The cross-cutting enabler (F17) — amplifies the features it lives in

- **F17 NFC & Barcode Scanning** — a fast-capture layer surfaced *inside* other screens, not a tab of its own. Scan-to-order makes the phone a credible **quick-POS** till (F8) for mini-mart/gift-shop/grab-and-go; scan-to-count removes the lookup step in **inventory** (F12); tap-to-identify opens the right **asset/room** first time (F10/F3/F4); proof-of-presence verifies rounds and PM. Its impact is leverage — one scan control speeds many features and cuts wrong-item/lookup errors. Precedent: retail-grade POS (Oracle Simphony, Toast, Lightspeed), inventory tools (MarketMan, Yellowdog), and hotel QR/NFC tagging (Knowcross, Quore, Optii).

---

## 3. Priority weighting within the package

All features ship together; the tiers below express **relative impact**, not a build sequence.

- **Operational spine (highest, most universal):** Auth/RBAC + **F1 Front Desk** (core check-in/out, folio) and **F3 Housekeeping** (room status).
- **Flow:** **F2 Reservations**, **F4 Maintenance**, **F6 Notifications** — keep inventory accurate and work timely.
- **Coordinate:** **F5 Task Management & Comms** — the accountability glue.
- **Resort upside:** **F8 F&B POS** and **F18 Events, Activities & Banquet** — highest ancillary value for the resort/MICE segment.
- **Cost & control:** **F9 Labor Management**, **F10 Preventive Maintenance**, **F11 Owner KPIs** — attack the biggest manual-cost lines and give ownership visibility.
- **Assurance:** **F12 Inventory**, **F13 Inspections & Compliance** — protect margin and reduce liability.
- **Experience & enablement:** **F14 Guest Feedback**, **F15 SOP/Training**, **F16 Lost & Found & Safety**.
- **Cross-cutting enabler:** **F17 NFC & Barcode Scanning** — surfaced inside F8/F12/F10/F3/F1; speeds quick POS and inventory capture.

The **Home tab** is a lightweight operational launcher (frontline) / operational Overview (managers) — see [Role-Based Design](staff-mobile-app-role-based-design.md).

---

## 4. Bottom line

- **Highest, universal ROI:** **F1, F3, F2, F6** — boost *both* productivity and revenue at every property.
- **Also core:** **F5** (glue), **F4** (reactive repairs), **F8** (F&B POS, resorts), **F18** (events/activities, resorts & MICE).
- **The cost-and-control layer:** **F9** (labor), **F10** (preventive maintenance), **F11** (owner KPIs) — the direct manual-cost attack.
- **Assurance & enablement:** **F12–F16** — protect margin, reduce liability, and speed staff onboarding.
- **Cross-cutting enabler:** **F17** (NFC & barcode scanning) — makes quick POS and inventory faster and more accurate from inside the features it serves.
- Every in-scope feature either makes frontline staff more productive, directly earns/protects revenue, or cuts manual operational cost — and all ship together.

---

## 5. Caveats

- Ratings are for a **typical property**; a large resort would raise F8. Re-score against the specific launch property.
- Revenue/productivity magnitudes should be validated against real property data (see the illustrative models in each deep-dive).
