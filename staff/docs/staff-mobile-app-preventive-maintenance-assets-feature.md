# Preventive Maintenance & Asset Register on Mobile — Feature Deep-Dive

**Product:** Atrium Staff (Hotel / Resort Operations Mobile App)
**Feature area:** F10 — Preventive Maintenance & Asset Register
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-15
**Status:** Draft v1.0
**Related:** [Feature Scope](staff-mobile-app-feature-scope.md) · [Maintenance (reactive) — Deep-Dive](staff-mobile-app-maintenance-work-orders-feature.md) · [Prioritization](staff-mobile-app-feature-prioritization.md) · [Competitive Feature Research](../HOTEL-~1.MD)

---

## 1. Executive Summary

**Preventive Maintenance & Asset Register** extends the reactive maintenance feature ([F4 — Mobile Maintenance & Work Orders](staff-mobile-app-maintenance-work-orders-feature.md)) from *fix-it-when-it-breaks* to *service-it-before-it-breaks*. It adds a register of the property's physical assets and a scheduler that raises planned work orders automatically — by calendar or by meter reading — so engineering services equipment on a plan instead of only reacting to failures.

Crucially, **PM and reactive maintenance share one work-order model.** A scheduled AC service and a guest-reported broken AC are the same kind of tracked, photo-rich, assignable work order — just triggered differently. F4 answers "who's fixing what's broken now"; F10 answers "what should we service before it breaks, and what do we own." One inbox, one lifecycle, one history.

Where reactive maintenance protects revenue by shortening downtime, preventive maintenance protects it by **preventing the downtime in the first place** — avoiding the catastrophic failure, the long out-of-order (OOO) stretch, and the premature replacement of expensive plant. This is a **pure cost-protection and asset-protection** feature: it defers capital expenditure by extending asset life and shrinks the emergency-repair bill.

Vendors treat asset history and preventive tasks as part of the maintenance pillar: ALICE/Actabl and Quore ship preventive maintenance with asset history and mobile readings (e.g., pool chemistry, minibar); protel and WebRezPro (maintenance alarms) round out the reactive side that this extends.

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What staff can do from the device |
|---|------------|-----------------------------------|
| 1 | **Asset register** | Maintain a record per asset — location, model/serial, install date, warranty terms, and full service history. |
| 2 | **Scheduled / recurring PM tasks** | Define preventive schedules by time (e.g., quarterly) or by meter reading (e.g., every 500 generator-hours); auto-generate a work order when one is due. |
| 3 | **Meter / reading logging** | Log readings from the device — pool chemistry, generator run-hours, boiler pressure, filter cycles — against the asset. |
| 4 | **Overdue-PM alerts** | Flag and escalate PM tasks that are due or overdue so nothing lapses silently. |
| 5 | **Asset history for decisions** | Use the accumulated record to support warranty claims, insurance, and repair-vs-replace / capex decisions. |

### 2.2 In scope (this release)
Asset register (location, warranty, service history); time- and meter-based PM scheduling that auto-generates work orders into the shared F4 model; on-device meter/reading logging against assets; overdue-PM alerts and escalation; asset service history for warranty/insurance/capex; RBAC; offline support (inherited from F4).

### 2.3 Out of scope (this release)
Full CMMS/EAM, parts-inventory purchasing and procurement, capital-project management, and energy/BMS automation. (Integrate with a CMMS where one is the system of record — see §9.)

### 2.4 Key assumptions
- The reactive maintenance feature (F4) is live; F10 reuses its work-order lifecycle, assignment, photo/note evidence, and OOO-with-PMS-sync mechanics.
- Someone seeds the asset register (the main onboarding effort — see §9).
- OOO room/area status syncs to the PMS (real-time write) or via middleware, exactly as in F4.
- Technicians have devices; the app degrades gracefully offline in plant rooms and basements.

---

## 3. Capability Deep-Dive — The Preventive Features

