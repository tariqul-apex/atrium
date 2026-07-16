# Labor Management on Mobile — Feature Deep-Dive

**Product:** Atrium Staff (Hotel / Resort Operations Mobile App)
**Feature area:** F9 — Labor Management (Scheduling + Attendance)
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-15
**Status:** Draft v1.0
**Related:** [Staff Mobile App — Feature Scope](staff-mobile-app-feature-scope.md) · [Role-Based Design](staff-mobile-app-role-based-design.md) · [Prioritization](staff-mobile-app-feature-prioritization.md) · [Competitive Feature Research](../HOTEL-~1.MD)

---

## 1. Executive Summary

**Labor Management** gives supervisors and managers a handheld system to build and publish rosters, and gives every frontline staff member a geofenced clock-in/out on their own device. It replaces the paper rota on the back-office wall, the wall-mounted punch clock, and the manual timesheet with a schedule-to-timesheet workflow that runs where the work happens.

Labor is the single largest **controllable** operating cost in a hotel. Rooms, utilities, and franchise fees are largely fixed; how many people are rostered against tonight's occupancy, and how many minutes of overtime bleed past shift end, are not. F9 is Atrium's **#1 manual-cost-reduction lever**: it right-sizes staffing against demand, cuts overtime and buddy-punching, and ends the manual rotas and paper timesheets that consume supervisor hours and hide waste.

Vendors treat the *demand* side of this as a core staff-ops capability — Amadeus HotSOS assigns workload by credits/minutes/area while respecting labor rules; ALICE/Actabl auto-generate task lists by employee work time and run operational analytics on labor; Stayntouch auto-assigns rooms by employee work time with role dashboards. Geofenced, on-device clock-in is Atrium's addition — vendor precedent for it is thinner, which is precisely the opening.

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What staff can do from the device |
|---|------------|-----------------------------------|
| 1 | **Shift scheduling / roster builder** | Build rosters by department, role, and shift; publish to staff so everyone sees their own schedule. |
| 2 | **Geofenced clock-in/out + break tracking** | Clock in/out and log breaks on-device inside the property geofence; timesheets auto-generate from the punches. |
| 3 | **Roster-to-demand** | Suggest staffing levels from the occupancy/arrivals forecast; surface overtime and coverage-gap alerts to managers. |
| 4 | **Shift-swap requests** | Staff request a swap; a manager approves or declines; the roster updates for everyone. |
| 5 | **Feeds ops surfaces** | Push housekeeping workload (credits/minutes) to F3 and live team status to the manager Overview. |
| 6 | **Event crews on the roster** | Take crew assigned to events in [F18](staff-mobile-app-events-activities-feature.md) as scheduled hours — an event within a person's shift adds no extra hours; only hours beyond their roster count as extra/overtime — all counted toward labor cost. |

### 2.2 In scope (this release)
Roster build/publish, geofenced clock-in/out + break tracking, timesheet auto-generation, roster-to-demand staffing suggestions, overtime/coverage alerts, shift-swap request + approval, workload feed to F3, team-status feed to the manager Overview, **event-crew intake from F18** (event assignments write into the roster as scheduled hours; overlap with an existing shift adds nothing, only hours beyond it flag as extra/overtime), timesheet **export** to payroll, offline clock-in queueing, RBAC.

