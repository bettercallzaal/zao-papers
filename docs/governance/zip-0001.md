---
zip: 1
title: The ZAO Framework
author: Zaal
status: Draft
created: 2026-07-15
last-updated: 2026-07-15
discusses: https://github.com/bettercallzaal/zao-papers/issues/1
---

# ZIP-1: The ZAO Framework

## Abstract

This ZIP documents the foundational governance architecture, economic model, organizational structure, and agent systems of The ZAO as they exist as of July 2026. It establishes the canonical definitions of: The ZAO's mission and philosophy; the Fractal weekly consensus game and Respect token system; on-chain governance via ORDAO/OREC; the portfolio of branded products; the monorepo-as-lab development model; and the autonomous agent layer. This is a descriptive ZIP - it captures the framework AS-IS. Future ZIPs amend specific components or introduce new policies.

## Motivation

The ZAO has operated for 101+ weeks with a coherent but undocumented governance model. Community members, new contributors, and external partners need a single source of truth that explains how The ZAO decides, distributes resources, and evolves. This ZIP codifies that framework - not to lock it in stone, but to make it visible, verifiable, and amendable through the ZIP process.

## Specification

### 1. What The ZAO Is

**The ZAO** is a music and culture distributed autonomous organization (DAO) based on the principle of contribution over capital. The community is organized around these core beliefs:

- **Artists own their profit, data, and intellectual property.** The ZAO is infrastructure, not gatekeeper.
- **Contribution is the measure of value.** Respect (reputation) is earned through peer evaluation, not purchased.
- **Build in public.** Transparency is a feature, not a policy.
- **Weekly accountability.** The Fractal weekly consensus game keeps the community aligned.

**Membership:** Open to anyone who participates. As of July 2026, 156 unique holders of Respect tokens represent active community members. The community began in 2024 and has operated continuously through weekly Fractal sessions.

**Location:** Primarily Discord (private community server) with on-chain governance on Optimism OP Mainnet.

### 2. Governance: The Fractal System

The ZAO's governance is built on the **Fractal** - a weekly peer-ranked consensus game pioneered by Dan Larimer's Fractally theory and adapted for music/culture context.

#### 2.1 The Weekly Game

Every Monday at 6pm EST, The ZAO runs a Fractal session:

1. **Gathering** - Members join Discord voice
2. **Randomization** - Members are split randomly into 3-6 person breakout groups
3. **Presentations** - Each member describes their contributions that week (code, mentorship, research, music, events, etc.) in ~4 minutes per person
4. **Consensus Ranking** - The breakout group collectively ranks who contributed most to least that week using elimination voting (highest ranked first, then second, etc.)
5. **Respect Distribution** - Based on rank, members receive Respect points via a Fibonacci curve (see section 2.3)
6. **On-Chain Submission** - Rankings are submitted to the OREC governance contract on Optimism, which triggers a 72-hour vote + 72-hour veto period

#### 2.2 Fractal History

