# Feedback & Service Recovery — Feature Deep-Dive

**Product:** Atrium Guest (Hotel / Resort Guest-Facing Mobile App)
**Feature area:** G14 — Feedback & Service Recovery
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-16
**Status:** Draft v1.0
**Related:** [Guest Mobile App — Feature Scope](guest-mobile-app-feature-scope.md) · [Competitive Feature Research](../../HOTEL-~1.MD)

---

## 1. Executive Summary

**Feedback & Service Recovery** catches the guest's verdict while they are still on property — and shows them it was acted on. The app prompts one-tap pulse ratings at smart moments (after check-in, after the first night, after an F&B order), keeps an always-available "something wrong?" channel one tap away, routes any negative signal instantly to the duty manager's recovery workflow in Atrium Staff ([F14](../../staff/docs/staff-mobile-app-guest-feedback-service-recovery-feature.md)), and lets the guest watch the issue move from "we're on it" to resolved. After checkout, review invitations are gated: promoters to Google or TripAdvisor; detractors to a private follow-up.

G14 is the **capture side** of a two-surface system: staff F14 defines the recovery workflow — alerting, apologise/comp/fix, resolution tracking — while G14 is where the signal *originates*: from the guest, in their own words and photos, the moment sentiment forms. A direct guest channel hears what guests would never say to a staff member's face, and hears it days earlier.

The economics run on one asymmetry: **a problem surfaced in-stay is a save; the same problem on a review site is permanent public damage.** In-stay capture converts detractors before checkout — smaller comps, fewer refund fights, protected scores — while post-checkout gating channels promoters into the public reviews that drive direct bookings. SuitePad (in-room feedback), Revinate (guest CRM, review-driven marketing), Canary, and Duve made in-stay sentiment capture a category standard; Knowcross and Quore prove the staff-side glitch workflow it feeds. Via QR (G15), it works even for guests who never installed anything.

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What the guest can do from the app |
|---|------------|------------------------------------|
| 1 | **Smart-moment pulse ratings** | One-tap ratings after check-in, after the first night, after an F&B order. |
| 2 | **"Something wrong?" channel with photo attachment** | Report a problem any time from a persistent entry point — no call, no desk visit — attaching a photo (the leaking tap, the wrong dish) so staff see what the guest sees. |
| 3 | **Instant negative-signal routing** | A low rating or report fires straight into the duty manager's recovery workflow (F14). |
| 4 | **Guest-visible issue status** | Watch the issue move: received → "we're on it" → resolved. |
| 5 | **Resolution confirmation ask** | After staff mark an issue resolved, the guest confirms — fixed, or reopen. |
| 6 | **Review invitation gating** | Post-checkout: promoters get a Google/TripAdvisor invite; detractors a private follow-up. |
| 7 | **App-less feedback QR (G15)** | Every capture flow works from a QR on the bedside card or table tent — no install. |

### 2.2 In scope (this release)
Smart-moment pulse prompts with frequency capping; always-available problem reporting with photo and room/reservation context; instant routing of negative signals into staff F14 (duty-manager alert via staff F6); guest-visible status timeline; resolution confirmation and reopen; post-checkout review gating with configurable thresholds; app-less QR/web capture (G15); Bangla/English capture UI.

### 2.3 Out of scope (this release)
The staff-side recovery workflow itself — alerting, apologise/comp/fix, comp RBAC, glitch roll-ups (that is [staff F14](../../staff/docs/staff-mobile-app-guest-feedback-service-recovery-feature.md); this doc is the capture surface); review-platform response management; post-stay email survey campaigns (a Revinate-class CRM's job — integrate, don't duplicate); sentiment analysis of external review sites.

### 2.4 Key assumptions
- Staff F14 (recovery workflow) and staff F6 (alerting) are live — G14 signals have somewhere to land and someone to wake.
- The property staffs a duty manager (or equivalent) empowered to act within the stay.
- Review gating complies with each platform's solicitation policies (see §9).
- Pulse prompts ride existing journey events (check-in complete, first-night morning, order delivered) from G2/G5/G8.