Each capability is elaborated — **what it is**, **how it works in Atrium**, **why it matters operationally**, and the **revenue/cost angle** — with vendor precedents noting the bar to clear.

### 3.1 Asset register (location, warranty, service history)
*Precedent: ALICE/Actabl, Quore (asset history / equipment status).*

**What it is.** A single record for each physical asset the property maintains — HVAC units, chillers, lifts, pool plant, kitchen equipment, generators, water heaters — capturing location, model/serial, install/commission date, warranty terms, and a running service history.

**How it works in Atrium.** Every work order (reactive *or* preventive) is logged against an asset, so the asset accumulates its own history automatically over time. Opening an asset shows what it is, where it is, when its warranty ends, and every service and failure it has had — with the photos and notes from those jobs already attached.

**Why it matters.** Without a register, "the history of the AC in room 412" lives in someone's memory and a stack of paper. Engineering can't tell whether a unit is under warranty, how many times it's failed this year, or whether it's cheaper to keep repairing or replace it. The register turns each asset into an auditable, decision-ready object.

**Revenue / cost angle.** Warranty terms surfaced at point of repair recover costs that would otherwise be paid out of pocket (cost); failure history feeds smarter repair-vs-replace and capex timing (cost / capex deferral); the record supports insurance claims and disputes (cost / risk).

### 3.2 Scheduled / recurring PM tasks — auto-generated work orders
*Precedent: ALICE/Actabl, Quore (preventive maintenance tasks).*

**What it is.** Preventive schedules attached to assets that fire on a rule — a **time** interval (monthly filter change, quarterly HVAC service, annual lift inspection) or a **meter** threshold (service the generator every 500 run-hours). When a schedule is due, Atrium auto-generates a work order.

**How it works in Atrium.** A supervisor defines the schedule once on the asset. When it comes due, Atrium raises a work order into the **same F4 inbox** as reactive jobs — assignable, trackable, photo-verified, and closed the same way. On completion, the asset's history updates and the next occurrence is scheduled. PM work sits alongside reactive work so engineering plans its week around both.

**Why it matters.** Preventive work is exactly what gets dropped when a team runs reactive-only: it's never today's fire. Auto-generation removes the human memory step — the service happens because the system raised the ticket, not because someone remembered. Reusing the F4 lifecycle means no second tool and no retraining.

**Revenue / cost angle.** Serviced equipment fails less, so fewer guest-facing breakdowns and fewer long OOO stretches (revenue protected); planned service is cheaper than emergency call-outs and extends asset life, deferring replacement capex (cost / capex).

### 3.3 Meter / reading logging on mobile
*Precedent: Quore (readings such as pool, minibar), ALICE/Actabl.*

**What it is.** Capturing operational readings from the device against the relevant asset — pool chemistry (pH, chlorine), generator run-hours, boiler pressure, temperatures, filter cycles — as a timestamped, attributable log.

**How it works in Atrium.** A technician on rounds logs the reading on the asset in a few taps. Readings both build a compliance-ready record and **drive meter-based PM triggers** (§3.2) — logging generator hours is what tips a run-hours schedule into a work order. Out-of-range readings can flag an alert.

**Why it matters.** Readings on paper clipboards are hard to trend, easy to fabricate, and invisible until an inspector asks. On-device logging makes them attributable and searchable, and lets a threshold do useful work — turning a number into a scheduled action instead of a note nobody reads.

**Revenue / cost angle.** Early detection of drift (pool chemistry, pressure) prevents the failure and the closure that follows (revenue protected); a clean reading log lowers the cost and risk of compliance audits (cost / risk).

### 3.4 Overdue-PM alerts
*Precedent: WebRezPro (maintenance alarms), ALICE/Actabl.*

**What it is.** Proactive flagging and escalation of PM tasks that are due or overdue, so a lapsed service is visible to a supervisor rather than silently skipped.

