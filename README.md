<!-- solve-problem:e050e440-5df1-40ce-94ae-4151c9d1a1c5 -->
# Durable stale-block observatory

A [Tangle Solve problem](https://solve.tangle.tools/problems/e050e440-5df1-40ce-94ae-4151c9d1a1c5).

## What is the problem?

This is an infrastructure/funding problem, not a purely mathematical one. In proof-of-work and similar consensus systems, nodes keep only the canonical chain and discard competing blocks ("stale" or "orphaned" blocks), and most networks do not record which block was *first seen* at which time. This destroys forensic evidence of network-layer attacks: selfish mining, temporary block withholding, and short reorganizations leave precisely the traces that get deleted.

The goal is a **durable stale-block observatory**: a geographically distributed set of listening nodes that continuously records, with authenticated provenance and timestamps:

- all block candidates seen (canonical and stale),
- reorganization events (depth, duration, timing),
- first-seen times per observing location,

with (a) long-term retention, (b) authenticated provenance (signed attestations, verifiable timestamps, ideally multi-party signatures so no single operator can forge records), and (c) independent access for researchers.

Formally, one wants a system producing an append-only, tamper-evident dataset D = {(block hash, header, body or header-only, observer location, first-seen timestamp t_i, signature)} plus reorg logs, such that a third party can verify that a record was honestly collected. Constraints: scale (full-block capture on high-throughput chains is expensive), node diversity, clock synchronization and skew handling, and sustainable funding — the census explicitly flags funding as the blocking issue, beyond case-specific one-off datasets.

Ambiguity note: "solved" here means operational and sustainable infrastructure, not a theorem. The research content lies in the measurement methodology (provenance authentication, first-seen clock reconciliation, statistical detection of selfish mining from such data).

## Why does it matter?

Selfish mining and block-withholding detection fundamentally depends on observing stale blocks, which canonical chains discard; without such records, attacks can only be studied retroactively when someone happened to be logging. The 2026 Qubic/Monero selfish-mining campaign was analyzable only because ad-hoc artifacts existed — future attacks may not be so lucky. A standing observatory would (1) enable real-time detection and quantification of deviating mining strategies, (2) provide the empirical base for calibrating theoretical models (selfish mining profitability thresholds, network delay parameters), and (3) give regulators/exchanges evidence for incident forensics. It is shared research infrastructure: one deployment serves the entire blockchain-security community.

## What's already known?

- Stale-block measurement is established methodology: earlier efforts (e.g., DataDash-style relay-network logs, "orphan" tracking on Bitcoin via forked chain data) motivated seminal selfish-mining analyses (Eyal–Sirer 2013/2014 lineage), but these were snapshots, not sustained infrastructure.
- The key exemplar is the 2026 Qubic selfish-mining study on Monero (Lee & Kim 2026, AFT), which built case-specific logging artifacts (public GitHub repository of study data). This demonstrated both feasibility and fragility: the dataset exists but is a one-off, tied to a specific campaign and research team.
- Existing public explorers record canonical chains; mempool/relay observatories exist for Bitcoin but retention and multi-jurisdiction distribution are limited; there is no neutral, provenance-authenticated, chain-agnostic stale-block archive.
- Funding models tried: grant-funded one-offs and volunteer node operators; both fail on durability. The census status is explicitly "funding target."

(Chain-specific operational details beyond the Qubic case: verify.)

## What would count as solving it?

Any of the following, in increasing strength:

1. **Funded, operational deployment**: ≥3 geographically and jurisdictionally distinct observers continuously recording stale blocks, reorgs, and first-seen times for at least one major PoW chain, with >6 months demonstrated uptime and public data access.
2. **Methodological contribution**: a published specification of the provenance/authentication protocol (how first-seen attestations are made unforgeable; how clock skew across observers is handled and bounded) that the community accepts as a standard.
3. **Research deliverable**: a peer-reviewed paper using observatory data to detect or bound a network-layer attack, demonstrating the infrastructure's evidentiary value.
4. **Sustainability plan**: a concrete governance/funding model (consortium, endowment, protocol-level support) with commitments — the criterion being a referee/funder accepting durable operation beyond any single grant cycle.

## How to start

1. Read Lee & Kim (2026) and clone the study artifacts repo to understand what data proved decisive in the Qubic case (which fields, granularity, retention were actually used); let that drive the schema.
2. Audit existing infrastructures for reusable pieces: relay-network logs, block explorers with fork data, academic measurement testbeds (e.g., prior Bitcoin network-measurement platforms). Don't rebuild from scratch.
3. Design the provenance layer first — signed first-seen attestations from N independent observers with timestamp attestation (RFC 3161-style or witness cosigning) and a documented clock-skew bound. This is the scientific novelty; the rest is engineering.
4. Draft the sustainability model: identify funders for shared research infrastructure (e.g., protocol foundations, academic infrastructure grants), and propose a consortium governance structure with multiple independent operators.

Main obstacle: ongoing operational cost and the incentive gap — everyone benefits, no one is obligated to pay.

## Key references

- Lee, S. and Kim, J. (2026). *Inside Qubic's Selfish Mining Campaign on Monero*. AFT 2026, DOI:10.4230/LIPIcs.AFT.2026.28 — case study reconstructing a real selfish-mining campaign from non-canonical block observations; the motivating dataset. (Verify author initials.)
- Qubic selfish-mining study artifacts, https://github.com/shlee-lab/Qubic-selfish-mining-study — public dataset/code from that study; the template for what a general observatory must generalize beyond. (Verify link.)
- Eyal, I. and Sirer, E. G. (2014). "Majority is not Enough: Bitcoin Mining is Vulnerable." Financial Cryptography — the canonical selfish-mining model whose empirical calibration depends on stale-block data. (Added from general field knowledge; verify edition.)

## Warnings

- This is classified as a *funding* problem: a purely technical prototype does not close it — durability and independent governance are the acceptance criteria.
- Do not assume first-seen timestamps are trustworthy without addressing clock skew and observer collusion; provenance authentication is the hard scientific part.
- Full-block retention on high-throughput chains is costly; header-only capture plus selective full-block capture is a legitimate design compromise, but state it explicitly.
- The census references (2026 paper, repo) should be verified before relying on details; some bibliographic fields above are uncertain.

---
*`BCGT-000275` · flagged for review*

## Acceptance guidance

Any of the following, in increasing strength:

1. **Funded, operational deployment**: ≥3 geographically and jurisdictionally distinct observers continuously recording stale blocks, reorgs, and first-seen times for at least one major PoW chain, with >6 months demonstrated uptime and public data access.
2. **Methodological contribution**: a published specification of the provenance/authentication protocol (how first-seen attestations are made unforgeable; how clock skew across observers is handled and bounded) that the community accepts as a standard.
3. **Research deliverable**: a peer-reviewed paper using observatory data to detect or bound a network-layer attack, demonstrating the infrastructure's evidentiary value.
4. **Sustainability plan**: a concrete governance/funding model (consortium, endowment, protocol-level support) with commitments — the criterion being a referee/funder accepting durable operation beyond any single grant cycle.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/e050e440-5df1-40ce-94ae-4151c9d1a1c5?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: BCGT-000275.

- [Public source catalog](https://drewstone.github.io/problems/#/p/BCGT-000275)
