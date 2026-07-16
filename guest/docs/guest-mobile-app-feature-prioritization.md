# Atrium Guest — Feature Prioritization (Revenue, Adoption & Cost-to-Serve)

**Product:** Atrium Guest (Hotel / Resort Guest-Facing Mobile App)
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-16
**Status:** Draft v1.0
**Start here:** [Project Overview & Documentation Index](../overview.md)
**Supporting detail:** [Executive Summary](guest-mobile-app-executive-summary.md) · [Navigation & Feature Map](guest-mobile-app-navigation-map.md) · [Journey-Stage Design](guest-mobile-app-journey-stage-design.md) · [Feature Scope](guest-mobile-app-feature-scope.md)

Atrium Guest ships as **one complete package** — every feature (G1–G15) is delivered. This document ranks the feature areas by **revenue impact**, **adoption / guest-experience impact**, and **cost-to-serve reduction** so their **relative weight within that package** is clear (which features carry the most impact), not to stage a rollout.

The guest app is the demand side of the platform: every guest action here lands in [Atrium Staff](../../staff/docs/staff-mobile-app-feature-prioritization.md) as a task, order, or reservation — so guest-side weight and staff-side weight are two views of the same operating loop.

---

## 1. Why three axes (weighting rationale)

The staff app was scored on *productivity* and *revenue*. The guest app needs a different pair of lenses plus one it shares:

- **Revenue impact** — does the feature directly earn money (upsells, F&B orders, direct bookings, activity fees) or protect it (deposit capture, dispute prevention, repeat direct bookings)? This is the axis ownership funds the app on.
- **Adoption / experience impact** — does the feature give the guest a reason to open (and keep opening) the app, and does it remove a known friction (the check-in queue, the bill surprise, the unanswered call)? Revenue features only fire if adoption features get the guest into the surface first. **Adoption is the multiplier on every other score.**
- **Cost-to-serve reduction** — does the feature deflect front-desk calls, order-taking labor, key-card handling, and form-filling? This is the guest-side mirror of staff productivity: work the guest now does for themselves is work the staff app no longer routes.

**Weighting rationale.** Revenue and adoption are weighted equally and above cost-to-serve. A guest app that saves labor but that guests don't open saves nothing; a guest app full of revenue features nobody adopts earns nothing. Cost-to-serve is real money — especially in Bangladesh, where front-desk labor absorbs the "what's the Wi-Fi password?" load — but it follows adoption rather than driving it. **G15 (app-less access) is scored as an enabler, not a feature**: it doesn't earn or save on its own, but it is the difference between the package reaching the ~30% of guests who install apps and effectively **every guest on property** — the same amplifying role staff F17 (scanning) plays inside the staff app.

Impact rated **High / Med / Low** for a typical property; resort-segment weightings flagged where they differ.

---

## 2. Scoring matrix

