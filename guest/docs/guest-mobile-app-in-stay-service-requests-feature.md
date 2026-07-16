# In-Stay Service Requests — Feature Deep-Dive

**Product:** Atrium Guest (Hotel / Resort Guest-Facing Mobile App)
**Feature area:** G4 — In-Stay Service Requests
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-16
**Status:** Draft v1.0
**Related:** [Guest Mobile App — Feature Scope](guest-mobile-app-feature-scope.md) · [Competitive Feature Research](../../HOTEL-~1.MD)

---

## 1. Executive Summary

**In-Stay Service Requests** gives the guest one structured place to ask for anything during the stay — extra towels, a broken AC, a wake-up call, a luggage pickup, a housekeeping time change — without picking up the phone, walking to the desk, or wondering whether anyone wrote it down. Every request is typed, timestamped, photographed where useful, and **auto-routed into the Atrium Staff app** as a tracked task with a service-level timer the guest can see.

This is the feature that most directly connects the two halves of the Atrium platform: the guest taps, and the request lands in [staff Task Management (F5)](../../staff/docs/staff-mobile-app-task-management-communication-feature.md) as an owned, deadlined task — issues route to [Maintenance & Work Orders (F4)](../../staff/docs/staff-mobile-app-maintenance-work-orders-feature.md), housekeeping preferences to [Housekeeping Management (F3)](../../staff/docs/staff-mobile-app-housekeeping-management-feature.md), lost items to [Lost & Found (F16)](../../staff/docs/staff-mobile-app-lost-found-staff-safety-feature.md). Nothing is relayed by radio, nothing is scribbled on a pad, nothing is lost between departments.

The business case rests on three effects: **satisfaction** (requests get done, visibly, on time — the single biggest driver of in-stay sentiment), **call deflection** (structured self-service removes the phone-and-desk traffic that consumes front-desk hours), and **accountability** (every request has an owner, an SLA, and an audit trail — the raw material of service-recovery and staffing decisions). Vendors who ship guest-initiated requests — Guestline Keez, Apaleo on the guest side; Knowcross and Quore on the staff-side request-management engine — treat closed-loop request handling as the defining test of a guest app that actually works, rather than one that merely looks good.

This document defines the feature's scope, quantifies its business case, and maps it to the journey moments and operations it improves.

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What the guest can do from the app |
|---|------------|-------------------------------------|
| 1 | **Request items** | Order towels, pillows, blankets, toiletries, adapters, cots, iron — from a picture-menu of amenities, quantity and delivery-time selectable. |
| 2 | **Report an issue (with photo)** | Photograph a broken fixture, a leak, a noisy AC; describe it in Bangla or English; submit — it becomes a staff work order automatically. |
| 3 | **Concierge requests** | Book a wake-up call, request transport (airport drop, local car), schedule a luggage pickup or storage — each a structured, time-boxed request. |
| 4 | **Housekeeping scheduling & Do-Not-Disturb** | Pick a preferred cleaning window, ask for service now, or set DND from the phone — status syncs live to the attendant's board. |
| 5 | **Green opt-out of daily cleaning** | Decline daily cleaning for a sustainability perk (loyalty points, F&B credit) — logged straight into the housekeeping schedule. |
| 6 | **Report a lost item** | Describe and photograph a lost item, state where it was last seen — filed directly into the staff Lost & Found register with a claim reference. |
| 7 | **Live request status with SLA timer** | See each open request's status (received → assigned → in progress → done) and the property's promised response time counting down. |
| 8 | **Standing / recurring requests** | Set repeat requests for the whole stay — "extra pillows every night," "turndown at 8pm," "daily 6am wake-up call" — entered once, executed daily. |
| 9 | **Transport & mobility requests** | Book an airport transfer with driver and vehicle details and live pickup tracking, check the shuttle timetable with live location, have the valet bring the car to the door, request porter/luggage help, or hand off to Pathao/Uber with the property address pre-filled. |
| 10 | **Guest safety & emergency assistance** | Reach a one-tap emergency button, request medical assistance, open the evacuation & emergency information page, ask for female staff attendance, and call emergency contact numbers — routed at critical priority to the duty manager and security. |