- **Started:** July 30, 2024 (Fractal session #1)
- **Weeks unbroken:** ~101 as of mid-July 2026
- **Participants per session:** 6-30 members (core ~6-10 active builders)
- **Status:** Actively running

The Fractal is the longest-running continuous Fractal in the ecosystem. Optimism Fractal (another notable Fractal deployment) paused in January 2026. Eden Fractal moved to Base blockchain in 2025.

#### 2.3 Respect Tokens and Scoring

**Respect** is a non-transferable reputation token that represents governance weight and historical contribution. The ZAO uses two ledgers:

**OG Respect (ERC-20, historical ledger):**
- Contract: `0x34cE89baA7E4a4B00E17F7E4C0cb97105C216957` (Optimism)
- Deployed: July 30, 2024 (block 123349892)
- Supply: 38,484 ZAO
- Holders: 122 addresses
- Transfer model: Soulbound (non-transferable)
- Purpose: Rewards for one-time contributions before ORDAO era (introductions, articles, website features)
- Status: Frozen since December 2025 (archive ledger)

**ZOR Respect (ERC-1155, active ledger, ORDAO era):**
- Contract: `0x9885CCeEf7E8371Bf8d6f2413723D25917E7445c` (Optimism)
- Deployed: September 11, 2025
- Token ID: 0 (single token, fungible Respect balance)
- Holders: ~20+ addresses (started at 4 early adopters, grows weekly via Fractal distributions)
- Transfer model: Soulbound (all ERC-1155 transfers revert)
- Purpose: Democratic weekly distributions via Fractal consensus
- Status: Active, minting weekly via OREC

**Vote weight calculation:**
- Members' on-chain voting weight = OG Respect balance + ZOR Respect balance (raw sum, no decay as of July 2026)
- Source: `src/lib/respect/voteWeight.ts` line 58 (ZAOOS codebase): `weight: Math.round(ogValue + zorValue)`

**Scoring Curve (per 6-person group):**

The ZAO uses a custom 2x Fibonacci curve instead of the canonical Fibonacci:

| Rank | Respect | Ratio |
|------|---------|-------|
| 1st (highest contribution) | 110 | 1.0x |
| 2nd | 68 | 0.618x |
| 3rd | 42 | 0.618x |
| 4th | 26 | 0.618x |
| 5th | 16 | 0.618x |
| 6th (lowest) | 10 | 0.625x |
| **Per-group total** | **272** | -- |

This curve (verified in `src/app/(auth)/fractals/AboutTab.tsx:33` of ZAOOS) is twice the canonical Fibonacci scale (55/34/21/13/8/5) used by Eden and Optimism Fractals. ZAO chose the steeper curve to reward top contributors more aggressively while still resisting gaming via peer ranking.

**Respect Distribution Gini Coefficient (inequality measure):**
- OG era: 0.73 (top 10 holders own 53% of 38,484 supply)
- Indicates unequal historical distribution; by design (early contributors earned more)

#### 2.4 Verified On-Chain Governance Parameters

| Parameter | Value | Source |
|-----------|-------|--------|
| OREC executor address | `0xcB05F9254765CA521F7698e61E0A6CA6456Be532` | community.config.ts |
| Voting period duration | 72 hours | Doc 981 |
| Veto period duration | 72 hours | Doc 981 |
| Passing conditions | YES weight > 2x NO weight, AND YES weight >= 1,000 Respect minimum | Doc 981 |
| Minimum participation threshold | 1,000 Respect (~2.6% of OG supply) | Doc 981 |
| Total OREC proposals submitted | 130+ | Doc 981 |
| Historical OREC signers | 2 wallets: zaal.eth and civilmonkey.eth | Doc 703 |

**Note:** As of July 2026, only 2 wallets have ever called Vote/Execute on OREC. This is the community's single point of failure for consensus verification. A future ZIP should address signer committee rotation.

#### 2.5 Governance Ratification Chain

```
Fractal breakout ranking (Monday 6pm)
    ↓ (group consensus)
On-chain OREC proposal submission
    ↓ (anyone can submit)
72-hour voting phase (YES/NO votes from Respect holders)
    ↓
72-hour veto period (challenge window for blocking)
    ↓ (no new YES votes accepted in veto period)
Check passing conditions:
  - YES votes >= 1,000 Respect
  - YES weight > 2x NO weight
  - Both periods elapsed
    ↓
Execute (anyone can trigger)
    ↓ (OREC mints ZOR to ranked members)
Respected proposal enters on-chain record
```

### 3. Economic Model

#### 3.1 Contribution Circles

The ZAO organizes work through **contribution circles** - working groups focused on specific domains (music, events, development, governance, etc.). Each circle proposes work, estimates effort and impact, and distributes earned Respect to members.

Example circles (as of July 2026):
- Music & curation
- Events & festivals (ZAO-STOCK, ZAO-PALOOZA)
- Development & agents
- Governance & research

#### 3.2 ZOL - Contribution Credits

**ZOLs** are internal contribution credits (separate from on-chain Respect tokens) that track effort in units compatible with accounting and budgeting. ZOL rates and policies are determined by contribution circles and approved through Fractal consensus.

Purpose: Enable bookkeeping, budgeting, and allocation of treasury resources without requiring all decisions to be on-chain votes.

#### 3.3 Treasury and Fund

The ZAO maintains a collective treasury managed through:

- **Artizen Fund:** Fund for Emerging Culture, structured as a legal entity (S6 status)
- **Owner:** Zaal (legal representative)
- **Purpose:** Revenue from ZAO projects, grants, and donations pool for reinvestment in the community

Funds are allocated through Fractal consensus, contribution circles, and approved ZIPs.

### 4. Organizational Brands and Portfolio

The ZAO operates a portfolio of branded products and initiatives. The community began with a single Farcaster social client and evolved into a multi-brand ecosystem:

#### 4.1 The ZAO (Core)

- **What:** Governance, Respect system, Fractal, community coordination
- **Founded:** July 2024
- **Members:** 156 Respect holders
- **Channels:** Discord (private), Farcaster (public), on-chain

#### 4.2 WaveWarZ

- **What:** Music partnership and artist collaboration platform
- **Status:** Active
- **Details:** [to confirm: specific products and current user base]

#### 4.3 ZABAL Games

- **What:** Talent engine and workshop/mentorship platform for musicians and builders
- **Features:** 3-month build-a-thon (Jun/Jul/Aug 2026), mentorship from industry partners, onboarding for newcomers
- **Status:** Announced May 2026, running through August 2026
- **Partners:** Magnetiq (event/launch platform), [to confirm: other partners]

#### 4.4 Sparkz

- **What:** [to confirm: product description and purpose]
- **Status:** [to confirm: current status]

#### 4.5 COC Concertz

- **What:** Emerging artist concert series and record label
- **Status:** Graduated to own repo (code deleted from ZAOOS, exists independently)

#### 4.6 ZAO Festivals (ZAO-STOCK, ZAO-PALOOZA)

- **What:** Live event programming and festival strategy
- **Events:** ZAO-STOCK (Oct 3 2026, Franklin St Parklet), ZAO-PALOOZA ([to confirm: date/location])
- **Status:** Active

#### 4.7 Stilo World

- **What:** [to confirm: product description]
- **Status:** [to confirm: current status]

### 5. Development Model: Monorepo as Lab

The ZAO's primary codebase is **ZAOOS** (ZAO OS), a monorepo that functions as a lab for prototyping and incubating new projects.

**Operating principle:**
- New ideas are developed in ZAOOS alongside other experiments
- A project graduates when it reaches: production readiness + public-facing users + clear value proposition
- On graduation: code is moved to its own repo + database + domain name, then deleted from ZAOOS to prevent drift
- Examples: COC Concertz (graduated), ZAO-STOCK (spinning out mid-2026)

**ZAOOS contains:**
- 302 API routes (as of June 11 2026, Doc 836)
- 295 React components
- 18 custom hooks
- ~820 active research documents (institutional memory)
- Integration with Farcaster (Neynar), XMTP, Stream.io, Wagmi/Viem, Supabase

**Tech stack:**
- Next.js 16, React 19, TypeScript
- Supabase (PostgreSQL + RLS for data access control)
- Tailwind v4 (no CSS modules)
- Vitest for testing

### 6. Agent Layer

The ZAO operates autonomous agents for task coordination, research, and governance:

#### 6.1 ZOE (Orchestrator)

- **Role:** Central concierge bot and autonomous fixer
- **Features:** Task coordination, capture/recall, brief/reflect, autonomous fix-PR pipeline (combining logic from the deprecated Hermes bot)
- **Interface:** Telegram (`@zaoclaw_bot`)
- **Architecture:** Modular memory blocks, multiple critic/worker specialized agents
- **Source:** `bot/src/zoe/` (ZAOOS repo)

#### 6.2 ZAO Devz

- **Role:** Group dispatch + hourly learning tip
- **Interface:** Telegram (`@zaodevz_bot`)
- **Source:** `bot/src/devz/` (ZAOOS repo)

#### 6.3 ZOL

- **Role:** Farcaster scout agent for The ZAO
- **Source:** Dedicated repo [to confirm: exact location]
- **Status:** Active

#### 6.4 Bonfire

- **Role:** Knowledge graph recall + multi-corpus ingest
- **Platform:** Bonfires.ai (Genesis tier, wallet-gated)
- **Purpose:** AI-readable context for ZAO ecosystem

**Decommissioned (as of May 2026):** OpenClaw container (7-agent squad), Hermes as separate bot (merged into ZOE), 10-bot branded fleet.

### 7. Legal Structure

**Registered entity:** BCZ Strategies LLC (Delaware)

**Ownership:** Zaal (managing member)

**Purpose:** Provide legal wrapper for The ZAO, manage contracts, hold IP, interface with real-world institutions

**Relation to on-chain governance:** BCZ Strategies is off-chain legal entity; ZAO governance (Fractal + ORDAO) is on-chain. They operate in parallel.

### 8. Location and Contact

- **Website:** https://thezao.xyz
- **Discord:** Private community (invite-only)
- **Farcaster:** @zaal (founder)
- **X (Twitter):** @bettercallzaal (primary account)
- **ORDAO on-chain:** Respect-weighted voting, OREC executor on Optimism OP Mainnet

## Rationale

This ZIP documents The ZAO framework as it stands in July 2026 for several reasons:

1. **Institutional memory.** New contributors, partners, and researchers need a single source of truth rather than digging through 820 research documents.

2. **Clarity for amendments.** Future ZIPs can reference specific sections (e.g., "ZIP-5 amends ZIP-1 Section 2.4 to increase OREC signer committee to 5 wallets") rather than re-explaining the whole system.

3. **On-chain record.** Documenting governance in a public repo creates an immutable, auditable record of how The ZAO decided to operate.

4. **Partner confidence.** External partners (brands, investors, collaborators) need to understand the structure. This ZIP is that document.

5. **Honest about gaps.** Where information is uncertain (marked `[to confirm]`), this ZIP flags it rather than guessing. Future verification updates this ZIP via amendment.

The design choices in The ZAO (Fractal over token voting, peer-ranked Respect over purchased tokens, weekly sessions over async voting) are intentional and documented in precedent research (Fractally theory, Nouns DAO comparative analysis in Doc 718d). This ZIP references that work without re-arguing it.

## Backwards Compatibility

This ZIP is descriptive, not prescriptive. It documents existing decisions and systems. It does not change current operations.

However, confirming this as THE canonical framework means:

- Future operational decisions should reference ZIP-1 where applicable
- Deviations from ZIP-1 should go through a formal ZIP amendment (not silent changes)
- If this document is outdated (e.g., new brands, new agents, new governance parameters), an amendment ZIP should be ratified to keep it current

No member action is required on acceptance of ZIP-1. The community continues operating as-is.

## Security and Governance Considerations

### Technical Security

1. **OREC signer bottleneck:** Only 2 wallets have ever submitted to OREC (zaal.eth, civilmonkey.eth). If either wallet is compromised or unavailable, governance halts. Recommendation: Establish a 3+ signer committee and rotate submission authority. (See Doc 703 Recommendation #1.)

2. **Respect calculation parity:** The main ZAOOS app's Respect vote-weight formula (`voteWeight.ts`) differs from the Discord bot's independent `eth_call` calculation. If these ever diverge, members' on-chain weight vs Discord-displayed weight will disagree. Recommendation: Audit both code paths and add fixed-wallet consistency tests. (See Doc 981 Gap #3.)

3. **OG/ZOR ledger reconciliation:** Historical Respect (OG era) and current Respect (ZOR era) exist as separate on-chain ledgers. No published mapping formula for migrations. Recommendation: Archive the conversion logic and publish it. (See Doc 703 Recommendation #5.)

### Governance Risks

1. **Centralization:** Weekly Fractal decisions depend on a consistent facilitator and Discord hosting. Single points of failure: Discord goes down, Zaal unavailable, or Discord is acquired and ToS changes. Mitigation: explore Telegram-native or GitHub-native Fractal variants (Doc 664 brainstorm).

2. **Opt-in governance:** Respect is earned by showing up to weekly Fractals. Members who miss sessions don't earn Respect that week. Over time, the same ~6-10 core members accumulate most Respect and decision-making power. Mitigation: contribute-in-absentia mechanisms (recorded contributions, asynchronous submission). (See Doc 703 Recommendation #4 re: documentation and onboarding.)

3. **Framework drift:** With ~820 research documents and a fast-moving community, policy can drift from the written record. Mitigation: use ZIPs to codify decisions, and update this framework ZIP as the community evolves.

### Process Risks

1. **Amendment precedent:** This ZIP establishes that amendments go through ratification. If a previous decision is discovered to conflict with this framework, does it need a ZIP to fix it, or can it be quietly corrected? Answer: Quietly correcting governance documents is exactly the credibility risk a whitepaper is supposed to prevent. Always propose amendments openly.

2. **Ratification by merge:** Merging a ZIP to main (this repo) = accepting it. This is different from ORDAO's on-chain vote system. Recommendation: establish clear ownership (who can merge ZIPs, under what conditions).

## Copyright

This ZIP is released under CC-BY-4.0. Referenced research documents (Doc NNN) remain in their original locations with their original licenses. On-chain contracts are open-source under their deployed licenses (see individual contracts for details).

---

## Sources and References

This ZIP synthesizes information from multiple research documents and on-chain queries conducted in July 2026:

| Source | Type | Key Content |
|--------|------|-------------|
| Doc 703 (ZAO Fractal Current State, May 2026) | Research | Current infrastructure, open issues, recommendations |
| Doc 981 (Fractal x Discord Bot Synthesis, July 2026) | Research | Verified on-chain numbers, bot implementation, gap analysis |
| Doc 942 (Fractal Whitepaper Outline v2, July 2026) | Research | Fractal theory, monetary policy, design rationale |
| Doc 056 (ORDAO & Respect Game, May 2026) | Research | OREC mechanics, voting system, Fibonacci scoring rationale |
| ZAOOS codebase (`src/lib/respect/`, `src/app/api/fractals/`, `community.config.ts`) | Code | Verified contract addresses, vote weight calculations, Fibonacci curve |
| Optimistic Etherscan (OREC contract) | On-chain | Transaction count, signer history, deployment block |

**Verified claim checks (July 2026):**
- Fractal running since 2024-07-30: Confirmed (Doc 981, `community.config.ts`)
- Respect holders (156 = 122 OG + 55 ZOR): Confirmed (Doc 981, section 6)
- Vote weight formula: Code-verified (Doc 981, `src/lib/respect/voteWeight.ts:58`)
- Fibonacci scoring (110/68/42/26/16/10): Code-verified (Doc 981, `AboutTab.tsx:33`)
- OREC parameters (72h vote/veto, 1,000 Respect threshold): Verified (Doc 981, Doc 056)

**Claims marked [to confirm]:**
- WaveWarZ product details
- Sparkz product description and status
- ZAO-PALOOZA date and location
- ZOL exact repository location
- ZABAL Games mentorship partner list (beyond Magnetiq)
- Current ZAO-STOCK spinout status (as of July 2026)

---

## Amendments and Future

This ZIP captures The ZAO as it exists in July 2026. As the community evolves, this ZIP will be amended through new ZIPs that:

- Add new brands or platforms (e.g., "ZIP-N: Add Stilo World to ZAO portfolio")
- Update governance parameters (e.g., "ZIP-N: Increase Respect distribution to 300 points per group")
- Change the Fractal format (e.g., "ZIP-N: Move to async GitHub-native Fractal")
- Clarify or correct the framework (e.g., "ZIP-N: Amend ZIP-1 Section 2.4 to add signer rotation policy")

Each amendment maintains the integrity of the prior document while moving the framework forward transparently.

---

**Status:** Draft  
**Next steps:** Community review, Fractal discussion, ratification via consensus  
**Last revised:** 2026-07-15
