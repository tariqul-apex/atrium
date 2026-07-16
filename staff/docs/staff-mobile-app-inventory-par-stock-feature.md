# Inventory & Par-Stock on Mobile — Feature Deep-Dive

**Product:** Atrium Staff (Hotel / Resort Operations Mobile App)
**Feature area:** F12 — Inventory & Par-Stock
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-15
**Status:** Draft v1.0
**Related:** [Staff Mobile App — Feature Scope](staff-mobile-app-feature-scope.md) · [Housekeeping — Deep-Dive](staff-mobile-app-housekeeping-management-feature.md) · [POS — Deep-Dive](staff-mobile-app-pos-multi-outlet-feature.md) · [Competitive Feature Research](../HOTEL-~1.MD)

---

## 1. Executive Summary

**Inventory & Par-Stock** gives housekeeping, F&B, and stores staff a handheld way to track the consumable stock that leaks margin when it goes uncounted: minibar items, linen and towels, guest amenities, and F&B consumables. It sets a target level (a "par") for each item and location, lets staff count against it on the device, logs what was consumed from the room or outlet, and raises an alert before stock runs out.

Inventory is a **margin** feature, not a guest-experience one: every unbilled minibar can, every miscounted case of linen, and every emergency top-up bought at a premium is money lost quietly. A lightweight, mobile par-stock system turns "we think we're low" into "here's the count, here's what was used, here's what to reorder" — and closes the gap between consumption and the charge that should follow it.

Vendors treat consumption tracking as a staff-ops feature worth shipping: Quore (minibar consumption tracking), protel (minibar and laundry/towel tracking), and Smart Software (BD) (inventory and recipe costing inside POS) all cover pieces of it. Atrium unifies them at par-stock depth — deliberately short of full procurement.

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What staff can do from the device |
|---|------------|-----------------------------------|
| 1 | **Par-level tracking** | Hold a target par per item per location for minibar, linen, guest amenities, and F&B consumables. |
| 2 | **On-device stock counts** | Count on hand against par during a shift or cycle count — no clipboard, no back-office spreadsheet. |
| 3 | **Low-stock & reorder alerts** | Get flagged when an item drops below its reorder point; surface a reorder list to the storekeeper/manager. |
| 4 | **Consumption logged at source** | Record what was used from the room or outlet — minibar restock, towel/linen issue — at the point it happens. |
| 5 | **Housekeeping & POS touchpoints** | Log minibar restock inside [F3 Housekeeping](staff-mobile-app-housekeeping-management-feature.md); reflect F&B depletion from [F8 POS](staff-mobile-app-pos-multi-outlet-feature.md). |

### 2.2 In scope (this release)
Par levels per item/location, on-device counts and cycle counts, low-stock/reorder **alerts**, consumption logging from room and outlet, minibar-restock touchpoint in Housekeeping, F&B-depletion touchpoint from POS, category setup (minibar / linen / amenities / F&B), offline counting, RBAC.

### 2.3 Out of scope (this release)
Full **procurement and purchasing** — purchase orders, supplier catalogs, price lists, goods-receipt, and accounts-payable — is **not** in F12. This feature raises reorder **alerts**; it does **not** place purchase orders in v1. Warehouse/multi-echelon logistics, automatic supplier reordering, and full recipe/BOM costing beyond POS depletion are also out. (A purchase-order/supplier system is a future capability — see §9 Future in the [Feature Scope](staff-mobile-app-feature-scope.md); integrate with an existing store/procurement system where one exists — see §9.)

### 2.4 Key assumptions
- Par levels can be established per item per location during setup (a one-time effort).
- POS (F8) can expose item-level depletion for F&B consumables, or F&B counts stay manual in v1.
- Staff have devices; the app degrades gracefully offline for counting in storerooms and dead zones.

---

## 3. Capability Deep-Dive — The Inventory & Par-Stock Features

Each capability is elaborated — **what it is**, **how it works in Atrium**, **why it matters operationally**, and the **revenue/cost angle** — with vendor precedents noting the bar to clear.

### 3.1 Par-level tracking (minibar, linen, amenities, F&B consumables)
*Precedent: protel (minibar & laundry/towel tracking), Smart Software (BD) (inventory in POS).*

**What it is.** A target stock level — a par — held per item per location, so every consumable has a known "should have" against which "actually have" can be measured.

**How it works in Atrium.** Each category (minibar, linen, guest amenities, F&B consumables) carries its items with a par and a reorder point per location — a room-block minibar, a linen closet, an outlet's back-bar. Staff and managers see current-vs-par at a glance; the par is the reference every count, alert, and reorder list is measured against.