### 2.2 In scope (this release)

Amenity-request catalog with quantities and delivery windows; photo-attached issue reporting; concierge request types (wake-up call, transport, luggage); housekeeping window selection, service-now, and DND; green opt-out with perk crediting; lost-item reporting with claim tracking; per-request live status and SLA countdown; standing/recurring requests; transport & mobility requests (airport transfer booking with driver details and pickup tracking, shuttle timetable with live location, valet car retrieval, porter/luggage help, ride-hailing deep links to Pathao/Uber); guest safety & emergency assistance (one-tap emergency button, medical-assistance request, evacuation & emergency information, female-staff-attendance option, emergency contact numbers) with critical-priority routing that bypasses normal SLAs; auto-routing into staff F5/F4/F3/F16 with department, room, and priority pre-filled; app-less access to every flow via QR / web link (G15); Bangla/English UI.

### 2.3 Out of scope (this release)

In-room environmental controls (lighting, temperature — a smart-room integration, future); F&B ordering (that is [G5](guest-mobile-app-food-beverage-ordering-feature.md)); spa/activity booking (that is [G12](guest-mobile-app-activities-spa-experiences-feature.md)); free-form chat with staff (that is [G7 Guest Messaging](guest-mobile-app-guest-messaging-feature.md) — though a request thread can escalate into a chat); staff-side dispatch logic and workload balancing (owned by staff F5).

### 2.4 Key assumptions

- Atrium Staff Task Management (F5) is deployed at the property — G4 is a front-end onto its routing, assignment, and SLA engine, not a parallel system.
- The property has defined a request catalog (items, concierge types, SLAs per request type) during onboarding; sensible defaults ship out of the box.
- Room context comes from the stay record (checked-in guest) or a scanned in-room QR code (app-less path), so requests never arrive without a location.
- Housekeeping status (DND, cleaning windows, green opt-out) syncs with the staff F3 room board in near-real time.

---

## 3. Capability Deep-Dive — The Ten Request Capabilities

The competitive research (Part A §4, *In-Stay Services & In-Room Engagement*) shows guest-initiated requests as a defining capability of the modern guest app. Each capability is elaborated below: **what it is**, **how it works in Atrium**, **why it matters**, and the **revenue/cost angle**. Vendor precedents are noted so we know the bar to clear.

### 3.1 Request items (towels, pillows, amenities)
*Precedent: Guestline Keez, Apaleo (in-app service requests).*

**What it is.** A visual catalog of requestable items — linens, toiletries, extras — the guest orders like a shopping list instead of describing over the phone.

**How it works in Atrium.** The guest opens Requests, picks a category, taps items and quantities, chooses "now" or a delivery window, and submits. The request lands in staff F5 pre-tagged with department (housekeeping runner), room number, items, and the property's SLA for that item class. The guest sees the timer start immediately.

**Why it matters.** Phoned item requests fail in specific, repeated ways: the line is busy, the agent mishears "two pillows" as "two towels," the note never reaches the runner, the guest calls again angrier. A structured order eliminates every one of those failure points — and works for guests who won't call at all (a majority of younger travelers) and for foreign guests who would struggle on a Bangla phone line.

**Revenue / cost angle.** Item requests are the highest-volume request type; deflecting them from phone to app removes a large slice of front-desk call traffic (cost). Fulfilled-on-time basics are the floor of review scores — missed towels are disproportionately cited in negative reviews (revenue protection).

### 3.2 Report an issue with photo
*Precedent: Guestline Keez, Apaleo; staff-side engine per Knowcross, Quore.*

**What it is.** A guest-facing defect report — photo, description, location — that becomes an engineering work order without a human transcribing it.

