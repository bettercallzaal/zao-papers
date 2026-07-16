# ZIP Process - The ZAO Improvement Proposal Workflow

A ZAO Improvement Proposal (ZIP) is a design document providing information to The ZAO community, or describing a new feature for The ZAO's governance, protocols, or processes. The ZIP should provide a concise technical specification of the feature and a rationale for it.

This document defines what a ZIP is, the required structure, the status lifecycle, and the governance workflow for ratification.

## What is a ZIP

A ZIP is a proposal for governance, protocol, or process change in The ZAO. It includes:

- A clear statement of what is being proposed
- Motivation and rationale
- Technical specification (if applicable)
- Discussion of alternatives and trade-offs
- Security and governance considerations
- References to source material

ZIPs are used to propose major decisions: changes to the Fractal mechanics, treasury policy, on-chain governance parameters, new platforms or systems, or clarifications of the ZAO framework itself.

## ZIP Scope

ZIPs address:

- Governance changes (Fractal timing, Respect distribution, voting thresholds)
- Protocol specifications (new contracts, token standards, on-chain systems)
- Process improvements (proposal workflow, documentation standards)
- Framework definitions (codifying how The ZAO operates)
- Major feature proposals (new brands, agent systems, platforms)

ZIPs do NOT address:

- Individual hiring or membership decisions
- One-off event coordination (use ZAO Devz or project boards instead)
- Day-to-day operations or internal comms
- Non-binding research or exploratory docs (those live in the research/ folder of ZAOOS)

## ZIP Preamble

Every ZIP must begin with a frontmatter preamble in YAML format:

```yaml
---
zip: 1
title: The ZAO Framework
author: Zaal
status: Draft
created: 2026-07-15
last-updated: 2026-07-15
---
```

### Preamble Fields

| Field | Description | Example |
|-------|-------------|---------|
| `zip` | ZIP number (assigned by repo maintainer) | `1`, `2`, `3` |
| `title` | Short, clear title (~5-10 words) | The ZAO Framework |
| `author` | Name(s) of author(s) | Zaal, Candy, Jose |
| `status` | Current status (see below) | Draft, Review, Accepted |
| `created` | Date ZIP was first drafted (YYYY-MM-DD) | 2026-07-15 |
| `last-updated` | Date of most recent update | 2026-07-15 |

Optional but recommended:

| Field | Description |
|-------|-------------|
| `discusses` | Link to GitHub issue or Discord thread |
| `replaces` | ZIP number this supersedes (if applicable) |
| `superseded-by` | ZIP number that replaces this (once accepted) |

## ZIP Structure

After the preamble, ZIPs follow this structure:

### 1. Abstract

A short (~100 word) plain-language summary of what is being proposed. Should answer: "What is the problem and what does this ZIP propose?"

### 2. Motivation

Why is this change needed? What problem does it solve? Who does it affect?

- Explain the current limitation or gap
- Describe the impact (positive and negative)
- Reference relevant context or prior decisions

### 3. Specification

The technical or procedural details of the proposal.

- Define new terms and concepts
- Explain how it works step-by-step
- Include diagrams if helpful
- Cite any code or on-chain references

### 4. Rationale

Why this design over alternatives?

- Discuss trade-offs
- Explain decisions made
- Reference precedent (EIPs, other DAOs, research docs)

### 5. Backwards Compatibility

How does this change affect existing systems or decisions?

- Will existing members need to migrate anything?
- What breaks or changes?
- How do we minimize disruption?

If not applicable, state: "Not applicable - this is a new system."

### 6. Security and Governance Considerations

What are the risks?

- Technical security (contract exploits, economic attacks)
- Governance risks (centralization, single points of failure)
- Process risks (conflicting decisions, precedent)

### 7. Copyright

All ZIPs are published under CC-BY-4.0 by default. If proposing a different license, state it here.

Example:
```
This ZIP is released under CC-BY-4.0. Code examples are released under MIT.
```

## Status Lifecycle

Every ZIP moves through these statuses:

### Draft
- Initial proposal, still being written or gathering early feedback
- May have open questions or incomplete sections
- Open to major changes

