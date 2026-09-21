# What Worked, What Failed
## A Forward-Tested Crypto Signal Research Record

This document summarizes a multi-month empirical investigation into whether a real-time crypto market scanner could identify tradeable directional edge.

The infrastructure worked.

The measurement system worked.

Several relationships looked promising during development.

But the central predictive hypotheses did not survive forward testing, realistic transaction costs, dependence-aware statistics, or robustness checks.

The purpose of publishing this is to preserve the negative results rather than publishing only successful-looking experiments.

> **Reading the evidence tiers.** Each major section is labeled *Preregistered test*, *Forward test*, *Exploratory analysis*, *Engineering measurement*, or *Retrospective interpretation*. The definitions are in [methodology.md](methodology.md); per-claim provenance is in [evidence/](evidence/).

---

# Executive Summary

The system monitored roughly 350 Binance USDT-M perpetual markets and generated structured alerts using market-state information including:

- price
- volume
- open interest
- funding
- account long/short positioning
- top-trader positioning
- CVD-related measures
- compression
- estimated liquidation-related activity
- BTC market regime
- multi-signal confluence

Every alert was mechanically recorded whether or not it was sent to a notification channel or manually traded.

That distinction became more valuable than the scanner itself.

The final conclusion was:

> The tested alert framework did not demonstrate tradeable directional edge after realistic costs.

This conclusion did not come from one failed strategy.

It emerged after multiple forward tests, rejected rule additions, corrected cost assumptions, dependence adjustments, placebo-style checks, and a final preregistered conditional hypothesis.

Some aspects of the process also failed.

Those failures are included here because they materially changed how the research was conducted. They are collected in [process_failures.md](process_failures.md).

---

# What Worked

## 1. Forward measurement infrastructure

*Engineering measurement.*

The most useful part of the project was not the alerts.

It was the ability to record signals mechanically and evaluate them later.

Notification rules were separated from measurement.

An alert could be suppressed by:

- direction filters
- funding alignment
- tier requirements
- BTC regime filters
- circuit breakers

while still being written to the research database.

This reduced the risk of evaluating only alerts that happened to be noticed, sent, or traded.

---

## 2. Multi-market data collection

*Engineering measurement.*

The system was designed to run continuously across hundreds of perpetual-futures markets.

It collected and stored market-state information for later analysis through:

- async collectors
- SQLite in WAL mode
- a FastAPI service
- a monitoring dashboard
- notification infrastructure
- mechanical outcome recording
- persistent research tables

Collection was not perfectly continuous.

A roughly **14.5-hour outage occurred on 2026-07-01 from 02:34–17:06 UTC** after a stale PID file caused the service to crash-loop **4,825 times**.

Earlier data also contained laptop-sleep contamination and had already been heavily explored during development.

For both data-quality and research-integrity reasons, the later forward window therefore begins on **2026-06-01** rather than reusing the over-analyzed pre-June sample.

---

## 3. Real execution-cost measurement

*Engineering measurement.*

One of the largest corrections in the project came from replacing assumed costs with measured execution costs.

The original round-trip assumption was:

**0.08%**

Real execution data from **7,109 taker fills** instead measured approximately:

- commission: **0.099%**
- execution vs midpoint: **0.325%**
- total measured cost: **0.424%**

The original assumption was therefore far below realized trading friction.

Execution cost also increased with volatility.

Once measured costs replaced the original assumption, several apparently acceptable results became clearly negative.

This turned out to be one of the most important findings of the project.

See [evidence/cost_measurement.md](evidence/cost_measurement.md).

---

## 4. Fixed-horizon returns replaced win-rate intuition

*Retrospective interpretation, supported by forward measurement.*

Early evaluation relied heavily on whether trades hit stop-loss or take-profit levels first.

Later work measured forward returns at fixed horizons instead.

That exposed an important distinction:

> Win rate is not the same thing as directional predictive information.

When a trade is classified by which barrier is touched first, its win rate depends partly on stop/target geometry.

A strategy can therefore win frequently while still having near-zero or negative expected return.

---

## 5. Robustness checks were able to kill attractive results

*Methodological.*

The research process gradually incorporated:

- preregistration
- forward windows
- measured cost floors
- effective sample size
- clustered confidence intervals
- leave-one-day-out fragility checks
- BTC-relative returns
- multiple-testing awareness
- placebo and survivorship investigations

Several results that initially looked attractive did not survive these checks.

That is a successful research outcome even though the trading hypotheses failed.

---

# Failed Hypothesis 1
## Trapped Longs Should Produce a Tradeable Liquidation Overshoot

*Preregistered test. Registered 2026-06-09; verdict executed 2026-09-01.*

### Hypothesis

During a BTC downtrend, markets containing a high concentration of trapped leveraged longs should experience stronger downward continuation because forced selling creates additional price pressure.

The preregistered proxy for a trapped-long state was:

- `ls_ratio` >= cohort median
- `oi_delta_pct` > 0

The claim was not simply:

> Shorts should work during a BTC decline.

The stronger hypothesis was:

> Short-side expected value should be materially higher in the high trapped-long-density group than in the low-density group.

This was intended to distinguish a liquidation interaction from ordinary short-market beta.

### Result

The preregistered verdict was run on **2026-09-01**.

Effective sample size for TRAPPED:

**127**

The headline estimates cleared the numerical thresholds on their face.

However, the result failed the preregistered robustness requirement.

Removing any one of these five days individually was enough to make the effect unable to afford the measured cost floor:

- June 9
- June 14
- June 24
- June 25
- July 27

The distribution was also strongly asymmetric.

Median TRAPPED EV was much stronger than the mean, and:

> **median Δ was approximately 5× mean Δ**

So the concentration shape was visible.

But the economically relevant average effect was being dragged down by a left tail of large losses.

### Verdict

**KILL**

The interaction pattern existed in shape.

It was not robust enough to support a tradeable-edge claim.

Note the precise scope of this verdict: the failure was **not** the "buckets are flat, therefore it is pure beta" branch of the preregistration. The interaction cleared its own confidence floor. What failed was **tradeability at measured cost under a fragility check**.

See [evidence/trapped_long_verdict.md](evidence/trapped_long_verdict.md).

---

# Failed Hypothesis 2
## BTC Downtrend Filtering Should Improve Short Alerts

*Preregistered test. Forward windows and a KILL condition were defined in advance.*

A preregistered BTC-regime rule tested whether short alerts became more reliable when BTC was already in a downtrend.

### Development result

Win rate:

**70.0%**

n = **70**

### Forward result

Out-of-sample win rate:

**50.0%**

Across the four preregistered windows:

**0 of 4 passed**

Pooled sample:

n = **284**

Expected value:

- **-0.049%** using the preregistered cost assumption
- **-0.243%** using measured cost

### Verdict

**Rejected**

The development result did not survive forward testing.

See [evidence/regime_test.md](evidence/regime_test.md).

---

# A Process Failure
## The Failed Preregistration Was Not Adjudicated Promptly

The BTC-regime test also exposed a governance failure.

Its 30-day KILL condition had already been met by **2026-06-30**.

The project did not formally adjudicate it until **2026-08-07**.

From the start of the forward period to adjudication, approximately 68 days had passed.

The important lesson is not the exact calendar count.

It is this:

> A preregistration does not protect against researcher discretion if nobody is forced to call the result when its decision rule is reached.

A failed preregistration that sits uncalled can slowly turn back into an exploratory hypothesis.

Future experiments therefore need explicit adjudication dates, not merely written rules.

---

# Failed Hypothesis 3
## Better Exit Design Should Rescue the Signal

*Retrospective analysis on recorded excursion data.*

One hypothesis was that the entries contained useful information but poor exits were destroying profitability.

This suggested tighter stops or different exit rules could be the highest-leverage improvement.

### Result

The original exit analysis used a cost assumption approximately:

**5.3× too low**

When measured costs were substituted, every pessimistic exit cell became negative.

### Verdict

**Refuted**

The exit logic was not hiding a profitable underlying signal.

The expected return was too small relative to trading friction.

---

# Failed Hypothesis 4
## Moving the Take-Profit Target Should Rescue the Strategy

*Retrospective analysis plus closed-form arithmetic.*

Another natural idea was that the take-profit level was simply badly chosen.

