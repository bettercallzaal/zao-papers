---
zip: TBD
title: [Your Title Here]
author: [Your Name]
status: Draft
created: YYYY-MM-DD
last-updated: YYYY-MM-DD
---

# ZIP-TBD: [Your Title Here]

## Abstract

A concise 1-2 sentence summary of what is being proposed and why.

Example: "This ZIP proposes to increase the weekly Respect distribution by 20% to better incentivize participation from non-core members."

Keep this under 100 words. It should answer: What problem does this solve?

## Motivation

Why is this change needed? What problem does it solve?

- Describe the current limitation or gap
- Explain how it affects The ZAO community
- Reference relevant context (prior decisions, research, feedback)
- Make a clear case for action

## Specification

The technical or procedural details of the proposal. Be precise and concrete.

- Define any new terms or concepts
- Explain step-by-step how it works
- Include diagrams or examples if helpful
- Reference on-chain contracts, code, or systems by full path/address
- Use verified numbers from the latest on-chain audit (see CLAUDE.md)

### Example Subsections

- How does this integrate with existing systems?
- What are the implementation steps?
- What data structures or contracts are needed?
- What are the parameters?

## Rationale

Why this design over alternatives?

- Discuss trade-offs: what are we gaining and losing?
- Explain why you chose this approach
- Reference precedent in other DAOs or research (EIPs, Fractally, etc.)
- Cite research docs (Doc NNN format)

Example: "We chose a 20% increase over 50% because Doc 703 showed that participation plateaued at similar incentive levels in other Fractals."

## Backwards Compatibility

How does this change affect existing systems?

- Will existing members need to migrate or take action?
- What breaks or changes?
- How do we minimize disruption?
- Is there a transition period?

If not applicable, state: "Not applicable - this introduces a new system that does not affect existing operations."

## Security and Governance Considerations

What are the risks and how do we mitigate them?

### Technical Security

- Could this be exploited? (e.g., gaming Respect, flashloan attacks, etc.)
- What are the contract-level risks?

### Governance Risks

- Does this centralize decision-making?
- Are there single points of failure?
- Could this inadvertently change who has power?

### Process Risks

- Does this conflict with existing policies or ZIPs?
- What precedent does this set?

## Copyright

This ZIP is released under CC-BY-4.0.

---

## Notes for Authors

- **Be accurate.** Every technical claim must cite a source (Doc NNN, contract address, GitHub, etc.)
- **Mark uncertainty.** Use `[to confirm]` for claims you haven't verified.
- **Avoid speculation.** Don't guess at numbers; look them up or say [to confirm].
- **No emojis or em dashes.** Use text labels and hyphens.
- **Brand spellings:** The ZAO, WaveWarZ, ZABAL Games, Sparkz, ORDAO, OREC, Respect, Fractal
- **PII:** Don't include personal emails, phone numbers, or home addresses.

## Submission

When ready to propose:

1. Rename this file to `zip-NNNN-title.md` (use 0000 if unsure of number)
2. Fill in all sections completely
3. Review against the checklist in PROCESS.md
4. Open a PR to this repo
5. The maintainer will assign a ZIP number and move to Review status
