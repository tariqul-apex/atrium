# Mobile Check-In & Check-Out — Feature Deep-Dive

**Product:** Atrium Guest (Hotel / Resort Guest-Facing Mobile App)
**Feature area:** G2 — Mobile Check-In & Check-Out
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-16
**Status:** Draft v1.0
**Related:** [Guest Mobile App — Feature Scope](guest-mobile-app-feature-scope.md) · [Competitive Feature Research](../../HOTEL-~1.MD)

---

## 1. Executive Summary

**Mobile Check-In & Check-Out** makes the front desk optional. The guest completes arrival — identity verification, e-signature, deposit or balance payment, room assignment — from their own device, gets a push the moment the room is ready, and at departure reviews the folio, settles it, and leaves with the receipt in their inbox. A QR-code path serves the guest who never installed the app, and a kiosk mode turns a lobby tablet into a self-service alternative.

The queue is the single most-cited arrival friction in hospitality, and the morning check-out crush wastes the guest's last hour — the one that colors the review. Self-service check-in/out removes both, cuts front-desk labor per arrival, and frees agents for the guests who actually want a human. This is the most widely shipped guest-app capability in the industry — Oracle OPERA Cloud, Mews, Canary, Duve, Stayntouch, IDS Next, Shiji Digital Stay, Infor HMS, eZee, Guestline GuestStay, protel, WebRezPro, RMS Cloud, and Operto all offer contactless check-in — yet almost no Bangladeshi vendor ships it at all.

G2 is the guest-side mirror of [Atrium Staff F1 — Mobile Front Desk](../../staff/docs/staff-mobile-app-front-desk-feature.md): every self-service check-in and check-out lands on the same arrivals/departures board staff work from, and the agent can pick up any guest's flow mid-stream. It consumes what [G1 — Booking & Pre-Arrival](guest-mobile-app-booking-pre-arrival-feature.md) collected, and it triggers what [G3 — Digital Room Key](guest-mobile-app-digital-room-key-feature.md) issues.

This document defines the feature's scope, quantifies its business case, and maps it to the journey moments and operations it improves.

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What the guest can do from their phone |
|---|------------|----------------------------------------|
| 1 | **Contactless check-in from own device** | Complete the entire arrival — verify, sign, pay, get the room — without visiting the desk. |
| 2 | **QR-code instant check-in** | Scan a QR at the entrance or from the booking email to jump straight into the check-in flow — no app install, no lookup. |
| 3 | **ID document scan & fraud checks** | Scan the NID/passport (or confirm the G1 pre-upload), with automated match against the booking name and basic fraud signals. |
| 4 | **E-signature** | Sign the registration card and property terms on the phone screen; the signed card files against the reservation. |
| 5 | **Deposit / balance capture** | Pay any outstanding deposit, pre-authorization, or arrival balance in-flow — bKash, Nagad, SSLCommerz, or card. |
| 6 | **Room assignment + room-ready notification** | Receive the room number at check-in and a push the moment housekeeping marks it ready — no hovering in the lobby. |
| 7 | **Express mobile check-out with folio review** | Review the itemized folio, query a line, settle the balance, and check out — receipt emailed instantly. |
| 8 | **App-less QR path (G15)** | Every step above served as a web flow from a link or QR — the booking email, a lobby stand, a bedside card. |
| 9 | **Kiosk-mode option** | The same flow in a locked-down tablet mode — a lobby self-service station for guests without a usable device. |

### 2.2 In scope (this release)

Self-service check-in (identity, signature, payment, assignment), room-ready notifications, express check-out with folio review and settlement, dispute-a-charge flagging to staff, QR/web app-less delivery of every step, kiosk mode, late-check-out request/purchase hand-off (with G6), and full Bangla/English localization.

### 2.3 Out of scope (this release)

Staff-side check-in tooling (that is [F1 — Mobile Front Desk](../../staff/docs/staff-mobile-app-front-desk-feature.md)), physical key encoding (F1; digital keys are [G3](guest-mobile-app-digital-room-key-feature.md)), booking creation and pre-arrival registration (G1), biometric identity verification beyond document checks (future), and automated upgrade pricing logic (offers surface here but are governed by G6).

### 2.4 Key assumptions

