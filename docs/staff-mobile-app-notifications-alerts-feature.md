# Notifications & Alerts on Mobile — Feature Deep-Dive

**Product:** Atrium Staff (Hotel / Resort Operations Mobile App)
**Feature area:** F6 — Staff Notifications & Alerts
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-14
**Status:** Draft v1.0
**Related:** [Staff Mobile App — Feature Scope](staff-mobile-app-feature-scope.md) · [Task Management & Communication — Deep-Dive](staff-mobile-app-task-management-communication-feature.md) · [Competitive Feature Research](../HOTEL-~1.MD)

---

## 1. Executive Summary

**Staff Notifications & Alerts** is the real-time nervous system that pushes the right event to the right staff member the moment it happens — a new booking, a cancellation, a guest message, an SLA breach, a VIP arrival. It is what makes every other feature *timely*: a work order, a room-ready status, or a guest request only creates value if someone acts on it quickly, and notifications are what close the gap between "event occurred" and "staff aware."

The design challenge is not *whether* to notify but *how* to notify without drowning staff. The value is in **relevance and timeliness**: filterable, prioritized, per-role, multi-property alerts with history — plus critical alerts that bypass Do-Not-Disturb — so staff trust the stream and act on it.

Vendors ship this as a core mobile capability: Cloudbeds (filterable real-time push with history, multi-property), SiteMinder Little Hotelier (instant booking notifications), and eZee.

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What staff receive on the device |
|---|------------|----------------------------------|
| 1 | **Real-time push notifications** | New bookings, modifications, cancellations, guest messages, task assignments — as they happen. |
| 2 | **Filterable & prioritized** | Filter by type, department, property; priority tiers so critical rises to the top. |
| 3 | **Notification history** | A searchable log of past alerts — nothing lost if missed live. |
| 4 | **Multi-property alerts** | Notifications across all properties a user is assigned to, not just the active one. |
| 5 | **Per-user preferences & quiet hours** | Each user tunes what they receive and when. |
| 6 | **Critical/emergency bypass** | Life-safety, VIP-arrival, and escalation alerts override Do-Not-Disturb. |
| 7 | **Tap-through to action** | Every alert deep-links to the relevant reservation, task, room, or thread. |
| 8 | **In-app inbox fallback** | If push fails, alerts still appear in an in-app inbox. |

### 2.2 In scope (this release)
Real-time push for bookings/cancellations/messages/tasks/SLA, filtering + priority tiers, notification history, multi-property alerts, per-user preferences + quiet hours, critical bypass, deep-link tap-through, in-app inbox.

### 2.3 Out of scope (this release)
Guest-facing notifications (guest app), marketing campaign delivery, and complex rules-engine automation beyond routing/priority. (See §9.)

### 2.4 Key assumptions
- Push infrastructure (APNs/FCM) and event streams from PMS/other features are available.
- Users have per-role, per-property assignments to scope notifications.
- Device notification permissions are granted (with an in-app fallback if not).

---

## 3. Capability Deep-Dive — The Notification Features

The competitive research (Part B §6, *Notifications & Alerts*) identified the capabilities below. Each is elaborated — **what it is**, **how it works in Atrium**, **why it matters operationally**, and the **revenue/cost angle** — with vendor precedents noting the bar to clear.

### 3.1 Real-time push notifications (filterable, with history)
*Precedent: Cloudbeds, SiteMinder (Little Hotelier), eZee.*

**What it is.** Instant push alerts for operationally important events — new bookings, modifications, cancellations, guest messages, task assignments — that staff can filter by type/department and review later in a searchable history.

**How it works in Atrium.** Events from the PMS and from other features (a new task, an SLA warning) fan out as push notifications scoped to the relevant staff. Users filter the stream (e.g., "only my tasks," "only front-office bookings"), and every alert is retained in history so a missed live notification is never truly lost.

**Why it matters.** Timeliness is the whole game: an OTA booking acted on in minutes vs. hours, a guest message answered now vs. later, an SLA warning caught before breach. Filtering and history are what make a high-volume stream usable rather than noise — staff see what's theirs and can always look back.

**Revenue / cost angle.** Fast reaction to bookings and guest messages captures demand and satisfaction (revenue); history and filtering cut the time staff spend chasing status (cost).

### 3.2 Multi-property notifications
*Precedent: Cloudbeds.*

**What it is.** Alerts spanning every property a user is assigned to — not just the one currently open — so cluster and multi-property staff stay aware across the portfolio.

**How it works in Atrium.** A user assigned to several properties receives (clearly labeled) notifications from all of them, filterable by property. A cluster revenue manager or central reservations agent doesn't have to switch context to know what's happening where.

**Why it matters.** Chains and clusters increasingly run shared roles across properties. Multi-property alerting is what lets one person cover several properties effectively without missing time-sensitive events at any of them.

**Revenue / cost angle.** Enables leaner cluster/central staffing across a portfolio (cost); ensures demand and issues at every property get timely attention (revenue).

