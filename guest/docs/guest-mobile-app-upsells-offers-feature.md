# Upsells, Offers & Add-Ons — Feature Deep-Dive

**Product:** Atrium Guest (Hotel / Resort Guest-Facing Mobile App)
**Feature area:** G6 — Upsells, Offers & Add-Ons
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-16
**Status:** Draft v1.0
**Related:** [Guest Mobile App — Feature Scope](guest-mobile-app-feature-scope.md) · [Competitive Feature Research](../../HOTEL-~1.MD)

---

## 1. Executive Summary

**Upsells, Offers & Add-Ons** is the revenue engine of the guest app: a system that surfaces the right paid offer — a room upgrade, early check-in, late check-out, a breakfast package, an airport transfer, an anniversary setup, a sunset cruise — to the right guest at the exact moment their willingness to spend peaks, and books the acceptance straight onto the folio and into the staff-side operational calendars with **one tap**.

The economics are unusually asymmetric. A guest's spending intent peaks at booking, at pre-arrival, at arrival, and repeatedly in-stay — exactly the moments where a fixed desk sells worst (agents are busy, pitching feels awkward, timing is missed). Automated, personalized offers monetize those moments at **near-zero marginal labor**: the offer costs nothing to show, the acceptance flows to the folio without a conversation, and the fulfillment lands as a task or calendar entry in Atrium Staff. Every accepted offer is close to pure margin on infrastructure the property already owns — the upgrade room that would sit empty, the check-in hour that costs nothing, the transfer seat already driving to the airport.

The vendor field validates this harder than any other guest feature: Canary, Duve, Mews, RoomRaccoon, and Oracle's upgrade-offers product all ship it as a headline capability; Revinate powers occasion-based targeting from CRM data; Operto sells in-stay "smart button" upsells. Vendor-reported uplifts on ancillary revenue run from strong double digits **up to ~250%** (vendor-reported figures; validate in pilot). Combined with Atrium's [personalization data (G13)](guest-mobile-app-loyalty-guest-profile-feature.md), [multi-channel delivery (G7/G8)](guest-mobile-app-guest-messaging-feature.md), and [app-less reach (G15)](guest-mobile-app-app-less-access-feature.md), this is the feature on which the app pays for itself — **the #1 revenue feature of Atrium Guest**.

This document defines the feature's scope, quantifies its business case, and maps it to the journey moments and operations it improves.

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What the guest can do from the app |
|---|------------|-------------------------------------|
| 1 | **Room upgrades** | See and accept upgrade offers at booking, pre-arrival, and check-in — with photos, the price *difference*, and live availability. |
| 2 | **Visual upgrade preview** | Open any upgrade offer into the target room's full gallery and 360° tour, with a current-vs-upgrade side-by-side comparison — see exactly what +৳2,000 gets. |
| 3 | **Early check-in & late check-out purchase** | Buy arrival and departure flexibility at property-set prices, gated by live room and housekeeping status. |
| 4 | **Add-on packages** | Add breakfast, parking, airport transfer, spa credit, or a celebration setup (cake, flowers, décor) to the stay in one tap. |
| 5 | **Experiences & local bookings** | Book tours, boat trips, transport, and curated local experiences — sold by the property or its partners — from the same offer surface. |
| 6 | **Personalized & occasion-based targeting** | Receive offers matched to profile, party, stay history, and occasion (honeymoon, birthday, family with kids) from G13 data — not a spray of generic promos. |
| 7 | **Multi-channel delivery** | Meet offers where the guest is: in-app cards, push ([G8](guest-mobile-app-notifications-campaigns-feature.md)), email, and WhatsApp ([G7](guest-mobile-app-guest-messaging-feature.md)) — each linking to an app-less acceptance page (G15). |
| 8 | **One-tap acceptance → folio + staff calendars** | Accept and the charge posts to the folio ([G9](guest-mobile-app-payments-folio-tipping-feature.md)) while fulfillment lands in the right staff system automatically. |