**How it works in Atrium.** The guest photographs the problem, adds a note (Bangla or English), and submits. Atrium classifies the issue type, attaches room and guest context, and files it directly into [staff F4 Maintenance & Work Orders](../../staff/docs/staff-mobile-app-maintenance-work-orders-feature.md) — the same photo-rich work order an engineer would have created, but authored by the guest at zero staff cost. The guest tracks status; on completion they get a "fixed — anything else?" confirmation.

**Why it matters.** The gap between "guest notices a problem" and "engineering knows about it" is where comps, complaints, and bad reviews are born. Guests routinely don't report small defects at all ("not worth the hassle of calling") — and unreported defects greet the *next* guest too. A ten-second photo report collapses that gap and surfaces defects that would otherwise compound.

**Revenue / cost angle.** Earlier defect detection means cheaper repairs and fewer out-of-order room-nights (cost and inventory). Every issue caught and fixed in-stay is a negative review pre-empted — direct protection of the review scores that drive rate power and direct bookings (revenue). Feeds [G14 Feedback & Service Recovery](guest-mobile-app-feedback-service-recovery-feature.md) when an issue signals dissatisfaction.

### 3.3 Concierge requests — wake-up call, transport, luggage
*Precedent: INTELITY (wake-up calls & service scheduling from the device).*

**What it is.** The classic concierge transactions — scheduled and structured rather than phoned and hoped-for.

**How it works in Atrium.** Wake-up calls are set to the minute and executed by automated call plus push, with a staff fallback task for VIPs. Transport requests capture destination, time, party size, and luggage, routing to the concierge/transport desk in F5 with a price quote where applicable (an upsell surface — see [G6](guest-mobile-app-upsells-offers-feature.md)). Luggage pickup/storage requests are timed tasks routed to the bell desk — particularly load-bearing on departure day.

**Why it matters.** These requests are time-critical: a missed wake-up call or late airport car is a stay-ruining failure with real consequences (missed flights). Structured scheduling with confirmation and an SLA timer converts them from verbal promises into system-guaranteed events.

**Revenue / cost angle.** Transport is directly billable — an in-app transport request is a folio charge on [G9](guest-mobile-app-payments-folio-tipping-feature.md), often at a healthy margin (revenue). Automated wake-up execution removes a nightly manual chore from the desk (cost). Departure-day luggage-and-transport flow smooths the checkout crush that otherwise demands extra morning staffing (cost).

### 3.4 Housekeeping scheduling & Do-Not-Disturb
*Precedent: staff-side DND flags per Infor HMS / protel; guest-side scheduling is the Atrium extension.*

**What it is.** Guest control over *when* housekeeping happens — a chosen window, a "service now" request, or DND — replacing the door hanger.

**How it works in Atrium.** The guest picks a preferred window or toggles DND; the choice syncs to the attendant's live board in [staff F3](../../staff/docs/staff-mobile-app-housekeeping-management-feature.md), which already sequences rooms around DND flags. "Service my room now" enters the attendant's queue as a priority insert. Changes are two-way: if housekeeping is about to skip a DND room at end of shift, the guest gets a "last chance for service today" nudge via [G8 notifications](guest-mobile-app-notifications-campaigns-feature.md).

**Why it matters.** The number-one housekeeping friction on both sides is the knock at the wrong time: guests hate being interrupted, attendants waste walk-backs to skipped rooms. Scheduling turns a guess into a plan — attendants route efficiently, guests are never surprised.

**Revenue / cost angle.** Fewer skipped-and-revisited rooms means measurably fewer wasted attendant minutes per floor per day (cost). Interruption-free stays lift satisfaction scores (revenue protection). Accurate DND data improves the staff-side workload forecast in F3.

### 3.5 Green opt-out of daily cleaning
*Precedent: SuitePad (green / opt-out housekeeping option).*

**What it is.** A one-tap choice to decline daily cleaning in exchange for a sustainability perk — loyalty points, an F&B credit, or a donation.

