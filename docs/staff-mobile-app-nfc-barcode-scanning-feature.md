# NFC & Barcode Scanning — Feature Deep-Dive

**Product:** Atrium Staff (Hotel / Resort Operations Mobile App)
**Feature area:** F17 — NFC & Barcode Scanning
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-15
**Status:** Draft v1.0
**Related:** [Staff Mobile App — Feature Scope](staff-mobile-app-feature-scope.md) · [POS — Deep-Dive](staff-mobile-app-pos-multi-outlet-feature.md) · [Inventory & Par-Stock — Deep-Dive](staff-mobile-app-inventory-par-stock-feature.md) · [Preventive Maintenance & Assets — Deep-Dive](staff-mobile-app-preventive-maintenance-assets-feature.md) · [Competitive Feature Research](../HOTEL-~1.MD)

---

## 1. Executive Summary

**NFC & Barcode Scanning** turns the phone's camera and NFC radio into a **fast-capture** layer that runs *inside* the features staff already use — most of all **quick POS** ([F8](staff-mobile-app-pos-multi-outlet-feature.md)) and **Inventory & Par-Stock** ([F12](staff-mobile-app-inventory-par-stock-feature.md)). Instead of scrolling a menu or hunting a row in a stock list, a staff member points at a barcode, QR, or NFC tag and the app instantly resolves *what it is* and offers *what to do*: add this product to the order, count this item against par, open this asset's PM schedule, confirm presence in this room.

Scanning is not a new destination in the app — it is a **capability surfaced on existing screens**, the way Notifications (F6) and Staff SOS (F16) are cross-cutting layers. It attacks the two slowest, most error-prone manual steps in floor operations: **typing/finding an item** and **proving a physical thing was handled**. A scan is faster than a search, is unambiguous, and leaves a timestamped record.

Scanning is table stakes in retail-grade POS and inventory tooling — Oracle Simphony, Toast, Lightspeed, Square (barcode add-to-order and retail lookup), and MarketMan / Craftable / Yellowdog (barcode stock counts) — while hotel ops vendors (Knowcross, Quore, Optii, Flexkeeping) use QR/NFC tags for asset identification and **proof-of-presence** on rounds and cleans. Atrium unifies both patterns into one scan capability shared across POS, inventory, assets, housekeeping, and front desk.

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What staff can do from the device |
|---|------------|-----------------------------------|
| 1 | **Multi-symbology scan** | Read 1D barcodes (EAN/UPC/Code 128), 2D codes (QR/DataMatrix), and NFC tags (NDEF) from one scan control. |
| 2 | **Scan-to-order (quick POS)** | Scan a packaged product (mini-mart, gift shop, back-bar bottle) to add it to the open [F8](staff-mobile-app-pos-multi-outlet-feature.md) order; scan a table/room QR to open the right tab. |
| 3 | **Scan-to-count (inventory)** | Scan an item to jump to its row and enter/adjust the count against par in [F12](staff-mobile-app-inventory-par-stock-feature.md); continuous "batch" scan for fast cycle counts and receiving. |
| 4 | **Scan-to-identify (assets & rooms)** | Scan an NFC/QR asset tag to open its record + PM schedule ([F10](staff-mobile-app-preventive-maintenance-assets-feature.md)); scan a room tag to open the room ([F3](staff-mobile-app-housekeeping-management-feature.md)) or work order ([F4](staff-mobile-app-maintenance-work-orders-feature.md)). |
| 5 | **Proof-of-presence** | An NFC/QR tap at a location or asset stamps *who* and *when* — verifying rounds, PM visits, and room cleans actually happened on site. |
| 6 | **Tag & label provisioning** | Generate and print QR/barcode labels and write NFC tags for rooms, assets, storage bins, and lost-&-found items. |
| 7 | **Offline scanning** | Resolve scans against a cached item/asset catalog and queue the resulting actions; sync when back in coverage. |
| 8 | **Manual fallback** | When a code is missing, damaged, or unreadable, fall back to search/keypad entry — scanning never blocks the task. |

### 2.2 In scope (this release)
Camera-based barcode/QR scanning and NFC tag read from a shared scan control; scan-to-order in POS (F8); scan-to-count and batch counting in Inventory (F12); scan-to-identify for assets/PM (F10) and rooms/work orders (F3/F4); proof-of-presence stamping; QR/barcode **label generation & printing** and **NFC tag writing** for rooms, assets, bins, and L&F items; a product/asset **code registry** (mapping a scanned code to a catalog item); offline scan resolution against a cached catalog; manual-entry fallback; support for the phone camera and optional paired **Bluetooth ring/sled scanners** for high-volume counts; RBAC.