- The PMS exposes real-time reservation, room-status, and folio APIs (or the middleware sync layer bridges it) — check-in must *write*, not just read.
- A payment path exists for in-flow capture: hosted payment pages for cards plus bKash/Nagad/SSLCommerz for the BD market, keeping card data off the device and PCI scope minimal.
- Housekeeping room-status updates flow in real time (staff F3) so room-ready pushes are trustworthy.
- Local regulation permits remote identity verification for check-in, or the property accepts a "verify remotely, glance-check on first key collection" hybrid.
- G3 lock integration exists for the fully desk-free journey; without it, mobile check-in ends at a fast-lane key pickup.

---

## 3. Capability Deep-Dive — The Nine Check-In & Check-Out Features

The competitive research (Part A §2, *Mobile Check-In / Check-Out*) identifies the capabilities that define best-in-class self-service arrival and departure. Each is elaborated below: **what it is**, **how it works in Atrium**, **why it matters**, and the **revenue/cost angle**. Vendor precedents are noted so we know the bar to clear.

### 3.1 Contactless check-in from the guest's own device
*Precedent: Oracle OPERA Cloud, Mews, Canary, Duve, Stayntouch, IDS Next, Shiji Digital Stay, Infor HMS, eZee, Guestline GuestStay, protel, WebRezPro, RMS Cloud, Operto.*

**What it is.** The complete arrival transaction — verify, sign, pay, receive the room — executed by the guest, on the guest's phone, anywhere: in the airport taxi, on the shuttle, at home the night before.

**How it works in Atrium.** From the check-in window (property-configured, e.g. from 6:00 on arrival day), the guest opens "Check in now" — pushed via G8 and linked in the booking email. The flow consumes everything G1 already collected: registration pre-filled, documents pre-uploaded, preferences known. The guest confirms identity (§3.3), signs (§3.4), pays any balance (§3.5), and receives their assignment (§3.6). The completed check-in posts to the PMS and flips the guest to "checked in — awaiting room" or "checked in — room ready" on the staff F1 board. At any point the guest can tap "help" and a front-desk agent picks up the same record mid-flow.

**Why it matters.** This is the feature the queue-weary guest actually wants — the industry's most universally shipped guest capability, and the reason lobby redesigns are removing desks. It converts arrival from a supervised transaction into a self-service one, with staff repositioned as greeters and exception-handlers.

**Revenue / cost angle.** Every self-served arrival is desk labor avoided — minutes per guest across thousands of arrivals (cost). The flow carries the G6 upgrade offer at the highest-intent moment, converting without an agent having to pitch (revenue).

### 3.2 QR-code instant check-in
*Precedent: IDS Next FX GeM.*

**What it is.** A QR code — at the porte-cochère, on the lobby stand, in the booking email — that drops the guest directly into *their* check-in, no app, no reservation lookup, no typing.

**How it works in Atrium.** Generic property QRs open a web flow (G15) that finds the booking by confirmation number + name or phone OTP; personalized QRs in the booking email deep-link straight into the guest's own flow, pre-authenticated. A coach party of forty scans the same lobby stand and processes in parallel — forty concurrent check-ins instead of one desk line.

**Why it matters.** In the BD market this is not a nice-to-have — it is the adoption strategy. Most guests will not install an app for a two-night stay; the QR path takes self-service check-in from the loyal few to effectively every literate smartphone holder in the lobby. It is also the property's zero-hardware surge capacity.

**Revenue / cost angle.** Adoption is the multiplier on every other number in this document — the QR path is what moves self-service from 10% of arrivals to a majority (cost leverage); group and event arrivals self-process without surge staffing (cost).

### 3.3 ID document scan & fraud checks
*Precedent: Hotelogix, Canary, IDS Next, eZee.*

**What it is.** Capturing and verifying the guest's identity document in the check-in flow, with automated checks that flag mismatches and suspicious patterns before a key is ever issued.

**How it works in Atrium.** If the guest pre-uploaded in G1, the flow shows the document for confirmation; otherwise guided capture runs here. Automated checks match the document name to the booking, validate document format and expiry, and flag anomalies — name mismatch, expired document, a card already declined on this profile — for staff review rather than silent acceptance. Flagged check-ins pause as "needs desk verification" on the F1 board; clean ones proceed. Documents are encrypted, role-gated, and retained per property policy.

**Why it matters.** Removing the human from check-in must not remove the checks. Chargebacks, walk-outs, and identity fraud concentrate exactly where verification is weakest — Canary built a business on this problem. Automated verification lets the property go contactless without going blind, and satisfies the legal identity obligations BD properties carry.

