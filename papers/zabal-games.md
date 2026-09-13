---
title: "ZABAL Gamez: The ZAO's 3-Month Builder Incubator"
author: Zaal Panthaki
status: Draft
created: 2026-07-17
last-updated: 2026-07-17
source-docs:
  - "Doc 1258 - Mid-Season State Audit (July 2026)"
  - "Doc 1255 - WaveWarZ August Battle Protocol"
  - "zabalgames repo CLAUDE.md, data/recaps.json, data/build-days.json, data/finals.json"
---

# ZABAL Gamez: The ZAO's 3-Month Builder Incubator

## Abstract

ZABAL Gamez is The ZAO's 3-month builder incubator, running June through August 2026. It is not a hackathon -- it is a season: structured into three distinct phases (workshops, open build, finals), with 32 people on the roster (organizers, mentors, workshop leads, builders) and 28 workshop sessions completed in June. The finals mechanic is unusual: builder projects compete head-to-head in live WaveWarZ battles in August, where community investment determines the winner. As of July 17, 2026, June workshops are complete, July open build phase has no documented activity, and August Finals require Zaal and Iman to lock the finalist roster and prize pool before August 1. This paper documents the structure, the season-to-date record, and the open questions going into the final month.

---

## The Problem

Hackathons produce code that no one ships. The standard format -- 48 hours, a theme, a prize -- selects for speed over depth. Participants build demos, not products. The winners often have polished presentations rather than usable tools. The community that forms around a hackathon is temporary; when the weekend ends, the builders scatter.

The deeper problem: who is the hackathon for? Most are for the sponsor, who wants demo day PR coverage and a pipeline of developers who might integrate their API. The builders get a small prize and a resume line.

ZABAL Gamez inverts this.

---

## The Inversion: A Season, Not a Sprint

ZABAL Gamez is a 3-month season with three phases, each serving a different function:

| Phase | Month | Function |
|-------|-------|----------|
| Workshops | June | Education and relationship-building; expert speakers teach builders the ZAO ecosystem |
| Open Build | July | Builders apply what they learned; self-directed building with ZAO community support |
| Finals | August | Head-to-head competition; community votes determine the winner via WaveWarZ battles |

The timeline forces depth. A builder who wants to compete in August has had 60+ days to build, iterate, and get feedback -- not 48 hours. The workshop phase means builders enter the open-build month with relationships, context, and skills they did not have before.

---

## Structure

**Platform:** zabalgamez.com (Vercel-hosted, Farcaster Mini App)
**Stack:** Static HTML + inline scripts, 32 Vercel edge functions, Upstash Redis activity backend
**Season:** June 1 -- August 31, 2026 (Season 1)

**Three tracks:**
- **Artist** -- musical and visual creators; outputs include music, visual art, performance
- **Builder** -- developers and aspiring builders; outputs include code, tools, integrations
- **Creator** -- media and distribution specialists; outputs include video, content, community growth

**Roster (as of July 2026):** 32 people across all roles (organizers, mentors, workshop leads, builders)

---

## June Workshop Month: What Ran

June 2026 delivered 28 documented workshop sessions across 6 categories.

| Category | Sessions | Representative Guests |
|----------|----------|----------------------|
| Guest workshops | 16+ | topocount (Neynar), Dylan Yarter (BizarreBeasts), Saltorious (Bankr), Dan Singjoy (Eden Fractal), AZKAL (FlowStage), Ali Tiknazoglu, Teresa Marrin Nakra (Stevens University), Meta Mu (Rose City Web3) |
| Farcaster Batches | 1 | ZABAL Gamez presentation on stage with Empire Builder, Defense of the Agents, Booster, Celebration Hub |
| AMA | 1 | The Farcaster Intern |
| Creator track | 1 | Ohnahji on starting and growing a livestream |
| Tool/protocol sessions | 2 | POIDH open bounty protocol; selling merch onchain |
| Bonfire + ZOL | 2 | Joshua.eth + Plat0x (Bonfire knowledge graph), yerbearserker (Empire Builder + ZAO ecosystem) |

**June ship log highlights (not exhaustive):**
- Farcaster Mini App: self-hosted SDK, capability detection, zero-CDN same-origin loader
- Empire Builder leaderboard live on /leaderboard with zabalgamez01e9af empire integration
- Workshop recordings system: /recordings archive, per-recording Farcaster comment threads
- Daily workshop reminder notifications: Farcaster + cron at 12:00 UTC
- Referral system, clips board, ZAO 2048 game layer, dream-leads demand board

The June phase demonstrates that ZABAL Gamez runs a speaker series with production quality -- not a casual online meetup. The diversity of guests (blockchain devs, music tech researchers, media builders, protocol founders) gives all three tracks substantive content.

---

## July Open Build Month: Current State

July was scheduled with 5 structured build days (July 1-5, featuring Empire Builder, POIDH, Vini App, Bonfire, Eden Fractal/Respect as spotlighted projects). As of July 17, 2026:

- July recaps in data/recaps.json: **0**
- July daily-updates in data/daily-updates.json: **0** (newest entry: June 3, 2026)
- Build day recordings: unknown (no recap entries)

