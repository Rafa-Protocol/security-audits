# RAFA Protocol security audits

This repository is the public record of independent smart-contract security
reviews commissioned for RAFA Protocol. Each report is published in its
original PDF form. Findings and statuses below summarize the reports; the PDFs
remain the authoritative source.

## Reports

### Grey Swan - final re-audit (July 2026)

[Read the final Grey Swan report](./audits/2026-07-grey-swan-final-reaudit.pdf)

| Field | Detail |
| --- | --- |
| Status | Final re-audit |
| Review period | July 1-22, 2026 |
| Initial commit | [`572e1a7`](https://github.com/Rafa-Protocol/rafa-fund/commit/572e1a7c98f0abad5d3996d920e9a9062b192f89) |
| Remediation commit | `c84f219b10a92e3415f0283c072e9a02341208a1` |
| Scope | The earlier `BaseETF`, `FundFactory`, Aerodrome interface, deployment configuration and deployment script |

Grey Swan reported 12 items in the initial review: 1 Critical, 2 High, 2
Medium, 3 Low and 4 architectural considerations. The final re-audit records
10 items as resolved or mitigated and 2 architectural considerations as
acknowledged or pending. No Critical, High, Medium or Low finding remained open
in the final report.

The Grey Swan review applies to the earlier prototype contracts at the commits
listed above. Those prototypes were subsequently removed when RAFA Fund
Protocol became the only supported implementation; this report does not claim
coverage of the current `RafaFundV2` contract set.

### DatSon360 - September 2026 review (draft v0.1)

[Read the DatSon360 draft](./audits/2026-09-datson360-audit-draft-v0.1.pdf)

| Field | Detail |
| --- | --- |
| Status | Draft v0.1 - preliminary, no fixes reviewed |
| Review period | September 28-October 4, 2026 |
| Audited commit | [`2ddd1c3`](https://github.com/Rafa-Protocol/rafa-fund/commit/2ddd1c386e29a6d6d63eae080f48f0d417d54f61) |
| Remediation commit | Pending in the supplied draft |
| Scope | `RafaFundV2`, `RafaAssetRegistry`, `FundFactoryV2`, `ChainlinkPriceOracle`, Aerodrome and Uniswap V3 adapters, interfaces and deployment tooling |

The supplied DatSon360 document is explicitly marked **Draft v0.1, not for
distribution**. It is retained here as a transparent record of the September
review period, not as a final audit or remediation verification.

The draft lists 12 preliminary findings: 0 Critical, 1 High, 2 Medium, 5 Low
and 4 Informational. All 12 are marked open in the document, and the report says
that severities, counts and wording may change before a final report.

## At a glance

| Auditor | Report status | Scope generation | Critical | High | Medium | Low | Informational / considerations | Open or pending in report |
| --- | --- | --- | ---: | ---: | ---: | ---: | ---: | ---: |
| Grey Swan | Final | Earlier prototype | 1 | 2 | 2 | 3 | 4 | 2 considerations |
| DatSon360 | Draft v0.1 | Current protocol at `2ddd1c3` | 0 | 1 | 2 | 5 | 4 | 12 preliminary findings |

## File integrity

| File | SHA-256 |
| --- | --- |
| `2026-07-grey-swan-final-reaudit.pdf` | `1d5c4ab0e679fe3b8b38d721f3e6b047ad71e9bb8b2b34c0c11b370336e42311` |
| `2026-09-datson360-audit-draft-v0.1.pdf` | `6644275f28cfe791fdd328d2cc59f6b13b634eb8573d5ce20ed79001e2a1b483` |

## Important limitation

A security audit is a time-bounded review of specific code. It is not a
guarantee that a system is free of defects. Users should verify the audited
commit, deployed bytecode, current contract addresses and subsequent code
changes before relying on any report.

Protocol source: [Rafa-Protocol/rafa-fund](https://github.com/Rafa-Protocol/rafa-fund)