Measured win rate was approximately:

**44.1%**

For a TP1 target at **1.5R**:

- required win rate under the old fee assumption: **42.7%**
- required win rate under measured cost: **54.2%**

The observed win rate was therefore approximately:

**10.1 percentage points below the realistic breakeven requirement**

More generally, the breakeven hit-rate requirement relative to the corresponding random-walk hit rate remained approximately:

**1.355×**

across target multiples.

The deficit was therefore scale invariant.

### Verdict

**Rejected by arithmetic**

Moving the target could rearrange the distribution of wins and losses.

It could not create the missing expected value.

---

# Failed Hypothesis 5
## Higher Signal Tier Should Mean Higher Independent Signal Quality

*Exploratory analysis.*

Higher tiers were intended to represent stronger combinations of evidence.

The assumption was:

> Higher-tier alerts should contain more predictive information.

The S+A tier filter turned out to overlap heavily with the BTC regime rule.

Tier was acting largely as a regime proxy rather than an independent quality dimension.

### Verdict

**Rejected as a distinct source of edge**

---

# Low-Cost Rule Screens (Hypotheses 6–10)

*Forward tests, evaluated prospectively via shadow scoring.*

Several additional rules were tested prospectively.

These should not be interpreted as strong statistical proofs that the underlying ideas are universally false.

The decision standard here was intentionally asymmetric:

> Accepting a new trading rule requires strong evidence.

> Declining to adopt a rule is cheap.

A weak or noisy result is enough to avoid adding complexity.

Therefore these tests were used primarily as **promotion screens**, not as definitive null-hypothesis proofs.

See [evidence/rule_screens.md](evidence/rule_screens.md).

---

## Failed Hypothesis 6 — Engine Agreement

Observed lift:

**-6.6 percentage points**

n = **122**

### Decision

**Not promoted**

The result provided no reason to add the rule.

---

## Failed Hypothesis 7 — CVD Divergence

Observed lift:

**-8.3 percentage points**

n = **41**

### Decision

**Not promoted**

The sample was small.

This is not evidence that CVD divergence can never matter.

It was insufficient evidence to justify incorporating this version of the rule.

---

## Failed Hypothesis 8 — Breakout Z-Score

Observed lift:

**-11.7 percentage points**

n = **58**

### Decision

**Not promoted**

---

## Failed Hypothesis 9 — Recommended-Take Rule

Observed lift:

**-31.8 percentage points**

n = **24**

### Decision

**Not promoted**

The estimate is highly uncertain at this sample size.

The negative point estimate was sufficient to avoid adding the rule, but should not be treated as a precise estimate of the true effect.

---

## Failed Hypothesis 10 — B-Long Edge Rule

Observed lift:

**-31.7 percentage points**

n = **23**

However:

**21 of the 23 fires came from quarantined symbols.**

This makes the result materially confounded.

### Decision

**Not promoted**

This is not a clean rejection of the underlying hypothesis.

The evidence is too contaminated to support such a claim.

---

# Failed Hypothesis 11
## Signal Persistence Should Identify Stronger Opportunities

*Forward test.*

Persistence was tested as a possible quality signal.

Forward performance:

- fire group: **26.7%**
- skip group: **40.8%**

Difference:

**-14.1 percentage points**

The forward failure did not appear to be ordinary rule decay.

Instead, it tracked a broader market-wide regime collapse.

Persistence therefore became more useful as a **thermometer of market state** than as an independent predictor.

### Verdict

**Rejected as directional edge**

But retained conceptually as a regime diagnostic.

---

# Failed Hypothesis 12
## Long Alerts Should Be Tradeable

*Forward test.*

Observed long-side performance:

- win rate: approximately **35%**
- n = **117**
- net expected value: approximately **-0.175%**

### Verdict

**Rejected**

Long alerts were subsequently blocked from notification. Database recording continued, so the cohort remained measurable.

---

# The Headline Result
## The Alerts Did Not Demonstrate Detectable Directional Information

*Forward test (not preregistered).*

The most important analysis removed stop-loss and take-profit mechanics entirely.

Every alert was instead held mechanically to fixed horizons.

