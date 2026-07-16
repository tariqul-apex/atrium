# Lost & Found & Staff Safety on Mobile — Feature Deep-Dive

**Product:** Atrium Staff (Hotel / Resort Operations Mobile App)
**Feature area:** F16 — Lost & Found & Staff Safety
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-15
**Status:** Draft v1.0
**Related:** [Feature Scope](staff-mobile-app-feature-scope.md) · [Role-Based Design](staff-mobile-app-role-based-design.md) · [Notifications — Deep-Dive](staff-mobile-app-notifications-alerts-feature.md) · [Competitive Feature Research](../HOTEL-~1.MD)

---

## 1. Executive Summary

**F16 bundles two light operational utilities** that share a theme — reducing **liability, paperwork, and risk** — rather than driving revenue:

**(a) Lost & Found** gives staff a digital register to log a found item with a photo, location, and date, match it to a guest or reservation, and track it through return or disposal. It replaces the paper L&F logbook and the shoebox of unlabelled items behind the desk.

**(b) Staff Safety** gives every staff member a **lone-worker SOS / panic button** — a global control present on every screen — that sends a location-tagged distress alert to security and management. It addresses the duty-of-care gap for solo housekeepers, night staff, and anyone working alone on a large property.

Neither feature is a revenue engine, and this document is deliberate about that (see §6–§7). Their value is **cost avoidance, reduced liability, regulatory/duty-of-care compliance, and staff trust** — increasingly a baseline expectation, not a differentiator, for L&F, and a genuine differentiator for SOS.

Vendor precedent is uneven. **Lost & Found is a shipped, mainstream capability** — Infor HMS carries concierge & lost-and-found task management on mobile, and incident-management specialists (Knowcross, Quore) handle the adjacent logging pattern. **Lone-worker SOS / panic is thin-to-absent in the PMS set** — it comes from safety-tech, not hospitality software — which makes it an Atrium differentiator built on a duty-of-care rationale.

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

**(a) Lost & Found**

| # | Capability | What staff can do from the device |
|---|------------|-----------------------------------|
| 1 | **Log a found item** | Capture item with photo, location found, date/time, and finder — on the spot. |
| 2 | **Match to guest / reservation** | Link an item to a room, stay, or guest profile to enable proactive return. |
| 3 | **Status tracking** | Move an item through Open → Unclaimed → Claimed → Disposed with an audit trail. |
| 4 | **Return & disposal** | Record hand-back (who claimed, ID/signature) or disposal per policy. |
| 5 | **Search & register view** | Browse/search the register by date, location, category, or status. |

**(b) Staff Safety — lone-worker SOS**

| # | Capability | What staff can do from the device |
|---|------------|-----------------------------------|
| 6 | **Global SOS button** | Trigger a distress alert from **any screen**, any role. |
| 7 | **Hold-to-send** | Press-and-hold to arm the alert, preventing accidental triggers. |
| 8 | **Location-tagged alert** | Attach device GPS / last-known location to the alert automatically. |
| 9 | **Receive & acknowledge** | Security / duty manager / GM receive, acknowledge, and respond to the alert. |
| 10 | **Escalation on no-ack** | Unacknowledged alerts escalate up the chain after a timeout. |

### 2.2 In scope (this release)
Digital L&F register (log/photo/location/date, guest match, status lifecycle, return/disposal record, search); lone-worker SOS (global hold-to-send button, location capture, role-based receive/acknowledge, escalation timeout), RBAC, offline queueing for L&F, and best-effort offline handling for SOS.

### 2.3 Out of scope (this release)
Automated guest-facing L&F claim portals and shipping/logistics integration; charity/auction disposal workflows; wearable panic devices and dedicated hardware buttons; integration with third-party monitoring/dispatch (ARC) or property CCTV/access-control (see §9, §11). Man-down/no-motion automatic detection is a future consideration, not a promise.

### 2.4 Key assumptions
- Devices have a camera (L&F photos) and location services (SOS).
- The property designates **who receives SOS** and an escalation chain before launch (see §11).
- L&F retention/disposal timelines are set by property policy and local law.

---

## 3. Capability Deep-Dive — The Two Utilities

Each capability is elaborated — **what it is**, **how it works in Atrium**, **why it matters operationally**, and the **cost/liability angle** — with vendor precedents noting the bar to clear. The two sub-features are kept separate throughout.

### 3.1 (a) Lost & Found — digital register (photo + location + date)
*Precedent: Infor HMS (concierge & lost-and-found task management on mobile); Knowcross, Quore (incident/task logging pattern).*

**What it is.** A structured digital record of every found item — captured with a photo, the location it was found, the date/time, and the finder — replacing the paper logbook and the unlabelled storage shelf.

**How it works in Atrium.** A housekeeper who finds an item in a checkout room opens L&F, photographs it, tags the location (room/area) and category, and logs it — the finder and timestamp are captured automatically. The item enters the register as **Open**. Front desk and the L&F custodian can search the register when a guest calls asking about a left-behind item.

