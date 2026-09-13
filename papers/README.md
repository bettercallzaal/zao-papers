# Sparkz and The ZAO Ecosystem Papers

This directory contains canonical papers, research, and documentation about Sparkz and The ZAO's brands, architecture, and systems.

## Papers

- **[zabal-games.md](zabal-games.md)** - ZABAL Gamez: The ZAO's 3-Month Builder Incubator. Season 1: 28 June workshops, WaveWarZ Finals mechanic, 9 intake-ready builders. Status: Draft.
- **[coc-concertz.md](coc-concertz.md)** - COC Concertz: The ZAO's Virtual Concert Series and Archive Protocol. 7 shows, Arweave UDL archive, WaveWarZ live integration. Status: Draft. (see PR #6)
- **[wavewarz.md](wavewarz.md)** - WaveWarZ: The ZAO's On-Chain Music Battle Platform. 1,108+ battles, 524.15 SOL, 98.5% ecosystem payout. Status: Draft. (see PR #5)
- **[sparkz.md](sparkz.md)** - Sparkz: Configurable Creator-Coin Launcher with AI advisor, energy-first coordination. Status: Draft.

## The ZAO Ecosystem: Brands and Systems

### Core Governance

The foundational ZIP that documents The ZAO's governance, organizational structure, and framework:

| Document | What | Status |
|----------|------|--------|
| **ZIP-1: The ZAO Framework** (parent README) | The ZAO's governance model (Fractal, Respect, ORDAO), organizational brands, development model, agent layer, legal structure | Draft |

### The ZAO Brands

A portfolio of music, culture, and community projects operating under The ZAO umbrella.

#### The ZAO (Core Governance & Community)

- **What:** Governance, Respect token system, Fractal consensus game, community coordination
- **Members:** 156 Respect holders (122 OG + 55 ZOR era)
- **Channels:** Discord, Farcaster, on-chain (Optimism)
- **Governance:** Weekly Fractal sessions (101+ weeks unbroken since July 2024)
- **On-chain:** ORDAO voting, Respect-weighted with 1,000 Respect proposal threshold
- **Context:** https://useicm.com/api/objects/icm_ohb0F_XOYDz9Tw_w4yX3PA/llm.txt
- **Human Directory:** https://thezao.xyz/list

#### WaveWarZ (Music Trading Platform)

- **What:** Artist-to-fan music trading game with real revenue splits
- **Live since:** [to confirm: launch date]
- **Volume:** ~458 SOL (~$39K at 2026-05-25), 979 battles
- **Team:** Hurric4n3IKE (founder/dev), Candytoybox/Samantha (design/marketing), Zaal (ecosystem)
- **Stack:** Solana mainnet + Base Sepolia testnet, Next.js/React, Supabase
- **Partners:** 7 major integrations (Coinflow, Juke, Magnetiq, Empire Builder, Neynar, RAM SongChain, Privy)
- **Daily:** 11 events per week (AMAs, Quick Battle Trading, special tournaments)
- **Positioning:** Front door + gravity well - brings external founders into The ZAO ecosystem
- **Doc Reference:** Doc 743 (canonical v2)

#### ZABAL Gamez (Builder Incubator)

- **What:** 3-month builder incubator -- Workshops (June) + Open build (July) + Finals (August). Three tracks: artist, builder, creator.
- **Season 1:** June 1 - August 31, 2026. Site: zabalgamez.com (Farcaster Mini App)
- **June:** 28 workshop sessions complete; expert guests from Neynar, BizarreBeasts, Bankr, Eden Fractal, etc.
- **July:** Open build phase; 9 intake-ready builder submissions in QV ballot
- **August:** Finals -- builder projects compete in WaveWarZ battles; community investment determines winner
- **Roster:** 32 people (organizers, mentors, leads, builders)
- **Full paper:** [zabal-games.md](zabal-games.md)
- **Doc Reference:** Doc 1258, Doc 1255

#### Sparkz (Creator-Coin Launcher)

