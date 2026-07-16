# Notifications & Campaigns — Feature Deep-Dive

**Product:** Atrium Guest (Hotel / Resort Guest-Facing Mobile App)
**Feature area:** G8 — Notifications & Campaigns
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-16
**Status:** Draft v1.0
**Related:** [Guest Mobile App — Feature Scope](guest-mobile-app-feature-scope.md) · [Guest Messaging — Deep-Dive](guest-mobile-app-guest-messaging-feature.md) · [Upsells, Offers & Add-Ons — Deep-Dive](guest-mobile-app-upsells-offers-feature.md) · [App-less Access — Deep-Dive](guest-mobile-app-app-less-access-feature.md) · [Staff Notifications & Alerts — Deep-Dive](../../staff/docs/staff-mobile-app-notifications-alerts-feature.md) · [Competitive Feature Research](../../HOTEL-~1.MD)

---

## 1. Executive Summary

**Notifications & Campaigns** is the push layer of Atrium Guest — the guest-side analog of [Atrium Staff Notifications & Alerts (F6)](../../staff/docs/staff-mobile-app-notifications-alerts-feature.md). Just as F6 is what makes staff features *actionable* (a task assigned silently is a task not done), G8 is what makes guest features *timely*: mobile check-in only removes the queue if the guest knows check-in is open; the digital key only delights if the guest learns the room is ready while still at the pool; express checkout only happens if the folio-ready nudge arrives before the guest joins the morning line.

The feature has two halves with very different characters. **Journey (transactional) notifications** are operational plumbing: booking confirmed, check-in open, room ready, order on its way, folio ready, activity reminder — each triggered by a real event in the stay, each deep-linking into the exact screen where the guest acts. Shiji's automated room-ready alerts are the canonical precedent. **Campaigns** are the property's targeted megaphone: tonight's live music, spa slots open tomorrow, the F&B promo — image, message, and call-to-action, targeted by journey stage and interest, in the pattern Nonius and the resort guest-app specialists have proven (and Duve now automates with AI agents).

Both halves live under one discipline: **per-guest preferences, quiet hours, and frequency caps** — because the fastest way to lose the channel is to abuse it. And because most guests won't install an app, every notification can **fall back through WhatsApp, SMS, or email** for app-less guests reached via [G15](guest-mobile-app-app-less-access-feature.md).

The business case is dual: transactional notifications are the **adoption engine for every other feature's business case**, and campaigns are a direct **revenue lever on perishable inventory** — tonight's empty tables and tomorrow's unbooked spa slots expire worthless unless someone tells the guests who are already on property.

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What the guest experiences |
|---|------------|----------------------------|
| 1 | **Journey / transactional notifications** | Timely, event-triggered updates across the stay: booking confirmed, online check-in open, room ready, key issued, order status, request status, activity reminders, queue & table-ready alerts ("your table is ready", "your cabana is confirmed", queue-position updates — [G12](guest-mobile-app-activities-spa-experiences-feature.md)), folio ready, checkout confirmation. |
| 2 | **Targeted property campaigns** | Rich promotional pushes — image, message, call-to-action — for tonight's live music, open spa availability, F&B promotions, events — targeted by journey stage, stay type, and interests. |
| 3 | **Quiet hours & per-guest preferences** | Control over what arrives and when: category-level opt-in/out, quiet hours (no promos at 11pm), and frequency caps enforced platform-side. |
| 4 | **Channel fallback (push → WhatsApp / SMS / email)** | App-less guests get the same timely message on the channel they have — cascading from push to WhatsApp to SMS/email until one lands. |
| 5 | **Deep links into the relevant screen** | Every notification opens the exact place to act: room-ready opens the key (G3), order update opens tracking (G5), a spa campaign opens booking (G12), folio-ready opens payment (G9) — in-app or as a G15 web flow. |

### 2.2 In scope (this release)
Event-triggered journey notifications wired to every Atrium Guest feature (G1 confirmation, G2 check-in open / room ready, G3 key issued, G4 request status, G5 order status, G6 offer moments, G9 folio ready / payment received, G12 booking confirmations, reminders & queue/table-ready alerts with hold windows, G14 pulse prompts); a property-side campaign composer with image + CTA, stage/interest/stay-type targeting, scheduling, and send caps; per-guest preference center with category opt-outs and quiet hours; platform-enforced frequency caps and transactional/promotional separation; channel fallback cascade (push → WhatsApp → SMS/email) with delivery tracking; deep links resolving into app screens or G15 web flows; campaign performance reporting (delivered, opened, converted).