The site is live (zabalgamez.com), the activity backend is connected, and the entry page accepts project registrations. The gap is in documented output: either the July 1-5 sessions ran without recap capture, or the open-build phase paused.

**What is live in July:**
- 2 open bounties on /bounties
- /dream-leads demand board (builders can claim projects)
- /build-ideas community build board (adoptable project ideas)
- 9 intake-ready builder submissions in the QV ballot (verified as of PR #551 in the ZABAL Gamez repo)

---

## August Finals: The WaveWarZ Mechanic

The August Finals are the most distinctive part of ZABAL Gamez. Builder projects do not present on demo day and get judged by a panel. Instead:

**Builders compete head-to-head in WaveWarZ.**

The mechanic (from doc 1255):
1. Finalist builder projects are registered in WaveWarZ as if they were songs
2. Community members buy shares in the project they believe in
3. The project with more volume wins the battle
4. Winner's pool is distributed to holders proportionally
5. Artists (builders in this context) receive 1% of every trade as a direct payout

This means:
- Winning is not about who gives the best presentation -- it is about which project the community is willing to put capital behind
- Builders who attract early believers benefit from a community that profits when their project wins
- The Finals double as a WaveWarZ event: the community engages with both the products and the market

**QV ballot layer (additional scoring):**
Parallel to the WaveWarZ battles, a QV (quadratic voting) ballot gives community members equal-weight votes to allocate across projects. The QV score + WaveWarZ battle outcome combine for the final rankings (exact formula to be confirmed per board task c993bb9c).

**Finals status (as of July 17, 2026):**
- finals.json: `status: 'pending'`, `finalists: []`, `window.starts: null`, `window.settles: null`
- 0 finalists locked
- QV ballot: PR #551 on zabalgames repo, CI-green, awaiting Zaal merge
- WaveWarZ-Base prediction market: separate contract work, status unknown

**What Zaal + Iman must decide before August 1:**
1. Finalist roster: which builders and projects advance to August
2. Prize pool: USDC + $ZABAL amounts per track
3. Battle schedule: when in August do battles run?
4. QV ballot weight: what percentage of final score is WaveWarZ volume vs. QV votes?

---

## What Makes ZABAL Gamez Distinctive

### 1. The Finals Are a Product Test, Not a Presentation

Most incubators culminate in "demo day" -- a presentation to investors and judges. Demo day selects for presentation skill, not product quality. A polished slide deck can outscore a working product in the wrong room.

WaveWarZ battles invert this. The community puts real capital behind the projects it believes in. A project with strong word-of-mouth and genuine utility will attract more buyers than a project with a good pitch. The market is the judge.

### 2. The Workshop Phase Is The Real Network

28 sessions in June mean 28 touchpoints between builders and the ZAO ecosystem. A builder who attended all 28 June sessions has met Neynar's team, BizarreBeasts, Bankr, Eden Fractal, Rose City Web3, Stevens University, and more. That is not a hackathon -- that is a professional network built over a month.

### 3. Three Tracks, One Season

Artist, builder, and creator tracks run simultaneously. A track-crossing project -- say, a builder who makes a tool for music artists, with a creator documenting the build -- fits multiple categories and connects different parts of the community. The interdisciplinary design is intentional: The ZAO is a music-tech-culture DAO, not a single-vertical community.

### 4. Farcaster-Native

ZABAL Gamez runs as a Farcaster Mini App. The zabalgamez.com site is installable from a Farcaster client, sends reminder notifications via Farcaster, and builds community engagement inside the Farcaster social graph. This is not a Web2 incubator with a Web3 prize -- it is built on the stack its audience already uses.

---

## The 9 Intake-Ready Builder Submissions

As of PR #551 (QV ballot, July 2026), 9 builder projects are intake-ready in the system. These are the projects eligible for the August Finals ballot. Project names and details are in data/builder-submissions.json in the ZABAL Gamez repo (not published in this paper to protect builder privacy before the Finals announcement).

---

## Open Questions

- **July open build documentation:** Did the July 1-5 build days run? If yes, add recaps and daily-updates to the data layer.
- **Finalist lock deadline:** Who decides which 9 (or fewer) projects advance, and by what criteria?
- **Prize pool composition:** $ZABAL + USDC -- what are the amounts, and who funds them?
- **WaveWarZ-Base contracts:** The Finals WaveWarZ battles require contract addresses. What is the deployment status?
- **Season 2:** If Season 1 runs through August 2026, does Season 2 begin in 2027? What improvements apply?

---

## Summary

ZABAL Gamez completed a strong June workshop month (28 sessions, expert guests from across the Farcaster/Web3/music ecosystem). July is the open-build phase -- low documented activity as of mid-July, but the site is live and 9 builder projects are intake-ready. August is the Finals: builders compete head-to-head in WaveWarZ battles, with QV ballot scoring alongside. The Finals have never run before -- ZABAL Gamez Season 1 will be the first time a builder incubator used an on-chain music battle platform as its judging mechanism. The decisions needed before August 1: finalist roster, prize pool, and QV ballot weight. Once those are locked, the August Finals are a new model for how builder competitions can work.

---

*Data verified from zabalgames repo and ZAO research docs as of July 17, 2026. Items marked [to confirm] require fresh verification or Zaal/Iman decision input.*
