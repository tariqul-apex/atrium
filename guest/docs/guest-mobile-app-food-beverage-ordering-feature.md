# Food & Beverage Ordering — Feature Deep-Dive

**Product:** Atrium Guest (Hotel / Resort Guest-Facing Mobile App)
**Feature area:** G5 — Food & Beverage Ordering
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-16
**Status:** Draft v1.0
**Related:** [Guest Mobile App — Feature Scope](guest-mobile-app-feature-scope.md) · [Competitive Feature Research](../../HOTEL-~1.MD)

---

## 1. Executive Summary

**Food & Beverage Ordering** puts every menu on the property into the guest's hand and lets them order from anywhere to anywhere — room service to the room, a burger to the pool cabana, coffee to a beach lounger, dinner to a restaurant table — charged to the folio or paid instantly on local rails. Orders land **directly in the staff Point of Sale ([F8](../../staff/docs/staff-mobile-app-pos-multi-outlet-feature.md))** at the right outlet, with location, items, modifiers, and payment state already structured: no phone call, no mishear, no re-key.

F&B is the largest ancillary-revenue line a hotel or resort has, and it is uniquely sensitive to friction: every barrier between appetite and order — finding the phone, waiting on hold, deciphering a printed menu in a second language, not knowing the poolside bar even delivers — is spend that simply doesn't happen. Menu-in-hand ordering with photos removes those barriers at the exact moments appetite peaks, and vendors across the market ship it as a flagship guest capability: INTELITY, Maestro, Shiji, Guestline Keez, Operto, and SuitePad globally; Mediasoft Pro-Inn locally in Bangladesh.

Two design choices multiply the reach. First, the **app-less QR path ([G15](guest-mobile-app-app-less-access-feature.md))**: a table tent or bedside card opens the same menu-and-order flow in a browser, which means the feature works for every guest on property from day one — and doubles as the ordering surface for **day visitors and walk-in locals**, an entirely additional revenue population. Second, **local payment rails** (bKash, Nagad, SSLCommerz) alongside charge-to-folio, serving the payment methods Bangladeshi guests and day visitors actually hold — a decisive local advantage global vendors don't offer.

This document defines the feature's scope, quantifies its business case, and maps it to the journey moments and operations it improves.

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What the guest can do from the app |
|---|------------|-------------------------------------|
| 1 | **Live multi-outlet menus** | Browse every outlet's current menu with photos, prices, and dietary filters (veg, halal-certified detail, allergens, spice level) — 86'd items disappear in real time. |
| 2 | **In-room dining orders** | Order room service with modifiers and notes; room number attached automatically from the stay. |
| 3 | **Order-to-location anywhere on property** | Order to a pool lounger, beach spot, cabana, or garden — location set by room number, scanned QR table tent, or a dropped map pin ([G11](guest-mobile-app-interactive-property-map-feature.md)). |
| 4 | **Charge-to-folio or direct pay** | One tap to the room bill ([G9](guest-mobile-app-payments-folio-tipping-feature.md)), or pay now via bKash / Nagad / SSLCommerz / card — day visitors always pay direct. |
| 5 | **Order status tracking** | Live states — received, preparing, on its way, delivered — with push updates ([G8](guest-mobile-app-notifications-campaigns-feature.md)). |
| 6 | **Scheduled orders** | Pre-order breakfast the night before with a delivery window; schedule any order for later (picnic at noon, cake at 8pm). |
| 7 | **App-less QR menu + order** | The full menu-and-order flow via table-tent / bedside QR in a browser — no install; the surface for day visitors and walk-in locals. |

### 2.2 In scope (this release)

Multi-outlet menu management surface (photos, descriptions, dietary tags, availability windows, real-time 86ing synced from staff F8); cart, modifiers, and special-instruction notes; in-room and to-location delivery with QR/map-pin location capture; charge-to-folio with stay verification; direct payment via local gateways and cards; order status tracking with push; scheduled/pre-orders including breakfast pre-order from pre-arrival; app-less QR ordering for in-house guests and day visitors; Bangla/English menus; order-level upsell prompts (add fries, pair a dessert) fed from G6.

