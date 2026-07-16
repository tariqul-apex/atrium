# Digital Guidebook & Property Info — Feature Deep-Dive

**Product:** Atrium Guest (Hotel / Resort Guest-Facing Mobile App)
**Feature area:** G10 — Digital Guidebook & Property Info
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-16
**Status:** Draft v1.0
**Related:** [Guest Mobile App — Feature Scope](guest-mobile-app-feature-scope.md) · [Competitive Feature Research](../../HOTEL-~1.MD)

---

## 1. Executive Summary

The **Digital Guidebook & Property Info** feature puts the property's answers to every common guest question — the Wi-Fi password, breakfast hours, the pool dress code, the taxi to the airport, the nearest pharmacy — into the guest's pocket: searchable, in Bangla or English, and available even when the resort Wi-Fi drops.

"What's the Wi-Fi password?" and its hundred siblings are the highest-volume, lowest-value interactions a front desk handles. Each costs the guest a call or a walk to the desk, and costs the property a staff interruption — hundreds of times a day, forever. A self-service guidebook answers each question **once, for every guest, at zero marginal cost**, freeing the desk for interactions that actually need a human.

Vendors treat the digital directory as table stakes — Oracle OPERA Cloud, Maestro, Operto, SuitePad's in-room directory, and the resort specialists (Nonius, STAY, LoungeUp) all ship one. Atrium adds what none offers locally: **Bangla/English content, prayer times as a first-class item, offline availability, and a property-editable CMS**.

This document defines the feature's scope, quantifies its business case, and maps it to the journey moments and operations it improves.

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What the guest can do from the app |
|---|------------|------------------------------------|
| 1 | **One-touch Wi-Fi connect** | Join the property network with a single tap — no password to find or mistype. |
| 2 | **Property directory** | Browse every outlet and amenity: hours, locations, house rules, dress codes. |
| 3 | **FAQs & how-tos** | Step-by-step answers — how the AC/safe works, how late check-out works. |
| 4 | **Transport, parking & prayer-time info** | Airport transfers, taxis, parking, shuttles, prayer times and prayer-room location. |
| 5 | **Curated local recommendations** | Property-picked restaurants, attractions, and emergency info (hospital, pharmacy, police). |
| 6 | **Search** | Type a question or keyword and jump straight to the answer. |
| 7 | **Bangla / English** | Every item authored and served bilingually. |
| 8 | **Offline availability** | The full guidebook caches on device — works in dead zones and outages. |
| 9 | **Property-editable CMS** | Staff edit hours, rules, FAQs, and tips — instant publish, no vendor involvement. |
| 10 | **Facility showcases** | Explore marketing-grade photo and video pages for the pool, spa, gym, kids' club, ballroom, and other venues — each ending in its booking or ordering action. |

### 2.2 In scope (this release)
Wi-Fi join; the property directory (outlets, hours, house rules, dress codes); facility showcases — marketing-grade photo/video pages per venue, drawing on the shared property media library, each ending in its action; FAQ/how-to library; transport, parking, shuttle, and prayer-time content; local recommendations with emergency information; full-text search; Bangla/English authoring and display; offline caching; the property CMS with instant publish, seasonal scheduling, and preview.

### 2.3 Out of scope (this release)
Live chat ([G7 Guest Messaging](guest-mobile-app-guest-messaging-feature.md) — the guidebook deflects there only when self-service fails); the map rendering of these places ([G11](guest-mobile-app-interactive-property-map-feature.md)); activity and restaurant *booking* ([G12](guest-mobile-app-activities-spa-experiences-feature.md)); third-party content syndication; languages beyond the authored Bangla/English pair.

### 2.4 Key assumptions
- The property dedicates a content owner (front office or marketing) — minutes per week, but someone must own it.
- One-touch Wi-Fi join is achievable via platform Wi-Fi APIs or captive-portal pass-through.
- Prayer times come from a standard calculation method configured per property, with local override.
- G15 app-less access serves the same content as QR-linked web pages for guests without the app.

---

