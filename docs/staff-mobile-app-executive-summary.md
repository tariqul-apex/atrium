# Atrium Staff — Executive Summary

**Product:** Atrium Staff (Hotel / Resort Operations Mobile App)
**Prepared for:** Leadership / Stakeholders
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-14
**Status:** Draft v1.0
**Supporting detail:** [Feature Deep-Dives Index](staff-mobile-app-feature-deep-dives-index.md) · [Feature Scope](staff-mobile-app-feature-scope.md) · [Competitive Research](../HOTEL-~1.MD)

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

## 3. The product: eight in-scope feature areas, one operating system

> **Scope note (2026-07-14):** the "less necessary" features were removed (see [Feature Prioritization](staff-mobile-app-feature-prioritization.md)). **F7 Dashboards/Reporting/Analytics is dropped entirely**; F4/F8/F9 are scoped down (reactive work orders; F&B-only POS; manual rates + channel sync). The Home tab is a lightweight operational launcher, not a KPI dashboard.

| # | Feature area | What it does | Primary economic lever |
|---|--------------|--------------|------------------------|
| F1 | [Front Desk](staff-mobile-app-front-desk-feature.md) | Check-in/out, folio, payment, key — anywhere on property | Upsell at arrival + labor per arrival |
| F2 | [Reservations](staff-mobile-app-reservations-management-feature.md) | Mobile calendar, blocks, create/edit, deposits (pay-by-link) | No-show reduction + occupancy |
| F3 | [Housekeeping](staff-mobile-app-housekeeping-management-feature.md) | Real-time room status, assignment, QA checklists | Faster sellable rooms + labor productivity |
| F4 | [Maintenance (reactive)](staff-mobile-app-maintenance-work-orders-feature.md) | Work orders — log/assign/close with photos | Downtime reduction |
| F5 | [Task & Comms](staff-mobile-app-task-management-communication-feature.md) | Cross-department routing, messaging | Accountability + coordination cost |
| F6 | [Notifications](staff-mobile-app-notifications-alerts-feature.md) | Real-time, filtered, priority alerts | Timeliness of every action |
| F8 | [POS (F&B core)](staff-mobile-app-pos-multi-outlet-feature.md) | Tableside/poolside F&B ordering, charge-to-room | Resort ancillary revenue |
| F9 | [Rate & Channel (manual)](staff-mobile-app-revenue-management-feature.md) | Manual rates, market-intel view, channel sync | RevPAR + distribution |

These are not eight tools — they are one system on a shared spine (RBAC, offline sync, real-time PMS integration, one guest/room/folio model). Housekeeping and maintenance produce the room state front desk sells; task management routes the work; notifications make it timely; POS and rate management optimize the money. *(Dashboards/reporting were removed from scope — the Home tab is a lightweight operational launcher.)*

---

## 4. The business case, rolled up

### 4.1 How it increases revenue
- **Upsell at the moment of highest intent** (F1) — upgrades and add-ons offered on-device at arrival.
- **Recovered no-show revenue** (F2) — pay-by-link deposits convert lost nights into secured revenue.
- **More sellable inventory, sooner** (F3, F4) — faster room turns and repairs enable early check-ins and same-day resells.
- **Resort ancillary capture** (F8) — frictionless charge-to-room and tableside upsell lift F&B spend.
- **Higher RevPAR** (F9) — responsive **manual** rate changes, distributed via channel sync, capture demand competitors miss.
- **Loyalty and direct bookings** (all) — better, faster, personalized service lifts review scores and repeat/direct stays, reducing OTA commission.

### 4.2 How it reduces operating cost
- **Fewer staff-hours per unit of work** — parallel mobile check-in, automated housekeeping assignment, tableside efficiency.
- **Less hardware** — tablets replace fixed terminals, printers, and per-outlet POS.
- **Radios and paper eliminated** — logged, contextual mobile comms and paperless workflows remove a whole error-prone loop.
- **Fewer costly failures** — fast reactive repairs, QA inspections, and SLA tracking cut comps, walks, rework, and downtime.
- **Right-sized staffing** — housekeeping workload data drives labor to actual demand.

### 4.3 Illustrative economics (single 150-room property)
*Figures illustrative — validate against real property data before external use.*

| Lever | Illustrative annual impact |
|-------|----------------------------|
| Check-in upsell (F1) | ~$128K incremental (8% attach × $40 × ~110 arrivals/day) |
| No-show recovery (F2) | ~$120–130K protected (deposits on ~half of 5% no-shows) |
| RevPAR uplift (F9) | Six-figure (3–5% on a ~$90 base) — from responsive **manual** rate changes |
| Housekeeping/maintenance | Recurring labor savings + recovered minibar + avoided comps/downtime |
| POS ancillary (F8, resort) | +5–10% F&B check average, compounding per cover |

The point is not the precise numbers — it is that **each area targets a distinct, recurring lever**, and they stack. Even conservatively, the combined revenue upside and cost savings materially exceed the build-and-run cost.

---

## 5. Our differentiation (especially in Bangladesh)

- **Offline-first** — keeps arrivals, housekeeping, and payments running through the Wi-Fi dead zones and outages common locally.
- **Local payment rails** — bKash, Nagad, SSLCommerz across front desk, reservations, and POS — which global vendors don't serve.
- **Integrated resort + revenue tooling on mobile** — F&B POS, manual rates, and channel sync in one app, rare among local vendors.
- **Bilingual, localization-ready** — Bangla/English UI, local VAT, multi-language frontline teams.
- **Global-standard, locally-priced** — matches what Oracle, Mews, Shiji, and Stayntouch offer, tuned to the BD market.

---

## 6. Phased rollout (de-risked)

| Phase | Focus | Areas |
|-------|-------|-------|
| **Phase 1 — Operational spine** | Prove core value fast | Auth/RBAC, **F1 Front Desk**, **F3 Housekeeping** |
| **Phase 2 — Flow & timeliness** | Keep inventory accurate, work timely | **F2 Reservations**, **F4 Maintenance**, **F6 Notifications** |
| **Phase 3 — Coordinate** | Accountability glue | **F5 Task Management & Comms** |
| **Phase 4 — Revenue upside** | Highest-value resort/revenue features | **F8 F&B POS**, **F9 manual rates + channel sync** |

Each phase ships standalone value, so ROI starts in Phase 1 and compounds. A single-property pilot validates before chain rollout.

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
5. **Segment priority** — city hotel (F1–F6) vs. resort (adds F8 F&B POS, F9 rates) as the beachhead.

---

## 9. Bottom line

> **Atrium Staff turns frontline operations from a fixed-cost, desk-bound process into a mobile, real-time, revenue-generating system — matching the global standard while owning a wide-open local market. It increases revenue at every guest touchpoint, cuts labor and operating cost across every department, and starts paying back in Phase 1.**
