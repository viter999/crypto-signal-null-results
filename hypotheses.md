# Hypotheses and Verdicts

Every hypothesis tested during the project, with its evidence tier and outcome. Nothing tested has been omitted; the numbering is contiguous so that a missing entry would be visible.

Tier definitions are in [methodology.md](methodology.md).

## Summary table

| # | Hypothesis | Tier | n | Result | Verdict |
|---|---|---|---:|---|---|
| 1 | Trapped longs produce tradeable liquidation overshoot | Preregistered | eff-n 127 | Cleared thresholds on face; failed fragility check | **KILL** |
| 2 | BTC downtrend filtering improves short alerts | Preregistered | 284 pooled | 0 of 4 forward windows passed; 70.0% → 50.0% | **Rejected** |
| 3 | Better exit design rescues the signal | Retrospective | 3,046 | Every pessimistic cell negative at measured cost | **Refuted** |
| 4 | Moving the take-profit target rescues the strategy | Retrospective + arithmetic | — | Deficit is scale-invariant at ~1.355× | **Rejected by arithmetic** |
| 5 | Higher signal tier means higher independent quality | Exploratory | — | Tier proxies regime, not quality | **Rejected as distinct edge** |
| 6 | Engine agreement improves performance | Forward | 122 | −6.6 pp lift | **Not promoted** |
| 7 | CVD divergence predicts the next move | Forward | 41 | −8.3 pp lift | **Not promoted** |
| 8 | Breakout z-score identifies better trades | Forward | 58 | −11.7 pp lift | **Not promoted** |
| 9 | Recommended-take signals outperform | Forward | 24 | −31.8 pp lift | **Not promoted** |
| 10 | Long-side edge exists in a specific tier | Forward | 23 | −31.7 pp lift; 21/23 fires confounded | **Not promoted (confounded)** |
| 11 | Signal persistence identifies stronger opportunities | Forward | — | 26.7% fire vs 40.8% skip (−14.1 pp) | **Rejected as edge; retained as diagnostic** |
| 12 | Long alerts are tradeable | Forward | 117 | ~35% WR, EV ≈ −0.175% | **Rejected** |

## The overarching test

Beyond the twelve rule-level hypotheses, the decisive measurement was a direct test of whether alerts carried directional information at all, with barrier mechanics removed.

| Claim | Tier | n | Result |
|---|---|---:|---|
| Alerts carry detectable BTC-relative directional return | Forward (not preregistered) | 2,791 at 4h | Mean −0.004%, CI [−0.072%, +0.064%] — **no detectable edge** |

This is the result that makes hypotheses 3, 4 and 5 moot rather than merely unsuccessful: if gross directional return is indistinguishable from zero, no exit policy, target placement or tier filter can produce positive expected value.

## Results that were interesting but not promoted

| Finding | Tier | Why not promoted |
|---|---|---|
| 13:00–21:00 UTC outperformance (+0.130%, t = 2.17) | Exploratory | Effect smaller than the cost floor; ~test #81, so family-wise error risk is extreme |
| Top-trader positioning monotonic in win rate | Disclosed secondary lens | EV after slippage was **not** monotonic; no clean economic hypothesis |
| Trapped-long concentration shape (median Δ ≈ 5× mean Δ) | Preregistered, post-verdict observation | The shape is real, but the preregistered verdict was KILL; any future use requires a fresh declaration |

## Reading the verdicts correctly

**"Not promoted" is not "disproven."** Hypotheses 6–10 were evaluated under a deliberately asymmetric standard: adopting a rule requires strong evidence, declining one is cheap. At n = 23–58 the confidence intervals are wide. These are promotion screens, not refutations.

**"Rejected by arithmetic" is stronger than an empirical rejection.** Hypothesis 4 fails from a closed-form relationship between breakeven and random-walk hit rates, not from a noisy estimate. More data cannot change it.

**"KILL" is the preregistered decision word** for Hypothesis 1 and carries the specific meaning defined in the original registration: the trapped-long alpha claim is retired under the declared rule. Re-reading other cuts or windows does not overturn it.
