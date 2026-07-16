# Atrium Guest — Journey-Stage Design & Visibility

**Product:** Atrium Guest (Hotel / Resort Guest-Facing Mobile App)
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-16
**Status:** Draft v1.0
**Start here:** [Project Overview & Documentation Index](../overview.md)
**Related:** [Navigation & Feature Map](guest-mobile-app-navigation-map.md) · [Feature Prioritization](guest-mobile-app-feature-prioritization.md) · [Feature Scope](guest-mobile-app-feature-scope.md)

The app is **one codebase, stage-adaptive**. Guests have no staff-style roles — this is the guest analog of the staff [Role-Based Design](../../staff/docs/staff-mobile-app-role-based-design.md) doc, with **journey stage** playing the role RBAC plays there. The principle is the same: **least clutter** — a guest sees only what their moment in the stay needs; everything else is not surfaced. This document defines the personas, the six journey stages, the feature×stage matrix, stage-detection rules, per-stage Home composition, the day-visitor and app-less variants, the hard rules, and privacy/PII handling.

> **Note:** stage adaptation is a *presentation* rule, not a permission system. A guest can always reach their own data (folio, receipts, bookings) from Home at any stage; what changes by stage is what the app **leads with**. The exceptions are the hard rules in §7, which are enforced server-side.

---

## 1. Personas

The six personas from the [product canon](../README.md) differ in **stay type** and **feature emphasis**, not in shell — every overnight persona gets the same five tabs; the day visitor gets the reduced shell (§6.1).

| Persona | Who | What the app is mostly for | Emphasis |
|---------|-----|----------------------------|----------|
| **Leisure traveler** | Couple / solo tourist, short stay | Frictionless arrival, F&B, local tips | G2, G3, G5, G10 — most likely **app-less (G15)** |
| **Business traveler** | Tight schedule, repeat city stays | Speed: check-in/out, key, folio, receipt | G2, G3, G9, G13 |
| **Resort family / group organizer** | One booker acting for a party | Activities, itinerary, key sharing, group ordering | G12, G11, G5, G3 (shared keys), G6 |
| **Event & banquet guest** | Wedding / conference attendee, often not the booker | Event schedule, wayfinding, F&B | G11, G12, G5, G10 — often app-less |
| **Day visitor / walk-in local** | Pool day-use, restaurant walk-in | Order, book day-use, pay, tip | G5, G9, G11, G12, G14 — **reduced shell** (§6) |
| **Returning loyalty member** | Repeat direct guest with a profile | Points, perks, personalized offers, rebooking | G13, G6, G1 (rebook) — the **native-app** persona |

