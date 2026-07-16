# Guest Messaging — Feature Deep-Dive

**Product:** Atrium Guest (Hotel / Resort Guest-Facing Mobile App)
**Feature area:** G7 — Guest Messaging
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-16
**Status:** Draft v1.0
**Related:** [Guest Mobile App — Feature Scope](guest-mobile-app-feature-scope.md) · [In-Stay Service Requests — Deep-Dive](guest-mobile-app-in-stay-service-requests-feature.md) · [Notifications & Campaigns — Deep-Dive](guest-mobile-app-notifications-campaigns-feature.md) · [App-less Access — Deep-Dive](guest-mobile-app-app-less-access-feature.md) · [Staff Task Management & Communication — Deep-Dive](../../staff/docs/staff-mobile-app-task-management-communication-feature.md) · [Competitive Feature Research](../../HOTEL-~1.MD)

---

## 1. Executive Summary

**Guest Messaging** gives every guest **one conversation with the property** — a single chat thread that opens at booking, stays alive through pre-arrival, arrival, the stay, and departure, and is answered by staff working the shared inbox in [Atrium Staff Task Management & Communication (F5)](../../staff/docs/staff-mobile-app-task-management-communication-feature.md). The guest never has to know *which* department they need; they just message the hotel, and the hotel answers.

The behavioral shift this feature serves is well documented and irreversible: **guests increasingly will not call, but will message**. A phone call requires finding a number, waiting on hold, and speaking a language the agent may not share. A chat message requires none of that — and it is the channel guests already use for everything else in their lives. Vendors across every market tier have converged on the same conclusion: Mews, Canary, Duve, Shiji, Apaleo, Guestline Keez, Operto, and SuitePad all ship two-way guest messaging, and Revinate Ivy has pushed it further with AI-suggested replies and automatic translation.

Atrium's version combines the full pattern in one thread: **automated answers** for the questions that don't need a human, **AI-assisted replies and auto-translation** so a guest writing in Bangla and an agent reading in English (or the reverse) are never blocked by language, **channel bridging** so the same conversation reaches guests on WhatsApp or SMS who never installed the app, and **chat-to-request escalation** so a message that *is* actually work becomes a tracked [G4 service request](guest-mobile-app-in-stay-service-requests-feature.md) with an owner and an SLA — not a sentence that scrolls away.

The business case is twofold: messaging **catches problems while they are still fixable** (the difference between an in-stay save and a public one-star review), and it **deflects routine question traffic** off the front-desk phone at near-zero marginal cost.

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What the guest can do from the device |
|---|------------|---------------------------------------|
| 1 | **One guest ↔ property thread** | Message the property in a single continuous conversation from booking confirmation through departure — questions, requests, photos, confirmations, all in one place. On group stays, a shared group thread (every companion plus the property) runs alongside each guest's personal thread. |
| 2 | **Staff-answered from the Atrium Staff inbox** | Every message lands in the shared team inbox in [Atrium Staff F5](../../staff/docs/staff-mobile-app-task-management-communication-feature.md), where any authorized agent can claim, answer, assign, or escalate it — the guest sees one hotel, not five departments. |
| 3 | **Automated answers / FAQ chatbot** | Get instant answers to common questions (Wi-Fi, breakfast hours, pool timing, checkout time) from an automated layer, with one-tap handoff to a human when the bot isn't sure or the guest asks. |
| 4 | **AI-assisted replies & auto-translation** | Write in Bangla and be read in English — and vice versa — with automatic two-way translation; staff get AI-suggested draft replies grounded in property information. |
| 5 | **Channel bridging (WhatsApp / SMS)** | Continue the same conversation over WhatsApp or SMS for guests who never installed the app — one thread, one history, whichever channel the guest prefers. |
| 6 | **Chat-to-request escalation** | A message that is actually a task ("we need two more towels", "the AC is leaking") converts in one tap into a tracked [G4 request](guest-mobile-app-in-stay-service-requests-feature.md) with routing, ownership, and a status the guest can watch. |