- **What:** Configurable creator-coin launcher with AI advisor, energy-first coordination
- **Core mechanism:** 0xSplits for adjustable fee-splitting, AI recommends governance + utility
- **Launch rail:** Clanker or Empire Builder
- **Status:** Draft, Iman dogfood on Base testnet early August [to confirm: exact date]
- **Legal review:** Greg Autonomous (web3 counsel) - memocoin vs utility framing [open]
- **Doc Reference:** Doc 1098 (master brief), Doc 1088 (token), Doc 1108 (legal), Doc 1132 (pilot)

#### COC Concertz (Artist Concert Series & Label)

- **What:** Emerging artist concert series and record label
- **Status:** Graduated to own repo (code deleted from ZAOOS, exists independently)
- **Deployment:** Own codebase, own database, own domain
- **Relationship:** ZAOOS retains research/institutional memory; no code duplication

#### ZAO Festivals (Live Events)

- **What:** Umbrella brand for The ZAO's IRL event programming
- **Upcoming:** ZAO-STOCK (October 3 2026, Franklin St Parklet, Ellsworth Maine)
- **Past:** ZAOCHELLA (Miami 2024) - brand-building flagship
- **In planning:** ZAO-PALOOZA [to confirm: date/location]
- **Structure:** Contribution circles, ZAOstock team, festival-focused Fractal sessions

#### Stilo World

- **What:** [to confirm: product description and purpose]
- **Status:** [to confirm: current status]

### AI-Readable Context Boxes (ICM)

The ZAO ecosystem maintains AI-readable context boxes on useicm.com for programmatic access to canonical knowledge:

| Name | ICM ID | Link | Purpose |
|------|--------|------|---------|
| **The ZAO** | icm_ohb0F_XOYDz9Tw_w4yX3PA | [LLM context](https://useicm.com/api/objects/icm_ohb0F_XOYDz9Tw_w4yX3PA/llm.txt) | Core governance, Fractal, Respect, ORDAO |
| **ZABAL Gamez** | icm_PiCDHNNZ3WZpNoF59OA8Dw | [LLM context](https://useicm.com/api/objects/icm_PiCDHNNZ3WZpNoF59OA8Dw/llm.txt) | Build-a-thon mentorship, builder context |
| **ZAO Assistant** | icm_-hsPHePpqX01RovoB_SEqA | [LLM context](https://useicm.com/api/objects/icm_-hsPHePpqX01RovoB_SEqA/llm.txt) | ZAO operator layer, linking to other boxes |

**How to use:** Fetch an ICM context box via `curl -s https://useicm.com/api/objects/<id>/llm.txt` to load grounded AI context. All ZAO ecosystem agents (ZOE, Bonfire, others) can query these boxes for current knowledge.

**Contributing:** To add or update an ICM box, see research/identity/icm-boxes/ in ZAOOS for the edit protocol and ownership keys.

### Governance ZIPs

- **ZIP-1: The ZAO Framework** - Foundational governance, Fractal, Respect, ORDAO, brands, agents (see parent README for full content)

## Member Articles & Community Research

Directory: `../members/`

Articles and research written by ZAO members on topics of ecosystem interest.

## How to Propose

1. **For Sparkz or new research papers:** Open a PR with the paper in `papers/<name>.md`
2. **For governance changes:** Use the ZIP process (see parent PROCESS.md and CLAUDE.md)
3. **For brand updates:** If a brand's status, team, or major milestones change, propose an amendment to this catalog or to ZIP-1

## Style Guide

- No emojis or decorative Unicode
- Brand names always exact: The ZAO, WaveWarZ, ZABAL Gamez, Sparkz, COC Concertz, ORDAO, Respect
- Mark uncertain claims with [to confirm]
- Ground all technical claims in Doc NNN, GitHub links, or on-chain data
- See CLAUDE.md for full contributor guidelines

## Links

- **The ZAO:** https://thezao.xyz
- **Community Directory:** https://thezao.xyz/list
- **Fractal:** Monday 6pm EST (Discord)
- **ORDAO:** https://useweb3.xyz/dapps/optimism-ORDAO
- **Farcaster:** @zaal
- **X (Twitter):** @bettercallzaal

---

**Last updated:** 2026-07-17  
**Maintainer:** @zaal  
**License:** CC-BY-4.0