### 2.3 Out of scope (this release)

Staff-side order fulfillment, kitchen display, and tableside POS workflows (owned by [staff F8](../../staff/docs/staff-mobile-app-pos-multi-outlet-feature.md)); restaurant *table reservations* (that is [G12](guest-mobile-app-activities-spa-experiences-feature.md)); off-property delivery / aggregator integration (future consideration); minibar auto-billing; recipe costing and F&B inventory (staff [F12 Inventory](../../staff/docs/staff-mobile-app-inventory-par-stock-feature.md)); loyalty-priced menus beyond simple perk credits (G13 integration matures post-v1).

### 2.4 Key assumptions

- Staff F8 POS is deployed and is the single source of truth for menus, prices, availability, and order routing — G5 is a guest-facing order-entry channel into it, not a second POS.
- Each outlet defines delivery zones and hours (the pool bar delivers to loungers; the specialty restaurant may be dine-in only).
- Charge-to-folio requires an in-house, checked-in guest with an open folio and property-set limits; everyone else pays direct.
- QR table tents / bedside cards are deployed with location identity encoded (table 12, room 304, cabana C5).
- Local payment gateway merchant accounts (bKash, Nagad, SSLCommerz) exist at the property.

---

## 3. Capability Deep-Dive — The Seven Ordering Capabilities

The competitive research (Part A §4, *In-Stay Services & In-Room Engagement*) identifies mobile F&B ordering — "order from any restaurant/bar on property for room or location delivery; orders route directly to staff" — as a core guest-app capability (INTELITY, Maestro, Shiji, Guestline Keez, Operto, SuitePad, Mediasoft Pro-Inn). Each capability is elaborated below: **what it is**, **how it works in Atrium**, **why it matters**, and the **revenue/cost angle**.

### 3.1 Live multi-outlet menus with photos & dietary filters
*Precedent: INTELITY, SuitePad (digital menus), Shiji.*

**What it is.** Every outlet's menu, digital and current — photographed dishes, honest prices, filterable by dietary need — replacing the laminated card and the leather folder.

**How it works in Atrium.** Menus are authored once in staff F8 and rendered to the guest per outlet with photos, Bangla/English descriptions, and tags (vegetarian, halal detail, contains-nuts, spice level). Availability windows (breakfast until 11) and real-time 86ing sync from the kitchen, so the guest can never order what isn't available. The property map (G11) deep-links each restaurant to its live menu.

**Why it matters.** Menus sell. Photographs demonstrably lift both conversion and item value versus text lists; dietary filters remove the hesitation of guests who would otherwise order nothing rather than risk asking; live accuracy removes the "sorry, that's finished" disappointment loop that kills a second attempt. For foreign guests, a bilingual visual menu dissolves the language barrier entirely.

**Revenue / cost angle.** Better presentation lifts average ticket (revenue); a single menu source ends reprint costs and the wrong-price disputes stale printed menus cause (cost). 86ing sync eliminates order-void-apologize labor cycles (cost).

### 3.2 In-room dining orders
*Precedent: INTELITY, Maestro, Guestline Keez, Mediasoft Pro-Inn (BD).*

**What it is.** Room service ordered from the phone — browse, customize, order, track — instead of dialing and dictating.

**How it works in Atrium.** A checked-in guest's room attaches automatically. Modifiers and free-text notes are structured per item; the order fires into F8 at the room-service outlet, prints/displays in the kitchen, and posts to the folio on delivery. Delivery confirmation prompts an optional tip ([G9](guest-mobile-app-payments-folio-tipping-feature.md)) and rating (G14).

**Why it matters.** Phone-based room service is the classic leaky funnel: busy lines, hold music, mishears across accents and languages, illegible order slips. Each leak is either lost revenue or a wrong tray — the most expensive kind of F&B failure, since it costs the food, the redelivery, and the goodwill. Menu-in-hand ordering with structured modifiers closes every leak at once.

**Revenue / cost angle.** Order volume rises when ordering is effortless — late-night, early-morning, and "don't want to talk to anyone" demand that phone lines suppress (revenue). Order-taking labor and mishear-driven remakes fall (cost).

