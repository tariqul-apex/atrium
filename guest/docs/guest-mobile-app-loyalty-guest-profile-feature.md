# Loyalty & Guest Profile — Feature Deep-Dive

**Product:** Atrium Guest (Hotel / Resort Guest-Facing Mobile App)
**Feature area:** G13 — Loyalty & Guest Profile
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-16
**Status:** Draft v1.0
**Related:** [Guest Mobile App — Feature Scope](guest-mobile-app-feature-scope.md) · [Competitive Feature Research](../../HOTEL-~1.MD)

---

## 1. Executive Summary

**Loyalty & Guest Profile** is the guest's identity across stays: one account holding contact details, saved documents, preferences, stay history, receipts, points, and tier status. Points earn on room, F&B, and activity spend; tiers unlock visible perks (early check-in, late check-out, upgrade priority); rewards redeem in-app.

Two economics make G13 the strategic anchor of the guest app. First, **repeat direct bookings**: a guest with points to protect and perks to use has a standing reason to book direct instead of through a commissioned OTA — commission avoided is the cleanest revenue math in this document set. Second, **the profile is the substrate of personalization**: the same identity that earns points lets [G6 offers](guest-mobile-app-upsells-offers-feature.md) target, [G8 campaigns](guest-mobile-app-notifications-campaigns-feature.md) segment, and [G1 registration](guest-mobile-app-booking-pre-arrival-feature.md) pre-fill. Without G13, every feature treats every guest as a stranger, every stay.

The pattern is proven — Maestro's guest loyalty module, tier tracking and redemption from the resort guest-app specialists (LoungeUp, STAY), and profile-driven personalization at the core of Agilysys, Mews Guest Intelligence, and Revinate CRM. In Bangladesh, where almost no local vendor ships a guest-facing app, a visible loyalty program in the guest's pocket is open ground.

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What the guest can do from the app |
|---|------------|------------------------------------|
| 1 | **Guest account & profile** | One account: contact details, saved documents for faster future check-in, preferences (room, pillow, dietary, language). |
| 2 | **Loyalty points earning** | Earn automatically on room, F&B, and activity spend posted to the folio. |
| 3 | **Tier progress & visible perks** | See current tier, progress, and what each tier grants — early check-in, late check-out, upgrade priority. |
| 4 | **In-app redemption** | Redeem points for upgrades, dining discounts, free nights — applied instantly. |
| 5 | **Personalization engine** | The profile feeds G6 offer targeting, G8 campaign segmentation, and pre-fills G1 registration. |
| 6 | **Stay history & receipts** | Browse past stays with itemized receipts; rebook in one tap. |
| 7 | **Family / companion profiles** | Add companions; preferences carry into bookings and key sharing (G3). |
| 8 | **Group & companion stay management** | Invite companions by link to scoped app access for the stay — own key, own requests and orders, shared group itinerary, linked multi-room reservations, organizer spend controls. |
| 9 | **Privacy controls & consent** | See held data, manage consent per channel, request deletion. |

### 2.2 In scope (this release)
Account and sign-in (including app-less web flows, G15); profile and preferences; document vault; points earning on folio-posted spend; tier engine with configurable thresholds and perks; in-app redemption; personalization feeds to G1/G6/G8; stay history and receipts; companion profiles; group & companion stay management (invite-by-link companion access, shared group itinerary, multi-room stay linking, organizer spend controls, shared group chat thread via G7); consent and deletion requests; Bangla/English UI.

### 2.3 Out of scope (this release)
Multi-property chain loyalty federation (single-property first); points purchase/transfer/gifting; co-brand card or airline-mile partnerships; a standalone marketing-automation suite (Atrium feeds a Revinate-class CRM where one exists); paid membership clubs.

