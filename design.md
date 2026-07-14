# Atrium Staff — UI Design Specification (for Stitch)

**Product:** Atrium Staff — Hotel / Resort Operations Mobile App
**Owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-14
**Purpose:** A complete, Stitch-ready design spec to generate the full mobile app UI — design system + screen-by-screen prompts.
**Context docs:** [Navigation & Feature Map](docs/staff-mobile-app-navigation-map.md) · [Feature Deep-Dives Index](docs/staff-mobile-app-feature-deep-dives-index.md) · [Executive Summary](docs/staff-mobile-app-executive-summary.md)

---

## 0. How to use this with Stitch

1. **Set the global style first.** Paste **§1–§4** (product, design language, tokens, components) into Stitch as the project/theme context so every screen shares one look.
2. **Generate screen-by-screen.** Each screen in **§5** has a copy-paste **"Stitch prompt"** block plus a component checklist and sample content. Generate one screen at a time for best fidelity.
3. **Platform:** Mobile, portrait, iOS + Android (Material 3 / iOS HIG hybrid). Target 390×844 pt.
4. **Theme:** Generate **light and dark** variants. Colors below define both.
5. **Consistency anchors:** persistent **bottom tab bar** (§4.2), **top app bar** (§4.1), **8pt spacing grid**, **12px card radius**, **Inter** typeface.

**Scope note (removed features):** F7 dashboards/reporting/analytics, automated dynamic pricing, spa/golf/retail POS, preventive-maintenance & asset history, multi-property/cluster, lost & found, shift/attendance, and concierge tasks are **out of scope** — the screens below reflect this. **Kept:** F1 Front Desk, F2 Reservations, F3 Housekeeping, F5 Task & Comms, F6 Notifications, F8 core F&B POS, F9 manual rates + channel sync, F4 reactive work orders.

---

## 1. Product & design principles

Atrium Staff is used by frontline hotel staff — front desk, housekeeping, maintenance, F&B, supervisors, managers — often **on the move, one-handed, sometimes gloved, in variable lighting**. The UI must be:

- **Glanceable** — key status visible in under a second; big numbers, clear color states.
- **Thumb-first** — primary actions in the lower half; large (≥48px) tap targets.
- **Calm & professional** — hospitality-grade, uncluttered, confident; not consumer-flashy.
- **Status-driven** — color-coded states (room status, SLA, priority) do the heavy lifting.
- **Resilient** — clear offline/sync indicators; nothing looks broken without a connection.
- **Role-aware** — each role sees a focused subset; never overwhelm.
- **Localization-ready** — Bangla/English, ৳ (BDT) currency, longer strings must not break layout.

**Design keywords for Stitch:** *clean, modern, professional, hospitality operations dashboard, spacious, rounded cards, soft shadows, high-contrast status colors, data-dense but calm.*

---

## 2. Design language / visual style

- **Overall:** Modern operational dashboard. Card-based. Generous white space. Soft elevation (not flat, not skeuomorphic).
- **Corners:** Cards 12px, buttons 10px, chips/pills fully rounded, sheets 20px top radius.
- **Elevation:** subtle shadows (y2 blur8 8% black) on cards; sticky bars use a hairline top border + blur.
- **Iconography:** rounded line icons (Lucide/Material Symbols Rounded), 24px, 2px stroke.
- **Imagery:** minimal; use avatars (guest initials), room-type thumbnails optional.
- **Motion (note for build):** 150–200ms ease; optimistic UI on task/status actions.
- **Tone of text:** short, operational, action-first ("Check in", "Mark clean", "Post charge").

---

## 3. Design tokens

### 3.1 Color — brand & UI

| Token | Light | Dark | Use |
|-------|-------|------|-----|
| `brand/primary` | `#1D4ED8` | `#3B82F6` | Primary actions, active tab, links |
| `brand/primary-ink` | `#0F2547` | `#E8EEFB` | Headlines, brand surfaces |
| `accent/upsell` | `#F59E0B` | `#FBBF24` | Upsell/upgrade prompts, highlights |
| `success` | `#10B981` | `#34D399` | Ready/clean, completed, paid |
| `warning` | `#F59E0B` | `#FBBF24` | Dirty, due-soon, attention |
| `danger` | `#EF4444` | `#F87171` | Overdue, OOO, errors, escalation |
| `info` | `#3B82F6` | `#60A5FA` | Inspected, informational |
| `bg` | `#F6F8FB` | `#0B1220` | App background |
| `surface` | `#FFFFFF` | `#111827` | Cards, sheets |
| `surface-2` | `#F1F5F9` | `#1E293B` | Inset rows, chips |
| `text/primary` | `#0F172A` | `#F1F5F9` | Primary text |
| `text/muted` | `#64748B` | `#94A3B8` | Secondary text, labels |
| `border` | `#E2E8F0` | `#243244` | Dividers, card borders |

### 3.2 Color — room / task status (color-coded chips & dots)

