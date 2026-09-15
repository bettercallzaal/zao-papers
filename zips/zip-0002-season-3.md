---
zip: 2
title: Season 3 - The Fractal Season
author: Zaal
status: Draft
created: 2026-09-15
last-updated: 2026-09-15
---

# ZIP-2: Season 3 - The Fractal Season

## Abstract

Season 3 starts 1 November 2026 and redefines who is "in The ZAO." It replaces the five
conflicting code-level meanings of "member" (brainstorm, Measured context 2026-09-11) with three
layers - provisional, full, and this month's active voting pool - joined by a soulbound,
gasless, one-signature manifesto mint, kept by monthly self-service activation with no burn, and
left only by a governance vote. Achievements, titles and roles are delivered automatically
through Hats Protocol. Season 3's scope is the fractal itself: ZAOstock, FISHBOWLZ and ZAO OS are
untouched; the one new project branch is ZABAL Gamez (brainstorm #31).

## Motivation

"Member" means five different, disconnected things in the codebase today: the ZAO OS
`allowlist` table, `users.member_tier`, the fractal's own `respect_members` roster, onchain OG
Respect, and onchain ZOR Respect - plus Discord server membership, which is a sixth surface with
no formal tie to any of the above (brainstorm, Measured context, 2026-09-11). None of these
answer the questions Zaal asked first: who is in, what does joining take, and what happens when
someone stops showing up.

Zaal named the fractal as the place to start, not because it is the biggest surface, but because
it is the org's governance: "ZAO Fractal is the governance of The ZAO. Not a project inside it:
the fractal's Respect and OREC decide for all of The ZAO. Projects sit under it" (brainstorm
#21). Season 3 is that governance layer's own membership redesign - not a rebrand of an existing
system, and not a vote on tokenomics, records audit or any of the other sub-projects the
brainstorm log names as later work (brainstorm, Sub-projects list).

## Specification

### 1. Three layers, not one membership

Being "in The ZAO" is not one state. Zaal's own answer to what makes someone a member: "Layers.
Joining is separate from Respect, and separate from being active this month. Inactive people stay
in and keep every point" (brainstorm #1). Season 3 implements this as three layers:

1. **Provisional member** - has minted the manifesto with an email-only identity (Privy). Has
   achievements only; earns no Respect and cannot vote (brainstorm #13, #14).
2. **Full member** - has minted the manifesto and linked at least one proven identity (Discord,
   Telegram, LinkedIn, X, or an ENS name checked onchain against the signing wallet) (brainstorm
   #4, #9). Full members keep every Respect point they have ever earned, whether or not they are
   active this month (brainstorm #1).
3. **This month's active pool** - full members who have signed the current month's activation, or
   who auto-activated by attending something this month (brainstorm #18, #19). This is the pool
   whose vote passes an OREC proposal (brainstorm #8, #23); mechanically, OREC reads Respect
   balance live per voter at cast time rather than a single proposal-creation snapshot (brainstorm,
   Measured context correction, 2026-09-11).

Onchain, membership is a **Manifesto Hat on Hats Protocol tree 226 (Optimism, chain 10)**
(brainstorm #17). This confirms the direction already set in ZAO OS research docs 942/962, which
describe "Signing the manifesto = minting a Hats Protocol hat" (brainstorm, doc 962 citation).

### 2. Joining

Zaal, verbatim: "Make the smallest thing possible to join just by signing a transaction and
getting a free mint to mint the manifesto which is our terms and conditions essentially and then
u can get different achievements and titles and roles for participating in different things"
(brainstorm #2). Concretely:

- **One signature.** The member signs the manifesto agreement; nothing else is required to become
  provisional (brainstorm #2, #13).
- **Soulbound.** The manifesto mint is locked to the wallet that signed it (brainstorm #3). Hats
  Protocol has no wearer-initiated transfer function at all, and the Manifesto Hat is set
  immutable so an admin cannot move it either - the only ways the hat leaves a wallet are the
  wearer's own `renounceHat()`, or the governance-controlled eligibility module burning it on
  revocation (season3-hats-protocol-research-2026-09-11.md, Q5).
- **Gas.** The ZAO sponsors gas; login is via Privy, including email-only (brainstorm #12).
  Concretely this is Hats Protocol's `claimHatFor()`: any relayer The ZAO operates can pay gas on
  an eligible wearer's behalf once the hat is set claimable-for (season3-hats-protocol-
  research-2026-09-11.md, Q2).
- **Identity to become full.** At least one linked identity, proven by OAuth (Discord, LinkedIn,
  X), the Telegram login widget, or an ENS name checked onchain against the signing wallet
  (brainstorm #4, #9).
- **Existing members.** Current Respect holders mint too. Their Respect and history stay exactly
  as they are; they count as members once they sign (brainstorm #5).
- **Where.** fractal.thezao.com - manifesto, login, public points and achievements (brainstorm
  #16). No DNS record exists for that subdomain yet as of 2026-09-11 (brainstorm #16, Measured).

### 3. Monthly activation

Activation is the monthly signature, and it never burns anything (Sub-project 3, brainstorm
Sub-projects list). Two ways to activate that month:

- Sign the current month's activation directly, or
- Attend something that month, which auto-activates you: "Would be cool if it auto opts you in
  when u attend something" (brainstorm #18).

You only re-sign the manifesto text itself once per version, not every month: "your next
activation asks you to sign it once" after a new version, and everyone in the active pool is
always on current terms (brainstorm #19, #15).

**What counts as "attending something" for auto-activation.** Brainstorm #20 deferred this by
name to settle a bigger structural question first (fractal-as-governance vs. fractal-as-incubated
project); that structural question is answered in section 6 below. Zaal ruled on the deferred
question itself in the grill on 2026-09-15: **a fractal, hosting a call or making an intro, a
festival, article or workshop count as attending. Signature alone does not** (decisions/
grill-2026-09-15-morning.md, "Agent stack" / Season 3 line). This overlaps heavily with what
already earns Respect today - intro and article both feed `event_respect`, hosting feeds
`hosting_respect`, festival feeds `bonus_respect` (brainstorm, Measured context) - with a fractal
session itself and a workshop also named as activating events.

**Mechanism.** OREC reads vote weight live, per voter, at the moment each address casts or
re-casts its vote - not a single snapshot taken for every voter at proposal-creation time
(measured directly from `Orec.sol`; corrects an earlier assumption in the brainstorm log,
season3-protocol-survey-2026-09-11.md, Layer 4 Measured code facts). `OREC.respectContract` is
swappable to any new contract implementing `IRespect` or ERC-20 `balanceOf`, without redeploying
OREC, via one governance-passed `setRespectContract` call (same source). Season 3's activation
layer is therefore **a small custom wrapper contract implementing `IRespect`, installed into OREC
through one governance proposal** - it never touches the real Respect ledger, it only answers "is
this address's activation current right now" when OREC reads it (Zaal's build-order list,
handoffs/season3.md; season3-protocol-survey-2026-09-11.md, Layer 4 Part B/recommended stack).
This is the "small custom wrapper OREC reads" item on Zaal's own post-launch build list and needs
review by a Solidity reviewer who did not write it before it ships (handoffs/season3.md). What
backs the wrapper's own activation record is not yet decided - see Open Item 5.

No off-the-shelf Hats Protocol module does self-service monthly re-activation with no admin
action; the closest reference implementation (Hats Elections Eligibility) still requires results
submitted by a separate "ballot box" hat wearer, not a self-declared signal from each member
(season3-hats-protocol-research-2026-09-11.md, Q3). The wrapper above is the build gap that
closes that.

### 4. Leaving

Membership is **revocable by vote only.** Only a governance proposal that the active pool passes
can remove a member; Respect and history stay onchain, untouched, and no single person can remove
anyone (brainstorm #8). Onchain, this is implemented by making OREC (or a hat OREC wears) the
`eligibility`/`arbitrator` party for the Manifesto Hat, so a passed OREC proposal - and only a
passed OREC proposal - can call `Hats.setHatWearerStatus()` to revoke it
(season3-hats-protocol-research-2026-09-11.md, Q4). Revocation burns only the Hats token; the
separate OG/ZOR Respect ledgers are never touched by a Hats-side action (same source, Q4 fit
table).

### 5. Achievements, titles and roles

"All automatic usui g hats protocol and the agents can do that after we confirm it works as
intended" (brainstorm #10, verbatim). Hats Protocol is the chosen substrate for every achievement,
title and role; agents grant them, but only after a human-checked phase proves the rules work.
This extends a pattern already live in tree 226 rather than introducing a new one: leaf hats such
as "ZAO 101 Beginner," Audio Artist, Visual Artist, Entrepreneur and WaveWarZ's Artist Agreement
hat already use self-claimable, sign-an-agreement eligibility modules in production
(season3-hats-protocol-research-2026-09-11.md, Q7 correction, 2026-09-13). A Discord intro keeps
earning intro Respect (`event_respect`) exactly as today, plus the matching achievement
(brainstorm #11); intro Respect requires full membership, which anyone who links Discord already
has, since that is where intros happen (brainstorm #14).

### 6. What the fractal governs, and what it does not

The fractal decides only shared things across The ZAO: membership, which projects carry The ZAO
name, the shared treasury, and the manifesto. Projects run themselves day to day - team,
decisions, money (brainstorm #22). A project becomes a ZAO project by proposal and vote: any full
member proposes, and it is official once an OREC proposal passes in the active pool. Existing
projects are admitted in one batch proposal at the start of Season 3, nothing grandfathered
silently (brainstorm #23).

**Season 3's own scope is the fractal only** - membership, manifesto, activation, and the public
points display. Zaal, verbatim, on adding ZAOstock, FISHBOWLZ, ZABAL Gamez or ZAO OS as projects
under this ZIP: "No season 3 is just the fractla season none of these sjould be addednother than
zabal gamez" (brainstorm #31, verbatim). **The only new project branch in Season 3 is ZABAL
Gamez.** ZAOstock, FISHBOWLZ and ZAO OS are unaffected by this ZIP.

The full measured tree-226 project-branch list is 17 branches: Community, Location, ZAO 101, ZAO
Fractals, Wave WarZ DAO, ZAO FESTIVALS, ZTalent Newsletter, ZAO Cards, Student $LOANZ, ZAO
Calendar, COC ConcertZ, MIDI-ZAO-NKZ, Let's Talk about Web 3, and Future Project 1-4
(season3-hats-protocol-research-2026-09-11.md, Q7) - `COC ConcertZ` is the measured onchain hat
name; the brand spelling used elsewhere in this ZIP is COC Concertz, same branch. Brainstorm #25
asked for this list to be pruned and for missing branches to be added; Season 3 prunes but does
not add any others, because #31's later, more specific ruling closes the list to exactly one new
branch, ZABAL Gamez. Of the 17: WaveWarZ stays live; ZAO 101 merges into ZAO Fractal
(onboarding/education side); ZAO Cards merges into ZAO FESTIVALS; Student $LOANZ goes dormant
(brainstorm #26); MIDI-ZAO-NKZ is inactive (brainstorm #27); Location and Community are delisted
as project branches and become member-profile attributes instead (brainstorm #28). ZAO Calendar,
COC Concertz and Let's Talk about Web 3 stay live, unchanged (brainstorm #27); ZTalent Newsletter
also stays live, unchanged (brainstorm #28). The "ZAO Fractals" branch itself is left exactly as
it is and deferred to Season 4 (brainstorm #29) - see Open Item 4 for the naming tension this
leaves unresolved. Future Project 1-4 are not mentioned anywhere in the brainstorm log and are
untouched by this ZIP.

### 7. Emergency council

Kept (brainstorm #29: "Emergency council: keep"). Its powers are narrow and reversible: pause a contract
or bot, pull content published in The ZAO's name, freeze a compromised wallet. Every action is
posted publicly and reversible by normal vote. It never mints Respect, spends treasury, or
revokes membership (brainstorm #30). OREC's own 72-hour vote plus 72-hour veto window means
nothing else moves in under six days regardless (brainstorm #29; contract facts corroborated in
bobbi-reply-2026-09-13.md #4).

### 8. Public points page

Live on day one alongside manifesto/join, monthly activation and the @-able bot (brainstorm #33).
Intro Respect, hosting Respect and bonus Respect display at fractal.thezao.com (brainstorm #16,
Sub-project 2). The recommended display mechanism is minting these offchain point categories as
onchain ZOR (already the ORDAO ecosystem's own pattern: mint an award, index it via `ornode`,
display it via the existing `orclient`/`gui` stack) rather than a new offchain-only ledger
(season3-protocol-survey-2026-09-11.md, Layer 4 Part C).

### 9. Timeline

Season 3 starts **1 November 2026** - the first activation lands that day (brainstorm #32). If
the activation wrapper contract is not reviewed by someone outside this estate in time, **the
season moves.** Zaal has ruled this specific tradeoff already: The ZAO does not ship unreviewed
governance code, and does not split activation into an off-chain interim step to hit the date
(brainstorm #34).

## Rationale

**Why layers, not a single membership state.** A single state cannot represent "inactive but
still a member with every point intact" at the same time as "active this month, counted for
governance." Zaal's own framing treats these as three separate questions with three separate
answers (brainstorm #1), and the code-level mess this ZIP replaces is itself evidence that
collapsing them caused the confusion in the first place (brainstorm, Measured context).

**Why Hats Protocol for achievements and membership, and why the custom wrapper for activation
rather than an off-the-shelf module.** Hats already fits nearly every locked decision directly:
sign-one-transaction joining (Agreement Eligibility), gasless minting (`claimHatFor`), soulbound
by construction, and revocable only through a governance-controlled eligibility party
(season3-hats-protocol-research-2026-09-11.md, "Fit against each Zaal decision" table). The one
place it does not fit off the shelf is exactly the one place Zaal's own decision (auto-activation,
no burn, self-service, monthly) does not resemble anything Hats or OREC ships natively -
SeasonToggle is branch-wide and admin-driven, not per-member and self-service (same source, Q3).
Building one small `IRespect` wrapper and installing it via a single OREC governance proposal is
the narrowest change that satisfies "no burn, no redeploy, no admin action per member," and OREC's
own documentation explicitly names swapping `respectContract` as its intended extension point
(season3-protocol-survey-2026-09-11.md, Layer 4 Measured code facts). The comparison against
three alternatives - an ERC20Votes-style self-delegation token, an EAS-attestation gate, and an
offchain Snapshot strategy - found each of them either solving a problem OREC does not have
(a proposal-creation-time snapshot) or breaking OREC's `msg.sender`-keyed `_vote()` call outright
(same source, Layer 4 Part B).

**Why the fractal, not a bigger redesign, first.** Zaal named the fractal as The ZAO's governance,
not one project among many (brainstorm #21). Season 3 deliberately does not touch ZAOstock,
FISHBOWLZ or ZAO OS, and defers the "ZAO Fractals" project-branch naming question to Season 4
(brainstorm #29, #31; see Open Item 4) - narrowing scope so the seven weeks from 2026-09-12 to
launch (brainstorm #32) are spent on membership and activation, not on redesigning the org's
project federation at the same time.

## Backwards Compatibility

Existing Respect holders are not migrated or reset. They mint the manifesto like anyone else, and
their Respect and history stay exactly as they are (brainstorm #5). The OG Respect (ERC-20) and
ZOR Respect (ERC-1155) ledgers are untouched by this ZIP; the activation wrapper is additive and
never writes to either (season3-protocol-survey-2026-09-11.md, Layer 4 Part B). No existing
project branch's hat is burned or deleted onchain: four branches merge or go dormant/inactive
(ZAO 101, ZAO Cards, Student $LOANZ, MIDI-ZAO-NKZ - brainstorm #26, #27), two (Location,
Community) are delisted as project branches and converted to member-profile attributes
(brainstorm #28), and one new branch (ZABAL Gamez) is added. Members who do not activate in a
given month lose nothing but that month's vote weight - they remain full members with every
point intact (brainstorm #1, #19).

## Security and Governance Considerations

### Technical Security

- **The activation wrapper is new, unaudited code on a path that determines vote weight.** It
  needs review by a Solidity reviewer outside this estate before it ships, and Zaal has already
  ruled that the season date moves rather than shipping it unreviewed (brainstorm #34;
  handoffs/season3.md).
- **Immutability vs flexibility on the Manifesto Hat.** Making the hat immutable is required for
  genuine soulbound behavior (brainstorm #3) but permanently forecloses reconfiguring its
  eligibility/toggle/admin later - a documented, deliberate Hats Protocol tradeoff, not a bug
  (season3-hats-protocol-research-2026-09-11.md, Q9).
- **OREC's existing single-relayer pattern is not fixed by this ZIP.** The last twelve fractal
  mint transactions were all sent by one address (handoffs/zaofractal.md, SUPERSEDING MEASUREMENT
  block). Season 3 does not change who can submit to OREC; it only changes what OREC reads for
  vote weight.

### Governance Risks

- **The emergency council's powers are deliberately narrow** to avoid recreating a
  single-point-of-failure admin: it cannot mint Respect, spend treasury, or revoke membership, and
  every action it takes is public and reversible by normal vote (brainstorm #30).
- **Revocation must route through governance, not a person.** The design pattern is to make OREC
  (or a hat OREC wears) the sole eligibility/arbitrator party capable of calling
  `setHatWearerStatus` on the Manifesto Hat (brainstorm #8; season3-hats-protocol-
  research-2026-09-11.md, Q4). This is not yet wired in tree 226: the top four levels have no
  automation wired at all (season3-hats-protocol-research-2026-09-11.md, Q7), though a deeper
  walk of levels 5-6 found real eligibility modules already live on some leaf hats (same source,
  Q7 2026-09-13 correction) - wiring the Manifesto Hat's own revocation path is part of this
  ZIP's build, not a future amendment.

### Process Risks

- **Naming collision.** Tree 226 already has a project branch literally named "ZAO Fractals,"
  which conflicts with brainstorm #21's decision that the fractal is the governance of The ZAO,
  not a project inside it. Season 3 leaves this branch exactly as it is and defers resolving the
  conflict to Season 4 (brainstorm #29) - see Open Item 4.
- **Scope creep risk.** Brainstorm #31 is an explicit, verbatim refusal to add ZAOstock, FISHBOWLZ
  or ZAO OS as projects under this ZIP. Any future PR that expands Season 3's scope to include
  them needs a new ZIP, not an edit to this one.

## Copyright

This ZIP is released under CC-BY-4.0.

---

## Open items

Numbered per zip-template.md's "Notes for Authors" - "mark uncertainty" rule. None of these are
invented answers: the brainstorm log either defers each by name or leaves it unanswered.

1. **The manifesto text does not exist yet.** Zaal chose the method - he rambles, the lane
   assembles strictly from his words, he reads it aloud and edits (brainstorm #35) - and brainstorm
   #6 itself says "Needs writing.", so this is a named gap, not silence. The capture session has
   not happened. This gates the 1 November launch outright (handoffs/status/zaofractal.md,
   NEEDS-ZAAL #4).
2. **Custom module budget and reviewer.** The activation wrapper needs a named external Solidity
   reviewer, and none has been assigned as of this draft (handoffs/season3.md; season3-hats-
   protocol-research-2026-09-11.md, Open Question 3).
3. **Automation gap in tree 226's top levels.** Every hat in the top four levels of the tree,
   except the Wave WarZ DAO branch, uses a non-contract sentinel address as its eligibility/toggle
   module, so nothing today can trigger automated revocation on those hats
   (season3-hats-protocol-research-2026-09-11.md, Q7). A deeper walk of levels 5-6 found real
   eligibility modules already live on many leaf hats (same source, Q7 2026-09-13 correction;
   branches 7-17 of that walk were not reached). Whether Season 3 replaces the top-level
   automation branch by branch, or starts a clean new branch for Manifesto/Provisional/Active hats
   and leaves the 17 existing project branches exactly as they are, is not decided (same source,
   Open Question 1).
4. **The "ZAO Fractals" branch name.** Deferred to Season 4 by Zaal's own words (brainstorm #29);
   this ZIP does not resolve the naming conflict with brainstorm #21 described under Process
   Risks above.
5. **The activation credential's backing store.** Brainstorm's own recommended stack lists
   "Unlock-style expiring key or wrapper for activation" - an "or," not a decision. A real Unlock
   Protocol expiring key needs less custom code and is already audited, but adds a protocol
   dependency and a per-key `extend` gas cost; a hand-rolled activation-registry contract needs
   more custom code but no external dependency. Both feed the same OREC `IRespect` wrapper either
   way, so this does not change section 3's wrapper-into-OREC mechanism, only what the wrapper
   reads from (season3-protocol-survey-2026-09-11.md, Open Question 2 and Recommended stack, row
   3b).

## Sources and References

- `projects/zao-membership-brainstorm-2026-09-11.md` - the 36 decisions log this ZIP is built
  from. Cited above as "brainstorm #N," N being the decision's row number in that file's table.
- `projects/season3-hats-protocol-research-2026-09-11.md` - Hats Protocol fit research,
  2026-09-11 and 2026-09-13 correction.
- `projects/season3-protocol-survey-2026-09-11.md` - protocol survey and recommended stack,
  2026-09-11.
- `projects/bobbi-reply-2026-09-13.md` - contract addresses and OREC parameters as stated
  publicly to a new member, 2026-09-13.
- `handoffs/season3.md` - the season3 lane's brief, including Zaal's post-launch build order.
- `handoffs/status/zaofractal.md` and `handoffs/zaofractal.md` - the fractal-bot lane's prior
  state and open NEEDS-ZAAL questions this ZIP inherits as Open Items.
- `decisions/grill-2026-09-15-morning.md` - Zaal's ruling on what counts as "attending
  something" for auto-activation, closing brainstorm #20.