### 2.3 Out of scope (this release)
Building the catalogs themselves — the **product menu/price list** lives in POS (F8), **par items** in Inventory (F12), and the **asset register** in F10; F17 *reads and links to* them, it does not own them. Also out: dedicated rugged scanner hardware beyond consumer phones + optional Bluetooth scanners; RFID gate/portal reading and bulk RFID inventory (long-range/UHF); computer-vision object recognition without a code; guest-facing scanning (a guest app is out of scope product-wide). Digital-key NFC provisioning is handled by the lock integration in F1, not here.

### 2.4 Key assumptions
- Target devices expose a usable camera; NFC read is available on most Android devices and modern iPhones (feature degrades gracefully to camera-only where NFC is absent).
- Packaged retail/back-bar goods carry manufacturer barcodes; house items and locations can be labeled with printed QR/NFC during setup (a one-time effort, see §9).
- The owning features (F8 menu, F12 pars, F10 assets) can expose an item ID that a scanned code maps to.

---

## 3. Capability Deep-Dive — The Scanning Features

Each capability is elaborated — **what it is**, **how it works in Atrium**, **why it matters operationally**, and the **revenue/cost angle** — with vendor precedents noting the bar to clear.

### 3.1 Multi-symbology scan (barcode · QR · NFC)
*Precedent: Oracle Simphony / Toast / Lightspeed (retail barcode), Knowcross / Quore (QR/NFC asset & room tags).*

**What it is.** One scan control that reads the three code families floor staff actually meet: linear barcodes on packaged goods, QR/DataMatrix on printed labels, and NFC tags tapped on rooms and assets.

**How it works in Atrium.** The staff member opens the scan control (a camera sheet, or a tap of the phone to an NFC tag) from wherever they are — an open order, a stock count, an asset screen. The app decodes the symbol, looks it up in the code registry, and routes to the matching action. The *same* control is reused everywhere, so staff learn scanning once.

**Why it matters.** A single, consistent scan gesture removes the cognitive load of "which mode am I in?" and works whether the property labels with cheap QR stickers, NFC tags, or relies on existing manufacturer barcodes.

**Revenue / cost angle.** Reusing one control across POS, inventory, and assets keeps training near-zero (cost) and makes every downstream capability faster (cost/revenue) — the leverage is that one component amplifies many features.

### 3.2 Scan-to-order — quick POS
*Precedent: Square / Lightspeed / Toast retail add-to-order; hotel mini-mart & gift-shop POS.*

**What it is.** Adding an item to an open F&B/retail order by scanning its barcode instead of finding it in a menu grid — the fast path for **quick POS**: mini-marts, grab-and-go, gift shops, and packaged back-bar stock.

**How it works in Atrium.** With an order open in [F8 POS](staff-mobile-app-pos-multi-outlet-feature.md), the staff member scans a product; the code registry resolves it to the menu/retail item and its price, and it drops into the order with a quantity stepper. Scanning a table or room QR opens or attaches the correct tab so the charge lands on the right folio.

**Why it matters.** For a 30-item grab-and-go fridge or a gift shop, tapping through categories is slow and error-prone; scanning is one motion per item and eliminates "wrong item, wrong price" rings. It makes a phone a credible quick-service till.

**Revenue / cost angle.** Faster rings mean more covers/transactions served per hour and shorter queues (**revenue** throughput); correct price lookups cut under-ringing and mischarges that leak margin or trigger comps (cost/revenue); it also makes previously-untracked mini-mart/gift retail practical to sell and capture (**revenue**).

### 3.3 Scan-to-count — inventory & receiving
*Precedent: MarketMan / Craftable / Yellowdog (barcode stock counts & receiving).*

**What it is.** Counting stock by scanning each item — jumping straight to its row against par — with a **continuous batch mode** for walking a shelf, and scan-on-receipt for goods coming in.

**How it works in Atrium.** In an [F12](staff-mobile-app-inventory-par-stock-feature.md) count, the storekeeper scans an item and enters on-hand (or increments via repeated scans in batch mode); the app computes variance and the reorder gap. Receiving scans confirm what physically arrived against what was expected. Counts and scans work offline in storerooms and sync later.

**Why it matters.** Finding the right row in a long linen or F&B list is the slow part of a count; scanning removes the search entirely and prevents counting the wrong item. Batch mode turns a shelf-walk into a rhythm rather than a lookup loop.

**Revenue / cost angle.** Faster, more accurate counts cut count labor and let cycle counts run often enough to catch shrinkage early (cost); scan-verified receiving stops paying for goods that never arrived (cost).

### 3.4 Scan-to-identify — assets, PM & rooms
*Precedent: Knowcross / Quore / Transcendent (QR/NFC asset tags → asset record & PM).*

