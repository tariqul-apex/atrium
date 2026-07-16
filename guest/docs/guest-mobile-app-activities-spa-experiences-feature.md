# Activities, Spa & Experiences Booking — Feature Deep-Dive

**Product:** Atrium Guest (Hotel / Resort Guest-Facing Mobile App)
**Feature area:** G12 — Activities, Spa & Experiences Booking
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-16
**Status:** Draft v1.0
**Related:** [Guest Mobile App — Feature Scope](guest-mobile-app-feature-scope.md) · [Staff Events & Activities — Deep-Dive](../../staff/docs/staff-mobile-app-events-activities-feature.md) · [Competitive Feature Research](../../HOTEL-~1.MD)

---

## 1. Executive Summary

**Activities, Spa & Experiences Booking** is self-service reservation for everything on property that takes a booking: spa treatments, restaurant tables, courts, kids' club sessions, poolside cabanas and sunbeds, boat trips, classes, tours, and day-use passes. The guest browses categorized experiences with **live slot capacity**, books in a few taps, pays in-app or charges to the folio, and watches the stay assemble into a **personal itinerary with reminders**.

The economics of resort ancillaries are unforgiving: they are **high-margin, capacity-bound, and perishable**. The 3pm spa slot unsold at 3pm is revenue destroyed, not deferred — and phone-or-desk-only booking guarantees empty slots, because it only sells when a staff member is reachable, the guest is willing to call, and both speak the same language. Self-service booking sells around the clock, in Bangla or English, at the moment intent peaks — walking past the spa (via the [G11 map](guest-mobile-app-interactive-property-map-feature.md)), reading the guidebook ([G10](guest-mobile-app-digital-guidebook-property-info-feature.md)), or receiving a "slots open tomorrow" campaign ([G8](guest-mobile-app-notifications-campaigns-feature.md)).

Critically, G12 is not a parallel system: it is the **guest surface of Atrium Staff's [F18 Events & Activities](../../staff/docs/staff-mobile-app-events-activities-feature.md)**. Every guest booking lands in the same slot capacity, guide rosters, and event calendars staff already run, and every charge posts through staff [F8 POS/folio](../../staff/docs/staff-mobile-app-pos-multi-outlet-feature.md). One inventory, two surfaces — no overbooked boats, no clipboard reconciliation.

Vendors validate the category: Agilysys and Maestro (property-wide spa/golf/dining/activity reservations), INTELITY and SuitePad (in-app and in-room amenity booking), Operto (guest-portal bookings), and the resort specialists Attractions.io and Nonius (activity planners with online payment). None serve the Bangladesh market with local rails and Bangla content. This document defines the feature's scope, quantifies its business case, and maps it to the journey moments and operations it improves.

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What the guest can do from the app |
|---|------------|------------------------------------|
| 1 | **Browse experiences with live capacity** | Explore activities, spa, dining, courts, kids' club, and tours by category, time, and price — every slot showing real remaining capacity from F18. |
| 2 | **Book & pay or charge to folio** | Reserve and settle in-app — folio charge, or direct payment on cards or local rails (bKash / Nagad / SSLCommerz via [G9](guest-mobile-app-payments-folio-tipping-feature.md)). |
| 3 | **Spa, restaurant, court & kids' club reservations** | Type-aware flows: treatment + therapist preference, table + party size, court + duration, kids' club + child details. |
| 4 | **Personal stay itinerary with reminders** | Every booking on one timeline; reminders via [G8](guest-mobile-app-notifications-campaigns-feature.md) — "Snorkeling starts in 45 minutes, meet at the jetty." |
| 5 | **Resort event & entertainment schedule** | Tonight's live music, the weekly BBQ — the F18 event calendar rendered guest-side, with add-to-itinerary. |
| 6 | **Virtual queues, waitlists & cancellation** | Join a waitlist on a full slot — or a restaurant/facility queue remotely, with live position and a "your table is ready" alert; cancel or reschedule within the property's policy window. |
| 7 | **Group bookings** | The family organizer books 6 snorkeling seats, 2 kids' club sessions, and a dinner table from one screen, one folio. |
| 8 | **Day-use booking** | Day visitors (no room, no folio) book and prepay day passes, activities, and tables — app-lessly via [G15](guest-mobile-app-app-less-access-feature.md). |
| 9 | **Rich experience previews** | Explore every activity visually before booking — photo/video gallery plus a what-to-expect section (duration, difficulty, age limits, what to bring) on each detail page. |
| 10 | **Pool & beach amenity booking** | Reserve cabanas, sunbeds, daybeds, and beach spots by time slot — picking the exact unit on the [G11 map](guest-mobile-app-interactive-property-map-feature.md), with premium cabanas charged to the folio. |