## 3. Capability Deep-Dive — The Guidebook Features

Each capability: **what it is**, **how it works in Atrium**, **why it matters**, and the **revenue/cost angle**, with vendor precedents noting the bar to clear.

### 3.1 One-touch Wi-Fi connect
*Precedent: Oracle OPERA Cloud; Nonius; SuitePad.*

**What it is.** Joining the property Wi-Fi with a single tap — no card to hunt for, no mistyped password, no call to the desk.

**How it works in Atrium.** The app joins the device via the platform Wi-Fi API, or through the captive portal with credentials pre-filled; the password stays visible for companions' laptops.

**Why it matters.** The Wi-Fi password is the most-asked desk question and the guest's first in-room task — friction here sets the tone for the stay.

**Revenue / cost angle.** Deflects the #1 query (cost); a connected guest is reachable for every notification, offer, and order in the app (revenue enablement).

### 3.2 Property directory — hours, amenities, house rules, dress codes
*Precedent: SuitePad digital directory; Maestro guest app; Operto guest portal.*

**What it is.** The structured catalogue of what the property offers and its rules: every restaurant, pool, gym, spa, and court with hours, location, and policies.

**How it works in Atrium.** Each outlet is a CMS item with hours (seasonal schedules included), photos, and rules, linking onward — a restaurant to its G5 menu, the spa to G12 booking, everything to its G11 map pin.

**Why it matters.** When this lives in a binder or a staff member's head, guests either interrupt staff or never find the amenity at all — and leave without spending on it.

**Revenue / cost angle.** Amenity discovery drives amenity spend (revenue); every hours question answered in-app is a desk interaction avoided (cost).

### 3.3 FAQs & how-tos
*Precedent: Operto guest portal; SuitePad; LoungeUp knowledge content.*

**What it is.** Step-by-step answers to operational questions: how the AC and safe work, how to get more towels, how late check-out and checkout billing work.

**How it works in Atrium.** A searchable, bilingual FAQ library with deep links into actions: the towels answer ends in a [G4 request](guest-mobile-app-in-stay-service-requests-feature.md) button; the late check-out answer links to the [G6 offer](guest-mobile-app-upsells-offers-feature.md).

**Why it matters.** These questions otherwise arrive as operator calls at all hours — trivial individually, a real labor line collectively, a service failure when unanswered at 11pm.

**Revenue / cost angle.** Deep links convert answers into transactions — a tracked request, a paid upsell (revenue); every self-served answer is a call that never happened (cost).

### 3.4 Transport, parking & prayer-time info
*Precedent: SuitePad/Operto directory content; prayer times are an Atrium localization — no global vendor ships them as a first-class item.*

**What it is.** How to arrive, park, and get around — airport transfers, taxi advice, parking rules, shuttle timetables — plus daily prayer times and the mosque/prayer-room location.

**How it works in Atrium.** Transport items carry schedules and tap-to-call/tap-to-book links (transfers route through G6/G12 where sold); prayer times compute daily from the property's location and convention and link to the prayer-room map pin.

**Why it matters.** Transport questions cluster at arrival and departure — when the desk is busiest. Prayer times are a daily need for most Bangladeshi guests; serving them natively signals a product built for this market.

**Revenue / cost angle.** Transfer guidance linking to a bookable transfer becomes ancillary revenue; prayer-time and parking self-service deflects a steady daily call stream (cost).

### 3.5 Curated local recommendations
*Precedent: Oracle OPERA Cloud area tips; Nonius, STAY, LoungeUp local guides.*

**What it is.** The property's picks for nearby restaurants, attractions, and day trips — plus emergency info: hospital, 24-hour pharmacy, police.

**How it works in Atrium.** Curated CMS items with photos, bilingual descriptions, distance, and price band; emergency info is a pinned, always-offline section; partner items can link to booking via G12.

**Why it matters.** Local knowledge today depends on which staff member the guest asks; publishing it serves every guest equally — and emergency info in one known place is a safety obligation.