| Status | Color | Label |
|--------|-------|-------|
| Clean / Ready | `#10B981` (green) | Clean |
| Dirty | `#F59E0B` (amber) | Dirty |
| Inspected | `#3B82F6` (blue) | Inspected |
| Occupied | `#6366F1` (indigo) | Occupied |
| Vacant | `#94A3B8` (slate) | Vacant |
| Out of Order | `#EF4444` (red) | OOO |
| Do Not Disturb | `#8B5CF6` (violet) | DND |
| In Progress | `#0EA5E9` (sky) | Cleaning |

### 3.3 Typography (Inter)

| Style | Size / Weight | Use |
|-------|---------------|-----|
| Display | 28 / 700 | Big KPI numbers |
| H1 | 22 / 700 | Screen titles |
| H2 | 18 / 600 | Section headers |
| Body | 15 / 400 | Default text |
| Body-strong | 15 / 600 | Emphasis, names |
| Caption | 13 / 500 | Labels, metadata |
| Micro | 11 / 600 | Chip/badge text, uppercase tracked |

### 3.4 Spacing & layout

- **Grid:** 8pt base. Screen padding 16px. Card padding 16px. Gap between cards 12px.
- **Tap target:** min 48×48px. List row height 64–72px.
- **Bottom tab bar:** 64px + safe area. **Top app bar:** 56px.

---

## 4. Global / shared components

### 4.1 Top app bar
- Left: screen title (H1) or back chevron + title.
- Right: 🔔 notification bell (with red dot badge when unread) + optional overflow/filter icon.
- Property name shown under the title on Home (single-property scope; multi-property switching is out of scope).
- Offline banner: thin amber strip below app bar — "Offline · 3 changes queued · syncing when back online."

### 4.2 Bottom tab bar (persistent, role-based)
5 items, icon + label, active item in `brand/primary`:
`🏠 Home` · `✅ Tasks` · `🛎️ Desk` · `💬 Inbox` · `⋯ More`
- Badge counts on Tasks (open) and Inbox (unread).
- Some roles show fewer tabs (see role matrix in the [nav map](docs/staff-mobile-app-navigation-map.md)).

### 4.3 Core components
- **Stat tile / KPI card:** big number (Display), label (Caption), delta chip (▲/▼ with success/danger color), optional sparkline.
- **List row:** leading avatar/room-dot, title (Body-strong), subtitle (Caption/muted), trailing status chip + chevron.
- **Status chip:** pill, colored dot + label, uses §3.2 colors.
- **Priority badge:** Low/Med/High/Urgent — grey/blue/amber/red.
- **SLA countdown:** small clock + time remaining; turns amber <15min, red when breached.
- **Primary button:** full-width, `brand/primary`, white text, 52px, 10px radius. **Secondary:** outline. **Destructive:** danger.
- **FAB:** bottom-right, `brand/primary`, "+" — for create task/reservation/order.
- **Segmented control / tabs:** for Arrivals/Departures/In-house, Today/Upcoming/Overdue.
- **Bottom sheet:** for actions, filters, quick post-charge.
- **Search bar:** rounded, `surface-2`, leading search icon.
- **Empty / loading / offline states:** friendly illustration or icon + one line + action.

---

## 5. Screen specifications

> Each screen: **Feature mapping → Stitch prompt (copy-paste) → Component checklist → Sample content.**
> Screens are grouped by tab. Generate individually.

---

### AUTH

#### S1 — Login / Sign in
**Feature:** Auth/RBAC (foundation)

**Wireframe:**
```
┌───────────────────────────┐
│                           │
│            ◆ A            │
│       Atrium Staff        │
│        Operations         │
│  ┌─────────────────────┐  │
│  │ Email               │  │
│  ├─────────────────────┤  │
│  │ Password        👁   │  │
│  ├─────────────────────┤  │
│  │ Property         ▾  │  │
│  ├─────────────────────┤  │
│  │   [   Sign in   ]   │  │
│  └─────────────────────┘  │
│    Use PIN / Face ID      │
│    Forgot password?       │
│          v1.0.0           │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a clean, professional mobile **login screen** for a hotel staff operations app called "Atrium Staff". Centered layout on a soft off-white background (dark: near-black). Top: app logo mark (a simple rounded "A" monogram in deep blue) and the wordmark "Atrium Staff", with a small tagline "Operations". Middle: a white rounded card (16px radius, soft shadow) containing an email field, a password field with show/hide, a "Property" dropdown, and a full-width deep-blue "Sign in" button (52px). Below: a "Use PIN / Face ID" secondary text button and a small "Forgot password?" link. Footer: version number in muted text. Generous spacing, Inter font, large tap targets.

**Checklist:** logo/wordmark · email · password (show/hide) · property selector · primary Sign in · biometric/PIN option · forgot link · light+dark.
**Sample:** Property: "Atrium Bay Resort, Cox's Bazar".

#### S2 — Quick unlock (PIN / biometric)

**Wireframe:**
```
┌───────────────────────────┐
│                           │
│    Welcome back, Rahim    │
│           ( ⚷ )           │
│         Face ID           │
│                           │
│       ● ● ● ○ ○ ○         │
│         1   2   3         │
│         4   5   6         │
│         7   8   9         │
│             0   ⌫         │
│   Use password instead    │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **quick-unlock screen**: greeting "Welcome back, Rahim", a large Face ID / fingerprint icon, and a 6-dot PIN entry with a numeric keypad below. Minimal, centered, deep-blue accents. "Use password instead" text link at bottom.

