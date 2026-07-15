# Guest Feedback & Service Recovery on Mobile — Feature Deep-Dive

**Product:** Atrium Staff (Hotel / Resort Operations Mobile App)
**Feature area:** F14 — Guest Feedback & Service Recovery
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-15
**Status:** Draft v1.0
**Related:** [Feature Scope](staff-mobile-app-feature-scope.md) · [Task Management — Deep-Dive](staff-mobile-app-task-management-communication-feature.md) · [Notifications — Deep-Dive](staff-mobile-app-notifications-alerts-feature.md) · [Competitive Feature Research](../HOTEL-~1.MD)

---

## 1. Executive Summary

**Guest Feedback & Service Recovery** gives staff a handheld way to hear what a guest thinks *while they are still on property* and to act on it before they leave. Staff capture in-stay feedback and NPS (either directly on the device or via a simple link/QR they surface to the guest), a negative signal fires an alert to the duty manager, and the issue drops into a service-recovery workflow — apologise, compensate, fix — that is assigned to the right department and tracked to closure.

Unlike the cost-focused features of the control layer (F9–F13), F14 is primarily a **revenue-protection and retention** feature. Its whole point is to **catch and fix a problem before checkout**: a glitch recovered in-stay costs far less than a refund argued at the desk, and it protects the review score that drives repeat and direct bookings — and lowers OTA commission dependence.

**Scope guard — this is a *staff-side* feature.** Atrium is a staff operations app. The full guest-facing companion app is out of scope (see [Feature Scope §2.3](staff-mobile-app-feature-scope.md)). F14 is staff-assisted feedback capture and the recovery workflow that follows — not a guest companion app. Where a guest self-serves, it is through a lightweight link or QR that staff surface, with the *recovery* handled entirely on the staff side.

Vendors treat guest feedback as a mature space: SuitePad captures in-room feedback, Revinate builds a guest CRM with Ivy two-way messaging and review-driven marketing, and two-way messaging hubs (Mews, Canary, Duve, Guestline Keez) route guest sentiment to the front desk. Complaint and incident handling is well-trodden by Knowcross and Quore. Guest-facing feedback apps are common; a **staff-side service-recovery workflow on mobile** is the Atrium angle.

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What staff can do from the device |
|---|------------|-----------------------------------|
| 1 | **In-stay feedback / NPS capture** | Record guest sentiment mid-stay — staff-assisted on the device, or via a simple link/QR the staff surfaces to the guest. |
| 2 | **Negative-signal alerts** | A low score or complaint routes an alert to the duty manager through [F6 Notifications](staff-mobile-app-notifications-alerts-feature.md). |
| 3 | **Complaint / glitch log + recovery workflow** | Log the issue; run the service-recovery play — apologise / comp / fix — and assign it to the responsible department. |
| 4 | **Resolution tracking** | Track a glitch to closure with owner, action taken, and outcome recorded. |
| 5 | **Recovery-action routing** | Push recovery actions into [F5 Task routing](staff-mobile-app-task-management-communication-feature.md) so the fix is owned and tracked like any other task. |

### 2.2 In scope (this release)
Staff-assisted in-stay feedback / NPS capture; link/QR feedback surfaced by staff; negative-signal alerting to the duty manager (via F6); complaint/glitch logging; service-recovery workflow (apologise/comp/fix) with department assignment; resolution tracking; recovery actions routed through F5 task management; RBAC on comp authorization.

### 2.3 Out of scope (this release)
The **guest-facing companion app** (all releases — see [Feature Scope §2.3](staff-mobile-app-feature-scope.md)); full review-platform publishing/management; a full guest CRM and marketing-campaign engine; automated post-stay email/survey marketing. (Integrate with a review platform or CRM where one exists — see §9.)

### 2.4 Key assumptions
- Feedback is captured *staff-side*; any guest self-service is a lightweight link/QR, not an installed guest app.
- A feedback channel (in-app staff capture, SMS/link, or QR) is available and permitted for guest contact.
- The property has a duty manager (or equivalent) to receive negative-signal alerts.

---

## 3. Capability Deep-Dive — The Feedback & Recovery Features

Each capability is elaborated — **what it is**, **how it works in Atrium**, **why it matters operationally**, and the **revenue/cost angle** — with vendor precedents noting the bar to clear.

### 3.1 In-stay feedback & NPS capture (staff-assisted or link/QR)
*Precedent: SuitePad (in-room feedback), Revinate (guest CRM + review-driven marketing).*