**What it is.** Tapping or scanning a tag on a physical thing — a chiller, a lift, a guest room — to open exactly its record without searching an ID.

**How it works in Atrium.** An NFC/QR tag on an asset opens its [F10](staff-mobile-app-preventive-maintenance-assets-feature.md) register entry, PM schedule, and "log reading"; a room tag opens the [F3](staff-mobile-app-housekeeping-management-feature.md) room or its [F4](staff-mobile-app-maintenance-work-orders-feature.md) work order. The technician standing in front of the equipment reaches its record in one tap.

**Why it matters.** Manually finding "AC-Chiller #2" in a list while standing in a plant room is slow and mistake-prone; the wrong record means the wrong PM logged. A tag guarantees the record matches the thing in front of the person.

**Revenue / cost angle.** Right-record-first-time cuts mislogged PM and rework (cost) and speeds every asset/room interaction (cost); faster fault identification returns OOO rooms to sale sooner (**revenue** protection).

### 3.5 Proof-of-presence
*Precedent: Optii / Flexkeeping / Knowcross (NFC/QR checkpoint verification on rounds & cleans).*

**What it is.** A scan/tap at a fixed tag that records *who* was *where* and *when* — turning "the round was done" into evidence.

**How it works in Atrium.** Security rounds, fire/safety inspections ([F13](staff-mobile-app-inspections-compliance-feature.md)), PM visits, and room cleans can require a tag tap at the location; the stamp attaches to the task/inspection record. Missing taps surface as gaps a supervisor can see.

**Why it matters.** Skipped rounds and "signed but not walked" checklists are a real liability and quality risk. A physical tap that can only happen on site makes completion honest and auditable.

**Revenue / cost angle.** Verifiable rounds reduce liability and disputed claims (cost) and lift the QA/inspection discipline that protects guest experience and brand standing (**revenue** protection).

### 3.6 Tag & label provisioning + code registry
*Precedent: standard barcode/QR label printing; NFC tag writing (NDEF).*

**What it is.** The setup side: generating and printing QR/barcode labels, writing NFC tags, and maintaining the **registry** that maps a scanned code to the right catalog item, asset, room, bin, or L&F record.

**How it works in Atrium.** A manager prints location/bin/asset labels and writes room/asset NFC tags from the app; manufacturer barcodes on packaged goods are linked to menu/par items by scanning once and confirming the match. The registry is the single source that every scan resolves against, online or cached offline.

**Why it matters.** Scanning is only as good as the mapping behind it; a clean registry is what makes an unknown code resolve to the right thing. Self-service labeling keeps rollout cheap and lets the property grow coverage over time.

**Revenue / cost angle.** Cheap QR/NFC labeling avoids specialized hardware (cost); a maintained registry is what unlocks every scan-driven saving above (cost/revenue).

---

## 4. Why This Matters

### 4.1 Typing and searching are the slow, error-prone steps
The two slowest moments on the floor are finding an item in a list and identifying a physical thing. Scanning collapses both to one motion and removes the wrong-item error class entirely.

### 4.2 Quick POS only works if rings are instant
A phone becomes a credible grab-and-go / mini-mart / gift-shop till only when items ring in one motion at the right price. Scanning is what makes previously-untracked retail practical to sell and capture.

### 4.3 Counts and receiving are labor you can shrink
Stock counts and goods-receipt are recurring manual labor whose cost is mostly *lookup*. Scan-to-count removes the lookup and makes accurate counts frequent enough to catch loss early.

### 4.4 Presence you can prove is liability you can defend
Rounds, PM, and inspections carry liability when completion can't be verified. A location tap turns completion into auditable evidence — and cheap QR/NFC tags make it affordable for a BD property.

---

## 5. Operations It Improves

| Operation today | Pain | With NFC & Barcode Scanning |
|-----------------|------|------------------------------|
| **Quick POS / mini-mart ring** | Tap through menus; wrong item/price | Scan product → right item + price into the order in one motion. |
| **Stock count** | Hunt the row in a long list | Scan item → jump to its row; batch-scan a whole shelf. |
| **Goods receiving** | Manual tick-off against a sheet | Scan-on-receipt confirms what arrived vs. expected. |
| **Asset / PM lookup** | Search an ID in the plant room | Tap the asset tag → its record + PM schedule opens. |
| **Room / work-order open** | Type the room number | Scan the room tag → room or work order opens. |
| **Rounds & inspections** | "Signed" without proof of presence | Tag tap stamps who/when at the location. |
| **Lost & found** | Loose paper labels | Printed barcode label ties the item to its record. |

---

## 6. How It Protects & Increases Revenue