---

## 3. Capability Deep-Dive — The Feedback & Recovery Features

Each capability below covers **what it is**, **how it works in Atrium**, **why it matters**, and the **revenue/cost angle**, with vendor precedents noting the bar to clear.

### 3.1 Smart-moment in-stay pulse ratings
*Precedent: SuitePad (in-room feedback), Revinate (in-stay sentiment), Canary/Duve (journey-triggered touchpoints).*

**What it is.** A one-tap sentiment check placed where sentiment forms: after check-in, the morning after the first night, after an F&B order lands.

**How it works in Atrium.** Journey events trigger a single-tap rating with optional comment, frequency-capped so a stay never feels surveyed. A positive tap says thanks; a negative tap opens the problem-report flow pre-filled with context — which moment, which room, which order.

**Why it matters.** Post-stay surveys measure the whole stay, too late to change it. Moment-anchored pulses catch the *specific* failure while it is fixable — a bad first night reported at 8 a.m. leaves the whole stay for the save.

**Revenue / cost angle.** Every problem surfaced on day one instead of on TripAdvisor is a review protected and a stay salvaged (revenue); one-tap capture rides existing journey events at zero staff labor (cost).

### 3.2 Always-available "something wrong?" channel with photo attachment
*Precedent: Canary, Duve (guest-initiated reporting); Guestline Keez, Apaleo (in-app requests).*

**What it is.** A standing entry point where a guest can say something is wrong at any moment, with a photo, without a call or a desk visit.

**How it works in Atrium.** Persistent across the in-stay surface and every app-less page (G15). The guest picks a category or types, attaches a photo, submits — room and reservation context attach automatically. The report lands in staff F14 as a logged glitch, distinct from routine G4 requests: G4 is "bring me a towel"; G14 is "I'm unhappy."

**Why it matters.** Most dissatisfied guests never complain in person — many will not confront staff, and calling the desk feels like escalation. They stay silent, leave, and post. A low-friction private channel captures exactly this silent majority.

**Revenue / cost angle.** Captures the silent detractors no staff-side process could reach (revenue); photos and auto-context cut diagnosis time per issue (cost).

### 3.3 Instant negative-signal routing to the duty manager
*Precedent: Revinate Ivy (sentiment-triggered alerts); staff-side workflow per Knowcross, Quore — implemented as staff F14 + F6.*

**What it is.** No inbox, no batch report: a detractor pulse or problem report becomes a duty-manager alert within seconds, with full context.

**How it works in Atrium.** Negative signals fire into the [staff F14](../../staff/docs/staff-mobile-app-guest-feedback-service-recovery-feature.md) pipeline: a duty-manager alert via staff F6 (critical severities bypass Do-Not-Disturb), a glitch logged against the reservation, the apologise/comp/fix play, and the fix routed as an owned task through staff F5.

**Why it matters.** Recovery has a half-life measured in hours. A manager who knocks twenty minutes after a bad-night pulse turns a detractor into a story told favorably; the same knowledge at checkout is worth a fraction — after checkout, nothing.

**Revenue / cost angle.** Minutes-fast routing converts detractors while conversion is still possible (revenue); an early apology-plus-fix costs far less than the refund negotiated at the desk (cost).

### 3.4 Guest-visible issue status — "we're on it" → resolved
*Precedent: consumer order-tracking pattern applied to complaints; Duve/Canary request-status visibility.*

**What it is.** The guest sees the report acknowledged and progressing: received → "we're on it" → resolved.

**How it works in Atrium.** Staff F14 status transitions reflect back to the guest's issue timeline automatically, with an optional human note ("Engineering is on the way — sorry about this"); a push (G8) announces each transition.

**Why it matters.** Half the damage of a problem is the silence after reporting it — a complaint that vanishes reads as indifference, and *that* is what the review will be about. Visible acknowledgment turns the complaint experience itself into evidence the property cares.

