# Interactive Property Map — Feature Deep-Dive

**Product:** Atrium Guest (Hotel / Resort Guest-Facing Mobile App)
**Feature area:** G11 — Interactive Property Map
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-16
**Status:** Draft v1.0
**Related:** [Guest Mobile App — Feature Scope](guest-mobile-app-feature-scope.md) · [Competitive Feature Research](../../HOTEL-~1.MD)

---

## 1. Executive Summary

The **Interactive Property Map** turns the resort's grounds into a tap-able surface: every restaurant, pool, court, spa, venue, ATM, and prayer room is a point of interest with live open/closed status, walking directions, and — what makes it a revenue feature rather than a signpost — a **tap-through to action**: tap a restaurant and its menu opens ([G5](guest-mobile-app-food-beverage-ordering-feature.md)); tap the spa and booking opens ([G12](guest-mobile-app-activities-spa-experiences-feature.md)); tap tonight's event and the schedule opens.

On a sprawling resort, the economics are blunt: **what guests can't find, they don't spend on**. A guest who never discovers the beach grill, or gives up looking for the spa, is folio revenue that evaporated for want of a map. Meanwhile "how do I get to…" is one of the desk's steadiest question streams — and a wedding with 300 outside guests turns wayfinding into an operational problem all its own.

The resort guest-app specialists prove the category: **Attractions.io** built its business on maps that jump straight from a point of interest into booking, and **Nonius** ships property maps in its resort guest app. No vendor serving the Bangladesh market offers anything comparable. Atrium's map is the visual front door of the guest app — turning wayfinding into discovery, and discovery into folio revenue.

This document defines the feature's scope, quantifies its business case, and maps it to the journey moments and operations it improves.

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What the guest can do from the app |
|---|------------|------------------------------------|
| 1 | **Interactive resort map with points of interest** | Pan and zoom a styled map; every dining outlet, pool, gym, spa, court, venue, ATM, and mosque/prayer room is a tappable pin. |
| 2 | **Live open/closed status** | See what is open right now — G10 hours plus staff overrides for weather or maintenance closures. |
| 3 | **Walking directions / wayfinding** | Get a walking route from current location (or room) to any point, with landmarks and estimated time. |
| 4 | **Tap-through to action** | Restaurant → G5 menu; spa/activity/court → G12 booking; event → schedule; amenity → G10 entry. |
| 5 | **Event-day overlays** | Themed overlays route wedding/banquet guests from gate to venue — shareable to non-app guests via G15 link/QR. |
| 6 | **Accessibility routes** | Step-free routing preferring ramps, lifts, and accessible entrances; accessible facilities as a filterable layer. |
| 7 | **Visual spot previews** | Tap any point of interest to see a photo-preview card — what the spot looks like and what's there — before walking over; venue previews for event guests. |

### 2.2 In scope (this release)
The interactive map with categorized, searchable points of interest; visual spot previews — photo cards per POI drawing on the shared property media library — including venue previews for event guests; live open/closed status from G10 hours plus staff overrides with reason and reopening estimate; walking directions with time estimates and landmark-based steps; tap-through deep links into G5, G12, event schedules, and G10 detail pages; event-day overlays shareable app-lessly via G15; accessibility routing and the accessible-facilities layer; offline map tiles and POI data; Bangla/English throughout; map content managed in the same property CMS as G10.

### 2.3 Out of scope (this release)
Indoor turn-by-turn navigation with beacon/UWB infrastructure (routing is outdoor/landmark-level; floor plans are static images at v1); live guest-location tracking; augmented-reality wayfinding; city maps beyond links to external map apps; staff-side geo features (asset locations, patrol routes — a possible Atrium Staff follow-on).

### 2.4 Key assumptions
- The property can supply or commission a base map (site plan or styled illustration) during onboarding; Atrium provides the POI/routing authoring tools on top.
- Walking paths can be traced as a simple path network at onboarding — enough for landmark-based directions without survey-grade GIS.
- Device GPS is adequate for outdoor "you are here" on resort grounds; indoor positioning is not required for v1.
- Hours and status come from the G10 CMS, so the map never contradicts the guidebook.

