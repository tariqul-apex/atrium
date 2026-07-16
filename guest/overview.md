# Atrium Guest — Project Overview & Documentation Index

**Product:** Atrium Guest (Hotel / Resort Guest-Facing Mobile App)
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-16
**Status:** Draft v1.0
**Purpose:** The **front door** to the project. Start here to understand what Atrium Guest is, how the documentation is organized, and where to go for any detail.
**Companion product:** [Atrium Staff — Overview & Index](../staff/overview.md)

---

## 1. What Atrium Guest is — in one paragraph

**Atrium Guest** is the guest-facing companion to Atrium Staff: one mobile surface that carries the guest through the entire stay lifecycle — **book → pre-register → check in → get a digital key → order, request, book, and chat → pay → check out → review and rebook** — without ever needing the front-desk queue. It is **one codebase, journey-stage-driven**: the app detects where the guest is in the stay (Discover & Book → Pre-Arrival → Arrival → In-Stay → Departure → Post-Stay) and surfaces only what is relevant now. Every guest action lands as live work on the staff side — requests become tasks (F5), orders hit the POS (F8), activity bookings fill the events calendar (F18), feedback triggers service recovery (F14) — making the two apps one platform with two surfaces. It is built for the Bangladesh market first — **Bangla/English bilingual, bKash / Nagad / SSLCommerz payment rails**, and crucially **app-less access (G15)**: every flow also works as a QR/web link, because most guests will not install an app for a two-night stay. Its **primary objectives are ancillary-revenue capture and front-desk load reduction** — monetizing the high-intent moments of the stay while deflecting the calls and queues that consume staff hours.

> **The three numbers it moves:** ancillary revenue per guest ↑, direct (commission-free) bookings ↑, front-desk contacts per stay ↓.

---

## 2. How the documentation is organized

