# Booking & Pre-Arrival — Feature Deep-Dive

**Product:** Atrium Guest (Hotel / Resort Guest-Facing Mobile App)
**Feature area:** G1 — Booking & Pre-Arrival
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-16
**Status:** Draft v1.0
**Related:** [Guest Mobile App — Feature Scope](guest-mobile-app-feature-scope.md) · [Competitive Feature Research](../../HOTEL-~1.MD)

---

## 1. Executive Summary

**Booking & Pre-Arrival** is where the guest relationship begins — and where the property either owns that relationship or rents it from an online travel agency at 15–25% commission. The feature combines a **mobile booking engine** (search, book, pay a deposit, direct and commission-free) with a **pre-arrival flow** that gets the registration card, identity documents, arrival time, preferences, and pre-ordered add-ons completed before the guest ever reaches the property.

For the property this is a double win: **direct booking recovers OTA commission** on every reservation the app captures, and **pre-arrival registration removes the form-filling that creates queues** — the guest arrives already known, and the property arrives already prepared. Vendors have proven both halves: RoomRaccoon, Mews, and Cloudbeds ship high-converting mobile booking engines; Oracle OPERA Cloud, Mews, Maestro, Guestline GuestStay, protel, and WebRezPro ship pre-arrival digital registration that auto-updates the PMS so front desk and housekeeping can plan ahead.

Every booking and every completed registration lands directly in the staff side — the reservation in [Atrium Staff F2 — Reservations Management](../../staff/docs/staff-mobile-app-reservations-management-feature.md), the arrival details on the [F1 — Mobile Front Desk](../../staff/docs/staff-mobile-app-front-desk-feature.md) next-arriving-guest board. One platform, two surfaces.

This document defines the feature's scope, quantifies its business case, and maps it to the journey moments and operations it improves.

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What the guest can do from their phone |
|---|------------|----------------------------------------|
| 1 | **Mobile booking engine** | Search availability by date/room type, see rates and photos, and book direct — commission-free for the property, best-rate for the guest. |
| 2 | **Room preview & comparison** | Explore every room type through photo galleries, video and 360° virtual tours, and per-room-type amenity lists — and compare rooms side-by-side to see what +৳2,000 more actually gets. |
| 3 | **Deposit payment** | Pay a booking deposit in-app on cards or local rails (bKash, Nagad, SSLCommerz) and receive instant confirmation. |
| 4 | **Pre-arrival digital registration card** | Complete the registration card from the phone before arrival — name, contact, nationality, party details — auto-syncing to the PMS. |
| 5 | **Document upload** | Photograph and upload identity documents (NID, passport, visa) ahead of arrival, pre-verified before the guest reaches the desk. |
| 6 | **Dynamic check-in forms** | Answer property-configurable arrival questions — purpose of visit, vehicle plate, special declarations — collected per property policy and local regulation. |
| 7 | **Arrival-time & preference capture** | State expected arrival time, transport needs, and stay preferences (high floor, twin beds, feather-free, crib) so the room is staged right the first time. |
| 8 | **Pre-order add-ons** | Add breakfast, airport transfer, early check-in, celebration setups, or a pre-stocked minibar before arrival — charged to the booking or folio. |
| 9 | **Multi-language (Bangla / English)** | Complete the entire booking and registration journey in Bangla or English, switchable at any step. |

### 2.2 In scope (this release)

Availability search and rate display, room-type photo galleries with video and 360° virtual tours, per-room-type amenity lists, side-by-side room comparison, direct booking with confirmation, deposit capture on cards and local gateways, digital registration card with PMS sync, document photo upload, property-configurable dynamic forms, arrival-time and preference capture, pre-arrival add-on ordering, Bangla/English UI, app-less web/QR path for every flow (per G15), and booking-modification/cancellation self-service within policy.

### 2.3 Out of scope (this release)

