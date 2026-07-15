# Atrium Staff — UI Design Specification (for Stitch)

**Product:** Atrium Staff — Hotel / Resort Operations Mobile App
**Owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-15
**Purpose:** A complete, Stitch-ready design spec to generate the full mobile app UI — design system + screen-by-screen prompts.
**Start here:** [Project Overview & Documentation Index](overview.md)
**Context docs:** [Navigation & Feature Map](docs/staff-mobile-app-navigation-map.md) · [Role-Based Design](docs/staff-mobile-app-role-based-design.md) · [Executive Summary](docs/staff-mobile-app-executive-summary.md)

---

## 0. How to use this with Stitch

1. **Set the global style first.** Paste **§1–§4** (product, design language, tokens, components) into Stitch as the project/theme context so every screen shares one look.
2. **Generate screen-by-screen.** Each screen in **§5** has a copy-paste **"Stitch prompt"** block plus a component checklist and sample content. Generate one screen at a time for best fidelity.
3. **Platform:** Mobile, portrait, iOS + Android (Material 3 / iOS HIG hybrid). Target 390×844 pt.
4. **Theme:** Generate **light and dark** variants. Colors below define both.
5. **Consistency anchors:** persistent **bottom tab bar** (§4.2), **top app bar** (§4.1), **8pt spacing grid**, **12px card radius**, **Inter** typeface.

**In scope:** the full feature set, one consistent design system.
- **Guest-facing operating loop:** F1 Front Desk, F2 Reservations, F3 Housekeeping, F4 Maintenance (work orders), F5 Task & Comms, F6 Notifications, F8 F&B POS.
- **Cost, control & assurance layer (F9–F16):** F9 Labor Management, F10 Preventive Maintenance & Assets, F11 Management Analytics / Owner KPIs, F12 Inventory & Par-Stock, F13 Inspections & Compliance, F14 Guest Feedback & Recovery, F15 SOP / Training, F16 Lost & Found & Staff Safety.
- **Cross-cutting enabler:** F17 NFC & Barcode Scanning — a scan/capture sheet surfaced *inside* POS (S20), Inventory (S29), Assets (S27), rooms, and Desk; never its own tab.
- **Resort ancillary:** F18 Events, Activities & Banquet Management — event/activity creation, run-of-show, and crew assignment with **custom event roles** (in ⋯ More; crew tasks surface in ✅ Tasks).
- Plus a Home operational launcher, role-based access, and the global Notifications / Offline / Staff-SOS layers.

> **Consistency rule:** every screen — including the F9–F18 screens (§5, S25–S38) — reuses the SAME tokens (§3), components (§4), tab bar, spacing, and status colors. New feature domains map onto the existing palette via the **extended status map (§3.5)**; they do not introduce new colors, radii, or type styles.

---

## 1. Product & design principles

Atrium Staff is used by frontline hotel staff — front desk, housekeeping, maintenance, F&B, supervisors, managers, and (for KPIs only) owners — often **on the move, one-handed, sometimes gloved, in variable lighting**. The UI must be:

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

### 3.2b Status colors — extended features (F9–F18)

The new feature domains **reuse the same semantic palette** (§3.1 / §3.2) — no new hues. This table fixes the mapping so the same state reads the same color everywhere.

| Domain (screen) | State | Token / color | Label |
|-----------------|-------|---------------|-------|
| **Shift / attendance (F9)** | On shift · clocked in | `success` green | On task |
| | On break | `warning` amber | Break |
| | Idle / unassigned | `text/muted` slate | Idle |
| | Off / not scheduled | `border`/muted | OFF |
| | On property (geofence ok) | `success` green | 📍 On property |
| | Off property (geofence fail) | `warning` amber | Not on property |
| **Roster coverage (F9)** | Meets demand | `success` green | Covered |
| | Below suggested staffing | `warning` amber | Understaffed |
| **Preventive maint. (F10)** | PM due soon | `warning` amber | Due |
| | PM overdue | `danger` red | Overdue |
| | Asset in service | `success` green | In service |
| **Inventory par (F12)** | At/above par | `success` green | Healthy |
| | Near par | `warning` amber | Low |
| | Below par | `danger` red | Reorder |
| **Inspection item (F13)** | Pass | `success` green | ✓ |
| | Fail | `danger` red | ✗ |
| | Pending | `text/muted` slate | ○ |
| **Guest feedback (F14)** | Positive (≥4★) | `success` green | — |
| | Neutral (3★) | `warning` amber | — |
| | Negative (≤2★) | `danger` red | Recover |
| **Lost & Found (F16)** | Open / Unclaimed | `warning` amber | Open |
| | Claimed / returned | `success` green | Claimed ✓ |
| | Disposed / expired | `text/muted` slate | Disposed |
| **Staff SOS (F16)** | Panic action / alert | `danger` red | SOS |
| **KPI delta (F11)** | Improving | `success` green | ▲ |
| | Worsening | `danger` red | ▼ |
| **Event crew (F18)** | Fully crewed | `success` green | Covered |
| | Understaffed / overtime | `warning` amber | Short |
| **Scan (F17)** | Resolved · not-found | `success` · `warning` | reuses existing hues — no new colors |

