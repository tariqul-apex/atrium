# Atrium Staff — Role-Based Design & Access

**Product:** Atrium Staff (Hotel / Resort Operations Mobile App)
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-15
**Status:** Draft v1.0
**Related:** [Overview & Index](../overview.md) · [Navigation & Feature Map](staff-mobile-app-navigation-map.md) · [Feature Prioritization](staff-mobile-app-feature-prioritization.md) · [design.md](../design.md)

The app is **one codebase, role-adaptive**. RBAC controls not just *data* but **which tabs, screens, and actions each role even sees** — the principle is **least privilege / least clutter**: a role sees only what its job needs, and everything else is invisible (not greyed-out, invisible). This document defines the roles, a feature-by-role visibility matrix, per-role navigation, and the **Line Manager operational overview**.

> **Note:** the Line Manager "at-a-glance overview" is **operational monitoring** — current status, recent activity, team status, exceptions — i.e., the "live department view" (see [scope doc](staff-mobile-app-feature-scope.md) F7 supervisor tools). It shows live operational state, not financial analytics: no RevPAR/ADR charts, no reports/exports.

---

## 1. Roles (personas)

| Role | Who | Core job on the app |
|------|-----|---------------------|
| **Front Desk Agent** | Reception / guest services | Arrivals, check-in/out, folio, payment, keys, reservations |
| **Housekeeping Attendant** | Room attendant | Clean assigned rooms, update status, log minibar, raise issues |
| **Maintenance Tech** | Engineering / facilities | Accept and close reactive work orders with photos/parts |
| **F&B Staff** | Waiter / room-service / bar | Take F&B orders, charge-to-room, take payment |
| **Line Manager** | Department supervisor (HK / Front Office / Engineering / F&B) | **Monitor department at a glance**, assign & reassign work, build rosters (F9), manage inventory/inspections, approve exceptions |
| **Duty Manager / GM** | Property manager | Cross-department overview, escalations, full access, **owner KPIs (F11)** |
| **Owner / Above-property** | Ownership / regional | **View performance KPIs (F11) only** — no operational tasks |

> **Line Manager is department-scoped.** A Housekeeping line manager, Front-Office line manager, Engineering line manager, and F&B line manager see the same *overview shape* but tuned to their department; they get full monitor/assign inside their department and view-only (or nothing) for others. The **GM** is the cross-department, property-wide version.

---

## 2. Feature × Role visibility matrix

**✅ Full** · **◐ Limited** (scoped subset) · **— Hidden** (invisible)

| Feature / area | FD Agent | HK Attendant | Maint Tech | F&B Staff | Line Manager* | Duty / GM |
|----------------|:-------:|:------------:|:----------:|:---------:|:-------------:|:---------:|
| 🏠 Home / Overview | Agent launcher | HK launcher | Maint launcher | F&B launcher | **✅ Dept Overview** | ✅ Property Overview |
| F1 Front Desk (check-in/out, folio, key) | ✅ | — | — | ◐ charge-to-room only | ✅ *(FO)* / — | ✅ |
| F2 Reservations (calendar, create/edit) | ✅ | — | — | — | ✅ *(FO)* / — | ✅ |
| F3 Housekeeping (room board, checklist) | ◐ view room-ready | ✅ my rooms | ◐ via work order | — | ✅ *(HK)* monitor/assign | ✅ |
| F4 Maintenance (reactive work orders) | ◐ raise only | ◐ raise only | ✅ my WOs | ◐ raise only | ✅ *(Eng)* monitor/assign | ✅ |
| F5 Task & Comms | ✅ | ◐ raise + message | ◐ raise + message | ◐ raise + message | ✅ assign/monitor | ✅ |
| F6 Notifications | ✅ | ✅ | ✅ | ✅ | ✅ (+ escalations) | ✅ (+ critical) |
| F8 F&B POS | ◐ charge-to-room | — | — | ✅ | ✅ *(F&B)* / — | ✅ |
| F9 Labor — my shifts / clock-in | ✅ self | ✅ self | ✅ self | ✅ self | ✅ + roster *(dept)* | ✅ + roster |
| F10 Preventive Maint. & assets | ◐ via WO | — | ✅ my PM WOs | — | ✅ *(Eng)* schedule/assign | ✅ |
| F11 Owner KPIs / analytics | — | — | — | — | — | ✅ *(GM/owner)* |
| F12 Inventory & par-stock | — | ◐ minibar | — | ◐ F&B stock | ✅ *(dept)* | ✅ |
| F13 Inspections & compliance | — | ◐ do assigned | ◐ do assigned | ◐ do assigned | ✅ *(dept)* schedule/review | ✅ |
| F14 Guest feedback & recovery | ◐ log/act | — | — | — | ✅ *(dept)* | ✅ |
| F15 SOP / training | ✅ | ✅ | ✅ | ✅ | ✅ + author | ✅ + author |
| F16 Lost & Found | ✅ | ◐ log found | — | — | ✅ | ✅ |
| F16 Staff SOS (panic) | ✅ send | ✅ send | ✅ send | ✅ send | ✅ send + receive | ✅ send + receive |
| F17 NFC / barcode scan | ◐ ID/booking, key | ◐ room + minibar | ◐ asset tags | ✅ scan-to-order | ✅ *(dept)* count/receive | ✅ |
| F18 Events & Activities | ◐ enquiry / activity sign-up | ◐ assigned crew tasks | ◐ assigned crew tasks | ✅ crew + service tasks | ✅ *(dept)* plan/crew/roles | ✅ |
| F18 Event-role overlay | scoped to event | scoped to event | scoped to event | scoped to event | ✅ assign roles | ✅ assign roles |
| ⋯ More (Guest profiles) | ✅ | — | — | — | ✅ | ✅ |
| ⚙ Settings | self | self | self | self | self + team | self + admin |

