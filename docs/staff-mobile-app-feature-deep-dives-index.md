# Atrium Staff — Feature Deep-Dives Index

**Product:** Atrium Staff (Hotel / Resort Operations Mobile App)
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-14
**Status:** Draft v1.0

This is the index for the per-feature deep-dive documents that expand **Part B — Staff / Operations Mobile App** of the [Competitive Feature Research](../HOTEL-~1.MD). Each deep-dive follows the same structure: scope, capability-by-capability detail (what it is → how it works in Atrium → why it matters → revenue/cost angle, with vendor precedents), why it matters, operations improved, revenue impact, cost reduction, KPIs, dependencies & risks, phased release, open questions, and a one-line business case.

Start with the [Feature Scope](staff-mobile-app-feature-scope.md) for the overall app; use this index to navigate the eight in-scope feature areas.

> **Scope decision (2026-07-14):** the "less necessary" features were **removed from scope** (see [Feature Prioritization](staff-mobile-app-feature-prioritization.md)). **F7 Dashboards, Reporting & Analytics is dropped entirely.** F4, F8, and F9 are **scoped down** (see notes). Kept: F1, F2, F3, F5, F6, F8 core F&B POS, F9 manual rates + channel sync, F4 reactive work orders.

---

## The eight in-scope feature areas (Part B)

| # | Feature area | Deep-dive | Core value in one line |
|---|--------------|-----------|------------------------|
| F1 | Front Desk on Mobile | [Deep-dive](staff-mobile-app-front-desk-feature.md) | Move reception to where the guest and the money are. |
| F2 | Reservations Management | [Deep-dive](staff-mobile-app-reservations-management-feature.md) | Keep the booking book accurate and actionable from anywhere. |
| F3 | Housekeeping Management | [Deep-dive](staff-mobile-app-housekeeping-management-feature.md) | Turn the biggest labor cost into the fastest revenue lever. |
| F4 | Maintenance & Work Orders *(reactive only)* | [Deep-dive](staff-mobile-app-maintenance-work-orders-feature.md) | Shorten broken-to-fixed. *(PM & asset history out of scope.)* |
| F5 | Task Management & Internal Communication | [Deep-dive](staff-mobile-app-task-management-communication-feature.md) | The accountability layer that makes the operation visible and owned. |
| F6 | Notifications & Alerts | [Deep-dive](staff-mobile-app-notifications-alerts-feature.md) | The nervous system that makes every other feature timely. |
| F8 | Point of Sale *(F&B core)* | [Deep-dive](staff-mobile-app-pos-multi-outlet-feature.md) | Capture the resort revenue that lives outside the room. *(Spa/golf/retail out of scope.)* |
| F9 | Rate & Channel Management *(manual)* | [Deep-dive](staff-mobile-app-revenue-management-feature.md) | Manual rates + channel sync on the phone. *(Automated dynamic pricing out of scope.)* |

**Removed from scope:** **F7 Dashboards, Reporting & Analytics** (deep-dive deleted); F9 automated dynamic-pricing engine; F8 spa/golf/retail; F4 preventive maintenance & asset history; multi-property/cluster; lost & found; shift/attendance; concierge tasks; operational analytics.

---

## How the areas relate

- **F1 Front Desk** and **F2 Reservations** split the guest-booking timeline: F1 governs *today's* arrivals/departures and the folio; F2 governs *future* inventory (calendar, blocks, create/edit, deposits).
- **F3 Housekeeping** and **F4 Maintenance** produce the room state F1 sells; the housekeeping *room-ready* alert and maintenance *OOO* control feed directly into front-desk and reservations availability.
- **F5 Task Management** is the connective layer: F1–F4 all generate cross-department work that F5 routes and tracks.
- **F6 Notifications** makes F1–F5 *timely* by pushing the right event to the right person. *(There is no analytics/dashboard layer — F7 was removed; the Home tab is a lightweight operational launcher, not KPIs.)*
- **F8 POS** (resort F&B ancillary revenue) and **F9 Rate & Channel Management** (manual rates/distribution) are the revenue layers; F8 charges post into the same F1 folio.

---

## Cross-cutting themes

- **BD-market opening.** Every area notes where Bangladeshi vendors under-serve the mobile capability — especially offline mode, local payment rails (bKash/Nagad/SSLCommerz), and integrated resort tooling. This is Atrium's consistent local differentiation.
- **Scope discipline.** Only tactical/manual controls are built (e.g., manual rate edits); full optimization/analytics engines (RMS, BI/dashboards, CMMS, procurement) are integrated or **out of scope**, not rebuilt.
- **Shared foundations.** RBAC, offline-with-auto-sync, PMS real-time read/write (or middleware), audit logging, and push infrastructure recur across every area — see the [Feature Scope](staff-mobile-app-feature-scope.md) non-functional requirements.
- **Illustrative economics.** Revenue/cost models in each doc are labeled illustrative and must be validated against real property data before external use.

---

## Suggested MVP ordering

Aligned with the [Feature Prioritization](staff-mobile-app-feature-prioritization.md) and [Executive Summary](staff-mobile-app-executive-summary.md):

1. **Foundation first:** Auth/RBAC + **F1 Front Desk** (core check-in/out, folio) and **F3 Housekeeping** (room status) — the operational spine.
2. **Then flow:** **F2 Reservations**, **F4 reactive work orders**, **F6 Notifications** — keep inventory accurate and work timely.
3. **Then coordinate:** **F5 Task Management & Comms** — the accountability glue.
4. **Then revenue upside:** **F8 F&B POS** and **F9 manual rates + channel sync** — highest value for the resort/revenue segment.

---

## Open cross-feature questions

- Which PMS(es) at launch, and do they support real-time **writes** (folio, room status, rates), not just reads?
- Single-property pilot for v1 (multi-property is out of scope) — confirm.
- BD-first market: which local payment gateways and channel manager are mandatory?
- BYOD vs. company-issued devices and MDM strategy across all frontline roles?
- Data residency, PCI approach, and photo/message retention policy across features?
