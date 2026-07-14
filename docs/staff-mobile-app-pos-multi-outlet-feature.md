# Point of Sale (F&B core) on Mobile — Feature Deep-Dive

**Product:** Atrium Staff (Hotel / Resort Operations Mobile App)
**Feature area:** F8 — Mobile Point of Sale (F&B core)
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-14
**Status:** Draft v1.1
**Related:** [Staff Mobile App — Feature Scope](staff-mobile-app-feature-scope.md) · [Front Desk — Deep-Dive](staff-mobile-app-front-desk-feature.md) · [Competitive Feature Research](../HOTEL-~1.MD)

> **Scope decision (2026-07-14):** this is **F&B core POS only**. **Spa/golf operations and retail POS are out of scope**, and reporting is reduced to basic consolidated sales totals (no dashboards/analytics). See [Feature Prioritization](staff-mobile-app-feature-prioritization.md).

---

## 1. Executive Summary

**Mobile Point of Sale (F&B core)** extends the staff app across the resort's **food-and-beverage outlets** — restaurants, bars, poolside, room service — with mobile/tableside ordering, payment, and charge-to-room, consolidated into one system. For a resort, where a large share of revenue is earned *outside* the room, this captures F&B spend at the point of service and posts it cleanly to the guest folio.

The value is twofold: **revenue capture** (order and upsell anywhere, charge to room frictionlessly, never miss a post) and **operational unification** (one F&B POS instead of siloed terminals). Tableside/mobile ordering also speeds service and turns tables faster.

Vendors anchor this in the resort segment: Hotelogix (multiple POS outlets — room service, restaurants; table/room/takeaway modes), Smart Software BD (mobile POS + tableside ordering, inventory & recipe costing), and Agilysys InfoGenesis (multi-venue F&B POS with consolidated reporting). *(Spa/golf and retail POS — offered by Agilysys/Maestro — are out of scope here.)*

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What staff can do from the device |
|---|------------|-----------------------------------|
| 1 | **Multi-outlet F&B POS** | Operate room service, restaurants, bars, poolside — with table / room / takeaway modes. |
| 2 | **Mobile / tableside ordering** | Take orders at the table or poolside; fire to kitchen/bar; process payment on the spot. |
| 3 | **Charge-to-room / folio posting** | Post any F&B charge directly to the guest folio ([front desk](staff-mobile-app-front-desk-feature.md) sees it live). |
| 4 | **Consolidated F&B sales totals** | Basic consolidated sales across F&B outlets *(advanced analytics/dashboards out of scope)*. |
| 5 | **Inventory & recipe/menu costing** | Track stock consumption and recipe costs from sales. |
| 6 | **Payments (incl. local rails)** | Cards, tap-to-pay, and bKash/Nagad/SSLCommerz for the BD market. |
| 7 | **Upsell & modifiers at point of sale** | Prompt add-ons, upsizes, and pairings at the moment of order. |

### 2.2 In scope (this release)
Multi-outlet **F&B** POS (table/room/takeaway), mobile/tableside ordering with kitchen/bar firing, charge-to-room/folio posting, payment capture, basic inventory/recipe costing, consolidated F&B sales totals, upsell/modifiers, offline support.

### 2.3 Out of scope (this release)
**Spa & golf operations, retail POS,** casino gaming, full supply-chain procurement, complex banquet/catering contracting, standalone restaurant-only deployments, and **multi-venue analytics/dashboards beyond basic sales totals.** (See §9.)

### 2.4 Key assumptions
- PMS folio-post API is available for charge-to-room.
- Kitchen/bar display or printer integration exists for order firing.
- A payment path (incl. local gateways) is in place within PCI scope.

---

## 3. Capability Deep-Dive — The F&B POS Features

Each capability is elaborated — **what it is**, **how it works in Atrium**, **why it matters operationally**, and the **revenue/cost angle** — with vendor precedents noting the bar to clear.

### 3.1 Manage multiple F&B outlets (table / room / takeaway)
*Precedent: Hotelogix.*

**What it is.** Operating several F&B outlets — room service, restaurants, bars, poolside — from one app, each in the appropriate service mode (table, room, or takeaway).

**How it works in Atrium.** Staff select the outlet and mode; the app presents the right menu, workflow, and pricing. A single guest can accrue F&B charges across outlets to one folio, and each outlet's sales roll into the consolidated totals.

**Why it matters.** Resorts run many small F&B points; siloed POS terminals fragment sales and complicate charge-to-room. One F&B POS unifies the guest's spend and the property's F&B picture.

**Revenue / cost angle.** More capture points and clean charge-to-room lift ancillary revenue (revenue); one system replaces multiple POS licenses/terminals (cost).

### 3.2 Mobile POS & tableside ordering (with inventory & recipe costing)
*Precedent: Smart Software (BD).*

**What it is.** Taking orders and processing payments at the table or poolside on a handheld, backed by inventory tracking and recipe/menu costing.

**How it works in Atrium.** The server takes the order tableside; it fires to the kitchen/bar instantly; payment or charge-to-room is completed without walking to a terminal. Each sale decrements inventory and rolls up against recipe costs for margin visibility.

**Why it matters.** Tableside ordering removes the walk-to-terminal round trips that slow service and cause order errors; it also opens the moment for upsell. Inventory/recipe costing turns POS from a cash register into a margin-management tool.

**Revenue / cost angle.** Faster turns + tableside upsell lift covers and check averages (revenue); inventory/cost tracking curbs waste and protects margin (cost).

### 3.3 Multi-outlet F&B POS with consolidated sales
*Precedent: Agilysys InfoGenesis.*

**What it is.** POS spanning F&B outlets — restaurants, bars, poolside, room service — with consolidated sales totals across venues.

