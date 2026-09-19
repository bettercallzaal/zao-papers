---
title: "COC Concertz: The ZAO's Virtual Concert Series and Archive Protocol"
author: Zaal Panthaki
status: Draft
created: 2026-07-17
last-updated: 2026-07-17
source-docs:
  - "Doc 1256 - COC Concertz Full Series Record (Shows 1-7)"
  - "Doc 1210 - COC #7 WaveWarZ Pilot Architecture"
  - "CoCConcertZ codebase (cocconcertz.com)"
---

# COC Concertz: The ZAO's Virtual Concert Series and Archive Protocol

## Abstract

COC Concertz is a live virtual concert series co-produced by The ZAO and Community of Communities (CoC). Seven shows ran between March 2025 and July 2026, all hosted inside the "Dope Stilo Music Club" space on Spatial.io and simulcast free to any viewer on Twitch at @bettercallzaal. Every show produces two types of permanent artifacts: fan gallery uploads archived to Arweave under UDL (Universal Data License) presets, and Firestore recaps retained for 7 days post-show. The July 2026 show (COC #7) debuted a live WaveWarZ battle voting widget inside the concert experience -- the first time The ZAO's two flagship IP properties operated in the same event.

---

## The Problem

Independent music events disappear. A livestream ends and the recording is buried inside a platform that owns the URL, can change the embed policy, and may shut down entirely. Fan contributions -- art, photos, comments -- are stored in the platform's database without the creator's ownership.

The model also breaks across geography. A virtual concert hosted purely on Twitch or YouTube requires an established streaming audience to build anticipation. Without the infrastructure to capture fan contributions in a lasting way, each show starts from scratch: no compounding asset, no proof that the community gathered, no artifacts that persist beyond the algorithm.

---

## What COC Concertz Is

**COC Concertz** is a recurring virtual concert series with a permanent fan archive protocol.

Three components define the model:

**1. Live virtual venue (Spatial.io):** The "Dope Stilo Music Club" is a recurring virtual room. The same space hosts every show -- it is The ZAO's permanent metaverse venue, not a one-time event page. Attendees access from any browser.

**2. Public simulcast (Twitch):** Every show streams live at @bettercallzaal. Free to any viewer worldwide. No ticket, no registration, no token required.

**3. Permanent fan archive (Arweave + UDL):** During each show, attendees can upload fan content (art, photos, screenshots) to the archive at cocconcertz.com. Uploads flow through Cloudinary (processing) to Arweave (permanent storage). Each upload carries a UDL license preset selected by the fan:

| UDL Preset | Commercial Use | Derivatives | Attribution |
|------------|----------------|-------------|-------------|
| community-share | No | OK | Required |
| collectible | No | No | Required |
| premium | OK | No | Required |
| open | OK | OK | Not required |

The UDL license is immutable once written to Arweave. The fan owns their contribution; the platform cannot change the terms retroactively.

---

## Full Show Record (Verified July 2026)

| # | Date | Title | Artists / Notes |
|---|------|-------|-----------------|
| 1 | Mar 29, 2025 | First COC Concertz | AttaBotty + Clejan -- first ZAO metaverse concert |
| 2 | Oct 11, 2025 | Second installment | Tom Fellenz / Stilo / AttaBotty -- larger audience |
| 3 | Mar 7, 2026 | Third StiloWorld show | Duo Do / Joseph Goats / Stilo -- monthly cadence begins |
| 4 | Apr 11, 2026 | The Rebrand Show | Joseph Goats, Tom Fellenz, Stilo World |
| 5 | May 9, 2026 | A Day in the Life of GodCloud | GodCloud headlines -- first WaveWarZ artist headline |
| 6 | Jun 13, 2026 | The African Experience | ZAO-Africa themed lineup |
| 7 | Jul 18, 2026 | WaveWarZ Takeover | DJ Zaal + WaveWarZ battle-circuit artists; live BattleVote widget; open-access pilot |

Data sourced from: `src/app/brand/page.tsx` and `src/components/home/PastShows.tsx` in the CoCConcertZ codebase.

---

## The Monthly Cadence

COC #1 (Mar 2025) and COC #2 (Oct 2025) were one-off events separated by 6 months. Starting with COC #3 (Mar 2026), the series shifted to a monthly cadence:

- March 2026: COC #3
- April 2026: COC #4
- May 2026: COC #5
- June 2026: COC #6
- July 2026: COC #7

Five consecutive monthly shows. This is the cadence COC Concertz runs at during active seasons.

---

## The Archive and IP Protocol

Every COC Concertz show produces onchain artifacts. This is the architectural difference between COC Concertz and any other virtual concert.

**What a show produces:**

1. **Fan gallery uploads** -- uploaded during the show window, stored permanently on Arweave with UDL licenses. These are owned by the fans who uploaded them, with terms they selected at upload time.

2. **Show recaps** -- stored in Firestore `recaps` collection (indexed by event ID). Includes: visitor count, chat messages, artists list, generated-at timestamp. Visible on cocconcertz.com for 7 days post-show, then archived.

**Why this matters for IP:**

Most virtual concerts leave no onchain trail. The recording exists on YouTube under YouTube's terms. Fan art lives in Discord. Neither is permanent or owned by the creator/fan.

COC Concertz's Arweave-first approach means:
- Fan contributions are permanent (Arweave data does not disappear)
- License terms are immutable (UDL preset written at upload time)
- The archive is a community-owned asset, not a platform-owned database

---

## COC #7: The Open-Access Pilot

COC #7 (July 18, 2026) introduced two changes from prior shows:

**1. Wallet gate removed:** Previous shows required a ZABAL token to access the fan archive. COC #7 dropped this requirement (`NEXT_PUBLIC_WALLET_GATE_ENABLED=false`). Anyone can upload to the fan archive without holding a token. The pilot measures whether open access brings in meaningfully more audience than the gated model.

**Pilot metrics captured** (at `/api/metrics/coc7`):
- Concurrent viewers
- Wallet-connected vs. not-connected split
- Archive uploads
- Pilot status flag

The decision whether to keep open access depends on the ratio of wallet-connected to non-connected users. If more than 50% of uploads come from non-connected wallets, the pilot is evidence that the gate was excluding a meaningful audience.

**2. Live WaveWarZ BattleVote widget:** A real-time voting panel embedded during the show. Concert attendees vote in live WaveWarZ battles from within the concert page. This is the first integration between The ZAO's two flagship IPs: the WaveWarZ community (battle traders) and the COC Concertz audience (virtual concertgoers) occupied the same event simultaneously.

---

## What Makes COC Concertz Distinctive

**1. The permanent archive model.** No other virtual concert series uses Arweave to archive fan gallery contributions under user-selected UDL licenses. The archive grows with each show and compounds over time. A fan who uploaded to COC #1 in March 2025 still owns that artifact; it cannot be deleted or relicensed.

**2. The same venue, every show.** The "Dope Stilo Music Club" on Spatial.io is a persistent space, not a one-time event URL. Returning audience members enter the same room. This builds spatial familiarity and social continuity between shows.

**3. Cross-IP integration.** COC #7's WaveWarZ integration is a template: a live music event where concert attendance and on-chain market participation happen in the same session. The feedback loop -- audiences learn about WaveWarZ at concerts; WaveWarZ traders attend COC for live music -- grows both communities simultaneously.

**4. Monthly cadence creates a content calendar.** Five monthly shows in 2026 means five recurring touchpoints per year for The ZAO's community, artists, and global audience. Each show generates: a Twitch recording, a Firestore recap, and a set of fan-uploaded Arweave artifacts. The compounding content layer is a moat that single-show events cannot replicate.

---

## 7 Citable Facts (for GEO, press, grants)

1. Seven live virtual concerts co-produced by The ZAO and Community of Communities, March 2025 - July 2026.
2. Spatial.io "Dope Stilo Music Club" is the recurring ZAO virtual venue -- same room across all 7 shows.
3. Monthly cadence since March 2026: 5 consecutive monthly shows (COC #3 through #7) with no gap.
4. All shows simulcast on Twitch at @bettercallzaal, free to any viewer worldwide.
5. Fan gallery uploads archived to Arweave with UDL licenses from every show -- permanent onchain IP record.
6. COC #7 (Jul 18, 2026) is the first open-access pilot: wallet gate removed; anyone can upload to the fan archive without holding ZABAL token.
7. Live WaveWarZ battle voting debuted at COC #7: real-time BattleVote widget turns concert attendees into WaveWarZ voters, bridging The ZAO's two flagship IPs.

---

## Open Questions

- **Wallet gate decision:** Should COC #8+ retain open access, re-enable the gate, or introduce a middle path (open viewing, gated archive uploads)?
- **Spatial.io vs. alternatives:** If Spatial.io discontinues or changes pricing, what is the fallback virtual venue?
- **Archive sustainability:** Arweave uploads require AR token. Who pays for storage long-term? Is there a community treasury allocation for archive costs?
- **COC #8 date and lineup:** [to confirm: whether the monthly cadence continues in August 2026]

---

## Summary

COC Concertz is 7 shows across 16 months, a permanent fan archive on Arweave, and a monthly cadence that has not missed a month since March 2026. The series proves that a small, focused community can run a recurring virtual music event with onchain artifact production -- not as a one-off experiment, but as a sustainable cultural institution. COC #7's open-access pilot and WaveWarZ integration mark the beginning of the next chapter: The ZAO's IPs are no longer separate products but interconnected surfaces in the same ecosystem.

---

*All data verified from CoCConcertZ codebase and ZAO research docs as of July 17, 2026. Items marked [to confirm] require fresh verification.*
