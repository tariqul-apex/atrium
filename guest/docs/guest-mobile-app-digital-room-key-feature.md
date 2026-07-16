# Digital Room Key — Feature Deep-Dive

**Product:** Atrium Guest (Hotel / Resort Guest-Facing Mobile App)
**Feature area:** G3 — Digital Room Key
**Document owner:** rdasgupta@apexdmit.com
**Last updated:** 2026-07-16
**Status:** Draft v1.0
**Related:** [Guest Mobile App — Feature Scope](guest-mobile-app-feature-scope.md) · [Competitive Feature Research](../../HOTEL-~1.MD)

---

## 1. Executive Summary

**Digital Room Key** makes the guest's phone the room key: a Bluetooth (BLE) mobile key issued through certified door-lock integrations the moment the room is ready, living in the app or the phone's wallet, shareable with a travel companion, opening every door the stay entitles the guest to — room, floor, gym, pool gate — and revoked automatically at check-out.

Without this feature, the contactless journey has a hole in it: a guest who booked in [G1](guest-mobile-app-booking-pre-arrival-feature.md) and checked in from the taxi in [G2](guest-mobile-app-check-in-check-out-feature.md) still ends up standing in a line — the key line. The digital key closes that gap, and removes an entire physical logistics chain with it: card stock, encoders, demagnetized-card re-key trips, and lost-card lockouts. INTELITY, Oracle OPERA Cloud, Mews, Infor HMS, RMS Cloud (via OpenKey), and Operto all ship mobile keys through certified lock partnerships (Salto, ASSA ABLOY, dormakaba, OpenKey); Mews puts the key on the iPhone lock screen via Apple Wallet + ASSA ABLOY.

One dependency is honest and structural: **digital keys need compatible lock hardware.** In the Bangladesh market, where many properties run older magstripe or offline RFID locks, retrofit cost is a real constraint — so Atrium treats the digital key as the flagship tier of a graceful ladder, with a physical-card fallback at the desk (encoded by staff [F1](../../staff/docs/staff-mobile-app-front-desk-feature.md)) keeping every property, fitted or not, on the same platform.

This document defines the feature's scope, quantifies its business case, and maps it to the journey moments and operations it improves.

---

## 2. What the Feature Is (Scope)

### 2.1 Core capabilities