**What it is.** Capturing how a guest feels *during* the stay — a quick NPS or satisfaction pulse — rather than waiting for a post-checkout review. Capture is staff-side: an agent records it on the device, or surfaces a simple link/QR the guest taps.

**How it works in Atrium.** A front-desk agent or attendant records a short feedback pulse against the guest's room/reservation, or hands over a QR/link at a natural touchpoint (check-in follow-up, mid-stay, housekeeping visit). Responses attach to the reservation and roll up for the duty manager. This is deliberately light — Atrium is a staff app, not a guest companion app, so the guest experience is a single link/QR, not an install.

**Why it matters.** The most expensive feedback is the one-star review posted *after* the guest has gone, when nothing can be done. Hearing a problem in-stay converts an eventual public complaint into a private, fixable one — while the guest is still on property and the outcome is still in the hotel's hands.

**Revenue / cost angle.** In-stay capture is what makes recovery possible at all; every glitch surfaced before checkout is a review protected and a refund-at-the-desk avoided (revenue protected). It costs nothing to route through existing staff touchpoints (cost).

### 3.2 Negative-signal alerts to the duty manager
*Precedent: Revinate Ivy (sentiment-driven messaging), two-way guest messaging hubs (Mews, Canary, Duve, Guestline Keez).*

**What it is.** Turning a low score or a logged complaint into an immediate, routed alert so a manager acts within the stay — not after the review lands.

**How it works in Atrium.** A detractor score or a flagged complaint fires a negative-signal alert to the duty manager through [F6 Notifications](staff-mobile-app-notifications-alerts-feature.md), carrying the room, reservation, and issue context. Critical signals can bypass Do Not Disturb per F6's alert rules. The manager triages and launches recovery from the alert itself.

**Why it matters.** Recovery only works if it is *fast*. An alert that reaches the duty manager in minutes — with the guest still in-house — is the difference between a quiet fix and a public one-star. Without routing, negative feedback sits unseen until checkout, when the only lever left is a refund.

**Revenue / cost angle.** Fast managerial response recovers at-risk guests and protects the review that drives rate power (revenue); catching it in-stay avoids the larger reactive comp negotiated at the desk (cost).

### 3.3 Complaint / glitch log with service-recovery workflow
*Precedent: Knowcross, Quore (complaint/incident handling).*

**What it is.** A structured log of complaints and "glitches" with a recovery play attached — apologise, compensate, fix — assigned to the department that owns the fix and tracked to a recorded outcome.

**How it works in Atrium.** A glitch is logged against the room/reservation with a category and severity. The recovery workflow prompts the standard play (apologise / comp / fix); comps are gated by RBAC authorization limits. The *fix* is pushed into [F5 Task routing](staff-mobile-app-task-management-communication-feature.md) and owned like any other task, while resolution tracking (§3.4) records what was done and how the guest responded. Recurring glitch types roll up so the property can attack root causes, not just symptoms.

**Why it matters.** Ad-hoc recovery — a manager improvising a comp, a note that never reaches housekeeping — is inconsistent and unmeasurable. A structured workflow makes recovery repeatable, keeps comp authority controlled, and turns a scattered set of favours into a managed process that closes the loop with the guest.

**Revenue / cost angle.** Consistent, tracked recovery retains guests and protects reviews (revenue); controlled comp authorization and a record of what was given curb over-generous or unaccountable giveaways (cost).

---

## 4. Why This Matters

### 4.1 A problem fixed in-stay is cheaper than a refund at checkout
The same glitch costs a fraction to recover in-stay — an apology and a small gesture — versus the refund or rate adjustment argued reactively at the desk. F14 moves the moment of truth earlier, where it is cheapest to resolve.

### 4.2 Reviews are revenue
Review scores drive rate power, conversion, and the share of bookings that come direct rather than through commissioned OTAs. Catching a detractor in-stay protects the score that protects the rate — and the direct-booking mix.

### 4.3 Recovery only works if it is fast and tracked
Sentiment that reaches a manager in minutes can be recovered; sentiment discovered at checkout cannot. Routing (via F6) plus a tracked workflow (via F5) is what makes recovery real rather than aspirational.

### 4.4 A staff-side recovery workflow is the local opening
Guest-facing feedback apps are common. A **staff-side** service-recovery workflow — capture, alert, recover, track — on mobile is far rarer in the BD market. Atrium can lead locally without building a guest companion app.

---

## 5. Operations It Improves

