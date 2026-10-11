---
title: "The ZAO Technical Whitepaper: one Respect ledger, four readers"
author: Zaal Panthaki
status: Draft
created: 2026-10-11
last-updated: 2026-10-11
source-docs:
  - "ZAOOS research governance/2656 - The ZAO as one system (measured 2026-10-10)"
  - "ZAOOS research governance/2657 - Respect as the binding token (measured 2026-10-10)"
  - "ZAOOS research doc 2562 - OREC and the Respect ledgers"
  - "ZAOOS research doc 2642 - voting systems inventory"
  - "ZAOOS research governance/981 - Fractal and bot synthesis, section 6 (on-chain pull of 2026-07-05)"
  - "ZAOOS research doc 059 - Hats tree 226 (last validated 2026-08-21)"
  - "ZIP-1 (zips/zip-0001-the-zao-framework.md) and the Season 3 ZIP (zips/zip-0002-season-3.md)"
  - "bettercallzaal/zingfisher main at 532e850 (packages/config/src/zao.ts, packages/lib/src/zao/)"
---

# The ZAO Technical Whitepaper: one Respect ledger, four readers

> **Draft for Zaal's review. Not published, not ratified.** This paper describes the system as measured on
> 2026-10-10 and 2026-10-11. It states what is live, what is specified and not built, and what is unknown.
> It proposes nothing new. Where the whitepaper (ZAODEVZ/ZAOfractal) says why, this paper says how.

## Abstract

The ZAO runs on one fact per member: how much Respect they hold. Respect is earned in a weekly peer-ranking
game and minted on Optimism. The OG contract is soulbound; whether ZOR can be
transferred is not established in the sources for this draft. Four systems read that one number: the
on-chain executor (OREC), the Snapshot space, the ZAO OS app, and the zingfisher learning and proposal
platform. Hats Protocol tree 226 names who holds which role and confers no vote weight. This paper lists the
contracts, the exact read each system performs, the gates that follow from it, the parameters, and the known
gaps. Every address and number below carries the date it was measured.

---

## 1. Components

| Component | What it is | Where | State |
|---|---|---|---|
| Respect OG | ERC-20, 18 decimals, non-transferable | Optimism `0x34cE89baA7E4a4B00E17F7E4C0cb97105C216957` | Live. No new mints since 2025-12-20 (doc 2562) |
| Respect ZOR | ERC-1155, token id 0, integer units | Optimism `0x9885CCeEf7E8371Bf8d6f2413723D25917E7445c` | Live. Minted weekly; on-chain period 114 on 2026-09-24 |
| OREC | Optimistic Respect-based executive contract (ORDAO, GPL-3.0) | Optimism `0xcB05F9254765CA521F7698e61E0A6CA6456Be532` | Live. 72 hour vote, then 72 hour veto |
| Hats tree 226 | Role tree on Hats Protocol (AGPL-3.0) | Optimism, Hats contract `0x3bc1A0Ad72417f2d411118085256fC53CBdDd137` | Live. Manual eligibility on top-level hats (doc 059) |
| Snapshot space | Gasless holder vote | `zaal.eth` | 16 proposals, newest 2025-12-21 (doc 2642) |
| Fractal bot v2 | Discord bot that runs the weekly game and stores rankings | `bettercallzaal/zao-fractal-bot` (MIT) | Live in Discord. No on-chain submit path in `src/` |
| ZAO OS | Member app | zaoos.com | Live |
| zingfisher | Learning hub, member pages, proposal drafting. Fork of CharmVerse via KFMEDIA, AGPL-3.0 | `bettercallzaal/zingfisher` | Preview deploy. No database wired on 2026-10-11 |

The OG `decimals()` call returned 18 through a public Optimism RPC on 2026-10-10. The three contract
addresses and the Hats address are the values in `packages/config/src/zao.ts` at zingfisher 532e850.

## 2. The Respect ledger

Respect has two contracts because the minting method changed, not because there are two kinds of standing.

- **OG** is the original ERC-20. It reports balances in wei. One whole Respect is `1000000000000000000`.
- **ZOR** is the ERC-1155 that OREC mints into. It reports whole units at token id 0.

**Respect weight is whole OG plus ZOR.** Read live from Optimism, OG divided by 10^18 and truncated, then
summed with ZOR. This was ruled on 2026-10-10 (decisions item 78) and confirms the 2026-09-22 ruling of a
1:1 unweighted sum.

A reader that skips the decimals step is wrong by a factor of 10^18. zingfisher did exactly this until its
PR #2. The fix is pinned by a test on one wallet whose balances were read on 2026-10-10: 3,094 OG plus 858
ZOR equals 3,952.