**Revenue / cost angle.** Fraud and chargeback losses drop precisely where self-service would otherwise raise them (revenue protected); staff verify only the flagged minority instead of every arrival (cost).

### 3.4 Electronic signature capture
*Precedent: WebRezPro, Smart Software (BD).*

**What it is.** The guest signs the registration card and property terms on their own screen; the signed record files digitally against the reservation.

**How it works in Atrium.** The flow renders the registration card — pre-filled from G1 — plus the property's terms (damage policy, smoking policy, house rules, in Bangla or English), and captures a finger-drawn signature. The signed PDF attaches to the reservation, visible to staff and exportable for audit. Versioned terms mean the property can prove exactly what the guest agreed to.

**Why it matters.** The signature is the legal spine of the registration process — without it, self-service check-in produces an incomplete record. Digital capture also ends the paper card: no printing, filing cabinets, or lost-card disputes.

**Revenue / cost angle.** Signed terms make damage and policy charges enforceable, cutting write-offs and disputes (revenue protected); eliminates printing, filing, and retrieval labor for thousands of paper cards a year (cost).

### 3.5 Deposit / balance capture in-flow
*Precedent: Mews, RoomRaccoon (integrated contactless payment at online check-in/out); Smart Software, Mediasoft (local rails).*

**What it is.** Taking whatever money the arrival requires — remaining deposit, pre-authorization, city ledger balance, arrival charges — inside the check-in flow, on the methods the guest holds.

**How it works in Atrium.** The flow shows exactly what is due and why, and captures it via hosted card payment, bKash, Nagad, or SSLCommerz. Successful payment posts straight to the folio (G9); failure routes the guest to alternate methods or flags "payment at desk" on the F1 board so staff know before the guest reaches them. Pre-authorizations are tokenized — no card data touches the device.

**Why it matters.** Payment is the step that most often blocks a fully remote check-in; if the guest still has to visit the desk to pay, the queue survives. In-flow local rails are the BD-market unlock: the domestic guest without an international credit card can still complete a contactless arrival.

**Revenue / cost angle.** Balances collected before the guest has the key means near-zero uncollected arrivals (revenue protected); payment self-service removes the POS step from every desk-free arrival (cost). Local gateways remain a decisive edge global vendors don't serve.

### 3.6 Room assignment + room-ready notification
*Precedent: Shiji (automated segmented "your room is ready" alerts); Infor HMS, Stayntouch (assignment in the self-service flow).*

**What it is.** The guest learns their room number at check-in — and if the room is still being turned, gets a push the moment housekeeping marks it ready, instead of hovering at the desk.

**How it works in Atrium.** Assignment runs against G1 preferences (floor, bed type, accessibility) and live room status from staff housekeeping (F3). If the room is ready, the guest proceeds straight to key issuance (G3). If not, the app shows an honest state — "Room 412 is being prepared, we'll notify you" — with an estimated ready time, and suggests the pool, café, or spa (a G6/G11 moment) in the meantime. The instant housekeeping flips the room to inspected-ready, the guest gets the push and the key issues automatically.

**Why it matters.** "Your room isn't ready" is the worst sentence at a fixed desk because the guest has nowhere to go but the lobby sofa, watching the desk. With a notification promise, the same delay becomes free time — and the property's F1/F3 room-turn sequencing gets a real-time customer for its output.

**Revenue / cost angle.** Waiting guests redirected to outlets spend instead of stewing (revenue); the desk stops re-answering "is it ready yet?" every five minutes, and early check-in becomes precisely deliverable — and sellable (cost, revenue).

### 3.7 Express mobile check-out with folio review
*Precedent: Maestro, Canary, Mews, Shiji, Infor HMS.*

**What it is.** Departure without the desk: the guest reviews the itemized folio on their phone, settles any balance, checks out, and gets the receipt by email.

**How it works in Atrium.** On departure morning (or the night before) the guest gets a "ready to check out?" push. The folio (live throughout the stay via G9) shows every charge itemized; the guest can flag a disputed line — which routes to the F1 desk queue *before* it becomes a public-review complaint — settle by card or local rails, and confirm departure. Check-out releases the room to housekeeping on the staff side instantly, revokes the digital key (G3), and emails the receipt. A late-check-out offer (G6) surfaces before final confirmation.

