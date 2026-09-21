# Methodology

How claims in this repository were produced, and how to read them.

## Evidence tiers

Every substantive claim is labeled with one of five tiers. The distinction matters because the most common way a negative result quietly becomes a positive one is by promoting an exploratory finding to the status of a confirmatory test after the fact.

| Tier | Definition | What it licenses |
|---|---|---|
| **Preregistered test** | Hypothesis, proxy, bucket definitions, pass/kill criteria and decision date were committed in writing *before* any inference was run on the relevant data | A genuine confirmatory verdict |
| **Forward test** | Rule defined first, then evaluated on data recorded afterward — but not formally preregistered | Reasonable evidence; still subject to selection over which rules were tried |
| **Exploratory analysis** | Found by searching the data | Hypothesis generation only; never a verdict |
| **Engineering measurement** | A measured property of the system, its data, or execution — not a market hypothesis | Fact about the apparatus, not about markets |
| **Retrospective interpretation** | Post-hoc reasoning about why results came out as they did | Explanation, not evidence |

Only two hypotheses in this project reached **Preregistered test** status: the trapped-long conditional cut (Hypothesis 1) and the BTC-regime rule (Hypothesis 2).

## Measurement design

**Recording was separated from notification.** Notification gates — direction blocks, funding alignment, tier floors, regime filters, circuit breakers — decided only whether an alert was *sent*. Database writes ran regardless. This means the research sample includes alerts that were never seen or acted on, which removes the most obvious source of survivorship bias.

**Outcomes were recorded mechanically.** Each alert's realized outcome, plus maximum favorable and maximum adverse excursion, was written by the system rather than judged after the fact.

**Two evaluation modes were used, and they answer different questions:**

1. *Barrier resolution* — did price touch the target or the stop first? This is the natural trading framing and was the project's original metric. It is contaminated by barrier geometry.
2. *Fixed-horizon return* — where was price at 1h / 4h / 12h / 24h, with BTC beta removed? This measures directional information directly and is the basis of the headline result.

Mode 2 supersedes mode 1 for questions about whether a signal predicts direction. Mode 1 remains valid for questions about how a specific trade construction would have performed.

## Statistical approach

**Cluster-robust inference.** Alerts arrive in regime clusters and are not independent. Confidence intervals on pooled cohorts are computed with day-level clustering.

**Design effect and effective sample size.** The measured day-clustered design effect on the pooled DUAL-short cohort was **11.1**, against an earlier assumed 1.9–2.5. Effective n is raw n divided by the design effect — approximately 330 rather than 3,692. Standard errors scale with the square root of the design effect, so intervals computed assuming independence were roughly 3.3× too narrow, and intervals using the earlier assumption were still roughly 2.1–2.4× too narrow.

A runs test on the DUAL-short outcome sequence gave **Z = −10.68**, decisively rejecting independence.

**Wilson intervals on effective n.** Where a proportion's confidence floor is used as a decision criterion, the interval is computed on effective n, not raw n. The verdict tooling originally violated this; see [process_failures.md](process_failures.md).

**Fragility checks.** The final verdict applied a leave-one-day-out check: if dropping any single day flips a decision clause, the margin is treated as unaffordable. This is what converted Hypothesis 1 from a face-value pass into a KILL.

**Multiple-testing awareness.** The project ran on the order of 81 tests against this dataset. A nominally significant isolated finding at that count carries an extreme family-wise false-positive risk, and was not treated as actionable.

## Cost basis

Two cost figures appear throughout, and they are deliberately not interchangeable:

| Basis | Value | Use |
|---|---|---|
| Preregistered | 0.08% fee + 0.15% slippage = **0.23%** | Locked before the verdict; used so the preregistered criterion stays computable exactly as declared |
| Measured | **0.424%** all-in | Measured from 7,109 real taker fills; reported alongside as a separate column |

The preregistered constant was **not** retroactively changed, because doing so would have altered a locked criterion mid-experiment. Instead the measured cost was reported in parallel. The direction of the correction is one-way safe: raising assumed cost can only demote a cohort, never promote one, so nothing previously killed could revive.

## Decision standards

The standard for adopting a rule and the standard for declining one were deliberately asymmetric:

> **Adopting** a trading rule requires strong evidence — a confidence floor clearing the cost floor, on effective n, robust to single-day removal.
>
> **Declining** to adopt a rule is cheap. A weak or noisy negative result is sufficient.

This asymmetry is why Hypotheses 6–10 are labeled "not promoted" rather than "disproven." At n = 23–58 those estimates are imprecise. They were enough to avoid adding complexity; they are not enough to claim the underlying ideas are false.

## Known limitations

- **Single venue.** All data is Binance USDT-M perpetual futures. Nothing here speaks to other venues or instruments.
- **One macro episode.** The forward window covers a period in which BTC fell approximately 19.2%. Conclusions about regime-conditional behavior are limited by having essentially one regime episode.
- **Liquidation fields are estimates.** They are reconstructed from candle data, not the forced-order tape, and should not be treated as observed liquidations.
- **A collection gap exists.** Roughly 14.5 hours on 2026-07-01 (02:34–17:06 UTC) were not recorded.
- **Pre-June data is excluded** for both data-quality (laptop-sleep contamination) and research-integrity (already heavily explored) reasons.
- **Expired outcomes are excluded** from resolved win/loss analysis because they are a survivorship-filtered sample, not a random one.
