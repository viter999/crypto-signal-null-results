# Crypto Signal Null Results

A public record of failed and falsified crypto market-signal hypotheses.

This project began as an attempt to build a real-time multi-market signal system across roughly 350 Binance USDT-M perpetual markets.

The infrastructure worked.

The central trading hypothesis did not.

Across 88 days and 10,683 labeled alerts, fixed-horizon BTC-relative returns showed no economically meaningful directional edge under the tested construction.

At the 4-hour horizon:

- n = 2,791
- mean BTC-relative return = **-0.004%**
- 95% cluster-robust CI = **[-0.072%, +0.064%]**

Measured all-in taker execution cost was approximately **0.424%**.

The project also produced multiple failed forward hypotheses, a preregistered KILL verdict, several research-process failures, and a number of lessons about transaction costs, dependence, survivorship bias, and false confidence from win rates.

This repository publishes those negative results intentionally.

## Read the research

- [Full research note](research_note.md) — the complete narrative record
- [Hypotheses and verdicts](hypotheses.md) — all twelve tested hypotheses in one table
- [Methodology](methodology.md) — how claims were measured, and how to read the evidence tiers
- [Process failures](process_failures.md) — what the research process itself got wrong
- [Evidence and provenance](evidence/) — per-claim source, sample, window, and verification status

## How to read the claims

Every substantive claim in this repository is labeled with an evidence tier:

| Tier | Meaning |
|---|---|
| **Preregistered test** | Hypothesis, proxy, buckets, thresholds and decision date locked in writing *before* inference |
| **Forward test** | Evaluated on data recorded after the rule was defined, but not formally preregistered |
| **Exploratory analysis** | Found by searching the data; subject to multiple-testing risk |
| **Engineering measurement** | A measured property of the system or of execution, not a market hypothesis |
| **Retrospective interpretation** | Post-hoc reasoning about why results came out as they did |

Conflating these is the main way negative results get quietly converted into positive ones. See [methodology.md](methodology.md).

## Main conclusion

The tested alert framework did not demonstrate tradeable directional edge after realistic costs.

This does **not** imply that open interest, CVD, funding, positioning, or crypto market microstructure contain no information.

It means the specific hypotheses, constructions, horizons, rules, and datasets tested here did not establish an economically meaningful directional edge.

## Status and scope

The underlying system ran on a single small cloud VPS collecting Binance USDT-M perpetual-futures market state. This repository publishes **derived evidence and research documentation only** — no infrastructure details, credentials, operational configuration, or raw database.

The verification status of each figure is tracked in [evidence/README.md](evidence/README.md). Figures carried forward from contemporaneous project records are labeled as such and are **not** presented as independently re-derived for this publication.

## License

Documentation is licensed **CC BY 4.0**; any code under `reproducibility/` is licensed **MIT**. See [LICENSE](LICENSE).