> **Rule of thumb:** green = good/done/healthy · amber = attention/due/low · red = overdue/critical/reorder/fail · slate = idle/inactive/neutral. Applies identically across F1–F18.

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
- Property name shown under the title on Home (single-property scope).
- Offline banner: thin amber strip below app bar — "Offline · 3 changes queued · syncing when back online."

### 4.2 Bottom tab bar (persistent, role-based)
5 items, icon + label, active item in `brand/primary`:
`🏠 Home` · `✅ Tasks` · `🛎️ Desk` · `💬 Inbox` · `⋯ More`
- Badge counts on Tasks (open) and Inbox (unread).
- Some roles show fewer tabs (see role matrix in the [nav map](docs/staff-mobile-app-navigation-map.md)).
- **The 5-tab shell never grows.** F9–F18 do not add tabs — they live inside existing tabs: PM & inspections under ✅ Tasks; shifts, inventory, assets, KPIs, knowledge base, lost & found, and **events & activities (F18)** under ⋯ More; feedback under 💬 Inbox; **scanning (F17)** is surfaced *inside* existing screens; and **Staff SOS is a global layer** (available on every screen, not a tab), like Notifications and Offline.

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

**Shared components introduced by F9–F18 (reuse everywhere, same tokens):**
- **Progress bar:** thin rounded track (`surface-2`) + `brand/primary` fill + "8/10" label. Used by inspections (S30) and onboarding (S32).
- **Live-timer card:** big monospaced elapsed time (Display) + context line + one primary action; used by clock-in (S25). Same card radius/elevation as all cards.
- **Geofence chip:** a **status chip** variant — `success` "📍 On property" / `warning` "Not on property" (S25). Not a new component, just the status-chip pattern.
- **Schedule grid:** staff rows × day columns of tappable **shift chips** (fully-rounded pills, §3.2b colors) with a **coverage row** using `success`/`warning` (S26). Horizontally scrollable inside its own container; page never scrolls sideways.
- **Pass/Fail/Pending toggle:** three-state control — `success` ✓ / `danger` ✗ / muted ○ (S30). A failed item expands to reveal photo + "raise task" link.
- **Rating stars:** 1–5 ★ in `accent/upsell` amber; sentiment of the row still uses §3.2b feedback colors (S31).
- **Hold-to-confirm button:** large `danger` button with a circular 3-second hold progress ring, to prevent accidental triggers; used by Staff SOS (S34). The only place a destructive action is hold-gated rather than tapped.
- **KPI dashboard grid:** the existing **stat tile** (§4.3) in a 2-up grid + trend line + bar chart; the only analytics surface (F11, S28), GM/owner-only.
- **Scan / capture sheet (F17, S35):** a full-bleed camera viewfinder with a rounded reticle, torch toggle, and a mode row (Barcode · QR · NFC); a resolved-item confirmation slides up from the bottom as a bottom sheet. A small **scan icon** (📷/⌫ barcode glyph) sits inline on POS (S20), Inventory (S29), and Asset (S27) screens to invoke it. Reuses card radius, `brand/primary`, and status chips — no new tokens. Includes a **batch/continuous** state (running tally chip) and a **manual-entry fallback** link.
- **Run-of-show timeline + crew-role chips (F18, S36–S38):** a vertical, time-ordered **segment list** (setup → service → teardown) built from the standard list-row + SLA-clock + status-chip patterns, and a **crew list** of avatar rows each carrying a **custom event-role chip** (fully-rounded pill in `brand/primary` tint; a **coverage** summary uses `success`/`warning` like the roster grid §4.3). Assigning a role instantiates that role's checklist as F5 tasks. No new tokens, radii, or type styles.

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

### HOME (🏠) — Today launcher (app shell) · role-variant

> Home is a lightweight **operational launcher** — operational counts, alerts, and quick actions (no financial KPIs, charts, or reports).
>
> **Home is role-variant** (see [Role-Based Design](docs/staff-mobile-app-role-based-design.md)): **frontline** roles get the personal **launcher** (S3); **line managers / GM** get the **operational Overview** (S24 — current status, recent operations, team, exceptions).

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

#### S24 — Line Manager operational overview (manager Home variant)
**Feature:** Home variant for Line Manager / GM — operational monitoring. See [Role-Based Design](docs/staff-mobile-app-role-based-design.md).

**Wireframe:** (Housekeeping line manager example)
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