Channel management and OTA distribution (the booking engine is the *direct* channel; OTA feeds remain in the PMS/channel manager), dynamic pricing and revenue management (rates come from the PMS), group-block and event contracting (staff-side, F2), travel-agent and corporate portals, and check-in itself — identity verification at arrival, e-signature, and room assignment live in [G2 — Mobile Check-In & Check-Out](guest-mobile-app-check-in-check-out-feature.md).

### 2.4 Key assumptions

- The PMS exposes real-time availability/rate reads and reservation writes, or a middleware sync layer bridges it.
- A payment gateway path exists for deposits — SSLCommerz/bKash/Nagad for the BD market plus an international card gateway.
- Uploaded identity documents can be stored and processed in compliance with local data-protection rules and police-reporting requirements for foreign guests.
- Guests without the app can reach every flow via the booking-confirmation email link and QR codes (G15) — the app is the upgrade path, not the gate.

---

## 3. Capability Deep-Dive — The Nine Booking & Pre-Arrival Features

The competitive research (Part A §1, *Booking & Pre-Arrival*) identifies the capabilities that define a best-in-class booking and pre-arrival experience. Each is elaborated below: **what it is**, **how it works in Atrium**, **why it matters**, and the **revenue/cost angle**. Vendor precedents are noted so we know the bar to clear.

### 3.1 Mobile booking engine — direct and commission-free
*Precedent: RoomRaccoon, Mews, Cloudbeds.*

**What it is.** A high-converting, mobile-first booking flow: search dates, compare room types with photos and rates, book, and pay — inside the guest's own app or an app-less web link, with no OTA in the middle.

**How it works in Atrium.** The engine reads live availability and rates from the PMS, presents room types with imagery, amenities, and Bangla/English descriptions, and completes the booking in a handful of taps. Confirmation is instant — in-app, push, and email — and the reservation appears immediately in [Atrium Staff F2 — Reservations Management](../../staff/docs/staff-mobile-app-reservations-management-feature.md). Loyalty members (G13) see member rates and earn points on direct bookings, giving guests a durable reason to book here instead of an OTA.

**Why it matters.** Every direct booking is a guest relationship the property owns — the email, the phone number, the preference history, the rebooking channel. OTA bookings are anonymous, commissioned, and rented. The booking engine is also the acquisition funnel for the whole app: the guest who books here is already onboarded for check-in (G2), the key (G3), and everything in-stay.

**Revenue / cost angle.** Direct bookings avoid 15–25% OTA commission — on a ৳10,000 room-night, ৳1,500–2,500 recovered per night, every night. Member rates and loyalty shift the channel mix stay after stay (revenue); no commission reconciliation or OTA extranet labor on these bookings (cost).

### 3.2 Room preview & comparison — galleries, tours, and side-by-side
*Precedent: RoomRaccoon, Cloudbeds, Mews (booking-engine media); INTELITY (rich guest-experience media); Nonius and STAY (resort guest-app specialists).*

**What it is.** Rich visual exploration of every room type before the guest commits: photo galleries, video walk-throughs, 360° virtual tours, per-room-type amenity lists, and a side-by-side comparison view that answers the question every guest silently asks — *what does +৳2,000 more actually get me?*

**How it works in Atrium.** Each room type in the booking flow opens into a full media page — property-managed photo galleries, video, and 360° tours with Bangla/English captions — plus a structured amenity list (size, bed configuration, view, balcony, bathtub). A compare view places two or three room types side-by-side: photos, amenities, and the rate *difference*, so the trade-up decision is visual and concrete rather than a leap of faith on a number. The assets live in one shared property media library: the same galleries and tours power [G6 upgrade offers](guest-mobile-app-upsells-offers-feature.md), [G10 facility showcases](guest-mobile-app-digital-guidebook-property-info-feature.md), [G11 map previews](guest-mobile-app-interactive-property-map-feature.md), and [G12 activity previews](guest-mobile-app-activities-spa-experiences-feature.md) — authored once, surfaced everywhere.