Holder counts from The ZAO's ICM box dated 2026-07-05, as carried in doc 2656: 156 unique holders, 122
holding OG, 55 holding ZOR, 21 holding both. These figures are three months old. OG supply was 38,484 in the
same 2026-07-05 pull (doc 981 section 6). `[to confirm: holder counts and OG supply as of October
2026]`

### How Respect is created

The weekly Fractal is the only source of governance Respect (ruled 2026-09-22, item 26).

1. A breakout group of six ranks its members' contributions by consensus.
2. The bot records the ranking (`fractal_sessions`, `fractal_scores`).
3. A person submits the result to OREC through the ORDAO front end at zao.frapps.xyz.
4. After the 72 hour vote and the 72 hour veto, OREC mints ZOR on the ladder 110, 68, 42, 26, 16, 10 for
   ranks one to six.

No Respect is burned and none decays. The 2 percent weekly decay in earlier drafts was ruled out on
2026-09-26.

## 3. Four readers of one ledger

| Reader | Exact read | Used for | Agrees with the ruling |
|---|---|---|---|
| OREC | `respectContract()`, which as of 2026-10-10 points at OG only | Vote weight on on-chain proposals | No. See section 7 |
| Snapshot `zaal.eth` | `erc20-balance-of` on OG plus `erc1155-balance-of` on ZOR | Holder votes on ZIPs | Yes |
| ZAO OS | Multicall of OG and ZOR, `formatEther` on OG | In-app proposal weight; refuses a vote when the read fails | Yes |
| zingfisher | `respectGate.ts`: OG and ZOR, whole units | Display on the member page, advisory vote weight | Yes, since PR #2 |

OREC reads weight live for each voter at the moment the vote is cast. The platform rule is to snapshot
weight when a proposal opens (2026-09-22, item 3). These differ by design, and a proposal page must say
which one it used.

## 4. Access gates

zingfisher decides access with CharmVerse's own token gates, not a second gate system. The definitions live
in one place (`zaoTokenGates` in `packages/config/src/zao.ts`) and are evaluated by
`packages/lib/src/zao/zaoTokenGates.ts`.

| Gate id | Passes when | Role it maps to | What the role grants |
|---|---|---|---|
| `zao-respect-holder` | At least one whole OG, or at least one ZOR | Respect holder | Draft and endorse proposals |
| `zao-hat-configurator` | Wears the Configurator hat on tree 226 | ZAO Configurator | A label. No platform powers |
| `zao-hat-governance-council-member` | Wears the Governance Council Members hat | ZAO Governance Council Member | A label. No platform powers |

The gates are evaluated live by the membership API at zingfisher 532e850. They become `TokenGate` and `Role` rows once a
database exists (`scripts/seedZaoTokenGates.ts`, idempotent, with a dry-run flag).

The $ZABAL token confers nothing in any gate (ruled 2026-09-22, item 6).

### What each action requires

| Action | Requirement | Source of the rule |
|---|---|---|
| Read a proposal or page | None | 2026-09-22 item 15 |
| Comment | A member account (ZID) | Items 12 and 15 |
| Draft a proposal | Respect weight above 0 | Item 2 |
| Endorse a draft | Respect weight above 0; the endorsement carries the endorser's weight | Item 2 |
| Publish to a vote | 1,000 Respect of endorsements from peers | Item 2 |
| Vote | Respect weight above 0 at proposal open | Items 1 and 3 |
| Pass | 72 hour vote, then 72 hour veto | Item 4, "match orec for now" |
| Execute | Nothing executes automatically | Item 7 |

The proposal flow in this table is specified and not built. Whether the author's own Respect counts toward
the 1,000 is an open question in the spec.

## 5. Roles: Hats tree 226

A hat is a role. Respect is standing. The two are kept apart on purpose.

- Tree 226 has a top hat, a Configurator hat (2 of 5 worn), a Governance Council Members hat (3 of 5 worn),
  and 17 project hats. These counts are from doc 059, last validated 2026-08-21; the day the wearers
  were counted is not established. `[to confirm: wearer counts as of October 2026]`
- Eligibility on the top-level hats is manual.
- Hat ids are computed as 16-bit levels from bit 224. The Configurator hat is
  `0x000000e2 0001 0000...` and Governance Council Members is `0x000000e2 0001 0002...`.
- No hat grants vote weight or proposal power (ruled 2026-09-22 afternoon, item 18). Hats voting modules
  count one vote per hat and cannot weight by a balance, so weight stays with the token.

