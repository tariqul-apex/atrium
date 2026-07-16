# Feature Scope — Hotel / Resort Guest-Facing Mobile App

**Product:** Atrium Guest (Hotel / Resort Guest-Facing Mobile App)
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-16
**Status:** Draft v1.0
**Start here:** [Project Overview & Documentation Index](../overview.md)

---

## 1. Overview

### 1.1 Purpose
A mobile-first application (and matching **app-less QR/web flows** — G15) that gives hotel and resort guests a single tool to book, arrive, unlock, order, request, pay, and give feedback from their own device across the whole stay lifecycle. It is the companion surface to [Atrium Staff](../../staff/docs/staff-mobile-app-feature-scope.md): every guest action lands as a task, order, or booking in the staff app — one platform, two surfaces.

### 1.2 Problem statement
- Guests queue at the desk to do things — register, check in, get a key, settle a bill — that their phone could do before they reach the lobby.
- In-stay requests travel by phone call and get lost between departments; guests never know if anything is happening.
- Ancillary revenue (F&B, spa, activities, upgrades) leaks at every friction point: finding a phone, waiting on hold, language barriers, a desk that sells worst at the moments guests buy most.
- The bill is a black box until checkout — the classic satisfaction killer and dispute generator.
- Problems surface on public review sites *after* departure instead of in-stay, when they could still be fixed.
- In Bangladesh specifically, guests hold bKash/Nagad rather than international cards, and **no local vendor offers a real guest-facing app** ([Competitive Research](../../HOTEL-~1.MD)) — properties have no way to serve this behavior today.

### 1.3 Goals
| Goal | Success metric |
|------|----------------|
| Shift arrivals to self-service | 40% of arrivals use mobile/QR check-in within 6 months |
| Reach guests without installs | 70% of on-property guests touch at least one G15 app-less flow per stay |
| Grow ancillary revenue | +10% F&B and activity revenue per occupied room via in-app/QR ordering and booking |
| Recover OTA commission | +10-pt shift of bookings to the direct mobile channel within 12 months |
| Deflect the desk | Front-desk calls and walk-up questions ↓ 30% |
| Catch problems in-stay | 60% of negative feedback signals raised (and addressed) before checkout |

### 1.4 Target users (personas)
The guest app has no staff-style roles; what a guest sees is driven by **journey stage** plus stay type (see the [README matrix](../README.md) and [Journey-Stage Design](guest-mobile-app-journey-stage-design.md)).

- **Leisure traveler** — books direct or via OTA; wants effortless arrival, discovery (map, guidebook), F&B, and a transparent bill.
- **Business traveler** — short stays, zero patience for queues; express check-in/out, digital key, folio/receipt handling, late check-out purchase. Least likely to install — served app-less (G15).
- **Resort family / group organizer** — books activities and dining for a party; shares the room key with companions (G3); heaviest user of the map (G11), activities (G12), and F&B (G5).
- **Event & banquet guest** — on property for a wedding/conference; needs the event schedule, wayfinding, and day-scoped F&B; often never a registered hotel guest — app-less by definition.
- **Day visitor / walk-in local** — F&B and day-use only: sees G5 (ordering), G9 (payment + tipping), G11 (map), G12 (day-use booking), and G14 — no stay, no key, no folio.
- **Returning loyalty member** — the profile persona: preferences on file, points and tier progress visible (G13), personalized offers (G6); the strongest case for the native app install.

---

## 2. Scope Summary

### 2.1 In scope
The fifteen feature areas G1–G15 (§3): mobile booking and pre-arrival, contactless check-in/out, digital room key, in-stay service requests, F&B ordering, upsells and offers, guest messaging, notifications and campaigns, payments/folio/tipping, digital guidebook, interactive property map, activities/spa/experiences booking, loyalty and guest profile, feedback and service recovery — all reachable both in-app and through app-less QR/web links (G15). All are delivered as part of the **one complete package** (§8).