**How it works in Atrium.** The evening before, the app offers the opt-out; the guest accepts and picks a perk. The room drops off the next day's cleaning list in F3 automatically, and the perk posts to the loyalty balance ([G13](guest-mobile-app-loyalty-guest-profile-feature.md)) or folio. The property configures eligibility (e.g., not on checkout days, max consecutive skips) and the perk economics.

**Why it matters.** It is the rare feature where guest preference, brand positioning, and cost reduction all point the same way. A meaningful share of leisure guests prefer *not* to be cleaned daily; today they express it with a door hanger the property can't plan around. An opt-out captured the night before is a room removed from tomorrow's workload *before* staffing is set.

**Revenue / cost angle.** Each opted-out room saves a full clean — attendant time, linen, laundry, chemicals; even a modest opt-out rate compounds into significant monthly savings (see §7). The perk cost is a fraction of the clean cost, and perks paid in loyalty points or F&B credit recycle spend back onto the property (revenue). A visible sustainability program also supports corporate/ESG-sensitive bookings.

### 3.6 Report a lost item
*Precedent: staff-side lost & found per Infor HMS concierge/lost-and-found task management.*

**What it is.** A structured lost-item report — description, photo if available, last-seen location — filed from the app during or after the stay.

**How it works in Atrium.** The guest submits the report; it lands in the [staff Lost & Found register (F16)](../../staff/docs/staff-mobile-app-lost-found-staff-safety-feature.md) as a claim with a reference number. Matches against logged found items are flagged to staff; the guest sees claim status and arranges return or courier (a billable service) in-app. The flow works post-checkout via web link (G15) — the moment most lost items are discovered.

**Why it matters.** Lost-item handling is a small-volume, high-emotion operation that today runs on phone calls to a desk that must relay to housekeeping and hope. A tracked claim with a reference number turns an anxious phone chase into a process — and post-stay reachability matters, since the guest has usually left when they notice.

**Revenue / cost angle.** Courier-return is a billable service (revenue, minor). The larger effect is deflected multi-call phone chases and the goodwill of a professional recovery — recovered items are a classic loyalty moment (revenue protection).

### 3.7 Live request status with SLA timer
*Precedent: staff-side SLA tracking per Knowcross, Quore; guest-visible timer is the Atrium differentiator.*

**What it is.** A visible, per-request status trail — received, assigned, in progress, done — with the property's promised response time counting down in front of the guest.

**How it works in Atrium.** Every request type carries a property-configured SLA (e.g., items 15 min, engineering assessment 30 min). The guest sees the countdown; staff F5 sees the same clock with escalation to a supervisor as breach approaches. Completion pings the guest with a one-tap "all good / not resolved" confirmation, closing the loop — an unresolved tap re-opens the task at higher priority and can trigger [G14 service recovery](guest-mobile-app-feedback-service-recovery-feature.md).

**Why it matters.** The worst part of any request today is silence — the guest doesn't know if anything is happening, so they call again, creating duplicate work and rising frustration. A visible timer replaces anxiety with confidence, and — because the same clock drives staff escalation — it makes the promise self-enforcing. This is the trust mechanism that makes guests use the feature a second time.

**Revenue / cost angle.** Eliminates duplicate "is it coming?" calls (cost). SLA performance data per department per hour becomes management's staffing and accountability instrument in staff F11 analytics (cost). Kept promises are the cheapest satisfaction uplift available (revenue protection).

### 3.8 Standing / recurring requests
*Precedent: Atrium extension of the request model; pre-arrival standing requests per the guest journey matrix (G4 ◐ at Pre-Arrival).*

**What it is.** Requests entered once and executed on a schedule for the whole stay — nightly extra pillows, daily 6am wake-up, turndown at 8pm, ice at 5pm.

**How it works in Atrium.** The guest marks any eligible request as recurring with a schedule; Atrium generates the daily task instances into F5/F3 automatically, tied to the stay dates and cancelled at checkout. Standing requests can be set pre-arrival (matrix stage ◐), so the first night is already right. Preferences learned here persist to the guest profile ([G13](guest-mobile-app-loyalty-guest-profile-feature.md)) and pre-populate the next stay.