### 3.3 Order-to-location anywhere on property
*Precedent: INTELITY, Operto, Shiji (location delivery); resort pattern per Nonius/Attractions.io map integration.*

**What it is.** The whole property becomes the restaurant's floor — orders delivered to a pool lounger, a beach spot, a cabana, a tennis court, a garden bench.

**How it works in Atrium.** Location is captured three ways: the guest's room number (default), a scanned location QR (cabana C5, pool bar table 7), or a pin dropped on the interactive property map (G11) for open areas like the beach. The order routes to the outlet that owns that zone in F8, and the runner sees the exact spot. Status updates tell the guest the drink is on its way — no waving at distant staff.

**Why it matters.** On a resort, the guest's moment of maximum appetite is rarely near a till: it's 2pm at the pool with no waiter in sight. Today that demand either evaporates or costs a walk (and the lounger). Order-to-location converts the entire grounds into revenue surface — this is *the* resort capability, and the reason resort-focused vendors lead with it.

**Revenue / cost angle.** Pure incremental revenue: orders that today never happen. One runner fulfilling pinned orders replaces multiple waiters patrolling acreage looking for raised hands (cost). Zone analytics (where do orders come from?) inform where to station staff and carts (cost/revenue).

### 3.4 Charge-to-folio or direct pay on local rails
*Precedent: charge-to-room per Shiji/Maestro; local gateways per Smart Software, Mediasoft (BD); pay-by-link per Cloudbeds, RoomRaccoon.*

**What it is.** Two payment paths: one tap onto the room bill, or immediate digital payment — bKash, Nagad, SSLCommerz, or card.

**How it works in Atrium.** In-house guests default to charge-to-folio (stay-verified, limit-checked, itemized live on [G9](guest-mobile-app-payments-folio-tipping-feature.md)); any guest can choose to pay now via local rails or card. Day visitors and walk-ins — who have no folio — always pay direct, which is exactly what makes the QR path safe to open to the public. Every payment posts to F8 and reconciles in the property's settlement reporting.

**Why it matters.** Payment friction is order friction. Charge-to-folio makes an in-stay order feel free at the moment of decision (a well-documented spend lifter), while bKash/Nagad serve the dominant payment behavior of the Bangladeshi market — including the local day-visitor who has a mobile wallet but perhaps no credit card. Global vendors ship neither rail; this is Atrium's home-field advantage.

**Revenue / cost angle.** Folio charging lifts in-stay spend per guest (revenue); local rails unlock the walk-in market (revenue); digital payment ends cash handling at the pool — float, shrinkage, and end-of-shift reconciliation (cost).

### 3.5 Order status tracking
*Precedent: order-status patterns per INTELITY, Operto; room-ready-style alerts per Shiji.*

**What it is.** Live order states — received, preparing, on its way, delivered — pushed to the guest.

**How it works in Atrium.** F8's order lifecycle drives guest-visible status via push (G8). The guest sees realistic timing expectations up front (kitchen-set prep estimates), progress as it happens, and a confirmation at delivery with one-tap tip and rating. If the kitchen flags a delay, the guest is told before they call.

**Why it matters.** The silent wait is where F&B satisfaction dies — the guest doesn't know if the order was even received, calls to check, and the desk interrupts the kitchen to ask. Visible status replaces all of it. Expectation-setting also *protects* satisfaction on genuinely busy nights: a known 40-minute wait rates better than an unexplained 30-minute one.

**Revenue / cost angle.** Eliminates where-is-my-order calls and the kitchen interruptions they cause (cost). Confidence in the process drives repeat orders within the same stay (revenue). Delivery-time analytics per outlet per hour feed staffing decisions (cost).

### 3.6 Scheduled orders (breakfast pre-order)
*Precedent: Guestline self-service portal pre-orders, Apaleo eCommerce (pre-arrival ancillaries).*

**What it is.** Orders placed now for later — tonight's guest pre-orders tomorrow's breakfast with a delivery window; a picnic basket is booked for noon; a celebration cake for 8pm.