### Review
- Submitted for community feedback
- Ready for discussion but not final
- Feedback from Discord, GitHub, or Fractal sessions incorporated

### Last Call
- Almost ready for ratification
- Open to final feedback for ~1 week
- Major changes unlikely

### Accepted
- Ratified and in effect
- Binding on The ZAO community
- Changes to an Accepted ZIP go through the full process again

### Rejected
- Community voted to not adopt this proposal
- Remains in repo for historical record
- May be revisited in future as a new ZIP

### Withdrawn
- Author decided not to pursue this proposal
- Remains in repo for historical record

## Ratification Process

How a ZIP becomes Accepted depends on scope:

### Small / Process ZIPs
- Require consensus in community discussion and Discord
- No formal vote needed if no objections after 1 week review

### Governance / Fractal ZIPs
- Ratified through The Fractal (weekly Respect-weighted consensus game)
- OR voted via ORDAO/OREC on-chain

### Protocol / On-Chain ZIPs
- Voted via ORDAO (Respect-weighted governance)
- Must pass OREC conditions: YES votes > 2x NO votes, meeting participation threshold

### Framework / Foundational ZIPs
- Ratified through Fractal discussion + community consensus
- May require explicit Zaal approval

## Amendment and Deprecation

To change an Accepted ZIP:

1. Draft a new ZIP that references the original (e.g., "ZIP-5: Amend ZIP-1")
2. Clearly state what is changing and why
3. Go through full review and ratification process
4. Once accepted, update the original ZIP's preamble: `superseded-by: 5`

To deprecate a ZIP:

1. Change status to `Withdrawn` with a note explaining why
2. Update the preamble: `superseded-by: X` if a new ZIP replaces it
3. Keep the deprecated ZIP in the repo for history

## Numbering

ZIP numbers are assigned sequentially starting from 1. The repo maintainer (currently @zaal) assigns numbers when a PR is opened.

- Do NOT assign your own ZIP number
- Do NOT renumber ZIPs after assignment
- Reserved ranges: none yet

## Submission Checklist

Before opening a PR with a new ZIP:

- [ ] Preamble includes: zip (TBD OK), title, author, status, created
- [ ] All sections present: Abstract, Motivation, Specification, Rationale, Backwards Compatibility, Security/Governance, Copyright
- [ ] Every technical claim has a source (Doc NNN, contract address, GitHub link)
- [ ] No unverified numbers (use `[to confirm]` if unsure)
- [ ] No emojis or em dashes
- [ ] Brand names spelled exactly (The ZAO, WaveWarZ, ZABAL Games, etc.)
- [ ] No PII or secrets
- [ ] File named `zips/zip-NNNN-title.md` (use 0000 if number TBD)

## Example ZIP Header

```markdown
---
zip: 1
title: The ZAO Framework
author: Zaal
status: Draft
created: 2026-07-15
last-updated: 2026-07-15
---

# ZIP-1: The ZAO Framework

## Abstract

This ZIP documents the foundational governance architecture of The ZAO: ...

## Motivation

The ZAO operates as...

## Specification

...
```

## Governance Chain

```
Proposal drafted
    ↓ (PR opened)
Community review (GitHub + Discord)
    ↓ (feedback incorporated)
Last Call period (1 week)
    ↓ (no new objections)
Ratification vote (Fractal or ORDAO)
    ↓ (passes)
Accepted (PR merged to main)
```

## Maintainers

The following people maintain this repo and the ZIP process:

- **@zaal** - Archive owner, ZIP numbering, final ratification decisions
- **ZAO Governance Team** - Community review, Fractal discussion facilitation

Questions about a ZIP? Open a GitHub issue or ask in the ZAO Discord.

## References

This process is inspired by:

- EIP-1: Ethereum Improvement Proposal Process (https://eips.ethereum.org/EIPS/eip-1)
- BIP 0002: Bitcoin Improvement Proposal Process (https://github.com/bitcoin/bips/blob/master/bip-0002.mediawiki)
- The ZAO governance research docs (Doc 703, 981, 942)

---

**Last revised:** 2026-07-15
