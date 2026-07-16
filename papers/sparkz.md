---
title: "Sparkz: Configurable Creator-Coin Launcher with AI Advisor"
author: Zaal Panthaki
status: Draft
created: 2026-07-16
last-updated: 2026-07-16
source-docs:
  - "Doc 1098 - Sparkz Master Brief"
  - "Doc 1088 - Sparkz Token & Crowdfund"
  - "Doc 1108 - Legal Shape (Memocoin vs Utility)"
  - "Doc 1132 - Zooster/Boostr Pilot"
---

# Sparkz: Configurable Creator-Coin Launcher with AI Advisor

## The Problem

The standard creator-token workflow is backwards: launch a token, then hope a community shows up.

This approach has failed repeatedly. The pattern: a creator drops a new token on Clanker or Pump.fun with generic defaults. The token pumps 20x in day one on pure hype. Then: no use case, no community reason to hold, no ongoing energy toward the creator's actual work. The token becomes a casino, the creator becomes a vanity project, and the community disappears.

Jesse Pollak (Farcaster/Clanker) acknowledged this inversion in early 2026: launching tokens without real creator engagement is not community building; it is speculation enabled by frictionless tech. The Farcaster ecosystem's conversation shifted mid-year toward "what actually keeps a creator-coin community alive?"

Sparkz inverts the sequence.

## The Inversion: Energy First, Token Last

**Sparkz is an energy-measured coordination mechanism that launches a token only after a creator has proven sustained engagement.**

The sequence:

1. **Creator builds a leaderboard** (energy measurement)
   - Supporters "back the album" - they vote for the creator's work over weeks or months
   - Voting is free. No capital required.
   - The leaderboard is public and ongoing - it measures real preference, not speculation.

2. **Energy threshold is reached** (community is real)
   - After N weeks of consistent high-energy backing, the creator has proof: a real community prefers them.
   - This is not a casino signal; it is a signal of actual support.

3. **Creator designs the token** (on creator terms)
   - Treasury split: who shares in the revenue? (50% creator / 30% community treasury / 20% studio production fund is the smart default)
   - Governance of ZAO's locked stake: does the creator want input from ZAO token-holders? (hybrid/creator-controlled/ZAO-weighted options)
   - Utility menu: what do token holders get? (request-a-song, gated early access, influence with veto, credit/recognition, treasury rewards, cross-utility with other tokens)
   - AI advisor: if the creator is unsure, the AI recommends based on their situation.

4. **Token launches** (with community already engaged)
   - The token is not the start of the relationship; it is the formalization of an existing community.
   - By this point, the creator knows what their supporters want.
   - The community knows the creator is serious.

## The Core Mechanism: 0xSplits for Fee-Splitting

The technical spine of Sparkz is **0xSplits contracts** for fee routing.

### Why 0xSplits

0xSplits allows a Sparkz creator token to automatically split revenues across an arbitrary number of recipients. This is load-bearing for Sparkz for three reasons:

1. **Splits are adjustable after launch**
   - Clanker's rewardBps (reward basis points) are immutable once the token is created (as of 2026). If a creator wants to change the split later (add a collaborator, increase the community treasury, adjust for changed circumstances), they cannot.
   - A 0xSplits contract can be updated by the owner. A creator can re-balance splits without redeploying the token. This turns creator-coin governance from "locked and sorry" to "actually responsive."

2. **Music collabs work natively**
   - A band token paid to a 0xSplits contract with members as recipients means revenue is split automatically to every band member. No manual accounting, no risk of someone forgetting to cut a check.
   - This is unique to Sparkz. Clanker treats bands as single addresses; 0xSplits treats them as organized collectives.

3. **ZAO community stake integrates as just another split recipient**
   - The ZAO is considering holding ~25% of each creator's token as a locked incubator stake (open research - see "Open Questions" below).
   - If this path is chosen, the 0xSplits contract handles it: ZAO's receiver address gets its percentage, no special code needed.
   - If the ZAO decides later to exit the incubator model, the split is removed. No token redeployment.

