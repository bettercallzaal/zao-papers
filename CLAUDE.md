# CLAUDE.md - ZAO Papers

Guidelines for Claude Code contributors working in this repo.

## What This Is

**ZAO Papers** is the canonical home of The ZAO's governance documents and the ZIP (ZAO Improvement Proposal) process. It is NOT a code repo - it's a documentation repo where every fact matters.

ZIPs are governance artifacts. They must be accurate, grounded in verified sources, and honest about uncertainty. A governance document that states false facts about the community's own systems is the single most credibility-destroying error a DAO can make.

## Style & Brand

- **No emojis.** Never use emojis in any file. No decorative Unicode either. Use text labels: `[MUSIC]`, `DONE`, `IN PROGRESS`.
- **No em dashes.** Use hyphens (`-`) instead of em dashes (`-`).
- **Brand spellings (always exact):**
  - The ZAO (not "ZAO" alone, not "the zao")
  - WaveWarZ (not "Wave Wars" or "Wavewarz")
  - ZABAL Games (not "Zabal" alone)
  - Sparkz (confirmed spelling)
  - COC Concertz (space between COC and Concertz, z not s)
  - ORDAO, OREC (all caps, no spaces)
  - Respect (capitalized, refers to the token)
  - Fractal (capitalized, refers to the weekly game)
  - Farcaster (not "Warpcast")

## Accuracy Over Completeness

- **Ground everything in verified sources.** Every technical claim must cite a research doc, on-chain data, or code.
- **Mark uncertain claims as `[to confirm]`.** Better to flag a gap than speculate. Examples:
  - `[to confirm: current fractal number as of 2026-07-15]`
  - `[to confirm: whether ZAOstock fractal sessions occurred in May]`
- **Never invent facts.** Especially for:
  - Member counts (don't guess "200 members" if you only verified 156)
  - On-chain numbers (Gini, Respect supply, OREC parameters)
  - Dates and timelines (the Fractal has been running since 2024-07-30)
  - Policy decisions (don't assume intent; state what was decided)

## Verified Reference Data

Use these facts from the latest on-chain audit (Doc 981, 2026-07-05) rather than older docs:

| Metric | Value | Status |
|--------|-------|--------|
| Unique Respect holders | 156 (122 OG + 55 ZOR, 21 holders of both) | VERIFIED |
| OG Respect (ERC-20) supply | 38,484 | VERIFIED |
| OG distribution Gini | 0.73 (top 10 holders = 53% of supply) | VERIFIED |
| ZOR Respect (ERC-1155) holders | ~20+ (started at 4 early adopters in March 2026) | VERIFIED |
| Fractal weeks unbroken | ~101 since 2024-07-30 | VERIFIED |
| OREC vote + veto windows | 72 hours each | VERIFIED |
| OREC proposal threshold | 1,000 Respect (~2.6% of OG supply) | VERIFIED |
| Active Fractal participants | 6-30 per session (core ~6-10 builders) | VERIFIED |
| Respect scoring curve | 110, 68, 42, 26, 16, 10 (per rank 1-6 in 6-person group) | VERIFIED |
| OREC submissions | Only 2 wallets historically (zaal.eth, civilmonkey.eth) | VERIFIED |

If drafting a ZIP and these numbers have aged, re-verify against the latest on-chain query or research doc before publishing.

## ZIPs as Governance Artifacts

- **Preamble is load-bearing.** Every ZIP must include: number, title, author, status, created date. This metadata is the ZIP's identity.
- **Status lifecycle:** Draft -> Review -> Last Call -> Accepted / Rejected / Withdrawn. Changes to status go through the proposal process (see PROCESS.md).
- **Never silently rewrite an Accepted ZIP.** If a ratified ZIP needs an amendment, draft a new ZIP that references and modifies the original. Example: "ZIP-5: Amend ZIP-1 to update OREC parameters."
- **Ratification happens on-chain or through Fractal consensus.** Code changes on main = the ZIP is accepted. A ZIP that documents decisions already made can have its status changed to Accepted after the fact, with sources cited.

## Contributing Workflow

1. **Open a PR** with the ZIP in `zips/zip-NNNN-title.md` (or with number TBD if you don't know it).
2. **Include sources in the preamble.** Every external claim must cite research docs, contracts, or published sources.
3. **Request review** from @zaal or the ZAO governance team.
4. **Address feedback** - clarity, accuracy, completeness.
5. **Merge when approved** (not just auto-merge). The merge to main represents ratification or status change.

## Community Member Profiles

The `members/` directory holds grounded profiles of ZAO community contributors. Each profile celebrates real people and their roles in the ecosystem.

**Style for member profiles:**
- Grounded in verified sources. No invention.
- Prose format, not bullet lists. One paragraph per section.
- Mark unknown details as `[to confirm with Zaal]` rather than guessing.
- Tag context quality: rich (full story known), moderate (some gaps), thin (minimal verified facts).
- Include handles (Farcaster, X), role in The ZAO, and links to shipped work.
- For people without public details, write short and honest. A thin profile is better than a fabricated thick one.

**File structure:**
- One `.md` file per community member: `members/<handle-or-slug>.md`
- Use the template in `members/README.md`
- Status: Draft until reviewed by @zaal; Accepted after revision

## Key Files

- `README.md` - overview and process
- `CLAUDE.md` - these guidelines (including members section)
- `PROCESS.md` - the ZIP specification (EIP-1 style)
- `members/` - community member profiles (new)
- `members/README.md` - profile template and submission process
- `zips/` - governance ZIPs (ZIP-1, ZIP-2, etc.)
- `zips/zip-template.md` - boilerplate for new ZIPs
- `zips/zip-NNNN-title.md` - individual ZIPs
- `papers/` - Sparkz and ecosystem research papers
- `papers/README.md` - catalog of brands, ICM boxes, and systems
- `papers/sparkz.md` - Sparkz paper (draft)
- `papers/drafts/` - works in progress and community feedback

## Linking and References

- Cite ZAO research docs as `Doc NNN` (e.g., "Doc 703 - Fractal current state").
- Cite on-chain contracts as full address + network: `0xcB05...Be532 (Optimism)`.
- Reference GitHub repos as full URLs: `https://github.com/bettercallzaal/fractalbotmarch2026`.
- Link to files in ZAOOS as relative paths from repo root: `src/lib/respect/voteWeight.ts`.

## PII and Secrets

- **Never include personal email addresses** unless they're public ZAO contact addresses (zaal@thezao.com, hello@thezao.com).
- **Never include private keys, API keys, or secrets** of any kind.
- **Never include personal Telegram handles** of non-public individuals (bot handles like @zaoclaw_bot are OK).
- **Wallets and on-chain addresses are public** - can be included freely.

## When in Doubt

- Ask. Open a draft PR and request review.
- Check existing ZIPs for style and citation patterns.
- Re-read PROCESS.md to understand the ZIP lifecycle.
- Verify any technical claim against the source before committing.