### 2.2 In scope (this release)
Categorized experience catalogue with photos, bilingual descriptions, prices, durations, and prerequisites; rich experience previews — per-experience galleries (photos, video) from the shared property media library plus structured what-to-expect details; live slot capacity read from F18; booking with folio charge or direct payment on cards/local rails (G9), including property-required prepayment/deposits; spa, restaurant, court, and kids' club reservation types; the personal itinerary with G8 reminders; the guest-facing event schedule from F18; waitlists with expiring first-in-line offers; virtual queues for restaurants and facilities — remote joining with live position, table-ready alerts via G8 with a hold window and auto-release, and app-less walk-in joining via a G15 QR at the door; pool and beach amenity booking — cabanas, sunbeds, daybeds, and beach spots by time slot, unit-picked on the G11 map, with premium-cabana folio charges via G9 and inventory managed as F18 resources; self-service cancellation/reschedule within policy, with property-defined fees; group bookings against one folio; day-visitor booking and prepayment via G15; Bangla/English throughout.

### 2.3 Out of scope (this release)
Staff-side activity administration — slot definition, capacity, guide assignment, run-of-show — lives in [F18](../../staff/docs/staff-mobile-app-events-activities-feature.md), not here; private-event booking (weddings, banquets — a sales conversation, not a self-service slot); third-party tour-marketplace integration beyond property-fulfilled experiences; dynamic/yield pricing of slots (property-set prices at v1); spa retail; gift cards.

### 2.4 Key assumptions
- F18 is the single source of truth for slots, capacity, and schedules; G12 reads and writes the same inventory in real time — no shadow calendar.
- F8 POS/folio accepts activity charges against a reservation folio; G9 provides the direct-payment path for prepay and day visitors.
- Cancellation windows, deposit rules, and prerequisites (age, swim ability) are property-set policy at onboarding.
- Restaurant reservations at v1 book against F18 capacity blocks; deeper table management can follow.

---

## 3. Capability Deep-Dive — The Booking Features

Each capability: **what it is**, **how it works in Atrium**, **why it matters**, and the **revenue/cost angle**, with vendor precedents noting the bar to clear.

### 3.1 Browse categorized experiences with live capacity
*Precedent: Attractions.io / Nonius activity planners; Agilysys amenity catalogues.*

**What it is.** The full menu of everything bookable on property — by category (water sports, spa, courts, kids, dining, tours), day, and price — with each slot showing genuine remaining capacity.

**How it works in Atrium.** The catalogue renders the slots staff define in F18 (recurring slots, capacity, assigned guides); "2 places left on the 10:00 snorkeling trip" is the same number the staff app enforces. Cards carry photos, bilingual descriptions, price, meeting point (G11 map-linked), and prerequisites.

**Why it matters.** Guests book what they can see. A laminated card at the pool desk merchandises nothing; a browsable catalogue turns the property's experience inventory into a storefront every guest carries — and live capacity adds urgency that is honest.

**Revenue / cost angle.** Catalogue visibility is the top of the ancillary funnel (revenue); guests answer their own "what is there to do?" questions (cost).

### 3.2 Book & pay, or charge to folio
*Precedent: Attractions.io book-and-pay; Agilysys folio integration; SuitePad; local rails are an Atrium/BD differentiator.*

**What it is.** Completing the reservation and its money in one flow: in-house guests charge to the room in a tap; anyone can pay by card or bKash/Nagad/SSLCommerz; properties can require deposits or prepayment.

**How it works in Atrium.** An in-house booking posts its charge to the folio through staff F8 — appearing instantly, itemized, on the guest's live G9 folio. Direct payment runs through G9's gateways; per-activity policy decides folio-only, prepay, or deposit, and cancellation fees post through the same path.

**Why it matters.** Payment friction is where bookings die — "come to the desk to confirm" loses half of them. Folio charging makes booking effortless; local rails make direct payment real in a market where card assumptions exclude most domestic guests.