**How it works in Atrium.** Any menu (where the outlet allows) can be ordered against a future window; breakfast pre-order is prompted the evening before via G8. Scheduled orders queue in F8 with production-planning visibility for the kitchen — tomorrow's breakfast count is known tonight. Pre-arrival guests can pre-order for arrival day from G1's flow (matrix stage ◐), and celebration setups cross-sell from [G6](guest-mobile-app-upsells-offers-feature.md).

**Why it matters.** Scheduled demand is the kitchen's favorite demand: it can be batched, prepped, and staffed for precisely. For the guest, the pre-ordered breakfast that arrives exactly at 7:30 before the tour bus is a small marvel of reliability — and the difference between eating on property and skipping (or eating off property).

**Revenue / cost angle.** Captures meals that time pressure otherwise sends off-property (revenue). Known production quantities cut buffet over-preparation and waste, and smooth the morning kitchen labor spike (cost).

### 3.7 App-less QR menu + order (table tents — the day-visitor surface)
*Precedent: QR web-ordering pattern per Operto, SuitePad-class in-room surfaces; app-less journey per Canary/Duve; G15 enabler.*

**What it is.** The complete menu-and-order flow served from a QR code in a browser — no download, no account — at every table, bedside, lounger, and cabana.

**How it works in Atrium.** Each QR encodes its location; scanning opens that outlet's live menu with the location pre-set. In-house guests can link their stay (folio charging); anyone else orders with direct payment on local rails. It is the same order pipeline into F8 — one system regardless of surface. This is G15 applied to F&B, and it is deliberately the **front door for day visitors and walk-in locals**: the persona the README scopes to G5/G9/G11/G12/G14, who will never install a hotel app but will scan a table tent.

**Why it matters.** Most guests will not install an app for a two-night stay — without the QR path, F&B ordering reaches a fraction of the property's appetite. With it, reach is effectively 100% of people on property, including the restaurant-only local market that, in Bangladesh's city hotels and resort destinations, can rival room-guest F&B covers. The app remains the richer upgrade path (history, loyalty, one-tap folio) rather than a prerequisite.

**Revenue / cost angle.** Multiplies the addressable base of every other capability in this document (revenue); a table-tent QR costs taka, not tablets (cost). Faster table-side ordering turns tables quicker at peak (revenue).

---

## 4. Why This Matters

### 4.1 F&B is the biggest ancillary line — and the most friction-sensitive
Rooms are booked once; food is bought many times a day, impulsively, perishably. Unlike room revenue, F&B revenue scales with *how easy ordering is at the moment of appetite*. Every removed step — find phone, wait, translate, walk — converts directly into orders that otherwise never exist. No other feature in the guest app has this many daily purchase moments to compound.

### 4.2 The property's whole footprint becomes sellable
A resort's acreage is an F&B asset only if orders can originate there. Order-to-location makes the pool, beach, and gardens revenue-productive without stationing waitstaff across all of them — the software does the patrolling.

### 4.3 One pipeline, two surfaces, zero re-keying
Because G5 is an entry channel into staff F8 — not a parallel system — every order arrives structured at the right outlet with payment state resolved. The kitchen and runners work exactly as they do for tableside staff orders; the property gains a channel, not a second workflow to reconcile.

### 4.4 Bangladesh-first is a genuine moat here
Mediasoft Pro-Inn advertises mobile ordering but with limited public detail (research Part A, BD note); no local vendor pairs a real guest ordering surface with bKash/Nagad/SSLCommerz payment *and* a staff-side multi-outlet POS. Atrium's combination — bilingual menus, local rails, QR reach to the walk-in market — is a package neither global nor local competitors currently assemble.

---

## 5. Journey Moments & Operations It Improves