---

## 3. Capability Deep-Dive — The Map Features

Each capability: **what it is**, **how it works in Atrium**, **why it matters**, and the **revenue/cost angle**, with vendor precedents noting the bar to clear.

### 3.1 Interactive resort map with points of interest
*Precedent: Attractions.io interactive maps; Nonius resort guest app.*

**What it is.** A pannable, styled map of the property where everything a guest might seek is a categorized pin: dining, pools, gym, spa, courts, venues, kids' club, ATM, mosque/prayer room, parking, medical.

**How it works in Atrium.** The base map — the property's site plan or a styled illustration — is georeferenced at onboarding. POIs are CMS items sharing data with G10 (bilingual name, category, photos, hours) plus a location; category filters and search narrow the view, and tapping a pin opens its card with photo, status, distance, and actions.

**Why it matters.** A resort is a place guests don't know, navigated at leisure, often at night; paper maps are static, monolingual, and silent about what's open. The interactive map is the property's browsable inventory of experiences — the difference between a guest who uses three amenities and one who uses eight.

**Revenue / cost angle.** Every additional amenity a guest discovers is a potential folio line (revenue); every "where is…" self-served is a desk interaction avoided (cost).

### 3.2 Live open/closed status
*Precedent: Attractions.io live status/wait-time patterns from the attractions world — largely absent from hotel guest apps, a differentiator.*

**What it is.** Pins that know whether the place is open right now — and say so before the guest walks fifteen minutes to a shut door.

**How it works in Atrium.** Scheduled hours flow from the G10 CMS; an "open now" state computes per POI. Staff override in a tap for unscheduled closures, with a reason and reopening estimate shown on the pin ("Closed — high wind. Expected reopen 16:00").

**Why it matters.** The walk to a closed outlet is wasted guest time, a sour moment, and often a desk complaint. Live status converts dead walks into redirected intent — the guest picks the open café instead.

**Revenue / cost angle.** Redirected intent keeps spend on property (revenue); fewer "it was closed" complaints and comp requests (cost); one staff override replaces a day of desk explanations (cost).

### 3.3 Walking directions / wayfinding
*Precedent: Attractions.io wayfinding; Nonius map navigation.*

**What it is.** A walking route from where the guest stands (or their room) to any POI, with estimated time and landmark-based steps — "past the main pool, left at the tennis courts".

**How it works in Atrium.** The onboarding-traced path network powers shortest-path routing; GPS supplies the start point, with "route from my room" as fallback. Directions render as a highlighted path plus plain-language steps in Bangla or English, with walking time shown up front.

**Why it matters.** "How do I get to the spa *from here*?" is a question no lobby map answers. Wayfinding is also confidence: guests explore further — and find more to spend on — when they can't get lost, especially after dark.

**Revenue / cost angle.** Confident guests roam to distant outlets — the beach bar earns more when guests trust they'll find it (revenue); "how do I get to…" leaves the desk queue (cost).

### 3.4 Tap-through to action
*Precedent: Attractions.io's map-to-booking jump — its signature pattern; Nonius map-to-service links.*

**What it is.** The pin is a doorway, not a label: every POI card ends in the action the place implies. Restaurant → live menu and ordering (G5). Spa, court, kids' club, activity → booking (G12). Tonight's event → its schedule. Amenity → its guidebook entry (G10).

**How it works in Atrium.** POI cards carry deep links resolved against type and state: an open restaurant shows "View menu · Order"; the spa shows "Book a treatment" with next slots surfaced from G12's live capacity; an event pin shows the schedule with add-to-itinerary. Closed outlets still allow forward action — "book a table for tomorrow" — so a closed door doesn't kill intent.

**Why it matters.** This is the difference between a map and a storefront: "what's that?" to paid booking becomes two taps with zero staff involvement — curiosity monetized at the moment it occurs, instead of decaying into "maybe later".