**Stitch prompt:**
> Design a mobile **operational overview / at-a-glance screen** for a hotel **department line manager** (this is live operational monitoring, NOT a financial KPI or analytics dashboard — no RevPAR/ADR charts, no reports). Top app bar: department + shift ("Housekeeping · AM shift"), manager name and hours, notification bell with a red dot. Section 1 **Current status**: a row of 4 compact status tiles with big counts and color dots — "Dirty 18" (amber), "Clean 42" (green), "Inspected 30" (blue), "OOO 3" (red) — with a caption "22 rooms remaining · 8 checkout-priority". Section 2 **Recent operations**: a live activity feed, newest first, each row a small icon + text + staff + time ("R508 clean · Rahim · 9:20"). Section 3 **Needs attention**: an amber/red exceptions card listing overdue tasks and pending approvals with a count badge. Section 4 **Team**: staff chips showing status (On task / On break / Idle) with colored dots. Bottom: a **Quick actions** row — "Assign", "Reassign", broadcast icon. Persistent bottom tab bar (Home active). Clean, operational, scannable, calm; status colors do the work.

**Checklist:** dept + shift header · current-status tiles (dept-specific) · recent-operations feed · needs-attention/approvals · team status chips · quick actions (assign/reassign/broadcast) · tab bar. NO financial analytics/charts/reports.
**Variants:** Front Office (arrivals/occupancy/ready + check-in/payment feed), Engineering (open/overdue WOs + WO feed), F&B (outlets/orders/covers + order feed).

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

**Checklist:** segmented Open/In-progress/Done · type filter chips · WO cards (title, location, priority, status, photo, time) · FAB.

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
> Design a mobile **work order detail**: header title + status + priority. Location + linked room. Description, reporter, timeline of status changes. Before/after **photo** grid. Parts & time log rows. Bottom sticky: "Assign" + "Complete & return to service".

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

### MORE (⋯) — F8 POS (F&B) · F9 Shifts · F10 Assets · F11 KPIs · F12 Inventory · F15 Knowledge · F16 Lost & Found · Settings

#### S19 — More menu (hub)

**Wireframe:**
```
┌───────────────────────────┐
│ (N) Nadia Islam           │
│ Duty Manager · Atrium Bay │
├───────────────────────────┤
│ REVENUE                   │
│  💳 POS & Outlets       › │
│  🎉 Events & Activities › │
│ MY WORK                   │
│  ⏱ My Shifts · Clock-in › │
│  📚 Knowledge & SOPs    › │
│ MANAGEMENT                │
│  🗓 Roster (dept)       › │
│  📦 Inventory & Par-Stock›│
│  🛠 Asset register       › │
│  📊 Performance / KPIs *›│
│ OPERATIONS                │
│  👤 Guest Profiles      › │
│  🧳 Lost & Found        › │
│ ACCOUNT                   │
│  ⚙ Settings            › │
├───────────────────────────┤
│ 🏠   ✅   🛎   💬   ⋯      │
└───────────────────────────┘
```
*\* Performance / KPIs (F11) shows only for GM/owner. Management rows show only for line managers (department-scoped). Rows the role can't access are invisible, not greyed.*

**Stitch prompt:**
> Design a mobile **"More" menu hub**. A list of large tappable rows with leading icons and chevrons, grouped into sections: "Revenue" (POS & Outlets, Events & Activities), "My Work" (My Shifts · Clock-in, Knowledge & SOPs), "Management" (Roster, Inventory & Par-Stock, Asset register, Performance / KPIs), "Operations" (Guest Profiles, Lost & Found), "Account" (Settings). A profile header at top: avatar, name "Nadia Islam", role "Duty Manager", property. Note that management/KPI rows are role-gated (shown only for managers/GM). Tab bar (More active).

**Checklist:** profile header · grouped menu rows with icons/chevrons · role-gated Management + KPIs sections (invisible when not permitted).

#### S20 — POS — outlet order (tableside)
**Feature:** F8 POS — F&B core

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

**Checklist:** outlet selector · Table/Room/Takeaway mode · category chips · product cards (+add) · **scan icon (F17) to add a product / open a table-room tab by barcode/QR** · order sheet (qty, subtotal, upsell) · Charge-to-room + Take payment.

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

### EXTENDED FEATURES (F9–F18) — cost, control, assurance & extensions

These screens implement the cost-and-control layer. They reuse the shared components (top bar, cards, status chips, bottom tab bar) and the same visual language.

#### S25 — My Shifts & Clock-in (F9 Labor Management · staff)
**Feature:** F9 — every role