### 2.2 In scope (this release)
Single persistent guest↔property thread across the stay lifecycle; delivery into the staff shared inbox (F5) with claim/assign/escalate; automated FAQ answering with confidence-gated human handoff; AI reply suggestions for staff; automatic two-way translation (Bangla ⇄ English at minimum, extensible); WhatsApp and SMS bridging into the same thread; chat-to-request conversion into G4 with back-linked status; a shared group thread for traveling parties ([G13](guest-mobile-app-loyalty-guest-profile-feature.md) group stays) alongside personal threads; photo attachments; message history retained across stays for returning guests; quiet-hours-aware notification of replies (via [G8](guest-mobile-app-notifications-campaigns-feature.md)); app-less web chat via [G15](guest-mobile-app-app-less-access-feature.md).

### 2.3 Out of scope (this release)
Voice and video calling; guest-to-guest messaging; OTA-inbox consolidation (Booking.com / Expedia message pull — a staff-side integration, future); marketing broadcast to guests (that is [G8 Campaigns](guest-mobile-app-notifications-campaigns-feature.md), not chat); full multilingual coverage beyond the launch language pairs; social channels beyond WhatsApp/SMS (Messenger, Viber, WeChat — future per market demand).

### 2.4 Key assumptions
- The staff-side shared inbox and routing/escalation model in F5 is live — G7 is its guest-facing mouth, not a parallel system.
- WhatsApp Business API access (via a BSP) and an SMS gateway are provisioned for the property's numbers.
- A translation service (cloud API or hosted model) meeting acceptable Bangla⇄English quality is available; AI reply suggestions can be grounded in the property's [G10 guidebook](guest-mobile-app-digital-guidebook-property-info-feature.md) content.
- Guest identity resolves from the reservation (booking reference / phone / email), so a WhatsApp message and an in-app message from the same guest merge into one thread.

---

## 3. Capability Deep-Dive — The Messaging Features

Each capability is elaborated — **what it is**, **how it works in Atrium**, **why it matters**, and the **revenue/cost angle** — with vendor precedents noting the bar to clear.

### 3.1 One guest ↔ property thread, pre-arrival to departure
*Precedent: Mews, Canary, Duve, Shiji, Guestline Keez, Operto, SuitePad.*

**What it is.** A single, persistent conversation between the guest and the property — not a per-department contact form, not a new chat per question — that opens when the booking is confirmed and stays live through checkout and beyond.

**How it works in Atrium.** The thread is anchored to the guest profile and the reservation. Pre-arrival, the guest asks about airport pickup; on arrival day, they say the flight is delayed; mid-stay, they ask about the spa; at departure, they ask for a late luggage hold — all in the same scrollable history. Staff replying see the full context: reservation, stay dates, room, prior messages, open requests. For returning guests, history persists across stays, so "the same driver as last time, please" makes sense to whoever answers. On group stays, the traveling party defined in [G13 group management](guest-mobile-app-loyalty-guest-profile-feature.md) also gets a **shared group thread** — all companions plus the property in one conversation for the plans that concern everyone ("dinner for six at eight?") — while each guest keeps their own personal thread for the rest.

**Why it matters.** Fragmented contact points (a phone number here, an email there, a form for complaints) put the burden of routing on the guest. One thread puts it on the system — where it belongs. It also produces something a phone call never does: a written record both sides can scroll back to.

**Revenue / cost angle.** A low-friction channel surfaces needs that would otherwise go unexpressed — including purchase intent ("can we book the sunset cruise?") that converts to [G12](guest-mobile-app-activities-spa-experiences-feature.md)/[G6](guest-mobile-app-upsells-offers-feature.md) revenue (revenue); the written record kills the "nobody told me" rework loop between guest and departments (cost).

### 3.2 Staff-answered from the Atrium Staff shared inbox (F5)
*Precedent: Mews, Shiji, Apaleo — guest messages routed into the staff operations platform.*