BTC-relative forward returns were measured across:

**88 days**

and:

**10,683 labeled alerts**

| Horizon | n | BTC-relative mean | 95% cluster-robust CI |
|---|---:|---:|---:|
| 1h | 2,833 | +0.014% | [-0.014%, +0.042%] |
| 4h | 2,791 | -0.004% | [-0.072%, +0.064%] |
| 12h | 2,718 | -0.023% | [-0.143%, +0.097%] |
| 24h | 2,781 | +0.052% | [-0.195%, +0.298%] |

The 4-hour result is especially informative.

Mean:

**-0.004%**

95% CI:

**[-0.072%, +0.064%]**

The uncertainty band itself is the result.

Its roughly ±0.07% width is approximately:

- **3× tighter** than the original ~0.23% economic hurdle
- **6× tighter** than the measured 0.424% trading cost

The upper confidence bound does not even reach one third of the cheapest relevant cost floor.

This was therefore not simply:

> We need more data.

The estimate became precise enough to rule out a directional effect of the size required for the tested system to be economically useful.

See [evidence/headline_results.md](evidence/headline_results.md).

---

# An Extremely Favorable Market Still Did Not Rescue the Signal

During the measurement sample:

BTC fell approximately:

**-19.2%**

The alert book was approximately:

**80% short**

This should have been an unusually favorable environment for a predominantly short signal system.

Yet the raw return remained approximately zero.

That makes the null result harder to explain as merely an unfavorable regime.

---

# Why Win Rate Was Misleading

*Retrospective interpretation.*

A trade classified by whether its take-profit or stop-loss is touched first does not measure pure directional information.

It measures the interaction between:

- price path
- stop location
- target location
- volatility
- holding period

For example, some S/A short categories were directionally correct approximately **55%** of the time at four hours and still produced approximately zero average return.

The distribution often looked like:

- many small favorable moves
- occasional large adverse moves

A respectable win rate therefore did not imply positive expected value.

---

# Transaction Costs Were Not a Detail

Measured all-in taker cost was approximately:

**0.424%**

That was much larger than many of the apparent signal effects.

If gross expected return is approximately zero:

> Net P&L ≈ -(turnover × trading cost)

At that point, changing thresholds, stacking more indicators, or optimizing exits cannot manufacture predictive information.

The mechanically reliable improvements are:

- trade less
- reduce execution cost

Neither creates alpha.

They reduce bleed.

---

# Maker vs Taker Execution

*Engineering measurement.*

One execution inefficiency was clearly visible.

Take-profit orders are naturally compatible with passive limit execution.

However:

**99.44% of recorded fills were taker fills**

That discarded a potential structural execution advantage.

A maker-style TP could theoretically reduce explicit trading cost.

But this benefit should not be overstated.

Resting limit orders:

- do not always fill
- can be adversely selected
- can miss favorable moves
- introduce selection effects not priced in the simple maker-cost comparison

Even under favorable execution assumptions, the remaining deficit was still too large to establish profitability.

Execution optimization could reduce losses.

It did not solve the missing gross edge.

---

# Statistical Dependence Was Much Larger Than Expected

*Engineering / statistical measurement.*

Earlier analyses assumed a day-clustered design effect around **1.9–2.5**.

The measured design effect was:

**11.1**

That is roughly a **4–6× miss in the assumed design effect**.

Because standard errors scale with the square root of the design effect:

- intervals computed assuming independence were roughly **3.3× too narrow**
- intervals using the earlier assumed design effect of 1.9–2.5 were still roughly **2.1–2.4× too narrow**

The effective sample size of pooled DUAL-short observations was therefore approximately:

**330**

rather than the raw:

**3,692**

A runs test also strongly rejected independent outcomes:

**Z = -10.68**

The lesson was:

> Thousands of alerts are not thousands of independent experiments.

Signals cluster inside common market regimes.

Ignoring this can dramatically overstate confidence.

---

# The Verdict Tool Was Initially Biased Toward PASS

A particularly important implementation failure occurred in the final adjudication tooling.

The preregistered criterion required an:

**effective-n confidence-interval floor**

However, the verdict script initially printed a:

**raw-n Wilson interval**

