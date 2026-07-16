# Sparkz for Music Collabs - multi-musician fee splits (draft)

**Status:** Draft (design spec)
**Created:** 2026-07-16
**Author:** Zaal Panthaki (idea), drafted via the ZAO build loop
**Extends:** [Sparkz: Configurable Creator-Coin Launcher](../sparkz.md) - specifically the "0xSplits for Fee-Splitting" section
**Board task:** `sparkz-music-collab` (P1) - "fee-splits over multiple musicians on one token."

## The one-line idea

A Sparkz collab token pays every musician on the project their share of fees automatically. The split contract already supports N recipients natively, so a band, a featured-artist track, or a producer-plus-vocalist collab becomes a first-class token type instead of a manual-accounting headache.

This is a design spec. It does not deploy anything and does not touch the wizard code (that lives in zaalcaster, owned separately). It specifies the mechanic and the "add collaborators" step so the wizard owner can implement it.

## Why this is nearly free to build

The Sparkz paper already establishes 0xSplits as the fee-routing spine. A split across many recipients is not a new contract or new code - it is the SAME 0xSplits contract with more than one row in the recipient list. Everything below is configuration on top of a rail Sparkz already uses.

## 0xSplits v2 - the grounded facts this design relies on

Verified 2026-07-16 (sources below):

- **N recipients, percentage-weighted.** Each recipient is an address plus an ownership share. Shares are percentages greater than 0 and less than 100, with up to 4 decimals of precision. The shares sum to 100 percent.
- **Mutable by a controller.** A Split can have a controller (owner) who can update the recipient list and shares without redeploying. If the controller is the zero address, the Split is immutable forever.
- **Pull-based claims.** Revenue accrues to the Split contract; each recipient claims their percentage. No push, no per-payment accounting.

Implication: a collab token is a Sparkz token whose fee receiver is a 0xSplits contract with one row per musician. Because the Split is controller-mutable, a band can add a member, remove one, or re-balance shares later without touching the token - exactly the advantage over Clanker's immutable rewardBps that the Sparkz paper already calls out.

## The collab split - design

### Split shapes offered

The wizard should offer three shapes, in order of decreasing simplicity:

1. **Equal split.** N musicians, `100 / N` each (the remainder from 4-decimal precision goes to the first recipient so the shares always sum to 100). One tap for "we split evenly."
2. **Weighted split.** Each collaborator gets an explicit percentage (lead 50, two features 25 each, etc). Live-validated to sum to 100.
3. **Role-based preset.** A starting template the creator then edits - e.g. "lead + producer + feature" seeds 60 / 25 / 15. Presets are a convenience; the numbers remain fully editable (consistent with the paper's "creator retains full control to override").

### The ZAO stake and treasury are just more rows

If the token uses the ZAO incubation model or a community-treasury cut, those are additional recipients on the SAME Split (e.g. musicians share 80 percent, community treasury 15, ZAO stake 5). No special code, no second contract - this is the paper's "ZAO community stake integrates as just another split recipient" made concrete for the multi-musician case.

### Mutable by default for collabs

Solo creator tokens may reasonably choose an immutable split ("locked and proud"). Collab tokens should default to **mutable** (a controller set), because bands change: a member leaves, a new collaborator joins, shares re-balance after a renegotiation. The controller should be:

- the band's own multisig if they have one, else
- the creator's address (with a clear in-wizard note that whoever holds the controller can re-balance the split), or
- during ZAO incubation, a ZAO-operated multisig that hands control back on graduation.

Immutability remains a one-click choice for anyone who wants it (set controller to the zero address), but it is not the default for collabs.

## The wizard "add collaborators" step (spec for the wizard owner)

A new step in the Sparkz creation flow, between "token basics" and "review". Design only - the wizard lives in zaalcaster.

```
Step: Collaborators
-------------------------------------------------
Who shares in this token's fees?

[ + Add collaborator ]   [ Split evenly ]

  1. @vocalist.eth ........... [ 50.0 % ]   [x]
  2. 0x1234...abcd (producer)  [ 30.0 % ]   [x]
  3. @feature.eth ............ [ 20.0 % ]   [x]
     community treasury ...... [  0.0 % ]   [+]
     ZAO stake ............... [  0.0 % ]   [+]

  Total: 100.0 %   [ OK ]        (turns red + blocks Next if != 100)

  [x] Allow re-balancing later (recommended for bands)
      Controller: [ band multisig / my wallet / ZAO (incubation) ]
-------------------------------------------------
```

Behaviour and validation:

1. **Add collaborator** takes an address or ENS name plus a share. Resolve ENS to an address at entry; store the address (ENS can change hands).
2. **Live sum.** The total updates on every edit. "Next" is blocked until the shares sum to exactly 100.0000 percent. Show the running total prominently.
3. **Split evenly** fills equal shares across the current rows and parks the rounding remainder on row 1.
4. **Per-recipient floor.** Reject a share of 0 (a 0 percent recipient is just noise on-chain) and reject shares at or above 100. Enforce the 4-decimal precision cap (round or reject beyond it).
5. **Duplicate guard.** The same address cannot appear twice - merge or reject.
6. **Treasury / ZAO rows** are optional toggles that add pre-labelled recipients so the creator does not have to paste those addresses by hand.
7. **Re-balancing toggle** sets whether the Split is mutable and, if so, who the controller is. Off = immutable (controller = zero address).
8. **Review step** shows the final recipient list, shares, and controller in plain language before anything is created ("These 3 wallets split fees 50 / 30 / 20; you can re-balance later.").

The AI advisor (already in the Sparkz design) should offer a recommendation here when asked - e.g. "For a 2-person band splitting evenly with room to add a producer later, use a mutable even split controlled by your shared wallet."

## Edge cases

- **A collaborator has no wallet.** Block creation until every recipient has a resolvable address; do not create a placeholder. A missing wallet is a real-world blocker, not a UX detail to paper over.
- **Member leaves after launch.** Controller updates the Split (remove the row, re-balance the rest to 100). The token is untouched; only the Split recipient list changes. This is the core advantage of the mutable-split default.
- **Someone disputes a share.** Off-chain agreement first, then the controller updates. The contract does not adjudicate; it executes whatever the controller sets. The wizard should surface who the controller is so this is understood up front.
- **Precision drift.** With N that does not divide 100 evenly, the 4-decimal cap can leave a fractional remainder; always assign it deterministically (row 1) so the shares sum to exactly 100.

## Open questions (for Zaal)

1. **Default controller for a collab with no shared multisig** - the creator's wallet, or a ZAO-operated one during incubation that hands back on graduation?
2. **Max recipients** the wizard should allow before recommending a CSV import (0xSplits supports large lists; the UX should stay sane).
3. **Preset roles** worth seeding beyond "lead / producer / feature"?
4. Should immutable ever be offered for collabs, or always mutable (bands change)?

## Sources

- 0xSplits v2 percentage precision, mutable controller, immutability at zero-address controller: [docs.splits.org SDK v2](https://docs.splits.org/sdk/splits-v2), [Splits split-contract help](https://splits.org/help/split-contract/)
- Protocol overview + pull-based claims: [splits.org protocol launch post](https://splits.org/blog/0xsplits-the-splits-protocol/), [solidnoob 0xSplits breakdown](https://www.solidnoob.com/blog/0xSplits)
- The Sparkz split model this extends: [Sparkz paper, "0xSplits for Fee-Splitting"](../sparkz.md)