**Why it matters.** The 8–10 a.m. check-out crush is the mirror image of the arrival queue, hitting at the exact moment guests are racing for flights and breakfast — the last impression before the review gets written. Express check-out deletes it, and folio-review-before-settlement is the single best defense against the bill-surprise disputes that poison satisfaction scores.

**Revenue / cost angle.** Instant room release accelerates the housekeeping turn sequence, enabling more same-day early check-ins — usable inventory on high-occupancy nights (revenue); the morning desk crush no longer sets peak staffing (cost). The late-check-out upsell converts at the moment the guest is deciding when to leave (revenue).

### 3.8 App-less QR path (G15)
*Precedent: IDS Next FX GeM (QR check-in); Oracle, Canary, Duve — "no app download required" online check-in.*

**What it is.** Every G2 capability — check-in, room-ready alert, folio review, check-out — delivered as a web flow from a link or QR code, with zero install.

**How it works in Atrium.** The booking-confirmation email carries a personalized check-in link; property QRs (lobby stand, key packet, bedside card) open the same flows with confirmation-number + OTP authentication. Room-ready and folio-ready notifications fall back from push to SMS/email for link-path guests. Feature parity is a design rule: the web path is the primary delivery channel, the native app the upgrade path for resort stays and loyalty members.

**Why it matters.** Most guests will not install an app for a short stay — this is the difference between self-service reaching 15% of arrivals and reaching most of them. For the BD market, where storage-constrained devices and data costs make installs even rarer, the app-less path *is* the product for the majority of guests.

**Revenue / cost angle.** Adoption drives every revenue and cost line in this document; the app-less path is the adoption engine (leverage on everything). It also serves the walk-in and OTA guest the app could never reach.

### 3.9 Kiosk-mode option
*Precedent: Mews (tablet as digital greeter).*

**What it is.** The same self-service flow running on a locked-down property tablet in the lobby — a check-in station for guests with no usable device, a dead battery, or no data.

**How it works in Atrium.** Kiosk mode is the G2 web flow in a hardened shell: single-app lockdown, session wipe between guests, an ID-scan camera, and an optional receipt printer. It reuses the identical flow logic — the kiosk is a delivery surface, not a second product. Staff see kiosk check-ins on the F1 board exactly like mobile ones.

**Why it matters.** Self-service must not have a coverage hole: the guest whose phone died at the airport still deserves the fast lane. A ৳40,000 tablet on a stand is also the cheapest "extra desk" a property will ever buy for peak periods — and a visible signal in the lobby that this property runs differently.

**Revenue / cost angle.** A kiosk absorbs peak arrivals at a fraction of a staffed desk position's cost (cost); it converts the no-device minority who would otherwise re-form the queue the feature exists to remove (cost, satisfaction).

---

## 4. Why This Matters

### 4.1 The queue is the arrival experience's single point of failure
Everything a property spends on rooms, design, and service is filtered through the first ten minutes — and the first ten minutes are, at most properties, a line. Self-service check-in removes the queue *structurally*: throughput scales with guests' own devices, not desk positions. The desk survives for those who want it; it stops being mandatory for those who don't.

### 4.2 Departure is the last impression — and it is currently the worst one
The check-out crush hits at peak stress (flights, breakfast, transport) and is the freshest memory when the review invitation (G14) arrives. Express check-out converts the stay's worst-designed moment into a non-event, and folio-review-in-app catches billing disputes while staff can still fix them quietly.

### 4.3 It completes the loop with staff F1 — one arrival pipeline, two surfaces
Self-service and desk service are the same pipeline: a guest can start on the phone and finish at the desk, or vice versa, with the [F1 Mobile Front Desk](../../staff/docs/staff-mobile-app-front-desk-feature.md) agent seeing exactly where each arrival stands. Staff effort concentrates on exceptions, VIPs, and guests who want the human touch — which is what humans are for.

### 4.4 It is a table-stake globally and wide open in BD
Fifteen-plus global vendors ship contactless check-in; it is the most standard guest-app feature in the industry. Almost no Bangladeshi vendor offers it — and none with a QR-first app-less path, Bangla UX, and bKash/Nagad settlement. Atrium can define the local category.

---

## 5. Journey Moments & Operations It Improves

