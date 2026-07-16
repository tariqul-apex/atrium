# App-less Access — QR & Web Links — Feature Deep-Dive

**Product:** Atrium Guest (Hotel / Resort Guest-Facing Mobile App)
**Feature area:** G15 — App-less Access (QR & Web Links)
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-16
**Status:** Draft v1.0
**Related:** [Guest Mobile App — Feature Scope](guest-mobile-app-feature-scope.md) · [Check-In & Check-Out — Deep-Dive](guest-mobile-app-check-in-check-out-feature.md) · [Food & Beverage Ordering — Deep-Dive](guest-mobile-app-food-beverage-ordering-feature.md) · [Guest Messaging — Deep-Dive](guest-mobile-app-guest-messaging-feature.md) · [Payments, Folio & Tipping — Deep-Dive](guest-mobile-app-payments-folio-tipping-feature.md) · [Staff NFC & Barcode Scanning — Deep-Dive](../../staff/docs/staff-mobile-app-nfc-barcode-scanning-feature.md) · [Competitive Feature Research](../../HOTEL-~1.MD)

---

## 1. Executive Summary

**App-less Access** makes every guest-facing flow in Atrium Guest reachable through a **web link or a QR code — no install, no account, no app store**. A magic link in the booking confirmation opens check-in. A QR on the table tent opens the menu and takes the order. A code on the bedside card opens chat, requests, and the folio. A QR at the desk takes a tip. The native app still exists — richer, faster, push-enabled, and the only home of the digital key — but it is the **upgrade path**, not the entry requirement.

Like [NFC & Barcode Scanning (F17)](../../staff/docs/staff-mobile-app-nfc-barcode-scanning-feature.md) on the staff side, this is a **cross-cutting enabler, not a screen of its own**. There is no "App-less" tab; there is a link-and-QR delivery channel running *inside* [check-in (G2)](guest-mobile-app-check-in-check-out-feature.md), [ordering (G5)](guest-mobile-app-food-beverage-ordering-feature.md), [requests (G4)](guest-mobile-app-in-stay-service-requests-feature.md), [chat (G7)](guest-mobile-app-guest-messaging-feature.md), [folio & payment (G9)](guest-mobile-app-payments-folio-tipping-feature.md), [feedback (G14)](guest-mobile-app-feedback-service-recovery-feature.md), and [tipping (G9)](guest-mobile-app-payments-folio-tipping-feature.md) — one web runtime, surfaced at physical and digital touchpoints across the journey.