### 2.2 In scope (this release)

Offer catalog authoring (property-defined offers with pricing, inventory, eligibility windows, photos, Bangla/English copy); upgrade offers driven by live room availability; visual upgrade previews — target-room galleries, video and 360° tours, and current-vs-upgrade comparison drawn from the shared property media library (see [G1](guest-mobile-app-booking-pre-arrival-feature.md)); early check-in / late check-out purchase gated by housekeeping status; add-on packages and celebration setups; experience and transport sales; targeting rules by journey stage, guest profile, party composition, and occasion; frequency capping and quiet hours; delivery via in-app surface, push, email, and WhatsApp with app-less acceptance links; one-tap acceptance with folio posting or direct payment (bKash/Nagad/SSLCommerz/card); automatic fulfillment routing into staff systems ([F1 Front Desk](../../staff/docs/staff-mobile-app-front-desk-feature.md) for upgrades and time flex, [F5 Task Management](../../staff/docs/staff-mobile-app-task-management-communication-feature.md) for setups, [F18 Events & Activities](../../staff/docs/staff-mobile-app-events-activities-feature.md) for experiences, [F8 POS](../../staff/docs/staff-mobile-app-pos-multi-outlet-feature.md) for F&B packages); offer performance analytics.

### 2.3 Out of scope (this release)

Algorithmic dynamic pricing / revenue-management integration (offers are property-priced; RMS-driven pricing is future); OTA-channel upsell delivery; third-party experience marketplaces with automated contracting (partner inventory is property-curated in v1); staff-side pitch scripting at the desk (the staff F1 doc covers agent-led upsell); loyalty *redemption* offers (that is [G13](guest-mobile-app-loyalty-guest-profile-feature.md) — though points can discount G6 offers).

### 2.4 Key assumptions

- Live room availability and housekeeping status are readable in real time (via the PMS/sync layer and staff F3), so upgrade and early check-in offers never oversell.
- The property authors and prices its offer catalog at onboarding; Atrium ships templates (upgrade ladder, time flex, breakfast, transfer, celebration sets).
- G13 profile data (preferences, party, occasion, history) exists at least minimally for targeting; untargeted stage-based offers work from day one without it.
- WhatsApp/email delivery uses the same channel infrastructure as G7/G8; consent and frequency rules are enforced centrally.
- Fulfillment routing depends on the staff app being live for the receiving department.

---

## 3. Capability Deep-Dive — The Eight Upsell Capabilities