**Why it matters.** Guests book what they can see. On an OTA the guest gets deep photo sets and reviews; a direct booking flow with three thumbnails loses on confidence, not price. Rich media closes that gap — the property's own channel finally shows the product as well as the channel that charges 15–25% for the privilege. And the comparison view reframes the premium room from a bigger number into a visible, desirable difference.

**Revenue / cost angle.** Better-shown rooms convert more lookers into direct bookers, and side-by-side comparison shifts selection up the room-type ladder — both pure-margin gains on traffic already in the flow (revenue). One media library serving G1, G6, G10, G11, and G12 means the photography and tour investment is made once and monetized on five surfaces (cost).

### 3.3 Deposit payment on local rails
*Precedent: Cloudbeds and RoomRaccoon (pay-by-link); Smart Software and Mediasoft (bKash/Nagad/SSLCommerz — BD vendors).*

**What it is.** Taking the booking deposit at the moment of booking — in-app or by payment link — on the payment methods the guest actually holds.

**How it works in Atrium.** At confirmation the guest pays the property-configured deposit via bKash, Nagad, SSLCommerz, or an international card. The payment posts to the reservation folio automatically; the guest sees the paid deposit reflected in their live folio (G9) from day one. Unpaid deposits trigger automated payment-link reminders (G8) before the hold expires.

**Why it matters.** A deposit-secured booking is a booking that shows up. In the BD market, requiring an international credit card at booking excludes most of the domestic audience — local mobile-money rails are the difference between a deposit policy that works and one that silently kills conversions.

**Revenue / cost angle.** Deposits slash no-show and late-cancellation losses on peak nights (revenue protected); automated capture and posting removes manual bank-transfer verification and phone-chasing (cost). Local rails are a decisive edge global vendors largely don't serve.

### 3.4 Pre-arrival digital registration card
*Precedent: Oracle OPERA Cloud, Mews, Maestro, Guestline GuestStay, protel, WebRezPro.*

**What it is.** The registration card — the form every hotel legally must complete — filled in by the guest from their own phone before arrival, typically prompted ~48 hours out, with the PMS auto-updated so front desk and housekeeping can plan ahead.

**How it works in Atrium.** At the property-configured pre-arrival window, the guest gets a push/email prompt (G8): "Complete your registration before you arrive." The form pre-fills from the booking and the loyalty profile (G13); the guest confirms or corrects details for each party member and submits. The completed card syncs to the PMS and surfaces on the staff [F1 next-arriving-guest board](../../staff/docs/staff-mobile-app-front-desk-feature.md) — the agent sees a green "registration complete" flag before the guest walks in.

**Why it matters.** The registration card is the single biggest time-sink at the desk: spelling names aloud, copying passport numbers, filling carbon forms. Moving it to the guest's sofa the day before turns a five-minute desk transaction into a greeting. It is also the gateway to online check-in (G2) — a guest who registered pre-arrival is one tap from checked-in.

**Revenue / cost angle.** Shorter arrival transactions raise desk throughput at peak with the same headcount (cost); registration completion is the top of the mobile check-in funnel, which drives the upsell-at-arrival moment (revenue).

### 3.5 Document upload
*Precedent: IDS Next FX GeM (QR upload + signature), Hotelogix, Canary, eZee (ID capture).*

**What it is.** The guest photographs their NID, passport, or visa in-app before arrival; the document attaches to the reservation for staff verification.

**How it works in Atrium.** Guided capture (frame overlay, blur detection) makes the photo usable on the first try. Documents encrypt at rest, attach to the registration record, and are visible to authorized front-desk staff for verification at arrival — a glance-and-match rather than a scan-and-type. For foreign guests, the data pre-populates the police "Form C"-style reporting the property must file. Retention follows a property-configurable policy.

