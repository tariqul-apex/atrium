# Atrium Staff — Feature Prioritization (Productivity & Revenue)

**Product:** Atrium Staff (Hotel / Resort Operations Mobile App)
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-14
**Status:** Draft v1.1
**Related:** [Feature Deep-Dives Index](staff-mobile-app-feature-deep-dives-index.md) · [Executive Summary](staff-mobile-app-executive-summary.md) · [Navigation & Feature Map](staff-mobile-app-navigation-map.md)

This document ranks the feature areas by **staff-productivity impact** and **business-revenue impact**, and records which features were **kept** vs. **removed from scope**.

> **Decision applied (2026-07-14):** the "less necessary" features were **removed from scope** across all docs and [design.md](../design.md). **F7 Dashboards, Reporting & Analytics is dropped entirely** (its deep-dive was deleted; the Home tab is now a lightweight operational launcher). F4, F8, and F9 are **scoped down**. See §3 for the exact removals.

---

## 1. Scoring matrix

Impact rated **High / Med / Low** for a typical property.

| # | Feature | Staff productivity | Revenue impact | Primary user | Status |
|---|---------|:---:|:---:|--------------|--------|
| F1 | [Front Desk](staff-mobile-app-front-desk-feature.md) | **High** | **High** | Front desk | 🟢 Kept — core |
| F3 | [Housekeeping](staff-mobile-app-housekeeping-management-feature.md) | **High** | **High** | Housekeeping | 🟢 Kept — core |
| F2 | [Reservations](staff-mobile-app-reservations-management-feature.md) | Med | **High** | Front desk / mgr | 🟢 Kept — revenue |
| F6 | [Notifications](staff-mobile-app-notifications-alerts-feature.md) | **High** | Med | All | 🟢 Kept — productivity |
| F5 | [Task Mgmt & Comms](staff-mobile-app-task-management-communication-feature.md) | **High** | Low–Med | All | 🟢 Kept — productivity |
| F8 | [POS (F&B core)](staff-mobile-app-pos-multi-outlet-feature.md) | Med | **High** *(resort)* | F&B / resort | 🟡 Kept — F&B only |
| F9 | [Rate & Channel (manual)](staff-mobile-app-revenue-management-feature.md) | Low *(few users)* | **High** | Revenue mgr | 🟡 Kept — manual only |
| F4 | [Maintenance (reactive)](staff-mobile-app-maintenance-work-orders-feature.md) | Med | Med *(protection)* | Engineering | 🟡 Kept — reactive only |
| ~~F7~~ | ~~Dashboards & Reporting~~ | Low *(frontline)* | Low *(indirect)* | Managers | 🔴 **Removed** |

---

## 2. 🟢 Kept — features that boost productivity AND revenue

### The core four — highest, most universal ROI

| Feature | Productivity driver | Revenue driver |
|---------|--------------------|--------------------|
| **F1 Front Desk** | Parallel/roving check-in; one device replaces PC + encoder + printer + POS | Upsell at the moment of highest intent; faster room-ready → more early check-ins |
| **F3 Housekeeping** | Auto-assignment, real-time status; replaces paper worksheets & radios (biggest frontline labor line) | Faster room turns = sellable inventory sooner; recovered minibar charges |
| **F2 Reservations** | Manage inventory anywhere; instant booking alerts | Pay-by-link deposits cut no-shows; better packing lifts occupancy |
| **F6 Notifications** | Productivity multiplier — makes every other feature *timely* | React to bookings/cancellations/messages in minutes, not hours |

### Kept, scoped-down

- **F5 Task Mgmt & Comms** — replaces radios/WhatsApp, adds cross-department accountability. High **productivity**; revenue only indirect. *(Operational analytics removed.)*
- **F8 POS — F&B core** — **high revenue for resorts** (charge-to-room + tableside upsell). *Spa/golf/retail removed.*
- **F9 Rate & Channel — manual** — **high revenue** (RevPAR) but few users. Manual rate edits + channel sync only; *automated dynamic-pricing engine removed (integrate an RMS if needed).*
- **F4 Maintenance — reactive** — reactive work orders (log/assign/close with photos). *Preventive maintenance & asset history removed.*

---

## 3. 🔴 Removed from scope

These were cut from all docs and design.md. "Removed" ≠ "never" — they can be revisited post-launch, but they are not in the current product.

| Removed | Was rated | Why removed |
|---|---|---|
| **F7 Dashboards, Reporting & Analytics** (whole feature) | Low prod / Low rev | Manager insight layer — doesn't make frontline staff productive or earn revenue directly. Deep-dive deleted; Home became a launcher. |
| **F9 automated dynamic-pricing engine** | High rev / tiny user base | Large build; integrate an RMS instead. Manual rates kept. |
| **F8 spa/golf + retail POS** | Resort-only | Kept F&B core POS only. |
| **F4 preventive maintenance + asset history** | Long-term value | Reactive work orders deliver most of F4's value. |
| **Multi-property / cluster views** | Chain-only | Single-property scope for v1. |
| **Lost & found, shift/attendance, concierge tasks** | Convenience | Low urgency; not job-critical. |
| **Operational analytics / reports / exports** | "Measure later" | No analytics layer in scope. |

---

## 4. MVP scope after removal

```
┌─────────────────────────────────────────────────────────┐
│  IN SCOPE (build)           │  REMOVED (not in product)   │
├─────────────────────────────┼─────────────────────────────┤
│  Auth / RBAC                │  F7 Dashboards & Reporting   │
│  F1 Front Desk              │  F9 automated pricing engine │
│  F3 Housekeeping            │  F4 PM + asset history       │
│  F2 Reservations            │  F8 spa/golf/retail POS      │
│  F6 Notifications           │  Multi-property / cluster    │
│  F5 Task & Comms            │  Lost & found, attendance,   │
│  F4 reactive work orders    │      concierge, analytics/   │
│  F8 F&B POS (resort)        │      reports                 │
│  F9 manual rates + channel  │                              │
└─────────────────────────────┴─────────────────────────────┘
```

**Home tab** = lightweight operational launcher (counts, alerts, quick actions) — **not** a KPI dashboard.

---

## 5. Bottom line

- **Core ROI (build first):** **F1, F3, F2, F6** — boost *both* productivity and revenue at every property.
- **Kept, scoped-down:** **F5** (glue), **F8** (F&B POS, resorts), **F9** (manual rates), **F4** (reactive repairs).
- **Removed:** **F7 dashboards/reporting/analytics**, automated pricing, PM/asset tracking, spa/golf/retail, multi-property, and the convenience modules.

**Net effect:** a leaner product focused entirely on features that make frontline staff more productive or directly earn/protect revenue — no management-insight or analytics overhead in v1.

---

## 6. Caveats

- Ratings are for a **typical property**; a large resort would raise F8/F9, a chain might justify revisiting multi-property. Re-score against the specific launch property.
- **Removed ≠ worthless.** F7 and the other cuts are genuinely useful later; they're simply not what earns back the build in the first release.
- Revenue/productivity magnitudes should be validated against real property data (see the illustrative models in each deep-dive).