**What it is.** The staff side of the thread: every guest message lands in the shared team inbox inside [Atrium Staff F5](../../staff/docs/staff-mobile-app-task-management-communication-feature.md), with claim/assign semantics, internal notes invisible to the guest, and escalation paths — the same accountability machinery staff already use for internal work.

**How it works in Atrium.** An incoming message appears in the inbox with the guest's context attached. An agent claims it (so two people don't answer at once), replies, or reassigns it to the right department with an internal note. Response-time SLAs are visible; unclaimed messages escalate to the duty manager. Nothing about this is a second system for staff to learn — it is the same F5 surface.

**Why it matters.** Guest messaging fails in practice not on the guest side but on the staff side: an unanswered chat is worse than no chat, because silence reads as neglect. Wiring the guest thread into the inbox that staff already live in — with ownership, SLAs, and escalation — is what makes "we always answer" an enforceable operational fact rather than a hope.

**Revenue / cost angle.** Answered messages read as attentive service, which shows up in review scores and rate power (revenue); one shared inbox means the whole property needs no dedicated "chat agent" headcount — messaging absorbs into existing shift work (cost).

### 3.3 Automated answers / FAQ chatbot with human handoff
*Precedent: Duve (AI agents), eZee (chatbot); Revinate Ivy.*

**What it is.** An automated first responder for the questions that make up the bulk of guest traffic — Wi-Fi password, breakfast hours, pool timing, checkout time, "do you have parking?" — answering instantly, around the clock, with a clean handoff to a human the moment the question exceeds its confidence or the guest asks for a person.

**How it works in Atrium.** The bot is grounded in the property's own content — the [G10 guidebook](guest-mobile-app-digital-guidebook-property-info-feature.md), outlet hours, policies — so answers are property-true, not generic. Low-confidence answers are never bluffed: the bot says it's connecting a person and the thread lands in the F5 inbox flagged for human reply, with the bot exchange visible so the guest never repeats themselves. Every automated answer carries an unobtrusive "talk to a person" affordance.

**Why it matters.** The majority of guest questions have deterministic answers that no human needs to type at 11pm. Automation makes the channel instant at all hours — which is exactly when a fixed front desk is thinnest — while the handoff rule protects the one thing that matters more than speed: never trapping a guest in a bot loop.

**Revenue / cost angle.** Instant answers at midnight keep late purchase intent alive ("is the bar still open?" → an order) (revenue); every deflected FAQ is a phone call or desk visit that no staff member handles — the deflection compounds daily (cost).

### 3.4 AI-assisted replies & auto-translation
*Precedent: Revinate Ivy (AI-suggested replies, chats with guests in their own language).*

**What it is.** Two AI assists on the staff side of the thread: **suggested draft replies** composed from property information and conversation context, and **automatic translation** in both directions — the guest writes in Bangla and the agent reads English; the agent replies in English and the guest reads Bangla. The same applies to any supported language pair for international guests.

**How it works in Atrium.** Translation is inline and marked ("translated from বাংলা"), with the original a tap away. Draft suggestions appear in the agent's compose box for one-tap send or edit — the agent always remains the sender of record; nothing auto-sends to a guest except the FAQ layer in §3.3. Language preference is detected per guest and remembered on the profile.

**Why it matters.** In the BD market this is not a nice-to-have — it is the difference between a channel that works and one that doesn't. A property serving Bangla-speaking domestic guests and English-speaking international guests with a mixed-fluency staff roster has a language mismatch on a large share of conversations. Auto-translation removes it silently. Suggested replies matter for the same operational reason: they let a junior agent on a night shift answer like a senior one.

**Revenue / cost angle.** Language-barrier-free service widens the sellable market on both sides — international guests served confidently, domestic guests served in their own script (revenue); suggested replies cut per-message handling time and flatten the training curve for high-turnover staff (cost).

### 3.5 Channel bridging — WhatsApp & SMS in the same thread
*Precedent: Mews, Canary, Duve, Revinate Ivy — messaging hubs spanning SMS, WhatsApp, email; Canary/Duve multi-channel delivery.*