---

### HOME (🏠) — Today launcher (app shell)

> Note: the former KPI/analytics dashboards and reports (**F7**) were removed from scope. Home is now a lightweight **operational launcher** — no RevPAR/ADR/occupancy analytics, no charts, no reports.

#### S3 — Home / Today launcher
**Feature:** App shell (operational launcher — not analytics)

**Wireframe:**
```
┌───────────────────────────┐
│ Good morning, Rahim    🔔•│
│ Atrium Bay Resort         │
├───────────────────────────┤
│ ┌────────┐ ┌────────┐     │
│ │Arrivals│ │Departs │     │
│ │  46    │ │  39    │     │
│ └────────┘ └────────┘     │
│ ┌────────┐ ┌────────┐     │
│ │My tasks│ │Dirty   │     │
│ │  7     │ │rms 18  │     │
│ └────────┘ └────────┘     │
│ Alerts                    │
│ ★ VIP arrival 3PM Mr Khan │
│ ⚠ SLA breach R412 maint   │
│ Quick actions             │
│ [ Check in ][ New task ]  │
├───────────────────────────┤
│ 🏠   ✅   🛎   💬   ⋯      │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **home / today launcher** for a hotel staff operations app (this is NOT a KPI or analytics dashboard). Top app bar "Good morning, Rahim" with the property name beneath and a notification bell with a red dot. Below, a 2×2 grid of **operational count tiles**, each a big number + label and tappable to its screen: "Arrivals 46", "Departures 39", "My tasks 7", "Dirty rooms 18". Then an **Alerts** list (2 rows: "VIP arrival 3:00 PM — Mr. Khan", "SLA breach: Room 412 maintenance"). Then a **Quick actions** row of two big buttons: "Check in" and "New task". Bottom: persistent 5-tab bar (Home active). Clean cards, operational, calm — no financial KPIs, no charts, no sparklines.

**Checklist:** greeting + property + bell · 2×2 operational count tiles (arrivals/departures/my tasks/dirty rooms) · alerts list · quick-actions row · tab bar. NO financial KPIs/charts/reports.
**Sample:** counts only; ৳ currency not shown on this screen.

---

### TASKS (✅) — F3 Housekeeping · F4 Maintenance · F5 Task Mgmt

#### S5 — My Tasks list
**Feature:** F5 Task Management

**Wireframe:**
```
┌───────────────────────────┐
│ Tasks                  🔔 │
│ [Today] Upcoming Overdue•3│
├───────────────────────────┤
│ ┌───────────────────────┐ │
│ │🟢 Deliver towels      │ │
│ │ R214·Request ⏱12m  Hi │ │
│ ├───────────────────────┤ │
│ │🟠 Fix AC        [📷]  │ │
│ │ R412  ⏱ overdue   URG │ │
│ ├───────────────────────┤ │
│ │🔵 Clean checkout      │ │
│ │ R508  Med             │ │
│ └───────────────────────┘ │
│                      (＋) │
├───────────────────────────┤
│ 🏠   ✅•7  🛎   💬   ⋯     │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **task list screen** titled "Tasks". A segmented control at top: "Today · Upcoming · Overdue" (Today active, Overdue shows a red count badge). Below, a scrollable list of **task cards**, each with: a leading colored category icon (cleaning=green, maintenance=orange, guest request=blue), task title in bold ("Deliver extra towels"), a subtitle "Room 214 · Guest request", a **priority badge** (High=amber), and an **SLA countdown** chip ("12 min left", amber). Some cards show a small photo thumbnail. A floating "+" button bottom-right to create a task. Pull-to-refresh. Persistent tab bar (Tasks active with a badge "7"). Clean cards, status colors, big tap targets.

**Checklist:** segmented Today/Upcoming/Overdue · task cards (icon, title, location, priority, SLA) · FAB · tab badge.
**Sample rows:** "Deliver extra towels · Room 214 · High · 12 min"; "Fix AC · Room 412 · Urgent · overdue (red)"; "Clean checkout · Room 508 · Med".

#### S6 — Task detail