| # | Feature | Revenue impact | Adoption / experience | Cost-to-serve | Peak stage(s) | Priority |
|---|---------|:---:|:---:|:---:|---------------|----------|
| G6 | [Upsells, Offers & Add-Ons](guest-mobile-app-upsells-offers-feature.md) | **High** | Med | Low | Pre-arrival → In-stay | 🟢 Revenue engine |
| G5 | [Food & Beverage Ordering](guest-mobile-app-food-beverage-ordering-feature.md) | **High** | **High** | Med | In-stay | 🟢 Revenue engine |
| G2 | [Mobile Check-In & Check-Out](guest-mobile-app-check-in-check-out-feature.md) | Med *(upsell moment)* | **High** | **High** | Arrival · Departure | 🟢 Journey spine |
| G1 | [Booking & Pre-Arrival](guest-mobile-app-booking-pre-arrival-feature.md) | **High** *(OTA recovery)* | Med | **High** *(pre-arrival forms)* | Discover → Pre-arrival | 🟢 Journey spine |
| G9 | [Payments, Folio & Tipping](guest-mobile-app-payments-folio-tipping-feature.md) | Med–High *(capture + local rails)* | **High** *(no bill surprise)* | Med | In-stay · Departure | 🟢 Money layer |
| G8 | [Notifications & Campaigns](guest-mobile-app-notifications-campaigns-feature.md) | Med *(perishable inventory)* | **High** *(timeliness)* | Med | All stages | 🟢 Glue |
| G4 | [In-Stay Service Requests](guest-mobile-app-in-stay-service-requests-feature.md) | Low–Med | **High** | **High** | In-stay | 🟢 Self-service core |
| G7 | [Guest Messaging](guest-mobile-app-guest-messaging-feature.md) | Low–Med *(recovery)* | **High** | **High** *(deflection)* | Pre-arrival → Departure | 🟢 Self-service core |
| G3 | [Digital Room Key](guest-mobile-app-digital-room-key-feature.md) | Low | **High** *(completes contactless)* | Med *(cards, encoders)* | Arrival → In-stay | 🟡 Experience completer |
| G12 | [Activities, Spa & Experiences](guest-mobile-app-activities-spa-experiences-feature.md) | **High** *(resort)* | **High** *(resort)* | Med | Pre-arrival → In-stay | 🟡 Segment (resort) |
| G10 | [Digital Guidebook & Property Info](guest-mobile-app-digital-guidebook-property-info-feature.md) | Low | Med | **High** *(call deflection)* | In-stay | 🔵 Deflection |
| G11 | [Interactive Property Map](guest-mobile-app-interactive-property-map-feature.md) | Med *(discovery → folio)* | Med | Med | In-stay *(resort)* | 🔵 Discovery (resort) |
| G13 | [Loyalty & Guest Profile](guest-mobile-app-loyalty-guest-profile-feature.md) | Med–High *(repeat direct)* | Med | Low | Post-stay → next Discover | 🟠 Retention |
| G14 | [Feedback & Service Recovery](guest-mobile-app-feedback-service-recovery-feature.md) | Med *(review protection)* | Med | Low | In-stay · Departure | 🟠 Retention |
| G15 | [App-less Access (QR & Web)](guest-mobile-app-app-less-access-feature.md) | Med *(reach)* | **High** *(reach)* | **High** *(reach)* | All stages | 🟡 Enabler *(amplifies everything)* |

---

## 3. The ranking, explained

### The revenue engines — G6 and G5

| Feature | Revenue driver | Why it outranks everything |
|---------|----------------|----------------------------|
| **G6 Upsells & Offers** | Upgrades, early check-in, late check-out, packages, experiences — sold at the exact moments willingness-to-spend peaks (arrival, in-stay), at near-zero marginal labor | The one feature the app can literally pay for itself on; vendor precedent (Canary, Duve) reports uplifts to ~250% on personalized, moment-timed offers |
| **G5 F&B Ordering** | F&B is the biggest ancillary line; menu-in-hand ordering lifts order volume and average ticket; orders land straight in staff POS ([F8](../../staff/docs/staff-mobile-app-pos-multi-outlet-feature.md)) | Doubles as an adoption driver — ordering food is the single most *repeated* guest action, the habit loop that keeps the app open |

G6 scores lower on adoption than G5 because guests don't open the app *to be sold to* — offers ride on the surfaces other features earn. That is exactly why G6 is scored as a **global layer** (surfaced on Home, at check-in, in Explore) rather than a destination.

### The journey spine — G2, G1, G3

- **G2 Check-In & Check-Out** — the queue is the most-cited arrival friction and the folio review at departure is the classic dispute point; self-service removes both, cuts front-desk labor per arrival ([staff F1](../../staff/docs/staff-mobile-app-front-desk-feature.md) receives the check-in), and check-in is the highest-converting upsell moment G6 gets all stay. High adoption *and* high cost-to-serve — the reason most guests claim their stay at all.
- **G1 Booking & Pre-Arrival** — direct mobile booking recovers 15–25% OTA commission (the single largest recoverable revenue line for most properties), and pre-arrival registration moves the form-filling off the desk queue entirely; bookings land in [staff F2 Reservations](../../staff/docs/staff-mobile-app-reservations-management-feature.md). Its adoption score is Med only because the *booking* half competes with OTA habit — the *pre-arrival* half is near-universal once a magic link is in the confirmation email.
- **G3 Digital Room Key** — near-zero direct revenue, but without it mobile check-in still ends in a key queue, which un-earns G2's whole promise. Scored **High on experience as a completer**: it is the feature that makes the contactless journey true, plus real hardware savings (card stock, encoders, re-key trips). Its rank is capped by lock-hardware dependency — it is High only on properties with BLE-capable locks.