**Revenue / cost angle.** Acknowledgment alone defuses escalation — some saves cost nothing but visibility (revenue); status transparency eliminates "any update?" calls and desk visits (cost).

### 3.5 Resolution confirmation ask
*Precedent: closed-loop practice from service-management tooling (Knowcross/Quore resolution tracking), extended to the guest.*

**What it is.** When staff mark an issue resolved, the guest gets the last word: a one-tap "all good now?" — confirm, or reopen.

**How it works in Atrium.** The resolved transition triggers a confirmation prompt. "Yes" closes the loop with a guest-confirmed outcome on the glitch record; "not fixed" reopens at escalated priority, straight back to the duty manager. Outcomes feed F14's resolution tracking and root-cause roll-ups.

**Why it matters.** Staff-declared and guest-experienced resolution are not the same thing — the gap between them is where "they said they fixed it" reviews are born. The confirmation ask finds that gap while it can still be closed.

**Revenue / cost angle.** Catches failed fixes before they become the second, angrier complaint or the review (revenue); guest-confirmed closure keeps recovery KPIs truthful, so root-cause investment targets real problems (cost).

### 3.6 Post-checkout review invitation gating
*Precedent: Revinate (review-driven marketing), Canary/Duve (post-stay flows); thresholds subject to review-platform policies — see §9.*

**What it is.** The post-stay ask, routed by sentiment: positive-signal guests are invited to Google or TripAdvisor; guests with unresolved or negative signals get a private "tell us more" instead.

**How it works in Atrium.** After checkout, pulse history and recovery outcomes compute a promoter/detractor classification against property-set thresholds. Promoters receive an invitation deep-linking to the property's review pages; detractors receive a private follow-up that routes into staff F14 as a post-stay recovery case — all within platform solicitation policies (§9).

**Why it matters.** Review scores are an average of who bothers to post — and the angry post unprompted. Inviting the satisfied majority corrects that selection bias; volume and recency of positive reviews move ranking and conversion where future guests decide.

**Revenue / cost angle.** Score and volume drive direct-booking conversion and rate power — the feature's offensive lever (revenue); the private detractor path resolves unhappiness at follow-up cost instead of public reputation-repair cost (cost).

### 3.7 App-less feedback QR
*Precedent: [G15](guest-mobile-app-app-less-access-feature.md) cross-cutting pattern; Canary-style no-download flows; staff-surfaced link/QR per staff F14.*

**What it is.** Every capture flow — pulse, report, photo, status — served as a web flow from a QR on the bedside card, the table tent, the checkout desk.

**How it works in Atrium.** Room-placed QR codes carry room context; a scan opens the report flow in the browser, no install. Where the guest is identifiable (booking link or lightweight verification), status visibility and the post-stay flow work app-less too. It is also the channel staff surface in F14's assisted flow — one capture pipeline, however the guest arrives.

**Why it matters.** The guests least likely to install an app — the two-night domestic guest, the day visitor, the event attendee — are not less likely to have problems. Without the QR path, capture reaches a fraction of guests; with it, effectively everyone on property.

**Revenue / cost angle.** Multiplies every save and promoter invite across the whole guest population, not just installers (revenue); one web codebase shared with G15 (cost).

---

## 4. Why This Matters

### 4.1 A detractor converted before checkout is the cheapest save in hospitality
The same unhappy guest costs an apology and a gesture on day one, a refund fight at checkout, and a permanent one-star after it. G14 moves discovery to the cheap end of that curve — and the guest-initiated channel discovers what staff-side capture never hears.

### 4.2 Reviews are the property's public rate card
Score and volume drive OTA ranking, direct-booking conversion, and the rate a property can hold. G14 works both ends: fewer deserved negatives (in-stay saves), more earned positives (promoter invitations).