**Wireframe:**
```
┌───────────────────────────┐
│ ‹ Task #4821              │
├───────────────────────────┤
│ Fix AC not cooling        │
│ [In progress] [URGENT]    │
│ ⏱ Overdue by 8 min        │
│ ┌───────────────────────┐ │
│ │📍 Room 412 · Floor 4  │ │
│ │ Guest: Mr Khan ★      │ │
│ └───────────────────────┘ │
│ Description...            │
│ Reported: Front Desk 9:14 │
│ Checklist                 │
│  ☑ Inspect unit           │
│  ☐ Replace filter         │
│ Photos [＋][img][img]     │
│ Notes __________________  │
├───────────────────────────┤
│ [ Reassign ][ Complete ]  │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **task detail screen**. Top app bar with back + title "Task #4821". Header block: task title "Fix AC not cooling", status chip "In progress" (sky), priority badge "Urgent" (red), SLA "Overdue by 8 min" (red). A **location card**: "Room 412 · Floor 4", with a small map/room icon and linked guest "Mr. A. Khan (VIP)". A **details** section: description text, reported by "Front Desk · 9:14 AM". A **checklist** of steps with checkboxes. A **photos** row (add photo tile + 2 thumbnails). A **notes** input. Bottom sticky action bar: secondary "Reassign" + primary "Complete". 

**Checklist:** header status/priority/SLA · location + linked guest · description + reporter · checklist · photos · notes · sticky actions.

#### S7 — Housekeeping room board
**Feature:** F3 Housekeeping

**Wireframe:**
```
┌───────────────────────────┐
│ Rooms          🔍   ⚙     │
│ All Dirty18 Clean Insp OOO│
├───────────────────────────┤
│ ┌────┐ ┌────┐ ┌────┐      │
│ │201 │ │202 │ │203 │      │
│ │🟢Cln│ │🟠Dty│ │🔵Ins│    │
│ └────┘ └────┘ └────┘      │
│ ┌────┐ ┌────┐ ┌────┐      │
│ │204 │ │205 │ │206 │      │
│ │🟣DND│ │🟠Dty│ │🔴OOO│    │
│ └────┘ └────┘ └────┘      │
│ ┌────┐ ┌────┐ ┌────┐      │
│ │207 │ │208 │ │209 │      │
│ │⬜Vac│ │🟦Cln│ │🟠Chk│    │
│ └────┘ └────┘ └────┘      │
├───────────────────────────┤
│ 🏠   ✅   🛎   💬   ⋯      │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **housekeeping room board**. Top app bar "Rooms" with a filter icon and a search field. A row of **status filter chips**: All, Dirty (amber), Clean (green), Inspected (blue), OOO (red), DND (violet) — each with a count. Below, a **grid of room tiles** (3 per row): each tile shows the room number big ("214"), a colored status dot/background matching its state, a tiny icon for occupied/vacant, and a small label ("Dirty", "Checkout"). Dirty and checkout-priority rooms stand out. Optionally a list-view toggle. Persistent tab bar. Clean, color-forward, scannable grid.

**Checklist:** search · status filter chips with counts · room grid tiles (number, status color, occ/vac, priority) · list toggle.
**Sample:** rooms 201–512, mix of statuses; "18 Dirty · 8 Checkout priority".

#### S8 — Room detail + cleaning checklist

**Wireframe:**
```
┌───────────────────────────┐
│ ‹ Room 214                │
│ [ Dirty → cleaning ]      │
├───────────────────────────┤
│ Checkout clean            │
│ Occupied until 11:00      │
│ Guest: Ms R Das           │
│ ✎ feather-free, high floor│
│ Checklist                 │
│  ☐ Bathroom        [📷]   │
│  ☐ Bed linen              │
│  ☐ Minibar restock        │
│  ☐ Vacuum                 │
│ ＋ Log minibar            │
├───────────────────────────┤
│ [Report issue][Mark Clean]│
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **room detail screen** for housekeeping. Top: "Room 214" with a large status chip "Dirty → cleaning". Info row: "Checkout clean · Occupied until 11:00 · Guest: Ms. R. Das". A **guest preferences** note ("feather-free pillows, high floor"). A **cleaning checklist** card: rows like "Bathroom", "Bed linen", "Minibar restock", "Vacuum" with checkboxes; some require a camera icon for photo evidence. A **minibar** quick-log row ("+ Log consumption"). Bottom sticky bar: "Mark Clean" primary button (green) and "Report issue" secondary (creates a maintenance work order). Show a DND state variant note.

**Checklist:** status · guest/occupancy · preferences · checklist with photo-required steps · minibar log · Mark Clean + Report issue.

#### S9 — Maintenance work orders list
**Feature:** F4 Maintenance

**Wireframe:**
```
┌───────────────────────────┐
│ Maintenance            🔔 │
│ [Open] In progress  Done  │
│ HVAC · Plumbing · Elec    │
├───────────────────────────┤
│ ┌───────────────────────┐ │
│ │🟠 AC not cooling [📷]  │ │
│ │ R412  URG  ⏱ 42m      │ │
│ ├───────────────────────┤ │
│ │🔵 Leaking tap         │ │
│ │ R305  Med  ⏱ 1h10     │ │
│ └───────────────────────┘ │
│                      (＋) │
├───────────────────────────┤
│ 🏠   ✅   🛎   💬   ⋯      │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **work orders list** titled "Maintenance". Segmented control "Open · In progress · Done". Filter chips by type (HVAC, Plumbing, Electrical). List of **work-order cards**: title ("AC not cooling"), location ("Room 412"), priority badge, status chip, small photo thumbnail, and elapsed/SLA time. FAB "+" to log a new work order. Tab bar.