**Revenue / cost angle.** Every completed in-app payment is a sale phone-tag would have lost (revenue); prepay/deposit policy slashes no-show losses (revenue protection); zero staff time per transaction (cost).

### 3.3 Spa, restaurant, court & kids' club reservations
*Precedent: Agilysys spa/dining reservations; Maestro multi-amenity booking; INTELITY, SuitePad.*

**What it is.** The reservation types that need more than a generic slot: treatment + therapist preference; table + party size; court + duration; kids' club with child name, age, allergies, and pickup contact.

**How it works in Atrium.** Each activity type in F18 declares its booking schema; G12 renders the matching flow. Details land staff-side ready to act — the spa sees the treatment on its F18 roster, the kids' club guide sees the child's details on the slot sheet. Nothing is re-keyed.

**Why it matters.** Generic bookings create follow-up calls ("what treatment did you want?") that erase the self-service gain; type-aware flows capture everything up front, and safety-relevant details arrive structured instead of scribbled.

**Revenue / cost angle.** Complete bookings enable utilization planning that lifts sold capacity (revenue); no clarification calls, no re-keying (cost); structured child/safety data cuts liability exposure (risk).

### 3.4 Personal stay itinerary with reminders
*Precedent: Nonius/STAY itinerary patterns; HolidayHero stay planning; reminders via G8.*

**What it is.** One timeline of the guest's whole stay — every activity, treatment, table, and event booked or saved — with reminders before each item, including meeting point and what to bring.

**How it works in Atrium.** Every booking auto-joins the itinerary; event items add by tap. G8 delivers reminders with the meeting point linked to its G11 map pin; companions on a group booking see the shared itinerary. Day-by-day gaps are visible — exactly where G8 campaigns and G6 offers aim.

**Why it matters.** A planned stay is a fuller stay: itinerary-holding guests show up on time and report higher satisfaction. The empty Tuesday afternoon visible on an itinerary is a merchandising target no phone-booking property can even see.

**Revenue / cost angle.** Reminders cut no-shows on perishable slots (revenue protection); itinerary gaps drive targeted fill campaigns (revenue); fewer "when was my massage?" calls (cost).

### 3.5 Resort event & entertainment schedule
*Precedent: Nonius push/schedule patterns; resort-app what's-on content; the staff side is F18's event calendar.*

**What it is.** The guest-facing "what's on": tonight's live band, the Friday BBQ, the kids' show — with add-to-itinerary and, where ticketed, booking.

**How it works in Atrium.** F18 events flagged guest-visible appear automatically with time, venue (G11-linked), and price if any; ticketed events book like any slot. G8 campaigns can promote an undersold event to on-property guests two hours before showtime.

**Why it matters.** Entertainment only fills seats if guests know it exists — and printed weekly programs are outdated by Tuesday. An attended event keeps guests (and their dinner-and-drinks spend) on property for the evening.

**Revenue / cost angle.** Event attendance drags F&B spend — the band is a loss-leader for the bar (revenue); one F18 calendar serves staff and guests with zero re-publication (cost).

### 3.6 Virtual queues, waitlists & cancellation
*Precedent: waitlist mechanics from activity-booking practice (FareHarbor/Xola-class, per the F18 precedents); SevenRooms-class restaurant waitlist and table-ready systems; largely absent from hotel guest apps.*

**What it is.** Full slots take a waitlist instead of a hard no; busy restaurants and walk-up facilities take a **virtual queue** instead of a visible line — join remotely, watch a live queue position, get told when it's your turn; cancellations are self-service within policy — and a cancellation automatically offers the freed place to the first waitlisted guest.

**How it works in Atrium.** "Join waitlist" replaces the dead end on a full slot. When a booking cancels (guest in-policy, or staff-side in F18), the slot offers itself to the head of the waitlist via G8 push with a claim window, cascading if unclaimed. For restaurants and facilities without a slot to book, the guest joins the queue from anywhere on property — the pool, the room — and watches a live position; walk-ins at the door join app-lessly by scanning a [G15](guest-mobile-app-app-less-access-feature.md) QR at the host stand. When the table frees, a "your table is ready" push via [G8](guest-mobile-app-notifications-campaigns-feature.md) opens a hold window; a no-show past the window auto-releases the place to the next party. Policy — free window, fee, cutoff, hold length — is property-set per activity or outlet, with fees posting via F8/G9.