**How it works in Atrium.** Due and overdue PM work is surfaced on the engineering view and escalated on the same priority/SLA rails as reactive orders (F4). A statutory or high-risk PM that slips past its window escalates rather than sitting unnoticed.

**Why it matters.** A preventive program is only as good as its follow-through. Without overdue visibility, "we do quarterly service" quietly becomes "we did it twice this year" — and the gap only shows up as a failure. Alerts keep the plan honest.

**Revenue / cost angle.** Prevents the lapse that becomes a breakdown and an OOO room/area (revenue protected); keeps compliance-mandated servicing (fire, lift, pool) inside its window (cost / risk).

### 3.5 Asset history for warranty, insurance & capex decisions
*Precedent: ALICE/Actabl, Quore (asset history).*

**What it is.** Using the register's accumulated service and failure record to make money decisions — claim under warranty, support an insurance claim, and time repair-vs-replace and capital spend on evidence.

**How it works in Atrium.** Because every job is logged against its asset, each asset carries a complete, dated, photo-backed history with no extra effort. Management can see which assets consume the most repair spend and downtime, and plan replacements before, not after, the failure.

**Why it matters.** Capex on plant is one of the largest controllable line items ownership faces, and it's usually decided on gut feel or a breakdown. A real history moves those decisions onto data — deferring spend on assets with life left and prioritizing the ones bleeding cost.

**Revenue / cost angle.** Warranty and insurance recovery offsets repair cost (cost); evidence-based replacement timing extends asset life and defers capex (capex); fewer surprise failures on unmonitored assets protects inventory (revenue).

---

## 4. Why This Matters

### 4.1 Prevention beats recovery
F4 shortens the time from broken to fixed; F10 attacks the *broke in the first place*. The cheapest OOO room-day and the cheapest comp are the ones that never happen — and planned service is how you avoid them.

### 4.2 Catastrophic failure and long OOO downtime are the expensive kind
A missed filter change or an unwatched chiller doesn't fail cheaply — it fails hard, taking rooms or whole areas out of service for days and turning a routine service into an emergency call-out. Preventive service trades a small planned cost for a large avoided one.

### 4.3 Asset life is deferred capex
Equipment that's serviced on plan lasts longer. Every year of extra life on a chiller, lift, or kitchen line is a capital replacement pushed further out — real money to ownership, and squarely in the "authority" concern this cost-and-control layer targets.

### 4.4 It costs almost nothing to build on F4
Because PM reuses the reactive work-order model, lifecycle, and evidence, F10 is mostly a register plus a scheduler on top of a feature that already exists. High protection, incremental build.

---

## 5. Operations It Improves

| Operation today | Pain | With Preventive Maintenance & Assets |
|-----------------|------|--------------------------------------|
| **Servicing equipment** | Ad-hoc, from memory; skipped under load | Schedules auto-raise work orders by time or meter — nothing forgotten. |
| **Knowing what we own** | Assets and warranties live in memory/paper | A register with location, warranty, and full service history. |
| **Readings (pool, generator)** | Paper clipboards, un-trendable | Logged on device, attributable, and driving meter-based triggers. |
| **Preventing failures** | React only after it breaks | Overdue-PM alerts keep the plan on track before failure. |
| **Repair-vs-replace / capex** | Gut feel, or forced by a breakdown | Asset history makes the call on evidence and defers spend. |

---

## 6. How It Increases Revenue

F10 is primarily cost protection; the "revenue" it produces is revenue *protected* — inventory and rate power kept intact by avoiding failures.

1. **Fewer emergency breakdowns.** Serviced equipment fails less, so fewer guest-facing failures (the comp-and-bad-review kind) and fewer emergency OOO closures on high-occupancy days.
2. **Shorter and rarer OOO stretches.** Planned service avoids the multi-day OOO stretch that a catastrophic failure forces — keeping rooms and revenue-earning areas (pool, restaurant, spa) sellable.
3. **Extended asset life → deferred capex.** Longer equipment life pushes out capital replacements, freeing capital for revenue-generating uses.