**Wireframe:**
```
┌───────────────────────────┐
│ ‹ My Shifts               │
│ ┌───────────────────────┐ │
│ │ On shift · 03:12:44   │ │
│ │ AM · 08:00–16:00      │ │
│ │ [  ⏹ Clock out  ]     │ │
│ │ 📍 On property ✓      │ │
│ └───────────────────────┘ │
│ THIS WEEK                 │
│ Mon 08–16  ✓ worked 8.0h │
│ Tue 08–16  • today       │
│ Wed OFF                   │
│ Thu 14–22  ⇄ swap pending│
│ [ Request swap ]          │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **"My Shifts & Clock-in"** screen for a hotel staff member. A prominent **clock-in card**: big live timer "On shift · 03:12:44", current shift window "AM · 08:00–16:00", a large "Clock out" button, and a geofence status chip "📍 On property ✓" (green when inside the property geofence, amber "Not on property" when outside — clock-in disabled). Below, a **week list** of shifts: day, hours, and a status (worked with hours, today, OFF, or "swap pending ⇄"). A "Request swap" button. Calm, reassuring, big tap targets for gloved hands.

**Checklist:** live clock-in card · geofence status chip · clock in/out button · week shift list with statuses · request-swap.

#### S26 — Roster / Schedule builder (F9 · manager)
**Feature:** F9 — line manager / GM

**Wireframe:**
```
┌───────────────────────────┐
│ ‹ Roster · Housekeeping   │
│ Week of 14 Jul   Occ 82% ▴│
│ Suggested: 7 attendants   │
├───────────────────────────┤
│        Mon Tue Wed Thu Fri│
│ Rahim  ● ● ● OFF ●        │
│ Sara   ● ● OFF ● ●        │
│ Karim  AM AM PM PM ●      │
│ …                         │
│ Coverage  6  7  5⚠ 7  7   │
├───────────────────────────┤
│ [ Auto-fill ][ Publish ]  │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **roster / schedule-builder** screen for a hotel department manager (Housekeeping). Header: department, week selector, and a demand hint "Occupancy 82% ▴ · Suggested 7 attendants". A **grid**: staff rows × weekday columns, each cell a shift chip (●/AM/PM/OFF), tappable to edit. A **coverage row** at the bottom summing staff per day, with an amber warning where coverage is below the suggestion. Buttons: "Auto-fill" (demand-based) and "Publish". Dense but scannable; the understaffed day should visually stand out.

**Checklist:** week + occupancy hint · staff×day grid with editable shift chips · coverage row with understaffing warning · **event-sourced shifts flagged (from F18)** · auto-fill + publish.

#### S27 — Asset register & Preventive Maintenance (F10)
**Feature:** F10 — engineering / manager (PM tasks also appear in S9–S10 work orders)

**Wireframe:**
```
┌───────────────────────────┐
│ ‹ Asset · AC-Chiller #2   │
│ 🟢 In service · Roof plant│
│ Warranty to 2027-03       │
├───────────────────────────┤
│ PM SCHEDULE               │
│ Filter clean  every 30d   │
│  next: in 4d              │
│ Gas check     every 90d   │
│  ⚠ overdue 3d             │
│ [ + Log reading ]         │
├───────────────────────────┤
│ HISTORY                   │
│ • 12 Jun · filter · Kabir │
│ • 03 Apr · WO#E-118 fixed │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **asset detail + preventive-maintenance** screen. Header: asset name "AC-Chiller #2", status chip "In service" (green), location, warranty date. A **PM schedule** list: each recurring task with interval ("every 30d"), and next-due ("in 4d" green / "overdue 3d" amber-red). A "Log reading" button (for meter-based PM). A **service history** timeline: dated entries with technician or linked work-order number. Utilitarian, engineering feel; overdue items clearly flagged.

**Checklist:** asset header (status, location, warranty) · recurring PM list with next-due / overdue · log-reading · service-history timeline.

#### S28 — Performance / Owner KPIs (F11)
**Feature:** F11 — GM / owner only (this is the ONLY analytics surface)

**Wireframe:**
```
┌───────────────────────────┐
│ Performance · Atrium Bay  │
│ This month ▾    ⤓ export  │
├───────────────────────────┤
│ ┌────────┐ ┌────────┐     │
│ │Labor % │ │Occupancy│    │
│ │ 28.4%  │ │  82%    │    │
│ │ ▾ 1.6  │ │ ▴ 4     │    │
│ └────────┘ └────────┘     │
│ ┌────────┐ ┌────────┐     │
│ │Rooms/hr│ │Ancillary│    │
│ │ 2.7    │ │ ৳6.4L   │    │
│ └────────┘ └────────┘     │
│ Labor cost — 6-wk trend   │
│  ▁▂▃▂▂▁  (line)           │
│ Dept productivity  [bars] │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **owner / management KPI dashboard** (this one IS analytics — unlike the operational manager overview). Header: property, a date-range selector "This month ▾", and an export icon. A grid of **KPI tiles** with big numbers and up/down deltas: "Labor cost % 28.4% ▾1.6", "Occupancy 82% ▴4", "Rooms/attendant-hr 2.7", "Ancillary ৳6.4L". Below, a **labor-cost trend line chart** (6 weeks) and a **department-productivity bar chart**. Boardroom-clean, data-dense but calm, restrained color, one accent for positive/negative deltas. This is for ownership — credible and precise, not flashy.

**Checklist:** date-range + export · KPI tiles with deltas (labor %, occupancy, productivity, ancillary) · trend line · department bar chart · owner-grade restraint.

