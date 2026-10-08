---
zip: 2
title: The ZIP System
author: Zaal
status: Draft
created: 2026-10-08
last-updated: 2026-10-08
---

# ZIP-2: The ZIP System

## Abstract

A ZIP is a ZAO Improvement Proposal. It is how The ZAO writes down a decision so anyone can read it, question it and change it in the open. This ZIP explains the system in one place: what a ZIP is, how one gets proposed, numbered, discussed and accepted, and where ZIPs sit next to the Fractal and Respect. It adds no new rules. Everything here is already written in PROCESS.md, ZIP-1 or this repo's README. Where those sources disagree or leave a gap, this ZIP says so and leaves the question open for Zaal.

## Motivation

The rules for ZIPs are spread across four files: PROCESS.md, README.md, the template and ZIP-1. They mostly agree, but not on everything, and none of them explains the whole system in plain words.

That matters now. Zaal ruled on 2026-10-08: "lets merge it then rewrite ZIP 2 into explaining the ZIP system then Ill make a ZIP for each project we have" (vault `decisions/grill-2026-10-08-grill-morning.md`, item 3). Before there is a ZIP for every project, people need to know what a ZIP is and what it is not.

## Specification

### 1. What a ZIP is

A ZIP is a design document for The ZAO. It either proposes a change to how The ZAO governs and runs, or describes a system as it already is (PROCESS.md, "What is a ZIP").

ZIPs can cover governance changes, protocol and onchain specs, process changes, framework definitions, and major features like new brands, agents or platforms (PROCESS.md, "ZIP Scope").

ZIPs do not cover hiring or membership calls about one person, one-off event coordination, day to day operations, or research. Research lives in the ZAOOS research folder (PROCESS.md, "ZIP Scope").

A ZIP can be descriptive. ZIP-1 is: it writes down The ZAO as it stood in July 2026 and changes nothing (ZIP-1, "Backwards Compatibility").

### 2. What every ZIP looks like

Every ZIP opens with a preamble: number, title, author, status, created date, last updated date. Optional fields are `discusses`, `replaces` and `superseded-by` (PROCESS.md, "ZIP Preamble").

Then seven sections, in order: Abstract, Motivation, Specification, Rationale, Backwards Compatibility, Security and Governance Considerations, Copyright (PROCESS.md, "ZIP Structure"). Start from `zips/zip-template.md`.

Every claim cites a source: a research doc, a contract, code or a decision. If something is not verified, it is marked `[to confirm]` instead of guessed (CLAUDE.md, "Accuracy Over Completeness").

No emojis, no em dashes, brand names spelled exactly. ZIPs are CC-BY-4.0 unless they say otherwise.

### 3. How a ZIP is proposed

1. Copy the template and write the ZIP.
2. Save it as `zips/zip-0000-title.md`. Zero means no number yet.
3. Open a pull request on this repo.

(PROCESS.md, "Submission Checklist"; template, "Submission"; README, "How to Propose a ZIP".)

### 4. How a ZIP is numbered

Numbers go in order from 1. The repo maintainer, currently Zaal, assigns the number when the PR is opened. Authors do not pick their own number, and a number is never changed once given (PROCESS.md, "Numbering").

### 5. How a ZIP is discussed

Review happens on the GitHub PR and in The ZAO Discord, and can come up in the weekly Fractal (PROCESS.md, "Review"; README, "How to Propose a ZIP"). The `discusses` field links to the thread where the talk happens.

### 6. The status a ZIP moves through

- **Draft** - being written, open to big changes.
- **Review** - ready for community feedback.
- **Last Call** - close to done, about one week for final feedback.
- **Accepted** - in effect and binding on The ZAO.
- **Rejected** - not adopted, kept for the record.
- **Withdrawn** - the author dropped it, kept for the record.

(PROCESS.md, "Status Lifecycle".) As of 2026-10-08 no ZIP has reached Accepted. ZIP-1 and the Season 3 ZIP are both Draft (`status:` lines in `zips/`).

### 7. How a ZIP is accepted

How a ZIP is ratified depends on what kind it is (PROCESS.md, "Ratification Process"):

- **Small or process ZIPs** - community consensus. If nobody objects after one week of review, no formal vote is needed.
- **Governance or Fractal ZIPs** - ratified through the Fractal, or voted on through ORDAO/OREC onchain.
- **Protocol or onchain ZIPs** - voted through ORDAO. They must pass OREC's conditions: YES weight more than twice NO weight, and at least 1,000 Respect voting YES.
- **Framework ZIPs** - Fractal discussion plus community consensus, and may need Zaal's explicit approval.

Once ratified, the PR merges and the status changes to Accepted (PROCESS.md, "Governance Chain").

### 8. How a ZIP is changed