That interval was approximately **1.4× too narrow**.

The error therefore operated in exactly the dangerous direction:

> It made a PASS easier.

The discrepancy was identified and corrected before the final verdict.

This was an important lesson:

> A preregistration is only as trustworthy as the code that adjudicates it.

The verdict implementation itself must be independently checked.

---

# A Statistically Significant Result That Was Not Accepted

*Exploratory analysis.*

One exploratory analysis found stronger performance during:

**13:00–21:00 UTC**

Estimated effect:

**+0.130%**

t-statistic:

**2.17**

This was not promoted into a trading rule.

Reasons:

1. the effect was already smaller than the realistic cost floor;
2. it arose after extensive exploratory analysis;
3. it was approximately test #81.

At roughly 81 opportunities for discovery, family-wise false-positive risk is extremely high.

Therefore:

> Statistical significance was not treated as evidence of tradeability.

---

# Top-Trader Positioning

*Disclosed secondary lens; not a preregistered confirmatory test.*

The dataset also contained Binance top-trader long/short positioning.

Win rate appeared monotonic with increasing large-account long exposure.

However:

**EV after slippage was not monotonic.**

The descriptive relationship therefore did not support a clean economic hypothesis.

### Result

Interesting pattern.

**No demonstrated tradeable edge.**

---

# Estimated Liquidations Were Not Ground Truth

*Data-quality finding.*

The system contained liquidation-related fields.

However, these values were reconstructed from candle information rather than taken from the real forced-order tape.

They should therefore be interpreted as:

**estimated liquidation-related activity**

not:

**observed forced liquidations**

This distinction matters when evaluating liquidation-driven mechanisms.

---

# Expired Trades Produced a Survivorship Illusion

*Data-quality finding, confirmed by simulation.*

Some unresolved alerts were classified as `expired`.

Their outcomes initially appeared favorable.

Further investigation showed that the effect arose from conditioning on survival through asymmetric barriers.

TP1 was approximately:

**1.5R**

while the stop was:

**1.0R**

A zero-drift Monte Carlo reproduced almost the same apparent positive endpoint behavior:

- simulated: **61.2%**
- observed: **61.8%**

No predictive market effect was required.

Expired observations were therefore excluded from ordinary resolved win/loss analysis.

---

# What the Research Process Got Wrong

The project became more rigorous over time, but it did not begin that way.

The full list is in [process_failures.md](process_failures.md). In summary:

- **Underestimating transaction costs** — 0.08% assumed versus 0.424% measured.
- **Underestimating dependence** — design effect assumed around 1.9–2.5 versus 11.1 measured.
- **Over-relying on win rate** — barrier geometry created misleadingly attractive classifications.
- **Allowing a failed preregistration to sit uncalled** — the decision rule existed, but governance did not force timely adjudication.
- **Building a verdict script that initially favored PASS** — raw-n uncertainty was used where effective-n uncertainty was required.
- **Cost corrections not propagating cleanly** — a dependent test stayed red for 42 days.
- **Treating collection as cleaner than it really was** — the final dataset contained a documented outage, and older data suffered from laptop-sleep contamination.
- **Testing many attractive ideas** — which made isolated nominally significant findings much less convincing.

These are not side notes.

They are part of the result.

---

# What the Project Ultimately Established

The system was useful as:

- data infrastructure
- alert logging infrastructure
- forward-measurement infrastructure
- execution-cost measurement infrastructure
- research tooling
- falsification machinery
- a source of later research questions

It was unsuccessful at demonstrating robust tradeable directional edge from the tested alert framework.

The progression was therefore not:

> bad strategy -> better strategy -> profitable strategy

It was:

> signal idea
> -> scanner
> -> live measurement
> -> apparently promising patterns
> -> forward testing
> -> realistic execution costs
> -> dependence correction
> -> robustness checks
> -> failed hypotheses
> -> better research methodology

The negative result is the result.

Publishing it prevents the same plausible ideas from being repeatedly rediscovered, overfit, and mistaken for evidence of edge.

It also documents something that successful-backtest repositories often omit:

> A system can work technically, generate thousands of observations, produce attractive intermediate statistics, and still fail to demonstrate economically meaningful predictive information.