**Why it matters.** Without a par, "are we low?" is a guess, and the answer only arrives when something runs out mid-service. Pars turn stock into a managed number instead of a periodic surprise, and make counts fast because staff are checking against a target, not building a list from scratch.

**Revenue / cost angle.** Right-sized pars free cash tied up in overstocked linen and amenities (cost) and prevent the stockouts that force premium emergency buys (cost); they also underpin the minibar charge-recovery that protects revenue (see §3.4).

### 3.2 On-device stock counts
*Precedent: Quore, protel, Smart Software (BD).*

**What it is.** Counting stock on hand directly on the phone — a full or cycle count against par — without a paper worksheet transcribed later into a spreadsheet.

**How it works in Atrium.** A storekeeper or attendant opens the location, walks the shelf, and enters on-hand quantities against each item's par; the app computes the variance and the reorder gap. Counts work offline in storerooms and sync when back in coverage. Count history is retained so variance can be trended, not just seen once.

**Why it matters.** Paper counts are slow, get transcribed with errors, and are stale by the time they reach the office. On-device counting collapses count-to-record to a single step, keeps a history, and makes cycle counts frequent enough to catch drift early.

**Revenue / cost angle.** Frequent, accurate counts surface shrinkage and waste while they're still small (cost) and keep reorder decisions based on real numbers rather than padded guesses (cost).

### 3.3 Low-stock & reorder alerts
*Precedent: Smart Software (BD) (inventory in POS); general par-stock practice.*

**What it is.** An automatic flag when an item falls below its reorder point, surfaced as a reorder list to the storekeeper or manager — **an alert, not a purchase order.**

**How it works in Atrium.** As counts and consumption move stock below the reorder point, the item is flagged and rolls up into a low-stock/reorder view. The manager reviews the list and acts through whatever purchasing process the property already runs — F12 hands off the signal; it does not raise the PO (see §2.3). Where a store/procurement system exists, the reorder list can be exported to it (see §9).

**Why it matters.** Emergency purchasing happens because nobody saw the shortage coming. A reorder alert moves the decision earlier, when there's time to buy at normal terms instead of paying a rush premium or sending someone to a retail shop mid-service.

**Revenue / cost angle.** Fewer rush buys and stockouts cut emergency-purchase premiums and lost F&B sales (cost/revenue); earlier reordering smooths cash and reduces both overstock and dead stock (cost).

### 3.4 Consumption logged from the room / outlet
*Precedent: Quore (minibar consumption tracking), protel (minibar & laundry/towel tracking).*

**What it is.** Recording what was actually consumed at the point it happens — a minibar item taken and restocked, linen/towels issued to a floor — from the room or outlet rather than reconstructing it later.

**How it works in Atrium.** During a room clean, the attendant logs minibar items consumed as part of restock inside [F3 Housekeeping](staff-mobile-app-housekeeping-management-feature.md); the consumption both decrements stock and produces the record that drives the minibar charge. Linen/towel issues are logged against the location. For F&B, [F8 POS](staff-mobile-app-pos-multi-outlet-feature.md) depletion reflects consumables sold so back-bar stock stays honest.

**Why it matters.** The biggest quiet leak in minibar is the can that's consumed but never billed — because the consumption was noticed too late or not at all. Logging at the source ties consumption to the charge and to the count in one action, so what leaves the shelf is both replenished and recovered.

**Revenue / cost angle.** Capturing minibar consumption at restock recovers charges that would otherwise go unbilled (**revenue recovered**); source-logged consumption keeps counts accurate and shrinkage visible (cost).

---

## 4. Why This Matters

### 4.1 Uncounted consumables are quiet margin leaks
An unbilled minibar can, a miscounted linen case, and a premium emergency buy each cost real money without ever showing up as an incident. Par-stock makes the leak visible and countable.

### 4.2 Minibar charges recovered are pure margin protection
Minibar consumption that isn't captured at restock is revenue simply lost. Logging it at the source recovers the charge that should have followed the consumption — protecting F&B and minibar margin directly.

### 4.3 Emergency purchasing is the expensive failure mode
Running out mid-service forces rush buys at retail prices and lost sales when there's nothing to serve. Reorder alerts move the buy decision earlier, onto normal terms.

### 4.4 A competitive opening in the BD market
Few BD vendors offer unified, mobile par-stock across minibar, linen, amenities, and F&B tied into housekeeping and POS. Atrium can lead locally — while deliberately staying short of full procurement.