> **Design note:** per [Role-Based Design](docs/staff-mobile-app-role-based-design.md), this is the *only* screen that shows financial/BI analytics, and it is invisible to every role except GM/owner. The manager "Overview" (S24) stays operational.

#### S29 — Inventory & Par-Stock (F12)
**Feature:** F12 — HK / F&B (scoped) · manager

**Wireframe:**
```
┌───────────────────────────┐
│ ‹ Inventory · Linen       │
│ 🔎 search        [Count]  │
├───────────────────────────┤
│ Bath towel   par 200      │
│  on hand 142   🟢         │
│ Hand towel   par 200      │
│  on hand  38   🔴 reorder │
│ Bath robe    par 60       │
│  on hand  51   🟡         │
├───────────────────────────┤
│ ⚠ 1 item below par        │
│ [   Create reorder   ]    │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **inventory / par-stock** screen for a hotel store (Linen category selector at top). A **list of items**: name, par level, on-hand count, and a status dot — green (healthy), amber (near par), red ("reorder") when below par. A "Count" action to enter a stock count. A bottom summary "1 item below par" and a "Create reorder" button. Practical, storeroom-friendly, large numbers, clear red for reorder.

**Checklist:** category selector · item rows (par, on-hand, status dot) · below-par red flag · count entry · **scan icon (F17) to jump to an item / batch-count by barcode** · create-reorder.

#### S30 — Inspection / Compliance checklist (F13)
**Feature:** F13 — HK / manager (generalizes F3 QA)

**Wireframe:**
```
┌───────────────────────────┐
│ ‹ Fire safety round · 3F  │
│ Progress ▓▓▓▓░ 8/10       │
├───────────────────────────┤
│ ✓ Extinguisher gauge OK   │
│ ✓ Exit signs lit          │
│ ✗ Stairwell door blocked  │
│    📷 photo · raise task ›│
│ ○ Alarm panel clear       │
│ ○ Hose reel sealed        │
├───────────────────────────┤
│ Score 8/10 · 1 fail       │
│ [   Submit inspection   ] │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **inspection / compliance checklist** screen (example: "Fire safety round · 3rd floor"). A progress bar "8/10". A **checklist**: each item a pass (✓ green), fail (✗ red), or pending (○) toggle; a failed item expands to show a photo thumbnail and a "raise corrective task ›" link. Footer: score summary "8/10 · 1 fail" and a "Submit inspection" button. Clean, auditable, pass/fail color-driven; failures obviously actionable.

**Checklist:** inspection title · progress · pass/fail/pending items · fail → photo + raise-task · score summary · submit.

#### S31 — Guest Feedback & Service Recovery (F14)
**Feature:** F14 — front desk / manager

**Wireframe:**
```
┌───────────────────────────┐
│ ‹ Feedback                │
│ Today · avg 4.2★  3 alerts│
├───────────────────────────┤
│ 🔴 R512 · Mr Khan  2★     │
│ "AC noisy, slow service"  │
│  [ Recover › ]            │
│ 🟢 R208 · Ms Roy   5★     │
│ 🟡 Poolside        3★     │
├───────────────────────────┤
│ Recovery · R512           │
│ ○ Apologise ○ Comp ○ Fix  │
│ [ Assign to dept ]        │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **guest feedback & service-recovery** screen. Header: today's average rating "4.2★" and an alert count. A **feed of feedback**: each row a room/guest/outlet, a star rating, a short quote, color-coded by sentiment (red ≤2★, amber 3★, green ≥4★); low scores show a "Recover ›" button. A **recovery panel**: quick options (apologise, comp, fix) and "Assign to department". Empathetic but efficient; negative feedback clearly prioritized for fast recovery before checkout.

**Checklist:** avg-rating header · sentiment-colored feedback feed · low-score "Recover" CTA · recovery actions + assign.

#### S32 — Knowledge base / SOPs (F15)
**Feature:** F15 — all roles

**Wireframe:**
```
┌───────────────────────────┐
│ ‹ Knowledge & SOPs        │
│ 🔎 search how-to…         │
├───────────────────────────┤
│ FOR YOU (Housekeeping)    │
│  📄 Turndown standard   › │
│  📄 Spill / biohazard   › │
│  🎬 Bed-making (2m)     › │
│ ONBOARDING                │
│  ✓ Day-1 checklist  4/6   │
│ POPULAR                   │
│  📄 Lost & found policy › │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **knowledge-base / SOP** screen. A search bar "search how-to…". Sections: "For you" (role-relevant SOP articles and a short how-to video with duration), "Onboarding" (a checklist with progress "4/6"), and "Popular". Each row: an icon (doc/video), title, chevron. Clean, readable, learn-on-shift; content grouped by role relevance.

**Checklist:** search · role-relevant SOP list · video with duration · onboarding checklist progress · popular articles.

#### S33 — Lost & Found (F16)
**Feature:** F16 — front desk / HK / manager

