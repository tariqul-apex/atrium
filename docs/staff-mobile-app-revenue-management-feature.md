# Rate & Channel Management (manual, mobile) — Feature Deep-Dive

**Product:** Atrium Staff (Hotel / Resort Operations Mobile App)
**Feature area:** F9 — Manual Rate & Channel Management
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-14
**Status:** Draft v1.1
**Related:** [Staff Mobile App — Feature Scope](staff-mobile-app-feature-scope.md) · [Reservations Management — Deep-Dive](staff-mobile-app-reservations-management-feature.md) · [Competitive Feature Research](../HOTEL-~1.MD)

> **Scope decision (2026-07-14):** the **automated dynamic-pricing engine is out of scope** (integrate an RMS if automation is required). This feature is **manual rate management + market-intelligence views + channel sync** on mobile. Multi-property/cluster rate control is also out of scope. See [Feature Prioritization](staff-mobile-app-feature-prioritization.md).

---

## 1. Executive Summary

**Mobile Rate & Channel Management** brings **manual** pricing control, market intelligence, and channel distribution to the manager's phone — so the person responsible for revenue can see demand and change rates and availability from anywhere, not only from a back-office workstation. It keeps a human fully in control: view the rate calendar and market context, edit rates/restrictions by hand, and sync out to the channels.

The value is **revenue agility**: demand shifts fast — an event announces, a competitor drops rate, a soft weekend appears in the pace — and the manager who can respond in minutes from a phone captures rate and occupancy the desk-bound manager misses. Channel-manager sync ensures every manual rate change reaches Booking.com, Expedia, and the hotel site without a separate step.

Vendors: Stayntouch (manage rates & groups from a tablet), SiteMinder Little Hotelier (Dynamic Revenue Plus — live market intelligence on mobile), and Smart Software BD / eZee (channel-manager sync from mobile). *(Automated dynamic pricing — e.g. RoomRaccoon RaccoonRev — is out of scope; integrate an RMS if needed.)*

> **Scope note.** This is **manual mobile rate control** — distinct from the reservation-level rate handling in [Reservations Management](staff-mobile-app-reservations-management-feature.md) and the tactical front-desk override in the [Front Desk deep-dive §3.9](staff-mobile-app-front-desk-feature.md). No automated pricing engine is built.

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What managers can do from the device |
|---|------------|--------------------------------------|
| 1 | **Manual rate & group management** | Edit rate plans and group pricing/blocks by hand from a phone or tablet. |
| 2 | **Live market intelligence** | See pricing, demand, and competitive/market signals on mobile and act on them. |
| 3 | **Channel-manager sync** | Push availability/rate changes to Booking.com, Expedia, and the website automatically. |
| 4 | **Rate calendar & restrictions** | View/edit rates, min-stay, CTA/CTD, and stop-sells across dates. |
| 5 | **Pace & forecast view** | Booking pace and forward demand to inform manual pricing. |

### 2.2 In scope (this release)
Manual rate & group management, live market-intelligence views, rate calendar + restrictions, pace/forecast view, channel-manager sync.

### 2.3 Out of scope (this release)
**Automated dynamic pricing / RMS optimization engine, approval-of-automated-recommendations workflow, multi-property/cluster rate control,** enterprise group-pricing/contracting suites, and total-revenue (F&B/ancillary) optimization. Where a dedicated RMS/channel manager exists, Atrium **surfaces and syncs** rather than replaces it. (See §9.)

### 2.4 Key assumptions
- Rates/restrictions can be read and written via the PMS or channel-manager API.
- A channel manager is connected for two-way availability/rate sync.
- Market-intelligence data (rate shopping/demand signals) is available via integration where offered.

---

## 3. Capability Deep-Dive — The Rate & Channel Features

Each capability is elaborated — **what it is**, **how it works in Atrium**, **why it matters operationally**, and the **revenue/cost angle** — with vendor precedents noting the bar to clear.

### 3.1 Automated dynamic pricing — ❌ REMOVED FROM SCOPE
*(Removed 2026-07-14. Precedent: RoomRaccoon RaccoonRev. Rates that adjust automatically to demand are valuable but a large build with a narrow user base — **integrate an RMS** if automation is required. This feature keeps pricing manual.)*

### 3.2 Manual rate & group management from a tablet
*Precedent: Stayntouch.*

**What it is.** Adjusting rate plans and group pricing/blocks **by hand** from a tablet or phone.

**How it works in Atrium.** Managers edit rate plans, seasonal rates, and group block pricing on the device; changes validate and sync out. Group blocks coordinate with the [reservations](staff-mobile-app-reservations-management-feature.md) layer so pricing and inventory stay aligned.

**Why it matters.** Rate and group decisions often need to happen during busy periods or off-site (a sales call, an event walk-through). Mobile control means the decision-maker acts when the opportunity appears, not when they're next at a terminal.

**Revenue / cost angle.** Timely rate/group decisions capture group and transient revenue (revenue); mobile access removes decision latency (cost/opportunity).

### 3.3 Live market intelligence on mobile
*Precedent: SiteMinder (Little Hotelier — Dynamic Revenue Plus).*

**What it is.** Real-time pricing, demand, and competitive/market signals surfaced on mobile so managers can act on distribution and pricing from the phone.

**How it works in Atrium.** Market and demand signals (rate positioning, demand indicators) are presented with the property's own pace and occupancy, so the manager sees both sides — market context and internal pace — and acts (adjust rate manually, open/close channels) in the app.