| Journey moment / operation today | Pain | With F&B Ordering |
|----------------------------------|------|-------------------|
| **Guest hungry at the pool** | No waiter in sight; walk to the bar and lose the lounger — or skip it | Scan lounger QR or drop a map pin; order arrives at the spot; charged to folio. |
| **Late-night room service** | Dial, hold, dictate across a language gap; mishears; wrong tray | Visual bilingual menu; structured modifiers; order fires straight into the kitchen. |
| **Breakfast before a 8am tour** | À-la-carte gamble or skipped meal; kitchen slammed at 7:45 | Pre-ordered the night before with a window; kitchen preps against known counts. |
| **Foreign guest with dietary needs** | Anxious menu interrogation via the desk | Halal/veg/allergen filters; photos; order without a single conversation. |
| **"That item is finished"** | Printed menu sells what the kitchen ran out of hours ago | 86'd items vanish from the guest menu in real time. |
| **Where's my order?** | Guest calls desk; desk calls kitchen; kitchen interrupted | Live status with realistic timing; delay notices pushed proactively. |
| **Walk-in locals at the restaurant** | Wait for a menu, wait to order, wait for the bill; staff stretched at peak | Table-tent QR: menu, order, and bKash payment self-served; staff deliver and host. |
| **Cash at the pool bar** | Float, shrinkage, soggy notes, end-of-shift reconciliation | Digital payment on local rails; every taka posted to F8 automatically. |
| **Order-taking labor at peak** | Waiters transcribing orders instead of serving | Guests self-enter; staff time shifts to delivery, hosting, and upselling. |
| **F&B manager planning staffing** | Gut feel about where and when demand happens | Zone, outlet, and hour-level order analytics from a single pipeline. |

---

## 6. How It Increases Revenue

1. **Order volume from removed friction.** Orders that phone lines, language gaps, and lounger-abandonment suppress simply start happening. Vendors in this space consistently report double-digit percentage lifts in in-room dining and pool-service volume when ordering moves to the guest's device; even the conservative end compounds daily.
2. **Higher average ticket.** Photos, descriptions, and automated prompts ("add fries", "pair a dessert wine") upsell every order consistently — the digital menu never forgets to ask, never feels pushy, and sells identically at 2pm and 2am.
3. **The property's grounds become revenue surface.** Order-to-location monetizes the pool, beach, and gardens — locations that today produce almost nothing between waiter passes.
4. **The day-visitor market via QR.** Table-tent ordering with bKash/Nagad opens the walk-in local F&B market on the same pipeline — an additional revenue population with zero room-inventory cost.
5. **Charge-to-folio lifts in-stay spend.** Deferred payment measurably increases purchase frequency versus pay-per-order; the bill consolidates on G9 where it settles once.
6. **Scheduled demand captured.** Pre-ordered breakfasts and picnics are meals won back from the street and the skipped-meal; celebration items cross-sold from G6 carry premium margins.
7. **Faster tables at peak.** QR self-ordering shortens the order-to-kitchen cycle, turning tables faster on full nights — more covers from the same room.

> **Illustrative model:** A 150-room resort at 75% occupancy (~112 occupied rooms, ~250 in-house guests) plus day visitors. If mobile ordering adds just **0.4 incremental orders per occupied room per day at a ৳900 (~$7.50) average ticket**, that is ~৳40,000/day ≈ **৳14.7M (~$122,000)/year** of incremental F&B revenue — before counting average-ticket uplift on existing orders (say +8% on ৳25M of existing mobile-addressable F&B ≈ ৳2M/year) and the walk-in QR market. *(Figures illustrative; validate against property data.)*

---

## 7. How It Reduces Operational Cost