---

## 5. Operations It Improves

| Operation today | Pain | With Inventory & Par-Stock |
|-----------------|------|-----------------------------|
| **Minibar restock** | Consumption noticed late; charges missed | Logged at restock in Housekeeping; charge recovered, stock decremented. |
| **Linen / towel control** | Counted on paper, if at all | On-device counts against par; issues logged by location. |
| **Amenity stock** | Overstocked or surprise-empty | Pars right-size holdings; alerts before stockout. |
| **F&B consumables** | Back-bar drifts from the books | POS depletion keeps counts honest. |
| **Reordering** | Emergency rush buys at a premium | Reorder alerts move the buy earlier, onto normal terms. |

---

## 6. How It Protects & Recovers Revenue

1. **Recovered minibar charges.** Logging consumption at restock captures minibar revenue that would otherwise go unbilled — the largest single recovery in this feature.
2. **Fewer F&B stockouts.** Knowing back-bar and consumable levels prevents "sorry, we're out" moments that lose covers and add-on sales.
3. **Protected F&B and minibar margin.** Accurate counts and honest depletion keep cost of goods where it belongs instead of bleeding into unexplained variance.

> **Illustrative model:** If source-logged restock recovers even ৳15,000/month of previously unbilled minibar consumption, that is ~৳180,000/year in recovered charges — before counting shrinkage and waste reductions. *(Figures illustrative; validate against property data.)*

---

## 7. How It Reduces Operational Cost

1. **Less shrinkage and waste.** Frequent counts and source-logged consumption surface loss while it's small, not at the annual stocktake.
2. **Fewer emergency buys.** Reorder alerts replace rush purchases at premium prices with planned reordering on normal terms.
3. **Right-sized holdings.** Tuned pars free cash tied up in overstocked linen and amenities and cut dead stock.

> **Illustrative model:** If earlier reorder alerts cut emergency-purchase premiums by ৳10,000/month and reduce linen/amenity shrinkage by a further ৳8,000/month, that is ~৳216,000/year in avoided cost. *(Figures illustrative; validate against property data.)*

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Recover minibar revenue | Unbilled-minibar leakage | ↓ measurable |
| Cut shrinkage/waste | Count-to-book variance | ↓ 30% |
| Avoid rush buys | Emergency purchases / month | ↓ measurable |
| Prevent stockouts | Stockout events / month | ≤ target |
| Count discipline | Locations counted on schedule | ≥ 90% |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| Par-level setup | Establishing pars per item/location is a one-time effort | Prioritize by category; seed from history; start with minibar. |
| Count discipline | Value depends on staff counting consistently | Simple count UX; schedule + reminders; supervisor visibility. |
| POS integration | F&B depletion needs item-level POS data | Start manual F&B counts; add POS depletion when available. |
| Scope creep | Pull toward full procurement/purchasing | Hold the line at alerts + handoff (§2.3); PO system is future. |
| Existing store system | Property may run a store/procurement tool | Export reorder list; integrate rather than duplicate. |
| Offline storerooms | Stores lack coverage | Offline counting with auto-sync. |

---

## 10. Scope delivered

Atrium Staff ships as one complete package. This feature delivers:

- Par levels + on-device counts for **minibar and linen**
- Minibar consumption logged at restock (F3 touchpoint)
- Low-stock/reorder alerts
- Offline counting
- RBAC
- Guest amenities and F&B consumables
- POS (F8) depletion for back-bar stock
- Cycle-count scheduling and variance trends
- Reorder-list export to an existing store/procurement system
- Category analytics
- Groundwork for a future purchasing module

---

## 11. Open Questions

- Which categories go first — minibar, linen, amenities, or F&B — and in what order?
- Barcode/QR/NFC scanning for counting is provided by [F17 NFC & Barcode Scanning](staff-mobile-app-nfc-barcode-scanning-feature.md) — which categories adopt scan-first counting at launch?
- Does the property run an existing store/procurement system to integrate the reorder list with?
- Per-room minibar model vs. central-store model — which fits the target properties, and can we support both?
- What triggers the minibar charge — the restock log alone, or a confirmation step — and how does it reconcile with the PMS folio?

---

## 12. One-Line Business Case

> **Inventory & Par-Stock protects margin from the floor — turning uncounted minibar, linen, amenity, and F&B consumption into counted stock, recovered charges, and earlier reorders, without carrying the weight of a full procurement system.**