**Persona ≠ stage.** A persona colors *emphasis and defaults* (a family organizer's Explore leads with kids' club; a business traveler's Home leads with express check-out); the **stage** decides *what is surfaced at all*. The matrix in §3 is stage-driven; persona tuning happens inside what the stage allows.

---

## 2. The six journey stages

| Stage | When | The app's job |
|-------|------|---------------|
| **Discover & Book** | Before there is a reservation | Convert: browse, book direct, pay deposit |
| **Pre-Arrival** | Booking confirmed → arrival day | Prepare: register, preferences, pre-order, pre-book |
| **Arrival** | Arrival day, until key issued | Land: check in, wait for room, get the key |
| **In-Stay** | Checked in → departure day | Serve & sell: order, request, book, chat, tip, track folio |
| **Departure** | Departure day | Leave well: review folio, express check-out, transport |
| **Post-Stay** | After check-out | Retain: receipt, review invite, points, rebook |

What Home *leads with* at each stage is defined in §5.

Stages are **strictly ordered per reservation** and transition automatically (§4); multi-reservation guests are staged by the **active or nearest** stay, with a switcher on Home.

---

## 3. Feature × journey-stage visibility matrix

**✅ Primary** · **◐ Limited** · **— Not surfaced** *(reproduced from the [README](../README.md) canon, with the elaboration column added)*

| Feature | Discover & Book | Pre-Arrival | Arrival | In-Stay | Departure | Post-Stay | What "limited" means |
|---------|:---:|:---:|:---:|:---:|:---:|:---:|----------------------|
| G1 Booking & Pre-Arrival | ✅ | ✅ | ◐ | — | — | ◐ | Arrival: finish registration only. Post-stay: rebook |
| G2 Check-In & Check-Out | — | ◐ | ✅ | — | ✅ | — | Pre-arrival: start check-in early (docs, signature). Departure: express check-out |
| G3 Digital Room Key | — | — | ✅ | ✅ | ◐ | — | Arrival: issued **only after room ready**. Departure: revoked at check-out |
| G4 In-Stay Service Requests | — | ◐ | ◐ | ✅ | ◐ | — | Pre-arrival: standing requests (crib). Departure: luggage, transport |
| G5 F&B Ordering | — | ◐ | ◐ | ✅ | ◐ | — | Pre-arrival: pre-order. Departure: breakfast/takeaway |
| G6 Upsells, Offers & Add-Ons | ◐ | ✅ | ✅ | ✅ | ◐ | ◐ | Booking: in-flow add-ons. Arrival: upgrade. Departure: late check-out only. Post-stay: opt-in win-back |
| G7 Guest Messaging | ◐ | ✅ | ✅ | ✅ | ✅ | ◐ | Booking: pre-sales questions. Post-stay: follow-up thread, N days |
| G8 Notifications & Campaigns | ◐ | ✅ | ✅ | ✅ | ✅ | ◐ | Booking: confirmation only. Post-stay: capped, opt-out (§7.8) |
| G9 Payments, Folio & Tipping | ◐ | ◐ | ◐ | ✅ | ✅ | ◐ | Booking: deposit. Arrival: balance due. Post-stay: receipt |
| G10 Guidebook & Property Info | ◐ | ✅ | ✅ | ✅ | ◐ | — | Booking: property preview. Departure: transport info foregrounded |
| G11 Interactive Property Map | ◐ | ✅ | ✅ | ✅ | — | — | Booking: preview map as marketing |
| G12 Activities, Spa & Experiences | ◐ | ✅ | ✅ | ✅ | — | — | Booking: browse only. Pre-arrival: pre-book |
| G13 Loyalty & Guest Profile | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | The only feature primary at *every* stage — identity persists |
| G14 Feedback & Service Recovery | — | — | ◐ | ✅ | ✅ | ◐ | Arrival: one arrival pulse. Post-stay: one review invite |
| G15 App-less Access (QR & Web) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | Cross-cutting channel at every stage — never a screen |

**Reading the matrix.** Down a column is *what this stage's app is*: Pre-Arrival is a preparation checklist with pre-booking; In-Stay is the full-service surface; Post-Stay is a receipt, a thank-you, and a reason to return. The stage system exists so the guest never sees the wrong app.

---

## 4. Stage-detection rules

The stage is computed **server-side** from **reservation state + date + check-in state** and pushed to the client (cached for offline); the client never derives stage locally except as a fallback.

| # | Rule (first match wins) | Stage |
|---|--------------------------|-------|
| 1 | No reservation linked to this identity | **Discover & Book** |
| 2 | Reservation `confirmed` **and** today < arrival date | **Pre-Arrival** |
| 3 | Reservation `confirmed` **and** today = arrival date **and** check-in not complete | **Arrival** |
| 4 | Check-in complete **and** today < departure date | **In-Stay** |
| 5 | Check-in complete **and** today = departure date **and** check-out not complete | **Departure** |
| 6 | Check-out complete (or reservation `no-show`/`cancelled` → back to rule 1 after receipt handling) | **Post-Stay** |

**Sub-states and transition triggers:**

- **Arrival splits on `room_ready`** (set by housekeeping in staff F3): before it, Home shows check-in / "we're preparing your room"; after it, the room-ready push fires (G8) and **the key issues (G3)** — gated on `room_ready ∧ check-in complete ∧ payment cleared`, never on stage alone.
- **Early/late edges:** an early check-in purchase (G6) moves rule 3's effective time; late check-out extends rule 5. Walk-in same-day bookings jump from rule 1 to rule 3.
- **Multi-reservation:** stage binds to a *reservation*, not the person; the nearest/active stay drives the shell, with a switcher on Home.
- **Companions** inherit the booker's stay stage with a scoped feature set (§6.3). **Event guests without a room** are staged by *event registration*: event-day = a constrained In-Stay (map, schedule, F&B), no room features.
- **Clock discipline:** date rules evaluate in the **property timezone** against PMS business date, not the phone clock.
- **Failure fallback:** offline, the client keeps the last known stage, disables state-changing CTAs (check-in, payment), and keeps read surfaces (guidebook, itinerary, cached key per lock-vendor policy).

---

## 5. Per-stage Home composition

Home is **not one screen** — it is one *template* (stay card → primary CTA → contextual sections → quick actions) recomposed per stage:

| Stage | Primary CTA | Contextual sections | Quick actions |
|-------|-------------|---------------------|---------------|
| **Discover & Book** | **Book your stay** | Rooms & rates, offers (G6), property preview (G10/G11), sign in / join (G13) | Search dates · Contact |
| **Pre-Arrival** | **Complete pre-arrival** (% complete) | Checklist (register · documents · arrival time · preferences), pre-book (G12), pre-order (G5), offers (G6), getting here (G10) | Chat · Edit booking |
| **Arrival** | **Check in now** → *(after room ready)* **Get your key** | Check-in status, upgrade offer (G6), while-you-wait (G11 map, G5 lobby café), arrival pulse (G14, once) | Chat · Directions |
| **In-Stay** | *(none — launcher mode)* | Quick-action grid (order · request · book · key), live folio (G9), tonight on property (G8/G12), offers (G6), my itinerary (G12) | Order · Request · Tip · Chat |
| **Departure** | **Express check-out** | Folio review (G9), late check-out offer (G6), luggage & transport (G4), breakfast hours (G10) | Pay folio · Transport · Chat |
| **Post-Stay** | **Book your next stay** | Receipt (G9), points + tier progress (G13), review invite (G14, once), win-back offer (G6, capped) | Receipt · Rebook · Chat |

### 5.1 Wireframe (In-Stay Home, resort family)

```
┌───────────────────────────┐
│ Shapla Resort      🌐 বাং 🔔•│
│ Room 412 · 12–16 Jul       │
├───────────────────────────┤
│ ┌─────┐┌─────┐┌─────┐     │
│ │🍽️   ││🛎️   ││📅   │     │
│ │Order││Req- ││Book │     │
│ │food ││uest ││spa  │     │
│ └─────┘└─────┘└─────┘     │
├───────────────────────────┤
│ Your folio        ৳12,450 │
│ Dinner · Pool bar · Spa → │
├───────────────────────────┤
│ Tonight on property        │
│ 🎶 Live band · Beach 8pm   │
│ 🧒 Kids' movie · 7pm  [Book]│
├───────────────────────────┤
│ 🎁 20% off sunset cruise → │
├───────────────────────────┤
│ 🏠  🛎️  🔑  🗺️  💬        │
└───────────────────────────┘
```

### 5.2 Stitch prompt

> Design a mobile **guest home screen** for a resort guest **mid-stay** (a stay companion, NOT a booking-site home). Top bar: property name, language toggle (English/বাংলা), notification bell with red dot; below it a compact stay card "Room 412 · 12–16 Jul". Section 1: three large quick-action tiles — "Order food", "Request", "Book spa". Section 2: a **live folio card** ("Your folio ৳12,450") with the last three line items. Section 3 **Tonight on property**: two event rows, each with a Book button. Section 4: one dismissible **offer banner** ("20% off sunset cruise"). Persistent 5-tab bottom bar: Home (active), Services, a prominent **center Key tab**, Explore, Inbox. Warm, resort-toned, big touch targets; no data tables, no admin feel.

---

## 6. Stay-type variants

### 6.1 Day visitor (reduced shell)

Day visitors (pool day-use, restaurant walk-ins, event attendees without rooms) get a **three-tab shell — Services · Explore · Inbox** — with **no Key tab, no stay card, no folio**:

| Surface | Day-visitor scope |
|---------|-------------------|
| 🛎️ Services | **G5 ordering only** (deliver-to-location by map pin/table) — no G4 room requests |
| 🗺️ Explore | G11 map, G12 **day-use booking** (pool, court, spa), G10 public-area info; G6 day-use offers only |
| 💬 Inbox | G7 order-scoped chat, G8 order/booking status alerts only |
| Payment | **Direct per transaction** (bKash/Nagad/SSLCommerz/card via G9's rails) — no folio, no charge-to-room |
| Feedback | G14 quick rating after the visit |

Entry is almost always **app-less (G15)**: a gate/table QR opens the reduced shell in the browser; identity is a phone number + OTP.

### 6.2 App-less guest (G15 parity)

The app-less web surface is the **same flows, one stage at a time**: each QR/link opens the stage-correct flow directly (email link → check-in; bedside card → guidebook + services; table tent → this outlet's menu; folio link → review & pay; tip QR → tip). Parity rules:

- **Feature parity, not shell parity** — every ✅/◐ cell in §3 must be reachable app-less **except G3** (mobile key needs the native app per lock-vendor SDKs; app-less guests use cards or a front-desk code path).
- **Session** = signed, expiring stay token in the link; re-entry via the same link or booking ref + surname. No password, no account.
- **Push substitutes:** where the app would push (G8), app-less guests get SMS/WhatsApp/email on the same triggers and preferences.
- **Upgrade path:** every app-less flow ends with a soft "get the app" deep link carrying the session — never a blocking install wall.

### 6.3 Companion / shared-key guest

Invited by the booker (G3 key share): inherits the stay's stage with a scoped set — key, services, explore, chat, the shared group itinerary (G13), and their *own* split share of the folio, settled via a per-person payment link (G9 split & group billing). **No full-folio visibility, no check-in/out, no booking modification** — those belong to the booker. Companion charges post to the stay folio flagged with who ordered.

---

## 7. Hard rules (must never be violated)

Stage logic is presentation; these are **server-enforced invariants**:

1. **The key never appears before the room is ready.** G3 issues only on `room_ready ∧ check-in complete ∧ payment cleared`, and is **revoked at check-out** — never merely hidden.
2. **The full folio is organizer/primary-only.** Only the booker (and co-guests the booker grants) sees G9's full folio detail; a companion sees their *own* split share and settles it via a per-person payment link (G9 split & group billing); day visitors have no folio at all — they pay per transaction.
3. **Check-out kills state:** express check-out (G2) settles the folio *before* confirming, revokes keys (G3), closes open requests (G4), and moves stage to Post-Stay atomically.
4. **No charge-to-room without a verified stay.** "Charge to folio" (G5/G12) appears only In-Stay for stay-verified identities; everyone else pays direct.
5. **Payment actions require fresh authentication** (biometric/OTP) even inside a valid session — a found phone must not settle a folio or buy an upgrade.
6. **G6 offers never interrupt a payment or check-in step** — offers ride *between* steps; Departure-stage offers are limited to late check-out.
7. **G14 prompts are frequency-capped** (one arrival pulse, smart-moment in-stay pulses, one review invite); a negative pulse always routes to recovery (staff F14) before any public-review invite.
8. **Post-Stay contact is capped and consent-based** — receipt and review invite by default; marketing win-backs only with explicit opt-in (G8).
9. **A cancelled or no-show reservation drops to Discover & Book immediately** — no stale stay cards, orphaned keys, or in-stay pushes.
10. **Stage never unlocks another guest's data.** The API authorizes every read against (identity, reservation), not against the stage claim.

---

## 8. Privacy & PII handling

The guest app handles more PII than the staff app exposes to any single role — identity documents, payment instruments, location-on-property, dietary/religious signals. Rules:

- **Data minimization by stage.** Collect at the stage that needs it: ID at pre-arrival/check-in (G1/G2), payment at booking/settlement (G9), preferences only as volunteered (G13). Nothing is collected "for later."
- **ID documents (G2)** go over an encrypted channel into the PMS document vault (retention per Bangladeshi regulatory requirements) and are **never visible in the app after submission** — status only ("verified ✓"). Staff access follows staff RBAC.
- **Payments (G9)** are tokenized through the gateway (SSLCommerz / bKash / Nagad / card acquirer); Atrium stores tokens and receipts, never PANs or wallet credentials. Folio detail requires the authenticated booker (§7.2).
- **Location** is foreground-only and purpose-bound (G5 deliver-to-location, G11 directions); **no background tracking, no movement history** beyond order fulfillment.
- **Messaging (G7)** threads are stay-scoped; auto-translation is transient; WhatsApp/SMS bridging stores the number with channel consent.
- **Preference sensitivity.** Dietary, accessibility, and prayer-time preferences (G10/G13) can reveal religion or health; they surface to staff only where a task needs them (dietary flag on a G5 order) and are excluded from marketing segmentation by default.
- **Companions and minors:** companion profiles hold the minimum (name, key credential, phone); the booker consents for minors; companion activity reaches the booker only as folio line items (§6.3).
- **Post-Stay retention:** operational data follows PMS retention policy; profile/loyalty data (G13) persists with consent and is deletable on request, preserving only legally required financial records.
- **App-less sessions (G15)** use signed, expiring tokens; links carry no PII in the URL beyond the opaque token and expire shortly after Post-Stay (receipt access then requires re-authentication).
- **Language ≠ profile.** The Bangla/English toggle is a device setting, not a stored inference about the guest.

---

## 9. Implementation notes

- **Server-computed stage.** Stage resolution (§4) lives in the API and ships in the session/stay payload; native and app-less clients render from it identically. The client hides; the server denies (§7).
- **One template, staged content.** Home is a composition of sections toggled per §5 — not six screens; the app-less surface reuses the same components flow-by-flow.
- **Stage transitions are events.** `checkin.completed`, `room.ready`, `checkout.completed` are the same events the staff app emits/consumes (F1/F3); the guest shell re-renders on the push that announces them (G8).
- **Persona tuning is configuration** — section ordering and default quick actions per persona are property-configurable weights, not code forks.
- **Offline:** guidebook (G10), itinerary (G12), and the issued key (per lock-vendor policy) are cached; state-changing actions disable per §4's fallback rule.
- **Auditability:** key issuance/revocation (G3), folio settlement (G9), and ID verification (G2) are audit-logged with actor, device, timestamp — guest-side entries in the same stream the staff app writes.

---

## 10. Open questions

- Day-visitor scope: is G4 (e.g., "more towels at the pool") wanted for day-use guests, or strictly G5/G12?
- Companion permissions: a companion seeing and settling their own split share is canon (G9 split & group billing); the residual question is whether an organizer can grant a companion visibility of *other* members' charges or the full folio.
- Post-Stay app-less token grace window before receipt access requires re-authentication — 7 / 30 / 90 days?
- Which lock vendors are certified at launch (G3), and is there a property-issued-code fallback for app-less guests?
- Event & banquet guests (§4): does v1 include event-schedule surfaces, or do they use the day-visitor shell?
- Regulatory review: ID-document retention and guest-registration reporting obligations for Bangladesh; foreign-passport handling.
- Win-back campaign caps (§7.8): property-configurable or platform-fixed?