An Accepted ZIP is never quietly rewritten. To change it, write a new ZIP that names the old one, for example "Amend ZIP-1", and take it through the full process. When the new one is accepted, the old one gets `superseded-by` in its preamble. To retire a ZIP, mark it Withdrawn and keep it in the repo (PROCESS.md, "Amendment and Deprecation"; CLAUDE.md, "ZIPs as Governance Artifacts").

A Draft is different. Drafts change all the time, and that is how this ZIP came to replace the old ZIP-2 text (see Backwards Compatibility).

### 9. Where ZIPs sit in The ZAO's governance

The Fractal is the governance of The ZAO. Zaal: "ZAO Fractal is the governance of The ZAO. Not a project inside it: the fractal's Respect and OREC decide for all of The ZAO. Projects sit under it" (Season 3 ZIP, Motivation, quoting brainstorm #21).

So a ZIP does not replace the Fractal or OREC. It is the written record that they decide on. Every week the Fractal ranks contributions and hands out Respect. Respect is the vote weight in OREC, with a 72 hour voting window and a 72 hour veto window (ZIP-1, section 2; the thezao ICM box, "Governance"). A ZIP is the text a governance vote points to, and the place the result is kept.

That is also why there will be a ZIP for each project. Projects sit under the Fractal, and a project ZIP writes down what that project is so the Fractal has something real to decide on. The project ZIPs are Zaal's to write. This ZIP does not draft them.

## Rationale

The system follows EIP-1 and BIP-2 (PROCESS.md, "References"), because they are proven and people already know them. This ZIP only gathers what is written. It does not invent process, because a governance document that states something nobody decided is worse than one that admits a gap (CLAUDE.md, "What This Is").

## Backwards Compatibility

Before this rewrite, ZIP-2 was "Season 3 - The Fractal Season" (PRs #9 and #13). That text is unchanged and now lives at `zips/zip-0000-season-3.md` with its number set to TBD, which is how the template marks a ZIP with no number yet. Season 3 was a Draft, so moving it breaks nothing that was in effect. Its new number is Zaal's to assign (see Open Questions).

Nothing in PROCESS.md, ZIP-1 or the template changes.

## Security and Governance Considerations

### Governance risks

Every acceptance path except the onchain one runs through one maintainer. Zaal assigns numbers and makes the final ratification calls (PROCESS.md, "Maintainers"). That is the same single point of failure ZIP-1 flags for OREC, where only two wallets have ever submitted (ZIP-1, section 2.4).

### Process risks

The written sources disagree on what a merge means. CLAUDE.md says "Code changes on main = the ZIP is accepted" and "The merge to main represents ratification or status change." PROCESS.md says a ZIP is ratified by Fractal or ORDAO first, and the merge comes after. ZIP-1 already names this ("Ratification by merge") and leaves it open. In practice, Drafts have been merged to main (ZIP-1, the Season 3 ZIP), so today a merge does not mean accepted. Until Zaal rules, read the `status:` field, not the merge.

## Open Questions for Zaal

1. **What does a merge mean?** Is a ZIP accepted when it merges, when the Fractal or ORDAO ratifies it, or only when its status says Accepted? (See Process risks.)
2. **Who can merge, and who is the "ZAO Governance Team"?** PROCESS.md names this team for review and Fractal facilitation, but no file lists its members.
3. **Where does discussion live?** ZIP-1 points `discusses` at issue #1 in this repo. As of 2026-10-08 this repo has no issues (`gh issue list --state all` returns none). Is a ZIP's discussion the PR, a GitHub issue, a Discord thread or the Fractal?
4. **What number does Season 3 get?** It moved to `zip-0000` in this PR. Is it ZIP-3, or does it wait until the project ZIPs are numbered?
5. **What kind of ZIP is a project ZIP?** Descriptive like ZIP-1, or a proposal that goes to a vote? That decides which acceptance path in section 7 it takes.
6. **How does a ZIP reach OREC?** PROCESS.md says protocol ZIPs are "Voted via ORDAO", but nothing written says how a ZIP becomes an OREC proposal or who submits it.

## Copyright

This ZIP is released under CC-BY-4.0.

---

## Sources

| Source | What it gives |
|--------|---------------|
| `PROCESS.md` | Definition, scope, preamble, structure, status, ratification, amendment, numbering |
| `README.md` | Proposal steps, review venues |
| `CLAUDE.md` | Accuracy rules, merge-as-ratification wording, amendment rule |
| `zips/zip-template.md` | Sections, `0000` naming |
| ZIP-1 | Fractal, Respect, OREC parameters, signer risk, "Ratification by merge" gap |
| Season 3 ZIP (`zips/zip-0000-season-3.md`) | "ZAO Fractal is the governance of The ZAO" (brainstorm #21) |
| `zao-icm` `boxes/thezao.llm.txt` | Governance summary: Respect, weekly game, OREC windows |
| vault `decisions/grill-2026-10-08-grill-morning.md` item 3 | Zaal's ruling to rewrite ZIP-2 |