### How It Works (Technical)

```
Creator earns revenue (trading fees, tipping, royalties)
    ↓
Revenue flows to 0xSplits contract
    ↓
Contract checks recipient list:
  - 50% to Creator wallet
  - 30% to Community treasury (multisig)
  - 20% to Studio production fund
  - [optional] 25% to ZAO locked stake wallet
    ↓
Each recipient claims their percentage
```

The recipient list is stored on-chain and can be updated by the contract owner (the creator or their team).

## Sparkz Architecture: Configurable Launcher + AI Advisor

### Core Product

**Sparkz is a configurable creator-coin launcher** sitting on top of Clanker or Empire Builder rails on Base or Solana.

A creator answers a questionnaire (or uses smart defaults) to specify:

| Dimension | Smart Default | Options | Who Decides |
|-----------|----------------|---------|------------|
| **Fee split** | 50/30/20 (Creator/Treasury/Studio) | Fully adjustable | Creator |
| **Treasury recipient** | ZAO-operated multisig | Any Ethereum address or multisig | Creator |
| **ZAO stake governance** | Hybrid: ZAO vote on fund use, creator owns token governance | Creator-only / ZAO-only / Hybrid | Creator |
| **Utility menu** | Iman's set (6 utilities) | Pick-and-choose from menu | Creator |
| **The word** | "back the album" | Any phrasing | Creator |
| **Launch rail** | [to confirm: Clanker or Empire Builder] | [to confirm: available options] | Creator |

**AI Advisor:** If the creator says "I'm not sure what governance model is right for me" or "what split should a band use?", the Sparkz AI advisor makes a recommendation based on:
- Creator profile (solo artist vs band vs label)
- Community size (small/large)
- Revenue model (primary: trading fees vs tipping vs royalties)
- Prior decisions in the ZAO (if any)

The recommendation is transparent: "Here's why this split works for 2-person bands." The creator retains full control to override.

### Stages

Sparkz launches in three stages:

1. **Dogfood (internal ZAO)**
   - Iman launches $IMAN using Sparkz config tools on Base testnet
   - ZAO members test the questionnaire and AI recommendations
   - Feedback on whether the defaults are actually useful
   - Deployment: early August 2026 [open: exact date]

2. **Shared app** (ZAO+Clanker)
   - Sparkz is deployed as a landing page where creators can configure, preview, and launch
   - Powered by Clanker or Empire Builder under the hood
   - ZAO brand is present but not required; creators from outside ZAO can use Sparkz
   - Metrics tracked: launches per week, creator satisfaction, average split configurations chosen

3. **Launch** (public)
   - Sparkz is promoted as the canonical "energy-first" token launcher
   - Positioned as "for creators who want to build community before launching tokens"
   - [open: whether this is a separate domain, embedded in Clanker UI, or standalone app]

## The Language: "Back the Album" Not "Buy the Token"

By design, Sparkz does not use casino language.

- NOT: "buy $CREATORTOKEN"
- NOT: "pump the chart"
- NOT: "early access to coin"

Instead:

- YES: "back the album"
- YES: "support the creator"
- YES: "join the community"
- YES: "coordinate as a group"

This is deliberate framing. The token is a tool for coordination, not speculation. The energy leaderboard is the primary interface; the token is secondary.

This language choice is grounded in The ZAO's positioning (Doc 1098 Grill #2): "We do coordination mechanisms, not casinos. Tokens are infrastructure. The story is about the creator's work, not the chart."

## Open Questions (Being Worked In Public)

The following questions are actively being researched and are open to community input:

### Business Model [open]

**Question:** How does Sparkz make money?