**Wireframe:**
```
┌───────────────────────────┐
│ ‹ Lost & Found   [+ Log]  │
│ 🔎 search   Filter: Open ▾│
├───────────────────────────┤
│ 📷 Phone · black          │
│  R512 · 14 Jul · Unclaimed│
│ 📷 Sunglasses             │
│  Pool · 13 Jul · Claimed ✓│
│ 📷 Charger                │
│  Lobby · 12 Jul · Open    │
├───────────────────────────┤
│ [ Match to guest ]        │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **lost-and-found register**. A "+ Log" action and search/filter. A **list of items**: photo thumbnail, item name, found location + date, and a status chip (Open / Unclaimed / Claimed ✓ / Disposed). A "Match to guest" action to link an item to a reservation. Practical, photo-forward, status-driven; replaces a paper logbook.

**Checklist:** log-item + photo · search/filter · item rows (photo, location, date, status) · match-to-guest.

#### S34 — Staff SOS (F16 · global safety)
**Feature:** F16 — every role (global button)

**Wireframe:**
```
┌───────────────────────────┐
│                           │
│         🆘                │
│   Hold to send SOS        │
│   ( ●───── 3s )           │
│                           │
│ Sends your location to    │
│ security & duty manager   │
│                           │
│ 📍 Locating… Roof plant   │
│ [ Cancel ]                │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **lone-worker SOS / panic** screen. A single large red **"Hold to send SOS"** button with a 3-second hold progress ring (prevents accidental triggers). Reassuring subtext: "Sends your location to security & duty manager". A live location line "📍 Locating… Roof plant" and a "Cancel" button. Calm-serious, unmistakable, one action only; accessible with one thumb. Also show the confirmed state ("SOS sent · help notified").

**Checklist:** single hold-to-send red button with progress ring · explains who is alerted · live location · cancel · sent-confirmation state.

#### S35 — Scan / capture sheet (F17 · cross-cutting)
**Feature:** F17 — invoked from POS (S20), Inventory (S29), Assets (S27), rooms/work orders, Desk

**Wireframe:**
```
┌───────────────────────────┐
│ ‹ Scan            🔦 torch │
│  [Barcode] QR   NFC       │
│ ┌───────────────────────┐ │
│ │                       │ │
│ │      ┌─────────┐      │ │
│ │      │  ▢ aim  │      │ │
│ │      └─────────┘      │ │
│ │   Point at a barcode  │ │
│ └───────────────────────┘ │
│ Batch: 7 scanned  ▓       │
│ ── resolved ──            │
│ 🟢 Grilled Fish  ৳650     │
│   [ − 1 + ]  [ Add ]      │
│ Enter code manually ›     │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **scan / capture** screen for a hotel staff app. A full-bleed **camera viewfinder** with a rounded scanning reticle in the center and a helper line ("Point at a barcode"). Top app bar: back chevron, title "Scan", and a **torch** toggle. Below the title, a small **mode row** segmented control: "Barcode · QR · NFC". When an item resolves, a **bottom sheet** slides up showing the matched item — a green status dot, name and price (e.g. "Grilled Fish · ৳650"), a quantity stepper, and a primary "Add" button (for POS) or "Count" (for inventory). Show a **batch mode** indicator with a running tally chip ("Batch: 7 scanned"). A muted "Enter code manually ›" fallback link at the bottom. Reuse the app's tokens — deep-blue primary, rounded cards, status chips; do not invent new colors. Calm, fast, one-handed; the viewfinder dominates and the resolved-item action is thumb-reachable.

**Checklist:** camera viewfinder + reticle · Barcode/QR/NFC mode row · torch · resolved-item bottom sheet (name, price, qty, Add/Count) · batch tally · manual-entry fallback. Contextual title/action per host screen (Add to order / Count / Open asset).
**Sample:** POS → "Grilled Fish ৳650 · Add"; Inventory → "Bath towel · on-hand +1"; Asset → "AC-Chiller #2 · Open".

#### S36 — Events & Activities list (F18)
**Feature:** F18 — events coordinator / F&B / manager

**Wireframe:**
```
┌───────────────────────────┐
│ Events & Activities  🔔 + │
│ [Events] Activities       │
│ [Today] Upcoming  Past    │
├───────────────────────────┤
│ ┌───────────────────────┐ │
│ │💍 Rahman Wedding      │ │
│ │ Ballroom · 18:00·250p │ │
│ │ 🟢 Crew 12/12  ▓▓▓▓   │ │
│ ├───────────────────────┤ │
│ │🎤 Tech Corp Conf      │ │
│ │ Hall B · 09:00·80p    │ │
│ │ 🟠 Crew 5/8  under    │ │
│ └───────────────────────┘ │
│                      (＋) │
├───────────────────────────┤
│ 🏠   ✅   🛎   💬   ⋯      │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **Events & Activities list** screen titled "Events & Activities". A top segmented control "Events · Activities"; below it a second segment "Today · Upcoming · Past". A scrollable list of **event cards**: a type icon (wedding, conference, party, banquet; or activity: excursion/class/kids-club), the event name (bold), a subtitle "Space · start time · pax" ("Ballroom · 18:00 · 250 pax"), and a **crew-coverage chip** ("Crew 12/12" green when fully staffed, "5/8 · understaffed" amber) with a small progress bar. On the Activities tab, cards show recurring slots with a capacity meter ("Snorkel 10:00 · 8/12 booked") and the assigned guide. A floating "+" to create an event/activity. Persistent tab bar (More context). Clean, calm, status-driven.