**What it is.** The same conversation, carried over the channel the guest actually uses. A guest who never installed the app messages the property's WhatsApp number; the thread is the same thread, in the same F5 inbox, with the same history — and if the guest later opens the app or web chat ([G15](guest-mobile-app-app-less-access-feature.md)), the conversation follows them.

**How it works in Atrium.** Inbound WhatsApp/SMS messages are matched to the reservation by phone number and merged into the guest's thread. Outbound staff replies are delivered to whichever channel the guest last used (or prefers). Channel is a delivery detail the staff never manage by hand — the agent just replies to the conversation.

**Why it matters.** This is the adoption answer for messaging specifically: WhatsApp penetration in Bangladesh and across Atrium's guest mix vastly exceeds any hotel app's conceivable install rate. Bridging means G7's reach is **every guest with a phone number**, not every guest who installed an app — the messaging-layer expression of the [G15 app-less principle](guest-mobile-app-app-less-access-feature.md).

**Revenue / cost angle.** Reach is the multiplier on everything above — deflection, recovery, and chat-sourced revenue all scale with the share of guests actually connected (revenue); one merged thread avoids the staff cost of monitoring WhatsApp on a shared handset, disconnected from the PMS, which is the messy status quo (cost).

### 3.6 Chat-to-request escalation
*Precedent: Guestline Keez, Apaleo (in-app requests); ALICE/Actabl-pattern routing on the staff side.*

**What it is.** The bridge from conversation to work: when a message is actually a task, the agent (or the automated layer, for unambiguous cases) converts it into a tracked [G4 service request](guest-mobile-app-in-stay-service-requests-feature.md) — typed, routed to the owning department, SLA-timed — without the guest doing anything differently.

**How it works in Atrium.** "The AC in 412 is leaking" gets a one-tap **Convert to request** in the staff inbox: the request is created pre-filled (room, guest, photo if attached, category = maintenance), routed via G4's rules to engineering, and back-linked into the chat — the guest sees a status chip ("Maintenance request created · In progress") right in the thread, and the closing update posts back to the conversation.

**Why it matters.** Chat without escalation is where requests go to die: a message is read, mentally noted, and lost at shift change. Conversion makes the thread a *front door to the operation*, not a suggestion box — everything actionable becomes owned, timed, and visible, on both sides.

**Revenue / cost angle.** Reliable fulfillment of chat-sourced requests is what converts messaging from a satisfaction feature into a service-delivery channel (revenue protection); structured conversion eliminates the duplicated and forgotten work of informally relayed requests (cost).

---

## 4. Why This Matters

### 4.1 Guests message; properties that only answer phones miss them
The guest who won't queue at the desk and won't wait on hold will happily type one sentence. If the property has no messaging channel — or has one that goes unanswered — those sentences are never sent, and the needs, complaints, and purchase intent inside them are lost. Every major guest-experience vendor (Mews, Canary, Duve, Shiji, Revinate) has made two-way messaging a core product for exactly this reason.

### 4.2 A message caught in-stay is a review saved
The highest-value messages are the unhappy ones. A guest who can type "the room smells of smoke" at 9pm and get a response in minutes is a guest whose problem is fixed on property — feeding the [G14 service-recovery loop](guest-mobile-app-feedback-service-recovery-feature.md) and the staff-side [F14 workflow](../../staff/docs/staff-mobile-app-guest-feedback-service-recovery-feature.md). The same guest with no channel airs it on a review site three days later, permanently.

### 4.3 The desk phone is a queue; the inbox is parallel
Phone traffic serializes through whoever picks up. A shared inbox with automation in front of it handles dozens of simultaneous conversations, triages them by urgency, and answers the routine majority with zero labor — service capacity stops scaling with headcount.

### 4.4 Language is a solved problem now — and BD vendors haven't solved it
Auto-translation at production quality is newly cheap. Almost no local competitor offers a guest-messaging channel at all, let alone a bilingual one; Revinate Ivy proves the pattern globally. Bangla⇄English messaging is a differentiator Atrium can own in this market.

---

## 5. Journey Moments & Operations It Improves