### 2.2 Out of scope (entirely)
Staff-facing functionality (that is [Atrium Staff](../../staff/docs/staff-mobile-app-feature-scope.md)); OTA/channel-manager distribution; in-room IoT controls (thermostat, lighting, curtains); flight/travel booking beyond property transfers; a general-purpose social network or guest-to-guest chat (the sole exception is G7's shared group thread for a traveling party); payroll of tip recipients (G9 routes tips; disbursement is a payout-provider concern). (See §9 Future.)

### 2.3 Assumptions
- Atrium Staff (or an equivalent PMS/ops backend) is live at the property: guest actions need a fulfillment loop — requests route to [F5 Task Management](../../staff/docs/staff-mobile-app-task-management-communication-feature.md), orders to [F8 POS](../../staff/docs/staff-mobile-app-pos-multi-outlet-feature.md), bookings to [F18 Events & Activities](../../staff/docs/staff-mobile-app-events-activities-feature.md).
- Door locks are BLE-capable or upgradeable via certified lock-vendor integrations (G3); physical key fallback always exists.
- Property provides guest Wi-Fi; app-less flows must also work on mobile data.
- Guests use personal smartphones (iOS 15+ / Android 10+) or any mobile browser for G15 flows.
- Local payment gateways (SSLCommerz, bKash, Nagad) plus a card acquirer are contracted by the property.

---

## 3. Feature Areas

Canonical IDs and definitions are in the [guest README](../README.md); each area has a deep-dive doc (linked per feature). Vendor precedents cite the [Competitive Research](../../HOTEL-~1.MD).

### G1. Booking & Pre-Arrival
*Deep-dive: [Booking & Pre-Arrival](guest-mobile-app-booking-pre-arrival-feature.md). Lands in staff [F2 Reservations](../../staff/docs/staff-mobile-app-reservations-management-feature.md).*
- Mobile booking engine: search availability, room/rate selection, book and pay a deposit (cards + bKash/Nagad/SSLCommerz) — precedent: Mews, Cloudbeds, RoomRaccoon.
- Room-type photo galleries, video and 360° virtual tours, and side-by-side room comparison — the guest sees what they're booking, from the shared property media library.
- Digital registration card pre-filled from the booking; document upload; e-signature — precedent: OPERA Cloud, Mews, Guestline GuestStay.
- Arrival details: ETA, transport needs, party composition; preference capture (bed, floor, dietary).
- Pre-order add-ons and pre-book activities before arrival (hands off to G6/G12).
- Everything syncs to the PMS so front desk and housekeeping plan ahead; Bangla/English throughout.

### G2. Mobile Check-In & Check-Out
*Deep-dive: [Check-In & Check-Out](guest-mobile-app-check-in-check-out-feature.md). Lands in staff [F1 Front Desk](../../staff/docs/staff-mobile-app-front-desk-feature.md).*
- Contactless check-in: identity verification (document scan), e-signature, balance/deposit payment, room assignment — precedent: Mews, Canary, Duve, Stayntouch, Shiji Digital Stay.
- "Room ready" push (G8) with key issuance handoff (G3).
- QR-code instant check-in for guests who never installed the app (G15) — precedent: IDS Next FX GeM.
- Express check-out: folio review (G9), settle, e-receipt by email; late-check-out purchase at the moment of need (G6).

### G3. Digital Room Key
*Deep-dive: [Digital Room Key](guest-mobile-app-digital-room-key-feature.md).*
- Bluetooth mobile key issued at check-in, revoked at check-out, via certified door-lock integrations — precedent: INTELITY, Mews + ASSA ABLOY/Salto, Operto/OpenKey.
- Wallet-based keys (Apple Wallet pattern); key sharing with registered travel companions.
- Shared-door entitlements: gym, pool gate, floor/lift access per stay.
- Graceful fallback: front desk can always issue a physical card; key state visible to staff.

### G4. In-Stay Service Requests
*Deep-dive: [In-Stay Service Requests](guest-mobile-app-in-stay-service-requests-feature.md). Lands in staff [F5 Task Management](../../staff/docs/staff-mobile-app-task-management-communication-feature.md).*
- Request items (towels, pillows, amenities) from a structured catalog; report an issue with photo.
- Schedule housekeeping / set Do-Not-Disturb; green opt-out of daily cleaning for a perk — precedent: SuitePad; requests pattern: Guestline Keez, Apaleo.
- Lost-item report (matches staff F16 Lost & Found register).
- Transport & mobility: airport transfer with driver details and pickup tracking, shuttle timetable with live location, valet car retrieval, porter/luggage help, ride-hailing handoff (Pathao/Uber deep link) — precedent: INTELITY valet, Duve transfers.
- Guest safety & emergency assistance: one-tap emergency/medical help with critical-priority routing to the duty manager and security (rides the staff [F16 SOS](../../staff/docs/staff-mobile-app-lost-found-staff-safety-feature.md) alerting pipeline); evacuation info; option to request female staff attendance.
- Live status per request (received → in progress → done) driven by the staff-side SLA timers.
- Auto-routing to the right department with no human relay; emergency requests bypass normal SLA.

### G5. Food & Beverage Ordering
*Deep-dive: [Food & Beverage Ordering](guest-mobile-app-food-beverage-ordering-feature.md). Lands in staff [F8 POS](../../staff/docs/staff-mobile-app-pos-multi-outlet-feature.md).*
- Live outlet menus with photos, prices, dietary filters, Bangla/English item names — precedent: INTELITY, Maestro, Shiji, SuitePad, Mediasoft Pro-Inn.
- Order to the room, to a pool/beach/court location, or to a table (QR table tent → G15); scheduled delivery windows.
- Charge-to-folio for in-house guests; direct payment on local rails for day visitors (G9).
- Order status tracking; reorder from history; orders land straight in the staff POS with zero re-entry.

### G6. Upsells, Offers & Add-Ons
*Deep-dive: [Upsells & Offers](guest-mobile-app-upsells-offers-feature.md).*
- Offers surfaced at the four buying moments: booking, pre-arrival, check-in, mid-stay — precedent: Canary, Duve, Mews, RoomRaccoon.
- Room upgrades with visual upgrade preview — the upgrade room shown through its gallery/tour so the guest sees what the spend buys; early check-in / late check-out, packages (breakfast, celebration setups), experiences (tours, transfers).
- Targeting by guest profile, party composition, occasion, and journey stage (G13 profile + G8 delivery).
- One-tap acceptance books to the folio and the staff-side operational calendar; multi-channel delivery (push, email, WhatsApp link).

### G7. Guest Messaging
*Deep-dive: [Guest Messaging](guest-mobile-app-guest-messaging-feature.md).*
- One two-way thread per stay, pre-arrival through departure, answered from the Atrium Staff inbox — precedent: Mews, Canary, Duve, Shiji.
- Automated answers for common questions with human handoff.
- Auto-translation both directions — guest and staff each write in their own language — precedent: Revinate Ivy.
- Shared group thread for traveling parties (companions per G13) alongside each guest's personal thread.
- WhatsApp/SMS reach-through: guests who prefer those channels join the same conversation.

### G8. Notifications & Campaigns
*Deep-dive: [Notifications & Campaigns](guest-mobile-app-notifications-campaigns-feature.md).*
- Journey notifications: booking confirmed, check-in open, room ready, order on its way, your table is ready (virtual queue, G12), activity reminder, folio ready — precedent: Shiji automated alerts.
- Property campaigns (tonight's live music, spa slots open) targeted by stay stage, segment, and interests — precedent: Nonius push campaigns.
- Per-guest notification preferences and quiet hours; journey messages always rank above marketing; frequency caps.
- App-less fallback: critical journey messages also deliverable via SMS/WhatsApp/email link (G15).

### G9. Payments, Folio & Tipping
*Deep-dive: [Payments, Folio & Tipping](guest-mobile-app-payments-folio-tipping-feature.md).*
- Real-time itemized folio — every room, F&B, spa, and activity charge visible as it posts.
- Pay a deposit, interim balance, or final bill in-app or by payment link — precedent: Cloudbeds/RoomRaccoon pay-by-link.
- **bKash / Nagad / SSLCommerz alongside international cards** — the rails Bangladeshi guests actually hold (BD-vendor precedent: Smart Software, Mediasoft).
- Split & group billing: split the folio across companions or rooms (equal, by-item, or organizer-pays-all) with per-person payment links; organizer spend controls per companion (set in G13); consolidated group invoice.
- Split and email receipts; VAT-compliant invoicing.
- QR-code cashless tipping routed to a person or a department — precedent: Canary digital tipping; works app-less (G15).

### G10. Digital Guidebook & Property Info
*Deep-dive: [Digital Guidebook & Property Info](guest-mobile-app-digital-guidebook-property-info-feature.md).*
- One-touch Wi-Fi connection; opening hours; amenity guides; house rules — precedent: OPERA Cloud, Maestro, SuitePad directory, Nonius/STAY/LoungeUp.
- Facility showcases: marketing-grade photo/video pages for the pool, spa, gym, and venues, each ending in its booking or ordering action.
- Transport information and **prayer times** — local-market essentials.
- Curated local recommendations (restaurants, attractions, shopping).
- Searchable, Bangla/English, and **available offline** once loaded.

### G11. Interactive Property Map
*Deep-dive: [Interactive Property Map](guest-mobile-app-interactive-property-map-feature.md).*
- Tap-able resort map: pools, restaurants, courts, spa, venues, kids' club — precedent: Attractions.io, Nonius.
- Live open/closed status per point of interest; walking directions.
- Photo previews of each spot — tap a pin to see what it looks like before walking over.
- Deep links into revenue: tap a restaurant → its menu (G5); tap the spa → booking (G12); tap an event → schedule.
- Day-visitor and event-guest friendly — no reservation required to browse.

### G12. Activities, Spa & Experiences Booking
*Deep-dive: [Activities, Spa & Experiences](guest-mobile-app-activities-spa-experiences-feature.md). Lands in staff [F18 Events & Activities](../../staff/docs/staff-mobile-app-events-activities-feature.md).*
- Bookable slots with live capacity for activities, spa, dining, courts, kids' club, day-use — precedent: Agilysys, Maestro, INTELITY, SuitePad.
- Rich experience previews: photo/video gallery and what-to-expect (duration, difficulty, age, what to bring) per activity.
- Reserve and pay (or charge to folio); cancellation windows and waitlists.
- Pool & beach amenity booking: reserve cabanas, sunbeds, and daybeds by time slot, picked from the map (G11); premium units charge to folio; anchors poolside G5 ordering.
- Virtual queues: join restaurant/facility waitlists remotely with live queue position and a "table ready" push (G8) with hold window; walk-ins join via QR (G15).
- Personal stay itinerary assembled from bookings, with reminders (G8).
- Resort event and entertainment schedule; every booking lands in the staff-side activity and event calendars with capacity and crew impact.

### G13. Loyalty & Guest Profile
*Deep-dive: [Loyalty & Guest Profile](guest-mobile-app-loyalty-guest-profile-feature.md).*
- Guest profile and preferences (room, pillow, dietary) persisted across stays — precedent: Agilysys profiles, Revinate CRM, Sabre SynXis guest-centric pattern.
- Loyalty points earn on stays and spend; visible tier progress — precedent: Maestro loyalty, IDS Next FX Club App.
- In-app redemption: upgrades, dining, late check-out.
- Group & companion stay management: the booker invites companions by link — each gets scoped app access (own key via G3, own requests/orders, itinerary view) with organizer-set charge permissions (G9); a shared group itinerary aggregates every member's G12 bookings; multi-room group stays link into one party view.
- Stay history and receipts; profile powers personalization in G6, G8, and G12.
- The strongest reason a guest installs the native app rather than staying app-less.

### G14. Feedback & Service Recovery
*Deep-dive: [Feedback & Service Recovery](guest-mobile-app-feedback-service-recovery-feature.md). Lands in staff [F14 Service Recovery](../../staff/docs/staff-mobile-app-guest-feedback-service-recovery-feature.md).*
- Short in-stay pulse ratings at smart moments (post check-in, post meal, mid-stay) — precedent: SuitePad feedback, Revinate.
- Always-available "something wrong?" channel — one tap from anywhere in the app.
- Negative signals route instantly to the duty manager's service-recovery workflow; the guest sees their issue's status.
- Post-checkout review invitation for satisfied guests, channeling promoters to public review platforms.

### G15. App-less Access — QR & Web Links *(cross-cutting enabler)*
*Deep-dive: [App-less Access](guest-mobile-app-app-less-access-feature.md).*
- **Not a screen of its own** — the delivery channel for every other feature, the guest-side analog of Atrium Staff's F17 scanning enabler.
- Check-in, menus and ordering, requests, chat, folio and payment, feedback, and tipping all served as link-and-QR web flows — precedent: Canary app-less journey, Duve web-based app, IDS Next FX GeM, WebRezPro no-download.
- Entry points: booking-confirmation email, bedside card, table tent, pool-bar QR, event signage.
- Session security: signed, stay-scoped, expiring links; no account creation required for basic flows.
- Native app positioned as the upgrade path for resort stays and loyalty members (G13) — never a gate. **Why it leads:** most guests will not install an app for a two-night stay; without app-less, every feature above reaches a fraction of guests.

---

## 4. Non-Functional Requirements

| Category | Requirement |
|----------|-------------|
| Platforms | iOS 15+, Android 10+ native/cross-platform app **plus** responsive mobile-web for all G15 flows (any modern mobile browser) |
| Performance | First meaningful screen < 2s on 4G; QR-link flows load < 3s cold on mobile data; optimistic UI on orders/requests |
| Offline | Guidebook (G10), map (G11), itinerary, and issued key (G3) usable offline; ordering/payment require connectivity with clear state |
| Reliability | 99.9% backend uptime; folio display must never show unreconciled totals as final |
| Security | TLS 1.2+, encrypted at rest; signed expiring links for app-less sessions; PCI-DSS via hosted/tokenized payment flows; key credentials in secure enclave/keystore |
| Privacy | Guest PII minimized, purpose-bound, access-logged; explicit consent for marketing (G8); GDPR/CCPA-aligned; local data-protection compliance |
| Accessibility | WCAG 2.1 AA; large tap targets; readable in bright outdoor (poolside) conditions |
| Localization | Bangla/English at launch; architecture for further languages; ৳ and multi-currency display; local VAT rules |
| Scalability | Single property v1; thousands of concurrent guest sessions (event days); campaign fan-out without journey-message delay |

---

## 5. Integrations

- **Atrium Staff / PMS** — the primary integration: reservations (G1→F2), check-in/out and room assignment (G2→F1), requests (G4→F5), orders (G5→F8), activity/event bookings (G12→F18), feedback (G14→F14), folio and charge posting (G9). Staff deep-dives at [../../staff/docs/](../../staff/docs/staff-mobile-app-feature-scope.md).
- **Door-lock systems** — certified BLE lock vendors (ASSA ABLOY, Salto-class) for G3; wallet-key provisioning.
- **Payment gateways** — SSLCommerz, bKash, Nagad + card acquirer; hosted payment pages / tokenization for PCI scope containment.
- **Messaging channels** — WhatsApp Business API, SMS gateway, email — for G7 reach-through and G15 link delivery.
- **Push** — APNs / FCM.
- **Identity verification** — document scan/OCR provider for G2 check-in.
- **Transport & mobility** — ride-hailing deep links (Pathao/Uber) and transfer-driver location feed for pickup tracking (G4); shuttle GPS where the property operates one.
- **Translation service** — machine translation for G7 auto-translate.
- **Review platforms** — outbound review invitations (G14) to Google/TripAdvisor-class destinations.
- **Media storage & delivery** — object storage/CDN for the property media library (photos, video, 360° tours) behind the visual surfaces of G1, G6, G10, G11, and G12; managed through the same property CMS as G10.
- **Analytics** — journey funnel and adoption metrics; campaign performance for G8; feeds the staff-side owner KPI view (F11).

---

## 6. Key User Flows

1. **Book → arrive without the desk** — guest books on mobile (G1) → completes registration pre-arrival → checks in from the car (G2) → gets "room ready" push (G8) → phone unlocks the door (G3). Staff side: reservation lands in F2, arrival in F1.
2. **Poolside lunch** — guest scans the pool-bar QR (G15) → menu opens (G5) → orders to a lounger → order lands in staff POS (F8) → status updates → charge posts to folio (G9) → QR tip to the server.
3. **Something's wrong with the AC** — guest photographs the unit and submits a request (G4) → auto-routes to maintenance as a staff task (F5) with an SLA timer → guest watches status → pulse prompt afterwards confirms the save (G14).
4. **Filling tomorrow's spa slots** — revenue manager pushes a stage-targeted campaign (G8) → guest taps through to spa booking (G12) → slot reserved, charged to folio → booking lands in the staff activity calendar (F18).
5. **Departure without the queue** — folio-ready push (G8) → guest reviews the live folio (G9) → pays with bKash → express check-out (G2) → e-receipt → review invite (G14) → loyalty points visible for the rebook (G13).

---

## 7. Data Model (high level)

`GuestProfile`, `StaySession` (reservation ↔ journey stage), `RegistrationCard`, `BookingRequest`, `DigitalKey` (credential, entitlements, expiry), `ServiceRequest`, `Order`/`OrderLine`, `Offer`/`OfferAcceptance`, `MessageThread`/`Message`, `Notification`/`Campaign`/`NotificationPreference`, `Folio`/`FolioEntry`, `Payment`/`Tip`, `GuidebookArticle`, `MapPOI`, `MediaAsset` (the property media library — one library of photo/video/360° assets surfaced by G1 room galleries, G6 upgrade previews, G10 showcases, G11 spot previews, and G12 experience previews), `ActivitySlot`/`ActivityBooking`/`ItineraryItem`, `AmenityUnit`/`AmenityBooking` (cabana/sunbed inventory, G12), `QueueEntry` (virtual waitlist position, G12), `LoyaltyAccount`/`PointsTransaction`/`Reward`, `CompanionLink` (companion access with scoped entitlements & spend limits, G13/G9), `FolioSplit`/`SplitShare` (group billing, G9), `FeedbackSignal`/`RecoveryCase`, `AccessLink` (signed, stay-scoped G15 session token).

Shared spine with Atrium Staff: `Property`, `Room`, `Reservation/Guest`, and `Folio` are one model across both surfaces; guest-side `ServiceRequest`, `Order`, `ActivityBooking`, and `FeedbackSignal` are the same records the staff app works as `Task/WorkOrder`, POS orders, `ActivityBooking`, and `Feedback/Glitch` ([staff data model §7](../../staff/docs/staff-mobile-app-feature-scope.md)).

---

## 8. Delivery

Atrium Guest is delivered as **one complete package** — all fifteen feature areas (G1–G15) ship together, not staged across releases, mirroring the Atrium Staff delivery model. The **relative weighting** of features is in the [Prioritization](guest-mobile-app-feature-prioritization.md); how they map onto the app shell and journey stages is in the [Navigation Map](guest-mobile-app-navigation-map.md) and [Journey-Stage Design](guest-mobile-app-journey-stage-design.md). App-less (G15) and native flows share one codebase and ship together — the QR path is the adoption default, the app the upgrade.

---

## 9. Future Considerations

In-room IoT controls (thermostat, lighting, TV cast); AI concierge over the guidebook and messaging; multi-property / chain-wide loyalty wallet; third-party excursion marketplace beyond property-sold transfers and tours; wearable keys; kiosk mode as a lobby fallback.

---

## 10. Open Questions

- Which door-lock vendors are installed at the pilot property, and do they expose a certified BLE SDK (G3)?
- Full direct-booking engine at launch (G1), or pre-arrival-first with booking as fast-follow?
- Loyalty program economics — earn/burn rates, tiers, funding of redemptions (G13)?
- Tip disbursement model — pooled vs. individual, and payout provider (G9)?
- WhatsApp Business API access and template approvals for the BD market (G7/G15)?
- Identity-verification requirements — what document scan/NID handling does BD hospitality regulation require at check-in (G2)?
- Which review platforms matter most for the pilot property's segment (G14)?

---

## 11. Risks

| Risk | Mitigation |
|------|------------|
| Guests don't install the app | G15 app-less QR/web flows as the default path; app is upgrade, never a gate |
| Door-lock integration gaps | Certified vendors only; pre-sale lock survey; physical-card fallback always |
| Folio/PMS sync errors erode trust | Real-time middleware with reconciliation; never display doubtful totals as final |
| Staff-side fulfillment lag | Guest features gated on the Atrium Staff loop being live; shared SLA timers and status |
| Payment fraud / PCI exposure | Hosted, tokenized flows on certified gateways; stay-scoped signed links; velocity checks on charge-to-folio |
| Notification fatigue → opt-outs | Stage targeting, frequency caps, preference center; journey messages prioritized over marketing |
| Guest data exposure | PII minimization, consent-based marketing, access logging, expiring app-less sessions |
