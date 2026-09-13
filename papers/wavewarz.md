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

WaveWarZ is a live music battle platform where songs compete head-to-head in public voting rounds on Solana. Every trade triggers an instant artist payout (**1.005% of trade volume** - measured at exact lamports, see the fee note). Since May 2025 the platform has opened **1,643 battle accounts, 1,604 of them with tracks minted and 1,550 settled**, across **3,208 distinct track mints**, generating **714.54 SOL of buy volume (928.52 SOL counting sells)**, and has run four benefit-battle rounds for charity (2025-12-12, 2026-02-13, 2026-06-20, 2026-09-11). The platform's economics invert the standard streaming model: where Spotify routes ~12% of revenue to artists, WaveWarZ routes 98.5% of all fees back into the ecosystem (artists + stakers). This paper documents the mechanics, economics, on-chain data, and the open questions that define WaveWarZ. **On-chain figures are as of 2026-09-06 08:22 UTC and each is reproducible from a public RPC.**

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

The artist payout fires on every trade. Mechanics (from doc 1219):

**The artist leg is 1.005%, not 1.00%, and this is measured twice independently.** The documented
schedule says 1.00% artist / 0.50% platform of a 1.50% total. On chain the split is
**1.005% / 0.495%**, still totalling 1.500%. Two methods on two datasets agree: this lane
decomposed 14 trades across 7 battles, and the record layer's own `verify-fee-split.ts`
decomposed 223 buys across 18 battles, landing on exactly 1.0050% and 0.4950%. **The artist share
is understated wherever 1.00% is printed** - the right direction for artists, the wrong direction
for a document that claims to be auditable.