**Why it matters.** Document handling is the second big desk delay after the registration card — and in BD it is a legal requirement, not a nicety. Pre-uploaded documents compress arrival to identity confirmation, and give the property its compliance paperwork before the guest is even on site.

**Revenue / cost angle.** Cuts minutes per foreign-guest arrival and removes photocopier/scanner steps (cost); pre-verified documents unlock the fully remote check-in path in G2 that makes queue-free arrival — a rate-power differentiator — possible (revenue).

### 3.6 Dynamic check-in forms
*Precedent: Mews.*

**What it is.** Property-configurable arrival questions and data-collection rules — different properties, jurisdictions, and stay types need different fields, and the form should adapt rather than being one hard-coded card.

**How it works in Atrium.** The property defines its form in staff-side configuration: required vs. optional fields, per-nationality requirements (visa details for foreign passports only), per-stay-type questions (vehicle plate for self-drive guests, event name for banquet guests), and declarations. The guest sees only what applies to them, in Bangla or English. Form versions are auditable for compliance.

**Why it matters.** Regulation and property policy vary — a Cox's Bazar resort, a Dhaka business hotel, and a Sylhet boutique property do not collect the same data. Hard-coded forms mean either missing fields (compliance risk) or irrelevant ones (guest friction). Configurability makes one product fit all three.

**Revenue / cost angle.** Compliance data collected right the first time avoids re-contact, fines, and audit scramble (cost); shorter, relevance-filtered forms complete more often, feeding the check-in and upsell funnels (revenue).

### 3.7 Arrival-time & preference capture
*Precedent: Oracle OPERA Cloud, Mews, Guestline GuestStay (pre-arrival planning data); Infor HMS (next-arriving-guest preferences on the staff side).*

**What it is.** Asking the guest, ahead of time, the two things the property most wants to know: **when are you arriving**, and **what do you want the room to be**.

**How it works in Atrium.** During pre-arrival the guest states an expected arrival window, flags transport needs (airport pickup — itself an upsell), and sets preferences: floor, bed configuration, pillow type, crib, dietary notes, accessibility needs. Preferences save to the guest profile (G13) so returning guests never repeat themselves. On the staff side, arrival times sequence the F1 arrivals board and let housekeeping prioritize which rooms to turn first; preferences drive room assignment before the guest arrives.

**Why it matters.** This is the data that turns arrival from reactive to anticipatory. A property that knows a family with a crib arrives at 14:00 stages that exact room by 13:30 — instead of discovering the need at the desk and making the family wait. Late-arrival flags also prevent erroneous no-show releases.

**Revenue / cost angle.** Arrival-time data enables early check-in *sold as an add-on* where inventory allows, and smooths housekeeping's turn sequence so fewer rush-turns and comps happen (cost); preference-matched rooms lift satisfaction and review scores, which support rate (revenue).

### 3.8 Pre-order add-ons
*Precedent: Guestline self-service portal, Apaleo eCommerce (pre-arrival ancillaries); Duve, RoomRaccoon (experiences, gift/picnic baskets, meals).*

**What it is.** Selling ancillaries — breakfast, transfers, early check-in, flowers-and-cake celebration setups, spa slots — in the pre-arrival window, when anticipation is high and planning is active.