**Why it matters.** Paper logbooks are illegible, easily lost, and impossible to search under pressure when a guest calls about a lost passport. A photo-rich, searchable register makes the "did anyone find…?" question answerable in seconds and creates a defensible record of custody.

**Cost / liability angle.** Ends the paper logbook and the manual effort of maintaining it (cost); a clear chain-of-custody record reduces disputes and liability when valuables go missing (liability). Revenue impact is negligible — this is a paperwork-and-risk feature, not a revenue feature.

### 3.2 (a) Lost & Found — match to guest / reservation & track return/disposal
*Precedent: Infor HMS (lost-and-found task management).*

**What it is.** Linking a found item to a guest or reservation to enable proactive return, and tracking each item through its full lifecycle — **Open → Unclaimed → Claimed → Disposed** — with who/when recorded at each step.

**How it works in Atrium.** An item found in a specific room auto-suggests the departing reservation, letting staff link it to the guest profile and proactively reach out. If unclaimed past the property window it flips to **Unclaimed**; on hand-back it becomes **Claimed** (recording claimant, verification/ID, and optional signature); if disposed per policy it becomes **Disposed** with the reason and authoriser. Every transition is timestamped and audited.

**Why it matters.** The value in L&F isn't storing items — it's returning them to the right guest and proving what happened to the rest. Guest-matched proactive return is a genuine service moment; a tracked disposal step protects the property when questioned months later.

**Cost / liability angle.** A defensible status lifecycle and disposal audit trail is the core liability control (liability); proactive return can protect a review or a loyalty relationship (indirect, minor revenue). The honest framing: value is dominated by liability and effort reduction, not revenue.

### 3.3 (b) Staff Safety — global lone-worker SOS / panic button
*Precedent: **Thin in the PMS set.** Lone-worker SOS/panic is a duty-of-care/safety-tech capability, not a standard hospitality-software feature — an Atrium differentiator, built on a compliance rationale rather than a vendor pattern.*

**What it is.** A **global** distress control present on every screen that any staff member can trigger to summon help, with the alert **sent universally** (every role can send) and **received** only by security, the duty manager, and the GM (role-based receive — per [Role-Based Design](staff-mobile-app-role-based-design.md)).

**How it works in Atrium.** The SOS control is a persistent, always-reachable element (not buried in a menu). To fire, a staff member **presses and holds** for a short interval — hold-to-send prevents pocket/accidental triggers. On release, Atrium captures the sender's identity and **device location**, raises a high-priority **SafetyAlert**, and pushes it to the receive-set (security / duty manager / GM) as a critical notification that bypasses do-not-disturb. Receivers see who, where, and when, and **acknowledge**; if no one acknowledges within the configured window, the alert **escalates** up the chain. A false alarm is cancelled with a reason, keeping the log clean.

**Why it matters.** Solo housekeepers, night auditors, and engineers working alone in plant rooms are exposed; a large resort is a big place to be in trouble with no way to call for help. A one-touch, location-tagged panic path is a concrete duty-of-care control — and a signal to staff that their safety is taken seriously.

**Cost / liability angle.** This is **not a revenue feature** — its return is meeting staff-safety compliance / duty-of-care obligations, reducing incident liability, and supporting recruitment and retention through demonstrated care. Framing it as a revenue driver would be dishonest; framing it as risk-and-trust infrastructure is accurate.

---

## 4. Why This Matters

### 4.1 Paper logbooks are a liability, not a record
An illegible, unsearchable L&F logbook fails exactly when it's needed — when a guest reports a lost valuable. A photo-rich, searchable, audited register turns L&F from a shoebox into a defensible system of record.

### 4.2 Duty of care is becoming an expectation
Lone-worker safety is increasingly treated as an employer obligation, not a nicety. A location-tagged SOS with a defined receive-and-escalate path is a tangible discharge of that duty — and something few hospitality platforms offer.

### 4.3 Both features reduce liability and manual effort
L&F cuts paperwork and disputes; SOS cuts incident exposure. Neither sells rooms; both lower the property's risk and administrative load — the honest reason to build them.

### 4.4 SOS is a competitive opening; L&F is table stakes
Lost & Found is mainstream (Infor HMS et al.) — Atrium must simply match it well. Lone-worker SOS is largely **absent** from the PMS field, making it a credible differentiator grounded in compliance rather than marketing.

---

## 5. Operations It Improves

| Operation today | Pain | With F16 |
|-----------------|------|----------|
| **Logging a found item** | Paper logbook, no photo | Photo + location + date captured on the spot, searchable. |
| **Guest asks about a lost item** | Flip through pages, guess | Search register in seconds; match to their stay. |
| **Returning / disposing items** | No record, dispute risk | Tracked Open→Claimed/Disposed with audit trail. |
| **Solo staff in trouble** | Radio (if reachable) or nothing | One-touch, location-tagged SOS to security/management. |
| **Responding to a distress call** | No location, no accountability | Alert shows who/where; acknowledged and escalated if not. |