1. **Faster POS throughput.** One-motion rings shorten queues and serve more covers/transactions per hour at grab-and-go, poolside, and events.
2. **Captured retail sales.** Scanning makes mini-mart and gift-shop retail practical to run and bill, opening an ancillary line that manual entry made too slow to bother with.
3. **Fewer mischarges and comps.** Correct scanned prices cut under-ringing and wrong-item disputes that leak margin.
4. **Faster fault-to-fix.** Tag-first asset identification speeds diagnosis, returning OOO rooms to sale sooner (protects room revenue).

> **Illustrative model:** If scanned quick-POS lets a poolside/mini-mart outlet serve even 10% more transactions at peak and capture ৳20,000/month of previously-untracked retail, that is ~৳240,000/year of ancillary revenue — before mischarge reductions. *(Figures illustrative; validate against property data.)*

---

## 7. How It Reduces Operational Cost

1. **Less count/receiving labor.** Removing the lookup step cuts the time per stock count and goods-receipt, and makes cycle counts frequent enough to catch shrinkage early.
2. **Fewer capture errors.** Scanned items and prices remove the wrong-item/wrong-price error class in POS and inventory, cutting rework and reconciliation.
3. **Right record, first time.** Tag-based asset/room identification prevents mislogged PM and misrouted work orders.
4. **Verified presence, fewer disputes.** Proof-of-presence reduces skipped-round liability and time spent adjudicating "was it done?".

> **Illustrative model:** If scan-driven counts save two stores staff ~30 minutes each per count over ~40 counts/month, and scan-verified receiving avoids ৳6,000/month in phantom-receipt loss, that is a meaningful recurring labor + shrinkage saving. *(Figures illustrative; validate against property data.)*

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Speed up POS rings | Avg. seconds per item added (scan vs. tap) | ↓ measurable |
| Speed up counts | Avg. time per stock count | ↓ 30% |
| Cut capture errors | Wrong-item / price-mismatch rate | ↓ measurable |
| Capture retail | Mini-mart / gift-shop transactions logged | ↑ measurable |
| Verify presence | Rounds/PM with a valid location tap | ≥ 95% |
| Scan reliability | First-scan resolve rate (code → item) | ≥ 98% |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| Catalog ownership | Scans resolve against F8 menu / F12 pars / F10 assets | F17 links only; owning features maintain the catalogs. |
| Tag/label rollout | Labeling rooms, assets, and bins is a one-time effort | Self-service label printing / NFC writing; prioritize by area; use existing barcodes where they exist. |
| Device NFC variance | Some devices lack NFC; camera quality varies | Degrade to camera/QR + manual fallback; support Bluetooth scanners for high-volume counts. |
| Damaged / missing codes | A code can be unreadable | Always offer search/keypad fallback; reprint labels on demand. |
| Lighting / motion | Storerooms and plant rooms are dim | Torch toggle, guided framing, offline resolution. |
| Registry drift | Codes remapped or items retired | Confirm-match on first scan; flag unresolved codes for cleanup. |
| Scope creep to RFID | Pull toward UHF/RFID portals | Hold at barcode/QR/NFC-tap in v1; RFID is future. |

---

## 10. Scope delivered

Atrium Staff ships as one complete package. This feature delivers:

- Multi-symbology scan control (barcode · QR · NFC) reused across features
- Scan-to-order for quick POS (F8), including table/room QR to open a tab
- Scan-to-count and continuous batch counting for Inventory (F12)
- Scan-on-receipt for goods receiving
- Scan-to-identify for assets/PM (F10) and rooms/work orders (F3/F4)
- Proof-of-presence stamping for rounds, PM, and inspections (F13)
- QR/barcode label printing and NFC tag writing for rooms, assets, bins, and L&F
- Product/asset code registry with confirm-on-first-scan matching
- Offline scan resolution against a cached catalog with queued actions
- Manual search/keypad fallback everywhere scanning appears
- Optional paired Bluetooth ring/sled scanner support

---

## 11. Open Questions

- Which outlets go first for quick-POS scanning — mini-mart, gift shop, poolside, or events?
- Tagging medium per surface: printed QR stickers, NFC tags, or both — and who owns label printing at onboarding?
- Is proof-of-presence mandatory for security rounds and PM in v1, or opt-in per property?
- Do any target properties already use manufacturer barcodes we can adopt, or must all items be house-labeled?
- Are paired Bluetooth scanners needed at launch for large-store counts, or is the phone camera sufficient?
- How do scanned mini-mart/gift-shop retail lines reconcile to the PMS folio and night audit?

---

## 12. One-Line Business Case

> **NFC & Barcode Scanning makes the phone a one-motion capture tool — scan to ring, scan to count, tap to identify, tap to prove presence — speeding quick POS and inventory while cutting the wrong-item errors and lookup labor that quietly drain margin, all by reusing one scan control across the whole app.**