### 2.3 Out of scope (this release)
Full HR/payroll **processing** (F9 exports timesheets; it does not calculate pay, run payroll, or cut cheques), benefits administration, recruiting/onboarding-as-HR, leave/absence entitlement management, and tax/statutory filing. (Integrate with the property's HR / T&A / payroll system — see §9.)

### 2.4 Key assumptions
- An occupancy/arrivals forecast is available from the PMS (or middleware) to drive roster-to-demand.
- Staff carry a device (company or BYOD) with GPS; the app degrades gracefully offline.
- Geofence radius, grace period, and on-device clock-in are acceptable under local labor agreements (see §11).

---

## 3. Capability Deep-Dive — The Labor Management Features

Each capability is elaborated — **what it is**, **how it works in Atrium**, **why it matters operationally**, and the **revenue/cost angle** — with vendor precedents noting the bar to clear.

### 3.1 Shift scheduling / roster builder (build + publish)
*Precedent: ALICE/Actabl (auto-generate task lists by employee work time), Stayntouch (role dashboards).*

**What it is.** Building a roster by department, role, and shift, then publishing it so every staff member sees their own upcoming schedule on the device.

**How it works in Atrium.** A supervisor drafts the week's roster in a grid — rows of staff, columns of shifts — filling coverage by role. On publish, each person's shifts land in their app; changes re-publish and notify the affected staff. The roster is the backbone the rest of F9 hangs off: attendance is measured against it, swaps modify it, and demand suggestions pre-fill it.

**Why it matters.** Paper and spreadsheet rotas live on one wall and in one person's head; staff phone in to ask when they work, changes don't propagate, and no-shows surface only when the shift is already short. A published, per-person mobile roster removes that friction and makes the schedule a shared source of truth.

**Revenue / cost angle.** Structured rostering is what makes right-sizing possible in the first place (cost); it reclaims the supervisor hours currently spent maintaining and re-explaining the rota (cost).

### 3.2 Geofenced clock-in/out + break tracking → auto-timesheets
*Precedent: thin — vendor T&A is typically separate hardware/HR software; on-device geofenced clock-in is Atrium's addition (an opening).*

**What it is.** Clocking in and out — and logging breaks — from the staff device, validated against the property geofence, with timesheets generated automatically from the punches.

**How it works in Atrium.** A staff member arriving on property opens the app and clocks in; the device confirms they are inside the geofence (with a configurable radius and grace period) before accepting the punch. Breaks are tracked the same way. Punches roll up into a timesheet per person per pay period, ready for a manager to review and **export** to payroll. Offline, punches queue on-device and reconcile on reconnect.

**Why it matters.** Wall-mounted punch clocks create queues and enable buddy-punching (one person clocking in for another); paper timesheets are transcribed by hand, late, and error-prone. Geofenced on-device attendance ties the punch to a real presence on property and turns hours into a clean, auditable timesheet with no re-keying.

**Revenue / cost angle.** Eliminating buddy-punching and rounding-up on paper timesheets directly trims the payroll line (cost); auto-generated timesheets remove hours of manual transcription and reconciliation each pay period (cost).

### 3.3 Roster-to-demand + overtime & coverage alerts
*Precedent: Amadeus HotSOS (workload by credits/minutes/area, respecting labor rules), ALICE/Actabl (operational analytics on task completion and labor).*

**What it is.** Suggesting staffing levels from the occupancy/arrivals forecast, and alerting managers when a draft roster is heading into overtime or leaving a shift short.

**How it works in Atrium.** F9 reads the PMS forecast — occupancy, arrivals, departures — and translates it into suggested headcount per department and shift (e.g., attendants needed for the projected checkout/stayover mix, front-desk cover for the arrivals curve). As the manager builds, the app flags where projected hours tip an individual into overtime, or where a shift falls below the suggested level. Housekeeping demand is expressed in the same credits/minutes vocabulary that feeds F3.

**Why it matters.** Rostering blind to demand is where labor cost leaks in both directions: over-staff a quiet Tuesday and pay for idle hours; under-staff a full-house Saturday and pay overtime plus miss SLAs. Tying the roster to the forecast, with alerts, closes both leaks before the schedule is published.

**Revenue / cost angle.** Matching labor to demand is the core cost lever — fewer idle hours and less reactive overtime (cost); adequate coverage on high-occupancy days protects SLAs, faster room turns, and the guest experience that sustains rate (revenue protected).

### 3.4 Shift-swap requests with manager approval
*Precedent: general staff-ops workflow (ALICE/Actabl unified ops + approvals pattern).*

**What it is.** A staff member requesting to swap or give away a shift, routed to a manager for approval before the roster changes.

**How it works in Atrium.** Staff raise a swap from their own shift, optionally naming a colleague to take it; the request goes to the supervisor, who approves or declines. On approval the roster updates for everyone and both parties are notified. Coverage and overtime checks (§3.3) run on the proposed swap so a manager approves with the cost consequence visible.

**Why it matters.** Swaps today happen over WhatsApp and verbal deals that never reach the rota — so the person who shows up isn't the person the schedule expects, and no-shows get blamed on confusion. A tracked request/approval flow keeps the published roster true and keeps a manager in control of coverage and cost.

**Revenue / cost angle.** Approved-and-recorded swaps prevent the accidental double-pay and uncovered shifts that informal swaps cause (cost); they cut the manager back-and-forth of arranging cover manually (cost).

### 3.5 Feeds F3 housekeeping workload and the manager Overview
*Precedent: Amadeus HotSOS (workload by credits/minutes/area), Stayntouch (auto room assignment by employee work time), ALICE/Actabl (mobile task reassignment).*

**What it is.** Publishing who is on shift — and their available work-time/credits — into the [Housekeeping](staff-mobile-app-housekeeping-management-feature.md) workload engine (F3) and the live manager Overview.

**How it works in Atrium.** Because F9 knows who has clocked in and their role, F3 can distribute room credits/minutes only across attendants actually present, and the manager Overview shows real-time team status — on shift, clocked in, on break, off — instead of an assumed roster. Attendance truth flows into assignment and into supervision.

**Why it matters.** Housekeeping assignment and the manager's live view are only as good as their picture of who is actually here. Wiring attendance into both means workload lands on present staff and supervisors act on reality, not the plan.

**Revenue / cost angle.** Assigning against real presence avoids the over/under-allocation that causes missed turns or idle attendants (cost); accurate live status lets managers rebalance fast on high-occupancy days (revenue protected).

### 3.6 Event crews feed the roster (F18 intake)
*Precedent: banquet-labor budgeting; unified staff scheduling (7shifts, HotSchedules).*

**What it is.** Crew assigned to an event in [F18 Events, Activities & Banquet](staff-mobile-app-events-activities-feature.md) flow **into** F9 as scheduled hours, so events are not a labor blind spot — they appear on the roster and count toward labor cost like any other shift.

**How it works in Atrium.** When an event's crew is published, F9 writes each person's event hours for the event window. An event that falls **within** a person's existing shift is not a conflict — it is simply what they are doing that shift and adds no extra hours or cost. Only hours **beyond** their rostered shift — running past shift end, or on a day off — are added and flagged to the manager as **extra/overtime** before publish. Those hours then roll into the same timesheets, labor-cost %, and owner KPI view (F11) as every other shift. Only scheduling is shared — the event-specific *role* a person plays (Bar Lead, Setup, etc.) stays inside F18 and does not change their F9 identity or app permissions.

**Why it matters.** Events staffed "on the side" are the classic hidden-labor leak: people double-booked, event overtime invisible, and the owner's labor-cost % understated. Taking event crews into the roster keeps the property's single biggest controllable cost complete and honest.

**Revenue / cost angle.** Bringing event labor into the same demand/overtime discipline as regular shifts lets managers right-size event crews and pre-empt overtime (cost); complete labor-cost reporting makes event profitability real to ownership (cost/visibility).

---

## 4. Why This Matters

### 4.1 Labor is the largest controllable cost
Room, utility, and fee costs are largely fixed; labor is the line management can actually move day to day. A tool that right-sizes staffing against demand and tightens attendance attacks the biggest cuttable number on the P&L.

### 4.2 Demand-blind rostering leaks money both ways
Over-staffing quiet periods pays for idle hours; under-staffing busy periods pays overtime and breaks SLAs. Roster-to-demand with overtime and coverage alerts closes both leaks before publish.

### 4.3 Attendance is only real if it's verified
Wall clocks and paper timesheets invite buddy-punching, queues, and transcription error. Geofenced on-device clock-in ties hours to real presence and produces a clean, auditable, export-ready timesheet.

### 4.4 A competitive opening in the BD market
Vendors do the demand side (workload by credits/minutes, task lists by work time) but on-device geofenced attendance for hotel frontline staff is thinly served. Atrium can lead locally by joining scheduling, attendance, and the ops surfaces in one app.

---

## 5. Operations It Improves

| Operation today | Pain | With Labor Management |
|-----------------|------|-----------------------|
| **Building the rota** | Wall spreadsheet; one person owns it | Roster built and published to every staff member's device. |
| **Setting headcount** | Guessed against a full house | Forecast-driven staffing suggestions with overtime/coverage alerts. |
| **Clocking in** | Punch-clock queue; buddy-punching | Geofenced on-device clock-in tied to real presence. |
| **Timesheets** | Hand-transcribed, late, error-prone | Auto-generated from punches, export-ready for payroll. |
| **Shift swaps** | WhatsApp deals never reach the rota | Tracked request → manager approval → roster updates for all. |
| **Assigning work** | Based on an assumed roster | F3 distributes workload across staff actually clocked in. |

---

## 6. How It Increases Revenue

Labor Management is primarily a **cost** feature; its revenue contribution is indirect but real.

1. **Coverage protects rate and SLAs.** Forecast-matched staffing on high-occupancy days keeps room turns fast and guest requests on-SLA — protecting the reviews and readiness that sustain occupancy and rate.
2. **Freed manager time redirects to revenue work.** Ending manual rotas, timesheet transcription, and swap-chasing gives supervisors hours back for guest recovery, upselling coaching, and floor presence.
3. **Right-sized service, not thinned service.** Cutting *idle* hours (not needed hours) preserves the service level that repeat and direct business depends on.

> **Illustrative model:** For a 150-room property with a monthly frontline payroll of **৳ 45,00,000**, trimming overtime and over-staffing by a conservative **3%** saves ≈ **৳ 1,35,000/month** (**≈ ৳ 16,20,000/year**) — before counting reclaimed supervisor hours and avoided buddy-punching. Even a **1%** improvement is ≈ **৳ 5,40,000/year**. *(Figures illustrative; validate against property payroll data — the actual payroll base, overtime rate, and over-staffing headroom vary by property.)*

---

## 7. How It Reduces Operational Cost

1. **Right-sized labor.** Forecast-driven rostering with overtime/coverage alerts cuts idle hours and reactive overtime — the core lever.
2. **No buddy-punching / timesheet padding.** Geofenced attendance ties hours to real presence, trimming the payroll line directly.
3. **No manual rotas or timesheets.** Published rosters and auto-generated, export-ready timesheets remove hours of supervisor and back-office admin every cycle.
4. **Fewer swap-driven errors.** Approved, recorded swaps prevent accidental double-pay and uncovered shifts.

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Right-sized labor | Labor cost % of revenue | ↓ measurable |
| Less overtime | Overtime hours as % of scheduled hours | ↓ 20% |
| Verified attendance | Clock-ins passing geofence validation | ≥ 95% |
| Admin removed | Timesheets auto-generated (no manual entry) | 100% |
| Coverage | Shifts published meeting suggested staffing | ≥ target % |
| Adoption | Staff viewing rosters / clocking in via app | 80% within 90 days |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| Forecast feed | Occupancy/arrivals forecast needed from PMS | Middleware sync; fall back to manual headcount targets |
| Labor-agreement acceptance | Geofenced/on-device clock-in may need union/staff sign-off | Configurable radius + grace; consult labor rules early (§11) |
| Device GPS / battery | Clock-in depends on location + a charged device | Grace window, manual-override with manager approval, low-power punch |
| Offline clock-in | Dead zones at shift start/end | Queue punches on-device; reconcile on reconnect |
| Payroll integration | Export must match the payroll system's format | Define export target early; standard timesheet export schema |
| Adoption | Staff used to punch clocks/paper | Simple UX, on-shift training, manager buy-in |

---

## 10. Scope delivered

Atrium Staff ships as one complete package. This feature delivers:

- Roster build/publish
- Geofenced clock-in/out + break tracking
- Auto-timesheets with payroll export
- Offline punch queueing
- RBAC
- Roster-to-demand suggestions
- Overtime/coverage alerts
- Shift-swap request + approval
- F3 workload feed (credits/minutes) and manager Overview team-status feed
- Event-crew intake from F18 (event assignments write into the roster; overlap with a shift adds nothing, only extra hours flag as overtime)
- HR/T&A/payroll integration

---

## 11. Open Questions

- Is geofenced, on-device clock-in acceptable to staff and under local labor agreements?
- What geofence radius and grace period balance anti-buddy-punching with fairness (large sites, staff entrances, break areas)?
- Which payroll / HR / T&A system(s) must F9 export to at launch, and in what format?
- What is the source and granularity of the occupancy/arrivals forecast driving roster-to-demand?
- BYOD vs. company devices — how is GPS/battery reliability guaranteed for attendance?

---

## 12. One-Line Business Case

> **Labor Management attacks the single largest controllable cost in the hotel — right-sizing staffing to demand and tying every clock-in to real presence, so payroll matches the work from the supervisor's and staff member's hand.**