*\*Line Manager column shows the **department-relevant** capability; the *(FO/HK/Eng/F&B)* tag means "full only for that department's manager, hidden otherwise."*

**Hard invisibility rules (a role must never see):**
- Attendants/techs/F&B never see **financials/RevPAR** or **other departments' management tools**.
- **F11 Owner KPIs / analytics is GM-and-owner-only** — no frontline or line-manager role sees the business-analytics surface (line managers get the *operational* Overview, not analytics).
- Housekeeping/Maintenance/F&B never see the **Desk** (check-in/folio/payment) beyond what their task needs (e.g., F&B charge-to-room).
- Frontline staff see **their own** assignments, shifts, and inventory scope only — not the department roster, workload board, or par-stock management (those are manager actions).
- **Rosters (F9) and PM scheduling (F10)** are manager actions; frontline see only *their* shifts and *their* assigned PM/inspection tasks.
- Only **GM** sees the cross-department property overview.
- **Staff SOS (F16) is universal to send**; only managers/GM/security *receive* alerts.

> **Custom event-role overlay (F18).** Events assign each crew member a **custom, event-scoped work role** (Event Captain, Setup Lead, Bar Lead, AV, Registration/Host, Activity Guide, Safety Marshal, Runner) that defines their duties **for that event only**. This is an **overlay on top of base RBAC — it does not change a person's department, permissions, or what they see in the rest of the app.** A housekeeper assigned as "Bar Lead" for a wedding gets that event's bar checklist inside their own F5 Tasks, but remains a housekeeper everywhere else; the same person can hold different roles on different events. Only **line managers (their department) and the GM** can create/assign event roles; crew see only their own role's tasks. Event roles are configurable per property (a starter library ships; managers tune it).

---

## 3. Per-role navigation (adaptive bottom tab bar)

The 5-tab bar shows only the tabs a role needs; the **landing tab** differs by role.

| Role | Bottom tabs (left→right) | Lands on | Hidden tabs |
|------|--------------------------|----------|-------------|
| **Front Desk Agent** | 🏠 Home · 🛎️ Desk · 💬 Inbox · ⋯ More | 🛎️ Desk | ✅ Tasks |
| **Housekeeping Attendant** | 🏠 Home · ✅ Tasks · 💬 Inbox | ✅ Tasks (room board) | 🛎️ Desk · ⋯ More |
| **Maintenance Tech** | 🏠 Home · ✅ Tasks · 💬 Inbox | ✅ Tasks (my WOs) | 🛎️ Desk · ⋯ More |
| **F&B Staff** | 🏠 Home · 💳 POS · ✅ Tasks · 💬 Inbox | 💳 POS | 🛎️ Desk · ⋯ More |
| **Line Manager** | 🏠 Overview · ✅ Tasks · 🛎️ Desk† · 💬 Inbox · ⋯ More | 🏠 **Overview** | — (department-scoped) |
| **Duty / GM** | 🏠 Overview · ✅ Tasks · 🛎️ Desk · 💬 Inbox · ⋯ More | 🏠 **Overview** | — (full) |
| **Owner / Above-property** | 📊 KPIs · ⋯ More | 📊 **Performance / KPIs** | ✅ Tasks · 🛎️ Desk · 💳 POS (all operational tabs) |

