# Payments, Folio & Tipping — Feature Deep-Dive

**Product:** Atrium Guest (Hotel / Resort Guest-Facing Mobile App)
**Feature area:** G9 — Payments, Folio & Tipping
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-16
**Status:** Draft v1.0
**Related:** [Guest Mobile App — Feature Scope](guest-mobile-app-feature-scope.md) · [Check-In & Check-Out — Deep-Dive](guest-mobile-app-check-in-check-out-feature.md) · [Feedback & Service Recovery — Deep-Dive](guest-mobile-app-feedback-service-recovery-feature.md) · [App-less Access — Deep-Dive](guest-mobile-app-app-less-access-feature.md) · [Staff Front Desk — Deep-Dive](../../staff/docs/staff-mobile-app-front-desk-feature.md) · [Competitive Feature Research](../../HOTEL-~1.MD)

---

## 1. Executive Summary

**Payments, Folio & Tipping** puts the guest's money relationship with the property in their own hands: a **live, itemized folio** they can check any time, the ability to **pay a deposit, an interim balance, or the final bill** from the phone or a payment link, **receipts** emailed and split cleanly, **cashless tipping** by QR code to a person or a department, and a **dispute-a-charge flow** that turns a billing question into a service-recovery case instead of a checkout argument.

The single most important design decision in this feature is the payment rails. International guest apps assume Visa and Mastercard; Bangladeshi guests hold **bKash, Nagad, and bank accounts behind SSLCommerz**. Atrium Guest supports **local rails as first-class citizens alongside international cards** — the pattern only local vendors (Smart Software, Mediasoft) serve today, and none of them with a guest-facing app. This is the decisive BD differentiator of the whole payments feature: a folio a guest can *see* but not *pay with the money they actually have* is only half a feature.

The rest of the pattern is well precedented globally: pay-by-link (Cloudbeds, RoomRaccoon), integrated contactless payment during online check-in/out (Mews), and app-less QR digital tipping by individual or department (Canary). Atrium combines them behind one folio, tokenizes saved payment methods so card data never touches Atrium systems (PCI scope stays with the certified gateway), and routes every disputed line to the staff-side [F14 service-recovery workflow](../../staff/docs/staff-mobile-app-guest-feedback-service-recovery-feature.md).

The business case: **bill surprises are the classic checkout satisfaction killer and dispute generator** — a transparent live folio prevents them; frictionless payment on the rails guests actually hold protects settlement and speeds departure; and digital tipping keeps a motivating staff income stream alive as cash disappears from guests' pockets.

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What the guest can do from the device |
|---|------------|---------------------------------------|
| 1 | **Live itemized folio** | See the running bill in real time — every room-night, order, spa booking, and fee as a line item with outlet, time, and amount — no surprises held back for checkout. |
| 2 | **Pay deposit / interim / final in-app** | Settle any part of the balance at any point in the stay: the booking deposit, a mid-stay partial payment, or the full folio at express checkout. |
| 3 | **Pay-by-link** | Receive a secure payment link (in chat, email, WhatsApp, or SMS) and pay without the app — for deposits, pre-arrival balances, or app-less settlement. |
| 4 | **Local rails + international cards** | Pay with **bKash, Nagad, or any method behind SSLCommerz** — as prominently as Visa/Mastercard/Amex. The rails Bangladeshi guests actually hold, first-class. |
| 5 | **Saved methods & tokenization** | Save a payment method once and reuse it across the stay and future stays — stored as gateway tokens, never as card data on Atrium systems (PCI scope stays with the certified processor). |
| 6 | **Receipts — email & split** | Get the receipt emailed instantly; split the folio (room vs. extras, between companions, personal vs. company) into separate receipts for expense claims. |
| 7 | **Digital tipping via QR** | Tip a specific staff member or a department by scanning a QR — app-less, cashless, card or bKash/Nagad — at any moment of the journey. |
| 8 | **Dispute-a-charge** | Question any folio line in two taps; the dispute routes to the duty manager's [F14 service-recovery queue](../../staff/docs/staff-mobile-app-guest-feedback-service-recovery-feature.md) and the guest tracks its resolution in-app. |
| 9 | **Split & group billing** | Split the folio across companions or rooms — equal split, by-item claim, or organizer-pays-all — with each member settling their share from their own phone by payment link, and a consolidated group invoice for the organizer. |