### The money layer — G9

**G9 Payments, Folio & Tipping** is the Bangladesh-first feature: **bKash / Nagad / SSLCommerz alongside cards** serves the payment instruments local guests actually hold — an international-vendor blind spot and a genuine local moat. The live folio prevents the bill surprise that generates disputes and comps (revenue *protection*), pay-by-link cuts no-shows via deposit capture, and QR tipping keeps a motivating income stream alive as cash disappears. It underwrites G2 express check-out, G5 direct payment, and G6 purchases — every revenue feature settles through it.

### The glue — G8

**G8 Notifications & Campaigns** is the guest-side analog of staff F6: a multiplier, not a destination. Every self-service feature depends on the guest knowing *when* to act — check-in open, room ready, order on its way, folio ready. Its own revenue line is filling perishable inventory (tonight's tables, tomorrow's spa slots) with stage-and-interest-targeted campaigns. Badly done, it is also the fastest way to get the app deleted — hence per-guest preferences and stage gating (see [Journey-Stage Design](guest-mobile-app-journey-stage-design.md)).

### The self-service core — G4 and G7

- **G4 Service Requests** — structured, tracked, auto-routed requests (landing in [staff F5 Task Management](../../staff/docs/staff-mobile-app-task-management-communication-feature.md) with SLA timers) replace the phoned request that gets lost between departments. High experience (guests see live status) and high cost-to-serve (call volume at the desk falls).
- **G7 Guest Messaging** — guests increasingly won't call but will message; one answered thread with auto-translation (Bangla ↔ English ↔ guest's language) and WhatsApp/SMS reach catches problems while they're fixable and deflects routine questions. Auto-answers push its cost-to-serve score to High.

### The resort segment — G12 and G11

- **G12 Activities, Spa & Experiences** — the guest-side counterpart of [staff F18](../../staff/docs/staff-mobile-app-events-activities-feature.md), and weighted exactly like it: **High revenue for resorts**, modest for city hotels. Resort ancillaries are high-margin, capacity-bound, and perishable; self-service booking fills slots phone-only booking leaves empty, and the personal itinerary it builds measurably lifts satisfaction and length-of-stay value.
- **G11 Interactive Property Map** — on a large resort, what guests can't find they don't spend on; the map converts wayfinding into discovery and discovery into folio revenue (tap a restaurant → G5 menu; tap the spa → G12 booking). City-hotel value is modest; resort value is real but derivative — it earns through G5/G12, which is why it scores Med rather than High.

### The deflection layer — G10

**G10 Digital Guidebook** is the purest cost-to-serve feature in the package: Wi-Fi one-touch, hours, house rules, transport and prayer times, local recommendations — searchable, **Bangla/English**, offline. Zero direct revenue, near-zero build risk, and it deflects the hundred small questions that consume front-desk hours daily. Also the cheapest adoption hook the app has: "connect to Wi-Fi" is many guests' first tap.

### The retention layer — G13 and G14

- **G13 Loyalty & Guest Profile** — repeat direct guests are the most profitable revenue a property has, and the profile is what makes personalization (G6 targeting, G8 campaigns, G5 dietary filters) possible everywhere else. Scored below the engines only because its payoff lands on the *next* booking, not this stay.
- **G14 Feedback & Service Recovery** — a problem caught in-stay is a save; the same problem on a review site is permanent public damage. Negative signals route instantly to the duty manager's recovery workflow ([staff F14](../../staff/docs/staff-mobile-app-guest-feedback-service-recovery-feature.md)); promoters get channeled to public reviews after checkout. Revenue impact is protective and reputational — Med, but with asymmetric downside protection.

