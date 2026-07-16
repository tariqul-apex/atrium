# Management Analytics / Owner KPIs on Mobile — Feature Deep-Dive

**Product:** Atrium Staff (Hotel / Resort Operations Mobile App)
**Feature area:** F11 — Management Analytics / Owner KPIs
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-15
**Status:** Draft v1.0
**Related:** [Feature Scope](staff-mobile-app-feature-scope.md) · [Role-Based Design](staff-mobile-app-role-based-design.md) · [Prioritization](staff-mobile-app-feature-prioritization.md) · [Competitive Feature Research](../HOTEL-~1.MD)

---

## 1. Executive Summary

**Management Analytics / Owner KPIs** is the app's boardroom view — a **GM- and owner-only** business-intelligence surface that turns the raw activity of every operational module into the handful of numbers ownership actually steers by: labor-cost %, occupancy, productivity, ancillary revenue, and task/SLA throughput, each with a trend and a delta.

It is the app's **single analytics surface**, and it is deliberately walled off from everything operational. Every other feature *does the work*; F11 *shows what the work added up to* — so the people with the authority to act can see where money is leaking and close the gap.

> **Critical distinction — read this first.** F11 is **not** the Line Manager "Overview." The [Overview](staff-mobile-app-role-based-design.md#5-line-manager--operational-overview-the-at-a-glance-screen) is **shift-floor monitoring** — live status, current activity, exceptions, no history, no analytics, no exports. F11 is **boardroom BI** — trends, deltas, date ranges, roll-ups, exports. The Overview answers *"what is happening on my floor right now?"* for a line manager; F11 answers *"is the property being run efficiently, and where is waste?"* for the GM and owner. Different audience, different data, different screen. See [Role-Based Design §2 hard-invisibility rules](staff-mobile-app-role-based-design.md#2-feature--role-visibility-matrix).

This is the feature that delivers the **"increase authority / owner visibility"** goal: it gives ownership the numbers to **act on** the savings the operational features generate — F9 labor, F10 preventive maintenance, F12 inventory, F3 housekeeping — by making waste **visible and cuttable**.

Vendors ship mobile KPIs as a manager pillar: **Hotelogix** (ARR, RevPAR, occupancy, house status, business-on-books, top OTAs on mobile), **Cloudbeds** (in-app occupancy meter and booking-trend insights), and **Stayntouch** (role-based Manager / Front Desk / Housekeeping dashboards).

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What the GM/owner can do from the device |
|---|------------|------------------------------------------|
| 1 | **KPI tiles with deltas** | See headline numbers — labor-cost %, occupancy, productivity (rooms/attendant-hour), ancillary revenue, task/SLA throughput — each with a period-over-period delta and direction. |
| 2 | **Trend charts** | View labor-cost trend and department-productivity trend over time as simple line/bar charts. |
| 3 | **Date-range + department filters** | Scope every tile and chart to a chosen date range and department. |
| 4 | **Export / share** | Export or share a KPI snapshot (PDF/image/CSV) with ownership or partners. |
| 5 | **Cross-module feed** | Read aggregated data fed from every operational module (F1–F14) into one owner view. |

### 2.2 In scope (this release)
Operational and cost KPI tiles with deltas, labor-cost and productivity trend charts, date-range + department filtering, snapshot export/share, cross-module data feed, and strict **GM/owner-only** access control (RBAC). Read-only: F11 never issues a task or a booking action.

### 2.3 Out of scope (this release)
- **Revenue management / dynamic pricing.** F11 is **operational & cost BI**, not a pricing engine. It does **not** recommend rates, forecast demand for pricing, or automate rate/rack changes — that engine was **deliberately removed from scope** for this product. F11 stays on **visibility and reporting**; any rate decision remains a human action taken elsewhere.
- Full data-warehouse / OLAP self-service, custom report-builder, and ad-hoc query.
- Channel/OTA revenue analytics, guest-segmentation marketing analytics, and financial accounting (P&L, GL).
- Above-property / multi-property consolidated roll-up **beyond** what §11 resolves for v1.

### 2.4 Key assumptions
- Every operational module writes clean, timestamped events to a shared store F11 can aggregate (see §9).
- The **owner/GM role** is the only role that can reach the surface — enforced server-side, not just hidden in the UI.
- Currency and figures shown are property-configured; illustrative figures in this doc are in **৳** and clearly labeled.

---

## 3. Capability Deep-Dive — The Analytics Features

Each capability is elaborated — **what it is**, **how it works in Atrium**, **why it matters operationally**, and the **revenue/cost angle** — with vendor precedents noting the bar to clear.

### 3.1 KPI tiles with deltas (labor-cost %, occupancy, productivity, ancillary, SLA)
*Precedent: Hotelogix (mobile KPIs — ARR, RevPAR, occupancy, house status, business-on-books), Stayntouch (role-based manager dashboards).*

**What it is.** A grid of headline KPI tiles, each showing a current value, a period-over-period **delta** (↑/↓ with %), and a direction color. Launch set: **labor-cost %** (payroll ÷ revenue), **occupancy**, **productivity** (rooms cleaned per attendant-hour), **ancillary revenue** (F&B / POS / extras), and **task/SLA throughput** (work orders and tasks closed within SLA).

**How it works in Atrium.** F11 aggregates events already captured elsewhere — F9 clock-in/labor hours, F3 housekeeping completions, F8/POS sales, F4/F10 work-order closures, PMS occupancy — into per-period rollups. The owner opens the Performance / KPIs surface (under ⋯ More; the owner role lands here) and sees the tiles for the selected range; tapping a tile drills to its trend (§3.2).

**Why it matters.** Ownership today reconciles labor from payroll spreadsheets, occupancy from the PMS, and productivity from nowhere — days late and never side by side. One tiled view, current and comparative, turns "how are we doing?" from a week-long ask into a glance, and a delta makes a drift obvious before it becomes a quarter.

**Revenue / cost angle.** Surfacing a labor-cost % overrun or a productivity dip **makes waste visible so management can cut it** (cost); ancillary-revenue and occupancy tiles show where upside is being left on the table (revenue, indirect — via better decisions, not automated pricing).

### 3.2 Trend charts (labor-cost trend, department productivity)
*Precedent: Cloudbeds (in-app occupancy meter and booking-trend insights).*

**What it is.** Simple, mobile-legible line/bar charts for the metrics that move with management action — **labor-cost % over time** and **department productivity over time** — so a number becomes a direction.

**How it works in Atrium.** From a KPI tile or the trend section, the owner sees the metric plotted across the selected date range, optionally split by department. Charts are read-only and scannable — the point is the shape of the line, not a pivot table.

**Why it matters.** A single number can mislead; a trend can't. Seeing labor-cost % climb three weeks running, or one department's productivity flatten while others rise, tells ownership *where* to look and *whether* a corrective action (a roster change, a PM push) actually worked.

**Revenue / cost angle.** Trends confirm whether cost-cutting is holding (cost) and whether ancillary/occupancy initiatives are trending up (revenue, indirect).

### 3.3 Date-range + department filters
*Precedent: Stayntouch (role-based dashboards scoped by function).*

**What it is.** Global controls that scope every tile and chart to a **date range** (today, week, month, custom) and a **department** (Front Office, Housekeeping, Engineering, F&B, or all).

**How it works in Atrium.** The filters sit at the top of the surface and re-scope the whole view at once. An owner compares this month to last, or isolates Housekeeping's productivity, without leaving the screen.

**Why it matters.** "The property is fine" and "Housekeeping is dragging the average" look identical until you can filter. Department and period scoping is what turns an aggregate into an accountable, actionable number.

**Revenue / cost angle.** Pinpointing the department and period where cost drifts or revenue lags focuses corrective effort where it pays back (cost + indirect revenue).

### 3.4 Export / share for ownership
*Precedent: Hotelogix (mobile KPI reporting for owners/operators).*

**What it is.** Exporting or sharing a KPI snapshot — a PDF/image of the current view, or a CSV of the underlying figures — for ownership, partners, or a board pack.

**How it works in Atrium.** The owner taps Share on the current (filtered) view; Atrium produces a dated snapshot with the tiles, deltas, and charts as shown, plus an optional CSV. Nothing operational is exposed — only aggregated performance data.

**Why it matters.** Ownership often needs to hand numbers *upward* — to co-owners, lenders, or a board — and today that means someone rebuilding a deck from raw exports. A one-tap, on-brand snapshot removes that lag and keeps the figures consistent with what the app shows.

**Revenue / cost angle.** Faster, consistent reporting cuts the analyst/admin time spent assembling owner packs (cost); clearer visibility supports better ownership decisions (revenue, indirect).

### 3.5 Cross-module data feed (the single source)
*Precedent: Hotelogix / Cloudbeds / Stayntouch (KPIs sourced from live PMS + ops modules).*

**What it is.** The pipeline that feeds F11 from **every** operational module — F9 labor, F3 housekeeping, F8/POS, F4/F10 maintenance, F12 inventory, PMS occupancy — so the owner view is one consistent read, not five reconciliations.

**How it works in Atrium.** Modules emit events to a shared store; F11 reads aggregated rollups from it (see §9). F11 is a **consumer only** — it computes and displays, it never writes back to operations.

**Why it matters.** The reason ownership can't see efficiency today is that the numbers live in separate systems that never agree. One feed, one definition per KPI, is what makes the analytics trustworthy enough to act on.

**Revenue / cost angle.** A single trusted source is the precondition for every cost cut and revenue decision F11 enables — without it, the numbers get argued instead of acted on.

---

## 4. Why This Matters

### 4.1 Savings only count when they're seen and acted on
F9 labor, F10 PM, F12 inventory, and F3 housekeeping *generate* savings; F11 is what lets ownership **see and bank** them. A 1–2 point labor-cost % overrun corrected because it was finally visible is the whole point — this feature **makes waste cuttable**.

### 4.2 Owner visibility is the authority the product promises
The "increase authority / owner visibility" goal lands here. F11 gives ownership the numbers — labor %, productivity, occupancy, SLA, ancillary trend — to hold the operation accountable without standing on the floor.

### 4.3 One analytics surface, walled off from operations
Analytics belongs in exactly one place, seen by exactly the people who act on it. Keeping F11 the **only** BI surface — GM/owner-only, distinct from the operational Overview — keeps frontline and line-manager screens clean and keeps financials invisible to roles that must never see them (see [Role-Based Design §2](staff-mobile-app-role-based-design.md#2-feature--role-visibility-matrix)).

### 4.4 A competitive opening in the BD market
Mobile owner KPIs exist abroad (Hotelogix, Cloudbeds, Stayntouch) but are thin locally. An **operational & cost** BI view — not a pricing engine — tuned to what BD ownership actually asks for is a differentiator Atrium can lead.

---

## 5. Operations It Improves

| Operation today | Pain | With Owner KPIs (F11) |
|-----------------|------|-----------------------|
| **Checking labor cost** | Payroll spreadsheet, days late | Labor-cost % tile with delta, live per period. |
| **Judging productivity** | No number exists | Rooms/attendant-hour tile + trend. |
| **Spotting cost drift** | Noticed at month-end | Deltas + trend flag drift within the period. |
| **Comparing departments** | Manual, inconsistent | Department filter isolates the laggard. |
| **Reporting to ownership** | Analyst rebuilds a deck | One-tap dated snapshot / CSV export. |
| **Seeing the whole property** | Five systems, five truths | One cross-module feed, one definition per KPI. |

---

## 6. How It Increases Revenue

Revenue impact is **indirect** — F11 informs better decisions; it does **not** set or recommend rates.

1. **Better-informed decisions.** Occupancy and ancillary-revenue tiles and trends show where upside is being left on the table, so ownership can direct effort (staffing, outlet hours, upsell focus) to capture it.
2. **Protected ancillary streams.** An ancillary-revenue trend that stalls is visible early, prompting a management response before a quarter is lost.
3. **Faster, sharper owner decisions.** Consistent, current numbers shorten the loop between "something's off" and a corrective decision that protects revenue.

> **Illustrative model:** If F11 surfacing a **1–2 point labor-cost % overrun** leads management to correct it on a monthly payroll of, say, **৳20,00,000**, a 1.5-point correction is ~**৳30,000/month** (~**৳3,60,000/year**) made visible and cuttable — before counting productivity and ancillary gains. *(Figures illustrative; validate against property data.)*

---

## 7. How It Reduces Operational Cost

1. **Waste made cuttable.** The core mechanic: labor-cost %, productivity, and SLA tiles expose overruns and dips management can act on — the savings the operational modules generate only bank once they're seen here.
2. **Less reporting labor.** One-tap snapshots and a single cross-module feed remove the analyst/admin hours spent reconciling systems and rebuilding owner decks.
3. **Faster corrective action.** Deltas and trends catch drift within the period instead of at month-end, so a correction lands while it still matters.

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Waste made visible | Labor-cost % overruns surfaced & acted on | Tracked, ↑ actioned |
| Owner adoption | GM/owner weekly active use of F11 | ≥ target % |
| Reporting efficiency | Time to produce an owner KPI snapshot | ↓ from hours to minutes |
| Data trust | KPI figures reconciled to source modules | ≥ 99% match |
| Decision speed | Time from cost drift to corrective action | ↓ measurable |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| Data pipeline / warehouse feed | F11 depends on clean, timestamped events from **all** modules (F9, F3, F8/POS, F4/F10, F12, PMS) | Shared event store / aggregation layer; defined KPI formulas; backfill + reconcile jobs |
| Data accuracy | A wrong KPI is worse than none — erodes owner trust | Source-of-truth per metric, reconciliation to modules, visible "as of" timestamps |
| Access control | Financials must never leak beyond GM/owner | Server-side RBAC (not UI-hidden only); F11 endpoints forbidden to all other roles |
| Single vs multi-property roll-up | Ownership may run several properties | Design KPI store property-scoped; resolve v1 scope (single) vs above-property roll-up (§11) |
| Read-only guarantee | F11 must never mutate operations | One-way consumer; no write paths from analytics to ops modules |
| Refresh cadence | Live vs daily rollup affects cost and expectations | Set and label cadence; cache rollups; show data freshness |
| Scope creep into pricing | Pressure to add rate recommendations | Hold the line — F11 is visibility/reporting; pricing engine is out of scope (§2.3) |

---

## 10. Scope delivered

Atrium Staff ships as one complete package. This feature delivers:

- KPI tiles with deltas (labor-cost %, occupancy, productivity, ancillary, SLA)
- Date-range + department filters
- GM/owner-only RBAC
- Cross-module feed for the full KPI set
- Trend charts (labor-cost, productivity)
- Export/share snapshots (PDF/image/CSV)
- Refresh-cadence controls
- Above-property / multi-property roll-up (if confirmed)
- Expanded KPI catalog
- Richer drill-downs — still visibility only, no pricing

---

## 11. Open Questions

- **Single-property vs above-property/multi-property roll-up for v1?** Does the launch owner need a consolidated multi-property view, or is single-property sufficient first? (Mirrors [Role-Based Design §7](staff-mobile-app-role-based-design.md#7-open-questions).)
- **Which KPIs are must-have for the launch owner?** Confirm the launch tile set and each metric's exact definition/formula.
- **Export formats.** PDF, image, CSV — which are required for v1, and is a branded owner-pack layout needed?
- **Refresh cadence.** Live (near-real-time rollup) vs daily batch — what does ownership expect, and what does the data pipeline support cost-effectively?
- **Data source of truth.** Which module owns each KPI's underlying figure, to avoid two systems disagreeing?

---

## 12. One-Line Business Case

> **Management Analytics / Owner KPIs is the app's single, GM/owner-only boardroom view — distinct from the shift-floor Overview — that makes operational waste visible and cuttable, giving ownership the labor, productivity, occupancy, SLA, and ancillary numbers to act on the savings every other feature generates. Visibility and reporting only — never a pricing engine.**
