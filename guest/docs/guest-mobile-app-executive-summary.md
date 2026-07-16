# Atrium Guest — Executive Summary

**Product:** Atrium Guest (Hotel / Resort Guest-Facing Mobile App)
**Prepared for:** Leadership / Stakeholders
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-16
**Status:** Draft v1.0
**Start here:** [Project Overview & Documentation Index](../overview.md)
**Supporting detail:** [Feature Scope](guest-mobile-app-feature-scope.md) · [Prioritization](guest-mobile-app-feature-prioritization.md) · [Navigation Map](guest-mobile-app-navigation-map.md) · [Journey-Stage Design](guest-mobile-app-journey-stage-design.md) · [Competitive Research](../../HOTEL-~1.MD) *(draft guideline only, not app scope)*

> The per-feature deep-dive docs are linked from the feature table in §3 below. Atrium Guest is the companion surface to **Atrium Staff** — every guest action here lands as a task, order, or booking there ([staff docs](../../staff/docs/staff-mobile-app-executive-summary.md)).

---

## 1. The opportunity in one paragraph

Guests now expect to book, check in, unlock the door, order dinner, ask for towels, and settle the bill **from their own phone** — the pattern global leaders (Mews, Canary, Duve, INTELITY, Shiji, Nonius) have made baseline. **Atrium Guest** is the guest-facing companion to Atrium Staff: one app (and, critically, one set of **app-less QR/web flows**) covering the whole journey from booking to review. It attacks the three numbers that decide a property's economics: **direct revenue** (bookings without OTA commission, upsells, F&B and activity spend), **guest satisfaction** (no queues, no phone-tag, no bill surprises), and **operating cost** (self-service deflects the desk's call and queue load). Critically for our target market, **almost no Bangladeshi vendor ships a real guest-facing app** — the competitive research finds local players strong on staff-side ERP and localization but essentially absent on the guest side. That is a wide-open local lane: Atrium Guest meets the global standard *and* speaks bKash, Nagad, SSLCommerz, and Bangla — which no global vendor does.

---

## 2. Why now

- **Guests won't call, but they will tap.** Messaging, self-service, and contactless flows are the expected default across every demographic and segment.
- **The revenue is mobile.** Upsells convert at the moments a fixed desk sells worst — pre-arrival and mid-stay; vendors report uplifts up to ~250% from timed, personalized offers (Canary, Duve, Revinate).
- **OTA commission is the largest recoverable leak.** A direct mobile booking path plus loyalty gives guests a reason to skip the OTA next time.
- **Atrium Staff needs a demand side.** The staff app routes and fulfills work; the guest app is where that work — orders, requests, bookings — originates without a human intermediary.
- **The local gap is wide open.** Of the Bangladeshi vendors reviewed, essentially none offers a genuine guest-facing app ([Competitive Research](../../HOTEL-~1.MD)). First-mover advantage is available now.

---

## 3. The product: fifteen feature areas, one guest journey

The app is **journey-stage adaptive**: each stage (Discover & Book → Pre-Arrival → Arrival → In-Stay → Departure → Post-Stay) surfaces only what is relevant now. Full matrix in the [README](../README.md) and [Journey-Stage Design](guest-mobile-app-journey-stage-design.md).

