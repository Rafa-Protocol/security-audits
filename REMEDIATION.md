# DatSon360 draft v0.1 remediation record

Published: October 5, 2026

Status: RAFA remediation implemented and merged; independent DatSon360 re-review
pending. This is RAFA's engineering response and must not be presented as an
auditor-issued final report or auditor-verified closure.

## Immutable references

| Item | Reference |
| --- | --- |
| Original report | [DatSon360 draft v0.1](./audits/2026-09-datson360-audit-draft-v0.1.pdf) |
| Audited source | [`2ddd1c3`](https://github.com/Rafa-Protocol/rafa-fund/commit/2ddd1c386e29a6d6d63eae080f48f0d417d54f61) |
| Remediation source | [`a32f98a`](https://github.com/Rafa-Protocol/rafa-fund/commit/a32f98ae20e96fd61aa1b4832d24101d6668616e) |
| Complete source diff | [`2ddd1c3...a32f98a`](https://github.com/Rafa-Protocol/rafa-fund/compare/2ddd1c386e29a6d6d63eae080f48f0d417d54f61...a32f98ae20e96fd61aa1b4832d24101d6668616e) |
| Review and CI | [Rafa Fund PR #2](https://github.com/Rafa-Protocol/rafa-fund/pull/2) |
| Main merge | [`d6f4d09`](https://github.com/Rafa-Protocol/rafa-fund/commit/d6f4d090a529f4724ac172e8948c27eed9de0c4e) |
| Internal engineering record | [`SECURITY_REVIEW.md`](https://github.com/Rafa-Protocol/rafa-fund/blob/a32f98ae20e96fd61aa1b4832d24101d6668616e/SECURITY_REVIEW.md) |
| Production operations runbook | [`OPERATIONS.md`](https://github.com/Rafa-Protocol/rafa-fund/blob/a32f98ae20e96fd61aa1b4832d24101d6668616e/OPERATIONS.md) |

No Base or Ethereum mainnet contract was deployed as part of this remediation.
The historical Base Sepolia contracts predate it and are explicitly marked
superseded for security evaluation.

## RAFA finding dispositions

The status in this table is RAFA's position only. The auditor must independently
confirm each item against the remediation commit.

| Finding | Severity in draft | RAFA disposition | Remediation evidence |
| --- | --- | --- | --- |
| HIGH-01: compromised trader can drain through repeated max-slippage trades | High | Mitigated; re-review pending | Official-fund slippage capped at 2%; rolling 24-hour notional capped at 100% of NAV; cumulative oracle-relative loss above 1% of NAV auto-pauses trading; trade-value limit can only decrease. |
| MED-01: NAV arbitrage from Chainlink latency | Medium | Mitigated; re-review pending | Six-hour cash-exit and transfer delay for newly minted shares; settlement-fresh prices required for entry and cash exit even at zero performance fee; immediate in-kind exit preserved. |
| MED-02: registry owner can instantly replace an oracle or adapter | Medium | Fixed in code; re-review pending | Asset must have admission and buying disabled; proposal is public for 48 hours; execution requires the asset to remain disabled and does not re-enable it. |
| LOW-01: settlement freshness was a side effect of fee accrual | Low | Fixed in code; re-review pending | Every price-dependent entry and cash-exit path calls strict settlement valuation explicitly. |
| LOW-02: dust donation can block asset removal | Low | Fixed in code; re-review pending | Disabled assets with no claims may sweep no more than $0.01-equivalent dust; claims and larger balances still block removal. |
| LOW-03: in-kind exit may skip performance fees during oracle outage | Low | Acknowledged design; re-review pending | Oracle-independent emergency exit is retained; NatSpec and the emitted event disclose the possible skipped fee. |
| LOW-04: configured token decimals were not verified | Low | Fixed in code; re-review pending | Registry verifies `decimals()` when token bytecode exists; zero-code exception retained for Base B20 native tokens. |
| LOW-05: official-fund registration omitted slippage and admin-delay checks | Low | Fixed in code; re-review pending | Factory rejects slippage above 2% and default-admin transfer delays below one day. |
| INFO-01: floating Solidity pragma | Informational | Fixed | Project contracts and interfaces pin Solidity `0.8.34`. |
| INFO-02: oracle lacked clamps and used deprecated `answeredInRound` semantics | Informational | Fixed in code; re-review pending | Immutable minimum/maximum price bounds added; deprecated completeness check removed while answer and timestamp validation remain. |
| INFO-03: exposure failure used the deposit-cap error | Informational | Fixed | Dedicated `ExposureLimitsBreached` error added. |
| INFO-04: factory event was named `FundCreated` although it registers | Informational | Fixed | Event renamed `FundRegistered`. |

## Validation supplied for re-review

- Production build uses Solidity `0.8.34`, Cancun EVM target, and optimizer 200.
- GitHub verification for PR #2 passed.
- Local full check passed: production build, bytecode-size gate, 30 Hardhat
  tests, and TypeScript typecheck.
- Production dependency advisory scan reported zero known vulnerabilities.
- Focused Slither high-impact scan reported no high-impact findings; broader
  detector warnings were triaged in the internal engineering record and remain
  available for auditor review.
- `RafaFundV2` runtime bytecode is 23,287 bytes, 1,289 bytes below the EVM limit.
- Tests cover cumulative adverse fills, rolling-notional exhaustion, zero-fee
  stale settlement, registry timelock and disabled state, decimals mismatch,
  factory safety gates, dust griefing, malicious adapters, restricted-token
  claims, donation attacks, and accounting invariants.

## Residual risks requiring explicit review

1. A compromised trader may still realize bounded loss within a risk window;
   monitoring and rapid role revocation remain required.
2. In-kind exits intentionally bypass the cash-exit delay and may skip fee
   crystallization when settlement oracles are unavailable.
3. First-time asset configuration and asset status changes remain immediate Safe actions.
4. Price bounds limit extreme oracle outputs but do not establish economic correctness.
5. Production Safe addresses, thresholds, feeds, venues, alerts, and exact-asset
   fork rehearsals are deployment controls, not properties proven by this code review.

## Requested auditor action

DatSon360 should re-review commit `a32f98a`, confirm or revise every finding's
severity and status, review the documented residual risks, and issue a final
report tied to the exact reviewed commit. When received, the final PDF will be
added without replacing or rewriting the original draft.