| Moment / operation today | Pain | With Mobile Check-In & Check-Out |
|--------------------------|------|----------------------------------|
| **Arrival after a long journey** | Guest queues 10–30 minutes at the exact moment patience is lowest | Checked in from the taxi; walks past the desk (or straight to key pickup where G3 isn't fitted). |
| **Peak / event arrivals** | A coach of 40 collapses the desk for an hour | Lobby QR processes forty concurrent self-check-ins; staff greet instead of type. |
| **Room not ready at arrival** | Guest camps in the lobby, re-asking the desk every 10 minutes | Honest status + push on ready; guest redirected to café/pool — a spend moment, not a grievance. |
| **Identity paperwork** | Passport photocopying and manual transcription at the desk | Scanned or pre-uploaded, auto-matched, flagged only on anomaly. |
| **Payment at arrival** | Card terminal queue; domestic guests without international cards stuck | In-flow bKash/Nagad/SSLCommerz/card; "payment at desk" flagged to staff only on failure. |
| **Morning check-out crush** | 8–10 a.m. line; guests miss breakfast and transport; agents rushed into billing errors | Folio reviewed and settled from the room; check-out is one tap and the receipt is in the inbox. |
| **Bill disputes at the desk** | Surprise charges argued publicly at the counter, refunded under pressure | Line-item flagging in-app routes disputes to staff quietly, before settlement. |
| **Room release to housekeeping** | Housekeeping learns of departures late; turns start late | Check-out releases the room instantly; turn sequence starts on time (staff F3). |
| **Guest without the app** | Excluded from self-service entirely | QR/web path (G15) serves every flow; kiosk covers the no-device case. |
| **Late-night arrivals** | One night agent, one desk, one line | Self-service absorbs routine arrivals; the agent handles exceptions. |

---

## 6. How It Increases Revenue

1. **Upsell at the highest-intent moments.** The check-in flow carries the G6 upgrade offer when the guest is committed and the room not yet fixed; the check-out flow carries late check-out when the guest is deciding when to leave. Self-service makes the pitch on *every* arrival and departure — a fixed desk pitches on few. Vendor upsell platforms (Canary, Duve) report large uplifts from exactly these placements.
2. **Faster room turns → more sellable inventory.** Instant check-out release plus room-ready sequencing enables more same-day early check-ins — effectively extra inventory on full nights, and early check-in itself becomes a reliably deliverable paid add-on.
3. **Redirected waiting spend.** "We'll notify you when it's ready" sends waiting guests to outlets instead of the lobby sofa — F&B (G5) capture during the ready-wait window.
4. **Dispute and fraud losses cut.** Folio review before settlement prevents the classic bill-surprise refund; document verification and fraud flags cut chargebacks and walk-outs precisely where contactless would otherwise invite them.
5. **Review-score lift → rate power and direct bookings.** Arrival and departure are the two moments reviews cite most; fixing both lifts scores, which support ADR and feed the direct-booking loop (G1/G13).
6. **Balance collection before key issuance.** No key until the folio path is secured — uncollected arrivals approach zero.

> **Illustrative model:** A 150-room hotel at 75% occupancy sees ~110 arrivals and ~110 departures/day. If self-service reaches 50% adoption via the QR path, the check-in upgrade offer converts 6% of those ~55 self-served arrivals at ৳2,500 average — ~৳8,200/day ≈ **৳3.0 million/year (~$25,000)** — and late check-out converts 8% of 55 departures at ৳1,500 — ~৳6,600/day ≈ **৳2.4 million/year (~$20,000)**. Combined ≈ **৳5.4 million (~$45,000)/year** before counting early-check-in inventory gains, redirected waiting spend, and dispute reduction. *(Figures illustrative; validate against property data.)*

---

## 7. How It Reduces Operational Cost

1. **Desk labor per arrival collapses.** A self-served check-in consumes near-zero agent minutes; even 50% adoption removes half the desk's transactional load — peak staffing stops being set by the queue.
2. **Morning crush de-staffed.** Express check-out flattens the 8–10 a.m. spike that currently dictates the morning roster.
3. **Group arrivals without surge staffing.** Coach parties and event blocks self-process in parallel off a QR stand — no borrowed staff, no overflow desks.
4. **Paper eliminated.** Digital registration cards and e-signatures end printing, filing, and retrieval for every arrival.
5. **Billing-error rework cut.** Guests reviewing their own folio catch errors before settlement — fewer refund transactions, adjustment postings, and chargeback administration.
6. **"Is my room ready?" traffic deleted.** The push notification answers the lobby's most repetitive question automatically; the same applies to "can I get my bill?"
7. **Kiosk as the cheapest desk position.** One hardened tablet absorbs peak overflow at a small fraction of a staffed position's annual cost.
8. **Exception-only verification.** Staff review flagged identities and failed payments rather than processing every clean arrival.

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Self-service adoption | Arrivals completed via mobile/QR/kiosk check-in | ≥ 50% by month 6 |
| App-less reach | Share of self-service check-ins via QR/web (no app) | ≥ 60% of self-served |
| Queue removal | Avg. peak-hour lobby wait | < 3 minutes |
| Departure relief | Departures via express mobile check-out | ≥ 40% |
| Revenue capture | Upgrade attach on self-served check-ins | ≥ 6% |
| Revenue capture | Late-check-out attach on self-served check-outs | ≥ 8% |
| Billing quality | Folio disputes raised post-departure | ↓ 50% vs. baseline |
| Fraud control | Chargeback rate on self-served arrivals | ≤ desk baseline |
| Inventory gain | Same-day early check-ins enabled | ↑ measurable |
| Labor efficiency | Front-desk labor hours per 100 arrivals | ↓ 25% |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| PMS write depth | Check-in/out state, assignment, and folio settlement must write in real time | Middleware sync layer; degrade to "pre-checked-in, finalize at desk" if writes unavailable |
| Regulatory identity rules | Some jurisdictions require in-person ID verification | Hybrid mode: remote flow + glance-check at key pickup; per-property configuration |
| Payment / PCI | In-flow card capture expands PCI scope | Hosted payment pages and tokenization; local rails (bKash/Nagad) are natively redirect-based |
| Room-status accuracy | Room-ready pushes are only as good as housekeeping updates (F3) | Inspected-ready as the trigger state; staff F1 override; conservative ETAs |
| Lock integration gap | Without G3, mobile check-in still ends at a key line | Dedicated express key-pickup lane; kiosk key dispensing as future option |
| Fraud shift | Contactless arrival attracts identity/card fraud | Document matching, fraud flags, staff-review queue, no key before verification |
| Adoption | Guests default to the desk out of habit | QR at every touchpoint, booking-email link, desk staff redirecting queues, Bangla-first UX |
| Kiosk hardening | Public device: session leakage, tampering | Single-app lockdown, auto session wipe, no stored credentials |
| Notification delivery | Push unavailable on the app-less path | SMS/email fallback for room-ready and folio-ready messages |

---

## 10. Scope delivered

Atrium Guest ships as one complete package. This feature delivers:

- Contactless check-in from the guest's own device (verify → sign → pay → room)
- QR-code instant check-in, generic and personalized
- ID document scan/confirmation with automated match and fraud flags, plus staff review queue
- E-signature on registration card and versioned property terms
- In-flow deposit/balance capture on bKash, Nagad, SSLCommerz, and cards
- Preference-aware room assignment with room-ready push (SMS/email fallback)
- Express mobile check-out: folio review, line-item dispute flagging, settlement, instant receipt
- Late-check-out and upgrade offer placements (content governed by G6)
- Full app-less web/QR delivery of every step (G15)
- Kiosk mode for lobby tablets with session hardening
- Bangla/English throughout
- Real-time hand-off to and from the staff F1 arrivals/departures board

---

## 11. Open Questions

- What do target jurisdictions require for identity verification at check-in — is fully remote verification acceptable, or is a glance-check at key collection required?
- Which fraud-check depth ships at launch: rule-based matching only, or a third-party document-verification service?
- Pre-authorization vs. full capture at check-in — what is the default policy, and per-property configurable how?
- Kiosk hardware reference spec: which tablets, stands, and (optional) passport scanners do we certify for the BD market?
- When the room isn't ready, do we show an ETA (housekeeping-data-dependent) or only "we'll notify you"?
- How does check-in interact with walk-ins — do we allow book-and-check-in in one flow (G1 + G2 fused) at the kiosk?

---

## 12. One-Line Business Case

> **Mobile Check-In & Check-Out makes the desk optional at the two moments guests judge hardest — deleting the arrival queue and the morning crush, capturing the upgrade and late-check-out sale on every self-served guest, and cutting desk labor per arrival, with a QR path that reaches even the guest who never installs an app.**