### 2.4 Key assumptions
- The PMS/folio layer (via [G9](guest-mobile-app-payments-folio-tipping-feature.md) and the staff platform) exposes posted spend by category.
- The property defines earn rates, tier thresholds, and perk grants — Atrium supplies the engine, not the program design.
- Inventory-touching redemptions (upgrades, free nights) check live availability via G6's channels — a promised perk is always deliverable.
- Guest PII is stored per applicable data-protection rules (including Bangladesh's evolving regime) — minimized, purpose-bound, deletable.

---

## 3. Capability Deep-Dive — The Nine Profile & Loyalty Features

Per the competitive research (Part A §A8, *Loyalty & Personalization*), each capability below covers **what it is**, **how it works in Atrium**, **why it matters**, and the **revenue/cost angle**, with vendor precedents noting the bar to clear.

### 3.1 Guest account & profile
*Precedent: Agilysys, Mews Guest Intelligence, Revinate CRM.*

**What it is.** One durable identity: contact details, consented documents for faster future check-ins, and preferences — room, floor, pillow, dietary, language.

**How it works in Atrium.** Account created at booking, in-stay, or post-stay via app-less link (G15). Documents captured at check-in (G2) store encrypted so the next arrival skips the scan; preferences carry into every future reservation and surface to staff (F1).

**Why it matters.** Never re-typing a passport number or re-explaining a feather allergy is the difference between being a *reservation* and being a *guest* — recognition is the core of loyalty before any point is earned.

**Revenue / cost angle.** Recognition drives repeat direct rebooking (revenue); documents and preferences on file cut minutes off every repeat arrival (cost).

### 3.2 Loyalty points earning on room, F&B, and activity spend
*Precedent: Maestro guest loyalty module; resort guest-app pattern (LoungeUp, STAY).*

**What it is.** Points that accrue automatically on what the guest actually spends — the room, the G5 dinner, the G12 spa treatment.

**How it works in Atrium.** Every folio charge carries a category; the earn engine applies the property's rate per category and credits points in near-real time — the balance visibly ticks up during the stay.

**Why it matters.** Earning on *all* spend turns every ancillary purchase into progress — a guest near a threshold books the second spa treatment on property, not off it.

**Revenue / cost angle.** Earn-on-everything nudges ancillary spend on-property (revenue); automatic accrual means zero points administration (cost).

### 3.3 Tier progress with visible perks
*Precedent: resort guest-app specialists — VIP perks, tier tracking, reward-progress notifications (LoungeUp, STAY).*

**What it is.** A tier ladder the guest can see: current status, distance to the next tier, and precisely what each tier grants.

**How it works in Atrium.** The profile shows tier, progress bar, and perk table. Perks are *operationalized*: a Gold guest's late check-out is honored automatically at departure (G2), upgrade priority feeds room assignment in staff F1, "one stay to Gold" nudges go out through G8.

**Why it matters.** Programs fail when status is invisible or perks theoretical. Visible progress creates the goal-gradient effect — guests accelerate spend near a threshold — and perks that *actually happen* make booking direct worth it.

**Revenue / cost angle.** Threshold-chasing lifts stay frequency and spend (revenue); early/late check-out perks cost little off-peak but anchor the direct-booking habit (cost).

### 3.4 In-app redemption — upgrades, dining discounts, free nights
*Precedent: Maestro guest loyalty module — redeem points for upgrades, amenities, dining discounts, free nights.*

**What it is.** Spending points as easily as earning them: an upgrade on this booking, a discount on tonight's dinner, a free night on the next reservation.

**How it works in Atrium.** The rewards screen shows what the balance affords; redemption checks live availability and applies instantly — upgrade re-assigned, discount on the folio, free night booked through G1. No vouchers, no desk negotiation.

**Why it matters.** Redemption friction is where programs die — hard-to-spend points read as worthless. Self-service redemption also puts a reward moment *inside* the stay, paying off in real time.

**Revenue / cost angle.** Easy redemption makes earning motivating (revenue); redemptions steer to marginal-cost inventory — an empty suite, an off-peak table — so perceived value far exceeds delivered cost (cost).

### 3.5 Personalization engine — feeding G6, G8, and G1
*Precedent: Revinate CRM, Mews Guest Intelligence (unified profiles powering targeted campaigns and upsells); Agilysys.*

**What it is.** The profile as an input, not just a record: preferences, history, and spend patterns drive what the app shows this guest.

**How it works in Atrium.** The profile exposes a feed the other features consume: G6 targets offers by history and party, G8 segments campaigns by interest and tier, G1 pre-fills registration to one confirmation tap.

**Why it matters.** Untargeted offers are spam; targeted offers are service. Every personalization claim in this document set depends on this engine — G13 is where "the app knows the guest" is actually implemented.

**Revenue / cost angle.** Vendor evidence (Canary, Duve, Revinate) puts targeting at the center of upsell uplift — the multiplier on every G6/G8 conversion (revenue); pre-fill removes repeat data entry (cost).

### 3.6 Stay history & receipts archive
*Precedent: guest-CRM pattern — rich profiles from PMS data (Revinate, Cendyn-class).*

**What it is.** Every past stay, itemized folio, and receipt in one archive the guest can browse, re-download, and rebook from.

**How it works in Atrium.** Closed folios from G9 archive automatically; each past stay shows dates, room, and spend, with a one-tap "book this again" that seeds a new G1 reservation with the same room and preferences.

**Why it matters.** For business travelers, self-service receipts end the "email me my invoice" tickets; for leisure guests, "the sea-view room from last winter" is one tap from rebooked.

**Revenue / cost angle.** "Book this again" is the shortest path to a repeat direct booking (revenue); self-service receipts remove a back-office chore (cost).

### 3.7 Family / companion profiles
*Precedent: resort guest-app pattern — group and family travel is the resort norm (STAY, LoungeUp).*

**What it is.** One account holding the whole travel party: companions with their own preferences (a child's dietary needs, a partner's pillow choice).

**How it works in Atrium.** Companions are added once; their details flow into future bookings, key sharing (G3), and dining personalization (G5/G6) — held under the primary guest's consent, deletable with the account.

**Why it matters.** Resorts sell to *parties*, not individuals. Making the family effortless to rebook binds the highest-value stay type — the multi-room family booking — to the direct channel through its organizer.

**Revenue / cost angle.** Retaining the organizer retains the party's multi-room, activity-heavy spend (revenue); companion details on file remove a group's worth of form-filling (cost).

### 3.8 Group & companion stay management
*Precedent: largely open ground — group-travel tooling is a gap most hotel guest apps leave unaddressed; companion access exists partially as key-sharing (Mews, Operto-pattern), but the full organizer toolset — scoped access, shared itinerary, spend controls — has no clean vendor precedent.*

**What it is.** The companion profiles of §3.7, made *live for the current stay*: the booker — the family-organizer persona — invites companions by link, and each companion gets their own scoped app access to the stay without needing the booker's account. Each companion holds their own digital key ([G3](guest-mobile-app-digital-room-key-feature.md)), makes their own [G4](guest-mobile-app-in-stay-service-requests-feature.md) requests and [G5](guest-mobile-app-food-beverage-ordering-feature.md) orders, and sees the itinerary — shared or personal view. A **shared group itinerary** aggregates every member's [G12 bookings](guest-mobile-app-activities-spa-experiences-feature.md) into one plan visible to all; multi-room group stays link their reservations so the organizer sees the whole party in one place.

**How it works in Atrium.** The organizer sends an invite link (app or app-less, G15); the companion accepts and is scoped to this stay — key, requests, orders, itinerary — under permissions the organizer sets. **Spend controls** let the organizer decide which companions may charge to the folio, with per-person limits, enforced at the payment layer ([G9](guest-mobile-app-payments-folio-tipping-feature.md)). Linked multi-room reservations roll up into the organizer's group view; every member's G12 bookings appear on the shared itinerary with reminders. A group stay also gets a shared group chat thread with the property ([G7](guest-mobile-app-guest-messaging-feature.md)).

**Why it matters.** Resorts sell to parties, but guest apps serve one phone per reservation — the organizer becomes the party's human switchboard ("what time is the kids' club?", "can you order for the other room?"). Giving every companion their own scoped surface removes that bottleneck, and the shared itinerary is what makes a multi-person resort stay legible: everyone knows where everyone is booked.

**Revenue / cost angle.** More devices per stay means more ordering and booking surfaces — each companion is an independent upsell target for G5/G6/G12, and group tooling differentiates Atrium for the resort family segment, the highest-value stay type (revenue); companion self-service removes the "can you add my husband's key" and "what did the kids order" interactions from the desk, and the organizer's spend controls pre-empt the checkout disputes group folios generate (cost).

### 3.9 Privacy controls & consent
*Precedent: table-stakes for any guest CRM under modern data-protection regimes; a differentiator where competitors treat it as an afterthought.*

**What it is.** Guest-facing transparency and control: what the property holds, per-channel marketing consent, and a working deletion request.

**How it works in Atrium.** A privacy screen shows data categories and consent toggles per channel (push, email, SMS/WhatsApp), plus a deletion request that erases PII while keeping legally required anonymized financial records. Documents encrypted; staff access RBAC-gated and logged.

**Why it matters.** G13 holds the product's most sensitive data — identity documents, children's details, spend patterns. With Bangladesh's data-protection regime taking shape and guests bringing GDPR-shaped expectations, doing this properly is both compliance and positioning.

**Revenue / cost angle.** Trust drives opt-in — and opted-in guests are the ones campaigns can legally reach (revenue); privacy-by-design avoids the cost of a data incident (cost avoided).

---

## 4. Why This Matters

### 4.1 Repeat direct guests are the most profitable revenue a property has
A repeat guest booking direct pays no OTA commission (15–25%), costs nothing to acquire, spends more on ancillaries, and complains less. Every mechanism in G13 exists to manufacture exactly this guest.

### 4.2 The profile is what makes the rest of the app personal
G6's targeting, G8's segmentation, G1's pre-fill, the arrival greeting in staff F1 — all read from the profile G13 maintains. Without it the app works, but generically; with it, the whole surface compounds per stay.

### 4.3 Loyalty is the reason to install the app at all
Most flows work app-less (G15) for the two-night guest. The native app's retention case *is* loyalty: points, tier, and history keep the icon on the phone between stays, reachable at zero marginal cost.

### 4.4 The local field is empty
Bangladeshi vendors are essentially absent on guest-facing apps — none ships a guest-visible loyalty program with local payment rails and Bangla/English UI. Atrium can define the local category rather than chase it.

---

## 5. Journey Moments & Operations It Improves

| Moment / operation today | Pain | With Loyalty & Guest Profile |
|--------------------------|------|------------------------------|
| **Rebooking a past stay** | Guest returns to the OTA — commission again | Points, perks, and "book again" pull the rebooking direct. |
| **Repeat-guest check-in** | Documents re-scanned, preferences re-asked | Registration pre-filled (G1); documents on file; preferences pre-applied. |
| **Recognition of regulars** | Depends on a veteran agent's memory | Tier and preferences surface to staff (F1) on every device. |
| **Loyalty administration** | Punch cards or a spreadsheet, if anything | Automatic earn from folio data; self-service redemption; zero admin. |
| **Perk delivery** | "I'm a member" argued at the desk | Late check-out auto-granted; upgrade priority in assignment. |
| **Receipt requests** | Emailed by back office, days later | Self-service archive — any past folio, instantly. |
| **Family rebooking** | Organizer re-enters the party's details | Companion profiles carry the party into the new booking. |
| **Marketing reach** | Untargeted blasts to a stale list | Consented, segmented, profile-driven campaigns (G8). |

**Guest friction removed:** re-registration, unrecognized status, worthless points, receipt-chasing. **Staff load removed:** repeat data capture, loyalty bookkeeping, perk arbitration, invoice-retrieval emails.

---

## 6. How It Increases Revenue

1. **OTA commission avoided on shifted rebookings.** The headline lever: every repeat booking that moves from OTA to direct saves 15–25% of room revenue — pure margin, recurring, compounding as the loyal base grows.
2. **Higher repeat rate.** Points to protect, a tier to keep, and a smoother stay give the guest a concrete reason to return rather than re-shop the market.
3. **Personalization lift across G6 and G8.** Targeted offers convert far better than generic ones — vendor evidence (Canary, Duve, Revinate) puts targeting at the center of upsell uplift.
4. **In-stay ancillary nudges.** Earn-on-everything and visible thresholds steer spa, dining, and activity spend on-property.
5. **Family and group retention.** Companion profiles bind the multi-room, activity-heavy family booking — the highest-value stay type a resort sells — to the direct channel.
6. **Redemption at marginal cost.** Rewards steered to distressed inventory deliver high perceived value at low delivered cost.

> **Illustrative model:** A 150-room resort doing **৳25 crore/year** in room revenue with 40% of bookings via OTAs at ~18% commission pays **~৳1.8 crore/year** in commission. If loyalty shifts just **10 percentage points** of bookings to direct, commission avoided is **~৳45 lakh/year** — before the repeat-rate gain, the G6/G8 personalization lift, or the ancillary spend nudged on-property. *(Figures illustrative; validate against real channel mix, ADR, and commission rates.)*

---

## 7. How It Reduces Operational Cost

1. **Zero acquisition cost on repeat direct bookings.** The loyal rebooking costs no commission and no marketing spend — the cheapest room-night a property sells.
2. **Repeat data capture eliminated.** Documents, preferences, and companion details on file remove minutes of desk work from every returning arrival.
3. **Loyalty administration automated.** Earn, tier, and redemption run from folio data — no bookkeeping, crediting, or voucher handling.
4. **Perk disputes ended.** Operationalized perks replace desk-side arbitration of "I was promised late check-out."
5. **Receipt chores removed.** The self-service archive eliminates back-office email requests.
6. **Marketing efficiency.** Consented reach through owned channels replaces paid re-acquisition of guests already served.

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Direct-channel shift | Share of repeat bookings made direct | ↑ 10 pts vs. baseline in year 1 |
| Program adoption | Guests with an active profile / eligible guests | ≥ 40% |
| Repeat behavior | Repeat-stay rate, members vs. non-members | Members ≥ 1.5× |
| Engagement | Members redeeming ≥ 1 reward per year | ≥ 30% of active members |
| Personalization lift | G6 offer conversion, members vs. anonymous | Members measurably higher |
| Group activation | Companions activated per group/family stay | ≥ 1.5 *(illustrative)* |
| Check-in speed | Repeat-guest registration time vs. first-time | ↓ 50%+ (pre-filled) |
| Consent health | Members with marketing consent granted | ≥ 60% |
| Trust | Deletion requests fulfilled within SLA | 100% |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| Folio/PMS data feed | Earn needs categorized spend in near-real time | Sync via the platform folio layer (G9/staff); batch reconciliation fallback |
| Program economics | Over-generous earn/redemption erodes margin | Configurable rates; marginal-cost redemption; liability reporting |
| Perk deliverability | Upgrades, late check-out need live inventory | Share G6's availability logic; blackout rules |
| PII & documents | The app's most sensitive data store | Encryption, PII minimization, RBAC + access logging, deletion workflow |
| Data-protection law | BD regime evolving; guests bring GDPR expectations | Consent-first design; purpose-bound storage; legal review |
| Companion identity & consent | Companions join by link, not by verified account; minors' data in the party | Stay-scoped invite tokens revocable by the organizer; permissions organizer-granted; minors' data held under organizer consent, minimized, deleted with the account |
| CRM overlap | Property may run Revinate/Cendyn-class CRM | Feed/sync rather than duplicate; define system of record |
| Cold start | No members at launch | Post-stay claim invitations via app-less links (G15); enroll at check-in (G2) |
| Points liability | Accrued points are a balance-sheet obligation | Expiry policy; owner liability dashboard (staff F11) |

---

## 10. Scope delivered

Atrium Guest ships as one complete package. This feature delivers:

- Guest account with sign-in across app and app-less (G15) surfaces
- Profile & preferences (room, pillow, dietary, language)
- Consented document vault for faster future check-ins
- Points earning on room, F&B, and activity folio spend
- Tier engine with visible progress and operationalized perks
- In-app redemption: upgrades, dining discounts, free nights
- Personalization feed to G6 offers, G8 campaigns, G1 pre-fill
- Stay history & receipts archive with "book again"
- Family/companion profiles
- Group & companion stay management: invite-by-link scoped companion access (own key, requests, orders), shared group itinerary across G12 bookings, multi-room stay linking, organizer spend controls with per-companion limits (enforced via G9), shared group chat thread with the property (via G7)
- Privacy screen: transparency, per-channel consent, deletion request
- Bangla/English bilingual profile UI

---

## 11. Open Questions

- Single-property programs at launch, or does the first customer need multi-property federation?
- Who designs program mechanics (earn rates, tiers, perks) — a console in Atrium Staff, or onboarding-time configuration?
- Is Atrium the guest-profile system of record, or does it sync with an incumbent CRM?
- Which document types may be retained, for how long, under which BD data-protection provisions?
- Do points expire, and how is the points liability reported to owners?
- Can day visitors and event guests (no room stay) earn on F&B/activity spend?

---

## 12. One-Line Business Case

> **Loyalty & Guest Profile turns one-time OTA bookings into repeat direct relationships — commission-free rebookings, perks that cost little but bind guests to the property, and the identity layer that makes every personalized offer, campaign, and pre-filled arrival in the rest of the app possible.**