| # | Feature area | What it does | Primary economic lever |
|---|--------------|--------------|------------------------|
| G1 | [Booking & Pre-Arrival](guest-mobile-app-booking-pre-arrival-feature.md) | Mobile booking engine + digital registration, preferences, pre-orders before arrival | OTA commission recovery + desk labor per arrival |
| G2 | [Mobile Check-In & Check-Out](guest-mobile-app-check-in-check-out-feature.md) | Contactless arrival and express departure from the guest's device | Queue elimination + desk labor |
| G3 | [Digital Room Key](guest-mobile-app-digital-room-key-feature.md) | Phone as room key via BLE lock integrations, wallet keys, key sharing | Card stock/encoder cost + contactless completion |
| G4 | [In-Stay Service Requests](guest-mobile-app-in-stay-service-requests-feature.md) | Structured requests with photos, DND, live status, transport & mobility (transfers, shuttle, valet), one-tap emergency assistance — auto-routed to staff | Call deflection + satisfaction |
| G5 | [Food & Beverage Ordering](guest-mobile-app-food-beverage-ordering-feature.md) | Order from any outlet to room/pool/table, charge-to-folio | The biggest ancillary line — order volume + ticket |
| G6 | [Upsells, Offers & Add-Ons](guest-mobile-app-upsells-offers-feature.md) | Timed, targeted upgrades, packages, experiences at booking/arrival/in-stay | Near-zero-labor incremental revenue |
| G7 | [Guest Messaging](guest-mobile-app-guest-messaging-feature.md) | One two-way thread plus a shared group thread, auto-translated, WhatsApp/SMS-reachable | Problem interception + desk deflection |
| G8 | [Notifications & Campaigns](guest-mobile-app-notifications-campaigns-feature.md) | Journey pushes (room ready, folio ready) + targeted campaigns | Perishable-inventory fill (tonight's tables, spa slots) |
| G9 | [Payments, Folio & Tipping](guest-mobile-app-payments-folio-tipping-feature.md) | Live folio, pay in-app or by link, split & group billing, bKash/Nagad/SSLCommerz, QR tipping | Dispute prevention + local-rails capture |
| G10 | [Digital Guidebook & Property Info](guest-mobile-app-digital-guidebook-property-info-feature.md) | Wi-Fi, hours, house rules, transport & prayer times, local tips — offline-capable | Zero-marginal-cost call deflection |
| G11 | [Interactive Property Map](guest-mobile-app-interactive-property-map-feature.md) | Tap-able resort map with live status, linking straight into ordering/booking | Discovery → folio revenue on large properties |
| G12 | [Activities, Spa & Experiences](guest-mobile-app-activities-spa-experiences-feature.md) | Self-service booking with live capacity, cabana/sunbed booking, virtual queues + personal stay itinerary | High-margin, perishable slot fill |
| G13 | [Loyalty & Guest Profile](guest-mobile-app-loyalty-guest-profile-feature.md) | Preferences, history, points, tiers, in-app redemption + group & companion management | Repeat direct bookings — the most profitable revenue |
| G14 | [Feedback & Service Recovery](guest-mobile-app-feedback-service-recovery-feature.md) | In-stay pulse ratings + "something wrong?" channel, routed to recovery | Detractor conversion + review scores |
| G15 | [App-less Access — QR & Web Links](guest-mobile-app-app-less-access-feature.md) | Every flow reachable by link/QR, no install — *cross-cutting enabler, not a screen* | Takes adoption from the few to every guest |

**One platform, two surfaces.** Every guest action lands in Atrium Staff: G4 requests → staff [Task Management (F5)](../../staff/docs/staff-mobile-app-task-management-communication-feature.md) · G5 orders → staff [POS (F8)](../../staff/docs/staff-mobile-app-pos-multi-outlet-feature.md) · G12 bookings → staff [Events & Activities (F18)](../../staff/docs/staff-mobile-app-events-activities-feature.md) · G14 signals → staff [Service Recovery (F14)](../../staff/docs/staff-mobile-app-guest-feedback-service-recovery-feature.md) · G2 check-ins → staff [Front Desk (F1)](../../staff/docs/staff-mobile-app-front-desk-feature.md) · G1 bookings → staff [Reservations (F2)](../../staff/docs/staff-mobile-app-reservations-management-feature.md). The guest app generates demand; the staff app fulfills it — neither is complete alone.

---

## 4. The business case, rolled up

### 4.1 How it increases revenue

- **Direct bookings without commission** (G1, G13) — a mobile booking engine plus visible loyalty gives guests a reason to skip the OTA; every shifted booking saves 15–25% commission.
- **Upsell at the moments of highest intent** (G6) — pre-arrival, check-in, and mid-stay offers convert where a fixed desk cannot; the vendor evidence (Canary, Duve, Revinate) is that timing + personalization is the multiplier.
- **Menu-in-hand F&B capture** (G5) — every barrier removed (finding a phone, holding, language) is spend recovered; charge-to-folio raises both order count and average ticket.
- **Perishable inventory filled** (G8, G11, G12) — tonight's tables, tomorrow's spa slots, and idle activity capacity get sold by push campaigns, map discovery, and self-service booking instead of expiring unsold.
- **Bill-to-review protection** (G9, G14) — a transparent live folio prevents the disputes, and in-stay recovery converts the detractors, that otherwise depress the review scores driving future bookings.

### 4.2 How it reduces operating cost

**The guest app's cost story is deflection: work guests do themselves is work staff never touch.**

- **Fewer desk-minutes per stay** — self-service check-in/out (G2), pre-arrival registration (G1), and self-answered questions (G10) remove queue staffing and form-filling.
- **Call volume collapses** — structured requests (G4), one message thread (G7), and the guidebook (G10) deflect the "Wi-Fi password / extra towel / what time does…" traffic that consumes front-desk hours daily.
- **Order-taking labor removed** — G5 orders land straight in the staff POS with no phone relay, no mishears, no re-entry.
- **No key hardware cycle** — digital keys (G3) eliminate card stock, encoders, and re-key desk trips.
- **Auto-translation replaces bilingual staffing pressure** (G7) — guest and staff each write in their own language.

### 4.3 Illustrative economics (single 150-room resort)

*Figures illustrative — validate against real property data before external use. ৳ = BDT.*

| Lever | Illustrative annual impact |
|-------|----------------------------|
| OTA → direct shift (G1, G13) | ~৳35–40 lakh commission recovered (10-pt direct shift × ~15% commission on ~৳25 crore room revenue) |
| Upsells & add-ons (G6) | ~৳70+ lakh incremental (8% attach × ৳2,500 avg × ~100 arrivals/day) |
| F&B in-app lift (G5) | +5–10% covers and check average on app/QR orders, compounding daily |
| Activity/spa fill (G12) | High-margin slot fill on capacity that phone-only booking leaves empty |
| Desk labor & call deflection (G2, G4, G10) | Recurring labor savings — fewer desk FTE-hours per arrival, fewer calls handled |

The point is not the precise numbers — it is that **each area targets a distinct, recurring lever**, and they stack on top of the Atrium Staff savings already modeled ([staff summary §4.3](../../staff/docs/staff-mobile-app-executive-summary.md)).

### 4.4 Per-feature summary — cost reduced + owner revenue increased

Every feature, on both dimensions the owner cares about.

| # | Feature | How it reduces operational cost | How it increases owner revenue |
|---|---------|---------------------------------|--------------------------------|
| **G1** | Booking & Pre-Arrival | Pre-filled registration ends desk form-filling; arrival details let housekeeping plan | Direct bookings recover OTA commission; pre-orders start spend before arrival |
| **G2** | Check-In & Check-Out | Self-service arrivals/departures cut desk-queue staffing and the checkout crush | Frees staff for high-touch guests; enables paid early check-in / late check-out at scale |
| **G3** | Digital Room Key | Eliminates card stock, encoders, demagnetized-card desk trips | Completes the contactless journey that makes G2 (and its upsell moments) actually used |
| **G4** | In-Stay Service Requests | Structured, auto-routed requests end phone relay and lost tickets | Satisfaction and review scores — the strongest indirect revenue driver |
| **G5** | F&B Ordering | Orders land in staff POS with zero order-taking labor or mishears | The biggest ancillary line: more orders, higher tickets, all-property reach |
| **G6** | Upsells & Offers | Near-zero marginal labor — offers sell themselves at the right moment | The app's payback feature: upgrades, late check-out, packages, experiences |
| **G7** | Guest Messaging | Deflects routine questions; auto-translation removes language handling cost | Catches problems while fixable — protects reviews and repeat stays |
| **G8** | Notifications & Campaigns | Replaces manual outreach (calls, printed flyers) with targeted push | Fills perishable inventory: tonight's music, tomorrow's spa slots |
| **G9** | Payments, Folio & Tipping | Live folio prevents checkout disputes and the staff time they burn | Local rails (bKash/Nagad/SSLCommerz) capture payments cards miss; tips retain staff |
| **G10** | Guidebook & Property Info | Deflects the "hundred common questions" at zero marginal cost | A property that feels effortless earns better reviews and return visits |
| **G11** | Interactive Property Map | Cuts "how do I get to…" desk traffic | Turns wayfinding into discovery and discovery into folio revenue |
| **G12** | Activities, Spa & Experiences | Self-service booking removes phone-booking labor and double-entry | Fills capacity-bound, high-margin slots; itineraries lift stay value |
| **G13** | Loyalty & Guest Profile | Preferences on file cut repeat data-gathering every stay | Repeat direct bookings — the most profitable revenue a property has |
| **G14** | Feedback & Service Recovery | In-stay saves cost less than checkout comps and refund handling | Converts detractors pre-review; channels promoters to public review scores |
| **G15** | App-less Access (QR/web) | One web codebase serves every flow — no install support burden | The adoption multiplier: without it, every lever above reaches a fraction of guests |

**Cross-cutting multipliers:** *app-less access (G15)* makes every lever reach effectively every guest, not just installers; *local payment rails* capture the payment methods Bangladeshi guests actually hold; *Bangla/English bilingual UI* serves both the domestic leisure market and international arrivals. See §5.

---

## 5. Our differentiation (especially in Bangladesh)

- **A guest app at all.** The competitive research is blunt: almost no Bangladeshi vendor offers a real guest-facing app ([Competitive Research](../../HOTEL-~1.MD)). Locally, shipping this is the differentiation.
- **App-less first (G15).** Most guests will not install an app for a two-night stay; Canary and Duve built category leadership on no-download flows. Every Atrium Guest flow works from a QR code or link — the native app is the upgrade path for resort stays and loyalty members.
- **Local payment rails.** bKash, Nagad, and SSLCommerz across booking deposits, folio settlement, F&B, and tipping — which no global guest app serves.
- **Bangla/English bilingual** guest journey, plus auto-translated messaging (G7) so guest and staff each write in their own language.
- **One platform with Atrium Staff.** Global guest apps bolt onto third-party PMSs; Atrium Guest lands every request, order, and booking natively in the staff app's task, POS, and event workflows — no integration seam, no dropped handoffs.
- **Global-standard, locally-priced** — matches the Mews / Canary / Duve / INTELITY / IDS Next FX Club App feature bar, tuned to the BD market.

---

## 6. One complete package

Atrium Guest ships as **one complete package** — every feature delivered together, not staged — mirroring the Atrium Staff delivery model. The features group into themes that together cover the journey:

| Theme | Focus | Areas |
|-------|-------|-------|
| **Arrival spine** | Book → register → check in → key | **G1 Booking & Pre-Arrival**, **G2 Check-In/Out**, **G3 Digital Key** |
| **In-stay service loop** | Ask, order, track — without the phone | **G4 Service Requests**, **G5 F&B Ordering**, **G7 Messaging** |
| **Revenue engine** | Sell at the right moments | **G6 Upsells & Offers**, **G12 Activities & Spa**, **G8 Notifications & Campaigns** |
| **Money & trust** | Transparent bill, local rails, tips | **G9 Payments, Folio & Tipping** |
| **Self-service knowledge** | Effortless property use | **G10 Guidebook**, **G11 Interactive Map** |
| **Retention** | Come back direct | **G13 Loyalty & Profile**, **G14 Feedback & Recovery** |
| **Cross-cutting enabler** | Reach every guest, installed or not | **G15 App-less Access (QR & web links)** |

Day visitors get a deliberately narrow slice (G5, G9, G11, G12, G14 — no stay, no key, no folio), served almost entirely app-less. A single-property pilot alongside the Atrium Staff pilot validates the closed loop before chain rollout.

---

## 7. Key risks and how we manage them

| Risk | Mitigation |
|------|------------|
| Guest adoption (nobody installs) | **G15 app-less flows as the default path** — QR at booking email, bedside, table; app as upgrade, never a gate |
| Door-lock integration depth (G3) | Certified BLE lock partnerships (ASSA ABLOY/Salto-class); property survey pre-sale; physical key fallback always |
| Payment / PCI + wallet-rail scope | Hosted/tokenized flows; certified local gateways (SSLCommerz, bKash, Nagad); no raw card data in-app |
| Staff-side fulfillment lag makes the app look broken | Guest features gated on the Atrium Staff loop being live — SLA timers and status visibility are shared plumbing |
| Bill/folio sync errors erode trust | Real-time PMS/folio middleware with reconciliation; conservative display on sync doubt |
| Notification fatigue → uninstalls/opt-outs | Stage-targeted, capped, preference-controlled campaigns (G8); journey messages always prioritized over marketing |

---

## 8. The decisions we need

1. **Launch posture** — app-less-first web flows with native app in parallel, or web-first then native? (Recommended: parallel, shared codebase.)
2. **Lock-vendor partnerships** — which BLE lock brands must G3 certify for the pilot property and the BD installed base?
3. **Payment gateways** — confirm SSLCommerz + bKash + Nagad as mandatory v1 rails; which card acquirer?
4. **Booking engine scope** — full direct-booking engine at launch (G1), or pre-arrival-first with booking behind a fast follow?
5. **Loyalty economics** — points earn/burn rates and tier design (G13) need owner sign-off before build.
6. **Pilot property** — same property as the Atrium Staff pilot (recommended — the closed loop is the product) and which guest segments first.

---

## 9. Bottom line

> **Atrium Guest puts the property in the guest's hand — booking, arrival, key, dinner, requests, bill, and tip — and lands every action directly in Atrium Staff's workflows. It recovers OTA commission, sells the moments a desk can't, deflects the desk's daily question load, and — through app-less QR access — reaches effectively every guest, not just the few who install. In a market where no local vendor ships a real guest app, it is a wide-open lane on top of a platform we already own.**