**Checklist:** segmented Open/In-progress/Done · type filter chips · WO cards (title, location, priority, status, photo, time) · FAB. *(Preventive maintenance & asset history are out of scope.)*

#### S10 — Work order detail

**Wireframe:**
```
┌───────────────────────────┐
│ ‹ WO #A-412               │
│ AC not cooling            │
│ [In progress] [URGENT]    │
├───────────────────────────┤
│ 📍 Room 412               │
│ Description...            │
│ Timeline                  │
│  • Logged 9:14 FrontDesk  │
│  • Assigned 9:20          │
│ Photos [before][after][＋]│
│ Parts/Time                │
│  Capacitor ×1 · 45 min    │
├───────────────────────────┤
│ [ Assign ][ Complete ]    │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **work order detail**: header title + status + priority. Location + linked room. Description, reporter, timeline of status changes. Before/after **photo** grid. Parts & time log rows. Bottom sticky: "Assign" + "Complete & return to service". *(No asset-history / preventive-maintenance section — out of scope.)*

---

### DESK (🛎️) — F1 Front Desk · F2 Reservations

#### S11 — Arrivals / Departures / In-house
**Feature:** F1 Front Desk

**Wireframe:**
```
┌───────────────────────────┐
│ Desk                   🔔 │
│ [Arrivals46] Dep In-house │
│ 🔍 Search guest/res       │
├───────────────────────────┤
│ ┌───────────────────────┐ │
│ │(AK) Mr Khan ★ Gold    │ │
│ │ R305 Deluxe 2n 3:00PM │ │
│ │ 🟢Ready     [Check in]│ │
│ ├───────────────────────┤ │
│ │(RD) Ms Das            │ │
│ │ Deluxe  ETA 5PM       │ │
│ │ 🟠Not ready [Check in]│ │
│ └───────────────────────┘ │
│                      (＋) │
├───────────────────────────┤
│ 🏠   ✅   🛎   💬   ⋯      │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **front desk screen** titled "Desk". A segmented control "Arrivals · Departures · In-house" (Arrivals active, badge "46"). A search bar "Search guest or reservation". List of **guest rows**: leading avatar with initials, guest name (bold) + VIP gold star if VIP, subtitle "Room 305 · Deluxe · 2 nights · ETA 3:00 PM", a status chip ("Room ready" green / "Not ready" amber), and a trailing "Check in" pill button. A loyalty tier chip on some rows. FAB "+" for walk-in / new reservation. Tab bar (Desk active). Clean, list-dense, quick-action forward.

**Checklist:** segmented Arrivals/Departures/In-house with counts · search · guest rows (avatar, VIP, room/rate/ETA, ready status, Check-in action, loyalty) · FAB.
**Sample:** "Mr. A. Khan ★ · Suite · Room ready · Check in"; "Ms. R. Das · Deluxe · ETA 5 PM · Not ready".

#### S12 — Guest / reservation detail

**Wireframe:**
```
┌───────────────────────────┐
│ ‹ Reservation             │
│ (AK) Mr Ariful Khan ★     │
│ Gold        📞   💬       │
├───────────────────────────┤
│ ┌───────────────────────┐ │
│ │ Room 712 · Suite      │ │
│ │ 14→17 Jul · 3 nights  │ │
│ │ ৳29,400               │ │
│ └───────────────────────┘ │
│ Preferences               │
│ [High floor][Late][Feath] │
│ Folio  Balance ৳12,300  › │
├───────────────────────────┤
│ [Assign][Upgrade][Check in]│
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **guest reservation detail**. Header: avatar, name "Mr. Ariful Khan", VIP star, loyalty "Gold", contact icons (call/message). **Stay card:** Room 712 (Suite), check-in 14 Jul, check-out 17 Jul, 3 nights, ৳29,400. **Preferences & notes** section (chips: "High floor", "Late arrival", "Feather-free"). **Folio summary** row "Balance ৳12,300 →". Actions: primary "Check in", secondary "Assign room", "Upgrade". Tabs or sections for Messages and History.

**Checklist:** guest header (VIP/loyalty/contact) · stay card · preferences/notes · folio summary · check-in/assign/upgrade actions.

#### S13 — Mobile check-in flow (stepper)

**Wireframe:**
```
┌───────────────────────────┐
│ ‹ Check in                │
│ ①Verify ②Room ③Upsell ④Key│
├───────────────────────────┤
│ ┌───────────────────────┐ │
│ │🟨 Upgrade Sea-View    │ │
│ │   Suite +৳2,000/night │ │
│ │ [img]   [Skip][Accept]│ │
│ └───────────────────────┘ │
│ Add-ons                   │
│  Early check-in  ৳1,500 ⃝ │
│  Breakfast    ৳900/pp   ⃝ │
│  Airport pickup ৳1,200  ⃝ │
│                           │
│ Total add-ons: ৳3,500     │
├───────────────────────────┤
│ [       Continue        ] │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **check-in flow screen** with a top progress stepper (4 steps: Verify · Room · Upsell · Key). Show the **Upsell step**: an **upgrade offer card** highlighted with the amber accent — "Upgrade to Sea-View Suite +৳2,000/night" with a room thumbnail, "Accept" and "Skip" buttons. Below, add-on toggles: "Early check-in ৳1,500", "Breakfast ৳900/pp", "Airport pickup ৳1,200". A running total chip at the bottom "Total add-ons: ৳3,500". Sticky "Continue" button. Make the upsell visually appealing and tappable.

