# Reproducibility

An honest account of what can and cannot be reproduced from this repository.

## What cannot be reproduced from here

**The underlying database is not published.** It contains identifiers associated with a private notification channel, and publishing it would expose personal data without adding evidential value. Roughly 12 GB of market-state snapshots, alert records and outcome labels therefore stay private.

This means **the numerical results in this repository cannot be independently re-derived by a third party from this repository alone.** That is a real limitation and is stated plainly rather than minimized.

What a reader *can* do is check internal consistency, check the arithmetic, check that the stated method would produce the stated quantity, and check whether the conclusions follow from the figures. The [evidence/](../evidence/) layer exists to make those checks possible.

## What is reproducible

**The arithmetic claims.** Several load-bearing results are closed-form and can be checked with nothing but the numbers in this repository:

| Claim | Check |
|---|---|
| Design effect correction | √11.1 = 3.33; √(11.1/2.5) = 2.11; √(11.1/1.9) = 2.42 |
| Effective sample size | 3,692 / 11.1 ≈ 333 |
| Interval vs cost floor | 0.23 / 0.07 ≈ 3.3; 0.424 / 0.07 ≈ 6.1 |
| Upper bound vs floor | +0.064% < 0.23% / 3 = 0.0767% |
| Take-profit scale invariance | 67.8/50.0 = 1.356; 54.2/40.0 = 1.355; 45.2/33.3 = 1.357 |
| Breakeven gap | 54.2% − 44.1% = 10.1 pp |
| Cost correction factor | 0.424 / 0.08 = 5.3 |

The take-profit row is worth checking specifically. The claim is that the breakeven-to-random-walk ratio is *identical across target multiples*, and the three independent values agreeing to three significant figures is what makes it a structural result rather than a coincidence at one point.

**The methodology.** The measurement design, the statistical treatment, and the decision criteria are described in [methodology.md](../methodology.md) in enough detail to be re-implemented against a different dataset.

**The pre-registration.** The locked hypothesis, proxy, buckets, criteria and decision date are reproduced in [evidence/trapped_long_verdict.md](../evidence/trapped_long_verdict.md). A reader can judge whether the verdict followed the declared rule.

## Scripts

The `scripts/` directory is currently empty.

It is intended for **self-contained re-derivations that do not require the private database** — for example, the zero-drift Monte Carlo that reproduces the expired-outcome survivorship artifact. That simulation needs only the barrier geometry (target at 1.5R, stop at 1.0R), a polling interval, and a volatility parameter calibrated to the observed survival rate. It requires no market data at all, and reproducing it is the cleanest available demonstration that the apparent effect needs no market edge to exist.

Adding that script is the highest-value reproducibility item outstanding.

## If the analysis code is published later

The analysis modules that produced these results exist in the (unpublished) project tree:

- `analysis/conditional_cut.py` — the preregistered verdict
- `analysis/top_ls_cut.py` — top-trader secondary lens
- `analysis/exit_replay.py` — exit-policy replay
- `analysis/rule_scoreboard.py` — rule screens
- `analysis/alpha_vs_beta.py` — BTC decomposition
- `analysis/regime_breaker.py` — regime detector and calibration replay
- `tools/btc_regime_killtest.py` — independent regime kill-test

Publishing these would require a scrub pass for paths, identifiers and configuration. They would still not be runnable without the database, but they would let a reader audit the *method* rather than take it on description.

## Verification status

Most figures in this repository are labeled `RECORDED` rather than `VERIFIED` — meaning they come from contemporaneous project records and were not re-executed for this publication. The full status vocabulary and the outstanding verification backlog are in [evidence/README.md](../evidence/README.md).

A reader deciding how much weight to place on any individual number should check its status label first.
