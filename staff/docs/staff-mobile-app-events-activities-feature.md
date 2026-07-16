# Events, Activities & Banquet Management — Feature Deep-Dive

**Product:** Atrium Staff (Hotel / Resort Operations Mobile App)
**Feature area:** F18 — Events, Activities & Banquet Management
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-15
**Status:** Draft v1.0
**Related:** [Staff Mobile App — Feature Scope](staff-mobile-app-feature-scope.md) · [Task Management & Communication — Deep-Dive](staff-mobile-app-task-management-communication-feature.md) · [POS — Deep-Dive](staff-mobile-app-pos-multi-outlet-feature.md) · [Labor Management — Deep-Dive](staff-mobile-app-labor-management-feature.md) · [Reservations — Deep-Dive](staff-mobile-app-reservations-management-feature.md) · [Competitive Feature Research](../HOTEL-~1.MD)

---

## 1. Executive Summary

**Events, Activities & Banquet Management** gives a property a structured way to plan and run the things that don't fit a room-night: weddings, conferences, parties and banquets, and resort activities (excursions, classes, kids' club, tours). Today these are coordinated over WhatsApp, printed function sheets, and a spreadsheet the coordinator keeps on a laptop — so setup tasks get dropped, the bar goes unstaffed, and the AV nobody booked shows up as a scramble an hour before guests arrive.

F18 turns an event into a **first-class object**: create the event or activity, build its **run-of-show** (the timeline of setup → service → teardown), reserve the spaces and resources it needs, attach its F&B, and — the part that makes it run — **assign a crew and give each person a custom, event-specific work role** (Event Captain, Setup Lead, Bar Lead, AV, Registration/Host, Activity Guide, Safety Marshal, Runner). Each role carries its own task checklist, so every crew member opens their phone and sees exactly their duties, in order, for this event — without changing their normal department or permissions.

Events and activities are **high-margin ancillary revenue** that most local properties coordinate by hand. Vendors treat this as its own discipline — Oracle OPERA Sales & Event Management and Tripleseat / Event Temple (banquet event orders and function sheets), ResortSuite and activity-booking tools like FareHarbor / Xola (activity slots and guest sign-ups), and role-based staff scheduling (7shifts, HotSchedules). Atrium brings the operational core of all three onto the same mobile app the crew already uses — deliberately short of a full sales CRM.

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What staff can do from the device |
|---|------------|-----------------------------------|
| 1 | **Event / activity creation** | Create an event or activity as its own task type — set type, date/time window, venue/space, expected pax, host/organizer, and link it to a reservation or group block. |
| 2 | **Run-of-show / function sheet** | Build the timeline of segments (setup, registration, service courses, entertainment, teardown) with times, spaces, and resources — the mobile banquet event order (BEO). |
| 3 | **Crew assignment + custom event roles** | Assign staff to the event and give each a **custom, event-scoped role** (Captain, Setup, Bar, AV, Host, Guide, Marshal, Runner) that carries its own duties — an overlay that does **not** change their base department/RBAC. |
| 4 | **Role-based task checklists** | Each crew member sees only their role's tasks for the event in their own [F5](staff-mobile-app-task-management-communication-feature.md) Tasks, with times, dependencies, and photo/checklist steps. |
| 5 | **Space & resource booking** | Reserve function rooms, equipment, and AV against an event; detect conflicts with other events and room blocks (ties to [F2](staff-mobile-app-reservations-management-feature.md)). |
| 6 | **Activity scheduling & sign-ups** | For resort activities: recurring slots with capacity, guide assignment, and guest sign-up charged to the folio via [F8](staff-mobile-app-pos-multi-outlet-feature.md). |
| 7 | **Live event board** | An event-day view: crew checked-in ([F9](staff-mobile-app-labor-management-feature.md)), segment progress, open tasks, and issues — with on-the-fly reassignment. |
| 8 | **Headcount, charges & summary** | Crew assignment **writes to the [F9](staff-mobile-app-labor-management-feature.md) roster** and counts toward labor cost; F&B and AV charges (F8) roll to the event's quote/folio; produce a post-event summary. |