**Checklist:** 4-step progress stepper · highlighted upgrade offer (amber) · add-on toggles with prices · running total · Continue.

#### S14 — Folio & payment
**Feature:** F1 Front Desk (payments)

**Wireframe:**
```
┌───────────────────────────┐
│ ‹ Folio R712 · Mr Khan    │
├───────────────────────────┤
│ Room (3n)        ৳29,400  │
│ Restaurant        ৳3,200  │
│ Spa               ৳4,500  │
│ Minibar             ৳800  │
│ ＋ Post charge            │
│ ┌───────────────────────┐ │
│ │ Subtotal    ৳37,900   │ │
│ │ VAT          ৳7,040   │ │
│ │ Balance due ৳44,940   │ │
│ └───────────────────────┘ │
│ Pay:[Card][bKash][Nagad]  │
│     [SSLComm][Cash]       │
├───────────────────────────┤
│ [ Take payment ৳44,940 ]  │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **folio & payment screen**. Header "Folio · Room 712 · Mr. A. Khan". A **charges list**: Room (3 nights) ৳29,400, Restaurant ৳3,200, Spa ৳4,500, Minibar ৳800, with a "Post charge +" row. **Totals card:** Subtotal, VAT, Total ৳44,940, Balance due ৳44,940 (bold). **Payment method** selector as chips with logos: Card, bKash, Nagad, SSLCommerz, Cash. A big primary "Take payment ৳44,940" button. On success: a green check state + "Email receipt" toggle. Clean, financial, trustworthy.

**Checklist:** charges list + post-charge · totals with VAT · payment method chips (Card/bKash/Nagad/SSLCommerz/Cash) · Take payment button · success/receipt state.

#### S15 — Reservations calendar (tape chart)
**Feature:** F2 Reservations

**Wireframe:**
```
┌───────────────────────────┐
│ Calendar ◂ Jul 2026 ▸ Tdy │
├───────────────────────────┤
│Rm │14 15 16 17 18 19 20   │
│───┼───────────────────────│
│305│▓▓Khan▓▓░              │
│306│   ▓▓▓Das▓▓▓           │
│307│▨▨block▨▨              │
│308│        ██OOO██        │
│309│  ▓▓▓Rahman▓▓▓         │
│Legend ▓Conf ▓In ▨Blk █OOO │
│                      (＋) │
├───────────────────────────┤
│ 🏠   ✅   🛎   💬   ⋯      │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **reservations calendar / tape chart**. A horizontal scrollable grid: room numbers down the left sticky column, dates across the top. Reservations shown as **colored horizontal bars** spanning nights, color-coded by status (confirmed=blue, in-house=indigo, blocked=violet, OOO=red). Bars show guest name truncated. A date navigator at top ("July 2026 ◂ ▸") and a "Today" pill. Tap-and-drag hint. A legend chip row. FAB "+" to create a booking. Keep it readable on a phone — clear grid lines, rounded bars.

**Checklist:** sticky room column · date axis + navigator · colored reservation bars (status legend) · Today pill · FAB · drag hint.

#### S16 — Create / edit reservation

**Wireframe:**
```
┌───────────────────────────┐
│ ‹ New reservation         │
├───────────────────────────┤
│ Guest 🔍 _______________  │
│ Contact ________________  │
│ In 14 Jul  Out 17 Jul (3n)│
│ Room type ▾ Deluxe (12 av)│
│ Rate ৳9,800  editable     │
│ Adults [-2+]  Child [-0+] │
│ Source ▾ Direct           │
│ Notes __________________  │
│ ┌───────────────────────┐ │
│ │ Deposit  Send Pay-link⃝│ │
│ │ Amount ৳9,800         │ │
│ └───────────────────────┘ │
│ 3 nights · ৳29,400        │
├───────────────────────────┤
│ [   Create reservation  ] │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **create reservation form**. Fields: Guest (name + search existing), Contact, Dates (check-in/out date pickers showing nights), Room type dropdown with live availability count, Rate (auto-filled, editable), Occupancy (adults/children steppers), Source/channel dropdown, Notes. A **deposit** section with "Send Pay-by-Link" toggle and amount. A summary bar "3 nights · ৳29,400". Sticky "Create reservation" primary button. Clean form, grouped sections, clear labels.

**Checklist:** guest + search · date range + nights · room type w/ availability · editable rate · occupancy steppers · source · notes · pay-by-link deposit · summary · Create.

---

### INBOX (💬) — F5 Comms · F6 Notifications

#### S17 — Messages (list + chat)
**Feature:** F5 Internal Communication

**Wireframe:**
```
┌───────────────────────────┐
│ Inbox                     │
│ [Chats] Notifications     │
├───────────────────────────┤
│ 📣 Manager: VIP group 3PM │
│ ┌───────────────────────┐ │
│ │[HK] Housekeeping   •3 │ │
│ │ "R508 ready"    9:20  │ │
│ ├───────────────────────┤ │
│ │(N) Nadia FrontDesk    │ │
│ │ "on it"         9:12  │ │
│ └───────────────────────┘ │
│ ── chat ──                │
│ ░ incoming       [R412]   │
│          outgoing ▓       │
│ [＋] message _______ [➤]  │
├───────────────────────────┤
│ 🏠   ✅   🛎   💬•3  ⋯     │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **staff messaging screen** titled "Inbox". Top tabs: "Chats · Notifications". Chats list: rows with avatar (or group icon), name/department ("Housekeeping Team", "Front Desk – Nadia"), last message preview, timestamp, unread count badge. A broadcast/announcement pinned at top with a megaphone icon ("Manager: VIP group arriving 3 PM — all hands"). FAB "+" for new message. Then show a **chat detail** variant: message bubbles (incoming grey left, outgoing blue right), a message that has an attached **room reference card** ("Room 412 · AC issue"), and an input bar with attach (photo/task/room) + send. Tab bar.