**Revenue / cost angle.** The map becomes a discovery-to-transaction funnel feeding G5 and G12 — the core of the revenue case; map-originated bookings arrive structured in staff systems (F18 calendars, F8 POS) rather than as desk questions (cost).

### 3.5 Event-day overlays
*Precedent: no direct hotel-app precedent — adapted from attraction/festival map modes (Attractions.io event layers); pairs with staff-side F18.*

**What it is.** A per-event map mode for wedding, banquet, or conference days: a themed overlay showing the venue, its entrance, parking, prayer room, washrooms, and the route from the gate — for guests who have never set foot on the property.

**How it works in Atrium.** When staff create an event in [F18 Events & Activities](../../staff/docs/staff-mobile-app-events-activities-feature.md), they can author its overlay: highlight the venue and route, mark event parking and facilities, set active hours. The overlay shares as a G15 link/QR — printed on the invitation or gate banner — so a wedding guest opens it in a browser, no install, in Bangla or English.

**Why it matters.** A 300-guest wedding brings 300 people who don't know the property, arriving in a two-hour window, all asking the same three questions — today answered by signage, ushers, and a swamped gate. The overlay answers gate-to-seat wayfinding before anyone asks; every guest who opens it is a prospective future hotel guest.

**Revenue / cost angle.** Smooth events win repeat banquet business, and the overlay is a marketing touch to hundreds of locals (revenue); fewer ushers and gate staff per function (cost).

### 3.6 Accessibility routes
*Precedent: accessibility layers in attraction-map practice; no BD-market precedent — a differentiator and an inclusion obligation.*

**What it is.** Step-free routing and an accessible-facilities layer: routes that prefer ramps, lifts, and accessible entrances; filters for accessible toilets, parking, and pool access.

**How it works in Atrium.** Path segments carry accessibility attributes (steps, slope, surface) set at onboarding; a per-route switch prefers step-free paths and flags unavoidable barriers honestly ("this route includes 6 steps"). Accessible facilities render as a filterable layer.

**Why it matters.** For wheelchair users, elderly family members, and parents with prams, a route with one staircase is not a route. Honest accessible wayfinding is the difference between using the property independently and needing an escort for every trip.

**Revenue / cost angle.** Accessible confidence wins the multigenerational family-group bookings that fill resort rooms in blocks (revenue); fewer staff-escort requests and complaint recoveries (cost).

### 3.7 Visual spot previews
*Precedent: Attractions.io POI cards; Nonius resort guest app.*

**What it is.** Tapping any point of interest opens a photo-preview card of the spot — what it looks like, what's there — before the guest walks over: the infinity pool as a swipeable gallery, the beach grill's deck at sunset, the ballroom a wedding guest is heading to.