### 2.3 Out of scope (this release)
Post-stay marketing automation and email drip campaigns (CRM territory — future); paid-media retargeting; cross-property campaign networks; in-app inbox archive beyond a simple recent-notifications list; SMS/WhatsApp inbound handling (that is [G7 Messaging](guest-mobile-app-guest-messaging-feature.md) — G8 is outbound); staff-facing alerts (that is [F6](../../staff/docs/staff-mobile-app-notifications-alerts-feature.md)).

### 2.4 Key assumptions
- Push infrastructure (APNs/FCM) is in place; WhatsApp Business API and SMS/email gateways are shared with G7's channel plumbing.
- Stay-stage state (pre-arrival / arrival / in-stay / departure) is derivable in real time from the PMS reservation status — targeting depends on it.
- Interest signals come from profile preferences (G13), on-property behavior (orders, bookings), and stated preferences at pre-arrival; campaigns degrade gracefully to stage-only targeting when signals are thin.
- WhatsApp template messages for transactional and promotional categories are pre-approved per property.

---

## 3. Capability Deep-Dive — The Notification & Campaign Features

Each capability is elaborated — **what it is**, **how it works in Atrium**, **why it matters**, and the **revenue/cost angle** — with vendor precedents noting the bar to clear.

### 3.1 Journey / transactional notifications
*Precedent: Shiji (automated, segmented room-ready alerts on the guest's preferred channel); Cloudbeds/SiteMinder-pattern event pushes.*

**What it is.** Event-triggered, personal, operational messages that track the guest's actual journey: *your booking is confirmed → online check-in is open → your room is ready → your key is on your phone → your order is on its way → your table is ready → your folio is ready for express checkout.*

**How it works in Atrium.** Every notification is fired by a real system event, not a schedule: the PMS flips the room to inspected-ready and the room-ready push goes out with the key-activation deep link; the kitchen marks the order out-for-delivery and the guest's phone says so. Timing rules prevent absurdities (no "check-in open" at 3am — held to the morning window). Each message is short, personal, and actionable — one tap lands the guest on the screen where the next step happens.

**Why it matters.** This is the difference between features that exist and features that get used. The guest who is told the room is ready while at lunch walks straight up with a live key — no desk visit, no "is it ready yet?" call. Every self-service flow in the app has a moment where the guest must be *told* it's their turn; G8 is that telling.

**Revenue / cost angle.** Room-ready alerts unlock more same-day early check-ins (sellable inventory) and folio-ready nudges drive express checkout adoption (revenue + cost); every status push pre-empts an inbound "any update?" call the desk would otherwise field (cost).

### 3.2 Targeted property campaigns — image + CTA
*Precedent: Nonius and resort guest-app specialists (push notifications & in-app campaigns with image + CTA); Duve (AI agents composing and timing engagement).*

**What it is.** The property's channel to guests **already on property or arriving soon**: a rich push — photo of tonight's band, two lines of copy, a **Book a table** button — sent to the guests for whom it's relevant, at the hour it converts.

**How it works in Atrium.** A manager composes a campaign in minutes: pick the audience (in-stay guests · families · arriving-tomorrow · spa-interested), the content (image, headline, body, CTA), and the send time. The CTA deep-links into the transactional flow — the live-music push opens the table booking in [G12](guest-mobile-app-activities-spa-experiences-feature.md), the F&B promo opens the menu in [G5](guest-mobile-app-food-beverage-ordering-feature.md) with the promo applied. Targeting is stage- and interest-aware by default: departure-day guests never see "join us tonight"; the couple never gets the kids' club push.

**Why it matters.** A resort's biggest untapped audience is asleep in its own rooms. Today the property reaches them with lobby posters and elevator cards — untargeted, unmeasurable, and invisible from the pool. A targeted push reaches exactly the guests who might come, with a one-tap path from impulse to booking.

**Revenue / cost angle.** This is the perishable-inventory lever: tonight's empty covers and tomorrow's open spa slots are revenue that expires at midnight — campaigns convert some of it, at near-zero marginal cost, every day (revenue); replaces printed flyers and door-drop promos and makes results measurable (cost).

### 3.3 Quiet hours & per-guest preferences
*Precedent: platform norms (APNs/FCM category permissions); the F6 staff-side quiet-hours discipline applied guest-side.*

**What it is.** The trust layer: a preference center where the guest controls categories (stay updates · offers & events · activity reminders), quiet hours during which promotional messages never fire, and platform-enforced frequency caps that no property campaign can exceed.

**How it works in Atrium.** Transactional and promotional traffic are strictly separated: stay-critical updates (room ready, order status) always deliver; promotional campaigns respect opt-outs, quiet hours (default 21:00–08:00 local, guest-adjustable), and a daily cap. The composer shows the property how many of its targeted guests are reachable under current caps — making restraint visible rather than aspirational.

**Why it matters.** Notification abuse is self-defeating: the first irrelevant midnight promo teaches the guest to disable notifications entirely, killing the transactional channel that every other feature depends on. Guarding the channel is guarding the whole app's usefulness — the same lesson F6 encodes for staff alert fatigue.

**Revenue / cost angle.** A trusted channel sustains open rates that keep both halves working — preference-respecting programs measurably outperform spray-and-pray on conversion per send (revenue); high opt-out rates would push properties back to untargeted printed promotion (cost avoidance).

### 3.4 Channel fallback — push → WhatsApp / SMS / email
*Precedent: Shiji ("on the guest's preferred channel"); Canary & Duve (multi-channel delivery over SMS/WhatsApp/email).*

**What it is.** Delivery that follows the guest rather than assuming the app: for guests without the app installed — the majority, per the [G15 thesis](guest-mobile-app-app-less-access-feature.md) — the same message cascades to WhatsApp, then SMS or email, until a channel confirms delivery.

**How it works in Atrium.** Each guest has a resolved channel profile (app push available? WhatsApp number verified? email on file?). Transactional messages cascade automatically — room-ready goes as a push if the app is present, else a WhatsApp template with the G15 web-key/check-in link, else an SMS. Campaigns use fallback more conservatively (WhatsApp/email only for opted-in guests) to respect both cost and consent. Delivery and click-through are tracked per channel.

**Why it matters.** Without fallback, G8 serves only app installers and the room-ready alert — the single highest-value notification of the stay — never reaches the two-night business guest who skipped the install. With fallback, **every guest with a phone number gets the timely moments**, and each message's G15 link is itself an on-ramp into the web flows.

**Revenue / cost angle.** Fallback multiplies the reachable audience for both journey nudges and campaigns by 3–5× over app-installers alone — directly scaling every conversion figure in §6 (revenue); WhatsApp/SMS per-message costs are pennies against the folio value a delivered message influences (cost-effective by orders of magnitude).

### 3.5 Deep links into the relevant screen
*Precedent: standard in best-in-class guest apps (Nonius-pattern CTA campaigns); table stakes for conversion.*

**What it is.** The rule that no notification ever dead-ends: each one carries a link that opens the exact screen where the guest acts — in the app when installed, as the equivalent [G15 web flow](guest-mobile-app-app-less-access-feature.md) when not.

**How it works in Atrium.** Deep links are typed and stay-scoped: `room-ready → G3 key activation`, `order update → G5 tracker`, `folio ready → G9 payment`, `spa campaign → G12 booking, pre-filtered to the promoted slots`. The same link token resolves to app screen or web flow depending on the device — one link, both worlds. Links embedded in WhatsApp/SMS fallback messages carry scoped, expiring session tokens (G15 security model).

**Why it matters.** Conversion dies at every extra step. A push that says "spa slots open!" but lands on the home screen loses the impulse; one that lands on tomorrow 3pm's bookable slot captures it. For transactional messages the same logic is guest relief: the notification *is* the shortcut.

**Revenue / cost angle.** Tap-to-action distance is the strongest lever on campaign conversion (revenue); deep-linked status updates end the "got the message — now where do I go?" confusion that otherwise lands on the desk (cost).

---

## 4. Why This Matters

### 4.1 Every other feature's business case routes through this one
G2's queue elimination assumes the guest knows check-in is open. G3's key delight assumes the guest learns the room is ready. G5's reorder assumes the guest saw the order arrive on time. G9's express settlement assumes a folio-ready nudge. G8 is not a feature beside the others — it is the **timing layer** that converts their capability into usage, exactly as [F6](../../staff/docs/staff-mobile-app-notifications-alerts-feature.md) converts staff features into action.

### 4.2 Perishable inventory expires nightly — campaigns are the rescue channel
Restaurant covers, spa slots, activity seats, and event tickets are the classic perishables: unsold at close, worth zero. The guests most likely to buy them are already on property, phones in pockets. A targeted 5pm push about tonight's availability is the only medium that reaches them in time — and the only one that is measurable.

### 4.3 The channel is an asset that must be defended
One guest, over one stay, tolerates a handful of well-chosen messages. The discipline half of G8 — separation, quiet hours, caps, preferences — is what keeps open rates high enough for the whole system to keep working. An undisciplined push channel converges to zero value in weeks.

### 4.4 An opening in the BD market
Local vendors offer no guest-facing engagement channel at all; global guest-app specialists rarely bridge to WhatsApp — the channel Bangladeshi guests actually live on. Stage-targeted campaigns with WhatsApp fallback is a combination nobody in this market ships today.

---

## 5. Journey Moments & Operations It Improves

| Moment / operation today | Pain | With Notifications & Campaigns |
|--------------------------|------|-------------------------------|
| **Room not ready at arrival** | Guest hovers at the desk, asking repeatedly | "Your room is ready" push with key deep-link finds the guest at lunch; desk hovering ends. |
| **Check-in queue forms** | Guests don't know online check-in exists | "Check-in is open" nudge the morning of arrival drives G2 adoption before the lobby. |
| **"Where's my food?"** | Guests call the outlet for status | Order-status pushes at each step; calls stop. |
| **Tonight's event is half-empty** | Lobby poster reaches nobody at the pool | Targeted 5pm campaign with photo + "Book a table" reaches every relevant in-house guest. |
| **Tomorrow's spa gaps** | Slots expire unsold; nobody knew | Interest-targeted push opens G12 pre-filtered to the open slots. |
| **Checkout crush at 10am** | Everyone settles at the desk at once | "Folio ready — express checkout" push the night before moves settlement to the phone. |
| **Activity no-shows** | Guests forget the 7am tour they booked | Automatic reminders with meeting-point map link cut no-shows. |
| **App-less guest misses everything** | All of the above reaches only installers | WhatsApp/SMS/email fallback carries the same moments to every guest with a number. |
| **Marketing can't measure promos** | Printed flyers, unknown effect | Per-campaign delivered/opened/converted reporting. |

---

## 6. How It Increases Revenue

1. **Perishable-inventory conversion.** Stage- and interest-targeted campaigns fill tonight's covers, tomorrow's spa slots, and event seats that would otherwise expire — high-margin ancillary revenue created from inventory already paid for.
2. **Adoption lift on every revenue feature.** Check-in-open nudges drive G2 (and its upgrade offers), room-ready alerts enable early-check-in fees, menu-moment pushes drive G5 order volume, offer notifications deliver G6 at the buying moment. G8 is the distribution arm of the app's entire revenue stack.
3. **Express-checkout folio nudges protect settlement.** Folio-ready messages drive in-app settlement (G9), reducing unpaid-balance leakage and disputes at departure.
4. **Reduced no-shows on booked ancillaries.** Reminders protect the realized revenue of G12 bookings — a no-showed spa slot is a double loss (unsold and unsellable).
5. **Fallback reach multiplies everything.** Every mechanism above scales linearly with the share of guests reachable; the WhatsApp/SMS cascade takes that share from app-installers to effectively all guests.

> **Illustrative model:** A 150-room resort at 75% occupancy has ~110 in-house parties nightly. If one daily targeted campaign reaches 80 relevant guests and converts 6% at an average ৳2,000 (dinner for two, a spa treatment, event seats), that is ~৳9,600/day ≈ **৳3.5M/year** from campaigns alone — before the adoption lift G8 gives every other feature's revenue case. *(Figures illustrative; validate against property data.)*

---

## 7. How It Reduces Operational Cost

1. **Status pushes pre-empt inbound calls.** "Is my room ready?" / "Where's my order?" / "Is my bill ready?" calls disappear when the answer arrives before the question — a large, daily, measurable share of desk and outlet phone traffic.
2. **Self-service adoption is a labor multiplier.** Every guest nudged into mobile check-in, express checkout, or in-app ordering is a transaction staff don't perform. G8 is the cheapest possible spend to move that adoption number.
3. **Campaign labor near zero.** A composed-in-minutes push replaces the design-print-distribute cycle of paper promotion — and the nightly walk to slip flyers under doors.
4. **Fewer no-show recovery costs.** Reminder-driven attendance means less staff time rebooking, refunding, and adjudicating missed activities.
5. **Measurement replaces guesswork.** Delivered/opened/converted data tells the property which promotions earn their send — ending spend on promotion that demonstrably converts nobody.

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Timeliness | Event-to-delivery latency for journey notifications | < 60 seconds |
| Reach | Stays reachable on ≥ 1 channel (push or fallback) | ≥ 90% |
| Channel health | Guests with notifications enabled / not opted out at checkout | ≥ 80% |
| Self-service lift | G2 mobile check-in share attributable to the check-in-open nudge | ↑ measurable |
| Call deflection | "Status question" calls to desk/outlets | ↓ 40% |
| Campaign conversion | CTA tap-to-purchase rate on targeted campaigns | ≥ 5% |
| Perishable fill | Same-day covers / next-day spa slots filled via campaigns | ↑ measurable |
| Discipline | Promotional sends per guest per day (platform cap) | ≤ 2, quiet-hours violations = 0 |
| No-show protection | No-show rate on reminded G12 bookings | ↓ 30% |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| Event sources | Journey notifications need real-time triggers from PMS/POS/G4/G12 | Same event bus as staff F6; degrade to polling where a system lags |
| Push permission | Guests may deny OS push permission | Ask in context ("get told when your room is ready"), not at first launch; fallback channels |
| Notification fatigue | Over-sending kills the channel for everything | Platform-enforced caps, quiet hours, transactional/promotional separation, opt-out analytics reviewed per property |
| WhatsApp templates & cost | Templates need approval; per-message fees | Pre-approve a standard template pack at onboarding; SMS/email fallback; cost dashboard per property |
| Stage-detection accuracy | Wrong stage = embarrassing targeting (promo after checkout) | Derive stage from live PMS status; suppress promos on departure day+; test harness for stage transitions |
| Deep-link integrity | Broken links turn nudges into dead-ends | Typed link registry shared with G15; automated link tests per release |
| Consent & regulation | Promotional messaging consent (and BTRC SMS rules in BD) | Explicit promo opt-in at booking/check-in; per-channel consent records; local-counsel review |
| Property misuse | A property blasting daily discounts damages the shared channel | Caps not overridable property-side; campaign quality guidance in onboarding |

---

## 10. Scope delivered

Atrium Guest ships as one complete package. This feature delivers:

- Event-triggered journey notifications across all guest features (confirmation, check-in open, room ready, key issued, request status, order status, booking reminders, queue & table-ready alerts, folio ready, checkout)
- Property campaign composer with image, copy, CTA, scheduling, and audience targeting by stage, stay type, and interest
- Per-guest preference center: category opt-in/out, quiet hours, channel preference
- Platform-enforced frequency caps and strict transactional/promotional separation
- Channel fallback cascade — push → WhatsApp → SMS/email — with per-channel delivery tracking
- Typed deep links resolving to app screens or G15 web flows from every notification
- Campaign reporting: delivered, opened, converted, opt-out impact
- Consent capture and records per channel

---

## 11. Open Questions

- Default quiet hours and daily promo cap — platform-fixed, or configurable within platform bounds per property?
- Who composes campaigns at the property — F&B/marketing directly, or gated through a manager approval step?
- Is campaign targeting in v1 rule-based only (stage/interest filters), or do we ship send-time/audience optimization (Duve-style AI) at launch?
- WhatsApp promotional messaging consent: captured at booking, at check-in, or both?
- Do day visitors (F&B/day-use personas) get campaign traffic, and under what consent?
- Which analytics events define "converted" per campaign type (order placed vs. booking made vs. offer accepted)?

---

## 12. One-Line Business Case

> **Notifications & Campaigns is the timing layer that makes every other guest feature actually get used — telling each guest the right thing at the right moment on whatever channel they have — while turning the property's perishable inventory of tonight's tables and tomorrow's spa slots into revenue with a targeted push instead of a lobby poster.**