---

## 6. Revenue Impact (Honest Assessment)

**F16 is not a revenue driver, and this document will not pretend otherwise.** Any revenue effect is indirect and minor:

1. **Proactive L&F return** can protect a review or a loyalty relationship when a guest's valued item is mailed back — a soft, unquantifiable service moment, not a revenue line.
2. **Staff-safety trust** supports retention, and lower turnover reduces rehiring/retraining cost — a cost effect, not new revenue.

There is **no room-night, ADR, or upsell mechanism** in either sub-feature. The business case (§7, §12) rests on cost, liability, and compliance — not top-line growth. Anyone modelling F16 as revenue-generating is mis-scoping it.

---

## 7. How It Reduces Cost, Liability & Risk

1. **Ends paper L&F administration.** No logbook to maintain, decipher, or reconcile; searchable register cuts the time spent hunting for items.
2. **Lowers L&F dispute and liability exposure.** A photographed, timestamped chain-of-custody and audited disposal record is a defensible answer when custody of valuables is questioned.
3. **Discharges duty-of-care obligations.** A working lone-worker SOS with defined receive-and-escalate is concrete evidence the property protects solo/night staff — reducing incident liability.
4. **Supports retention.** Demonstrated safety investment aids recruitment and retention of the frontline staff hardest to keep, avoiding rehiring/retraining cost.

> **Illustrative model:** If a searchable register saves the L&F custodian ~30 min/day of logbook and search effort, that is ~180+ hours/year of reclaimed labour per property; at an illustrative ৳250/hour fully-loaded, ~৳45,000/year — before counting a single avoided liability dispute or the (unpriceable) value of a safe response to one lone-worker incident. *(Figures illustrative; validate against property data.)*

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| L&F digitised | Found items logged in-app vs. paper | ≥ 95% |
| L&F return rate | Matched items returned to guest | ↑ measurable |
| L&F accountability | Items with photo + audited status trail | ≥ target % |
| SOS reliability | Alerts delivered to receive-set | ≥ 99% (when connected) |
| SOS responsiveness | Median time to first acknowledgement | ≤ target (e.g. < 60s) |
| SOS integrity | False-alarm rate (accidental triggers) | ↓ toward negligible |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| L&F photo storage & retention | Volume + guest PII in item photos | Compression, secure storage, retention/disposal policy, access logging |
| L&F guest PII | Matching to guest/reservation exposes profile data | RBAC, PII minimisation, audit every access |
| SOS offline reliability | Alert may fire where there is no connectivity | Queue + retry; capture last-known location; degrade to best-effort, never silently drop |
| SOS receive-set & escalation | Who receives, and what if no one acks | Configured receive-set (security/duty mgr/GM) + escalation timeout (see §11) |
| False alarms | Accidental triggers erode trust in the alert | Hold-to-send arming; quick cancel-with-reason; monitor false-alarm rate |
| Device GPS accuracy | Indoor/basement location may be coarse | Best-available location + area context; do not over-promise precision |
| SOS device dependency | A dead/absent phone can't summon help | Document as a limitation; consider wearables/hardware later (out of scope v1) |

---

## 10. Scope delivered

Atrium Staff ships as one complete package. This feature delivers:

- (a) L&F register: log with photo/location/date, guest/reservation match, status lifecycle (Open→Unclaimed→Claimed→Disposed), return/disposal record, search, RBAC, offline queue
- (b) SOS: global hold-to-send button, location capture, role-based receive/acknowledge, escalation timeout
- L&F guest-facing return/claim assistance
- SOS delivery hardening (cellular fallback, retry visibility), configurable escalation policies
- L&F disposal/charity workflows
- SOS integration with property security/CCTV/access control and/or third-party monitoring
- Wearable/hardware panic and man-down detection

---

## 11. Open Questions

- **L&F retention & disposal:** what retention window and disposal timelines apply (property policy + local law), and who authorises disposal?
- **SOS receive-set:** who receives alerts — security, duty manager, or both — and what is the exact escalation path and timeout on no-acknowledgement?
- **SOS connectivity:** must alerts send over **cellular when off Wi-Fi**, and what is the fallback when fully offline?
- **Security integration:** should SOS integrate with property **security / CCTV / access control** (or third-party monitoring), or remain in-app only for v1?
- **False-alarm handling:** what cancel/confirm flow and reporting keep the alert trustworthy without adding friction?
- **Guest PII in L&F:** what access-logging and minimisation standard applies to item photos and guest matching?

---

## 12. One-Line Business Case

> **Lost & Found & Staff Safety are two light utilities that cut paperwork and cut risk — a searchable, audited L&F register that replaces the logbook, and a one-touch, location-tagged lone-worker SOS that discharges duty of care — valued for liability, compliance, and staff trust, not revenue.**