**How it works in Atrium.** The pre-arrival screen surfaces a curated add-on shelf targeted by stay type and party (per G6's targeting rules): a couple on an anniversary stay sees the celebration setup; a family sees the kids'-club pass and connecting-room offer. Purchases charge to the booking or folio and land in the right staff module — transfers as F5 tasks, dining and spa in the F18 activity calendars, F&B pre-orders toward F8 POS.

**Why it matters.** Pre-arrival is a high-intent, zero-labor sales window: the guest is planning the trip anyway, and no staff member has to make the pitch. It is also operationally kinder — a transfer ordered two days out is a scheduled job; one requested at the kerb is a scramble.

**Revenue / cost angle.** Pre-arrival upsell platforms report attach rates that meaningfully exceed desk selling because the offer meets the guest at planning time (revenue); pre-orders convert last-minute scrambles into scheduled, batched work (cost).

### 3.9 Multi-language guest journey (Bangla / English)
*Precedent: Oracle OPERA Cloud (nine languages); Smart Software, Mediasoft and BD vendors (Bangla/English bilingual UI).*

**What it is.** The entire booking and pre-arrival journey — screens, forms, emails, confirmations — available in the guest's own language.

**How it works in Atrium.** Every G1 surface ships fully localized in Bangla and English, switchable per guest and remembered on the profile. Property-authored content (room descriptions, add-on copy, form questions) supports both languages with an authoring fallback. The architecture is n-language so additional languages (Arabic, Hindi, Chinese for inbound tourism) can follow without rework.

**Why it matters.** A Bangla-speaking domestic guest asked to complete an English-only legal registration form either abandons it or fills it wrong — both outcomes land back on the desk. For the BD-first strategy, Bangla is not a translation task; it is the difference between the domestic mass market using pre-arrival self-service or ignoring it.

**Revenue / cost angle.** Language-matched flows convert more bookings and complete more registrations, feeding every downstream revenue moment (revenue); correctly completed forms remove the desk-side re-entry and correction labor (cost). Almost no BD vendor ships a guest app at all, let alone a bilingual one — this is open ground.

---

## 4. Why This Matters

### 4.1 Direct booking is the highest-margin revenue a property has
Every reservation that moves from an OTA to the app keeps the commission — typically 15–25% of room revenue — inside the property, and hands the property the guest relationship (contact, preferences, rebooking channel) the OTA otherwise owns. The booking engine plus loyalty (G13) is the only durable mechanism to shift that channel mix.

### 4.2 Pre-arrival is where arrival friction is actually solved
The queue at the desk is not created at the desk — it is created by all the data collection deferred to the desk. Registration, documents, preferences, and payment done pre-arrival mean [G2 check-in](guest-mobile-app-check-in-check-out-feature.md) can be a tap, and the staff-side [F1 Mobile Front Desk](../../staff/docs/staff-mobile-app-front-desk-feature.md) greets a *known* guest instead of interviewing a stranger.

### 4.3 It is the acquisition funnel for the entire guest app
G1 is most guests' first touch with Atrium Guest. A guest who books or registers here is onboarded for the key (G3), F&B ordering (G5), upsells (G6), and feedback (G14). The app-less path (G15) — booking-email links and QR codes — means even the guest who never installs the app still completes pre-arrival.

### 4.4 It is a competitive opening in the BD market
Global vendors ship booking engines and pre-arrival registration; almost no Bangladeshi vendor ships a genuine guest-facing app, and none combines it with Bangla-first UX and bKash/Nagad/SSLCommerz deposits. Atrium can lead locally on exactly this feature.

---

## 5. Journey Moments & Operations It Improves

| Moment / operation today | Pain | With Booking & Pre-Arrival |
|--------------------------|------|----------------------------|
| **Guest discovers the property** | Books on an OTA; property pays commission and learns nothing about the guest | Books direct in-app or via web link; property keeps the margin and the relationship. |
| **Deposit collection** | Bank-transfer instructions, screenshots on WhatsApp, manual verification | In-app bKash/Nagad/SSLCommerz/card deposit, auto-posted to the folio. |
| **48 hours before arrival** | Property knows a name and a date, nothing else | Registration complete, documents uploaded, ETA and preferences known — staff plan the day. |
| **Foreign-guest paperwork** | Passport photocopying and form-filling at the desk; compliance reporting typed after the fact | Documents pre-uploaded and pre-verified; reporting data ready before arrival. |
| **Peak-day arrivals queue** | Every guest fills the registration card at the desk — 5+ minutes each | Pre-registered guests are greeted, verified, and done — the queue's root cause is removed. |
| **Room staging** | Preferences discovered at the desk; crib/twin-bed scrambles while the guest waits | Preferences captured days ahead; housekeeping stages the right room the first time. |
| **Housekeeping turn sequence** | No ETA data — rooms turned in room-number order, early guests wait | Arrival windows sequence the turns; early arrivals' rooms are ready first. |
| **Ancillary selling pre-stay** | Nobody sells anything between booking and arrival | Targeted add-on shelf converts the planning window at zero staff labor. |
| **No-shows on peak nights** | Unsecured bookings evaporate; the room sells for nothing | Deposit-secured bookings arrive — or the deposit compensates. |
| **Bangla-speaking domestic guests** | English-only forms filled wrong or handed back to staff to fill | Full Bangla journey; forms come back complete and correct. |

---

## 6. How It Increases Revenue

1. **OTA commission recovery.** Every direct booking avoids 15–25% commission. This is the largest single line: shifting even a modest share of bookings from OTA to direct compounds across every room-night, forever.
2. **Pre-arrival add-on attach.** Breakfast, transfers, early check-in, and celebration setups sold in the planning window — vendor upsell platforms (Canary, Duve) report large uplifts when offers land at high-intent moments, and pre-arrival is the highest-intent, lowest-labor window of all.
3. **Deposit-protected peak inventory.** Deposits convert no-shows from a total loss into either an arrival or compensation — directly protecting revenue on the nights that matter most.
4. **Early check-in monetized, not comped.** Arrival-time data plus housekeeping sequencing turns early check-in from an ad-hoc favor into a sellable, deliverable add-on.
5. **Preference-matched stays lift rate power.** Rooms staged right the first time lift satisfaction and review scores, which support ADR and drive the direct rebooking loop with G13.
6. **Funnel effect into every downstream feature.** Each completed pre-arrival flow onboards a guest into check-in upsells (G2/G6), F&B ordering (G5), and activities (G12) — G1's conversion rate multiplies every other feature's revenue.

> **Illustrative model:** A 150-room hotel at 75% occupancy, ৳10,000 ADR, with 40% of bookings on OTAs at 18% commission, pays ~৳26.6 million/year (~$220,000) in commission. If Atrium Guest shifts just **10 points of channel mix** from OTA to direct, that is ~৳6.6 million/year (~$55,000) recovered. Add pre-arrival add-ons at a conservative **12% attach × ৳1,200 average** across ~30,000 stays/year: ~৳4.3 million (~$36,000). Combined ≈ **৳11 million (~$91,000)/year** before counting deposit-protected no-shows and early-check-in sales. *(Figures illustrative; validate against property data.)*

---

## 7. How It Reduces Operational Cost

1. **Minutes cut from every arrival.** Registration, documents, and preferences collected pre-arrival remove the longest steps from the desk transaction — across tens of thousands of arrivals a year, that is entire shifts of labor.
2. **Peak-desk staffing relief.** Because pre-registered arrivals process in a fraction of the time, peak periods need fewer agents stacked at the desk purely to fight the queue.
3. **Deposit automation.** Auto-captured, auto-posted deposits eliminate manual bank-transfer verification, WhatsApp screenshot checking, and payment-chasing calls.
4. **Compliance paperwork done once, correctly.** Pre-uploaded documents and structured registration data feed foreign-guest reporting without re-typing — and without the audit-day scramble.
5. **Fewer wrong-room reworks.** Preference data ahead of assignment means fewer mid-arrival room changes, re-keys, and apology comps.
6. **Reservation-desk call deflection.** Self-service booking, modification, and cancellation within policy removes a large share of the phone traffic the reservations team (staff F2) handles today.
7. **Scheduled work instead of scrambles.** Pre-ordered transfers, setups, and amenities arrive as scheduled staff tasks (F5), not last-minute interruptions.

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Channel shift | Direct-booking share of total reservations | +10 points vs. baseline in year 1 |
| Commission recovery | OTA commission paid per available room | ↓ measurably vs. baseline |
| Visual conversion | Booking-flow conversion after a gallery/tour view | ↑ measurably vs. non-viewers |
| Tier mix | Share of bookings above base room tier | +5 points vs. baseline *(illustrative)* |
| Pre-arrival adoption | Registrations completed before arrival | ≥ 60% of app/link-reached guests |
| Document readiness | Foreign-guest arrivals with documents pre-uploaded | ≥ 50% |
| Ancillary revenue | Pre-arrival add-on attach rate | ≥ 12% of stays |
| Deposit protection | No-show rate on deposit-secured bookings | < 2% |
| Arrival speed | Median desk time for pre-registered guests | ↓ 60% vs. non-registered |
| Localization | Bangla-language journey completion rate | ≥ parity with English |
| Funnel health | Pre-registered guests proceeding to mobile check-in (G2) | ≥ 70% |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| PMS API depth | Real-time availability reads and reservation/registration writes required | Middleware sync layer; graceful degradation to request-and-confirm booking |
| Rate parity | Direct rates undercutting OTA contracts can breach parity clauses | Member-rate and value-add (loyalty perks, add-on bundles) strategies rather than raw undercutting |
| Payment gateways | Deposit capture needs SSLCommerz/bKash/Nagad + card gateway agreements per property | Hosted payment pages/links to keep PCI scope minimal; gateway-per-property configuration |
| Document data protection | ID documents are sensitive personal data; local retention/reporting rules apply | Encryption at rest, role-gated access, configurable retention, audit log |
| Guest adoption | Guests may ignore pre-arrival prompts | App-less email/QR path (G15), Bangla-first UX, well-timed G8 reminders, staff prompt at booking |
| Form compliance variance | Registration requirements differ by property and jurisdiction | Dynamic check-in forms with per-property configuration and versioned audit |
| Content upkeep | Room photos, add-on copy, bilingual content go stale | Staff-side content management with review reminders |
| Fraud on deposits | Stolen-card deposits and chargebacks | Gateway 3-D Secure / OTP rails; deposit-refund policy controls |

---

## 10. Scope delivered

Atrium Guest ships as one complete package. This feature delivers:

- Mobile booking engine with live availability, rates, and instant confirmation
- Room-type galleries (photos, video, 360° virtual tours), amenity lists, and side-by-side room comparison from the shared property media library (reused by G6, G10, G11, G12)
- Deposit payment on bKash, Nagad, SSLCommerz, and international cards
- Pre-arrival digital registration card with PMS auto-sync
- Guided identity-document upload with staff-side verification view
- Dynamic, property-configurable check-in forms
- Arrival-time, transport, and preference capture feeding staff F1/F2 and housekeeping
- Pre-arrival add-on shelf with folio/booking charging
- Full Bangla/English journey, n-language architecture
- Booking modification and cancellation self-service within policy
- App-less web/QR path for every flow (G15)

---

## 11. Open Questions

- Which PMS(es) at launch, and do they support reservation *writes* and registration-card sync (not just availability reads)?
- Rate-parity posture: member-only rates, value-add bundles, or open undercutting — what do target properties' OTA contracts allow?
- Deposit policy defaults: percentage vs. first-night, refund windows, and who configures them per property?
- What are the exact document-retention and foreign-guest reporting requirements per target jurisdiction, and do any require integration with government systems?
- Is add-on merchandising content managed in Atrium or pulled from the PMS/upsell engine (shared question with G6)?
- Launch sequencing: booking engine and pre-arrival together, or pre-arrival first for properties keeping an existing booking engine?

---

## 12. One-Line Business Case

> **Booking & Pre-Arrival moves the guest's first transaction and first paperwork from the OTA and the front desk to the guest's own phone — recovering commission on every direct booking and deleting the queue's root cause before arrival day, while handing every downstream feature a guest who is already known, verified, and ready to spend.**