**Checklist:** Chats/Notifications tabs · chat rows (avatar, dept, preview, unread) · pinned broadcast · FAB · chat detail with bubbles + attached room/task card + input.

#### S18 — Notifications center
**Feature:** F6 Notifications

**Wireframe:**
```
┌───────────────────────────┐
│ Notifications          ⚙  │
│ ‼ Critical                │
│ 🔴 SLA breach R412   View │
├───────────────────────────┤
│ Just now                  │
│▎🔵 New booking OTA   9:41 │
│  🟢 Msg from HK      9:38 │
│ Earlier today             │
│  🔴 Cancellation     8:10 │
│  ★ VIP arrival 3PM        │
│               Mark all ✓  │
├───────────────────────────┤
│ 🏠   ✅   🛎   💬   ⋯      │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **notifications center**. Title "Notifications" with a filter icon (by type/property). Grouped by time: "Just now / Earlier today / Yesterday". Rows: a colored type icon (booking=blue, cancellation=red, message=green, SLA=amber, VIP=gold), bold title, subtitle, timestamp; unread rows have a left accent bar and tinted background. Some rows have a quick action ("View", "Assign"). A small "Critical" section at top for DND-bypass alerts (red). "Mark all read" text button. Tab bar.

**Checklist:** filter · time groups · type-colored rows (read/unread states) · critical section · quick actions · mark-all-read.

---

### MORE (⋯) — F8 POS (F&B) · F9 Rates · Settings

#### S19 — More menu (hub)

**Wireframe:**
```
┌───────────────────────────┐
│ (N) Nadia Islam           │
│ Duty Manager · Atrium Bay │
├───────────────────────────┤
│ REVENUE                   │
│  💳 POS & Outlets       › │
│  📈 Rates & Channels    › │
│ OPERATIONS                │
│  👤 Guest Profiles      › │
│ ACCOUNT                   │
│  ⚙ Settings            › │
├───────────────────────────┤
│ 🏠   ✅   🛎   💬   ⋯      │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **"More" menu hub**. A list of large tappable rows with leading icons and chevrons: "POS & Outlets", "Rates & Channels", "Guest Profiles", "Settings". Group into sections ("Revenue", "Operations", "Account"). A profile header at top: avatar, name "Nadia Islam", role "Duty Manager", property. Tab bar (More active). *(Reports & Exports, Lost & Found, and Shift & Attendance were removed from scope.)*

**Checklist:** profile header · grouped menu rows with icons/chevrons.

#### S20 — POS — outlet order (tableside)
**Feature:** F8 POS — F&B core *(spa/golf/retail out of scope)*