| Moment / operation today | Pain | With Guest Messaging |
|--------------------------|------|----------------------|
| **Pre-arrival questions** (transport, early check-in) | Emails to a rarely-read inbox; international calls | Answered in the thread within minutes; commitments recorded in writing. |
| **"What's the Wi-Fi / breakfast time?"** | Front-desk phone rings all day with FAQs | Automated layer answers instantly, 24/7; staff never touch the routine majority. |
| **Guest with a problem at 10pm** | Won't call, won't walk down; complaint surfaces in a review later | Types one sentence; duty team responds and fixes it in-stay. |
| **Non-Bangla guest ↔ Bangla-speaking staff** (and the reverse) | Miscommunication, avoidance, wrong deliveries | Auto-translation both ways; each side reads their own language. |
| **Guest without the app** | Unreachable except by phone or knocking | Same conversation over WhatsApp/SMS — every guest with a phone number is connected. |
| **"Can we get two more towels?" in chat** | Message read, relayed verbally, lost at shift change | One-tap conversion to a tracked G4 request with status visible in the thread. |
| **Staff side: who answers?** | WhatsApp on a shared handset, no ownership, no record | F5 shared inbox with claim, assign, SLA, and escalation — logged and auditable. |
| **Returning guest context** | Every conversation starts from zero | Persistent history on the profile; recognition without asking. |

---

## 6. How It Increases Revenue

1. **Problems caught in-stay protect review scores.** Messaging is the lowest-friction complaint channel that exists; complaints that reach the property instead of TripAdvisor are recoverable. Review scores directly drive OTA ranking, direct-booking conversion, and rate power.
2. **Chat is a sales channel.** Questions like "is the spa free tomorrow?" and "can you arrange a birthday cake?" are purchase intent. Agents (with AI-suggested replies that include the relevant offer) convert them into [G12 bookings](guest-mobile-app-activities-spa-experiences-feature.md) and [G6 add-ons](guest-mobile-app-upsells-offers-feature.md) inside the conversation.
3. **Answered pre-arrival questions convert bookings and add-ons.** A prospective or booked guest whose question is answered in minutes books the transfer, the dinner, the late checkout — and a property that answers fast pre-arrival wins bookings a silent one loses.
4. **Language coverage widens the market.** Confident bilingual service makes the property genuinely sellable to both international guests and Bangla-first domestic travelers — a mix most BD properties serve unevenly today.
5. **Reach via channel bridging multiplies all of the above.** Every revenue mechanism in this list scales with the share of guests connected; WhatsApp/SMS bridging takes that share to effectively everyone.

> **Illustrative model:** A 150-room property at 75% occupancy hosts ~110 in-house parties on a typical night. If messaging surfaces purchase intent from just 5% of stays converting at an average ৳1,500 (a spa slot, a dinner booking, a transfer), that is ~ ৳8,000+/day ≈ **৳2.9M/year** in chat-sourced ancillary revenue — before counting the review-score protection from in-stay saves, which most operators value higher. *(Figures illustrative; validate against property data.)*

---

## 7. How It Reduces Operational Cost

