# Atrium Staff — Project Overview & Documentation Index

**Product:** Atrium Staff (Hotel / Resort Operations Mobile App)
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-15
**Status:** Draft v1.0
**Purpose:** The **front door** to the project. Start here to understand what Atrium Staff is, how the documentation is organized, and where to go for any detail.

---

## 1. What Atrium Staff is — in one paragraph

**Atrium Staff** is a mobile-first operations app that puts the whole property in every staff member's hand. It replaces radios, paper worksheets, and desk-bound PMS terminals with one role-adaptive app covering front desk, reservations, housekeeping, maintenance, task/communication, notifications, F&B POS, a **cost-and-control layer** (labor scheduling, preventive maintenance, owner analytics, inventory, compliance, feedback, training, safety), and **resort events & activities** — with cross-cutting **barcode/NFC scanning** speeding POS and inventory throughout. Its **primary objective is to reduce manual operational cost** — manpower, time, and error — while lifting revenue at every guest touchpoint. It is **one codebase, role-driven**: at login, the signed-in role and department resolve which tabs, screens, and actions the user even sees (least privilege / least clutter). It is built **offline-first** with **local payment rails** (bKash / Nagad / SSLCommerz) for the Bangladesh market, where almost no vendor ships a feature-complete staff app today.

> **The three numbers it moves:** guest satisfaction ↑, revenue capture ↑, labor + operating cost ↓.

---

## 2. How the documentation is organized