**Why it matters.** For perishable capacity the waitlist is a revenue-recovery machine: a 4pm cancellation of a 5pm treatment resells itself in minutes with zero staff effort. Self-service cancellation also surfaces frees *earlier* — guests cancel in-app hours ahead where they would simply no-show a phone booking. And a visible line is a demand killer: the party that sees a 40-minute crowd at the host stand walks away; the party holding a queue position from a sunbed stays — and often orders a drink while waiting.

**Revenue / cost angle.** Recovered cancellations on high-margin slots go straight to the bottom line (revenue); virtual queues keep would-walk-away parties in the funnel, covering more of the restaurant's demand curve (revenue); enforced fees end awkward desk negotiations (cost); no phone-tag to fill freed slots, no host clipboard, fewer crowding complaints at the entrance (cost).

### 3.7 Group bookings
*Precedent: party-size booking in activity platforms; the "resort family / group organizer" persona — under-served by every incumbent.*

**What it is.** One organizer booking one experience for many people — 6 snorkeling seats, a table for 10, kids' club for three children — in a single flow, on one folio.

**How it works in Atrium.** The organizer sets party size; per-person details are collected where the schema requires them. Capacity is checked and held atomically for the whole party — no partial bookings that strand half a family. Charges consolidate to the organizer's folio; linked companions see the bookings on their own itinerary.

**Why it matters.** The multigenerational family group is the backbone of the Bangladeshi resort market, and one organizer plans for everyone. If the app cannot book for eight, that organizer goes to the desk — and the self-service gain evaporates for the highest-spending party type on the property.

**Revenue / cost angle.** Group bookings are the largest tickets the catalogue takes (revenue); one in-app flow replaces the longest, most error-prone desk interactions (cost).

### 3.8 Day-use booking for day visitors
*Precedent: day-pass patterns from the attractions world (Attractions.io); the Atrium Guest day-visitor persona — G5/G9/G11/G12/G14 only, no stay.*

**What it is.** Booking for guests who never check in: locals buying a pool day pass, a spa afternoon, a court hour, or a brunch table — without a reservation or folio.

**How it works in Atrium.** Day visitors reach the same catalogue through G15 link/QR web flows — no install, no account beyond a phone number. With no folio, payment is always up-front via G9's rails; the booking lands in the same F18 inventory, flagged day-use, and the visitor gets a QR confirmation for the gate. Policy can hold day-use inventory separate from in-house allocations.

**Why it matters.** The local day market is a revenue pool most properties serve with a phone line and a gate ledger. Self-service day booking opens midweek and off-peak capacity to an audience of millions — and every day visitor is a future room-night prospect the property now has a channel to.

**Revenue / cost angle.** Day-use monetizes capacity room guests can't fill — off-peak pools, weekday courts, brunch covers (new revenue); prepayment eliminates non-guest no-show risk; the gate ledger and its leakage disappear (cost/control).

### 3.9 Rich experience previews
*Precedent: Attractions.io activity pages; Nonius resort guest app; SuitePad in-room media.*

**What it is.** Every activity and experience detail page carries a gallery — photos, video — plus a structured what-to-expect section: duration, difficulty, age limits, and what to bring — so the guest can explore the full catalogue visually before committing to a booking.