### 2.2 In scope (this release)
Real-time folio view synced from the PMS with line-level detail; in-app payment of deposit, interim, and final balances; generated pay-by-link deliverable over email/WhatsApp/SMS/chat; gateway integrations for bKash, Nagad, SSLCommerz, and an international card acquirer; tokenized saved payment methods; instant e-receipts with folio split (by line, by percentage, by payer); split & group billing for group and family stays — equal split, by-item claim, or organizer-pays-all, with per-person payment links, organizer spend controls set in [G13](guest-mobile-app-loyalty-guest-profile-feature.md), and a consolidated group invoice; QR digital tipping to individuals and departments with property-configured distribution policy and payout reporting; dispute-a-charge with F14 routing and in-app status; folio-ready notification via [G8](guest-mobile-app-notifications-campaigns-feature.md); all payment flows available app-less via [G15](guest-mobile-app-app-less-access-feature.md).

### 2.3 Out of scope (this release)
Currency exchange and dynamic currency conversion; buy-now-pay-later and installment products; corporate/city-ledger billing workflows (stays back-office); loyalty-point payment (redemption lives in [G13](guest-mobile-app-loyalty-guest-profile-feature.md) and posts as folio credits); staff-side payment capture (that is [F1](../../staff/docs/staff-mobile-app-front-desk-feature.md)); payroll integration of tip payouts (reported to the property's payroll process, not executed by Atrium); crypto.

### 2.4 Key assumptions
- The PMS exposes the folio in real time (reads) and accepts payment postings (writes), or the middleware sync layer bridges both.
- Merchant accounts exist per property for SSLCommerz (or direct bKash/Nagad merchant integration) and an international card acquirer; Atrium integrates as a facilitator, not the merchant of record.
- All card handling occurs in gateway-hosted fields/pages — Atrium stores tokens only, keeping Atrium out of PCI DSS scope beyond SAQ-A-level obligations.
- The property defines a tipping policy (who may receive, department pools, distribution rules) and owns local tax/labor treatment of tips.

---

## 3. Capability Deep-Dive — The Payments, Folio & Tipping Features

Each capability is elaborated — **what it is**, **how it works in Atrium**, **why it matters**, and the **revenue/cost angle** — with vendor precedents noting the bar to clear.

### 3.1 Live itemized folio
*Precedent: Mews / Shiji-pattern guest folio access during online check-out; standard in best-in-class digital guest journeys.*

**What it is.** The guest's bill as a living document: every charge appears as it posts — the room-night at audit, the poolside order minutes after delivery, the spa booking when confirmed — itemized with outlet, timestamp, and amount.

**How it works in Atrium.** The folio view reads the PMS folio in real time through the same integration the staff app uses. Charges from [G5 orders](guest-mobile-app-food-beverage-ordering-feature.md) and [G12 bookings](guest-mobile-app-activities-spa-experiences-feature.md) appear within moments of posting. Each line is tappable for detail — and carries the dispute affordance (§3.8). A running total and any pre-authorization are always visible.

**Why it matters.** Almost every checkout dispute begins the same way: the guest sees the bill for the first time at the moment they are trying to leave. A folio the guest has watched all stay contains no surprises — errors get caught the day they post, in one tap, instead of at the desk with a queue behind them.

**Revenue / cost angle.** Guests who trust the bill charge more to the room — folio transparency underwrites the charge-to-room convenience every ancillary feature depends on (revenue); catching posting errors mid-stay is minutes of correction, versus the desk-time, comps, and chargebacks of checkout disputes (cost).

### 3.2 Pay deposit, interim, or final — in-app
*Precedent: Mews, RoomRaccoon (integrated & contactless payment during online check-in/out).*

**What it is.** Settlement whenever the guest wants it: the deposit at booking ([G1](guest-mobile-app-booking-pre-arrival-feature.md)), a partial payment mid-stay (keeping a long-stay balance manageable, or splitting spend between companions over time), and the full folio at express checkout ([G2](guest-mobile-app-check-in-check-out-feature.md)) — without visiting the desk.

**How it works in Atrium.** From the folio, **Pay now** offers full or partial amounts against any supported method (§3.4) or a saved token (§3.5). Payments post to the PMS folio instantly and are reflected on both guest and [staff F1](../../staff/docs/staff-mobile-app-front-desk-feature.md) surfaces. The night before departure, a folio-ready nudge ([G8](guest-mobile-app-notifications-campaigns-feature.md)) invites review and settlement; checkout morning becomes a confirmation, not a transaction.

**Why it matters.** The checkout crush is a payment queue. Moving settlement to the guest's own device — at 9pm on the sofa, not 9am in the line — dissolves it. Mid-stay payment also matters for the BD market specifically: mobile-wallet transaction limits can make one large final payment impossible where several smaller ones are fine.

**Revenue / cost angle.** Pre-settled folios protect revenue that walk-outs, disputed balances, and failed cards at departure otherwise leak (revenue protection); every in-app settlement is a desk transaction that never happens — the express-checkout labor case (cost).

### 3.3 Pay-by-link
*Precedent: Cloudbeds, RoomRaccoon (guests pay online from a link).*

**What it is.** A secure hosted payment page reached from a link — sent in the [G7 chat](guest-mobile-app-guest-messaging-feature.md), a WhatsApp message, an SMS, or an email — that takes payment from anyone, app or no app.

**How it works in Atrium.** Staff (from F1) or automated flows (deposit due, folio ready) generate a link scoped to a specific amount and folio, delivered on the guest's channel. The link opens a gateway-hosted payment page offering the full method set (§3.4); on success the payment posts to the folio and both sides get confirmation. Links expire and are single-use ([G15 security model](guest-mobile-app-app-less-access-feature.md)).

**Why it matters.** Pay-by-link is the universal adapter of the payments feature: it serves the guest who never installed anything, the booker paying for someone else's stay (a parent, a company), the pre-arrival deposit chase, and the post-departure balance — all cases where "open the app" is the wrong answer.

**Revenue / cost angle.** Converts balances that phone-and-ask collection loses — deposit no-shows, post-stay balances, third-party payers (revenue protection); ends the manual card-over-the-phone handling that is both labor-intensive and a PCI hazard (cost + risk).

### 3.4 Local rails — bKash, Nagad, SSLCommerz — alongside international cards
*Precedent: Smart Software, Mediasoft (BD local gateway integration) — but staff-side only; no BD vendor pairs local rails with a guest app.*

**What it is.** The decisive BD differentiator of this feature: **the payment methods Bangladeshi guests actually hold, treated as first-class** — bKash and Nagad wallets and the full local card/banking spread behind SSLCommerz — presented alongside (not behind) Visa, Mastercard, and Amex for international guests.

**How it works in Atrium.** Every payment surface in the app — deposit, interim, final, pay-by-link, tipping — presents the method set appropriate to the guest, with local wallets given equal or leading placement for domestic guests. Wallet flows follow each rail's native UX (bKash/Nagad app handoff or USSD-style PIN confirmation via the gateway); settlement and reconciliation flow into the property's existing merchant reporting. Method preference is remembered per guest.

**Why it matters.** This is where global vendors structurally cannot follow quickly: Mews, Canary, and Duve ship card-first stacks built on Western acquirers. In the domestic market that Atrium's target properties largely serve, a card-only guest app is unusable at the moment of payment for a large share of guests — which silently kills the express checkout, in-app ordering with direct payment, and tipping cases too. Local rails are not a checkbox; they are the condition for the whole feature working in this market.

**Revenue / cost angle.** Every flow in this document converts at the rate its payment step permits — local rails raise that rate for the majority segment of guests (revenue, multiplicative); wallet transactions also displace cash handling, with its counting, float, shrinkage, and reconciliation costs (cost).

### 3.5 Saved payment methods & tokenization
*Precedent: Mews (integrated payments, saved card on profile); universal e-commerce pattern.*

**What it is.** Pay once, keep the method: card and (where the rail supports it) wallet credentials saved for one-tap reuse across the stay and on return visits — stored as **gateway tokens**, never as raw card data anywhere in Atrium.

**How it works in Atrium.** Card entry happens exclusively in gateway-hosted fields; Atrium receives and stores only the token and display metadata (brand, last four). Tokens attach to the [G13 guest profile](guest-mobile-app-loyalty-guest-profile-feature.md), so the returning loyalty guest pays a deposit next season in one tap. **PCI note:** because no cardholder data transits or rests on Atrium systems, Atrium's PCI DSS burden stays at the SAQ-A level — the certified gateway and acquirer carry the heavy compliance scope.

**Why it matters.** Re-entering a 16-digit card on a phone is exactly the kind of friction that abandons a mid-stay payment or an impulse spa booking. Tokenized one-tap payment makes the folio-settlement and charge-to-alternatives cases actually fluid — and it is what makes repeat-guest payment feel like recognition.

**Revenue / cost angle.** One-tap payment lifts completion on every payment flow, especially small impulse transactions (revenue); keeping Atrium out of PCI scope avoids an audit and engineering burden that would otherwise dwarf the feature's build cost (cost avoidance).

### 3.6 Receipts — instant email & folio split
*Precedent: express-checkout receipt emailing (Maestro, Canary, Mews-pattern); split-folio is standard PMS practice surfaced guest-side.*

**What it is.** Documentation without the desk: the receipt in the guest's inbox the moment payment completes, and the ability to split the folio — room to the company card, extras to a personal wallet; or a shared villa's bill divided among friends — each split settled and receipted separately.

**How it works in Atrium.** Split is defined guest-side (by line selection, percentage, or payer) within property-configured rules, mirrored to the PMS as standard folio splits so night audit sees nothing exotic. Each sub-folio can be paid by a different method — one on a corporate Visa, one on bKash — and receipts (VAT-compliant, per BD requirements) issue per payer.

**Why it matters.** Business travelers live and die by clean receipts — a mixed personal/company folio is an expense-report headache that sours an otherwise good stay. Groups splitting a stay currently do painful desk arithmetic at checkout. Both become self-service.

**Revenue / cost angle.** Clean expense documentation is a rebooking factor for the business segment (revenue); guest-side splitting removes one of the slowest, most error-prone desk transactions that exists — the group-checkout bill division (cost).

### 3.7 Digital tipping via QR — to a person or a department
*Precedent: Canary Technologies (app-less QR digital tipping by individual or department across the journey).*

**What it is.** Cashless gratitude: the guest scans a QR — on the housekeeping card, the bellman's badge, the table tent, the checkout screen — picks or confirms the recipient (a named staff member or a department pool), picks an amount, and pays in seconds. **App-less by design** (a [G15](guest-mobile-app-app-less-access-feature.md) web flow), because tipping moments don't wait for installs.

**How it works in Atrium.** Each QR encodes a recipient scope (person, team, department). The web flow shows the recipient (name/photo for individuals, where the property enables it), suggested amounts, and the full method set — including bKash/Nagad, which for domestic guests makes tipping as fast as any wallet transfer they make daily. Funds flow to the property's tip account; Atrium tracks attribution and produces the distribution report the property pays out against (per its policy). Staff see their tips recognized; managers see department patterns.

**Why it matters.** Guests carry less cash every year — in urban Bangladesh, wallet payment is displacing cash at speed — and tips disappear with it. That is a real income loss for housekeepers, bellmen, and servers, and a motivation loss the property feels in service quality and retention. Digital tipping restores the stream at the exact moments gratitude occurs, from the money guests actually carry: their phones.

**Revenue / cost angle.** Tips are staff income, not property revenue — but tip-earning staff stay longer and serve warmer, and service warmth is the review-score input that drives rate and rebooking (indirect revenue); documented digital tips also displace the disputes and leakage of informal cash pooling (cost + fairness).

### 3.8 Dispute-a-charge
*Precedent: guest-side surfacing of the service-recovery pattern (Atrium Staff F14); no clean vendor precedent — a differentiator.*

**What it is.** A civilized path for "what is this charge?": two taps on any folio line open a dispute — reason, optional note — that routes straight into the duty manager's [F14 service-recovery queue](../../staff/docs/staff-mobile-app-guest-feedback-service-recovery-feature.md), with status the guest can track in the folio.

**How it works in Atrium.** A disputed line is flagged on both guest and staff views (frozen from settlement pressure but not hidden). The duty manager investigates in F14 — checking the POS ticket, the order trail, the signature — and resolves: adjust, explain, or partially credit, with the resolution and any adjustment appearing in the guest's folio and a note in the thread. Dispute-rate analytics per outlet feed management ([F11-pattern KPIs](../../staff/docs/staff-mobile-app-management-analytics-owner-kpis-feature.md)).

**Why it matters.** Today the dispute channel is an argument at the desk at the worst possible moment, or a chargeback weeks later — the most expensive way a property can learn about a posting error. In-stay, in-app dispute converts a flashpoint into a workflow: the guest feels heard immediately, and the property fixes the error while the evidence is fresh.

**Revenue / cost angle.** Every in-app dispute resolved is a potential chargeback (with its fees, penalties, and lost revenue) and a potential angry review avoided (revenue protection); structured disputes with evidence trails resolve in minutes, versus the desk escalations and blanket comps of checkout arguments (cost).

### 3.9 Split & group billing
*Precedent: split-pay patterns proven at scale in consumer fintech; hotel-specific split billing is rare enough that this is an opening, not a bar to clear.*

**What it is.** The receipt-split machinery of §3.6, extended into full group settlement: the folio of a group or family stay divided across companions or rooms — **equal split**, **by-item claim** (each member claims their own lines), or **organizer-pays-all** — with each member settling their share from their own phone, and the organizer receiving one consolidated group invoice covering the whole party.

**How it works in Atrium.** The organizer — defined with the traveling group in the [G13 guest profile](guest-mobile-app-loyalty-guest-profile-feature.md) — picks the split mode at any point in the stay. Each member gets a per-person payment link (§3.3) scoped to their share, payable on the full method set including bKash/Nagad/SSLCommerz (§3.4) — app-less via [G15](guest-mobile-app-app-less-access-feature.md), so a companion who never installed anything still settles from their own phone. Upstream, the organizer's **spend controls and per-companion charge limits** (set in G13) govern what each member may post to the folio in the first place. Lines no one claims fall back to the organizer's share by rule, flagged visibly before departure day; splits mirror to the PMS as standard sub-folios (§3.6), so night audit sees nothing exotic.

**Why it matters.** The group checkout is the slowest transaction a front desk runs: one folio, five payers, desk arithmetic, and one person reluctantly fronting it all. Multi-room family and group stays are the resort norm, yet almost no guest app lets the party settle itself — the split happens at the desk or not at all.

**Revenue / cost angle.** Group folios settle faster and more completely — per-person links on local rails convert shares that one large card transaction (or a wallet over its limit) would fail on, and fewer end-of-stay disputes means fewer comps and chargebacks (revenue protection); removing the checkout-desk bill-splitting scrum eliminates the single most labor-intensive desk transaction, and the consolidated invoice ends the organizer's receipt-chasing (cost).

---

## 4. Why This Matters

### 4.1 The bill is the last impression — and the most fragile one
A flawless stay ends at checkout with a folio the guest has never seen; one wrong minibar line, and the memory of the stay is the argument about it. Folio transparency plus in-app settlement makes the financial ending as smooth as the digital beginning — and the ending is what gets reviewed.

### 4.2 Payment rails decide whether the app works in Bangladesh
Every ancillary flow in Atrium Guest — ordering, booking, upsells, tipping — terminates in a payment step. If that step only speaks Visa, it fails for the domestic majority, and every upstream feature's conversion fails with it. **bKash/Nagad/SSLCommerz as first-class rails is the single most defensible differentiator in this document** — global vendors don't ship it; local vendors have no guest app to put it in.

### 4.3 Charge-to-room runs on folio trust
The convenience economy of a resort — sign it to the room, pay at the end — only works if guests trust the accumulating bill. A live folio is the trust mechanism; without it, guests hesitate on exactly the impulse spending the property most wants.

### 4.4 Cashless is coming for tips, with or without the property
The tip stream shrinks as cash does. Properties that give guests a digital path (Canary made this a product category) protect a staff-income stream that underwrites service quality and retention — properties that don't simply watch it evaporate.

---

## 5. Journey Moments & Operations It Improves

| Moment / operation today | Pain | With Payments, Folio & Tipping |
|--------------------------|------|--------------------------------|
| **Guest wonders what they've spent** | No visibility until checkout; anxiety or overspend regret | Live folio, every line itemized as it posts. |
| **Checkout morning** | Payment queue at the desk; missed breakfasts and transport | Folio settled from the sofa the night before; checkout is a confirmation. |
| **Wrong charge discovered at checkout** | Argument at the desk, queue behind, comp given under pressure | Disputed the day it posted; resolved through F14 before departure day. |
| **Domestic guest reaches payment** | Card-only flow fails; guest walks to the desk to pay cash | bKash/Nagad/SSLCommerz first-class in every flow. |
| **Deposit collection pre-arrival** | Phone calls, bank-transfer instructions, card details read aloud | Pay-by-link on WhatsApp; paid in a minute, posted automatically. |
| **Company-vs-personal expenses** | Desk manually splits folio at checkout, errors common | Guest self-splits; each sub-folio paid its own way, receipts per payer. |
| **Guest wants to tip but has no cash** | The intention dies; staff income shrinks | QR tip in seconds — to the housekeeper by name or the team — on any wallet. |
| **Group settles a shared villa** | Painful desk arithmetic, one person fronts it all | Split by line or share, each pays their part in-app. |
| **Post-departure unpaid balance** | Chase by phone; often written off | Pay-by-link with the final receipt; settled remotely. |

---

## 6. How It Increases Revenue

1. **Payment friction removed at every conversion point.** Orders (G5), bookings (G12), upsells (G6), and deposits (G1) all complete at the rate the payment step permits. Local rails plus one-tap tokens raise that completion rate across the board — the payments feature is a multiplier on the whole app's revenue stack.
2. **Charge-to-room confidence lifts ancillary spend.** Guests who can watch the folio charge to the room freely; opaque bills make guests defensive spenders.
3. **Settlement leakage recovered.** Pre-settled folios, pay-by-link on outstanding balances, and mid-stay interim payments recover the walk-outs, failed-card departures, and written-off post-stay balances that quietly leak revenue today.
4. **Chargebacks and comps avoided.** In-stay dispute resolution intercepts the postings that otherwise return as chargebacks (with fees and penalties) or get comped under checkout pressure.
5. **Service quality via the tip stream.** Digital tipping sustains the staff income that correlates with warmth, retention, and the review scores that drive rate power and direct bookings.
6. **Business-segment rebooking.** Clean splits and instant VAT-compliant receipts make the property easy to expense — a real selection factor for the corporate repeat segment.

> **Illustrative model:** A 150-room property at 75% occupancy turning ~110 departures/day: if in-app settlement recovers just 1% of folio value currently lost to disputes, comps, and unpaid balances on an average ৳12,000 folio, that is ~৳13,000/day ≈ **৳4.8M/year** protected — before counting the conversion lift local rails give every ancillary purchase, which is plausibly larger. *(Figures illustrative; validate against property data.)*

---

## 7. How It Reduces Operational Cost

1. **Checkout desk labor collapses.** Every self-settled folio is a desk transaction that never happens; the morning crush stops driving front-desk staffing — the same labor case as [F1](../../staff/docs/staff-mobile-app-front-desk-feature.md), attacked from the guest side.
2. **Dispute handling moves from argument to workflow.** Structured in-app disputes with evidence trails resolve in minutes in F14, replacing desk escalations, manager call-downs, and pressure comps.
3. **Cash handling shrinks.** Wallet and card settlement displaces cash — with its counting, float management, deposit runs, shrinkage, and reconciliation labor.
4. **No more card-over-the-phone.** Pay-by-link eliminates manual card capture for deposits and remote balances — labor-intensive today and a PCI/fraud hazard besides.
5. **Group and split checkouts self-serve.** The slowest desk transaction of all — dividing a shared bill — becomes guest-side self-service mirrored cleanly to the PMS.
6. **Tip administration formalized.** QR tips with attribution and distribution reports replace informal cash pools and the disputes they generate.
7. **PCI scope stays minimal.** Tokenization keeps cardholder data off Atrium and property systems, avoiding the audit and remediation costs of expanded PCI scope.

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Folio transparency | In-house stays viewing the folio at least once | ≥ 60% |
| Self-settlement | Departures settled in-app / by link (no desk payment) | ≥ 40% |
| Local-rail share | Domestic-guest payments completed on bKash/Nagad/SSLCommerz | ≥ 50% of domestic volume |
| Payment completion | Started-to-completed rate on in-app payment flows | ≥ 90% |
| Checkout speed | Median departure transaction time (settled guests) | < 1 minute |
| Dispute conversion | Folio disputes raised in-app vs. at desk/chargeback | ≥ 70% in-app |
| Chargeback rate | Chargebacks per 1,000 stays | ↓ 50% vs. baseline |
| Tipping adoption | Stays with ≥ 1 digital tip | ↑ measurable; ৳ volume tracked per department |
| Receipt automation | Receipts issued without desk involvement | ≥ 95% |
| Group settlement | Share of group-stay folios settled via split-pay | ≥ 30% *(illustrative)* |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| PMS folio API | Real-time folio reads and payment writes required | Same middleware layer as staff F1; read-only degradation with pay-at-desk fallback |
| Gateway integrations | bKash, Nagad, SSLCommerz, and card acquirer — four integrations, different UX models | SSLCommerz aggregation covers most local methods in one integration; direct wallet integrations phased |
| PCI scope | Any card-data touch expands compliance burden | Gateway-hosted fields/pages only; tokens only at rest; SAQ-A posture validated pre-launch |
| Reconciliation | Guest-app payments must reconcile with desk payments in night audit | Unified settlement reporting with the F1 payment stream; per-gateway settlement reports mapped to folios |
| Wallet limits & failures | bKash/Nagad per-transaction and daily limits can block large folios | Interim/partial payment support; automatic suggestion to split large settlements; card fallback |
| Tipping policy & tax | Tip distribution, payroll treatment, and tax vary by property | Property-configured policy engine; Atrium reports, property pays out; local-counsel guidance in onboarding |
| Fraud on links | Payment links forwarded or intercepted | Scoped single-use expiring links (G15 token model); amount locked; gateway 3-DS/OTP where available |
| Dispute misuse | Frivolous disputes freezing folio lines | Dispute ≠ automatic credit — F14 adjudicates; per-guest dispute-rate visibility |
| Refund latency on wallets | Wallet refund rails slower than card voids | Set guest expectations in-flow; folio credit as immediate remedy, rail refund follows |
| Unclaimed split items | Lines no companion claims sit unsettled at checkout | Fall back to the organizer's share by rule; flagged visibly to organizer before departure day; organizer-pays-all as the default mode |

---

## 10. Scope delivered

Atrium Guest ships as one complete package. This feature delivers:

- Live itemized folio synced in real time from the PMS, with line-level detail
- In-app payment of deposits, interim balances, and final settlement
- Pay-by-link over email, WhatsApp, SMS, and chat — single-use, amount-scoped, expiring
- bKash, Nagad, and SSLCommerz as first-class rails alongside international cards, in every payment flow
- Tokenized saved payment methods on the guest profile (gateway-held; SAQ-A posture)
- Instant e-receipts (VAT-compliant) with guest-side folio split by line, share, or payer
- QR digital tipping to individuals and departments — app-less web flow, wallet and card rails, attribution and distribution reporting
- Dispute-a-charge on any folio line, routed to staff F14 with in-app status tracking
- Split & group billing: equal split, by-item claim, or organizer-pays-all across companions and rooms, per-person payment links on local rails, organizer spend controls and per-companion charge limits (set in G13), and a consolidated group invoice
- Folio-ready and payment-confirmation notifications via G8
- Full app-less availability of folio view, payment, and tipping via G15

---

## 11. Open Questions

- Merchant-of-record model per property: direct property merchant accounts (Atrium as technical facilitator) confirmed, or is a platform-aggregated model needed for small properties?
- SSLCommerz-only at launch (covering bKash/Nagad indirectly) vs. direct bKash/Nagad merchant integrations for better UX and rates — which ships v1?
- Which international acquirer/gateway for card volume, and is 3-D Secure mandated on all card transactions?
- Tip flows: are individual-recipient tips permitted at launch, or department-pool only until payout policy matures?
- VAT receipt format requirements — is the PMS the invoice of record, or does Atrium generate compliant receipts directly?
- Does dispute-a-charge freeze the line from settlement until resolved, or settle-then-adjust?
- Post-departure folio access duration — how long does the guest retain receipt/folio history in-app?

---

## 12. One-Line Business Case

> **Payments, Folio & Tipping makes the money side of the stay transparent and effortless — a live bill with no checkout surprises, settled in seconds on the rails Bangladeshi guests actually hold, split and receipted cleanly, disputes resolved before they become chargebacks, and tips flowing again in a cashless world — protecting settlement, checkout speed, and the last impression that gets reviewed.**