### 2.2 In scope (this release)
Event and activity creation as a task type; run-of-show / function-sheet builder; **custom event-role definitions** (per property) with default per-role task checklists; crew assignment with the event-role overlay; role-based task lists surfaced in F5; space/resource/AV booking with conflict detection against the reservations/space calendar; activity slots with capacity, guide assignment, and guest sign-up charged to folio (F8); a live event-day board with reassignment; **crew assignment written back to the F9 roster** (counts toward labor cost — an event within a person's existing shift adds no extra hours, while hours beyond their roster or on a day off are added and flagged as extra/overtime); F&B/AV charge roll-up (F8) to an event quote/folio; a basic event quote; post-event summary; offline access to the run-of-show and role checklists; base RBAC plus the event-role overlay.

### 2.3 Out of scope (this release)
A full **catering & events sales CRM** — lead pipeline, e-proposals, contracts, e-signature, and commission tracking — is **not** in F18; it holds a basic quote and hands richer sales to an existing S&C/CRM where one exists (see §9). Also out: public ticketing with a card-payment gateway beyond folio charge / pay-by-link; full AV/production and vendor-management; a Gantt/project-management engine (F18 stays at run-of-show depth); and menu/recipe authoring (owned by F8). Multi-day mega-conference agenda apps for attendees are out — F18 is the **staff-side** operations tool, not a guest event app.

### 2.4 Key assumptions
- Function spaces and bookable resources can be set up per property during onboarding (a one-time effort).
- Custom event roles and their default checklists can be configured per property and reused across events.
- F8 POS can attach F&B/AV charges to an event or folio; F9 can roster staff to an event window; F2 exposes a space/room calendar for conflict checks.

---

## 3. Capability Deep-Dive — The Events & Activities Features

Each capability is elaborated — **what it is**, **how it works in Atrium**, **why it matters operationally**, and the **revenue/cost angle** — with vendor precedents noting the bar to clear.

### 3.1 Event / activity creation as a task type
*Precedent: Oracle OPERA S&E, Tripleseat, Event Temple (event/function records).*

**What it is.** Creating an event or activity as its own structured object — not a generic task and not a room booking — with the fields an event actually needs: type, date/time window, space, expected pax, host/organizer, and a link to the reservation or group block behind it.

**How it works in Atrium.** A coordinator or manager taps "New event", picks a type (wedding, conference, party, banquet, excursion, class, kids' club, tour), sets the window and space, and links the group block or host contact. The event becomes the container everything else hangs off — run-of-show, crew, resources, charges.

**Why it matters.** Coordinating an event as a pile of loose WhatsApp messages loses the thread; a single event object gives everyone one source of truth for what's happening, when, where, and who owns it.

**Revenue / cost angle.** A captured, structured event pipeline makes ancillary bookings repeatable and visible (**revenue**) and removes the coordinator's manual re-keying across chats and spreadsheets (cost).

### 3.2 Run-of-show / function sheet (mobile BEO)
*Precedent: banquet event orders (BEOs) in OPERA / Tripleseat / Planning Pod.*

**What it is.** The timeline of an event — setup, registration, courses, entertainment, teardown — each segment with a time, a space, and the resources it needs. The mobile equivalent of the printed banquet event order.

**How it works in Atrium.** The planner adds segments in order; each segment can carry resources (tables, AV, décor), an F&B menu line, and notes. The run-of-show drives the crew's task timing: a "setup" segment at 14:00 generates the setup tasks due before it. Crew see the same timeline, read-only, so everyone works to one clock.

**Why it matters.** The expensive failures in events are timing failures — the room not turned in time, the meal served late, the teardown that runs into the next booking. A shared run-of-show makes the timeline explicit and keeps a multi-department crew synchronized.

**Revenue / cost angle.** Tighter turns let a space host more events per day (**revenue**); fewer timing failures cut comps, overtime, and the reputation damage that loses repeat event business (cost/revenue).

### 3.3 Crew assignment with custom event roles
*Precedent: role-based staff scheduling (7shifts, HotSchedules); banquet captain/section staffing models.*

**What it is.** Assigning staff to an event and giving each one a **custom, event-scoped work role** — Event Captain, Setup Lead, Service, Bar Lead, AV/Tech, Registration/Host, Activity Guide, Safety Marshal, Runner — configurable per property. The role defines the person's **duties for that event only**; it is an **overlay** that does not change their base department or app permissions.

**How it works in Atrium.** A manager opens the event's crew panel, adds people (often drawn from several departments), and assigns each an event role from the property's configurable list. Each role has a **default task checklist**, so assigning "Bar Lead" instantly gives that person the bar setup/stock/service/close tasks. A person keeps their normal RBAC (a housekeeper stays a housekeeper in the rest of the app) but, *inside this event*, they act in their assigned role. One person can hold different roles on different events.

**Why it matters.** Event crews are temporary teams pulled from across the property for one night; without clear, event-specific roles, "who's doing the bar?" is answered at 6pm in a panic. Custom roles make accountability crisp for a one-off crew and let the same staff flex into whatever the event needs — without an admin rebuilding permissions each time.

**Revenue / cost angle.** Clear roles mean the event is fully and correctly staffed with fewer people standing idle or duplicated (cost); a well-run, well-staffed event protects the premium guests paid for and drives repeat/referral bookings (**revenue**).

### 3.4 Role-based task checklists
*Precedent: task/checklist assignment (F5); event captain's checklist practice.*

**What it is.** Each crew member's event duties, delivered as a personal, time-ordered checklist scoped to their event role — surfaced in the same [F5 Tasks](staff-mobile-app-task-management-communication-feature.md) list they already use.

**How it works in Atrium.** When a role is assigned, its default checklist instantiates as tasks tied to the event's run-of-show timing, and a planner can add event-specific one-offs. The Bar Lead sees "stock the bar by 17:30 · set glassware · open bar 18:00 · last call 22:30 · reconcile & close"; the Setup Lead sees the room-turn steps. Tasks carry photos/checklists and mark complete like any other F5 task; the event board rolls them up.

**Why it matters.** A crew member shouldn't have to read the whole function sheet to find their three jobs. Role-scoped checklists put exactly their duties in front of them, in order, and make completion visible to the captain.

**Revenue / cost angle.** Nothing gets dropped because everything is assigned and tracked (cost/quality); managers spend the event supervising exceptions, not chasing status (cost).

### 3.5 Space & resource booking with conflict detection
*Precedent: function-space diaries in OPERA S&E; resource booking in Planning Pod.*

**What it is.** Reserving the function rooms, equipment, and AV an event needs, and catching clashes — two events in the same ballroom, or AV double-booked — before they happen.

**How it works in Atrium.** Each event books its space and resources for its window; the app checks them against other events and against the [F2](staff-mobile-app-reservations-management-feature.md) room/space calendar and flags conflicts. Setup and teardown buffers are part of the booking so back-to-back events don't collide.

**Why it matters.** A double-booked ballroom is a public failure with a paying host in the room. Conflict detection moves that discovery to planning time, when it's a reschedule instead of a disaster.

**Revenue / cost angle.** Conflict-free scheduling lets spaces be sold to capacity with confidence (**revenue**) and avoids the comps and refunds a clash forces (cost).

### 3.6 Activity scheduling, capacity & guest sign-ups
*Precedent: ResortSuite activities; FareHarbor / Xola (slots, capacity, bookings).*

**What it is.** For resort activities — snorkeling trips, yoga classes, kids' club, guided tours — recurring time slots with a capacity, an assigned guide/instructor, and guest sign-up that charges the folio.

**How it works in Atrium.** A manager defines an activity and its recurring slots with capacity and a guide; front desk or the guest signs guests into a slot, and the fee posts to the folio via [F8](staff-mobile-app-pos-multi-outlet-feature.md). The guide sees their roster and headcount for the slot; capacity and safety limits are enforced.

**Why it matters.** Activities are recurring ancillary revenue that's usually run on a clipboard at the pool desk; structured slots capture the fee, prevent over-booking a boat, and make sure a guide is actually assigned.

**Revenue / cost angle.** Captured activity fees and upsells are direct **ancillary revenue**; enforced capacity and rostered guides prevent both lost sales (slot looked full but wasn't) and safety/over-capacity incidents (cost/liability).

### 3.7 Crew hours feed the roster & labor cost (F9 write-back)
*Precedent: banquet-labor budgeting; unified staff scheduling (7shifts, HotSchedules).*

**What it is.** When a manager staffs an event, those assignments are **not** kept in an island — they are written back to [F9 Labor Management](staff-mobile-app-labor-management-feature.md) as scheduled hours, so event crews appear on the roster and their hours count toward labor cost.

**How it works in Atrium.** Publishing an event's crew (S38) writes each person's event hours into F9 for the event window. Because an event usually happens *during* a person's normal shift, an overlapping assignment is **not** a conflict — it is simply what they are doing that shift, and adds no extra hours or cost. Only hours that fall **outside** their rostered shift — extending past shift end, or on a day off — are added to the roster and flagged to the manager as **extra/overtime** before publish. Event labor then flows into the same timesheets, labor-cost %, and the owner KPI view (F11) as every other shift — one source of truth for who is working and what it costs. Base RBAC is unchanged; only the *scheduling* is shared (the event-role overlay from §3.3 stays event-scoped).

**Why it matters.** An event staffed "on the side" is invisible labor: people get double-booked, event overtime hides from the cost number, and the owner's labor-cost % understates reality. Writing crews back to the roster closes that gap and keeps the property's single biggest controllable cost honest.

**Revenue / cost angle.** Visible event labor lets managers right-size event crews and catch overtime before it happens (cost); accurate labor-cost reporting to ownership (F11) makes event profitability real rather than assumed (cost/visibility).

---

## 4. Why This Matters

### 4.1 Events and activities are high-margin revenue run by hand
Banquets, parties, and activities carry strong margins but are coordinated on chat and paper. Structuring them captures revenue that leaks and removes the manual coordination tax.

### 4.2 Timing and staffing are where events fail
The room not turned in time and the bar left unstaffed are the classic event failures — both are timing and role-assignment problems that a shared run-of-show and clear crew roles solve directly.

### 4.3 Custom roles fit how event crews actually form
Event crews are one-night teams pulled from every department. Event-scoped roles give accountability without permanently changing anyone's job or permissions — the model matches reality.

### 4.4 A competitive opening in the BD market
Few local properties have mobile banquet/activity operations tied into F&B, labor, and the space calendar. Atrium can own the operational core — while leaving the heavy sales CRM to specialists.

---

## 5. Operations It Improves

| Operation today | Pain | With Events & Activities |
|-----------------|------|---------------------------|
| **Planning an event** | Spreadsheet + WhatsApp; no single source | One event object with run-of-show, crew, resources, charges. |
| **Staffing the crew** | "Who's on the bar?" decided last-minute | Assign crew + **custom event roles** with default checklists. |
| **Crew knowing their jobs** | Read the whole function sheet | Each sees only their role's timed tasks in F5. |
| **Booking spaces / AV** | Clashes found on the day | Conflict detection at planning time against the space calendar. |
| **Running activities** | Clipboard at the pool desk | Slots with capacity, assigned guide, folio-charged sign-ups. |
| **Event day** | Manager chases status by radio | Live board: crew checked-in, segment progress, open tasks. |
| **After the event** | Charges reconciled from memory | Headcount + F&B/AV roll to the quote; post-event summary. |

---

## 6. How It Protects & Increases Revenue

1. **Captured event & banquet business.** A structured, repeatable event operation lets a property sell and deliver more functions with confidence.
2. **F&B and AV attach.** Event F&B, bar, and AV charges roll up cleanly, so nothing served goes unbilled.
3. **Activity fees & upsells.** Slot-based sign-ups charged to the folio capture recurring activity revenue that clipboards miss.
4. **Repeat & referral bookings.** A visibly well-run, well-staffed event is what wins the next wedding or corporate booking.
5. **Spaces sold to capacity.** Conflict-free scheduling lets function rooms be booked back-to-back without fear of a clash.

> **Illustrative model:** If structured events add even 2 extra banquets/month at ৳150,000 revenue each, and activity sign-ups capture ৳40,000/month in previously-uncharged fees, that is ~৳4.1M/year of ancillary revenue. *(Figures illustrative; validate against property data.)*

---

## 7. How It Reduces Operational Cost

1. **Less coordination labor.** One event object replaces the coordinator's manual re-keying across chats, calls, and spreadsheets.
2. **Right-sized crews.** Role-based staffing means events are covered without over-calling staff "just in case" — cutting event overtime.
3. **Fewer timing failures.** A shared run-of-show cuts the late-turn overtime, comps, and rework that timing slips cause.
4. **Fewer clashes and over-bookings.** Conflict detection and enforced activity capacity avoid the comps, refunds, and incidents that cost real money.

> **Illustrative model:** If role-based staffing trims 8 avoidable crew-hours per large event and the property runs ~12 such events/month, that is ~1,150 crew-hours/year saved — before counting comps and clash refunds avoided. *(Figures illustrative; validate against property data.)*

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Capture event business | Events/activities run through the app | ↑ measurable |
| Staff events correctly | Events fully crewed & role-assigned before start | ≥ 95% |
| Hit the timeline | Segments completed on schedule | ≥ 90% |
| Capture activity revenue | Activity sign-ups charged to folio | ↑ measurable |
| Avoid clashes | Space/resource double-bookings | ≈ 0 |
| Control event labor | Event overtime hours / event | ↓ measurable |
| Bill what's delivered | Event F&B/AV charges reconciled | ≥ 98% |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| Space/resource setup | Function spaces and resources must be configured | One-time onboarding; prioritize by most-used spaces. |
| Role library setup | Custom event roles + checklists need defining | Ship starter roles (Captain/Setup/Bar/AV/Host/Guide); let property tune. |
| F8 charge tie-in | Event F&B/AV and activity fees post via POS/folio | Start with manual charge lines; add automated roll-up when POS exposes it. |
| F9 roster write-back | Event crews must reflect in the roster and labor cost | Crew assignment writes to the F9 roster; an event within a person's shift adds no extra hours, only hours beyond their roster are added and flagged as extra/overtime before publish. |
| Space calendar source | Conflict check needs a space/room calendar | Use F2 calendar; treat spaces as bookable inventory. |
| Scope creep to sales CRM | Pull toward proposals/contracts/pipeline | Hold at quote + operations; integrate an S&C/CRM (§2.3). |
| Offline on event day | Venues may have poor coverage | Cache run-of-show and role checklists; queue completions. |

---

## 10. Scope delivered

Atrium Staff ships as one complete package. This feature delivers:

- Event & activity creation as a dedicated task type
- Run-of-show / function-sheet builder (mobile BEO)
- Configurable **custom event roles** with default per-role checklists
- Crew assignment with the event-role overlay (no change to base RBAC)
- Role-scoped, time-ordered task lists surfaced in F5
- Space / resource / AV booking with conflict detection against the space calendar (F2)
- Activity slots with capacity, guide assignment, and folio-charged guest sign-ups (F8)
- Live event-day board with on-the-fly reassignment
- Crew assignment writes back to the F9 roster and counts toward labor cost (hours within a person's shift add nothing; only extra/overtime hours are flagged)
- F&B/AV charge roll-up (F8) to an event quote/folio
- Basic event quote and post-event summary
- Offline access to run-of-show and role checklists
- RBAC base + event-role overlay

---

## 11. Open Questions

- Which event types lead at launch — weddings/banquets, corporate/conference, or resort activities?
- Is a basic in-app quote enough for v1, or must F18 integrate with an existing sales & catering / CRM system from day one?
- How are custom event roles governed — a fixed property library, or can a manager create ad-hoc roles per event?
- Do event fees and activity sign-ups always post to a folio, or are walk-in cash/gateway payments needed at launch?
- **Resolved — crew assignment writes back to the F9 roster and counts toward labor cost; an event during a person's normal shift is not a conflict, and only hours beyond their roster (or on a day off) count as extra/overtime.** Open follow-on: how are event **overtime** and **cross-department** hours attributed for costing?
- How do function spaces model against the room/space calendar — as inventory in F2, or a separate space diary?

---

## 12. One-Line Business Case

> **Events, Activities & Banquet Management turns weddings, parties, banquets, and resort activities into structured, staffed, on-time operations — create the event, build its run-of-show, and assign a crew with custom event roles that each get their own checklist — capturing high-margin ancillary revenue while cutting the last-minute coordination and staffing chaos that events run on today.**