**How it works in Atrium.** Every F&B venue operates on the same platform; sales, discounts, and voids consolidate into one sales view. Management sees total F&B performance, not per-terminal fragments. *(Deep analytics/dashboards are out of scope.)*

**Why it matters.** The value is the single pane: one menu/pricing engine, one guest-F&B-spend view across the resort. It's the difference between managing outlets and managing an F&B business.

**Revenue / cost angle.** Consolidated visibility supports menu, pricing, and staffing decisions (revenue + cost); one platform reduces integration and maintenance overhead (cost).

### 3.4 Spa & golf operations — ❌ REMOVED FROM SCOPE
*(Removed 2026-07-14. Precedent: Agilysys, Maestro. Spa/golf scheduling and resource coordination are high-value resort features but out of scope for v1 — the app covers F&B POS only.)*

---

## 4. Why This Matters

### 4.1 Resort F&B revenue lives outside the room
A large share of resort spend is food & beverage. Capturing it at the point of service — and posting it cleanly to the folio — is where this feature pays for itself.

### 4.2 Charge-to-room is the frictionless spend engine
When a guest can charge F&B, anywhere, to their room in seconds, they spend more. Reliable folio posting is the mechanism that turns convenience into ancillary revenue.

### 4.3 One F&B platform beats many terminals
Unifying F&B outlets removes silos, gives one guest-spend view, and cuts the cost and complexity of running many POS systems.

### 4.4 A competitive opening in the BD market
Integrated resort F&B POS with charge-to-room and local payment rails is rare among BD vendors. Atrium can lead locally in the resort segment.

---

## 5. Operations It Improves

| Operation today | Pain | With Mobile F&B POS |
|-----------------|------|--------------------------------|
| **Tableside/poolside service** | Walk-to-terminal round trips | Order + pay at the table; fire to kitchen instantly. |
| **Charge-to-room** | Manual, error-prone posting | Clean folio posting from any F&B outlet. |
| **Multi-outlet sales view** | Siloed per-terminal data | Consolidated F&B sales totals. |
| **Inventory & margin** | Manual counts, unknown margin | Sales-driven inventory + recipe costing. |
| **Upselling F&B** | Missed at point of order | Modifiers/upsells prompted at order time. |
| **Payments** | Terminal-bound; no local rails | Mobile pay incl. bKash/Nagad/SSLCommerz. |

---

## 6. How It Increases Revenue

1. **Frictionless charge-to-room lifts spend.** Effortless room-charging across F&B outlets increases ancillary spend per guest.
2. **Tableside upsell.** Modifiers, add-ons, and pairings prompted at the point of order lift check averages.
3. **Faster table turns.** Tableside ordering and payment speed service, increasing covers per shift.
4. **Never-miss posting.** Reliable folio capture recovers charges that manual posting loses.

> **Illustrative model:** If tableside ordering and prompted upsells lift the average F&B check by even 8% across a resort's F&B outlets, the ancillary revenue gain compounds across every cover, every day — on top of recovered charge-to-room posts. *(Figures illustrative; validate against property data.)*

---

## 7. How It Reduces Operational Cost

1. **Fewer POS systems.** One F&B POS platform replaces multiple licensed terminals and their integration/maintenance overhead.
2. **Less waste, protected margin.** Inventory and recipe costing curb over-ordering, theft, and portioning loss.
3. **Faster service, same staff.** Tableside efficiency raises covers-per-server, improving labor productivity.
4. **Cleaner reconciliation.** Consolidated sales totals and folio posting reduce end-of-day reconciliation labor and disputes.

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Ancillary revenue | F&B spend per occupied room | ↑ measurable |
| Check average | Average F&B check | ↑ 5–10% |
| Service speed | Table turn time | ↓ measurable |
| Margin control | Food/beverage cost variance vs. recipe | ↓ / on target |
| Posting accuracy | Charge-to-room posting errors/disputes | ↓ near zero |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| Folio-post API | Charge-to-room needs PMS post path | Integrate folio API; charge queue fallback |
| Kitchen/bar firing | KDS/printer integration required | Support common KDS/printer protocols |
| Payments / PCI | Mobile payment expands PCI scope | P2PE/tap-to-pay; hosted flows; tokenization |
| Local gateways | bKash/Nagad/SSLCommerz for BD | Prioritize local-rail integration |
| Offline service | Poolside/remote outlets lose Wi-Fi | Offline order queue with auto-sync |
| Scope creep | Full restaurant/back-office ERP; spa/golf/retail | Keep F&B-core; defer full F&B ERP and non-F&B outlets |

---

## 10. Release Phasing

| Phase | Scope |
|-------|-------|
| **MVP** | Core F&B POS (table/room/takeaway), tableside ordering + kitchen/bar firing, charge-to-room/folio posting, payment capture, basic offline. |
| **Phase 2** | Consolidated F&B sales totals, upsell/modifiers, inventory + recipe costing, local payment gateways. |
| **Phase 3** | Advanced margin costing. *(Spa/golf, retail, and multi-venue analytics are out of scope.)* |

---

## 11. Open Questions

- Which F&B outlets are in scope for launch? *(Spa/golf/retail are out of scope.)*
- Is there an existing POS to integrate/replace, and what KDS/printer hardware is in place?
- Which payment providers and local gateways for the BD market?
- Is inventory/recipe costing required at launch or a later phase?
- Resort-only, or must this also serve standalone F&B venues?

---

## 12. One-Line Business Case

> **Mobile F&B POS captures the resort F&B revenue that lives outside the room — order and upsell anywhere, charge to the folio in seconds, and run every F&B outlet on one platform with a single sales picture.**