| Operation today | Pain | With Feedback & Recovery |
|-----------------|------|--------------------------|
| **Hearing guest sentiment** | Learned only from the post-stay review | Captured in-stay, staff-side, against the reservation. |
| **A negative signal** | Sits unseen until checkout | Alerts the duty manager in minutes (via F6). |
| **Handling a complaint** | Improvised, inconsistent | Structured apologise/comp/fix play, department-assigned. |
| **The actual fix** | A verbal note that gets lost | Routed as a tracked task (via F5), owned to closure. |
| **Comps & giveaways** | Unaccountable, uncontrolled | Gated by RBAC authorization limits and recorded. |
| **Recurring problems** | Invisible in aggregate | Glitch types roll up to expose root causes. |

---

## 6. How It Increases Revenue

*F14 is primarily a revenue-protection feature; this section carries the weight.*

1. **Recovered at-risk guests.** A detractor caught and recovered in-stay stays a guest — and is far more likely to return and to book direct — instead of leaving a one-star and never coming back.
2. **Protected review scores and rate power.** Converting in-stay glitches into private fixes keeps negative reviews off public platforms, protecting the score that supports pricing and conversion.
3. **More direct / repeat bookings, less OTA commission.** Guests who feel heard and recovered rebook directly; a stronger review profile lifts direct-booking share and reduces dependence on commissioned OTA channels.

> **Illustrative model:** Suppose in-stay capture and recovery save just **2 at-risk guests per week** from leaving unhappy. If each recovered guest represents one retained future stay worth **৳12,000** in room revenue, that is **~৳1,248,000/year** in protected revenue — before counting the rate power of a higher review score or the OTA commission avoided on the direct rebookings it drives. *(Figures illustrative; validate against property data — ADR, repeat rate, and channel mix.)*

---

## 7. How It Reduces Operational Cost

1. **Fewer reactive comps and refunds.** A glitch recovered in-stay with a small gesture avoids the larger refund or rate adjustment negotiated reactively at the desk.
2. **Controlled comp authorization.** RBAC limits and a recorded log of what was given curb over-generous or unaccountable giveaways.
3. **Root-cause reduction.** Rolled-up glitch types let the property fix recurring problems at source, cutting the volume of recoveries over time.

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Catch problems in-stay | Share of glitches logged *before* checkout | ≥ 80% |
| Fast recovery | Median negative-signal alert → manager action time | ↓ measurable |
| Closed-loop recovery | Complaints closed with recorded outcome | ≥ 90% |
| Protected reputation | Negative public reviews attributable to in-house glitches | ↓ measurable |
| Controlled recovery cost | Comp value per recovered guest | Within policy limits |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| Guest PII handling | Feedback ties to reservation/guest data | Minimize PII, RBAC, access logging (per [Feature Scope §4](staff-mobile-app-feature-scope.md)) |
| Feedback channel | SMS/link/QR availability and consent | Confirm permitted channel; fall back to staff-assisted capture |
| Guest-contact spam | Over-surveying annoys guests | Cap frequency; one light touch per stay; staff-surfaced only |
| Reviews/CRM integration | Property may run Revinate-class CRM or a review platform | Integrate/sync rather than duplicate; F14 stays staff-side |
| Scope creep to guest app | Pressure to build a guest companion | Hold the line: F14 is staff-side capture + recovery only |
| Recovery ownership | Unclear who owns the workflow | Define front desk vs. duty manager ownership (see §11) |

---

## 10. Scope delivered

Atrium Staff ships as one complete package. This feature delivers:

- Staff-assisted in-stay feedback / NPS capture
- Negative-signal alerts to the duty manager (via F6)
- Complaint/glitch log with apologise/comp/fix workflow
- Recovery actions routed via F5
- Resolution tracking
- RBAC comp limits
- Link/QR guest self-capture surfaced by staff
- Glitch-type roll-ups and root-cause reporting
- Integration with a review platform / guest CRM where present

---

## 11. Open Questions

- Which feedback channel(s) ship in v1 — staff-assisted capture only, or SMS/link/QR self-capture too?
- Do we integrate with an existing review platform or guest CRM (e.g. Revinate-class), or is Atrium the system of record for in-stay feedback?
- Who owns the recovery workflow — front desk or duty manager — and where does hand-off occur?
- What are the comp-authorization limits by role, and how are they enforced via RBAC?
- What guest-contact frequency caps and consent rules apply to avoid over-surveying?

---

## 12. One-Line Business Case

> **Guest Feedback & Service Recovery catches problems before checkout — capturing in-stay sentiment staff-side, alerting the duty manager, and running a tracked apologise/comp/fix workflow that protects reviews, rate power, and repeat bookings while cutting reactive refunds.**