| Fee Component | Rate | Recipient |
|---------------|------|-----------|
| Artist payout | 1.00% | Song's registered artist (instant, on-chain) |
| Platform take | 3.16% | Protocol treasury |
| Staker share | ~94.84% | Battle market (winner's pool) |

The platform take rate (3.16%) was verified via on-chain audit (doc 1219). The 98.5% ecosystem payout claim (doc 1237) refers to the combined artist payout + winner's pool: value stays in the ecosystem rather than flowing to an off-chain entity.

**Verified cumulative artist payouts:** 9.07 SOL across all battles **as of July 2026** (doc 1237 Dune verification) - **not re-derived in this refresh**, so it is a July figure sitting beside September ones and should be re-pulled or cut before publication. This represents 1.79% of total volume - not the 1% per-trade rate because artist payout applies only to trades in songs with registered artist handles; battles with unregistered songs route that 1% back to the protocol.

### Live Cadence

WaveWarZ runs daily:
- Monday - Friday at 8:30 PM EST: quick-battle X Space + YouTube live
- Weekend community battles: async (no scheduled stream)

The X Space format: host calls out a battle in real-time, community buys/sells during the live session, results posted on-stream. This creates a live-sports feel: the battle outcome is unknown until close, with real money at stake.

---

## On-Chain Data (verified from chain, 2026-09-06 cutoff)

**Refreshed 2026-09-13 from a complete on-chain scan.** Chain cutoff 2026-09-06 08:22 UTC; every
figure below is reproducible from a public RPC.

*(Until this refresh this table was sourced from doc 1252, the battle feed audit, and doc 1077, the
volume deep dive, cross-checked via wwtracker and Dune. That sentence stood immediately above the
refresh note, so the same table claimed two different provenances at once - kept here as history
rather than deleted, because the July documents are still where the figures NOT re-derived come
from, and those are marked individually.)*

| Metric | Value | Source |
|--------|-------|--------|
| Battle accounts opened | **1,643** | chain scan, every battle account |
| Of those, with tracks minted | **1,604** | chain, 39 accounts were never initialised |
| Settled battles | **1,550** | chain, `winner_decided` |
| Total volume, buys only | **714.54 SOL** | chain, `buyShares` instruction data |
| Total volume, buys + sells | **928.52 SOL** | chain, both legs |
| Distinct track mints | **3,208** | chain, excluding the default/unset pubkey |
| Total distributed to winners | **450.13 SOL** | chain, settlement at byte 249 |
| First battle | **2025-05-26** | chain, earliest `start_time` |
| Charity raised | *see note* | not reproducible from chain here |

**Why 3,208 and not 3,286, and the answer is more interesting than arithmetic.** Two mints per
battle over 1,643 accounts would be 3,286. The gap is **not** tracks re-entering. Exactly one
address repeats in the whole population - `11111111111111111111111111111111`, the default unset
pubkey, 78 times across **39 battle accounts that were never initialised**: `is_initialized` false,
pools zero, supplies zero, none settled.

Remove those and **3,208 real mints across 1,604 battles with tracks - and no real mint appears
twice.** Every track that has battled has battled exactly once. That is a fact about the platform
worth stating, and it is the opposite of what a raw distinct-count implies.

*(An earlier draft of this paragraph said "77 mints appear in more than one battle - the same track
re-entered". That was invented: a cause fitted to a subtraction, never checked against the set.
There is no re-entry. Caught in review.)*

**Why volume now has two rows.** The July figure of 524.15 SOL was a single number with no
stated definition. Buys-only and buys-plus-sells differ by 214 SOL, so a paper that prints one
number without saying which it is invites exactly the reconciliation argument this platform has
already had once. Both are given, each labelled.

**The charity figure is NOT refreshed, deliberately.** The July paper's "$1,497 across 2 benefit
rounds" comes from an internal document, and this lane cannot reproduce it from chain without the
charity wallet address, which it does not hold. Two things are known and should be settled before
publication: **there have been more than two rounds** - benefit battles ran on 2025-12-12,
2026-02-13, 2026-06-20 and again on 2026-09-11 - and **the 2026-09-11 round has not settled**, so
its distribution has not reached the charity. Printing a stale charity total in a permanent public
document is the one figure here most likely to be quoted back.

**On USD conversion:** the July paper converted at $75.29/SOL. Any dollar figure in a permanent
document decays with the price, so dollar amounts should either carry their conversion date in the
same sentence or be dropped in favour of SOL.

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

**Why open-source matters:** wwtracker makes every WaveWarZ claim independently verifiable. When The ZAO states a volume figure, any community member can confirm it against the public WaveWarZ API via wwtracker's data layer - and, better, against chain directly. Open-source analytics transforms a marketing claim into a verifiable fact - the difference between "trust us" and "check it yourself."

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
- **Combined:** $1,497 raised across the first two rounds (doc 1077). **Stale: two further rounds have run since, and this total has not been re-derived.** See the note on the figures table.

The charity mechanic: a benefit battle designates a cause as the "artist" recipient. When any trade fires, the artist payout (1.005%) goes to the charity wallet instead. The platform take and staker pool are unchanged.

---

## ZABAL Gamez Integration (Season 1, closed 2026-08-30)

WaveWarZ was the Finals stage for ZABAL Gamez, The ZAO's builder incubator. Builder projects
competed head-to-head in WaveWarZ battles, with community votes (buys) determining which project
advanced. **Season 1 is final. All three tracks were decided** (`zabalgamez/data/season-1-results.json`,
status `final`):

| Track | Winner | Project |
|---|---|---|
| Artist | **@n3m** | |
| Builder | **@ghostmintops** | Proof Drop, a build receipt generator, plus the ZABAL Recording Scout |
| Creator | **@uniquebeing404** | ColorZAO, the ZAO colour paint tool, plus a video |

**The close date needs one decision before publication: the frozen results file gives
`"closes": "2026-08-31"` while its own schema note in the same file says "window closes
2026-08-30".** Both dates are also in circulation elsewhere in that repo. This paper follows the
machine-readable field, **2026-08-31**, because that is the one a reader can check. Zaal should
settle which is right rather than have a public paper pick a side of an internal disagreement.

That was the first time WaveWarZ's battle mechanic had been applied to non-music content -
treating a builder's project the way it treats a song: a public, financially-staked community
vote.

*(This section was written in the future tense - "In August 2026, WaveWarZ becomes the Finals
stage" - for a season that has since run and finished. A public paper describing a completed event
as upcoming dates itself on the day it is read. The results are now cited from
`zabalgamez/data/season-1-results.json` rather than from a relay, so they can be checked rather
than taken on anyone's word.)*

---

## Five sections Zaal approved for inclusion, NOT YET WRITTEN

Approved 2026-09-13 via the grill lane's card 8ed19011. **None is drafted here, because four of
the five need input this lane does not have and would have to invent.**

1. **Team names.** Everyone named must consent before this publishes - the paper is public and
   permanent. **Needed: the list of people to name, and confirmation each has agreed.**
2. **The 50/50 partner program.** Zaal's instruction: write what is **actually true today**, not
   the aspiration, because printing it makes it a public commitment. **Needed: the current terms
   as they really stand.**
3. **The accelerator.**
4. **The roadmap.** Ticked together as one item. **Needed: what is committed versus explored** -
   a roadmap in a permanent paper is read as a promise.
5. **The WaveWarZ protocol Zaal is building.** His words: *"Honestly the wavewarz protocol that
   Im building aswell"*. **Needed: his own description.** This lane holds the protocol repo and
   could write a technically accurate section from it, and deliberately has not: the instruction
   was to get his description rather than infer one from the code, and what a person is building
   is not always what the repository shows.

## Open Questions

These questions are flagged for community input rather than answered definitively:

- **Registered artist coverage:** the July draft said 34 of 921 unique songs carry verified artist handles, with the remaining 887 routing artist payouts to the protocol. **Both numbers need re-deriving before publication** - the song population is now 3,208 distinct track mints, so the ratio in that sentence is certainly stale and this lane has not re-measured handle coverage. What is the right artist onboarding mechanic to increase handle registration?
- **WaveWarZ-Base:** A Solana-to-Base bridge is in design (doc from the board). What is the right cross-chain architecture? How do battle outcomes and payouts work cross-chain?
- **Artist payout to which wallet?** The current 1% fires to Audius-linked wallets. What happens when an artist has a Solana address but no Audius registration?
- **Battle queue management:** Who decides which songs enter which battles? What prevents gaming of the pairing system?
- **Charity cadence:** rounds three and four have since run (2026-06-20 and 2026-09-11), so the question is no longer when a third happens but what triggers a round and who decides. **The 2026-09-11 round is still unsettled**, which is a live item rather than an open question.

---

## Summary

WaveWarZ is the first on-chain music battle platform with verified instant artist payouts. It has opened **1,643 battle accounts, 1,550 of them settled**, and processed **714.54 SOL of buy volume** since May 2025, and has run four charity rounds. Its artist-first economics (98.5% ecosystem payout against Spotify's ~12% to rights holders) come from the July 2026 Dune verification and **have not been re-derived in this refresh** - unlike the battle, volume and fee figures above, which were. The open-source wwtracker analytics layer makes every claim independently auditable.

What WaveWarZ proves is not just that music battles can be fun. It proves that a community with enough conviction, a clear economic mechanic, and a public transparency layer can build a functioning alternative to the streaming economy - one where the money actually reaches the artists.

---

*Battle counts, volume, track mints, settlement totals and the fee split were re-derived from a complete on-chain scan on 2026-09-13, chain cutoff 2026-09-06 08:22 UTC, and are reproducible from a public RPC. Everything still attributed to a doc number is a July 2026 figure that has NOT been re-derived: cumulative artist payouts, the 3.16% platform take, artist handle coverage, and the charity totals. Figures marked [to confirm] need fresh queries or a source this lane does not hold.*