The Season 3 ZIP specifies a Manifesto Hat as the membership credential. It is not built.

## 6. Decisions: three routes

| Kind of decision | Route | Binding |
|---|---|---|
| Discussion, drafts, sense of the room | zingfisher proposal page (specified, not built) | Advisory |
| Approval of a ZIP | Snapshot space `zaal.eth` | Holder vote, off chain (ruled 2026-10-10, item 79) |
| Anything on chain | OREC | Executes after 72 hours of voting and 72 hours of veto |

OREC's proposal threshold is 1,000 Respect, and 130 OREC proposals had been made from 4 wallets, 94
percent of them from one relayer. Both figures are from ZAOOS doc 981 section 6, which reports an on-chain
pull of 2026-07-05 recorded in doc 975. Doc 975 was not opened and neither figure was re-read from the
chain for this draft. The 1,000 here is OREC's threshold; the 1,000 in section 4 is the platform's publish
bar, a separate rule. `[to confirm: proposal and wallet counts as of October 2026]`

## 7. Known gaps

Each of these is taken from the sources below, with its date where the source gives one.

1. **OREC counts OG only.** OG has had no new mints since 2025-12-20, so members who joined after that date
   hold ZOR and have no weight in OREC. 41 wallets hold ZOR and no OG, 7,476 ZOR between them (onboarding
   spec, question 10). The ruled fix is a wrapper contract answering OG plus ZOR, installed by one passed
   proposal calling `setRespectContract`. The wrapper is not written. It needs an external Solidity
   reviewer before anything goes on chain. No reviewer is assigned.
2. **One submitter.** The last 12 mints came from one address. The bot has no submit path, so a person
   submits each breakout by hand.
3. **A hole in the session record.** The newest session row in the bot's database is dated 2026-04-14, and
   `respect_points` is zero on 771 of 801 rows (doc 2562). The chain is the record; the database mirror is
   not complete.
4. **No database behind zingfisher.** Member profiles (ZIDs) are held in memory and do not survive a
   restart. Proposals are not built.
5. **The Manifesto Hat, the manifesto text, and the three membership layers** are specified in the Season 3
   ZIP and not built.
6. **Upstream ORDAO issues that shape these rules:** OREC lets a zero-Respect account create a proposal
   (sim31/ordao issue 20), and one rejecting receiver aborts a whole Respect distribution (issue 27).

## 8. Season 3

Season 3 is the production release of this system. It starts on 1 December 2026 (ruled 2026-09-26,
confirmed 2026-10-10). The Season 3 ZIP is the specification. This paper does not restate it. The pieces it
adds to the system described here are the wrapper in gap 1, the Manifesto Hat, a rolling 90-day activation
window, and achievements recorded through Hats.

## 9. What this paper does not cover

- Why the system is shaped this way. See the whitepaper.
- WaveWarZ, COC Concertz and Sparkz, which have their own papers in this directory.
- Festival and project Respect ledgers, which are ruled (2026-09-26) and have no contract or table yet.
- Treasury and legal structure. See ZIP-1.

## Licences and credit

zingfisher is a fork of CharmVerse by way of the KFMEDIA fork, under AGPL-3.0. ORDAO and OREC are by
sim31 and contributors, GPL-3.0. Hats Protocol is AGPL-3.0. The licence of each was read from its LICENSE
file on 2026-10-10.

## Sources

- ZAOOS research governance/2656 and governance/2657, both merged 2026-10-10. Every address, file path and
  measurement in this paper is taken from one of the two or from the files named below.
- ZAOOS research docs 2562, 2642 and 059, as cited in 2656 and 2657. Docs 2562 and 2642 were not re-read
  for this draft; doc 059 was opened only for its validation date.
- ZAOOS research governance/981 (last validated 2026-07-06), section 6, opened 2026-10-11: OG supply, the
  OREC proposal threshold, and the proposal and wallet counts. It relays doc 975, which was not opened.
- zingfisher main at 532e850: `packages/config/src/zao.ts`, `packages/lib/src/zao/zaoTokenGates.ts`,
  `packages/lib/src/zao/respectGate.ts`, `scripts/seedZaoTokenGates.ts`. Read 2026-10-11.
- Rulings: vault decision files of 2026-09-22 (morning and afternoon) and 2026-09-26, and items 78, 79 and
  85, each stamped 2026-10-10 in the decision file that opened on 2026-10-09.
- Not searched for this draft: Google Drive and Notion. An earlier technical whitepaper may exist there.
  None was found in zao-papers, zao-website, ZAOfractal or the ZAOOS research index on 2026-10-10.