**Wireframe:**
```
┌───────────────────────────┐
│ Poolside Grill ▾          │
│ [Table] Room  Takeaway    │
│ Starters Mains Drinks Dsrt│
├───────────────────────────┤
│ ┌─────┐ ┌─────┐ ┌─────┐   │
│ │Fish │ │Prawn│ │Rice │   │
│ │৳650+│ │৳900+│ │৳250+│   │
│ └─────┘ └─────┘ └─────┘   │
│ ══ Order · Table 7 ══     │
│ Grilled Fish  ×1   ৳650   │
│ 🟨 Add fries ৳250?        │
│ Subtotal          ৳650    │
│ [Charge room][Take pay]   │
├───────────────────────────┤
│ 🏠   ✅   🛎   💬   ⋯      │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **POS ordering screen** for a resort restaurant. Top: outlet selector "Poolside Grill ▾" and a mode segmented control "Table · Room · Takeaway". A **menu grid**: category chips (Starters, Mains, Drinks, Desserts) and product cards with name, price (৳), and "+" add. A running **order panel** as a bottom sheet: line items with qty steppers, subtotal, an **upsell suggestion** chip ("Add fries ৳250?"), and two buttons: "Charge to room" and "Take payment". Table/room number field. Vibrant but clean, food-app energy but professional.

**Checklist:** outlet selector · Table/Room/Takeaway mode · category chips · product cards (+add) · order sheet (qty, subtotal, upsell) · Charge-to-room + Take payment.

#### S21 — Revenue management — rate calendar
**Feature:** F9 — manual rate management + channel sync *(automated dynamic pricing out of scope)*

**Wireframe:**
```
┌───────────────────────────┐
│ ‹ Rates & Channels        │
│ RevPAR৳8040 Pace▲12 Pk+14 │
├───────────────────────────┤
│ Jul    M  T  W  T  F      │
│ ┌────┐ ┌────┐ ┌────┐      │
│ │14🟢│ │15🟠│ │16🔴│      │
│ │9.8k│ │10k │ │11k │      │
│ │82% │ │88% │ │95% │      │
│ └────┘ └────┘ └────┘      │
│ Comp set median ৳10,200   │
│ (tap day → edit sheet)    │
│  ⤒ rate · min-stay · stop │
├───────────────────────────┤
│ 🏠   ✅   🛎   💬   ⋯      │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **rate & channel management screen**. Top strip: "RevPAR ৳8,040", "Pace ▲12% vs LY", "Pickup +14 rooms". A **rate calendar**: a month grid where each day cell shows the date, the current rate ("৳9,800"), a tiny demand indicator (green/amber/red dot), and occupancy %. Tapping a day opens a **bottom sheet** to **manually** edit rate, min-stay, and stop-sell, with a "Sync to channels" note (Booking.com, Expedia, Website logos). A market-intelligence card "Comp set median ৳10,200". Professional, data-forward, confident. (Manual rate control only — no automated dynamic-pricing engine.)

**Checklist:** RevPAR/pace/pickup strip · rate calendar (rate, demand dot, occ%) · manual edit sheet (rate/min-stay/stop-sell) · channel sync · comp-set card. *(No automated dynamic-pricing toggle.)*

#### S23 — Profile & settings

**Wireframe:**
```
┌───────────────────────────┐
│ ‹ Settings                │
│ (N) Nadia · Duty Manager  │
├───────────────────────────┤
│ ACCOUNT                   │
│  Edit profile          › │
│  Change PIN            › │
│  Biometric        [ON]  │
│ NOTIFICATIONS             │
│  Bookings         [ON]  │
│  Quiet hours 22:00–06:00 │
│  Critical bypass DND  🔒 │
│ APP                       │
│  Language  EN / বাংলা ▾  │
│  Theme  System ▾         │
│  Offline data     [ON]  │
│ [       Sign out       ] │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **settings screen**. Profile header (avatar, name, role, property). Sections: "Account" (edit profile, change PIN, biometric toggle), "Notifications" (per-type toggles, quiet hours time pickers, a "Critical alerts bypass DND" locked/info row), "App" (language: English/বাংলা selector, theme: System/Light/Dark, offline data toggle), "Support" (help, about, version), and a red "Sign out" button. Clean grouped list with toggles.

**Checklist:** profile · account · notification prefs + quiet hours · language (EN/বাংলা) + theme · offline · support · sign out.

---

## 6. Screen ↔ feature index (for reference)

| Screen | Tab | Feature |
|--------|-----|---------|
| S1–S2 Login / Unlock | — | Auth / RBAC |
| S3 Home / Today launcher | 🏠 Home | App shell |
| S5–S6 Tasks & detail | ✅ Tasks | F5 |
| S7–S8 Housekeeping board & room | ✅ Tasks | F3 |
| S9–S10 Work orders | ✅ Tasks | F4 (reactive) |
| S11–S12 Desk list & guest detail | 🛎️ Desk | F1 |
| S13 Check-in / upsell | 🛎️ Desk | F1 |
| S14 Folio & payment | 🛎️ Desk | F1 |
| S15–S16 Reservations calendar & form | 🛎️ Desk | F2 |
| S17 Messaging | 💬 Inbox | F5 |
| S18 Notifications | 💬 Inbox | F6 |
| S19 More hub | ⋯ More | — |
| S20 POS (F&B) | ⋯ More | F8 |
| S21 Rates & channels | ⋯ More | F9 (manual) |
| S23 Settings | ⋯ More | — |

*Retired when F7 dashboards/reporting was removed from scope: **S4** (extra home dashboard variant) and **S22** (Reports). Screen IDs are kept stable; numbering intentionally skips them.*

---

## 7. Global states to generate for key screens

For each primary screen, also generate:
- **Empty state** (e.g., "No tasks — you're all caught up").
- **Offline state** (amber banner + queued-changes indicator).
- **Loading skeleton** (shimmer cards/rows).
- **Error state** (retry).
- **Dark mode** variant.

---

## 8. Do / Don't for Stitch generations

**Do:** big glanceable numbers · color-coded status · thumb-reachable primary actions · rounded cards · realistic hotel sample data · ৳ currency · light+dark.
**Don't:** tiny tap targets · cramped tables · consumer-flashy gradients · more than one primary action per screen · decorative imagery that hides data.