The core argument is arithmetic. Most guests will not install an app for a two-night stay — industry experience puts voluntary install rates for hotel apps in the low double digits at best, and the app-less pattern is precisely why Canary (app-less check-in and tipping), Duve (web-based guest app), and IDS Next FX GeM (QR check-in) built their products around links rather than installs. **Without G15, every other feature's business case applies to a fraction of guests. With it, the addressable base becomes effectively every guest on property — adoption is the multiplier on every other feature in this product.** A feature that converts brilliantly for the 15% who installed is worth less than one that converts modestly for the 95% who scanned.

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What it enables |
|---|------------|-----------------|
| 1 | **Magic-link entry** | A personalized, secure link in the booking confirmation (email/WhatsApp/SMS) opens the guest's stay — check-in, preferences, folio — with no install and no password. |
| 2 | **QR touchpoints across the property** | Bedside card, table tent, reception standee, back-of-door card, poolside signage, staff badges — each QR opens the *right flow in the right context*: this table's menu, this room's requests, this desk's tip jar. |
| 3 | **Web flows for the core journey** | Check-in (G2), menus & ordering (G5), service requests (G4), chat (G7), folio & payment (G9), feedback (G14), and tipping (G9) all run fully in the mobile browser. |
| 4 | **Progressive-web-app quality bar** | The web experience is held to app-grade standard: instant load on hotel Wi-Fi and 4G, touch-first UI, add-to-home-screen, graceful behavior on weak connectivity. |
| 5 | **Session security for link access** | Scoped, expiring tokens: a link grants exactly what it should (this stay's check-in, this table's ordering, this folio's payment) for exactly as long as it should — with step-up verification for sensitive actions. |
| 6 | **Native app as the upgrade path** | The web flows invite — never require — the install, at the moments it pays: resort stays, loyalty members, and the digital key (G3), which is native-only. |

### 2.2 In scope (this release)
Tokenized magic links generated per reservation and delivered via booking confirmation, G8 fallback messages, and staff-sent (F1/F5) links; a property QR system — generation, printing templates, and placement kit for bedside, table, reception, back-of-door, and tipping touchpoints — with context encoded per code (room, table, outlet, recipient); mobile-web versions of G2 check-in/checkout, G5 browsing and ordering, G4 request creation and tracking, G7 chat, G9 folio view/payment/tipping, and G14 feedback; PWA installability (add-to-home-screen, offline shell for static content); scoped/expiring session tokens with device binding and step-up verification (booking reference + name, or OTP) for payment and identity-sensitive steps; contextual install prompts with stay-type and loyalty targeting; per-touchpoint scan and conversion analytics.

### 2.3 Out of scope (this release)
Digital room key on the web (BLE/wallet key issuance is native-only by platform constraint — the web flow explains and hands off to the install); offline-first web operation beyond a cached shell (full offline is a native-app strength); web push notifications (fallback messaging via WhatsApp/SMS/email covers app-less guests — see [G8](guest-mobile-app-notifications-campaigns-feature.md)); kiosk hardware (the QR standee pattern makes the guest's own phone the kiosk); NFC-tag guest touchpoints (QR ships first; NFC tags are additive later and already staff-side in F17); guest accounts with passwords (magic links and OTP replace them by design).

### 2.4 Key assumptions
- Every guest-facing feature exposes its flows through the shared web runtime — G15 is the delivery channel, not a reimplementation; feature teams own their flows on both surfaces.
- Reservation contact details (email/phone) are reliable enough to deliver magic links; front desk can re-send or QR-display a link for any guest whose contacts fail.
- Properties will physically deploy the QR kit (printing and placement is an onboarding task, analogous to F17's tag rollout on the staff side).
- Guest phones can scan QR codes from the native camera (universal on modern devices) and have at least intermittent data or property Wi-Fi.

---

## 3. Capability Deep-Dive — The App-less Features

Each capability is elaborated — **what it is**, **how it works in Atrium**, **why it matters**, and the **revenue/cost angle** — with vendor precedents noting the bar to clear. As with staff F17, the pattern to hold onto is that **each capability amplifies other features rather than standing alone**.

### 3.1 Magic-link entry from the booking confirmation
*Precedent: Canary (app-less check-in from a link), Duve (web-based guest app opened from pre-arrival messages), Guestline GuestStay (emailed registration).*

**What it is.** The guest's front door to their stay: a personalized link — in the confirmation email, the WhatsApp confirmation, or an SMS — that opens *their* reservation in the browser, already authenticated to the right scope, with no password ever created.

**How it works in Atrium.** At booking, Atrium mints a stay-scoped token and embeds it in the confirmation across channels ([G1](guest-mobile-app-booking-pre-arrival-feature.md) sends it; [G8](guest-mobile-app-notifications-campaigns-feature.md) fallback messages re-carry it at each journey moment — the check-in-open nudge carries the check-in link, the folio-ready nudge carries the payment link). Opening the link lands the guest in the pre-arrival flow: registration, preferences, upsells, check-in when the window opens. The same link (re-verified) serves them all stay. Front desk can re-send it or show it as a QR from the [F1 staff app](../../staff/docs/staff-mobile-app-front-desk-feature.md) for any guest standing in front of them.

**Why it matters.** The confirmation message is the one touchpoint with a 100% delivery rate to every booked guest — OTA or direct, app or no app. Making it the entry ramp means pre-arrival registration, upsell capture, and check-in adoption are no longer gated on an install decision the guest was never going to make. This is exactly the mechanism Canary and Duve built categories on.

**Revenue / cost angle.** Pre-arrival upsell and early-check-in offers reach every booked guest instead of installers only (revenue, multiplicative); every registration completed from the link is desk data-entry that never happens ([G2's labor case](guest-mobile-app-check-in-check-out-feature.md), unlocked at full population) (cost).

### 3.2 QR touchpoints — the property as the interface
*Precedent: IDS Next FX GeM (QR-code instant check-in), Canary (QR tipping), table-QR ordering as global F&B standard practice.*

**What it is.** Physical QR placements that make each location its own entry point: a **reception standee** (check in here from your phone), a **bedside card** (chat, requests, folio, order to the room), a **table tent** in every outlet (this table's menu and ordering), a **back-of-door card** (checkout, feedback, housekeeping requests), **tip QRs** at desks and on housekeeping cards.

**How it works in Atrium.** Every code encodes context: property, location type, and the specific room/table/recipient. Scanning the pool-bar table tent opens [G5](guest-mobile-app-food-beverage-ordering-feature.md) already scoped to that outlet and that table — no "where are you?" step; the bedside card opens a stay-hub scoped to that room (with a light verification step before anything sensitive). Codes are generated, versioned, and printed from a property kit; each placement's scans and conversions are tracked, so the property learns which touchpoints earn their laminate. This is the guest-facing mirror of [F17's tag provisioning](../../staff/docs/staff-mobile-app-nfc-barcode-scanning-feature.md): cheap printed codes, one-time placement effort, compounding returns.

**Why it matters.** In-stay, the guest's need is *situated*: hungry at this table, missing towels in this room, grateful to this bellman. QR placement puts the entry point at the exact point of need with context pre-filled — the shortest possible path from impulse to transaction, requiring zero prior setup by the guest.

**Revenue / cost angle.** Table and poolside QRs convert impulse into orders at the moment of hunger — the proven table-QR uplift in covers-per-server and average ticket (revenue); context pre-fill removes the mis-routed requests and "which room was that?" clarification loops (cost).

### 3.3 Web flows for the core journey
*Precedent: Duve (fully web-based guest app), Canary (app-less check-in & tipping), WebRezPro (no-download pattern, staff-side).*

**What it is.** The substance behind the links: browser-native implementations of the journey's core — check-in and checkout (G2), menus and ordering (G5), requests with photo attach (G4), the chat thread (G7), folio, payment, and tipping (G9), and feedback (G14) — functionally equivalent to the app for everything a short-stay guest needs.

**How it works in Atrium.** One shared web runtime serves every flow; entry context (from link or QR) routes directly to the relevant flow with stay/table/room scope applied. Everything lands in the same backend the app uses — a web order is a [staff POS (F8)](../../staff/docs/staff-mobile-app-pos-multi-outlet-feature.md) ticket, a web request is an [F5-routed task](../../staff/docs/staff-mobile-app-task-management-communication-feature.md), a web payment posts to the same folio. Staff never know or care which surface the guest used. What stays native-only is the digital key (G3, platform constraint), rich offline behavior, and OS push — each web flow that touches those boundaries explains and offers the upgrade (§3.6).

**Why it matters.** App-less access is only as credible as its weakest flow. If the QR opens a menu but ordering requires the install, the property has built a disappointment machine. Full transactional capability on the web is the difference between a marketing brochure and a service channel — this is Duve's core architectural bet, validated at scale.

**Revenue / cost angle.** Ordering, upsells, payments, and tips all transact at full population reach (revenue — this is where the multiplier physically happens); one backend and one business-logic layer for both surfaces keeps the second surface's marginal engineering cost low (cost).

### 3.4 Progressive-web-app quality bar
*Precedent: Duve (web guest app at native-feel quality); PWA as industry standard practice.*

**What it is.** A non-negotiable performance and polish standard for the web runtime: first meaningful render in low single-digit seconds on hotel Wi-Fi and BD mobile data, touch-first layouts, no layout jank, installable to home screen, and graceful degradation (cached shell, clear retry states) when connectivity stutters.

**How it works in Atrium.** The web runtime is built and budgeted as a PWA: aggressive asset budgets, server-rendered first paint, image discipline on menu photos, offline-cached static content (guidebook pages, menus browsed before ordering), and an add-to-home-screen prompt after a successful transaction — which yields an app-like icon without an app store. Performance budgets are release-gated, tested against throttled-3G profiles, because the target guest is on a loaded guest network or a patchy cellular link.

**Why it matters.** A slow web flow doesn't just fail its own transaction — it teaches the guest that the QR codes at this property are not worth scanning, poisoning every other touchpoint. The quality bar is what makes the whole G15 thesis true in practice rather than in architecture diagrams.

**Revenue / cost angle.** Load time is conversion: every second of delay measurably cuts completion on ordering and payment flows (revenue); a web experience good enough to *be* the product avoids pressure to build kiosk hardware or staff-assisted workarounds (cost).

### 3.5 Session security for link access
*Precedent: hospitality magic-link session patterns (Canary, Duve); payment-link scoping norms (Cloudbeds/RoomRaccoon pay-by-link).*

**What it is.** The trust architecture that makes install-free safe: every link and QR session carries a **scoped, expiring token** — this stay, this table, this folio, this tip recipient; nothing more — with sensitive actions gated behind step-up verification.

**How it works in Atrium.** Tokens are tiered by sensitivity. A menu-browsing QR is anonymous. Placing a room-charged order or opening a stay hub requires binding to the reservation (room + name confirmation or an OTP to the booking phone). Folio view, payment, and identity steps in check-in step up again ([G9 pay-links](guest-mobile-app-payments-folio-tipping-feature.md) are single-use and amount-locked; card entry is gateway-hosted per G9's PCI posture). Tokens expire with their purpose — table sessions in hours, stay links at checkout — and every session is auditable. A forwarded or leaked link exposes as little as the design allows, and re-verification walls the rest.

**Why it matters.** The app-less model replaces the password with the link — so the link's scope discipline *is* the security model. Done right, it is both safer and smoother than passwords (nothing to reuse or phish at scale, nothing for the guest to remember); done lazily, one leaked URL is someone else's folio. This capability is what lets the property say yes to app-less payments and check-in with a straight face.

**Revenue / cost angle.** Trustworthy link security is the precondition for running payment and identity flows app-less at all — it protects the entire G15 revenue multiplier (revenue enabling); scoped tokens and audit trails contain the blast radius and investigation cost of any incident (cost/risk).

### 3.6 Native app as the upgrade path
*Precedent: the two-tier pattern across the market — Canary/Duve web-first with app-grade journeys; INTELITY/Mews native keys and wallet integration.*

**What it is.** The deliberate positioning of the native app as the *earned* tier: the web serves everyone; the app is offered exactly where it pays — the **digital key (G3)**, which is native-only; **resort stays**, where a week of G11 maps, G12 itineraries, and G8 push makes the install worthwhile; and **loyalty members (G13)**, whose relationship outlives any one stay.

**How it works in Atrium.** Install prompts are contextual and value-anchored, never a gate: at check-in completion — "skip the key desk: put your room key on your phone"; on a 7-night resort booking — "your itinerary, map, and tonight's events, all offline"; at loyalty enrollment — "track your points and unlock member rates." Web session state carries into the app on install (same stay, same thread, same folio) so upgrading never restarts anything. Guests who decline lose nothing they had.

**Why it matters.** The install question stops being "will guests download our app?" (mostly no) and becomes "which guests get enough value to upgrade?" (a knowable, targetable set). This resolves the false either/or between web reach and native depth: reach for all, depth for the guests who repay it — and the web flows themselves become the app's best-qualified acquisition channel.

**Revenue / cost angle.** Loyalty members and resort guests — the highest-lifetime-value segments — end up on the highest-retention surface with push and key (revenue, targeted); marketing spend on app acquisition is replaced by conversion moments the journey generates for free (cost).

---

## 4. Why This Matters

### 4.1 Adoption is the multiplier on every other business case
Every deep-dive in this product ends with a revenue and cost model — and every one of those models scales linearly with the share of guests who can actually reach the feature. Voluntary hotel-app install rates are a small fraction of guests; QR-and-link reach approaches all of them. G15 is not the fifteenth feature; it is the coefficient in front of the other fourteen.

### 4.2 The two-night guest was never going to install
The install ask fails not because apps are bad but because the stay is short: 30 seconds of app-store friction against 48 hours of use is a trade most guests rationally decline. Canary and Duve built winning products by accepting this instead of fighting it. The app-less path meets the guest where they already are — the camera and the browser.

### 4.3 It is also the walk-in and day-visitor channel
Day visitors, restaurant walk-ins, and event guests — personas with no reservation and no reason to install anything — become addressable customers the moment a table tent can take their order and their payment. G15 is how the guest platform serves people who were never "guests" in the PMS sense at all.

### 4.4 It mirrors a pattern Atrium already committed to
Staff-side, [F17](../../staff/docs/staff-mobile-app-nfc-barcode-scanning-feature.md) proved the model: a cheap, printed, physical code layer that makes every digital feature one motion away. G15 is the same bet on the guest side — same provisioning discipline, same "capability inside other features" architecture, same compounding return on a one-time placement effort.

---

## 5. Journey Moments & Operations It Improves

| Moment / operation today | Pain | With App-less Access |
|--------------------------|------|----------------------|
| **Guest receives booking confirmation** | A PDF and a phone number; nothing actionable | Magic link opens registration, upsells, and check-in — for 100% of booked guests. |
| **Arrival queue at reception** | Only app-installers could self-check-in | Standee QR: anyone in line checks in from their phone while they wait. |
| **Hungry at the pool** | Flag a server, or nothing | Table-tent QR opens that location's menu; order lands in POS with the spot pre-filled. |
| **Needs towels; won't call** | Need goes unmet; satisfaction quietly drops | Bedside QR opens requests, pre-scoped to the room, photo attach and all. |
| **Wants to query the bill mid-stay** | March to the desk or wait for checkout | Bedside/stay-link opens the live folio; disputes route to F14 (G9). |
| **Wants to tip; has no cash** | The tip dies with the intention | Tip QR takes bKash/Nagad/card in seconds — Canary-pattern, app-less by design. |
| **Checkout morning** | Desk queue for folio + payment | Back-of-door QR or folio-ready link settles everything from the room. |
| **Day visitor at the restaurant** | Invisible to every digital system | Orders, pays, tips, and leaves feedback via QR — no reservation required. |
| **Property wants app adoption** | Begging via lobby signage; low-teens install rates | Web flows convert the right guests (key, resort, loyalty) at proven-value moments. |
| **Staff asked "do you have an app?"** | An apology or a QR to an app store | Any staff member surfaces any flow as a link from F1/F5 in seconds. |

---

## 6. How It Increases Revenue

1. **It multiplies every other feature's revenue case.** Upsell capture (G6), F&B ordering (G5), activities (G12), payments (G9), and campaign conversion (G8) each scale with reach. Moving reach from install-rate to scan-rate — from perhaps 15% to effectively all guests — is worth more than any single-feature optimization in this product.
2. **Table and location QRs create orders that otherwise never happen.** The global table-QR pattern consistently lifts order frequency and average ticket by removing the wait-for-a-server step — pure incremental F&B volume at the pool, the beach, and the lobby.
3. **Pre-arrival monetization reaches every booking.** Magic-link entry puts G1/G6 upsells — early check-in, upgrades, transfers, celebration packages — in front of 100% of confirmed guests, including OTA bookings the property otherwise can't market to.
4. **Day visitors and walk-ins become digital customers.** Ordering, payment, and tipping revenue from personas with no reservation — a population invisible to an install-first strategy.
5. **App-less tipping revives a dying stream.** Cashless tip capture (G9, Canary precedent) sustains the staff-income → service-quality → review-score chain that underwrites rate power.
6. **The upgrade path concentrates the app where it earns.** High-LTV guests (resort, loyalty) get pushed to the surface with push notifications and the key — the segments where native retention translates into direct rebooking.

> **Illustrative model:** Take the G5 ordering case: if in-app ordering lifts ancillary F&B by ৳X among installers, the same feature behind table QRs applies that lift to ~6× the guest population (95% reachable vs. ~15% installed). A pool outlet capturing just 10 incremental QR orders/day at ৳600 average is ~৳6,000/day ≈ **৳2.2M/year from one touchpoint type at one outlet** — before check-in upsell reach, day-visitor volume, and tip capture. *(Figures illustrative; validate against property data.)*

---

## 7. How It Reduces Operational Cost

1. **Self-service at full population, not installer population.** Every desk-labor saving claimed by G2 (check-in/out), G4 (requests vs. phone calls), G9 (settlement), and G10 (FAQ deflection) was gated on guest reach; G15 releases those savings across effectively all guests.
2. **The guest's phone is the kiosk.** QR standees deliver the self-check-in pattern with zero kiosk hardware to buy, mount, clean, and maintain — the guest brings and maintains the device.
3. **Context pre-fill kills clarification loops.** Room- and table-scoped entry means requests and orders arrive with location attached — no "which room?", no mis-delivered trays, no re-contact labor.
4. **One backend, two surfaces.** Web flows reuse the app's services and business logic; staff-side, everything lands in the same F5/F8/F1 queues regardless of surface — no parallel process, no extra training.
5. **Printed QR is the cheapest infrastructure in hospitality.** Laminated cards and standees cost effectively nothing against the transactions they carry (the F17 lesson, guest-side), and versioned codes mean reprints, not re-engineering.
6. **No app-store operations burden for the majority path.** The web channel updates instantly for all guests — no forced-update campaigns, no review-gating, no "please update your app" support tickets for the population that never installed.

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Reach | Stays engaging with ≥ 1 digital flow (web or app) | ≥ 80% (vs. install-only baseline) |
| Entry health | Magic-link open rate on booking confirmations | ≥ 50% |
| Touchpoint traction | Scans per placed QR per week, by touchpoint type | ↑ tracked; prune non-performers |
| Web conversion | Scan-to-completed-transaction rate (order, check-in, payment) | ≥ 60% |
| Performance bar | p75 first-meaningful-render on throttled 3G | < 3 seconds |
| Parity | Core-journey flows fully transactable on web | 100% (excl. native-only key) |
| Security | Sessions with correct scope; step-up completion rate | 100% scoped; ≥ 90% step-up completion |
| Upgrade path | Web-to-app conversion among resort/loyalty segments | ≥ 25% of prompted |
| Multiplier proof | Share of G5 orders / G2 check-ins / G9 payments arriving via web | Reported per feature, per property |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| Feature-team parity | Every feature must ship its web flow — G15 can't retrofit them | Parity is a release-gate per feature; shared runtime and components make web the default target, not an extra |
| Link deliverability | Magic links depend on booking contact quality (esp. OTA bookings) | Multi-channel delivery (email + WhatsApp/SMS); desk re-send and on-screen QR from F1 |
| Token leakage | Forwarded/shared links expose scoped sessions | Tiered scopes, short expiry, device binding, step-up for sensitive actions, full session audit |
| Physical rollout | QR placement is a real onboarding task; codes fade, peel, get moved | Property QR kit with placement guide; versioned codes; scan analytics flag dead placements (F17 playbook) |
| Web performance | Hotel Wi-Fi and BD mobile data are unforgiving | Release-gated performance budgets; throttled-network testing; server-rendered first paint; cached shell |
| Guest trust in QR | QR-phishing awareness may make guests hesitant | Branded, consistent code design; human-readable URLs on property domain; staff able to vouch and demonstrate |
| Malicious QR overlay | Third party stickers a fake code over the property's | Tamper-evident placements, periodic touchpoint audits, property-domain URL discipline guests can verify |
| iOS/Android web limits | No web BLE key; web push immature on iOS | Key is explicitly native-only with a clean handoff; G8 WhatsApp/SMS fallback replaces web push |
| Upgrade-prompt fatigue | Over-prompting the install annoys the majority who decline | Contextual prompts only at value moments; frequency-capped; never gate a working flow |

---

## 10. Scope delivered

Atrium Guest ships as one complete package. This feature delivers:

- Stay-scoped magic links minted per reservation, delivered via confirmation and G8 journey messages, re-sendable from staff F1/F5
- Property QR kit: code generation, versioning, print templates, and placement guidance for bedside, table tent, reception standee, back-of-door, outlet, and tipping touchpoints
- Context-encoded QR routing — room, table, outlet, and tip-recipient scope pre-filled on entry
- Full mobile-web flows for check-in/checkout (G2), menus & ordering (G5), service requests (G4), chat (G7), folio, payment & tipping (G9), and feedback (G14), landing in the same staff-side systems as the app
- PWA quality bar: performance budgets, add-to-home-screen, offline-cached shell for static content
- Tiered session security: scoped expiring tokens, device binding, step-up verification for payment- and identity-sensitive actions, session audit trail
- Contextual native-app upgrade prompts (key issuance, resort stays, loyalty enrollment) with session continuity from web to app
- Per-touchpoint scan, conversion, and placement-health analytics

---

## 11. Open Questions

- Web runtime architecture: one shared PWA shell serving all flows, or per-flow micro-frontends behind one URL scheme?
- Step-up verification default for room-charged orders — room + name confirmation, or OTP-only — and does it vary by property class?
- Who owns physical QR production and placement at onboarding — Atrium field team, or property staff with the kit and guide (the F17 question, guest-side)?
- Magic-link lifetime after checkout: how long does the stay hub remain accessible for receipts and feedback?
- Do OTA bookings reliably carry guest contact details per channel, and what is the fallback entry story where they don't (desk-issued QR only)?
- Is add-to-home-screen promoted enough to count as an adoption tier of its own, between raw web and native install?
- BD data pricing reality: should key web flows ship a zero-rated/lite mode negotiated with local carriers?

---

## 12. One-Line Business Case

> **App-less Access is the reach layer of Atrium Guest — every flow one scan or one link away for every guest, no install required — turning feature adoption from a fraction of guests into effectively all of them, and thereby multiplying the revenue and labor case of every other feature in the product; the app remains, but as the upgrade for the guests who repay it.**