**Why it matters.** Pricing decisions are only as good as the market context behind them. Mobile market intelligence puts that context in the manager's hand at the moment of decision, turning reactive pricing into informed, timely moves.

**Revenue / cost angle.** Better-informed, faster manual pricing/distribution decisions lift RevPAR and capture share (revenue).

### 3.4 Channel-manager sync from mobile
*Precedent: Smart Software (BD), eZee.*

**What it is.** Automatic synchronization of availability and rate changes across OTAs (Booking.com, Expedia) and the hotel website — driven from mobile.

**How it works in Atrium.** Any manual rate/availability change made in-app propagates through the connected channel manager to all channels, and bookings flow back — keeping availability accurate everywhere and preventing oversell. The manager never has to log into each extranet.

**Why it matters.** Distribution is where rate decisions become revenue. Without sync, a mobile rate change is stranded; with it, one action updates the whole distribution landscape instantly and safely. It's also the guardrail against the oversell that manual multi-channel updates cause.

**Revenue / cost angle.** Instant, consistent distribution captures demand across channels and prevents oversell/walk cost (revenue + cost); eliminates manual extranet updates (cost).

---

## 4. Why This Matters

### 4.1 Demand moves faster than a desk-bound manager
Events, competitor moves, and pace shifts happen continuously. Mobile rate control lets the revenue owner respond in minutes from anywhere — the difference between capturing a demand spike and missing it.

### 4.2 A rate change is only revenue once it's distributed
Channel-manager sync is what turns a manual pricing decision into bookings across every channel — safely, without oversell. Mobile rate control without distribution is half a feature.

### 4.3 Manual, with market context, beats blind manual
Market intelligence on the same screen as the property's pace makes each manual rate move informed rather than a guess.

### 4.4 A competitive opening in the BD market
Mobile-accessible manual rate control with market intelligence and channel sync is largely absent among BD vendors (some offer channel sync alone). Atrium can lead locally by bringing the full manual loop to mobile.

---

## 5. Operations It Improves

| Operation today | Pain | With Mobile Rate & Channel Management |
|-----------------|------|--------------------------------|
| **Reacting to demand spikes** | Slow, desk-bound rate changes | Manually adjust and distribute rate in minutes from mobile. |
| **Group pricing** | Requires back-office access | Manage rates & group blocks from a tablet. |
| **Market awareness** | Delayed, separate tools | Live market intelligence on mobile. |
| **Multi-channel updates** | Manual extranet edits; oversell risk | One change syncs all channels via CM. |

---

## 6. How It Increases Revenue

1. **Faster demand response.** Mobile access compresses the time from demand signal to manual rate action, capturing spikes competitors miss.
2. **Informed pricing decisions.** Live market intelligence improves the quality of rate and distribution moves.
3. **Consistent, wide distribution.** Channel sync ensures every manual rate decision reaches every channel instantly, maximizing capture.
4. **Timely group/transient balance.** Mobile rate/group control optimizes the mix when opportunities appear.

> **Illustrative model:** Even a modest RevPAR uplift from more responsive **manual** pricing compounds across every room-night. For a 150-room hotel, a 3–5% RevPAR gain on a ~$90 base is meaningful six-figure annual revenue — here made mobile-actionable without an automated engine. *(Figures illustrative; validate against property data.)*

---

## 7. How It Reduces Operational Cost

1. **No extranet drudgery.** Channel sync eliminates logging into each OTA to update rates/availability.
2. **Fewer oversell incidents.** Consistent CM sync prevents the walks/comps that manual multi-channel updates cause.
3. **Decision latency removed.** Mobile access avoids the opportunity cost of waiting to reach a workstation.

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Revenue lift | RevPAR vs. baseline | ↑ 3–5% (illustrative) |
| Response speed | Time from demand signal to rate change | < 15 minutes |
| Distribution integrity | Oversell incidents from rate/availability | → near zero |
| Manager mobility | Rate actions performed on mobile | ≥ 50% |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| Rate read/write | Needs PMS or CM rate API | Integrate; reconciliation |
| Channel manager | Two-way sync required for distribution | Certified CM integration; reconciliation |
| Market-intel data | Rate-shop/demand feeds may be third-party | Integrate where available; degrade gracefully |
| Oversell risk | Rate/availability errors across channels | Server-side validation; CM guardrails |
| Scope | Building a full RMS is a large undertaking | **Out of scope** — surface & sync only; integrate an RMS for automation |

---

## 10. Release Phasing

| Phase | Scope |
|-------|-------|
| **MVP** | Rate calendar + restrictions on mobile, manual rate & group management, channel-manager sync (availability/rate), pace/forecast view. |
| **Phase 2** | Live market-intelligence views. |
| **Phase 3** | RMS integration depth (if automated pricing is adopted later). *(Automated dynamic pricing and multi-property control are out of scope for v1.)* |

---

## 11. Open Questions

- Which channel manager(s) must be supported for two-way sync at launch?
- Is third-party market-intelligence/rate-shopping data in scope, and from whom?
- If automation is wanted later, which RMS would Atrium integrate?
- Single-property scope confirmed (multi-property/cluster is out of scope)?

---

## 12. One-Line Business Case

> **Mobile Rate & Channel Management brings manual pricing, market intelligence, and channel distribution to the manager's phone — turning demand signals into distributed rate changes in minutes, keeping a human in control without building an automated pricing engine.**