### 3.3 Preferences, quiet hours & critical bypass
*Precedent: platform best-practice; complements Cloudbeds/eZee real-time model.*

**What it is.** Per-user control over what is received and when (quiet hours), with a critical tier (emergency, VIP arrival, escalation) that bypasses Do-Not-Disturb.

**How it works in Atrium.** Each user configures notification types and quiet windows appropriate to their role and shift. Life-safety and designated critical alerts ignore quiet hours and device DND to guarantee delivery.

**Why it matters.** Without controls, notification fatigue sets in and staff ignore everything — including the alert that mattered. Preferences preserve signal; the critical bypass guarantees the rare must-see alert always gets through.

**Revenue / cost angle.** Preserving trust in the stream keeps response times low (revenue + cost); guaranteed critical delivery protects safety and VIP experience (risk/revenue).

---

## 4. Why This Matters

### 4.1 Notifications make every other feature timely
A work order, a room-ready flag, or a guest request only creates value when acted on quickly. Notifications are the mechanism that turns events into prompt action across the whole app.

### 4.2 Relevance is the feature, not volume
The hard part is delivering signal without noise. Filtering, priority, per-role scoping, and quiet hours are what keep staff trusting and acting on alerts instead of muting them.

### 4.3 Timeliness is directly monetizable
Reacting to an OTA booking, a cancellation to resell, or a guest message in minutes rather than hours has a direct revenue and satisfaction impact.

### 4.4 A competitive opening in the BD market
Filterable, multi-property, priority-tiered staff notifications with history are not standard among BD vendors. Atrium can lead locally.

---

## 5. Operations It Improves

| Operation today | Pain | With Notifications & Alerts |
|-----------------|------|-----------------------------|
| **New OTA/phone bookings** | Seen only when someone checks | Instant push; act on demand immediately. |
| **Cancellations** | Room sits unsold | Alert enables prompt resell. |
| **Guest messages** | Slow replies | Real-time push to the right team. |
| **SLA breaches** | Discovered too late | Priority escalation alerts. |
| **VIP arrivals** | Missed prep | Critical alert bypasses DND. |
| **Multi-property coverage** | Context-switching to check | Portfolio-wide alerts, filterable. |
| **Missed alerts** | Lost forever | Searchable notification history. |

---

## 6. How It Increases Revenue

1. **Faster demand capture.** Instant booking and cancellation alerts let staff act on demand and resell in minutes, not hours.
2. **Quicker guest response.** Real-time message alerts drive faster replies that lift satisfaction, conversion, and reviews.
3. **VIP experience protection.** Guaranteed VIP-arrival alerts ensure preparation that drives loyalty and premium spend.
4. **SLA adherence.** Escalation alerts prevent service failures that cost reviews and rate power.

---

## 7. How It Reduces Operational Cost

1. **Less status-chasing.** Push + history removes the manual checking staff do to stay informed.
2. **Leaner cluster staffing.** Multi-property alerts let fewer people cover more properties.
3. **Fewer missed-event costs.** Timely alerts prevent the comps, resell losses, and escalations that missed events cause.
4. **Preserved attention.** Filtering and quiet hours reduce notification fatigue that otherwise wastes attention and hides real signals.

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Faster reaction | Median event-to-acknowledgement time | < 5 minutes |
| Signal trust | Notification open/action rate | ≥ target % |
| No lost events | Critical alerts delivered/acknowledged | 100% |
| Multi-property coverage | Cross-property events acted on within SLA | ≥ 90% |
| Fatigue control | Alerts muted/disabled by users | ↓ / low |
| Resilience | Alerts delivered via inbox when push fails | 100% |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| Push infrastructure | APNs/FCM reliability | In-app inbox fallback; delivery monitoring |
| Event streams | Needs events from PMS + features | Standard event bus; graceful degradation |
| Notification fatigue | Over-alerting kills trust | Filtering, priority tiers, quiet hours |
| Permissions | Users may deny device push | In-app inbox; onboarding prompt |
| Multi-property scoping | Wrong-property noise | Clear labeling + per-property filters |
| Critical bypass abuse | Overuse of DND-override tier | Restrict critical tier by policy/RBAC |

---

## 10. Release Phasing

| Phase | Scope |
|-------|-------|
| **MVP** | Real-time push for bookings/cancellations/messages/tasks, tap-through deep-links, in-app inbox, basic per-user preferences. |
| **Phase 2** | Filtering + priority tiers, notification history, quiet hours, critical/DND bypass. |
| **Phase 3** | Multi-property notifications, delivery analytics, advanced routing rules. |

---

## 11. Open Questions

- Which events are in scope for launch, and what are their priority tiers?
- What qualifies for the critical/DND-bypass tier, and who governs it?
- Is multi-property notification needed at launch or a later phase?
- What are the default quiet-hours and preference defaults per role?
- Delivery-monitoring/SLAs required for critical alerts?

---

## 12. One-Line Business Case

> **Staff Notifications & Alerts is the nervous system that makes every other feature timely — pushing the right event to the right person the instant it happens, with the relevance controls that keep staff trusting and acting on the stream.**
