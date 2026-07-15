# ZAO Papers

The canonical home of The ZAO's governance documents and the ZIP (ZAO Improvement Proposal) process.

## What is This

**zao-papers** is where The ZAO community proposes, discusses, and ratifies governance changes through a formal process inspired by Ethereum Improvement Proposals (EIPs) and Bitcoin Improvement Proposals (BIPs).

ZIPs are living documents that capture:
- **Governance decisions** - changes to The Fractal, Respect distribution, treasury policy
- **Architectural proposals** - new protocols, contracts, systems (agents, on-chain infrastructure)
- **Process improvements** - how we decide, vote, and evolve as a community
- **Framework definitions** - the foundational rules and principles that define The ZAO

## The ZIP Process

A ZAO Improvement Proposal (ZIP) is a structured document that proposes a change, explains the motivation and rationale, and gathers community feedback before ratification.

See [PROCESS.md](PROCESS.md) for the complete ZIP lifecycle, status definitions, and submission requirements.

## Quick Links

- **[PROCESS.md](PROCESS.md)** - The ZIP specification and workflow
- **[zips/](zips/)** - All published ZIPs
- **[zips/zip-template.md](zips/zip-template.md)** - Template for new proposals
- **[CLAUDE.md](CLAUDE.md)** - Guidelines for contributors using Claude Code

## Key ZIPs

| # | Title | Status | Description |
|---|-------|--------|-------------|
| **1** | [The ZAO Framework](zips/zip-0001-the-zao-framework.md) | Draft | Foundational governance architecture: Fractal, Respect, Brands, Agents |

## How to Propose a ZIP

1. **Draft:** Write a ZIP using the [template](zips/zip-template.md). Keep it focused and cite sources.
2. **Open a PR:** Submit to this repo with `zips/zip-NNNN-title.md`. The repo owner will assign a ZIP number.
3. **Review:** Community feedback happens in GitHub discussions and The ZAO Discord.
4. **Ratification:** Major proposals are voted on via the Fractal (Respect-weighted) or ORDAO governance.
5. **Acceptance:** Once ratified, the ZIP status changes to Accepted. Amendments go through the same process.

## Governance Chain

```
Draft proposal (this repo)
    ↓
Community review (Discord + GitHub)
    ↓
Fractal discussion (weekly session)
    ↓
ORDAO vote (if blockchain-relevant)
    ↓
Accepted ↔ Rejected ↔ Withdrawn
```

## Accuracy First

ZIPs are governance artifacts. They must be factual, grounded in verified sources, and open about assumptions or unknowns. No speculation. Mark uncertain claims as `[to confirm]` and cite the source.

## Links

- **The ZAO:** https://thezao.xyz
- **Fractal:** Monday 6pm EST (Discord)
- **ORDAO Contract:** `0xcB05F9254765CA521F7698e61E0A6CA6456Be532` (Optimism)
- **On-Chain Governance:** Respect-weighted voting via OREC

## License

ZAO Papers are published under CC-BY-4.0. Code examples and technical specifications may be licensed separately (see individual ZIPs).