**How it works in Atrium.** POI cards lead with photos (and video where authored) drawn from the **shared property media library** that also powers [G10's facility showcases](guest-mobile-app-digital-guidebook-property-info-feature.md), [G1's room-type galleries](guest-mobile-app-booking-pre-arrival-feature.md), and [G6's upgrade previews](guest-mobile-app-upsells-offers-feature.md) — one upload, every surface. The preview sits above the card's status, distance, and actions (§3.4), so seeing leads straight into going or booking; event-day overlays (§3.5) give banquet guests a venue preview before they arrive.

**Why it matters.** Preview turns the map from navigation into discovery: a labeled pin answers "where is it?", but the photo is what converts "what's that?" into a visit and a spend — the guest browses the grounds visually the way they browsed the hotel before booking.

**Revenue / cost angle.** Previews raise the share of POI views that become walks, orders, and bookings — more fuel for the §3.4 tap-through funnel (revenue); media reuse from the shared library means no map-specific content production (cost).

---

## 4. Why This Matters

### 4.1 On a resort, discovery is the revenue bottleneck
Room revenue is booked before arrival; ancillary revenue depends on what the guest finds once there. A property can build a world-class spa and still earn nothing from the guest who never realized where it was. The map is the merchandising layer for the physical property.

### 4.2 Wayfinding questions never stop on their own
Every new arrival resets the clock: the same "where is the gym?" questions, every day, concentrated at the desk at peak times. Signage can't answer from the guest's current location, can't say what's open, and can't speak two languages at night.

### 4.3 It is the visual front door of the guest app
Lists of amenities are skipped; maps are explored. The map is a browsable home surface guests return to daily — and every return is an impression for the pins, statuses, and offers it carries.

### 4.4 A category the BD market hasn't seen
Attractions.io and Nonius prove guests use interactive maps and book from them. Local vendors offer nothing comparable; for the coastal resorts (Cox's Bazar, Kuakata) and event-heavy city properties, the map is a visible, demo-winning differentiator.

---

## 5. Journey Moments & Operations It Improves

| Moment / operation today | Pain | With Interactive Map |
|--------------------------|------|----------------------|
| **First hours on property** | Guest orients by wandering or asking; misses half the amenities | Browsable map reveals everything; day one becomes discovery. |
| **"How do I get to…" at the desk** | Steady interrupt stream; verbal directions fail | Self-served routes from wherever the guest stands. |
| **Walk to a closed outlet** | Wasted trip, sour guest, desk complaint | Live status redirects to what's open before the walk. |
| **Deciding where to eat** | Guest defaults to the nearest/known outlet | Map merchandises all outlets; menu one tap away (G5). |
| **Spa/activity awareness** | Guests learn of the spa on the last day, or never | Pin + "book now" from day one (G12). |
| **Wedding/banquet day** | 300 outside guests, swamped gate, ushers everywhere | Event overlay routes gate-to-venue, app-lessly via QR (G15). |
| **Guests with mobility needs** | Staff escorts; discovered-too-late staircases | Step-free routes and accessible-facilities layer. |

Guest friction removed: disorientation, dead walks, undiscovered amenities, language-dependent directions. Staff load removed: the wayfinding interrupt stream, closure explanations, event-day ushering, escort requests.

---

## 6. How It Increases Revenue

1. **Discovery → folio conversion.** The map surfaces every revenue point on the property to every guest, with the transaction one tap away; moving guests from using three amenities to five is a direct folio multiplier.
2. **Redirected intent when things are closed.** Live status converts "walked to a shut grill, gave up" into "went to the open café instead" — spend retained on property.
3. **Distant outlets get found.** The beach bar at the far end earns in proportion to guests' confidence they can find it and get back — wayfinding is free marketing for the periphery.
4. **Event overlays win banquet business.** Visibly smooth 300-guest wayfinding is part of what sells the next wedding — and every overlay open is a brand impression on a local prospective guest.
5. **Accessibility wins group bookings.** Multigenerational family groups — a core resort segment in Bangladesh — book where the grandmother can get around independently.
6. **Session frequency feeds every other feature.** The map is a daily-return surface; its traffic compounds G5 orders, G6 offers, and G12 bookings.

> **Illustrative model:** A 250-room resort at 70% occupancy hosts ~350 in-house guests. If map-driven discovery adds one incremental F&B/activity transaction per 10 guests per day at ৳1,800 average, that is ~৳63,000/day ≈ **৳23M/year** in incremental ancillary revenue — before banquet wins and closed-outlet spend retention. *(Figures illustrative; validate against property data.)*

---

## 7. How It Reduces Operational Cost

1. **Wayfinding deflection.** "How do I get to…" and "is the pool open?" leave the desk and operator queues — a daily labor saving that costs nothing per additional guest served.
2. **Closure broadcasting.** One staff override replaces a day of desk explanations, printed notices, and door signs for every weather or maintenance closure.
3. **Leaner event-day staffing.** Overlay-driven self-wayfinding trims the ushers, gate marshals, and direction-givers each function absorbs — labor F18's crew costing makes visible.
4. **Fewer comps from dead walks.** The guest who trekked to a closed spa and demands a gesture is a recurring, avoidable comp line; live status prevents the grievance.
5. **Fewer escorts.** Accessible routing and confident night wayfinding reduce "can someone show me the way" requests that pull staff off station.
6. **Signage economy.** The map absorbs the long tail of temporary signs, arrows, and reprints that follow every layout change.

> **Illustrative model:** If the map deflects ~40 wayfinding/status queries/day (~2 staff-minutes each) and saves 2 usher-shifts per large event across ~10 events/month, that is ~490 staff-hours/year of deflection plus ~960 event staff-hours/year — before comps avoided. *(Figures illustrative; validate against property data.)*

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Drive discovery | Distinct POIs viewed per guest stay | ≥ 8 |
| Convert discovery | Map tap-throughs to G5 menu / G12 booking | ≥ 15% of POI views |
| Previews convert curiosity | POI previews followed by directions, menu, or booking tap | ≥ 30% of previews |
| Revenue attribution | Folio revenue on map-originated sessions | ↑ measurable |
| Deflect wayfinding | "Where is / how do I get to" desk queries | ↓ 50% vs. baseline |
| Prevent dead walks | Complaints citing closed outlets after a walk | ↓ 70% |
| Event wayfinding | Event-overlay opens per event guest | ≥ 40% (via G15 QR) |
| Daily engagement | Guests opening the map per in-stay day | ≥ 50% of app users |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| Base map production | Site plan / styled map needed per property | Onboarding georeferences the existing site plan; styled illustration as an upgrade. |
| Path network authoring | Routing needs traced walkways with attributes | Simple tracing tool at onboarding; landmark directions tolerate coarse paths. |
| GPS reliability | Tree cover and buildings degrade outdoor GPS | "Route from my room / from reception" fallback; landmark-first directions. |
| Status accuracy | A wrong "open" is worse than no status | Single hours source (G10 CMS); one-tap staff override; auto-expiring overrides. |
| Stale POIs | Renovations move things | POI edits in the shared CMS; review prompts tied to G10's staleness report. |
| Event overlay authoring | Coordinators must build overlays per event | Templates per venue; overlays optional — the default map still works. |
| Accessibility data honesty | Wrong step-free claims harm real users | Onboarding audit walk; guest-reported corrections routed to staff via G14. |
| Offline behavior | Grounds have dead zones | Offline tiles + POI cache; routing works offline; status marked with last-sync time. |
| Deep-link consistency | Map, guidebook, menus, and booking must agree | Shared POI model across G10/G11; links into G5/G12 tested per property. |

---

## 10. Scope delivered

Atrium Guest ships as one complete package. This feature delivers:

- Interactive resort map with pan/zoom and categorized, searchable POIs (dining, pools, gym, spa, courts, venues, kids' club, ATM, mosque/prayer room, parking, medical)
- Visual spot previews: photo-preview cards on every POI, with venue previews for event guests, served from the shared property media library (with G1/G6/G10)
- Live open/closed pin status from G10 hours, with staff overrides carrying reason and reopening estimate
- Walking directions with time estimates and landmark steps, from GPS position or the guest's room
- Tap-through actions per POI: G5 menus, G12 booking with surfaced slots, event schedules, G10 detail pages
- Event-day overlays authored from staff F18 events, shareable via G15 link/QR
- Accessibility: step-free routing preferences, honest barrier flags, accessible-facilities layer
- Bangla/English POI names, cards, and directions
- Offline map tiles, POI data, and routing with last-sync indicators
- POI and status content managed in the shared property CMS (with G10), RBAC-gated via Atrium Staff

---

## 11. Open Questions

- Base map format per launch property: georeferenced site plan, styled illustration, or satellite-derived — and who produces it?
- Is indoor floor-plan support (static images per building level) needed at v1 for the city-hotel segment, or resort-outdoor only?
- Are event overlays authored by the property in the F18 flow, or as an Atrium service task for early customers?
- Should live status extend to capacity hints ("pool busy now") in a later release, and what would feed it?
- Which launch property validates GPS wayfinding — a large coastal resort is the strongest proof, but the hardest terrain?

---

## 12. One-Line Business Case

> **The Interactive Property Map turns the resort's grounds into a browsable storefront — every amenity findable, its status live, and its booking one tap away — converting wayfinding into discovery and discovery into folio revenue, while the "how do I get to…" queue at the desk disappears.**