**Why it matters.** Repeating the same request daily is friction the guest eventually gives up on — and each re-request is another phone call, another chance to be forgotten. Standing requests are how a property *systematically* delivers the "they remembered" feeling that defines luxury service, without depending on any individual staff member's memory.

**Revenue / cost angle.** One entry replaces N calls (cost). Persistent preferences are the raw material of the personalization that drives loyalty and direct rebooking (revenue) — and for long stays and resort family groups, the difference between adequate and remarkable.

### 3.9 Transport & mobility requests
*Precedent: INTELITY (valet & wake-up services from the device); Duve (transfers); RMS / Operto (guest services).*

**What it is.** The property's cars, shuttles, valets, and porters as one self-service surface — extending the basic transport request of §3.3 into full mobility: airport transfers, shuttle times, valet car retrieval, luggage handling, and a ride-hailing handoff where the property doesn't drive.

**How it works in Atrium.** The guest books an airport transfer with flight, time, party size, and luggage; once a driver is assigned, the app shows driver and vehicle details and live pickup tracking. The shuttle timetable lists scheduled runs with the shuttle's live location on the route. Valet retrieval is one tap — "bring my car" — timed so the car is at the door when the guest reaches it. Porter/luggage requests cover pickup, a storage tag, and departure-day retrieval. For destinations the property doesn't serve, a ride-hailing handoff deep-links to Pathao or Uber (the BD market's rails) with the property address pre-filled as the pickup point. Paid transfers are sold as add-ons via [G6](guest-mobile-app-upsells-offers-feature.md) and charge to the folio via [G9](guest-mobile-app-payments-folio-tipping-feature.md); every fulfillment task routes to staff F5 like any other G4 request.

**Why it matters.** Arrival and departure are the stay's highest-anxiety moments — a late airport car has flight-missing consequences, and the "where is my car / where is my bag" limbo at checkout is the guest's last memory of the property. Today the cars-and-luggage operation runs on a phone-and-radio chain (guest → desk → bell desk → driver) with no visibility for anyone. A tracked transfer with a named driver and a valet retrieval timed to the guest's walk to the door turn the property's most fragile logistics into its most visible competence.

**Revenue / cost angle.** Transfer and valet fees are directly billable — demand a busy phone line silently turns away — and smoother arrivals and departures protect the first- and last-impression moments that anchor review scores (revenue). On the cost side, it kills the phone-and-radio coordination chain around cars and luggage: no relayed messages, no desk agent chasing a driver, no bell-desk guesswork on departure morning.

### 3.10 Guest safety & emergency assistance
*Precedent: Atrium extension — the guest-facing mirror of Atrium Staff's F16 lone-worker SOS; guest-side emergency assistance is thin-to-absent in the vendor set.*

**What it is.** A distinct, always-reachable emergency layer in the Services surface: a one-tap emergency assistance button, a medical-assistance request, an evacuation & emergency information page, the option to request female staff attendance, and emergency contact numbers.

**How it works in Atrium.** The emergency button is visually distinct and always reachable in the Services surface — never buried in a catalog. Firing it, or submitting a medical-assistance request, bypasses the normal request SLA entirely: the alert lands at critical priority with the duty manager and security, mirroring the receive side of [Atrium Staff's F16 SOS infrastructure](../../staff/docs/staff-mobile-app-lost-found-staff-safety-feature.md) — one alerting pipeline, two sources (staff SOS + guest emergency), the same acknowledge-and-escalate chain. The evacuation & emergency information page is served from [G10 guidebook](guest-mobile-app-digital-guidebook-property-info-feature.md) content, so it stays current and works offline. The female-staff-attendance option lets a guest ask that any in-room service visit be carried out by female staff — a women-traveler safety measure that is meaningful in the BD market. Emergency contact numbers (property security, duty manager, national services) are one tap from the same page.

**Why it matters.** Guest trust and safety are booking criteria — especially for solo and women travelers, who actively screen properties on them. A guest in trouble at 2am will not navigate a request catalog; they need one obvious button that summons help and proof someone is responding. And the property that can show a defined, always-acknowledged emergency path is discharging a duty of care, not just offering a feature.

**Revenue / cost angle.** This is not a revenue feature and the document will not pretend otherwise. Its value is trust that wins bookings from safety-conscious segments (revenue protection), reduced liability exposure from a logged, acknowledged emergency path, and — decisively — near-zero marginal build cost: the staff F16 alerting pipeline (critical push, acknowledge, escalation timeout) already exists; G4 adds a second alert source, not a second system.

---

## 4. Why This Matters

### 4.1 Requests are where in-stay satisfaction is won or lost
Reviews rarely praise the lobby; they praise (or savage) whether the extra blanket arrived and whether the AC got fixed. The request loop *is* the in-stay experience for most guests. A closed loop — ask, see progress, confirm done — converts the property's operational competence into visible, review-shaping service.

### 4.2 Guests increasingly will not phone
The behavioral shift is well documented across vendors: guests message and tap; they don't call. Every request that requires a phone call is a request a growing share of guests simply won't make — they'll do without, and quietly rate the stay lower. Structured requests recover demand for service that phone-only properties never see. And with the [app-less path (G15)](guest-mobile-app-app-less-access-feature.md) — a QR card at the bedside — this works for every guest on property, not just app installers.

### 4.3 It is the staff app's best data source
Every guest-typed request arrives in staff F5 already structured: department, room, type, priority, photo. That is dispatch-quality data with zero transcription labor — better than what a busy desk agent produces by hand. G4 doesn't add load to staff operations; it *upgrades the input quality* of the load they already carry.

### 4.4 A visible opening in the Bangladesh market
Almost no Bangladeshi vendor offers a genuine guest-facing app (research Part A, BD note); phone-and-desk is the universal default. A Bangla/English structured request system with a guest-visible SLA timer — backed by local-market staff-side maturity — is a differentiator no local competitor currently answers.

---

## 5. Journey Moments & Operations It Improves

| Journey moment / operation today | Pain | With In-Stay Service Requests |
|----------------------------------|------|-------------------------------|
| **Guest needs extra towels** | Call the desk, hold, hope it was written down; call again in 40 minutes | Tap-tap-submit; SLA timer visible; runner dispatched with exact items and room. |
| **AC is noisy at 11pm** | Guest won't call that late; suffers, mentions it in the review | Photo report filed in 30 seconds; work order in engineering's morning queue — or night escalation if urgent. |
| **Wake-up call before a flight** | Verbal promise to a night agent; occasionally missed, catastrophically | Scheduled to the minute, executed by system call + push, staff fallback for VIPs. |
| **Housekeeping knocks mid-nap** | Door hanger unseen; guest interrupted; attendant walks back later | Guest-set window and DND sync live to the attendant board — no knock, no walk-back. |
| **Guest doesn't want daily cleaning** | No way to say so except the hanger; room cleaned anyway | One-tap green opt-out; room off tomorrow's list before staffing is set; perk credited. |
| **Item left behind after checkout** | Multi-day phone chase across departments | Web-link claim with reference number; match flagged; courier return arranged in-app. |
| **"Is anyone coming?"** | Silence after every request; guest calls again; duplicate tasks | Live status + countdown; completion confirmed with one tap. |
| **Same request every day of a 7-night stay** | Guest re-phones daily or gives up | Standing request entered once; daily task instances auto-generated. |
| **Front desk at peak** | Phone ringing over the check-in queue; requests scribbled between arrivals | Request traffic moves off the phone entirely; desk serves the guests in front of it. |
| **Duty manager wants accountability** | "Who took that call? Where did it go?" — no record | Every request logged, owned, timed; SLA breaches escalate automatically. |

---

## 6. How It Increases Revenue

1. **Review-score protection compounds into rate power.** Request handling is the most review-cited in-stay factor. Closed-loop requests with visible SLAs raise "service" sub-scores, and higher review scores demonstrably support higher ADR and more direct bookings (less OTA commission). This is the feature's largest — if indirect — revenue line.
2. **Billable concierge services captured.** Transport bookings, courier returns, premium amenities (celebration setups requested via concierge) post straight to the folio ([G9](guest-mobile-app-payments-folio-tipping-feature.md)) — demand that a busy phone line silently turns away.
3. **Issues fixed in-stay are refunds not given.** Early defect reporting converts would-be comps, discounts, and chargebacks into cheap fixes — measurable margin protection via the [G14 service-recovery](guest-mobile-app-feedback-service-recovery-feature.md) funnel.
4. **Green perks recycle as on-property spend.** Opt-out perks paid as F&B credit return to the property's own outlets at retail value while costing wholesale.
5. **Preference data powers the upsell engine.** Standing requests and stated preferences feed the guest profile (G13) that targets [G6 offers](guest-mobile-app-upsells-offers-feature.md) — the app's #1 revenue feature depends on data G4 collects for free.
6. **More sellable room-nights.** Faster defect-to-fix cycles mean fewer rooms stuck out-of-order — recovered inventory on high-occupancy nights.

> **Illustrative model:** A 150-room resort at 75% occupancy (~112 occupied rooms/night) sees ~45 guest requests/day. If in-app requests lift the service review sub-score enough to support even a 1% ADR gain on a ৳9,000 ($75) ADR, that is ~৳90/room-night × 112 × 365 ≈ **৳3.7M (~$31,000)/year** — before counting billable transport (say 4/day at ৳500 margin ≈ ৳730K/year), avoided comps (10/month at ৳3,000 ≈ ৳360K/year), and green-opt-out savings counted in §7. *(Figures illustrative; validate against property data.)*

---

## 7. How It Reduces Operational Cost

1. **Call deflection at the desk and switchboard.** If 60–70% of ~45 daily requests move from phone to app, the desk sheds 30+ calls/day — each a 2–4 minute interruption plus relay labor. That is roughly one to two staff-hours per day recovered at the front desk alone.
2. **Zero transcription and relay labor.** Requests arrive in staff F5 pre-structured — no agent writes, re-reads, radios, or re-explains anything. Mishears and lost notes (and their re-do costs) disappear.
3. **Green opt-out saves whole room cleans.** At a 15% opt-out rate on 112 occupied rooms, ~17 cleans/day are removed — attendant time (~25 min each ≈ 7 attendant-hours/day), linen, laundry, and chemicals — against a perk cost that is a fraction of the clean. *(Illustrative.)*
4. **No duplicate work from silent requests.** The status timer eliminates the guest's second and third calls — and the duplicate tasks they spawn.
5. **Efficient housekeeping routing.** Guest-set windows and DND remove skipped-room walk-backs and let F3 sequence floors optimally.
6. **Cheaper defect handling.** Photo-first reports let engineering arrive with the right part the first time; early reports mean smaller repairs.
7. **Management by data, not anecdote.** SLA and volume analytics per request type, department, and hour feed staffing decisions in staff F11 — replacing over-staffing "just in case."

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Channel shift | Share of guest requests arriving via app/QR vs. phone | ≥ 60% by month 6 |
| Responsiveness | Requests completed within SLA | ≥ 90% |
| Silence eliminated | Duplicate/follow-up contacts per request | ↓ 70% vs. baseline |
| Call deflection | Front-desk request calls per occupied room | ↓ 50% |
| Sustainability savings | Green opt-out rate (eligible nights) | ≥ 12% |
| Defect capture | Guest-reported issues per 100 occupied rooms | ↑ (more reported = more caught) |
| Guest experience | "Service" review sub-score | ↑ vs. baseline |
| Loop closure | Requests confirmed resolved by guest | ≥ 85% |
| Safety readiness | Emergency-alert acknowledge time | ≤ 60 seconds *(illustrative)* |
| Transport capture | Airport-transfer attach rate on arriving stays | ≥ 15% *(illustrative)* |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| Staff-side adoption | G4 is only as good as F5 execution — an ignored task queue breaks the guest promise | Deploy F5 first; SLA escalation to supervisors; launch request types gradually |
| SLA credibility | A guest-visible timer that routinely breaches is worse than no timer | Conservative property-set SLAs; auto-escalation before breach; per-type tuning |
| Request catalog quality | Vague or missing catalog items push guests back to the phone | Curated default catalog at onboarding; "something else" free-text fallback into F5 |
| Photo upload on weak Wi-Fi | Resort dead zones stall submissions | Background upload with retry; request submits first, photo follows |
| App-less reach | Most short-stay guests won't install the app | Every flow served via bedside QR / web link (G15) with room context embedded |
| Abuse / noise | Frivolous or duplicate requests inflating staff load | Rate limiting, duplicate detection, front-desk override |
| Green perk economics | Perk cost must stay below clean cost | Property-configurable perks; caps on consecutive opt-outs |
| Language | Bangla free-text must route correctly | Bilingual UI; structured categories carry routing; free text is supplementary |
| Emergency-routing reliability | A missed or unacknowledged emergency alert is the worst possible failure; false alarms erode responder trust | Reuse the proven staff F16 pipeline (critical push, acknowledge, escalation timeout); 24/7 acknowledgment protocol staffed at the property; confirm-to-send and cancel-with-reason against false alarms |
| Ride-hailing deep-link availability | Pathao/Uber link formats and app coverage vary by device and market | Graceful fallback to a phone-numbers card; link formats verified per market at onboarding; property transport offered first |

---

## 10. Scope delivered

Atrium Guest ships as one complete package. This feature delivers:

- Amenity request catalog with quantities and delivery windows
- Photo-attached issue reporting → staff F4 work orders
- Concierge requests: wake-up calls, transport, luggage
- Housekeeping window scheduling, service-now, and in-app DND → staff F3 sync
- Green opt-out of daily cleaning with configurable perks
- Lost-item reporting and claim tracking → staff F16
- Live per-request status with guest-visible SLA countdown and completion confirmation
- Standing/recurring requests (settable from pre-arrival)
- Transport & mobility requests: airport transfer with driver details and pickup tracking, shuttle timetable with live location, valet car retrieval, porter/luggage help, ride-hailing handoff (Pathao/Uber) with the property address pre-filled
- Guest safety & emergency assistance: one-tap emergency button, medical-assistance request, evacuation & emergency information (G10 content), female-staff-attendance option, emergency contact numbers — critical-priority routing on the staff F16 alerting pipeline
- Auto-routing of every request into staff F5 Task Management with department, room, and priority pre-filled
- Full app-less (QR / web-link) access to every request flow (G15)
- Bangla/English bilingual UI

---

## 11. Open Questions

- Which request types carry guest-visible SLAs at launch, and who owns the default SLA table — Atrium or the property?
- Should issue reports auto-classify severity (night escalation) or always route through a human dispatcher first?
- Green perk defaults for the BD market: loyalty points, F&B credit, or a charitable option — and at what value?
- Does the "not resolved" tap escalate straight to duty manager (G14 flow) or re-queue to the same department first?
- How long after checkout does the lost-item web flow remain active per guest?
- Do standing requests carry across stays automatically via the G13 profile, or require re-confirmation each stay?
- Who operates the transfer fleet — property-owned vehicles, a contracted third party, or both — and how does that split pricing, availability, and liability?
- What protocol and legal review does the medical-assistance flow require — what the property promises versus dispatches (first aid, doctor-on-call, ambulance), and how is that stated to the guest?

---

## 12. One-Line Business Case

> **In-Stay Service Requests turns every guest need into a tracked, deadlined staff task — the guest sees the promise being kept, the desk stops answering phones, and the property converts its operational discipline into the review scores that set its rates.**