**How it works in Atrium.** Galleries draw from the **shared property media library** that also feeds [G1's room-type galleries](guest-mobile-app-booking-pre-arrival-feature.md), [G6's upgrade previews](guest-mobile-app-upsells-offers-feature.md), and [G10's facility showcases](guest-mobile-app-digital-guidebook-property-info-feature.md) — one upload, every surface. What-to-expect fields come from the F18 activity definition (the same prerequisites §3.1 shows), rendered bilingually; the preview page ends where every card does — in the live slots and the book button.

**Why it matters.** Preview quality directly drives slot fill: an activity the guest can see sells, and the boat trip is a paragraph until the video shows the reef. What-to-expect answers the hesitations — "is it too hard for my mother? do the kids need shoes?" — that otherwise stall a booking or become a desk call.

**Revenue / cost angle.** Better previews lift conversion on the same high-margin, perishable inventory — pure funnel gain (revenue); media reuse from the shared library means no G12-specific shoots (cost); pre-answered logistics questions cut clarification calls and day-of surprises (cost).

### 3.10 Pool & beach amenity booking
*Precedent: resort guest-app specialists — Nonius, STAY, LoungeUp; Agilysys multi-amenity resorts.*

**What it is.** Reserving the sun itself: cabanas, sunbeds, daybeds, and beach spots bookable by time slot — with the guest picking the **exact unit** ("that cabana, by the pool bar") rather than a generic category, and premium units carrying a fee.

**How it works in Atrium.** Cabanas and sunbeds are [F18](../../staff/docs/staff-mobile-app-events-activities-feature.md) resources with time-slot inventory, and the picker is the [interactive property map (G11)](guest-mobile-app-interactive-property-map-feature.md) — the guest taps the exact spot they want and sees its price and availability. Premium cabana fees charge to the folio via [G9](guest-mobile-app-payments-folio-tipping-feature.md). Once settled in, towel service is a [G4](guest-mobile-app-in-stay-service-requests-feature.md) request and cabana F&B a [G5](guest-mobile-app-food-beverage-ordering-feature.md) order — both placed from the reserved spot, whose location rides along so staff find the guest without explanation. Inventory and turnover between slots are managed staff-side through F18 resources.

**Why it matters.** The sunbed scramble — towels claimed at dawn, arguments at noon — is a daily satisfaction sore on every resort, and today it is refereed by pool staff with no system behind them. Reservation ends the scramble, and a guest anchored at a known spot for hours is the easiest F&B customer on the property.

**Revenue / cost angle.** Monetizes previously free-for-all sun inventory — premium cabana fees are new, near-pure-margin revenue — and the anchored guest is exactly where G5 poolside ordering captures its spend (revenue); ends the towel-and-argument chaos over sunbeds and the staff time spent refereeing it (cost).

---

## 4. Why This Matters

### 4.1 Perishable capacity punishes friction
Every unsold slot expires worthless at its start time. Phone-and-desk booking sells only during staffed hours, to guests willing to call, in the language of whoever answers. Self-service removes every one of those filters — the inventory is on sale 24/7 to every guest at once.

### 4.2 Intent is impulsive; booking must be instant
The urge to book a massage strikes walking past the spa; the snorkeling decision happens at breakfast looking at the sea. If acting means finding a phone number, the impulse dies. G12 puts the transaction inside the moment of intent — fed by the map (G11), the guidebook (G10), and campaigns (G8).

### 4.3 One inventory, two surfaces — or chaos
A guest channel that doesn't share the staff side's capacity creates the overbooked boat and the double-booked therapist. G12's defining architecture is that it *is* F18's inventory, guest-facing: staff run the slot sheet in the staff app, guests fill the seats from theirs, and neither can contradict the other.

### 4.4 The BD market opening
Agilysys-class amenity booking exists only at international luxury flags; no local vendor ships guest-facing activity booking at all — let alone with bKash/Nagad, Bangla content, group flows, and day-use. For Bangladesh's resort belt this is white space Atrium can own.

---

## 5. Journey Moments & Operations It Improves

| Moment / operation today | Pain | With Activities & Experiences Booking |
|--------------------------|------|---------------------------------------|
| **Pre-arrival planning** | Guest arrives with no plan; peak slots go unsold | Pre-book from the catalogue the day the stay confirms. |
| **"What is there to do here?"** | Ask the desk; get a verbal list | Browsable, priced catalogue with live availability. |
| **Booking a spa slot** | Call during spa hours; hold; hope | Live slots, therapist preference, folio charge — 90 seconds. |
| **Family organizer planning a day** | Long desk session; partial bookings | One group flow: 6 seats + kids' club + dinner, one folio. |
| **Slot is full** | "Sorry" — intent dies | Waitlist joins; freed places resell via push. |
| **Late cancellation** | Slot dies unsold; fee argued at the desk | Policy-enforced fee; waitlist refills the slot in minutes. |
| **Guest forgets the booking** | No-show; guide waits; slot wasted | G8 reminder with meeting point and map directions. |
| **Tonight's entertainment** | Printed program, outdated and unseen | Live what's-on schedule; add to itinerary in a tap. |
| **Pool-desk clipboard sign-ups** | Illegible, uncharged, over-capacity risk | Structured bookings, enforced capacity, automatic charge. |
| **Local wants a day pass** | Phone the property; cash at the gate | G15 web booking, bKash prepaid, QR at the gate. |

Guest friction removed: phone-tag, language barriers, desk queues, dead ends on full slots, forgotten bookings. Staff load removed: order-taking calls, clipboard reconciliation, capacity policing, cancellation arguments, re-keying into calendars.

---

## 6. How It Increases Revenue

1. **Filling perishable, high-margin capacity is the headline.** Spa hours, boat seats, and court slots carry strong margins and zero salvage value. Self-service widens the selling window from staffed-hours-by-phone to always-on — and the difference lands almost entirely as gross profit, since the therapist and the boat were being paid for anyway.
2. **Pre-arrival capture locks spend in early.** Guests who pre-book commit ancillary spend before competing options (town restaurants, outside operators) get a look — and pre-booked guests build bigger itineraries than walk-ups.
3. **No-show and cancellation recovery.** Reminders cut no-shows; prepay/deposit policy covers the rest; waitlists resell freed slots automatically — converting the leakiest part of activity revenue into collected revenue.
4. **Group flows multiply ticket size.** Serving the family organizer turns one interaction into six-seat, multi-activity, single-folio transactions.
5. **Day-use opens a new market.** Prepaid day passes, courts, and brunch covers monetize off-peak capacity with an audience the property previously had no channel to — and seed the room-night pipeline.
6. **Events drag F&B.** A well-attended music night keeps guests and their bar spend on property; the schedule plus G8 promotion fills the room that fills the bar.

> **Illustrative model:** A resort running a 10-treatment/day spa at 55% utilization that lifts to 70% via always-on booking, waitlist refill, and reminder-cut no-shows adds ~1.5 treatments/day; at ৳4,000 average that is ~৳2.2M/year from the spa alone. Add 20 prepaid day-use bookings/week at ৳1,500 (~৳1.6M/year) plus gains across boats, courts, and kids' club, and G12 plausibly clears **৳5M+/year** for a mid-size resort. *(Figures illustrative; validate against property data.)*

---

## 7. How It Reduces Operational Cost

1. **Order-taking labor disappears.** Every self-served booking replaces a phone call or desk interaction — across spa reception, the pool desk, restaurant phones, and the concierge.
2. **No shadow systems to reconcile.** One F18 inventory ends the clipboard-vs-calendar-vs-POS reconciliation and the uncharged sign-ups that leak revenue and audit time.
3. **Capacity is enforced, not policed.** The system cannot oversell the boat; staff stop refereeing over-capacity situations, and the liability exposure of an overloaded activity drops with them.
4. **Cancellation policy without confrontation.** The app enforces the property's published policy identically for everyone — no desk arguments, no inconsistent waivers, no goodwill comps.
5. **Structured details, fewer errors.** Treatments, party sizes, and child details arrive typed and complete — no callbacks, no misheard bookings, no guide surprised by seven guests on a six-seat boat.
6. **Guides plan from real rosters.** F18 slot sheets fed by live bookings let managers right-size guide assignments and cut idle hours held "just in case" walk-ups appear.

> **Illustrative model:** If self-service absorbs ~50 booking interactions/day at ~4 staff-minutes each, that is ~3.3 staff-hours/day ≈ **1,200 staff-hours/year** — before reconciliation time, dispute handling, and comps avoided. *(Figures illustrative; validate against property data.)*

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Fill perishable capacity | Slot utilization (spa / activities / courts) | ↑ 10–15 points vs. baseline |
| Shift channel | Bookings via app/G15 vs. phone/desk | ≥ 60% within 6 months |
| Capture early | Stays with ≥1 pre-arrival booking | ≥ 25% |
| Previews sell slots | Experience-preview → booking conversion | ≥ 10% of preview views |
| Cut no-shows | No-show rate on reminded bookings | ↓ 50% vs. baseline |
| Recover cancellations | Freed slots refilled via waitlist | ≥ 40% |
| Monetize sun inventory | Premium cabana occupancy / revenue per available cabana | ≥ 70% peak-day occupancy *(illustrative)* |
| Keep queued demand | Virtual-queue abandonment rate | ≤ 20% *(illustrative)* |
| Grow ticket size | Group-booking share of booking revenue | Tracked; ↑ |
| Open day market | Prepaid day-use bookings per month | ↑ measurable |
| Bill everything | Bookings charged (folio or prepaid) vs. delivered | ≥ 99% |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| F18 inventory readiness | Slots, capacity, and guides must be defined staff-side first | F18 onboarding precedes G12; catalogue goes live per activity as configured. |
| Real-time capacity integrity | Guest and staff writes race on the last seat | Atomic slot holds with short expiry; F18 capacity is the single arbiter. |
| F8 folio posting | Charges must post to the correct folio reliably | Post-on-confirm with reconciliation report; failed posts alert staff before service. |
| Payment rails (G9) | Prepay/day-use depends on gateway coverage | bKash/Nagad/SSLCommerz + cards; folio-charge path works even if a gateway is down. |
| Cancellation policy design | Too strict kills bookings; too loose kills slots | Property-set per activity with BD-market defaults; measure and tune. |
| Waitlist claim mechanics | Unclaimed offers can strand a slot | Short claim windows with cascade; fallback flags the slot to staff for walk-up sale. |
| Map inventory accuracy | Unit-level cabana/sunbed booking needs the G11 map to match the physical layout | Unit map validated at onboarding; re-surveyed on layout changes; staff flag mismatches via F18. |
| Hold-window no-show policy | Too short angers a party mid-walk; too long strands the table | Property-set hold length with market defaults; G8 warning push before auto-release; measure and tune. |
| Restaurant-booking depth | Table management is its own discipline | v1 books against F18 capacity blocks; deeper integration as a follow-on. |
| Child-data handling | Kids' club bookings carry minors' details | Minimal collection, purpose-limited retention, access restricted to assigned guides. |
| Adoption cliff without app | Booking limited to app installers | G15 serves the full catalogue as link/QR web flows — same inventory, no install. |

---

## 10. Scope delivered

Atrium Guest ships as one complete package. This feature delivers:

- Categorized experience catalogue (activities, spa, dining, courts, kids' club, tours) with photos, bilingual descriptions, prices, prerequisites
- Rich experience previews: photo/video galleries and what-to-expect details (duration, difficulty, age, what to bring) per experience, served from the shared property media library (with G1/G6/G10)
- Live slot capacity read from, and booked against, staff-side F18 inventory — one source of truth
- Booking with folio charge via staff F8, or direct payment on cards and local rails via G9, with property-set prepay/deposit rules
- Type-aware flows: spa treatment & therapist preference, table & party size, court & duration, kids' club with child details
- Personal stay itinerary with G8 reminders carrying G11 map-linked meeting points
- Guest-facing event & entertainment schedule rendered from F18, with add-to-itinerary and ticketed booking
- Waitlists with push-notified, expiring claim offers and automatic cascade
- Virtual queues for restaurants and facilities: remote joining with live position, walk-in QR joining via G15, "your table is ready" alerts via G8 with property-set hold windows and auto-release on no-show
- Pool & beach amenity booking: cabanas, sunbeds, daybeds, and beach spots by time slot, unit-picked on the G11 map, premium-cabana folio charges via G9, towel service and cabana F&B from the spot via G4/G5, inventory and turnover as F18 resources
- Self-service cancellation and reschedule with property-defined policy windows and fees
- Group booking: atomic multi-person holds, per-person details, consolidated folio, shared itinerary visibility
- Day-use booking and prepayment for day visitors via G15 web flows, with QR confirmation
- Bangla/English throughout

---

## 11. Open Questions

- Which activity categories lead at launch — spa and water sports (resort belt) or dining and day-use (city properties)?
- Is guest self-service cancellation right for all activity types, or should high-value slots (boats, private tours) need staff approval?
- How is day-use inventory governed — shared with in-house guests, separate allocation, or dynamic by occupancy?
- Are F18 capacity blocks sufficient for restaurant reservations at launch, or does a flagship outlet force earlier table-management depth?
- Should therapist/guide *selection* (not just preference) be exposed, and how do properties feel about staff being individually bookable?
- Do group bookings need split-payment across companions at v1, or is single-folio consolidation enough (splits settle later via G9)?

---

## 12. One-Line Business Case

> **Activities, Spa & Experiences Booking puts the property's entire bookable inventory on sale to every guest, around the clock, in their language — filling perishable high-margin slots that phone-only booking leaves empty, recovering cancellations automatically, and landing every booking straight into the staff calendars and folios that run the operation.**