1. **FAQ deflection at zero marginal cost.** The automated layer absorbs the routine majority of guest questions — every one of them a phone call or desk visit that no longer consumes staff minutes. Across hundreds of daily contacts, this is a material share of front-desk load.
2. **Parallel handling replaces serialized phone answering.** One agent works many conversations concurrently in the inbox; the same agent handles one phone call at a time. Peak-period question load stops driving staffing.
3. **Faster per-message handling via AI drafts.** Suggested replies cut compose time per message and let junior staff answer at senior quality — lowering both handling cost and training cost in a high-turnover market.
4. **No lost-request rework.** Chat-to-request conversion eliminates the duplicated, forgotten, and mis-relayed requests of verbal relay — each of which otherwise costs a second trip, an apology, or a comp.
5. **One system instead of a shadow channel.** Replacing the unmanaged shared-handset WhatsApp with bridged, logged, PMS-linked messaging removes reconciliation labor and the compliance exposure of guest data on personal apps — the guest-side twin of the argument in [staff F5](../../staff/docs/staff-mobile-app-task-management-communication-feature.md).
6. **Translation without interpreters.** Auto-translation removes the hidden costs of language mismatch: wrong deliveries, repeated clarification trips, and dependence on whichever staff member happens to speak the guest's language.

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Channel adoption | Share of in-house stays with an active message thread | ≥ 50% |
| Responsiveness | Median first-response time (human-handled messages) | < 5 minutes |
| Automation leverage | Share of inbound messages resolved by automated answers | ≥ 40% |
| Handoff quality | Bot conversations escalated to a human without guest repeat | 100% context carried |
| Deflection | Front-desk call volume | ↓ 30% vs. baseline |
| Work capture | Chat messages converted to tracked G4 requests | ≥ 90% of actionable messages |
| Language coverage | Cross-language conversations completed without human translation | ≥ 95% |
| Recovery | Negative signals raised in chat and resolved in-stay | ↑ measurable; feeds G14/F14 |
| Reach | Guests connected via app + WhatsApp/SMS bridge combined | ≥ 85% of stays |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| Staff inbox (F5) | G7 is only as good as the answering side | Ship wired into F5 claim/SLA/escalation from day one; unanswered-thread alerts to duty manager |
| Response discipline | An ignored chat is worse than no chat | Response-time SLAs, escalation, visible team metrics; set guest expectations ("typically replies in minutes") |
| WhatsApp Business API | BSP onboarding, template approval, per-conversation pricing | Provision during property onboarding; SMS as universal fallback |
| Translation quality | Bangla⇄English MT errors in service context | Show-original affordance; glossary of property terms; human override; measure correction rate |
| Bot overreach | Wrong or bluffed automated answers damage trust | Ground strictly in property content; confidence gating; always-visible human handoff; audit transcripts |
| Guest identity matching | WhatsApp number must map to the reservation | Match on booking phone/email; confirm-on-first-message flow for ambiguous matches |
| Data protection | Guest PII in messages, on-device and in transit | Encryption in transit/at rest, retention policy, RBAC on the staff side, audit log |
| Notification fatigue | Reply pings compete with G8 traffic | Route through G8 preference/quiet-hours engine; message notifications always user-controllable |

---

## 10. Scope delivered

Atrium Guest ships as one complete package. This feature delivers:

- One persistent guest↔property chat thread, booking through departure, with cross-stay history
- Shared group thread for the traveling party (companions per G13) alongside each guest's personal thread
- Delivery into the Atrium Staff F5 shared inbox with claim, assign, internal notes, SLA, and escalation
- Automated FAQ answering grounded in property content, with confidence-gated human handoff
- AI-suggested draft replies for staff (agent always sends)
- Automatic two-way translation (Bangla ⇄ English at launch), with show-original
- WhatsApp and SMS bridging into the same merged thread, matched to the reservation
- Chat-to-request conversion into G4 with back-linked status in the thread
- Photo attachments in both directions
- App-less web chat entry via G15 links and QR touchpoints
- Reply notifications via G8 with per-guest preferences and quiet hours

---

## 11. Open Questions

- Which WhatsApp Business Solution Provider (BSP), and who owns the per-conversation messaging cost — platform or property?
- Launch language pairs beyond Bangla⇄English — Hindi, Arabic, Chinese for the international mix?
- Is the FAQ bot allowed to auto-convert unambiguous requests to G4 (e.g., "more towels please"), or is conversion human-only in v1?
- Message retention period and guest data-deletion policy — per property, or platform-wide?
- Do pre-booking (Discover & Book stage) visitors get chat access, or does the thread open only at confirmed booking?
- Staffing model guidance for properties: who owns the inbox on the night shift?

---

## 12. One-Line Business Case

> **Guest Messaging gives every guest one always-answered thread to the property — catching problems while they're still fixable, deflecting the routine question load off the desk, selling in the conversation, and speaking the guest's own language — with every actionable message becoming tracked, owned work instead of a sentence that scrolls away.**