*† Desk tab appears for Front-Office line managers; other line managers see their department's primary tab instead (HK → room board, Eng → work orders, F&B → POS).*

**Where F9–F18 live in the tab bar (no new bottom tabs):**
- **F9 My Shifts / Clock-in** — every role, under ⋯ More (managers also get **Roster** there); a shift card also surfaces on 🏠 Home.
- **F10 Preventive Maintenance** — inside ✅ Tasks → Work orders (alongside reactive); **Asset register** under ⋯ More.
- **F11 Owner KPIs** — ⋯ More → **Performance / KPIs**, GM/owner only (owner lands here).
- **F12 Inventory**, **F15 Knowledge base**, **F16 Lost & Found** — under ⋯ More (scoped).
- **F13 Inspections** — inside ✅ Tasks. **F14 Feedback** — inside 💬 Inbox.
- **F16 Staff SOS** — global button on every screen (send for all; receive for managers/GM).
- **F17 NFC & Barcode Scanning** — a cross-cutting scan control surfaced *inside* POS, Inventory, Assets, rooms, and Desk; not a tab.
- **F18 Events, Activities & Banquet** — under ⋯ More (create/plan/crew); crew tasks surface in ✅ Tasks; the event-role overlay is event-scoped (see the callout above).

### 3.1 What each role does NOT see (invisibility, by role)
- **Front Desk Agent:** no housekeeping assign/reassign, no work-order management, no POS management (charge-to-room only), no roster/inventory management, no owner KPIs, no team/approvals.
- **Housekeeping Attendant:** no Desk, no reservations, no POS, no other attendants' boards, no roster/PM scheduling, no owner KPIs, no financials — just *my rooms*, *my shifts*, minibar stock, assigned inspections + messaging.
- **Maintenance Tech:** no Desk, no reservations, no POS, no housekeeping board, no owner KPIs — just *my work orders* (reactive + PM), *my shifts* + messaging.
- **F&B Staff:** no Desk (except charge-to-room), no reservations, no housekeeping/maintenance boards, no owner KPIs — just POS, F&B stock, my F&B tasks, *my shifts* + messaging.
- **Line Manager:** everything for **their department** (incl. roster, PM scheduling, inventory, inspections, feedback); **other departments hidden or view-only**; **no owner KPIs (F11)**; no property-wide GM escalations.
- **Duty / GM:** everything, cross-department, plus admin and **owner KPIs (F11)**.
- **Owner / above-property:** **F11 KPIs only** — no operational tabs or task actions.

---

## 4. Home is role-variant

The 🏠 Home tab is **not one screen** — it adapts:

| Role | Home variant | Content |
|------|--------------|---------|
| Frontline (FD / HK / Maint / F&B) | **Launcher** | Personal counts (my arrivals / my rooms / my WOs / my orders), my alerts, quick actions. No team data. |
| **Line Manager / GM** | **Operational Overview** | Department current status + recent operations feed + team status + exceptions/approvals + quick actions. |

The frontline **launcher** (design.md S3) stays focused on the individual. The manager **Overview** (new — §5) is the "at a glance" screen you asked for.

---

## 5. Line Manager — Operational Overview (the "at a glance" screen)

**Goal:** in one screen, a department line manager sees **current status**, **recent operations**, **team activity**, and **what needs attention** — and can act, without opening reports or analytics.

### 5.1 Sections (top → bottom)

1. **Header** — greeting, department, shift window, live clock, 🔔.
2. **Current status** — department-specific status tiles (live counts), e.g.:
   - *Housekeeping mgr:* Dirty / Cleaning / Clean / Inspected / OOO, "Rooms remaining", "Checkout-priority left".
   - *Front-Office mgr:* Arrivals done/pending, Departures pending, Occupancy, Rooms ready.
   - *Engineering mgr:* Open WOs, Overdue, In-progress, OOO rooms.
   - *F&B mgr:* Outlets open, Open orders, Covers so far.
3. **Recent operations** — live activity feed (timeline): "R508 marked clean · Rahim · 9:20", "WO #A-412 completed", "Mr Khan checked in", "Deposit ৳9,800 collected". Newest first.
4. **Needs attention** — exceptions: SLA breaches, overdue tasks, escalations, **approvals pending**.
5. **Team status** — staff on shift: On task / On break / Idle, with workload balance; tap to reassign.
6. **Quick actions** — Assign task · Reassign · Broadcast to team.

> **Not on this screen:** RevPAR/ADR/occupancy *analytics*, trend charts, reports, exports. This is live operational state, not business intelligence.

### 5.2 Wireframe (Housekeeping line manager example)