1. **Order-taking labor collapses.** Guests self-enter orders that staff today transcribe by phone or at tableside — at scale, this is the largest single labor line in F&B service, redirected to delivery and hospitality.
2. **Mishears and remakes disappear.** A structured order with explicit modifiers cannot be misheard across a phone line or a language gap — ending the remade-tray cycle that costs food, labor, and goodwill.
3. **No re-keying between channels.** Orders arrive in F8 born-digital at the right outlet; nobody retypes a phone order into the POS, and channel-reconciliation errors vanish.
4. **Menu operations go to zero-marginal-cost.** Price changes, new items, seasonal menus, and 86ing publish instantly everywhere — no reprints, no stale laminated cards, no wrong-price refunds.
5. **Cash handling shrinks.** Digital payment at pools, beaches, and gardens removes float management, shrinkage exposure, and reconciliation labor from the most cash-leaky zones on property.
6. **Kitchen production planning improves.** Scheduled orders and demand analytics per hour/zone/outlet cut over-preparation waste and smooth the morning labor spike.
7. **Fewer status interruptions.** Live tracking ends the guest-calls-desk-calls-kitchen chain that fragments kitchen focus at the busiest moments.

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Adoption | Share of F&B orders arriving via app/QR | ≥ 35% by month 6 |
| Revenue volume | Incremental orders per occupied room per day | ≥ +0.3 |
| Ticket size | Average ticket, digital vs. phone/tableside orders | +8–12% |
| New market | QR orders from non-resident (day visitor) payers | ↑ measurable, tracked separately |
| Grounds monetization | Orders originating from pool/beach/garden zones | ↑ vs. near-zero baseline |
| Accuracy | Order remake / correction rate on digital orders | ↓ 60% vs. phone baseline |
| Labor | Order-taking minutes per 100 covers | ↓ 30% |
| Guest experience | F&B review sub-score; post-delivery in-app rating | ↑ vs. baseline |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| F8 POS deployment | G5 has no fulfillment path without the staff POS and kitchen routing | F8 ships in the same package; G5 activates per outlet as F8 goes live |
| Menu content quality | Poor photos / stale data kill conversion and trust | Onboarding includes menu photography & data setup; single-source authoring in F8 |
| Delivery logistics | Order-to-location fails without runners and zone ownership | Per-zone delivery windows and outlet assignment; property can scope zones at launch |
| Wi-Fi / data coverage at pool & beach | Weak coverage breaks ordering exactly where it earns most | Lightweight web flow tolerant of slow links; property Wi-Fi survey at onboarding |
| Payment gateway integration | bKash/Nagad/SSLCommerz merchant setup and settlement reconciliation | Standard gateway modules; unified settlement reporting with F8 |
| Charge-to-folio abuse | Fraudulent room charges via QR | Stay verification, per-folio limits, confirmation for large orders |
| Kitchen overload spikes | Effortless ordering can spike demand beyond capacity | Kitchen-set prep-time throttles and per-outlet order pacing in F8 |
| Two-surface parity | App vs. QR-web feature drift confuses guests and staff | One codebase for the ordering flow; QR path is the same flow, differently entered |

---

## 10. Scope delivered

Atrium Guest ships as one complete package. This feature delivers:

- Live multi-outlet menus with photos, bilingual descriptions, dietary filters, and real-time availability
- In-room dining ordering with modifiers and notes
- Order-to-location across the property (room number, location QR, map pin via G11)
- Charge-to-folio with stay verification and limits
- Direct payment on bKash / Nagad / SSLCommerz and cards
- Live order status with push updates, delay notices, and delivery confirmation
- Post-delivery one-tap tipping (G9) and rating (G14) hooks
- Scheduled orders and breakfast pre-order (from pre-arrival onward)
- App-less QR menu + ordering at tables, bedsides, loungers, and cabanas (G15)
- Direct order injection into staff F8 POS at the correct outlet — one pipeline for all surfaces
- Order-level upsell prompts fed from G6

---

## 11. Open Questions

- Which outlets and delivery zones are in scope for the pilot property, and who owns zone-to-outlet mapping?
- Charge-to-folio limits: property-set flat cap, per-night cap, or deposit-linked?
- Do day-visitor QR orders require a phone-number OTP (for order notifications and abuse control) or stay fully anonymous?
- Service-charge and VAT presentation on digital menus for the BD market — inclusive pricing or itemized at cart?
- Is tipping prompted per order, at delivery confirmation, or left to the G9 journey-level flow?
- Aggregator/off-property delivery (foodpanda-class) — explicitly out of scope for v1, but does the pilot property want a roadmap position?

---

## 12. One-Line Business Case

> **F&B Ordering puts the menu in every pocket on property — turning appetite anywhere into an order in the POS, lifting the biggest ancillary-revenue line while cutting the order-taking, mishears, and cash handling that erode its margin.**