| # | Capability | What the guest can do from their phone |
|---|------------|----------------------------------------|
| 1 | **BLE mobile key via certified lock integrations** | Hold the phone to the door and it opens — through certified integrations with Salto, ASSA ABLOY, dormakaba, and OpenKey lock systems. |
| 2 | **Wallet key (Apple Wallet / lock-screen)** | Keep the key in Apple Wallet (Google Wallet where supported) — usable from the lock screen, no app launch, works in power-reserve mode. |
| 3 | **Key share with companion** | Send a key to a travel companion's phone — spouse, family member, colleague — within party limits the property sets, each share logged and individually revocable. |
| 4 | **Common-door access** | The same key opens what the stay entitles: floor access, gym, pool gate, executive lounge, parking — entitlement-driven, per stay type and loyalty tier. |
| 5 | **Auto-issue on room-ready + auto-revoke at check-out** | The key appears the instant the room is ready (G2's room-ready event) and dies the instant check-out completes — no desk step at either end. |
| 6 | **Physical-card fallback at desk** | A keycard encoded by staff (F1) at any time — for unfitted properties, incompatible phones, dead batteries, or guest preference. |

### 2.2 In scope (this release)

BLE key issuance and door unlock via certified lock-vendor integrations, Apple Wallet key where the lock vendor supports it, companion key sharing with audit and revocation, entitlement-based common-door access, lifecycle automation (issue on room-ready, extend on stay change, revoke on check-out), key state visibility for staff, and the physical-card fallback path via staff F1 encoding.

### 2.3 Out of scope (this release)

Lock hardware supply and installation (property capital project; Atrium certifies integrations, not locks), in-room controls — lighting, temperature (a G4-adjacent future with Operto-class smart-room vendors), employee/back-of-house access control (staff domain, separate system), PIN-pad and smart-lock consumer devices outside certified hospitality lock lines, and offline key issuance to phones that have never connected since check-in.

### 2.4 Key assumptions

- Target properties have — or will retrofit — BLE-capable locks from a certified vendor (Salto, ASSA ABLOY, dormakaba, or OpenKey-compatible lines); **this is the feature's hard dependency and a real cost constraint in the BD market**, where many properties run magstripe or offline RFID locks.
- The lock vendor's key-issuance cloud/SDK is reachable from Atrium's backend with per-property credentials.
- Room-ready and check-out events flow reliably from G2/PMS/housekeeping (F3) — lifecycle automation is only as trustworthy as those triggers.
- Wallet-key availability depends on lock-vendor + platform partnerships (e.g., Apple Wallet via ASSA ABLOY-class integrations) and may lag the in-app BLE key per vendor.
- The desk retains a working card encoder (F1) — the fallback is a permanent tier, not a transition artifact.

---

## 3. Capability Deep-Dive — The Six Digital Key Features

The competitive research (Part A §3, *Digital / Mobile Room Key*) identifies the capabilities that define a best-in-class mobile key. Each is elaborated below: **what it is**, **how it works in Atrium**, **why it matters**, and the **revenue/cost angle**. Vendor precedents are noted so we know the bar to clear.

### 3.1 BLE mobile key via certified lock integrations
*Precedent: INTELITY, Oracle OPERA Cloud, Mews, Infor HMS, RMS Cloud via OpenKey, Operto / YourKey; lock partnerships with Salto, ASSA ABLOY, dormakaba (Mews, INTELITY, Oracle).*

**What it is.** The phone as the key: a cryptographic credential provisioned to the guest's device through the lock vendor's certified cloud, unlocking the door over Bluetooth Low Energy with a tap or proximity hold.

**How it works in Atrium.** Atrium integrates the certified issuance APIs of the major hospitality lock vendors — Salto, ASSA ABLOY (Vingcard-class), dormakaba, and OpenKey as a multi-vendor aggregation path. When G2 fires the key-issue event, Atrium requests a credential scoped to the guest's room and entitled common doors, valid for the stay dates; the credential lands in the app and the door unlocks offline over BLE — no property Wi-Fi needed at the door. Every unlock is logged in the lock system's audit trail; Atrium never invents its own lock protocol.

**Why it matters.** Certification is the whole game: door security is life-safety infrastructure, insurers and flag standards demand certified lock systems, and no property will trust an uncertified path. Building on the vendors' own rails gives Atrium bank-grade key security on day one and a portfolio answer — whichever certified locks a property has or buys, Atrium speaks to them.

**Revenue / cost angle.** The mobile key is the capability that completes and sells the contactless-journey story properties pay premium software rates for (revenue); every phone-key stay avoids card issuance, and every avoided card avoids its share of loss, demagnetization, and re-key labor (cost).

### 3.2 Wallet key — Apple Wallet / lock-screen
*Precedent: Mews (Apple + ASSA ABLOY integration — key on the lock screen and Apple Watch).*

**What it is.** The key living in the phone's wallet rather than behind an app launch: surfaced from the lock screen, usable on a watch, and functional even in the phone's power-reserve state after the battery "dies."

**How it works in Atrium.** Where the property's lock vendor supports platform wallet provisioning (today most maturely Apple Wallet via ASSA ABLOY-class integrations), Atrium offers "Add to Wallet" at key issuance. The wallet credential parallels the in-app key — same room, same entitlements, same revocation — and express-mode unlocking means the guest holds the phone to the door with no unlock, no app, no thought. Google Wallet support follows per lock-vendor availability.

**Why it matters.** The wallet key removes the last frictions the in-app key still has: finding the app, waking the phone, the dead-battery panic (power-reserve keys keep working for hours). It is also the premium signal — the "key on your lock screen like a boarding pass" experience guests associate with world-class hotels.

**Revenue / cost angle.** A flagship differentiator for the properties Atrium wants as lighthouse accounts — no BD competitor is remotely close (revenue via positioning); wallet express mode cuts the door-fumbling support moments that generate desk calls (cost, minor).

### 3.3 Key share with companion
*Precedent: Operto / YourKey (multi-guest access); OpenKey-class sharing patterns.*

**What it is.** The booking guest sends a key to a companion's phone — the spouse who arrives on a later flight, the colleague sharing a twin room, a family member — without anyone visiting the desk.

**How it works in Atrium.** From the key screen, the guest invites a companion by phone number or link; the companion lands via the invite link (G15), verifies with an OTP, and is handed off to the app install to receive their own credential — not a copy of the primary's (key issuance is native-only; the web flow verifies identity and hands off). The property sets policy: maximum keys per room, whether companions must complete identity capture (G2 registration of accompanying guests), and which common doors companions inherit. Every share is logged; the primary guest or staff can revoke any single companion key without touching the others.

**Why it matters.** Real stays are multi-person, and the "second key" is a genuine desk-queue generator — the later-arriving spouse queues at reception at 11 p.m. for a card. Sharing closes the last everyday reason a contactless stay touches the desk. Per-person credentials (rather than shared secrets) keep the audit trail honest, which matters for security investigations.

**Revenue / cost angle.** Eliminates second-key desk visits and their card issuance across nearly every multi-guest stay (cost); family- and group-friendliness is a resort-segment selling point where parties of four-plus are the norm (revenue via positioning).

### 3.4 Common-door access — floor, gym, pool gate
*Precedent: Certified lock platforms' multi-door credentialing (Salto, ASSA ABLOY, dormakaba estates); Operto property-access patterns.*

**What it is.** One credential for everything the stay entitles: the room, the keyed lift/floor, the gym, the pool gate, the executive lounge, parking — with entitlements driven by stay type, rate plan, and loyalty tier.

**How it works in Atrium.** Property configuration maps door groups to entitlements: every in-house guest gets floor + gym; club-rate and gold-tier guests (G13) add the lounge; a paid booking or upgrade (G12/G6) can extend an in-house guest's entitlements mid-stay. Atrium composes the entitlement set at issuance and updates it live — buy lounge access mid-stay as a G6 upsell and the door starts opening within minutes.

**Why it matters.** A key that opens the room but not the pool gate sends the guest back to the desk — the failure mode that makes guests abandon the digital key entirely. Entitlement-driven access also turns doors into *product*: access to spaces becomes sellable, tierable, and instantly deliverable.

**Revenue / cost angle.** Gated amenities become upsellable line items (lounge access, premium-floor upgrade) with zero fulfillment labor — the door itself delivers the purchase (revenue); ends the "my card doesn't open the gym" desk traffic and re-encode churn (cost).

### 3.5 Auto-issue on room-ready + auto-revoke at check-out
*Precedent: Shiji-class room-ready automation feeding key issuance; Mews, Operto lifecycle automation.*

**What it is.** The key's lifecycle runs itself: issued the moment housekeeping marks the room ready and check-in is complete, extended when the stay changes, and revoked the instant check-out (or a staff action) says so.

**How it works in Atrium.** G2's room-ready event is the trigger: check-in complete + room inspected-ready → key issues and the guest's room-ready push doubles as "your key is live." A room move (staff F1) reissues for the new room and kills the old credential; a stay extension re-scopes validity; express check-out (G2) revokes within seconds, as does a staff-side security revocation. Staff see key state — issued, active, shared ×2, revoked — on the guest record, and every transition is audited.

**Why it matters.** Lifecycle automation is what makes the digital key *safer* than plastic, not just slicker: physical cards routinely outlive their stay (expiry mis-encodes, unreturned cards), while a digital credential dies at check-out by construction. It also removes the desk from both ends of the key's life — the final piece of the fully self-service arrival and departure.

**Revenue / cost angle.** Enables the complete queue-free arrival that drives G2's adoption and upsell numbers (revenue); removes key-encode labor from every fitted-property arrival and the security exposure of live keys past departure (cost, risk).

### 3.6 Physical-card fallback at the desk
*Precedent: Universal industry practice; staff-side encoding per Stayntouch-class mobile front desk — Atrium Staff F1 key encoding.*

**What it is.** The honest tier below the digital key: a keycard encoded at the desk (or by a roving F1 agent with a mobile encoder) for any guest or any property the digital path doesn't cover.

**How it works in Atrium.** The fallback is first-class, not an apology. At unfitted properties, Atrium runs the full G1/G2 journey and ends arrival at an express key-pickup step — staff F1 encodes the card in seconds against the already-completed check-in. At fitted properties, it covers incompatible phones, dead batteries, guest preference, and lock-system outages. Card and digital keys coexist on the same room without conflict, and staff manage both from one key panel.

**Why it matters.** This is the capability that makes the BD go-to-market work. Lock retrofit is real capital — often the largest hardware line in a property's digitalization budget — and many properties will adopt Atrium years before they re-lock. The fallback keeps them on the platform with 90% of the journey contactless, and makes the lock retrofit an *upgrade decision with a visible payoff* rather than an adoption precondition.

**Revenue / cost angle.** Decouples Atrium's sales pipeline from customers' lock capex — the addressable market is every property, not just the re-locked ones (revenue); pre-completed check-ins make even card issuance a seconds-long transaction (cost).

---

## 4. Why This Matters

### 4.1 It is the keystone of the contactless journey
G1 gets the guest known, G2 gets them checked in — and without G3, they still stand in a line for a piece of plastic. The digital key is the difference between "mobile check-in" as a marketing phrase and a genuinely desk-free arrival. Everything G2 promises lands here.

### 4.2 Plastic keys are a permanent operating drag
Cards get lost, demagnetized by phones and magnets, left in pockets at departure, and reissued endlessly; encoders jam and age out; every re-key is a guest trudging back to the desk mid-stay. The digital key deletes this entire logistics chain for every phone-key stay — and is *more* secure, because credentials die at check-out by construction and every unlock is attributable to a person.

### 4.3 Doors become sellable product
Entitlement-driven access turns spaces into instantly deliverable inventory: lounge access, premium floors, pool day-passes, early gym hours. With G6's offer engine and G13's tiers, the key system is the fulfillment layer for a whole class of zero-labor upsells.

### 4.4 In BD, honesty about hardware is the differentiator
Almost no Bangladeshi vendor ships a guest app; none ships mobile keys. But BD properties' lock estates are the constraint — so the winning position is not "digital key required," it is Atrium's ladder: full journey + card fallback today, certified BLE keys the day the property re-locks, with the same app on the guest's phone throughout. Global vendors don't offer BD properties that path; Atrium can.

---

## 5. Journey Moments & Operations It Improves

| Moment / operation today | Pain | With Digital Room Key |
|--------------------------|------|------------------------|
| **After mobile check-in** | Guest still queues at the desk — for a card | Key auto-issues on room-ready; guest walks straight to the room. |
| **Companion arriving later** | Spouse queues at 11 p.m. for a second card | Primary guest shares a key from the phone; companion walks in. |
| **Demagnetized / lost card mid-stay** | Trudge to the desk, verify identity again, re-encode; lost cards = security exposure | No card to lose; a phone-key problem is solved by reopening the app, and any credential is revocable in seconds. |
| **Card works on room but not gym/lift** | Encoding error sends the guest back to the desk | Entitlements composed centrally at issuance — every entitled door works from minute one. |
| **Check-out key handling** | Cards to collect, drop-boxes to empty, unreturned cards live past departure | Credential revokes itself at check-out; nothing to collect, no live keys walking out. |
| **Room move** | Old card to retrieve, new card to encode, guest visits the desk | Old key dies, new key issues — the guest just gets a notification. |
| **Selling lounge / premium access** | A sticker on the card, a list at the door, manual checks | The door itself enforces the purchase, activated minutes after the upsell. |
| **Key-stock logistics** | Card purchasing, encoder maintenance, peak-day stockouts | Entire chain removed for phone-key stays; card volume shrinks to the fallback tier. |
| **Security investigation** | "A card opened the door" — whose? | Per-person credentials and lock audit trails answer it. |
| **Unfitted BD property** | Locked out of the modern journey entirely | Full G1/G2 journey + seconds-long express card pickup (staff F1); lock retrofit becomes an upgrade, not a prerequisite. |

---

## 6. How It Increases Revenue

1. **It completes the contactless product that wins premium accounts.** Mobile key is the visible flagship of the modern guest journey — the capability that lets a property market "arrive and walk straight to your room." That story sells rooms and sells Atrium; in BD, no competitor can tell it at all.
2. **Access-based upsells with zero fulfillment labor.** Lounge access, premium-floor upgrades, pool and gym day-passes, early check-in to a ready room — G6 sells them, the key system delivers them, the door enforces them. Entirely new folio lines at near-100% margin.
3. **Full G2 adoption unlocked.** Self-service check-in adoption — and its upgrade/late-check-out revenue — is capped while arrival still ends in a key line; G3 removes the cap.
4. **Resort and group appeal.** Shareable family keys and party-wide access are decisive in the resort segment where parties are large and desk trips are long walks.
5. **Review-score lift on arrival.** "Walked straight to the room" is a review sentence; smoother arrivals lift scores, rate power, and the direct-booking loop (G1/G13).
6. **Access-based monetization (post-v1 extension).** Time-boxed day-pass credentials (pool, gym via G12) could let day-use guests buy access that starts and stops itself — a natural extension once the in-house key is stable. Per the current canon, day visitors remain keyless in v1.

> **Illustrative model:** A 150-room fitted resort at 75% occupancy (~30,000 stays/year). If access-based upsells (lounge, premium floor) attach on 5% of stays at ৳1,800 average, that is ~৳2.7 million/year (~$22,500). Add the G2 upsell revenue G3 unlocks by lifting self-service adoption from 50% to 70% (+৳1.8 million on the G2 model's assumptions): ≈ **৳4.5 million (~$37,500)/year** attributable, before review-score and positioning effects. *(Figures illustrative; validate against property data.)*

---

## 7. How It Reduces Operational Cost

1. **Card logistics shrink toward zero.** Card stock, sleeves, encoder hardware, encoder maintenance, and peak-day stock management all scale down with every phone-key stay — at scale, thousands of cards a year not bought, encoded, or chased.
2. **Re-key desk traffic eliminated.** Demagnetized, lost, and mis-encoded card visits — among the most frequent desk interruptions at any property — disappear for phone-key guests, and are solved in seconds (revoke + reissue) for the rest.
3. **Key-encode labor removed from every fitted arrival.** Issuance is an API call, not a staff step — and at check-out there is nothing to collect or reconcile.
4. **Second-key and room-move handling automated.** Companion shares and room moves happen without a staff touch.
5. **Security-incident exposure cut.** Auto-revocation ends the live-key-past-checkout problem; per-person credentials and audit trails shorten investigations and reduce lock re-programming after losses.
6. **Access-control administration consolidated.** Entitlement changes (a guest buys lounge access, a tier upgrade lands) propagate from one place instead of manual re-encodes and door lists.
7. **Fallback path still cheaper.** Even at unfitted properties, express card pickup against a pre-completed G2 check-in cuts the desk transaction to seconds.

---

## 8. Success Metrics (KPIs)

| Objective | Metric | Target |
|-----------|--------|--------|
| Digital-key adoption | Fitted-property stays using mobile key | ≥ 40% by month 6 |
| Desk-free arrival | Check-ins completing with zero desk contact (with G2) | ≥ 30% at fitted properties |
| Reliability | First-attempt unlock success rate | ≥ 99% |
| Companion coverage | Multi-guest stays using key share | ≥ 25% |
| Security | Credentials active past check-out | 0 |
| Desk-load relief | Key-related desk interactions per 100 stays (re-keys, second keys) | ↓ 60% |
| Cost line | Keycards issued per 100 stays at fitted properties | ↓ 50% |
| Revenue | Access-based upsell attach (lounge, premium floor, day-pass) | ≥ 5% of stays |
| Fallback quality | Median express card-pickup time (unfitted path) | < 60 seconds |
| Wallet tier | Share of digital keys added to Wallet (where supported) | ≥ 30% |

---

## 9. Dependencies & Risks

| Item | Detail | Mitigation |
|------|--------|------------|
| **Lock hardware (the hard one)** | BLE keys require certified locks; many BD properties run magstripe/offline RFID — retrofit is significant capital | Card-fallback tier keeps every property on-platform; phased retrofit guidance (new wings/floors first); OpenKey-class aggregation widens compatible vendors; retrofit ROI material for owners |
| Lock-vendor integration | Each vendor (Salto, ASSA ABLOY, dormakaba, OpenKey) has its own cloud/SDK, certification cycle, and fees | Integration adapter layer; certify vendors by target-property demand; OpenKey as multi-vendor shortcut |
| Wallet availability | Apple/Google Wallet keys depend on lock-vendor platform partnerships and may lag by vendor/region | In-app BLE key as the universal baseline; wallet as progressive enhancement |
| Trigger reliability | Auto-issue/revoke depends on G2 check-in, F3 room-ready, and PMS check-out events | Conservative trigger states (inspected-ready), staff F1 manual issue/revoke override, alerting on stuck states |
| Phone edge cases | Dead battery, no BLE, companion without a smartphone | Wallet power-reserve where supported; card fallback always one desk (or roving F1 agent) away |
| Security & credential hygiene | Device theft, share abuse, credential replay | Vendor-certified crypto rails only; per-person credentials; instant revocation; share limits and audit; optional companion ID capture |
| Guest trust | "Will the door really open?" anxiety suppresses adoption | First-use walkthrough, ≥99% unlock SLO before scale-out, desk staff able to see key state instantly |
| Data protection | Unlock logs are movement data | Role-gated access, retention policy, use limited to security investigation |
| Lock-system outage | Vendor cloud down blocks new issuance (issued BLE keys keep working offline) | Offline-capable credentials for the stay; card encoder as property-side backstop |

---

## 10. Scope delivered

Atrium Guest ships as one complete package. This feature delivers:

- BLE mobile key via certified lock integrations (Salto, ASSA ABLOY, dormakaba, OpenKey)
- Apple Wallet / lock-screen key where the lock vendor supports it
- Companion key sharing with per-person credentials, limits, audit, and individual revocation
- Entitlement-driven common-door access (floor, gym, pool gate, lounge, parking) with live mid-stay updates
- Auto-issue on room-ready + check-in complete; auto re-scope on room move and stay change; auto-revoke at check-out
- Staff-visible key state and manual issue/revoke controls (surfaced in staff F1)
- Physical-card fallback tier: express card pickup against completed mobile check-in, full coexistence of card and digital keys
- Unlock and lifecycle audit trail
- Bangla/English throughout

---

## 11. Open Questions

- Which lock vendor(s) do the launch properties actually have — and which certification (Salto, ASSA ABLOY, dormakaba, OpenKey) do we sequence first?
- What is the realistic retrofit cost per door in the BD market, and do we produce owner-facing ROI material (or financing partnerships) to accelerate re-locking?
- Wallet key at launch or fast-follow — which target properties' lock vendors support Apple Wallet provisioning today?
- Companion policy defaults: max keys per room, and is companion identity capture required by regulation in target jurisdictions?
- Day-visitor credentials (pool/gym passes via G12) sit outside the current canon (day visitors have no key) — confirm as a post-v1 extension and, if pursued, update the README day-visitor scope.
- Mobile encoders for roving F1 agents at unfitted properties — in the fallback scope or desk-only at launch?

---

## 12. One-Line Business Case

> **Digital Room Key turns the phone into the key and the door into a product — completing the desk-free arrival that mobile check-in only promises, deleting the card-and-encoder logistics chain, and making every gated space a sellable, self-fulfilling upsell — with a physical-card fallback that keeps every property, retrofitted or not, on the same platform.**