### 4.3 The loop must be visibly closed, or capture backfires
Asking for feedback and then going silent is worse than not asking. Status visibility, the resolution confirmation, and the detractor follow-up make G14 a service experience rather than a survey.

### 4.4 Guest capture and staff recovery are one system
G14 without staff F14 is a complaint box no one empties; F14 without G14 hears only what guests say to staff faces. Wired together they form the full loop neither surface delivers alone — and in the BD market, where no local vendor ships either side, the pair is the differentiation.

---

## 5. Journey Moments & Operations It Improves

| Moment / operation today | Pain | With Feedback & Service Recovery |
|--------------------------|------|----------------------------------|
| **Rough arrival** | Guest stews silently; property learns from the review | Post-check-in pulse catches it within the hour. |
| **Bad first night** | Colors the whole stay; surfaces at checkout, if ever | Morning-after pulse triggers recovery with the whole stay left. |
| **Disappointing meal** | Plate goes back half-eaten; kitchen never knows | Post-order pulse flags the dish; F&B recovers guest and recipe. |
| **Guest won't complain in person** | Silent detractor — no signal until the review | Private in-app/QR channel captures what face-to-face never would. |
| **Complaint reported, then silence** | "Nothing happened" becomes the review headline | Visible status: received → we're on it → resolved. |
| **"Fixed" but not fixed** | Second complaint arrives angrier | Resolution confirmation reopens at escalated priority. |
| **Happy guests leave quietly** | Score set by the angry minority | Promoter invitations turn the majority into public reviews. |
| **Unhappy guest already gone** | Lost forever; may still post | Private detractor follow-up routes a post-stay recovery case. |
| **Day visitors & non-installers** | No feedback channel at all | QR capture (G15) reaches every guest on property. |

**Guest friction removed:** confronting staff, phoning the desk, chasing updates to be heard. **Staff load removed:** checkout complaint ambushes, "any update?" traffic, reputation-repair after preventable public negatives.

---

## 6. How It Increases Revenue

*G14's revenue case is protection plus reputation — the defensive save and the offensive review engine.*

1. **Detractors converted before checkout.** Every in-stay save keeps a future guest: recovered guests rebook — often more loyally than guests who never had a problem — instead of churning.
2. **Review scores protected.** Each negative intercepted privately is a public one-star that never happens.
3. **Promoters channeled into public reviews.** Gated invitations harvest positive reviews the satisfied majority would never post unprompted — volume and recency lift ranking and conversion.
4. **Direct-booking mix strengthened.** A stronger review profile converts more lookers into direct bookers, compounding the OTA-commission recovery G1 and G13 lead.
5. **Fewer refunds and rate adjustments.** Problems settled mid-stay with a gesture do not become checkout refund negotiations or chargebacks.
6. **Root-cause intelligence.** Guest-worded, photo-evidenced, moment-stamped signals feed F14's roll-ups — fixing the recurring failure lifts every future stay.

> **Illustrative model:** A 150-room resort at 75% occupancy hosts ~2,000 stays/month. If pulses surface problems in **5% of stays** (~100/month) and recovery converts **half** of those would-be detractors, that is ~50 saves/month; at one retained future stay worth **৳12,000** each, **~৳72 lakh/year** in protected revenue — before the review-score effect: 15–20 gated promoter reviews/month compound ranking and conversion on every future booking. *(Figures illustrative; validate against property ADR, repeat rate, review baseline, and channel mix.)*

---

## 7. How It Reduces Operational Cost