> **Illustrative model.** Suppose preventive service on the register's critical assets avoids **2 premature equipment replacements** over the register's first two years — say a chiller and a rooftop AC bank at ~৳9,00,000 each — that is roughly **৳18,00,000 in deferred/avoided capex**. Separately, if it eliminates even **20 emergency OOO room-days/year** at a ৳12,000 ADR, that's **~৳2,40,000/year** in protected room revenue, before counting avoided emergency call-out premiums and protected reviews. *(Figures purely illustrative; validate against property data and asset costs.)*

---

## 7. How It Reduces Operational Cost

1. **Planned service is cheaper than emergency repair.** Scheduled work avoids out-of-hours call-out premiums, expedited-parts costs, and the collateral damage a failure causes to connected equipment.
2. **Warranty and insurance recovery.** Warranty terms surfaced at point of repair, plus a photo-backed asset history, recover costs and support claims that would otherwise be absorbed.
3. **Deferred capital expenditure.** Extending asset life on evidence pushes out the single largest controllable engineering line item — capital replacement.
4. **Lower compliance-audit cost and risk.** On-device reading and service logs make mandated servicing (fire, lift, pool) auditable in minutes, not binders.

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| PM discipline | PM tasks completed on schedule (in-window) | ≥ 90% |
| Prevention working | Reactive/emergency work orders on covered assets | ↓ measurable |
| Less downtime | Unplanned OOO room-days from equipment failure | ↓ measurable |
| Register coverage | Critical assets with a complete register record | ≥ target % |
| Capex protection | Cost recovered via warranty / avoided premature replacement | ↑ (tracked) |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| Register seeding | Entering the asset base is real up-front effort | Prioritize by criticality (HVAC, lifts, pool, kitchen first); bulk import; grow the register as jobs are logged |
| Shared model with F4 | F10 depends on F4's work-order lifecycle being live | Sequence F10 after F4; reuse, don't fork, the model |
| PMS OOO sync | Planned OOO windows must reflect in the PMS | Middleware sync; queue + reconcile (inherited from F4) |
| CMMS overlap | Property may already run a CMMS/EAM as system of record | Integrate/sync rather than duplicate; F10 as mobile front-end where a CMMS owns the data |
| Meter-based triggers | Triggers only fire if readings are logged | Simple on-round logging UX; overdue-reading alerts; sensible time-based fallback schedules |
| Compliance scope | Mandated PM logs (fire, lift, pool) vary by market | Confirm target-market requirements before committing to statutory templates |

---

## 10. Scope delivered

Atrium Staff ships as one complete package. This feature delivers:

- Asset register (location, warranty, service history)
- Time-based PM schedules auto-generating work orders into the shared F4 model
- Overdue-PM alerts
- RBAC
- Meter/reading logging on mobile and meter-based PM triggers
- Asset-history reporting for warranty/capex decisions
- Compliance-PM templates (fire, lift, pool)
- CMMS integration where a CMMS is the system of record

---

## 11. Open Questions

- Is there an existing CMMS/EAM at the launch property, and if so is it the system of record (with F10 as a mobile front-end) or is Atrium the system of record?
- Which asset classes do we track first — HVAC, lifts, pool plant, kitchen equipment, generators?
- What preventive-maintenance logs are compliance-mandated (fire, lift, pool) in the target market, and what evidentiary/retention rules apply?
- How is the register seeded at onboarding — bulk import, staggered manual entry, or accrue-as-you-go from work orders?
- Does the launch PMS support the real-time OOO status writes that planned-service windows need (shared with F4)?

---

## 12. One-Line Business Case

> **Preventive Maintenance & Assets turns maintenance from break-fix into service-before-it-breaks — using one shared work-order model with F4 to prevent costly failures and long OOO downtime, extend asset life, and defer capital spend, all from the technician's hand.**