The docs fall into three layers, mirroring the [Atrium Staff docs](../staff/overview.md). Read top-to-bottom for a full picture, or jump via the [reading paths](#7-reading-paths-by-audience) in §7.

```
Atrium Guest docs
├── 0. START HERE
│   ├── README .................................. feature list (G1–G15), journey stages, feature×stage matrix
│   └── Overview & Index ......................... (this document)
├── 1. STRATEGY & PLANNING
│   ├── Executive Summary ........................ the business case
│   ├── Feature Scope ............................ what's in / out, NFRs, data model
│   └── Feature Prioritization ................... ranking + priority weighting
├── 2. EXPERIENCE & ARCHITECTURE
│   ├── Navigation & Feature Map ................. information architecture (tabs → screens)
│   └── Journey-Stage Design ..................... stage-adaptive UI, personas, stage detection
└── 3. FEATURE DEEP-DIVES (one per feature, G1–G15)
    └── 15 documents — scope, journey impact, revenue, cost, KPIs, delivered scope, risks
```

*External reference (not part of the app spec):* the [Competitive Feature Research](../HOTEL-~1.MD) is a **draft market-research guideline only** — its **Part A** maps what other vendors ship on the guest side and informed feature selection. It is **not a specification of Atrium Guest**.

*Not yet written:* a screen-by-screen **UI Design Specification** (the guest-side analog of Atrium Staff's `design.md`) is a planned follow-up.

---

## 3. Document catalog

### 3.1 Strategy & planning

| Doc | What it gives you |
|-----|-------------------|
| [Executive Summary](docs/guest-mobile-app-executive-summary.md) | The business case: opportunity, feature areas, revenue + cost levers (incl. the per-feature table), adoption strategy, risks, decisions needed. |
| [Feature Scope](docs/guest-mobile-app-feature-scope.md) | Purpose, guest personas, in/out of scope, feature areas G1–G15, non-functional requirements, integrations, data model, delivery. |
| [Feature Prioritization](docs/guest-mobile-app-feature-prioritization.md) | Ranking of every feature by revenue, adoption/experience, and cost-to-serve impact — the relative weight each carries within the complete package. |

### 3.2 Experience & architecture

| Doc | What it gives you |
|-----|-------------------|
| [Navigation & Feature Map](docs/guest-mobile-app-navigation-map.md) | The information architecture: the 5-tab shell (Home · Services · Key · Explore · Inbox), feature→screen map, stage-based navigation, and key guest journeys. |
| [Journey-Stage Design](docs/guest-mobile-app-journey-stage-design.md) | Personas, the six journey stages, the feature×stage visibility matrix, stage-detection rules, per-stage Home composition, day-visitor and app-less variants, privacy rules. |

### 3.3 Feature deep-dives (G1–G15)

Each deep-dive follows the same 12-section template as the staff docs: executive summary · scope · capability deep-dive · why it matters · journey moments & operations improved · how it increases revenue · how it reduces cost · KPIs · dependencies & risks · scope delivered · open questions · one-line business case.

| # | Feature | Deep-dive | Layer |
|---|---------|-----------|-------|
| G1 | Booking & Pre-Arrival | [booking-pre-arrival](docs/guest-mobile-app-booking-pre-arrival-feature.md) | Journey spine |
| G2 | Mobile Check-In & Check-Out | [check-in-check-out](docs/guest-mobile-app-check-in-check-out-feature.md) | Journey spine |
| G3 | Digital Room Key | [digital-room-key](docs/guest-mobile-app-digital-room-key-feature.md) | Journey spine |
| G4 | In-Stay Service Requests | [in-stay-service-requests](docs/guest-mobile-app-in-stay-service-requests-feature.md) | In-stay services |
| G5 | Food & Beverage Ordering | [food-beverage-ordering](docs/guest-mobile-app-food-beverage-ordering-feature.md) | Ancillary revenue |
| G6 | Upsells, Offers & Add-Ons | [upsells-offers](docs/guest-mobile-app-upsells-offers-feature.md) | Ancillary revenue |
| G7 | Guest Messaging | [guest-messaging](docs/guest-mobile-app-guest-messaging-feature.md) | Communication |
| G8 | Notifications & Campaigns | [notifications-campaigns](docs/guest-mobile-app-notifications-campaigns-feature.md) | Communication |
| G9 | Payments, Folio & Tipping | [payments-folio-tipping](docs/guest-mobile-app-payments-folio-tipping-feature.md) | Money |
| G10 | Digital Guidebook & Property Info | [digital-guidebook-property-info](docs/guest-mobile-app-digital-guidebook-property-info-feature.md) | Self-service content |
| G11 | Interactive Property Map | [interactive-property-map](docs/guest-mobile-app-interactive-property-map-feature.md) | Self-service content |
| G12 | Activities, Spa & Experiences | [activities-spa-experiences](docs/guest-mobile-app-activities-spa-experiences-feature.md) | Ancillary revenue (resort) |
| G13 | Loyalty & Guest Profile | [loyalty-guest-profile](docs/guest-mobile-app-loyalty-guest-profile-feature.md) | Retention |
| G14 | Feedback & Service Recovery | [feedback-service-recovery](docs/guest-mobile-app-feedback-service-recovery-feature.md) | Retention |
| G15 | App-less Access (QR & Web Links) | [app-less-access](docs/guest-mobile-app-app-less-access-feature.md) | Enabler (cross-cutting) |

> **Numbering note:** features use a **product-area scheme (G1–G15)**, the guest-side counterpart of the staff app's F1–F18. **G15 (App-less Access)** is a **cross-cutting enabler** — it is the delivery channel for every other feature (the analog of staff F17 scanning), not a screen of its own. There is no guest analog of staff roles: what a guest sees is governed by **journey stage**, defined in the [Journey-Stage Design](docs/guest-mobile-app-journey-stage-design.md).

---

## 4. Feature catalog at a glance

Every feature, its primary journey stage, and the economic lever it pulls. All ship as part of the complete package. Deep-dives linked in [§3.3](#33-feature-deep-dives-g1g15).

| # | Feature | Primary stage | Primary lever |
|---|---------|---------------|---------------|
| G1 | Booking & Pre-Arrival | Discover & Book / Pre-Arrival | **Direct bookings** — OTA commission avoided + prepared arrivals |
| G2 | Check-In & Check-Out | Arrival / Departure | Queue elimination + front-desk labor per arrival |
| G3 | Digital Room Key | Arrival / In-Stay | Contactless journey completion + card/encoder cost |
| G4 | In-Stay Service Requests | In-Stay | Request capture & accountability; desk-call deflection |
| G5 | Food & Beverage Ordering | In-Stay | **F&B ancillary revenue** — order volume + average ticket |
| G6 | Upsells, Offers & Add-Ons | Pre-Arrival → In-Stay | **The #1 revenue feature** — monetizing high-intent moments |
| G7 | Guest Messaging | All stages | Problem interception + desk deflection |
| G8 | Notifications & Campaigns | All stages | Timeliness of every feature + perishable-inventory fill |
| G9 | Payments, Folio & Tipping | In-Stay / Departure | Bill transparency, local rails, dispute prevention |
| G10 | Digital Guidebook & Property Info | Pre-Arrival / In-Stay | Call deflection at zero marginal cost |
| G11 | Interactive Property Map | In-Stay | Discovery → folio revenue on resorts |
| G12 | Activities, Spa & Experiences | Pre-Arrival / In-Stay | Fills perishable high-margin capacity |
| G13 | Loyalty & Guest Profile | Post-Stay → next Book | **Repeat direct bookings** + personalization engine |
| G14 | Feedback & Service Recovery | In-Stay / Post-Stay | Detractor conversion + review scores |
| G15 | App-less Access (QR & Web) | All stages | **Adoption multiplier** on every other feature |

**Cross-cutting foundations (every feature depends on these):** stay-claim / account authentication · journey-stage engine · real-time sync with the Atrium Staff platform (one guest/room/folio model) · notifications (G8) as a global layer · app-less parity (G15) · bilingual Bangla/English · local payment rails · guest-PII minimization and consent.

---

## 5. The app in one picture

**Navigation shell:** `Claim stay / Sign in → Home → 5-tab bottom bar`, stage-adaptive. Full detail in the [Navigation & Feature Map](docs/guest-mobile-app-navigation-map.md).

| Tab | Houses | Features |
|-----|--------|----------|
| 🏠 **Home** | Stay card, stage CTAs (check in / room ready / check out), folio & account | G2, G9, G13 |
| 🛎️ **Services** | Service requests, F&B ordering | G4, G5 |
| 🔑 **Key** | Digital room key (center tab) | G3 |
| 🗺️ **Explore** | Guidebook, interactive map, activities & spa booking, offers | G10, G11, G12, G6 |
| 💬 **Inbox** | Guest↔property chat, notification history | G7, G8 |

*Cross-cutting: **G15 App-less Access** delivers every flow as QR/web links (not a tab); **G8 Notifications** and **G6 Offers** are global layers; **G1** runs before the stay exists (booking web/app entry); day visitors get a reduced shell (no Key, no stay card).*

**Stages** (what surfaces when — full matrix in [Journey-Stage Design](docs/guest-mobile-app-journey-stage-design.md)): Discover & Book · Pre-Arrival · Arrival · In-Stay · Departure · Post-Stay, plus the Day-visitor variant.

---

## 6. The complete package at a glance

Atrium Guest ships as **one complete package** — every feature delivered together, not staged. The features group into themes that together cover the guest journey end-to-end. Relative weighting is in [Prioritization](docs/guest-mobile-app-feature-prioritization.md).

| Theme | Features |
|-------|----------|
| **Journey spine** | G1 Booking & Pre-Arrival, G2 Check-In/Out, G3 Digital Key |
| **In-stay services** | G4 Service Requests |
| **Ancillary revenue** | G5 F&B Ordering, G6 Upsells & Offers, G12 Activities & Spa |
| **Communication** | G7 Messaging, G8 Notifications & Campaigns |
| **Money** | G9 Payments, Folio & Tipping |
| **Self-service content** | G10 Guidebook, G11 Property Map |
| **Retention** | G13 Loyalty & Profile, G14 Feedback & Recovery |
| **Cross-cutting enabler** | G15 App-less Access (delivery channel for G2, G4, G5, G7, G9, G14) |

**How the two apps connect** (one platform, two surfaces):

| Guest action (Atrium Guest) | Lands in (Atrium Staff) |
|------------------------------|--------------------------|
| G1 booking / pre-registration | [F2 Reservations](../staff/docs/staff-mobile-app-reservations-management-feature.md) · [F1 Front Desk](../staff/docs/staff-mobile-app-front-desk-feature.md) |
| G2 check-in / check-out | [F1 Front Desk](../staff/docs/staff-mobile-app-front-desk-feature.md) |
| G4 service request / issue | [F5 Task Management](../staff/docs/staff-mobile-app-task-management-communication-feature.md) · [F4 Maintenance](../staff/docs/staff-mobile-app-maintenance-work-orders-feature.md) · [F3 Housekeeping](../staff/docs/staff-mobile-app-housekeeping-management-feature.md) |
| G5 F&B order | [F8 POS](../staff/docs/staff-mobile-app-pos-multi-outlet-feature.md) |
| G7 chat message | [F5 Comms (staff Inbox)](../staff/docs/staff-mobile-app-task-management-communication-feature.md) |
| G12 activity / spa booking | [F18 Events & Activities](../staff/docs/staff-mobile-app-events-activities-feature.md) |
| G14 feedback / complaint | [F14 Guest Feedback & Service Recovery](../staff/docs/staff-mobile-app-guest-feedback-service-recovery-feature.md) |

---

## 7. Reading paths by audience

- **Executive / stakeholder / investor:** [Executive Summary](docs/guest-mobile-app-executive-summary.md) → this index §4–§6 → skim any deep-dive's §1 and §12.
- **Product manager:** [Feature Scope](docs/guest-mobile-app-feature-scope.md) → [Prioritization](docs/guest-mobile-app-feature-prioritization.md) → the relevant [feature deep-dives](#33-feature-deep-dives-g1g15).
- **Designer:** [Navigation & Feature Map](docs/guest-mobile-app-navigation-map.md) → [Journey-Stage Design](docs/guest-mobile-app-journey-stage-design.md) → deep-dives for the flows being designed.
- **Engineer:** [Feature Scope](docs/guest-mobile-app-feature-scope.md) (NFRs, integrations, data model) → [Journey-Stage Design](docs/guest-mobile-app-journey-stage-design.md) (stage engine) → the feature deep-dive you're building — plus the staff-side counterpart doc for any connected flow (§6 table).
- **Market / competitive context (optional):** [Competitive Feature Research, Part A](../HOTEL-~1.MD) — *draft guideline only, not app scope.*

---

## 8. Conventions & status

- **Feature IDs** use the product-area scheme G1–G15 (G15 is a cross-cutting enabler — see the numbering note in [§3.3](#33-feature-deep-dives-g1g15)). Staff-app features are referenced as F1–F18.
- **Currency** in illustrative models is ৳ (BDT) or $; all economic figures are illustrative and must be validated against real property data before external use.
- **Status:** all documents are **Draft** (dates and versions in each doc's header). This index is the canonical entry point; update it when documents are added or renamed.
- **Owner:** rdasgupta@apexdmit.com.

---

## 9. Open cross-project decisions

Consolidated from the individual docs — the choices that unblock the build:

1. **App-less-first or native-first** — is the PWA/web flow (G15) the primary surface with native as upgrade, or vice versa? (Affects G3 digital key, which is native-only.)
2. **Lock hardware** — which properties have (or will retrofit) BLE-capable locks for G3, and which lock vendor(s) to certify first.
3. **Payment partners** — which of bKash / Nagad / SSLCommerz (and which card acquirer) are mandatory for v1; tipping settlement model.
4. **Booking engine** — build the G1 direct-booking engine or integrate an existing one at launch.
5. **Messaging channels** — is WhatsApp Business API bridging (G7/G8) in v1 or fast-follow.
6. **Loyalty economics** — points earn/burn rates and who funds redemptions (property vs. platform).
7. **Guest PII & consent** — data-protection posture (retention, deletion, ID-document storage) for the BD market and foreign guests.
8. **Shared decisions with Atrium Staff** — launch PMS(es), launch market/model, and device strategy (see the [staff overview §9](../staff/overview.md)).

---

> **Atrium Guest puts the whole stay in the guest's hand — booking, arrival, key, spending, and feedback — turning high-intent moments into ancillary revenue and desk queues into self-service, on the app-less rails the Bangladesh market actually uses.**
