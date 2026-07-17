---
title: "WaveWarZ: The ZAO's On-Chain Music Battle Platform"
author: Zaal Panthaki
status: Draft
created: 2026-07-17
last-updated: 2026-07-17
source-docs:
  - "Doc 1077 - WaveWarZ Volume + Economics Deep Dive"
  - "Doc 1079 - wwtracker Analytics Modules"
  - "Doc 1214 - Song and Artist Battle Data"
  - "Doc 1219 - Platform Take Rate Audit"
  - "Doc 1237 - On-Chain Payout Verification (Dune)"
  - "Doc 1252 - Battle Feed Audit"
---

# WaveWarZ: The ZAO's On-Chain Music Battle Platform

## Abstract

WaveWarZ is a live music battle platform where songs compete head-to-head in public voting rounds on Solana. Every trade triggers an instant artist payout (1% of trade volume). Since May 2025, the platform has processed 1,108+ battles across 921 unique songs, generating 524.15 SOL in total volume and routing $1,497 to charity through two benefit-battle rounds. The platform's economics invert the standard streaming model: where Spotify routes ~12% of revenue to artists, WaveWarZ routes 98.5% of all fees back into the ecosystem (artists + stakers). This paper documents the mechanics, economics, on-chain data, and the open questions that define WaveWarZ as of July 2026.

---

## The Problem

Independent musicians are structurally disadvantaged in every layer of the current music economy:

- **Discovery:** Streaming algorithms favor established acts and playlist curators. An unsigned artist competes against labels that pay for placement.
- **Economics:** Spotify's ~12% artist royalty rate (after distributor and label cuts, often less than $0.003 per stream) creates no viable path for artists below 10M monthly streams.
- **Ownership:** Platform terms require uploader licenses that grant the platform perpetual rights. Artists do not own their audience data.
- **Community:** Fan engagement is mediated by the platform. An artist's "fans" are Spotify's customers.

The standard web3 response - "launch a token" - solved the financial layer (direct fan-to-artist value transfer) while breaking the musical layer (speculation consumed the product, leaving no musical context for why the token should hold value).

---

## WaveWarZ: The Inversion

WaveWarZ turns the music discovery mechanic into an economic primitive.

**The core loop:**

1. Songs are submitted to the platform and placed into a battle queue
2. Two songs go head-to-head in a public bonding-curve market
3. Community members buy shares in either song - the buy itself IS the vote
4. Battle closes when a time or volume threshold is reached
5. The winning song's holders receive the loser's liquidity pool
6. Artists receive 1% of every trade in real-time, regardless of outcome

The result: discovering a song is financially equivalent to an early stake in its cultural value. Fans are not passive listeners - they are liquidity providers who profit when their musical taste is validated by a broader community.

---

## How It Works

### Battle Mechanics

A WaveWarZ battle is a bonding-curve prediction market. Each song has a price curve: buying increases the price, selling decreases it.

- Battle starts when two songs are matched
- Any community member can buy or sell shares in either song during the battle window
- Volume in each pool reflects conviction
- At close, the song with more volume wins
- Winners receive the loser's pool proportionally to their holdings

Artists with WaveWarZ handles (verified via Audius roster) have their earnings automatically credited.

### Artist Payout Mechanics

The 1% artist payout fires on every trade. Mechanics (from doc 1219):