### The cross-cutting enabler — G15

**G15 App-less Access** is the guest-side analog of staff F17: a delivery channel surfaced *through* every feature, never a screen of its own. Most guests — and in Bangladesh, most emphatically — **will not install an app for a two-night stay**. Check-in, menus, requests, chat, folio, payment, feedback, and tipping all ship as link-and-QR web flows (booking email, bedside card, table tent), with the native app as the upgrade path for resort stays and loyalty members. Its score *is* the reach multiplication: it converts every per-guest impact number above from "× app installers" to "× guests on property." See the market map — Canary and Duve built category-leading businesses on exactly this app-less-first posture ([competitive research, Part A](../../HOTEL-~1.MD)).

---

## 4. Priority weighting within the package

All features ship together; the tiers below express **relative impact**, not a build sequence.

- **Revenue engines (highest):** **G6 Upsells & Offers** and **G5 F&B Ordering** — the lines the app is funded on; G6 rides as a global layer, G5 is the habit loop.
- **Journey spine:** **G1 Booking & Pre-Arrival**, **G2 Check-In/Out**, **G3 Digital Key** — the contactless arc that gets every guest into the surface and defines the product's first impression.
- **Money layer:** **G9 Payments, Folio & Tipping** — local rails (bKash/Nagad/SSLCommerz), live folio, tipping; every other revenue feature settles through it.
- **Glue:** **G8 Notifications & Campaigns** — makes self-service timely and fills perishable inventory.
- **Self-service core:** **G4 Service Requests** and **G7 Messaging** — the biggest guest-satisfaction and call-deflection pair.
- **Resort upside:** **G12 Activities/Spa/Experiences** and **G11 Interactive Map** — highest ancillary value for the resort segment (mirrors staff F8/F18 weighting).
- **Deflection:** **G10 Guidebook & Property Info** — pure cost-to-serve, cheapest adoption hook.
- **Retention:** **G13 Loyalty & Profile**, **G14 Feedback & Recovery** — next-booking revenue and review-score protection.
- **Cross-cutting enabler:** **G15 App-less Access** — the reach multiplier on all of the above; every flow must have QR/web parity from day one.

The **Home tab** is a stage-adaptive stay card + launcher, not a dashboard — see [Journey-Stage Design](guest-mobile-app-journey-stage-design.md) and the [Navigation Map](guest-mobile-app-navigation-map.md).

---

## 5. Bottom line

- **The app is funded on G6 + G5** (offers and F&B ordering) — everything else earns its keep by getting guests to the surfaces where those two fire.
- **The app is adopted on G2 + G3 + G10** (check-in without a queue, phone as key, Wi-Fi in one tap) — the frictions guests already resent.
- **The app cuts cost on G4 + G7 + G10 + G1's pre-arrival half** — every self-served request, answered question, and pre-filled form is desk labor that never happens.
- **G9 is the Bangladesh moat** — local payment rails plus a transparent folio, which international vendors do not ship here.
- **G15 decides whether any of this reaches the market** — app-less parity is not an edge case, it is the primary channel for short-stay guests; the installable app is the resort/loyalty upgrade path.
- Every guest feature lands on a staff surface (G4→F5, G5→F8, G12→F18, G14→F14, G2→F1, G1→F2) — the two apps are one loop, and this ranking is the demand-side half of the staff ranking.

---

## 6. Caveats

- Ratings are for a **typical property**; a large resort raises G12 and G11 to 🟢 and a city business hotel lowers them. Re-score against the specific launch property.
- **G3's score assumes BLE-capable locks**; on properties without them, G3 drops to a roadmap item and G2 hands off to desk key pickup (still queue-shortened by mobile check-in).
- **G1's booking half** competes with entrenched OTA habit; its OTA-recovery revenue should be modeled conservatively and validated against the property's direct-booking baseline.
- Adoption assumptions (app-install rate vs. app-less reach) should be instrumented from launch — the G15 multiplier is the number every other projection hangs on.
- Revenue magnitudes should be validated against real property data (see the illustrative models in each deep-dive).