The docs fall into three layers. Read top-to-bottom for a full picture, or jump via the [reading paths](#7-reading-paths-by-audience) in §7.

```
Atrium Staff docs
├── 0. START HERE
│   └── Overview & Index ......................... (this document)
├── 1. STRATEGY & PLANNING
│   ├── Executive Summary ........................ the business case
│   ├── Feature Scope ............................ what's in / out, NFRs, data model
│   └── Feature Prioritization ................... ranking + priority weighting
├── 2. EXPERIENCE & ARCHITECTURE
│   ├── Navigation & Feature Map ................. information architecture (tabs → screens)
│   ├── Role-Based Design & Access .............. RBAC, per-role UI, manager Overview
│   └── UI Design Specification (design.md) ...... screen-by-screen spec for Stitch
└── 3. FEATURE DEEP-DIVES (one per feature, F1–F18)
    └── 18 documents — scope, ops impact, revenue, cost, KPIs, delivered scope, risks
```

*External reference (not part of the app spec):* the [Competitive Feature Research](HOTEL-~1.MD) is a **draft market-research guideline only** — it maps what other vendors ship and informed feature selection. It is **not a specification of Atrium Staff** and should be read as background, not as product scope.

---

## 3. Document catalog

### 3.1 Strategy & planning

| Doc | What it gives you |
|-----|-------------------|
| [Executive Summary](docs/staff-mobile-app-executive-summary.md) | The business case: opportunity, feature areas, revenue + cost levers (incl. the per-feature cost/revenue table), risks, decisions needed. |
| [Feature Scope](docs/staff-mobile-app-feature-scope.md) | Purpose, personas, in/out of scope, the extended features (§3A, F9–F18), non-functional requirements, integrations, data model, delivery. |
| [Feature Prioritization](docs/staff-mobile-app-feature-prioritization.md) | Ranking of every feature by productivity + revenue + cost impact — the relative weight each carries within the complete package. |

### 3.2 Experience & architecture

| Doc | What it gives you |
|-----|-------------------|
| [Navigation & Feature Map](docs/staff-mobile-app-navigation-map.md) | The information architecture: the 5-tab shell, which screens sit under each tab, feature→screen map, role→tab visibility, and how features map onto the shell. |
| [Role-Based Design & Access](docs/staff-mobile-app-role-based-design.md) | Personas, the feature×role visibility matrix, per-role navigation, hard invisibility rules, and the Line Manager operational Overview. |
| [UI Design Specification](design.md) | The Stitch-ready screen-by-screen spec: design tokens, components, wireframes + prompts for every screen (S1–S34), and global states. |

### 3.3 Feature deep-dives (F1–F18)

Each deep-dive follows the same 12-section template: executive summary · scope · capability deep-dive · why it matters · operations improved · how it increases revenue · how it reduces cost · KPIs · dependencies & risks · scope delivered · open questions · one-line business case.

| # | Feature | Deep-dive | Layer |
|---|---------|-----------|-------|
| F1 | Front Desk | [front-desk](docs/staff-mobile-app-front-desk-feature.md) | Guest-facing loop |
| F2 | Reservations | [reservations](docs/staff-mobile-app-reservations-management-feature.md) | Guest-facing loop |
| F3 | Housekeeping | [housekeeping](docs/staff-mobile-app-housekeeping-management-feature.md) | Guest-facing loop |
| F4 | Maintenance (reactive) | [maintenance-work-orders](docs/staff-mobile-app-maintenance-work-orders-feature.md) | Guest-facing loop |
| F5 | Task Management & Communication | [task-management-communication](docs/staff-mobile-app-task-management-communication-feature.md) | Guest-facing loop |
| F6 | Notifications & Alerts | [notifications-alerts](docs/staff-mobile-app-notifications-alerts-feature.md) | Guest-facing loop |
| F8 | POS (F&B core) | [pos-multi-outlet](docs/staff-mobile-app-pos-multi-outlet-feature.md) | Guest-facing loop |
| F9 | Labor Management | [labor-management](docs/staff-mobile-app-labor-management-feature.md) | Cost & control |
| F10 | Preventive Maintenance & Assets | [preventive-maintenance-assets](docs/staff-mobile-app-preventive-maintenance-assets-feature.md) | Cost & control |
| F11 | Management Analytics / Owner KPIs | [management-analytics-owner-kpis](docs/staff-mobile-app-management-analytics-owner-kpis-feature.md) | Cost & control |
| F12 | Inventory & Par-Stock | [inventory-par-stock](docs/staff-mobile-app-inventory-par-stock-feature.md) | Assurance |
| F13 | Digital Inspections & Compliance | [inspections-compliance](docs/staff-mobile-app-inspections-compliance-feature.md) | Assurance |
| F14 | Guest Feedback & Service Recovery | [guest-feedback-service-recovery](docs/staff-mobile-app-guest-feedback-service-recovery-feature.md) | Experience |
| F15 | SOP / Training Knowledge Base | [sop-training-knowledge-base](docs/staff-mobile-app-sop-training-knowledge-base-feature.md) | Enablement |
| F16 | Lost & Found & Staff Safety | [lost-found-staff-safety](docs/staff-mobile-app-lost-found-staff-safety-feature.md) | Enablement |
| F17 | NFC & Barcode Scanning | [nfc-barcode-scanning](docs/staff-mobile-app-nfc-barcode-scanning-feature.md) | Enabler (cross-cutting) |
| F18 | Events, Activities & Banquet Management | [events-activities](docs/staff-mobile-app-events-activities-feature.md) | Ancillary revenue (resort) |

> **Numbering note:** features use a **product-area scheme (F1–F18)**. **F17 (NFC & Barcode Scanning)** is a **cross-cutting enabler** — it is surfaced inside other features' screens (mostly F8 POS and F12 Inventory) rather than being a tab of its own. **F18 (Events, Activities & Banquet Management)** is a full feature area with its own screens, in the ⋯ More tab. **F7 is intentionally unused** as a product area — supervisor/manager tooling is not a standalone feature but is expressed through the [Role-Based Design](docs/staff-mobile-app-role-based-design.md) (manager Overview, assign/monitor). The [Feature Scope](docs/staff-mobile-app-feature-scope.md) doc additionally carries an **older internal taxonomy** (its §3 F1–F9 = Auth, Task, Guest-request, Room-board, Comms, Notifications, Supervisor, Offline, Audit); the product-area scheme is canonical and reconciled inside that doc.

---

## 4. Feature catalog at a glance

Every feature, its primary user, and the economic lever it pulls. All ship as part of the complete package. Deep-dives linked in [§3.3](#33-feature-deep-dives-f1f18).

| # | Feature | Primary user | Primary lever |
|---|---------|--------------|---------------|
| F1 | Front Desk | Front desk | Upsell at arrival + labor/arrival |
| F2 | Reservations | Front desk / mgr | No-show recovery + occupancy |
| F3 | Housekeeping | Housekeeping | Faster room turns + labor productivity |
| F4 | Maintenance (reactive) | Engineering | Downtime reduction |
| F5 | Task Mgmt & Comms | All | Coordination cost / accountability |
| F6 | Notifications | All | Timeliness of every action |
| F8 | POS (F&B core) | F&B / resort | Resort ancillary revenue |
| F9 | Labor Management | Managers / all | **Labor cost** — the largest controllable line |
| F10 | Preventive Maintenance & Assets | Engineering | Downtime & asset-life protection |
| F11 | Management Analytics / Owner KPIs | GM / owner | **Owner visibility** — waste made cuttable |
| F12 | Inventory & Par-Stock | HK / F&B / mgr | Margin — shrinkage & waste |
| F13 | Inspections & Compliance | HK / mgr | Liability & rework |
| F14 | Guest Feedback & Recovery | Front desk / mgr | Retention / review scores |
| F15 | SOP / Training | All | Ramp time & error cost |
| F16 | Lost & Found & Staff Safety | All | Liability & compliance |
| F17 | NFC & Barcode Scanning | F&B / stores / all | Capture speed & accuracy (quick POS + inventory) |
| F18 | Events, Activities & Banquet Management | Events / F&B / mgr | Ancillary event & activity revenue + coordination cost |

**Cross-cutting foundations (every feature depends on these):** authentication & RBAC · offline-first with auto-sync · real-time PMS integration · one guest/room/folio model · notifications (F6) as a global layer · Staff SOS (F16) as a global layer · bilingual (Bangla/English) + local payment rails.

---

## 5. The app in one picture

**Navigation shell:** `Login → Home → 5-tab bottom bar`, role-adaptive. Full detail in the [Navigation & Feature Map](docs/staff-mobile-app-navigation-map.md).

| Tab | Houses | Features |
|-----|--------|----------|
| 🏠 **Home** | Launcher (frontline) / Operational Overview (managers) | app shell |
| ✅ **Tasks** | Room board, work orders (reactive + PM), inspections, my tasks | F3, F4, F10, F13, F5 |
| 🛎️ **Desk** | Check-in/out, folio, payment, key, reservations calendar | F1, F2 |
| 💬 **Inbox** | Messaging, notification history, guest feedback | F5, F6, F14 |
| ⋯ **More** | POS, events & activities, my shifts/roster, inventory, assets, owner KPIs, knowledge base, lost & found, settings | F8, F18, F9, F11, F12, F15, F16 |

*Cross-cutting: **F17 NFC & Barcode Scanning** is surfaced inside POS, inventory, assets, rooms, and Desk (not a tab); **F6 Notifications** and **F16 Staff SOS** are global layers on every screen.*

**Roles** (who sees what — full matrix in [Role-Based Design](docs/staff-mobile-app-role-based-design.md)): Front Desk Agent · Housekeeping Attendant · Maintenance Tech · F&B Staff · Line Manager (department-scoped) · Duty Manager / GM · Owner (F11 KPIs only).

---

## 6. The complete package at a glance

Atrium Staff ships as **one complete package** — every feature delivered together, not staged. The features group into themes that together cover the guest-facing operating loop and turn the same platform into a manual-cost-reduction engine, plus a cross-cutting scan enabler. Relative weighting (which features carry the most impact) is in [Prioritization](docs/staff-mobile-app-feature-prioritization.md).

| Theme | Features |
|-------|----------|
| **Operational spine** | F1 Front Desk, F3 Housekeeping |
| **Flow & timeliness** | F2 Reservations, F4 Maintenance, F6 Notifications |
| **Coordination** | F5 Task Management & Comms |
| **Resort upside** | F8 F&B POS, F18 Events, Activities & Banquet |
| **Cost & control** | F9 Labor, F10 Preventive Maintenance, F11 Owner KPIs |
| **Assurance** | F12 Inventory, F13 Inspections & Compliance |
| **Experience & enablement** | F14 Feedback, F15 SOP/Training, F16 Lost & Found + Safety |
| **Cross-cutting enabler** | F17 NFC & Barcode Scanning (inside F8, F12, F10, F3, F1) |

---

## 7. Reading paths by audience

- **Executive / stakeholder / investor:** [Executive Summary](docs/staff-mobile-app-executive-summary.md) → this index §4–§6 → skim any deep-dive's §1 and §12.
- **Product manager:** [Feature Scope](docs/staff-mobile-app-feature-scope.md) → [Prioritization](docs/staff-mobile-app-feature-prioritization.md) → the relevant [feature deep-dives](#33-feature-deep-dives-f1f18).
- **Designer:** [Navigation & Feature Map](docs/staff-mobile-app-navigation-map.md) → [Role-Based Design](docs/staff-mobile-app-role-based-design.md) → [UI Design Specification](design.md).
- **Engineer:** [Feature Scope](docs/staff-mobile-app-feature-scope.md) (§4 NFRs, §5 integrations, §7 data model) → [Role-Based Design](docs/staff-mobile-app-role-based-design.md) (§6 implementation notes) → the feature deep-dive you're building.
- **Market / competitive context (optional):** [Competitive Feature Research](HOTEL-~1.MD) — *draft guideline only, not app scope.*

---

## 8. Conventions & status

- **Feature IDs** use the product-area scheme F1–F18 (F7 unused; F17 is a cross-cutting enabler — see the numbering note in [§3.3](#33-feature-deep-dives-f1f18)).
- **Currency** in illustrative models is ৳ (BDT); all economic figures are illustrative and must be validated against real property data before external use.
- **Status:** all documents are **Draft** (dates and versions in each doc's header). This index is the canonical entry point; update it when documents are added or renamed.
- **Owner:** rdasgupta@apexdmit.com.

---

## 9. Open cross-project decisions

Consolidated from the individual docs — the choices that unblock the build:

1. **Launch PMS(es)** and whether they support real-time writes (not just reads).
2. **Launch market & model** — BD-first single-property pilot, then chain.
3. **Payment + channel partners** — which local gateways / channel manager are mandatory for v1.
4. **Device strategy** — company-issued vs. BYOD, and MDM approach.
5. **Segment priority** — city hotel (F1–F6) vs. resort (adds F8) as the beachhead.
6. **Owner analytics scope** — single-property vs. above-property/multi-property roll-up for F11.
7. **Labor policy** — is geofenced clock-in (F9) acceptable to staff/labour agreements, and at what geofence radius/grace.

---

> **Atrium Staff turns frontline operations from a fixed-cost, desk-bound process into a mobile, real-time, revenue-generating system — matching the global standard while owning a wide-open local market.**