The competitive research (Part A §5, *Upselling & Ancillary Revenue*) identifies this feature set directly: upgrades and add-on packages (Canary, Duve, Mews, RoomRaccoon, Oracle's upgrade-offers product), personalized/occasion-based upsells with vendor-reported uplifts up to ~250% (Canary, Duve, Revinate, Operto smart buttons), experiences and local bookings (Duve, RoomRaccoon), pre-arrival self-service ancillaries (Guestline, Apaleo eCommerce), and multi-channel delivery (Canary, Duve). Each capability is elaborated below: **what it is**, **how it works in Atrium**, **why it matters**, and the **revenue/cost angle**.

### 3.1 Room upgrades at booking, pre-arrival, and check-in
*Precedent: Canary, Duve, Mews, RoomRaccoon, Oracle upgrade offers.*

**What it is.** The upgrade sold as a *difference*, not a decision: "Sea-view room, +৳2,500/night" with photos, offered at each moment the room assignment is still fluid.

**How it works in Atrium.** Offers generate from live availability — only categories with sellable inventory on the stay dates are offered, at the property's price ladder. The offer appears at booking confirmation, again in the pre-arrival flow ([G1](guest-mobile-app-booking-pre-arrival-feature.md)), and again during mobile check-in ([G2](guest-mobile-app-check-in-check-out-feature.md)) — the highest-converting moment, when arrival excitement peaks. Acceptance re-assigns the room in the PMS, posts the difference to the folio, and notifies front desk F1. Declines are remembered; the guest is not re-pitched the same offer hourly.

**Why it matters.** Upgrade inventory is the classic perishable asset: the suite that sails empty tonight earns zero forever. Desk agents pitch upgrades inconsistently — queues, awkwardness, and clunky desktop flows suppress the ask. An automated offer asks *every* eligible guest, politely, with photos, at the right price and moment. The guest self-selects; nobody is sold to.

**Revenue / cost angle.** Selling a ৳2,500/night difference on a room that would otherwise be vacant is nearly 100% margin (revenue). Empty-suite nights convert to ADR lift with zero agent labor (cost). This is consistently the single largest line in vendor upsell case studies.

### 3.2 Visual upgrade preview — see what the difference buys
*Precedent: RoomRaccoon, Cloudbeds, Mews (booking-engine media); INTELITY (rich guest-experience media); Nonius and STAY (resort guest-app specialists).*

**What it is.** Every room-upgrade offer opens into the target room's full media — photo gallery, video, 360° virtual tour — plus a current-vs-upgrade side-by-side comparison: the room the guest has next to the room on offer, with photos, amenities, and the price difference in one view. "See what +৳2,000 gets you," shown rather than described.

**How it works in Atrium.** The upgrade card (§3.1) carries a "see the room" action that opens the target category's gallery and tour, and a compare view that renders the guest's booked room against the offered one — bed, view, size, balcony, bathtub — with the "+৳2,500/night" difference anchored to visible value. The media comes from the shared property media library that powers [G1 room previews](guest-mobile-app-booking-pre-arrival-feature.md), [G10 facility showcases](guest-mobile-app-digital-guidebook-property-info-feature.md), [G11 map previews](guest-mobile-app-interactive-property-map-feature.md), and [G12 activity previews](guest-mobile-app-activities-spa-experiences-feature.md) — no separate G6 content pipeline. App-less acceptance pages (G15) embed the same gallery, so a WhatsApp-delivered offer is just as visual as the in-app one.

**Why it matters.** A text offer asks the guest to imagine the sea view; a visual offer shows it — and visual offers convert materially better than text offers, which is why the media-rich vendors lead with photography in every upsell case study. The comparison view does the persuasion no agent can politely do at the desk: it makes the guest's current room look like the smaller choice, without a word being said.

**Revenue / cost angle.** Higher conversion on the same upgrade inventory — the same multiplier logic as targeting (§3.6), applied to the offer's presentation rather than its selection (revenue). Zero incremental content cost: the galleries and tours already exist in the shared media library authored for G1 (cost).

### 3.3 Early check-in & late check-out purchase
*Precedent: Canary, Duve, Mews, RoomRaccoon (headline add-ons in every vendor's list).*

**What it is.** Arrival and departure time flexibility sold as a product — the most universally wanted, most operationally free upsell in hospitality.

**How it works in Atrium.** Early check-in offers gate on live housekeeping status (staff F3): the offer only shows when a room in the guest's category can genuinely be ready. Late check-out gates on the next arrival's needs. Pricing is property-set (flat fee or tiered by hours); acceptance posts to the folio and updates the arrival/departure boards in F1 and the housekeeping sequence in F3 — the attendant's board re-prioritizes automatically. The offer is timed: early check-in pitches with the pre-arrival flow and the flight-time question; late check-out pitches the evening before departure.

**Why it matters.** Guests with a 6am arrival flight or a 9pm departure flight have an *acute, known, moneyed* need that most properties monetize only through ad-hoc desk negotiation — inconsistent, unpriced, and frequently given away free by an agent avoiding friction. Productizing it captures the value systematically, and gating on live operational data means it never sells what housekeeping can't deliver.

**Revenue / cost angle.** The marginal cost of an empty room being occupied three hours early approaches zero — the fee is margin (revenue). Systematic pricing ends the free-late-checkout leak (revenue protection). Pre-known time flex feeds the housekeeping plan, smoothing the turn schedule instead of disrupting it (cost).

### 3.4 Add-on packages — breakfast, parking, transfer, celebration setups
*Precedent: Canary, Duve, RoomRaccoon (breakfast, parking); Guestline self-service portal & Apaleo eCommerce (pre-arrival ancillaries).*

**What it is.** The property's package shelf: breakfast plans, parking, airport transfers, spa credits, romance/birthday/anniversary setups — attachable to the stay in one tap from booking through in-stay.

**How it works in Atrium.** Each package carries price, availability window, and a fulfillment route: breakfast attaches to the rate and informs the kitchen's counts (F8); transfers create a scheduled concierge task ([G4](guest-mobile-app-in-stay-service-requests-feature.md)-class, into F5); celebration setups generate a timed, checklisted room task in F5 with items drawn from inventory (F12). The pre-arrival window ([G1](guest-mobile-app-booking-pre-arrival-feature.md)) is the prime shelf-moment — the guest is planning, and delivery is guaranteed-ready.

**Why it matters.** Add-ons are bought when they're *visible*. Most properties sell packages only at booking (buried in rate plans) or on request (invisible). A persistent, journey-timed shelf sells the airport transfer when the guest is thinking about flights, and the anniversary setup when the occasion field says anniversary. Celebration setups in particular are high-emotion, high-margin, and almost never proactively offered today.

**Revenue / cost angle.** Attach-rate revenue across every stay (revenue); breakfast pre-attachment improves kitchen forecasting (cost); transfers fill vehicle capacity already on the road (revenue). Celebration setups carry some of the best margins on property and create the photographed, shared moments that market the property for free.

### 3.5 Experiences & local bookings — tours, transport
*Precedent: Duve, RoomRaccoon (tours, transportation, gift/picnic baskets, course meals).*

**What it is.** The property as the trusted storefront for the destination — tours, boat trips, day excursions, and transport, sold in-app with the property's margin attached.

**How it works in Atrium.** The property curates its experience catalog — own inventory (the resort's sunset cruise) and vetted partners (a Sundarbans tour operator, a Cox's Bazar parasailing vendor) — with schedules, capacity, and pricing. Bookings charge to folio or pay direct on local rails, and land in [staff F18 Events & Activities](../../staff/docs/staff-mobile-app-events-activities-feature.md) calendars (own inventory) or as partner-notification tasks (F5). The [property map (G11)](guest-mobile-app-interactive-property-map-feature.md) and [guidebook (G10)](guest-mobile-app-digital-guidebook-property-info-feature.md) cross-sell into the same catalog — the guidebook's "things to do nearby" is a shop, not just a list.

**Why it matters.** Guests spend heavily on experiences during resort stays — the only question is whether the property intermediates that spend or watches it walk out the door to street vendors and Google. In Bangladeshi resort destinations, where quality and safety signals are scarce, the property's curation is genuinely valuable to the guest — trust the guest will pay for.

**Revenue / cost angle.** Commission or markup on every partner booking, full margin on own inventory (revenue); demand data on what guests actually book informs what the property should build itself (revenue). Deflects "what should we do tomorrow?" concierge load into self-service (cost).

### 3.6 Personalized & occasion-based targeting
*Precedent: Canary, Duve, Revinate (CRM-driven), Operto smart buttons — vendor-reported uplifts up to ~250%.*

**What it is.** The difference between a promo blast and a well-judged suggestion: offers selected by who the guest is, who they're with, why they're traveling, and what stage of the journey they're in.

**How it works in Atrium.** Targeting rules read the [G13 profile](guest-mobile-app-loyalty-guest-profile-feature.md) — party composition (kids → kids' club and family cabana, not couples' spa), occasion (honeymoon flag → celebration setups, private dinner), history (booked the spa last stay → spa package first), stay type (business → late check-out and pressing, not tours) — and journey stage per the README matrix (booking → add-ons; pre-arrival → the full shelf; check-in → upgrades; in-stay → tonight's experiences; departure eve → late check-out). Frequency caps and quiet hours keep pressure low. Every impression, tap, and acceptance is logged, so offer performance tunes on real data.

**Why it matters.** Targeting is where the vendor-reported multiples come from — Canary, Duve, and Revinate attribute uplifts *up to ~250%* (vendor-reported; treat as directional) to personalized, well-timed offers versus generic ones. The same offer that annoys a business traveler delights an anniversary couple; relevance is the entire difference between an upsell engine and spam. Personalization is also self-reinforcing: every interaction enriches the profile that targets the next offer.

**Revenue / cost angle.** Multiplies conversion on the same offer inventory and the same guest flow — the highest-leverage line in this document (revenue). Prevents the offer fatigue that would otherwise erode G8 notification opt-in rates, protecting every other feature's channel (revenue protection).

### 3.7 Multi-channel delivery — push, email, WhatsApp
*Precedent: Canary, Duve (offers via SMS, WhatsApp, email).*

**What it is.** Offers delivered where the guest actually looks — in-app cards for installers, push at the timed moment, email pre-arrival, WhatsApp for the market where WhatsApp *is* the phone.

**How it works in Atrium.** Each targeted offer picks its channel by stage and guest preference, riding the shared channel infrastructure of [G7 Messaging](guest-mobile-app-guest-messaging-feature.md) and [G8 Notifications](guest-mobile-app-notifications-campaigns-feature.md). Every channel lands on the same **app-less acceptance page** (G15): offer, photo, price, one tap, pay or charge-to-folio — no install required. Consent, language (Bangla/English), and frequency caps are enforced centrally across channels, so three channels never means three pitches.

**Why it matters.** The best offer unseen is revenue unearned. Most guests won't install an app for a short stay — without channel reach, upsells touch only the installed minority. WhatsApp in particular is near-universal among Bangladeshi and South Asian travelers; an upgrade offer that arrives as a WhatsApp message with a one-tap link reaches essentially every guest with a phone. This is the G15 reach argument applied to the app's #1 revenue feature — and it multiplies everything in §3.1–3.6.

**Revenue / cost angle.** Channel reach converts the upsell engine from "app users only" to "every guest" — a straight multiplier on the addressable base (revenue). One centrally-governed pipeline avoids per-channel tooling and the brand damage of uncoordinated spam (cost, revenue protection).

### 3.8 One-tap acceptance → folio + staff calendars
*Precedent: Mews / RoomRaccoon (integrated PMS posting); Operto (instant-fulfillment smart buttons).*

**What it is.** The close: acceptance, payment, and operational fulfillment as a single atomic action — no form, no desk visit, no human relay.

**How it works in Atrium.** The guest taps accept; Atrium charges the folio (or takes direct payment on local rails), confirms in-app, and routes fulfillment: upgrades re-assign the room and notify F1; time flex updates the F1 boards and F3 housekeeping sequence; celebration setups create timed, checklisted F5 tasks; experiences book into F18 calendars; F&B packages register in F8. The guest sees the confirmed item on their stay itinerary and folio (G9) instantly. If fulfillment can't be guaranteed (room category just sold out), the offer withdraws before acceptance rather than failing after.

**Why it matters.** Conversion dies at every added step — an offer that ends in "visit the front desk to arrange" converts a fraction of one-tap. And an upsell *sold but not delivered* — the anniversary setup that never reached housekeeping — is worse than no sale: it's a paid-for disappointment. Atomic folio-plus-fulfillment is what makes high volume safe: the operational half of the promise is booked in the same instant as the revenue half.

**Revenue / cost angle.** One-tap maximizes conversion on every offer above (revenue); zero-touch fulfillment routing means upsell volume scales without adding coordination labor, and the sold-but-undelivered failure mode — refunds, comps, apology — is engineered out (cost).

---

## 4. Why This Matters

### 4.1 Willingness to spend peaks where the desk sells worst
The guest is most persuadable at booking confirmation (planning mode), pre-arrival (anticipation), check-in (excitement, room not yet fixed), and in-stay evenings (what shall we do tomorrow?). Fixed desks monetize almost none of these: agents are queue-bound at arrival and absent from every other moment. Automated offers occupy *all* of them, every day, for every guest, without a single additional staff-hour.

### 4.2 It sells inventory that otherwise expires worthless
Empty suites, unstaffed early-morning hours, unsold tour seats, tomorrow's spa slots — the property is full of perishable capacity that earns nothing by default. G6 is the mechanism that converts perishing capacity into folio revenue at near-total margin. Nothing new is built; what exists is finally sold.

### 4.3 It is the app's payback story
Every other feature earns indirectly — satisfaction, deflection, protection. G6 earns in taka, visibly, from week one, and its per-property numbers are the line item that justifies the entire Atrium Guest investment to an owner (see §6). The vendor field's aggressive uplift claims — up to ~250%, vendor-reported — exist because this is where guest apps demonstrably pay.

### 4.4 The Bangladesh market is unclaimed
No Bangladeshi vendor ships a real guest-facing upsell engine (research Part A, BD note); international platforms lack bKash/Nagad acceptance and WhatsApp-first delivery tuned to this market. Atrium can own the "your property earns more" pitch locally with a capability no direct competitor answers.

---

## 5. Journey Moments & Operations It Improves

| Journey moment / operation today | Pain | With Upsells, Offers & Add-Ons |
|----------------------------------|------|--------------------------------|
| **Suite empty tonight** | Nobody pitches it; it earns zero | Every eligible guest sees "+৳2,500 sea view" with photos at check-in; self-serve acceptance. |
| **Guest lands at 6am** | Waits in the lobby, or an agent improvises a fee — or waives it | Early check-in offered pre-arrival at a set price, gated by live housekeeping status. |
| **9pm departure flight** | Ad-hoc desk negotiation; late check-out often given free | Priced offer pushed the evening before; folio posted; F3 re-sequences automatically. |
| **Anniversary stay** | Occasion invisible; no one offers anything | Occasion-targeted celebration setup offered at pre-arrival; F5 task fulfills it on time. |
| **"What should we do tomorrow?"** | Concierge queue, brochure rack, or Google — spend leaves the property | Curated experiences bookable in-app; commission captured; F18 calendar fills. |
| **Desk agent at peak** | No time or appetite to pitch upgrades to a queue | The app pitched before the guest arrived; the agent just hands over a better key. |
| **Guest never installed the app** | Unreachable by every offer | WhatsApp/email offer with one-tap app-less acceptance (G15) — reach ≈ every guest. |
| **Sold upsell, missed fulfillment** | Paid anniversary setup never reached housekeeping — refund + apology | Acceptance atomically books the F5 task; sold-but-undelivered is engineered out. |
| **GM asks "what do upsells earn us?"** | Nobody knows; desk upsells are untracked | Per-offer impressions, conversion, and revenue in the analytics feed (staff F11). |
| **Airport transfer under-used** | Vehicle runs half-empty; guests take unvetted taxis | Transfer offered with the flight-time question; capacity fills at set prices. |

---

## 6. How It Increases Revenue

This is the app's #1 revenue feature; the model deserves detail.

1. **Upgrade revenue from perishing room inventory.** Every night with unsold higher-category rooms is upgrade inventory. Offered automatically to every eligible arrival with photos and one-tap acceptance, vendors consistently report conversion in the mid-single to low-double digits. The sold difference is nearly pure margin — the room, the cleaning, the utilities all existed anyway.
2. **Time-flex fees at near-zero cost.** Early check-in and late check-out monetize hours, not rooms. They are the highest-frequency upsell (relevant to nearly every arrival with a flight), the cheapest to fulfill, and today the most commonly given away free. Systematic pricing alone recovers a leak most properties don't measure.
3. **Add-on attach rate across every booking.** Breakfast, parking, transfers, spa credits, and celebration setups attach a margin-bearing item to a meaningful share of stays — and the pre-arrival shelf (Guestline/Apaleo pattern) sells when planning intent is highest.
4. **Experience commissions intermediate destination spend.** Tour and transport bookings capture margin on money the guest was going to spend anyway — currently with street vendors. Own-inventory experiences (sunset cruise, private dinner) carry full margin and fill F18 capacity.
5. **Targeting multiplies all of the above.** Personalized, occasion-based, journey-timed offers are where vendor-reported uplifts reach ~250% versus untargeted baselines (vendor-reported; treat as directional and validate in pilot). Same inventory, same guests — multiplied conversion.
6. **Channel reach multiplies the base.** WhatsApp/email delivery with app-less acceptance (G15) extends offers from app installers to effectively every guest — in the BD market, this alone can triple the addressable audience of every offer.
7. **Higher ADR and direct-booking gravity.** Guests who buy upgrades and experiences report higher satisfaction and rebook direct at higher rates ([G13](guest-mobile-app-loyalty-guest-profile-feature.md) loop) — compounding, commission-free future revenue.

> **Illustrative model:** A 150-room resort, 75% occupancy, ~110 arrivals/day, ৳9,000 ($75) ADR.
> - **Upgrades:** 8% of arrivals accept an average ৳2,000 ($17)/stay upgrade difference → ~৳17,600/day.
> - **Time flex:** 10% of arrivals buy early check-in or late check-out at ৳1,200 ($10) → ~৳13,200/day.
> - **Add-ons & celebrations:** 6% of stays attach an average ৳1,500 ($12.50) package → ~৳9,900/day.
> - **Experiences:** 5 bookings/day at ৳600 ($5) average margin → ~৳3,000/day.
>
> Total ≈ **৳43,700/day ≈ ৳16M (~$133,000)/year** of high-margin ancillary revenue — before targeting uplift, and at conversion rates well below the top of vendor-reported ranges. Against typical guest-app licensing, this line alone funds the entire app several times over. *(Figures illustrative; validate against property data and pilot conversion.)*

---

## 7. How It Reduces Operational Cost

1. **Selling without sellers.** The offer engine performs thousands of perfectly consistent pitches per month at zero marginal labor — work that would otherwise require trained, incentivized, always-present staff and still miss most moments.
2. **Zero-touch fulfillment coordination.** Acceptance auto-books the room change, the F5 setup task, the F3 re-sequence, the F18 slot — removing the relay chain (desk → housekeeping → F&B) that upsell fulfillment costs today, and the failure-apology-refund labor when the chain breaks.
3. **The free-upgrade and free-late-checkout leak closes.** Ad-hoc desk generosity — unpriced, untracked, inconsistent — becomes a priced product with an audit trail.
4. **Demand smoothing.** Pre-purchased early check-ins and late check-outs are *known before staffing is set* — housekeeping sequences around them instead of scrambling; kitchen counts include pre-attached breakfasts.
5. **Concierge deflection.** Self-service experience booking absorbs the "what should we do?" desk traffic; the concierge handles the bespoke, not the routine.
6. **One offer pipeline, not per-channel tooling.** Centralized authoring, targeting, consent, and analytics across in-app/push/email/WhatsApp replaces fragmented promo tools and the brand-damage cleanup of uncoordinated blasts.
7. **Marketing efficiency.** Offer analytics reveal what guests actually buy — replacing spray-and-pray promotion spend with evidence, and feeding owner-level reporting in staff F11 at no extra effort.

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Headline revenue | Incremental ancillary revenue per occupied room-night | ≥ ৳350 (~$3) by month 6 *(illustrative)* |
| Upgrade capture | Upgrade offer conversion (eligible arrivals) | ≥ 8% |
| Visual lift | Upgrade-offer conversion, with vs. without a gallery/tour view | ≥ 1.5× *(illustrative)* |
| Time-flex capture | Early check-in / late check-out attach rate | ≥ 10% of arrivals |
| Add-on attach | Stays with ≥1 purchased add-on | ≥ 6% |
| Targeting lift | Conversion, targeted vs. untargeted offers | ≥ 2× |
| Reach | Guests receiving ≥1 offer (any channel, incl. app-less) | ≥ 85% of stays |
| Offer hygiene | Offer opt-out / mute rate | < 5% |
| Fulfillment integrity | Accepted offers fulfilled without manual intervention | ≥ 98% |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| Live availability & housekeeping data | Upgrade and time-flex offers must never oversell reality | Real-time gating on PMS/F3 status; offers withdraw before acceptance when inventory moves |
| Offer catalog quality | Lazy pricing or poor photos suppress conversion | Onboarding templates, photo standards, and pilot-based price guidance |
| Over-pitching | Aggressive frequency erodes trust and G8 opt-ins across the whole app | Central frequency caps, quiet hours, decline-memory, opt-out honored everywhere |
| Fulfillment reliability | A sold-but-undelivered upsell is a paid disappointment | Atomic acceptance→task routing; F5 SLA escalation on fulfillment tasks |
| WhatsApp/email consent & compliance | Channel rules (opt-in, templates) vary and evolve | Consent captured at booking/registration; centrally managed channel policies (G7/G8) |
| Partner experience quality | A bad partner tour damages the property's brand | Curated partner list, guest ratings loop (G14), removal criteria |
| Payment & folio integrity | Direct-pay vs. folio postings must reconcile cleanly | Unified posting via G9 rails into PMS/F8 settlement reporting |
| Uplift expectations | Vendor-reported ~250% figures may anchor owners unrealistically | Present as vendor-reported; set pilot-based targets; measure honestly in F11 |

---

## 10. Scope delivered

Atrium Guest ships as one complete package. This feature delivers:

- Property-authored offer catalog with pricing, inventory, eligibility windows, photos, and bilingual copy
- Room upgrade offers at booking, pre-arrival, and check-in, driven by live availability
- Visual upgrade previews: target-room gallery, video and 360° tour, and current-vs-upgrade comparison from the shared property media library (see G1)
- Early check-in and late check-out purchase, gated by live housekeeping status
- Add-on packages: breakfast, parking, airport transfer, spa credit, celebration setups
- Experiences and local bookings (own and curated-partner inventory)
- Personalized and occasion-based targeting from the G13 profile, with journey-stage timing
- Frequency capping, quiet hours, decline-memory, and central opt-out
- Multi-channel delivery: in-app, push (G8), email, WhatsApp (G7) — each with app-less one-tap acceptance (G15)
- One-tap acceptance with charge-to-folio (G9) or direct pay on bKash / Nagad / SSLCommerz / cards
- Automatic fulfillment routing into staff F1, F3, F5, F8, and F18
- Per-offer impression, conversion, and revenue analytics (surfaced in staff F11)

---

## 11. Open Questions

- Who prices offers at launch — pure property control, or Atrium-recommended price ladders from pilot data?
- Upgrade offer timing at check-in: before room assignment (max flexibility) or after (guest sees exactly what they'd leave)?
- Are partner-experience commissions contracted per-property, or does Atrium offer a standard partner-settlement mechanism?
- WhatsApp Business API sender identity — per-property numbers or a shared Atrium sender with property branding?
- Do loyalty points (G13) discount G6 offers at v1, or is points-redemption kept strictly inside G13 initially?
- What is the minimum viable targeting rule set for launch properties with no historical guest data (cold start)?
- Should declined upgrade offers re-price automatically (e.g., cheaper day-of-arrival) — and does the property control that?

---

## 12. One-Line Business Case

> **Upsells, Offers & Add-Ons sells the property's perishing capacity — the empty suite, the idle morning hour, the unsold tour seat — to the right guest at the moment they most want it, at near-zero marginal labor: it is the feature that pays for the app, and then keeps paying.**