**Checklist:** Events/Activities segment · Today/Upcoming/Past filter · event cards (type icon, name, space/time/pax, crew-coverage chip) · activity variant (slot + capacity meter + guide) · FAB create.
**Sample:** "Rahman Wedding · Ballroom · 18:00 · 250p · Crew 12/12"; "Snorkel trip · 10:00 · 8/12 · Guide: Karim".

#### S37 — Event detail (run-of-show + crew + resources)
**Feature:** F18 — the core planning/ops screen

**Wireframe:**
```
┌───────────────────────────┐
│ ‹ Rahman Wedding      ⋯   │
│ Ballroom · 14 Jul · 250p  │
│ [Run of show][Crew][More] │
├───────────────────────────┤
│ RUN OF SHOW               │
│ 14:00 Setup      ▓ done   │
│ 17:30 Bar open   ⏱ 1h10   │
│ 18:00 Guests in           │
│ 19:30 Dinner service      │
│ 23:00 Teardown            │
├───────────────────────────┤
│ CREW (12) · coverage 🟢   │
│ (RA) Rahim  · Captain     │
│ (SA) Sara   · Bar Lead    │
│ (KA) Karim  · Setup Lead  │
│ (NA) Nadia  · Host        │
│ [ + Assign crew & roles ] │
├───────────────────────────┤
│ Resources: Ballroom·AV·St │
│ Charges → event folio  ›  │
├───────────────────────────┤
│ [ Edit ][ Start event ]   │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **event detail** screen for a hotel banquet/event. Header: event name "Rahman Wedding", type icon, and a line "Ballroom · 14 Jul · 250 pax", with an overflow menu. Section tabs: "Run of show · Crew · More". **Run of show**: a vertical time-ordered timeline of segments (Setup 14:00, Bar open 17:30, Guests in 18:00, Dinner 19:30, Teardown 23:00), each a row with time, label, and a status chip (done green / in-progress sky with an SLA clock / upcoming muted). **Crew**: a coverage chip (green "12/12" / amber "understaffed") and a list of crew rows — avatar, name, and a **custom event-role chip** (Captain, Bar Lead, Setup Lead, Host, AV, Guide) — plus a primary "+ Assign crew & roles" button. **More**: resources booked (Ballroom, AV, staging) and a "Charges → event folio ›" row. Bottom sticky: "Edit" + "Start event". Operational, timeline-forward, role-colored; reuse existing cards/chips only.

**Checklist:** event header (type, space, date, pax) · Run-of-show/Crew/More tabs · timeline segments with status + SLA · crew rows with **event-role chips** + coverage · assign-crew CTA · resources + charges-to-folio · Edit/Start actions.
**Variants:** activity detail (recurring slot, capacity meter, assigned guide, guest sign-up list charged to folio).

#### S38 — Assign crew & custom event roles (F18 · manager)
**Feature:** F18 — line manager / GM (event-role overlay)

**Wireframe:**
```
┌───────────────────────────┐
│ ‹ Assign crew · Wedding   │
│ 🔎 add staff (any dept)   │
├───────────────────────────┤
│ ROLES TO FILL             │
│ Captain    1/1 🟢         │
│ Bar Lead   1/2 🟠         │
│ Setup      3/4 🟠         │
│ Host       2/2 🟢         │
├───────────────────────────┤
│ CREW                      │
│ (RA) Rahim   [Captain ▾]  │
│ (SA) Sara    [Bar Lead ▾] │
│ (KA) Karim   [Setup ▾]    │
│  ↳ adds Setup checklist   │
│ + New role…               │
├───────────────────────────┤
│ [   Publish assignments ] │
└───────────────────────────┘
```

**Stitch prompt:**
> Design a mobile **assign crew & event-roles** screen for a hotel manager. Top: search "add staff (any department)". A **"Roles to fill"** summary: each role with a filled/needed count and a coverage dot ("Bar Lead 1/2" amber, "Captain 1/1" green). A **crew list**: each added person is an avatar + name with a **role selector chip/dropdown** (Captain, Bar Lead, Setup, Host, AV, Guide, Marshal, Runner) — choosing a role shows a small note "adds the Bar Lead checklist". A "+ New role…" affordance to create a custom event role. Make clear that this is an **event-scoped overlay** — a caption "Roles apply to this event only; base access is unchanged." When an event assignment adds hours **beyond** a person's rostered shift (runs past shift end, or lands on a day off), show a small amber **overtime chip** on their row; an event that falls **within** their existing shift needs no flag (it adds no extra hours). The assignment writes their hours into the F9 roster on publish. Sticky "Publish assignments" button (notifies crew and writes crew hours to the roster). Clean, manager-grade, role-forward.

**Checklist:** staff search (cross-department) · roles-to-fill coverage · crew rows with **event-role selector** · role→checklist note · **overtime chip only for hours beyond a shift (F9 write-back; overlap with an existing shift adds nothing)** · create-custom-role · "event-only overlay" caption · publish (notifies crew + writes to F9 roster).
**Sample roles:** Captain, Bar Lead, Setup Lead, Host/Registration, AV/Tech, Activity Guide, Safety Marshal, Runner.

---

## 6. Screen ↔ feature index (for reference)

| Screen | Tab | Feature |
|--------|-----|---------|
| S1–S2 Login / Unlock | — | Auth / RBAC |
| S3 Home / Today launcher *(frontline)* | 🏠 Home | App shell |
| S24 Line Manager overview *(manager Home variant)* | 🏠 Home | Operational monitoring |
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
| S23 Settings | ⋯ More | — |
| S25 My Shifts & Clock-in | ⋯ More / Home | F9 |
| S26 Roster / schedule builder *(mgr)* | ⋯ More | F9 |
| S27 Asset register & PM | ⋯ More / ✅ Tasks | F10 |
| S28 Performance / Owner KPIs *(GM/owner)* | ⋯ More | F11 |
| S29 Inventory & Par-Stock | ⋯ More | F12 |
| S30 Inspection / Compliance | ✅ Tasks | F13 |
| S31 Guest Feedback & Recovery | 💬 Inbox | F14 |
| S32 Knowledge base / SOPs | ⋯ More | F15 |
| S33 Lost & Found | ⋯ More | F16 |
| S34 Staff SOS *(global)* | any screen | F16 |
| S35 Scan / capture sheet *(cross-cutting)* | POS / Inventory / Assets / rooms / Desk | F17 |
| S36 Events & Activities list | ⋯ More | F18 |
| S37 Event detail (run-of-show + crew) | ⋯ More / ✅ Tasks | F18 |
| S38 Assign crew & event roles *(mgr)* | ⋯ More | F18 |

*Screen IDs are stable; numbering skips S4, S21, and S22 (unused). S25–S34 are the extended features (F9–F16); S35 is the cross-cutting scan capability (F17), invoked from other screens rather than owning a tab; S36–S38 are Events, Activities & Banquet (F18).*

---

## 7. Global states to generate for key screens

For each primary screen, also generate:
- **Empty state** (e.g., "No tasks — you're all caught up").
- **Offline state** (amber banner + queued-changes indicator).
- **Loading skeleton** (shimmer cards/rows).
- **Error state** (retry).
- **Dark mode** variant.

**Feature-specific states to include (F9–F18), same visual language:**
- **Clock-in (S25):** clocked-out (idle), clocked-in (live timer), **geofence-blocked** (amber "Not on property"), offline clock-in queued.
- **Roster (S26):** understaffed-day warning; unpublished vs published.
- **Inventory (S29):** below-par (red reorder) row; all-healthy empty state.
- **Inspection (S30):** in-progress vs submitted; failed item with corrective task raised.
- **Feedback (S31):** negative-feedback alert (needs recovery) vs resolved.
- **SOS (S34):** idle → holding (progress ring) → **sent/confirmed** ("help notified"); offline fallback (send over cellular).
- **KPIs (S28):** positive vs negative delta styling; no-data / access-denied for non-owner roles.
- **Scan (S35):** scanning (viewfinder active) → resolved (item bottom sheet) → **not-found / no-match** (amber, offer manual entry) → **no-permission/no-camera** fallback; batch-mode running tally; offline resolve against cached catalog.
- **Events (S36–S38):** understaffed event (amber crew coverage) vs fully-crewed (green); event states draft → published → in-progress (segment running) → complete; empty "no upcoming events"; activity slot full vs available; offline run-of-show/checklist (cached).

---

## 8. Do / Don't for Stitch generations

**Do:** big glanceable numbers · color-coded status (map new domains via the **extended status map §3.2b**) · thumb-reachable primary actions · rounded cards · realistic hotel sample data · ৳ currency · light+dark · reuse the shared components in §4.3 (progress bar, status/geofence chip, pass-fail toggle, hold-to-confirm, stat tile, scan/capture sheet, run-of-show timeline + crew-role chips) rather than inventing new ones.
**Don't:** tiny tap targets · cramped tables · consumer-flashy gradients · more than one primary action per screen · decorative imagery that hides data · **new colors/radii/type styles for F9–F18** (they must reuse §3 tokens) · add a 6th bottom tab (extended features live inside existing tabs or as global layers).