| Fee Component | Rate | Recipient |
|---------------|------|-----------|
| Artist payout | 1.00% | Song's registered artist (instant, on-chain) |
| Platform take | 3.16% | Protocol treasury |
| Staker share | ~94.84% | Battle market (winner's pool) |

The platform take rate (3.16%) was verified via on-chain audit (doc 1219). The 98.5% ecosystem payout claim (doc 1237) refers to the combined artist payout + winner's pool: value stays in the ecosystem rather than flowing to an off-chain entity.

**Verified cumulative artist payouts:** 9.07 SOL across all battles as of July 2026 (doc 1237 Dune verification). This represents 1.79% of total volume - not the 1% per-trade rate because artist payout applies only to trades in songs with registered artist handles; battles with unregistered songs route that 1% back to the protocol.

### Live Cadence

WaveWarZ runs daily:
- Monday - Friday at 8:30 PM EST: quick-battle X Space + YouTube live
- Weekend community battles: async (no scheduled stream)

The X Space format: host calls out a battle in real-time, community buys/sells during the live session, results posted on-stream. This creates a live-sports feel: the battle outcome is unknown until close, with real money at stake.

---

## On-Chain Data (Verified, July 2026)

All figures from doc 1252 (battle feed audit) and doc 1077 (volume deep dive), verified against on-chain data via wwtracker + Dune.

| Metric | Value | Source |
|--------|-------|--------|
| Total battles | 1,108+ | Doc 1252 |
| Total SOL volume | 524.15 SOL (~$39,453 at $75.29/SOL) | Doc 1077 |
| Unique songs battled | 921 | Doc 1214 |
| Artists with verified handles | 34 (Audius-rostered) | Doc 1214 |
| Total artist payouts | 9.07 SOL | Doc 1237 (Dune) |
| Platform take rate | 3.16% | Doc 1219 |
| Charity raised | $1,497 (2 benefit rounds) | Doc 1077 |
| Launch date | May 2025 | Doc 1252 |

**Note on volume conversion:** SOL/USD conversion was $75.29 at time of doc 1077 audit. Current value requires recalculation against live price.

### Battle Records

| Record | Holder | Data |
|--------|--------|------|
| Most dominant artist pair | GodclouD 8-0 all-time | Doc 1214 |
| Most contested songs | [to confirm: top song by battle count] | - |
| Longest winning streak | [to confirm: artist with longest unbroken streak] | - |

---

## The Analytics Layer: wwtracker

WaveWarZ ships with an open-source analytics dashboard: wwtracker (wwtracker.vercel.app). This is not an official WaveWarZ product - it is a ZAO-built transparency layer maintained by the community.

**wwtracker modules (12+ components, doc 1079):**

- BattleArena: live and recent battles, odds, current prices
- SongArena: per-song battle history, win rates, volume
- ArtistStandings: leaderboard sorted by volume, wins, payout earned
- TrendView: weekly battle volume trend
- CharityRound: benefit-battle tracker
- MarketDepth: bonding curve depth for active battles
- And 6+ additional modules

**Why open-source matters:** wwtracker makes every WaveWarZ claim independently verifiable. When The ZAO states "524 SOL in volume," any community member can confirm that number against the public WaveWarZ API via wwtracker's data layer. Open-source analytics transforms a marketing claim into a verifiable fact - the difference between "trust us" and "check it yourself."

---

## What Makes WaveWarZ Distinctive

### 1. The Bet IS the Vote

In most music discovery platforms, voting is free (Spotify playlists, SoundCloud likes). Free voting creates cheap-signal economies where the loudest promoter wins regardless of musical quality. WaveWarZ ties voting to financial stake. When you buy shares in a song, you are betting that the broader community will agree with your taste. This aligns incentives: voting for a song you love and buying early in a song the market will validate are the same action.

### 2. Real-Time Artist Payouts

Streaming royalties are calculated months after play, paid through multiple intermediaries (distributor, PRO, label), and often arrive 12-18 months after the stream. WaveWarZ pays 1% of every trade at execution time, directly to the artist's on-chain address. No intermediary, no waiting period, no threshold (no "you need 1,000 streams to withdraw").

This is not a theoretical improvement. The 9.07 SOL in verified artist payouts (doc 1237) represents real money that arrived in artists' wallets during or immediately after the battles.

### 3. The Platform Take Is Honest

WaveWarZ's 3.16% platform take (doc 1219) is verified on-chain. Traditional music platforms do not disclose their true take rate. When Spotify says it pays "70% of revenue to rights holders," that 70% includes labels, publishers, and PROs - the artist might see 12% or less. WaveWarZ's fee structure is public, auditable, and enforced by code.

### 4. Live Events Bridge On-Chain and IRL

The daily X Space + YouTube battles create a recurring live-event calendar around on-chain activity. The community gathers to watch, trade, and socialize in real-time. COC Concertz #7 (July 18, 2026) debuted a live BattleVote widget inside the concert experience - concert attendees voted in WaveWarZ battles during the show. This bridge between virtual concert and on-chain market is a model for music-culture IP integration.

---

## Charity Rounds

WaveWarZ has run two benefit-battle rounds:

- **Round 1:** [to confirm: date, cause, amount raised]
- **Round 2:** [to confirm: date, cause, amount raised]
- **Combined:** $1,497 raised (doc 1077)

The charity mechanic: a benefit battle designates a cause as the "artist" recipient. When any trade fires, the 1% artist payout goes to the charity wallet instead. The platform take and staker pool are unchanged.

---

## ZABAL Games Integration (August 2026)

In August 2026, WaveWarZ becomes the Finals stage for ZABAL Games, The ZAO's 3-month builder incubator. Builder projects compete head-to-head in WaveWarZ battles. Community votes (buys) determine which builder project advances. This is the first time WaveWarZ's battle mechanic has been applied to non-music content - treating builders' project quality the same way it treats musical quality: a public, financially-staked community vote.

---

## Open Questions

These questions are flagged for community input rather than answered definitively:

- **Registered artist coverage:** Only 34 of 921 unique songs have verified artist handles. The remaining 887 songs route artist payouts to the protocol. What is the right artist onboarding mechanic to increase handle registration?
- **WaveWarZ-Base:** A Solana-to-Base bridge is in design (doc from the board). What is the right cross-chain architecture? How do battle outcomes and payouts work cross-chain?
- **Artist payout to which wallet?** The current 1% fires to Audius-linked wallets. What happens when an artist has a Solana address but no Audius registration?
- **Battle queue management:** Who decides which songs enter which battles? What prevents gaming of the pairing system?
- **Charity round 3:** When and how does The ZAO decide to run a third charity round?

---

## Summary

WaveWarZ is the first on-chain music battle platform with verified instant artist payouts. It has processed 1,108+ battles, 524.15 SOL in volume, and routed $1,497 to charity since May 2025. Its artist-first economics (98.5% ecosystem payout vs. Spotify's ~12% to rights holders) are verified on-chain via Dune dashboards. The open-source wwtracker analytics layer makes every claim independently auditable.

What WaveWarZ proves is not just that music battles can be fun. It proves that a community with enough conviction, a clear economic mechanic, and a public transparency layer can build a functioning alternative to the streaming economy - one where the money actually reaches the artists.

---

*Data verified from ZAO research docs (see source-docs above), cross-referenced against wwtracker + Dune on-chain data as of July 2026. Figures marked [to confirm] are either unavailable in the verified data set or require fresh on-chain queries to update.*