1. **Cheaper saves.** An early apology and small gesture replace the larger comp or refund that late discovery forces.
2. **Checkout-desk conflict removed.** Issues settled mid-stay do not become long, staffed disputes on the last morning — protecting express check-out (G2) too.
3. **Follow-up traffic eliminated.** Visible status and push updates end the "any news?" calls and desk visits.
4. **Failed fixes caught cheap.** The resolution confirmation catches an unfixed issue as a reopened task, not a second complaint plus a comp.
5. **Zero-labor capture.** Pulses and QR reports ride existing journey events and G15 infrastructure — no staff time soliciting or transcribing.
6. **Root-cause reduction.** Categorized signals let the property fix the recurring failure once instead of recovering from it weekly.
7. **Reputation-repair work avoided.** Every intercepted negative is a public review that never needs a crafted management response.

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Hear guests in-stay | Pulse response rate per prompted moment | ≥ 30% |
| Catch problems early | Guest-reported issues surfaced before checkout | ≥ 80% |
| Fast recovery | Median negative signal → duty-manager first action | < 30 minutes |
| Loop closed | Issues closed with guest confirmation | ≥ 90% |
| Real closure | Reopen rate after staff-marked resolution | < 10% |
| Detractor conversion | Detractors recovered before checkout | ≥ 50% |
| Review engine | Public reviews/month via promoter invitations | ↑ vs. baseline |
| Reputation | Average public review score | ↑ within 2 quarters |
| Reach | Share of feedback arriving via app-less QR (G15) | Tracked — validates non-installer reach |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| Staff F14/F6 live | Guest signals need the workflow and alerting to land in | Ship G14 where staff F14 is deployed; degrade to logged-only with clear expectations |
| Review-platform policies | Google/TripAdvisor restrict selective solicitation | Configurable compliance mode (invite-all, sentiment-informed timing); policy review per platform |
| Survey fatigue | Over-prompting trains guests to dismiss | Hard frequency caps; one-tap format; skip-and-quiet rules per stay |
| Duty-manager capacity | Alerts without an empowered responder destroy trust | Staffing prerequisite at onboarding; escalation chain in staff F6 |
| Expectation of speed | A visible "we're on it" implies action | SLA timers in staff F5/F14; honest status copy; escalate breaches |
| Guest identity in QR flows | App-less reports need stay context without heavy login | Room-coded QR + lightweight verification; anonymous fallback accepted but unlinked |
| PII & sensitive content | Complaints carry personal data and photos | PII minimization, retention limits, RBAC + access logging (per staff F14 §9) |
| CRM/review-tool overlap | Property may run Revinate-class reputation tooling | Integrate/sync post-stay flows; define system of record |
| Gaming | Staff incentives tied to scores can distort capture | Guest-confirmed closure as metric of record; audit trail on gating |

---

## 10. Scope delivered

Atrium Guest ships as one complete package. This feature delivers:

- Smart-moment pulse ratings (post-check-in, post-first-night, post-F&B-order) with frequency capping
- Always-available "something wrong?" channel with photo and auto-context
- Instant negative-signal routing into staff F14 (duty-manager alert via staff F6, glitch log, recovery workflow)
- Guest-visible status timeline with push updates
- Resolution confirmation ask with reopen-at-escalated-priority
- Post-checkout review gating (promoter → Google/TripAdvisor; detractor → private follow-up), platform-policy-configurable
- App-less QR/web capture for every flow (G15), including day visitors
- Bangla/English capture UI
- Signal feed into staff F14 roll-ups and root-cause reporting

---

## 11. Open Questions

- Which pulse moments ship enabled by default, and what frequency cap per stay?
- Which review-gating compliance mode is the launch default, given Google/TripAdvisor solicitation policies?
- How is guest identity established in QR flows — room code, booking-link token, or sign-in — and is anonymous feedback accepted?
- Does the guest-visible timeline show the comp offered, or only progress states (comp handling stays staff-side per F14 RBAC)?
- Where a property runs Revinate-class tooling, does Atrium's post-stay flow hand off or run alongside?
- Do day-visitor signals (no reservation) route the same way, and against what record?

---

## 12. One-Line Business Case

> **Feedback & Service Recovery hears the guest while the stay can still be saved — one-tap pulses and a private "something wrong?" channel routed straight to the duty manager's recovery workflow, converting detractors before checkout and channeling promoters into the public reviews that drive direct bookings.**