**Revenue / cost angle.** Partner-linked recommendations open a commission line (revenue); concierge-question deflection frees skilled staff for high-touch requests (cost).

### 3.6 Search
*Precedent: knowledge-base search in LoungeUp/Duve-class guest platforms.*

**What it is.** One search box over the entire guidebook — the guest types "pool hours" or "iron" and lands on the answer.

**How it works in Atrium.** Full-text, bilingual search with synonym handling (Bangla and English queries match either language's content); zero-result searches log to the CMS as content gaps.

**Why it matters.** A guidebook nobody can navigate is a binder with a battery; search makes forty categories usable in the five seconds a guest will spend.

**Revenue / cost angle.** Higher answer-success means higher deflection (cost); the query log is free market research on guest demand, feeding G6 offer ideas (revenue insight).

### 3.7 Bangla / English bilingual content
*Precedent: OPERA Cloud multi-language journey; BD vendors' Bangla/English UI — none pair it with a guest guidebook.*

**What it is.** Every guidebook item authored and served in Bangla and English, with the guest's choice applying app-wide.

**How it works in Atrium.** The CMS holds both language versions side by side, flags untranslated items, and never blocks publishing on a missing translation (it falls back with a marker).

**Why it matters.** For domestic guests — the majority at most Bangladeshi properties — English-only content suppresses use of the entire app.

**Revenue / cost angle.** Doubles the addressable audience for every deflection and offer; a decisive differentiator against global vendors shipping English-plus-European languages.

### 3.8 Offline availability
*Precedent: no guest-app vendor ships a genuinely offline guidebook — mirrors Atrium Staff's offline-first stance (F1); a BD-market differentiator.*

**What it is.** The complete guidebook — directory, FAQs, transport, emergency info — cached on device and fully readable with no connectivity.

**How it works in Atrium.** Content syncs on first open and refreshes in the background; emergency information is pinned always-offline; stale content shows its last-sync time.

**Why it matters.** Resort Wi-Fi has dead zones and coastal-Bangladesh coverage is uneven; the moments a guest most needs the guidebook — an emergency, late at night — are when connectivity is least reliable.

**Revenue / cost angle.** Deflection keeps working during outages (cost resilience); reliability builds the guest habit that carries over to the revenue features.

### 3.9 Property-editable CMS
*Precedent: SuitePad content manager; Operto/LoungeUp property dashboards.*

**What it is.** The staff-side content system where the property creates and edits every guidebook item — instantly, without a vendor ticket.

**How it works in Atrium.** An authorized staff role (RBAC-gated via Atrium Staff) edits text in both languages, photos, hours, and publish/expire schedules with preview; a staleness report and the zero-result log keep content honest.

**Why it matters.** One wrong opening time and guests stop trusting the whole guidebook — and the calls come back. Vendor-mediated editing is why printed compendia rot; property-owned editing keeps this one alive.

**Revenue / cost angle.** No per-change vendor fees or delays (cost); current content sustains deflection and keeps promotional items timely enough to sell (revenue).

### 3.10 Facility showcases
*Precedent: INTELITY mobile guest experience; SuitePad in-room media; Nonius/STAY/LoungeUp resort guest apps.*

**What it is.** Marketing-grade photo and video pages for the pool, spa, gym, kids' club, ballroom, and other venues — distinct from the informational directory entries (§3.2): the directory says when the spa opens and what to wear; the showcase makes the guest want to go.

**How it works in Atrium.** Each showcase pairs a venue's directory entry with a curated gallery — photos, video, bilingual copy — drawn from the **shared property media library** that also feeds [G1's room-type galleries](guest-mobile-app-booking-pre-arrival-feature.md) and [G6's upgrade previews](guest-mobile-app-upsells-offers-feature.md), so one upload serves every surface. Every showcase ends in its action: the spa page in "Book a treatment" ([G12](guest-mobile-app-activities-spa-experiences-feature.md)), the restaurant page in "See the menu" ([G5](guest-mobile-app-food-beverage-ordering-feature.md)), every venue in "Get directions" ([G11](guest-mobile-app-interactive-property-map-feature.md)).

**Why it matters.** The directory answers questions; showcases create demand. Guests spend on what they can see — a guest who has watched the pool video and browsed the spa gallery arrives already wanting them, where a line in an hours table persuades no one.

**Revenue / cost angle.** Showcases are the merchandising layer on top of deflection — each one funnels demand into a booking, order, or visit (revenue); media is produced once in the shared library and reused across G1, G6, and G10 (cost).

---

## 4. Why This Matters

### 4.1 Repetitive questions are a permanent, hidden labor line
Every property answers the same few hundred questions thousands of times a year — unbudgeted, but consuming real staff-hours daily. The guidebook converts that recurring labor into one authoring task plus minutes-per-week upkeep.

### 4.2 Self-service is what guests now prefer
The same guests who won't call the desk will happily search an app. Answering where the guest already is reads as effortless service; making them queue for trivia reads as friction.

### 4.3 It is the adoption engine for the whole guest app
The guidebook gives every guest a reason to open the app on day one (Wi-Fi, hours, prayer times) — and every open is an impression for G5 ordering, G6 offers, and G12 booking.

### 4.4 A localization opening no global vendor serves
Bangla content, prayer times, local transport realities, and offline resilience are exactly what OPERA, SuitePad, and the resort-app specialists don't ship. Atrium can be definitively better *here*.

---

## 5. Journey Moments & Operations It Improves

| Moment / operation today | Pain | With Digital Guidebook |
|--------------------------|------|------------------------|
| **First minutes in the room** | Hunt for the Wi-Fi card; call the desk | One tap connects. |
| **Planning the day** | Guests don't know what the property offers | Full directory with hours, photos, booking links. |
| **"How does the AC / safe work?"** | Call the operator; wait | Illustrated how-to, in Bangla or English, instantly. |
| **Peak desk hours** | Trivia questions queue behind check-ins | Routine questions self-served; desk handles arrivals. |
| **Night shift** | One staffer fields every call | Guidebook answers 24/7 at no marginal labor. |
| **Prayer times & prayer room** | Guest asks staff daily, or goes without | Times on the home screen; location one tap away. |
| **Emergency (illness, lost passport)** | Guest panics; staff scramble for numbers | Pinned emergency section, available offline. |

Guest friction removed: queueing and dialing for trivia, language barriers, dead-zone failures. Staff load removed: the operator's call volume, the desk's interrupt tax, the briefing burden of keeping every shift's answers consistent.

---

## 6. How It Increases Revenue

1. **Amenity discovery drives ancillary spend.** Guests spend on what they know exists. One-tap paths into G5 menus and G12 booking convert awareness into folio revenue — the top of the funnel for every outlet on property.
2. **Answer-to-action deep links.** FAQ answers that end in a button — request towels (G4), buy late check-out (G6), book the transfer (G12) — turn deflected questions into transactions instead of dead ends.
3. **Partner commissions.** Local recommendations with booking links open a commission line on spend that previously left the property invisibly.
4. **App adoption compounds every other feature.** The guidebook is the most-opened surface in guest apps of this class; each visit is a zero-cost impression for revenue-carrying campaigns (G8) and offers (G6).
5. **Service perception → reviews → rate power.** "Effortless" is a review-score word; scores feed pricing power and direct-booking share.

> **Illustrative model:** A 200-room resort where the guidebook drives even 3 incremental amenity/experience transactions/day at an average ৳2,500 adds ~৳2.7M/year — before counting partner commissions and the campaign audience its daily opens create. *(Figures illustrative; validate against property data.)*

---

## 7. How It Reduces Operational Cost

1. **Call and desk deflection at zero marginal cost.** Each authored answer serves every guest forever; if routine contacts drop by half, the operator and desk reclaim hours every day — the core economic engine.
2. **Night and peak coverage without headcount.** The guidebook is the extra staffer on the night shift and at the check-in crush — answering in parallel, in both languages, free.
3. **Consistency without briefing overhead.** One source of truth ends the cost of keeping every shift's answers current — and the comps guests extract after acting on wrong information.
4. **No print, no reprint.** Room compendia, info cards, and their perpetual reprint cycle go away.
5. **Content gaps found automatically.** The zero-result search log tells staff exactly which answer to write next.
6. **Fewer misdirected requests.** Guests who understand how things work file fewer malformed requests and billing disputes.

> **Illustrative model:** If informational calls/desk queries at a 200-room property run ~120/day at ~2 staff-minutes each, a 50% deflection saves ~2 staff-hours/day ≈ **730 staff-hours/year** — roughly a third of a full-time position, redeployable to service that earns revenue. *(Figures illustrative; validate against property data.)*

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Deflect routine questions | Informational calls/desk queries per occupied room | ↓ 40–50% vs. baseline |
| Guests find answers | Search success rate (result tapped) | ≥ 80% |
| Wi-Fi friction removed | Wi-Fi-related contacts to desk/operator | ↓ 70% |
| Adoption engine | Guests opening the guidebook during stay | ≥ 60% of app users |
| Content freshness | Items reviewed within 90 days | 100% |
| Bilingual reach | Guidebook sessions in Bangla | Tracked; ↑ with domestic mix |
| Answer-to-action | FAQ deep-link conversions | ↑ measurable |
| Showcases create demand | Showcase → booking/menu click-through (G12/G5) | ≥ 10% of showcase views |
| Offline resilience | Guidebook content availability offline | 100% |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| Content authoring | Initial library is a real onboarding task | BD-tuned starter template; property fills specifics. |
| Content rot | Stale hours destroy trust and revive calls | Staleness report, seasonal scheduling, zero-result log. |
| Wi-Fi join mechanics | Captive portals vary by vendor | Platform APIs; portal pass-through; visible-password fallback. |
| Prayer-time accuracy | Conventions vary by locality | Standard calculation methods with per-property override. |
| Translation quality | Poor Bangla reads worse than none | Human-authored both languages; untranslated-item flags; no auto-translate. |
| Offline cache size | Photo-heavy content on low-storage devices | Tiered caching: text always, images at usable resolution. |
| Recommendation liability | A recommended venue disappoints or closes | "Property picks" framing, review cadence, one-tap takedown. |
| App-less reach | Non-app guests still need answers | G15 serves the same content as QR web pages. |

---

## 10. Scope delivered

Atrium Guest ships as one complete package. This feature delivers:

- One-touch Wi-Fi connect (visible-password fallback)
- Property directory: outlets, hours, locations, house rules, dress codes
- Facility showcases: marketing-grade photo/video pages for pool, spa, gym, kids' club, and venues, each ending in its action (G12 booking, G5 menu, G11 directions), served from the shared property media library (with G1/G6)
- FAQ & how-to library with deep links into G4 requests, G6 offers, G12 booking
- Transport, parking, and shuttle info with tap-to-call/tap-to-book
- Daily prayer times and prayer-room/mosque location
- Curated local recommendations with pinned emergency information
- Full-text bilingual search with zero-result logging
- Bangla/English authoring and display for every item
- Offline caching of the complete guidebook
- Property-editable CMS: instant publish, seasonal scheduling, preview, staleness reporting, RBAC-gated via Atrium Staff

---

## 11. Open Questions

- Which Wi-Fi network vendors are in the launch properties, and which support one-touch join?
- Who owns guidebook content at a typical property — front office, marketing, or the GM — and does the CMS need an approval step?
- Should local recommendations support paid placement / partner-commission tracking at launch, or start editorial-only?
- Prayer-time convention: fixed per property, or guest-selectable calculation method?
- How much of the guidebook does G15 app-less serve at launch — full mirror, or a top-questions subset per QR touchpoint?

---

## 12. One-Line Business Case

> **The Digital Guidebook answers every routine guest question once — searchably, bilingually, offline, and kept current by the property itself — deflecting the desk's highest-volume workload at zero marginal cost while turning every answered question into a doorway to orders, offers, and bookings.**