**Current options being explored:**
- Small platform fee on launches (1-3% of creator's trading fees)
- Subscription model: creators pay monthly for AI advisor access
- Revenue-share: ZAO takes a percentage of community treasury distributions
- Freemium: basic launcher is free, advanced governance configs require upgrade

**Status:** Not decided. Iman and Zaal are researching which aligns with ZAO's non-extractive values. See Doc 1098 section "Business Model" for ongoing notes.

### What Concretely Measures "Energy"? [open]

**Question:** The "energy-first" inversion requires a clear definition of energy. What is it?

**Current hypothesis:** Energy = frequency + consistency + size of backing over time.

```
Energy score = count of unique supporters + average backing size + weeks uninterrupted
```

But the exact formula is not locked.

**Why it matters:** If energy is poorly defined, creators can game it (fake supporters, artificial volume). The definition must be defensible.

**Status:** Zaal and Iman are researching leaderboard mechanics. See Doc 1098 section "Leaderboard Design" for the current proposal.

### ZAO Locked Stake: Legal Shape [open]

**Question:** Can the ZAO hold a locked ~25% of each creator's token as an incubator stake?

**The inversion hypothesis:** Instead of taking a fee on token sales (extractive), the ZAO buys in as a stakeholder. If the creator succeeds, the ZAO succeeds.

**Blockers:**
- Is this an investment (requiring securities registration)?
- Is the token itself a memecoin (no promises) or a utility token (triggering regulations)?
- What does "locked" mean legally? (non-transferable? non-voting? both?)

**Status:** Under active legal review with Greg Autonomous (Doc 951 - ZAO's web3 legal counsel). Expect preliminary answer by late August 2026.

**Current assumption:** "Memocoin" framing (no promises to token holders) is legally safer than "utility token" (which implies the creator is making performance promises). But this is [to confirm] with counsel.

### First Launch Sequencing [open]

**Question:** Who should be the first creator to launch on Sparkz post-dogfood?

**Considerations:**
- Should it be Iman (trustworthy, known to ZAO community)? Or an unknown creator (real-world test)?
- Should the first launch be on Solana (WaveWarZ scale) or Base (Clanker scale)?
- What defines success for the first launch? (1,000 backers? $10k volume? sustained energy over 3 months?)

**Status:** Not decided. Zaal is gathering input from Iman, founders of Clanker/Empire Builder, and early Sparkz advisors.

## Grounded Design Choices

Every major Sparkz design choice is grounded in a specific problem or precedent:

| Choice | Why | Source |
|--------|-----|--------|
| 0xSplits for fee routing | Immutable Clanker rewardBps prevent later adjustments; band collabs need auto-split | Doc 1108 - Iman's catch |
| Energy leaderboard before token | Jesse Pollak acknowledged launch-then-community fails | Doc 1137 - Farcaster mid-2026 pivot |
| AI advisor + smart defaults | Creators are often unsure about governance; smart defaults reduce decision fatigue | Doc 1098 - UX research |
| Hybrid governance default | Creator autonomy matters; ZAO stake should not be a veto | Memory: Grill 2026-07-15 |
| "back the album" language | ZAO is coordination, not speculation; language should reflect mission | Doc 1098 Grill #2 |
| Three-stage rollout | Dogfood + shared app + public reduces risk; each stage has metrics | Doc 1098 section "Stages" |

## The Honest Open Questions

Sparkz does not claim to be finished. It is being worked in public.

The questions above ([open] tags throughout) are not afterthoughts. They are central to whether Sparkz works.

If you read this paper and think "that business model doesn't make sense" or "energy is easy to game," you are right to raise it. The ZAO is working these out. Future versions of this paper (and future ZIPs) will document the answers.

## How to Engage

- **Comment on design:** If you have thoughts on the splits, governance, or utility menu, ping Zaal or Iman.
- **Contribute research:** If you are familiar with Clanker/Empire Builder internals, token legal frameworks, or leaderboard design, the ZAO welcomes collaborators.
- **Build pilots:** Interested in launching a Sparkz-prototype on testnet? Reach out.

This is a research document in active motion. Feedback is the design material.

---

## Copyright

This document is released under CC-BY-4.0. Referenced code and contracts retain their original licenses.

**Last updated:** 2026-07-16  
**Status:** Draft  
**Next steps:** Legal review of stake mechanics (Greg Autonomous), community feedback on business model, Iman dogfood launch early August