```
┌───────────────────────────┐
│ Housekeeping · AM shift 🔔•│
│ Nadia · 08:00–16:00       │
├───────────────────────────┤
│ Current status            │
│ ┌────┐┌────┐┌────┐┌────┐  │
│ │Dty ││Cln ││Insp││OOO │  │
│ │ 18 ││ 42 ││ 30 ││ 3  │  │
│ └────┘└────┘└────┘└────┘  │
│ 22 rooms remaining · 8 pri│
├───────────────────────────┤
│ Recent operations         │
│ • R508 clean · Rahim 9:20 │
│ • R214 inspected    9:18  │
│ • R412 issue → Eng  9:05  │
├───────────────────────────┤
│ ⚠ Needs attention (3)     │
│ • R305 overdue clean 40m  │
│ • Swap approval: Karim    │
├───────────────────────────┤
│ Team (6 on shift)         │
│ ● Rahim task ● Sara break │
│ ● Karim idle  ○ …         │
│ [ Assign ][ Reassign ][📢]│
├───────────────────────────┤
│ 🏠   ✅   💬   ⋯           │
└───────────────────────────┘
```

### 5.3 Stitch prompt

> Design a mobile **operational overview / at-a-glance screen** for a hotel **department line manager** (this is live operational monitoring, NOT a financial KPI or analytics dashboard — no RevPAR/ADR charts, no reports). Top app bar: department + shift ("Housekeeping · AM shift"), manager name and hours, notification bell with a red dot. Section 1 **Current status**: a row of 4 compact status tiles with big counts and color dots — "Dirty 18" (amber), "Clean 42" (green), "Inspected 30" (blue), "OOO 3" (red) — with a caption "22 rooms remaining · 8 checkout-priority". Section 2 **Recent operations**: a live activity feed, newest first, each row a small icon + text + staff + time ("R508 clean · Rahim · 9:20"). Section 3 **Needs attention**: an amber/red exceptions card listing overdue tasks and pending approvals with a count badge. Section 4 **Team**: staff chips showing status (On task / On break / Idle) with colored dots. Bottom: a **Quick actions** row — "Assign", "Reassign", broadcast icon. Persistent bottom tab bar (Home active). Clean, operational, scannable, calm; status colors do the work.

### 5.4 Department variants (same screen, different tiles/feed)
- **Front Office:** status = Arrivals/Departures/Occupancy/Rooms-ready; feed = check-ins, payments, room-ready; attention = VIP arrivals, unassigned rooms.
- **Engineering:** status = Open/Overdue/In-progress WOs, OOO rooms; feed = logged/assigned/completed WOs; attention = overdue/critical WOs.
- **F&B:** status = Outlets open / Open orders / Covers; feed = orders fired/paid; attention = delayed orders, voids needing approval.

---

## 6. Implementation notes

- **Server-side authorization.** Role gating is enforced on the API, not just hidden in the UI — a hidden tab must also be a forbidden endpoint. The client hides; the server denies.
- **One build, role-driven layout.** Tab set, landing tab, and Home variant are resolved from the signed-in role + department at login; no separate app per role.
- **Department parameter.** Line-manager screens take a `department` context; the Overview template renders the department's tiles/feed/exceptions.
- **Role changes / multi-role.** A user may hold more than one role (e.g., supervisor who also works the desk); the union of permissions applies, with a role switcher if needed.
- **Escalation path.** Frontline → Line Manager (department) → Duty/GM (property). Exceptions and approvals route up this chain via F5/F6.
- **New features feed existing screens.** The manager Overview's **Team status** is driven by F9 clock-in/attendance (on-shift/break/idle); **Needs attention** absorbs F12 low-stock alerts, F13 failed inspections, and F14 negative-feedback signals; F9 shift-swap and roster-change approvals join the approvals queue.
- **F11 is analytics, not operations.** The owner KPI view is the *only* business-intelligence surface and is walled off from the operational Overview; it reads aggregated data and never issues task actions.

---

## 7. Open questions

- Confirm the department set for line managers (HK, Front Office, Engineering, F&B — others?).
- Can a single user hold multiple roles, and is a role switcher needed?
- Should the Overview activity feed be department-only, or include cross-department items a manager subscribes to?
- Approval types in scope for line managers (task reassignment, shift swap (F9), escalations)?
- Does the **Owner (F11-only)** role need above-property/multi-property KPI roll-up, or single-property for v1?
- Is **geofenced clock-in (F9)** acceptable to staff/labour agreements, and what geofence radius/grace applies?
- Who **receives** Staff SOS (F16) alerts — security, duty manager, or both — and is there an escalation timeout?
