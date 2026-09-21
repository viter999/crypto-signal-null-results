# Evidence — Headline Result

The decisive measurement: do the alerts carry directional information once barrier mechanics are removed?

---

## Claim 1 — Fixed-horizon BTC-relative returns

```
Claim:    Alerts carry no detectable BTC-relative directional return at any
          tested horizon.
Tier:     Forward test (NOT preregistered)
Value:    see table below
Sample:   10,683 labeled alerts total; per-horizon n in table
Window:   2026-05-12 .. 2026-08-07 (88 days)
Source:   Horizon measurement run 2026-08-07 against the live project
          database. Analysis scripts were session-scratch (`clean.py` for
          the measurement, `validate.py` for sign/coverage checks); they
          are NOT part of the committed project tree.
Status:   RECORDED
```

| Horizon | n | BTC-relative mean | 95% cluster-robust CI |
|---|---:|---:|---:|
| 1h | 2,833 | +0.014% | [−0.014%, +0.042%] |
| 4h | 2,791 | −0.004% | [−0.072%, +0.064%] |
| 12h | 2,718 | −0.023% | [−0.143%, +0.097%] |
| 24h | 2,781 | +0.052% | [−0.195%, +0.298%] |

**Notes.**
- Measurement holds each alert from fire time to a fixed horizon with **no stop, no target and no path rule**, then strips BTC beta. This is deliberately not a tradeable construction; it is a test of directional information.
- The 4h horizon is closest to the observed median holding period.
- Population measured is the set the notification tier floor actually sent.
- The interval, not the point estimate, is the result. A mean of −0.004% with a ±0.07% band is a *precise* zero, not an underpowered one.
- **This analysis was not preregistered.** It was run after the project's direction of travel was already understood. Its credibility rests on the width of the interval and on the fact that it uses no tunable barrier parameters — not on prior commitment.

---

## Claim 2 — The interval is tight relative to the cost floor

```
Claim:    The 4h confidence interval is 3-6x tighter than the relevant cost
          floors, so the upper bound excludes an economically useful effect.
Tier:     Retrospective interpretation (arithmetic on Claim 1)
Value:    ±0.07% band vs 0.23% preregistered and 0.424% measured cost
Sample:   as Claim 1
Window:   as Claim 1
Source:   arithmetic on Claim 1 and evidence/cost_measurement.md
Status:   VERIFIED (arithmetic re-checked during preparation)
```

**Notes.**
- 0.23 / 0.07 ≈ 3.3; 0.424 / 0.07 ≈ 6.1.
- Upper bound +0.064% is below one third of 0.23% (= 0.0767%), so the interval excludes even a third of the cheapest relevant floor.
- This is the step that converts "no evidence of an effect" into "evidence of no economically relevant effect."

---

## Claim 3 — A favorable regime did not rescue the result

```
Claim:    BTC fell ~19.2% over the sample and the alert book was ~80% short,
          yet raw directional return remained approximately zero.
Tier:     Forward test / descriptive
Value:    BTC −19.2%; book composition 6,099 short vs 1,527 long (~80% short);
          raw return ≈ −0.002%
Sample:   7,626 directional alerts
Window:   2026-05-12 .. 2026-08-07
Source:   same analysis run as Claim 1
Status:   RECORDED
```

**Notes.**
- This is the strongest available argument against a "wrong regime" explanation. A predominantly short book in a −19.2% market is close to a best case.
- It independently reproduces an earlier finding that the system's shorts performed no better than simply shorting BTC over the same windows.

---

## Claim 4 — Win rate and directional correctness diverge

```
Claim:    Some S/A short categories were directionally correct ~55% of the
          time at 4h while still producing approximately zero mean return.
Tier:     Retrospective interpretation
Value:    ~55% directionally correct at 4h; mean ≈ 0
Sample:   subset of Claim 1 population
Window:   as Claim 1
Source:   same analysis run as Claim 1
Status:   RECORDED
```

**Notes.**
- The distribution is many small favorable moves against occasional large adverse ones — the skew of being short a positively-skewed asset.
- This is the mechanism that made win-rate-based evaluation misleading for the first several months of the project.

---

## Claim 5 — No subgroup escaped

```
Claim:    Every alert-kind x direction cell straddles zero at the 4h horizon.
Tier:     Forward test
Value:    all subgroup intervals include zero
Sample:   subgroups of Claim 1 population
Window:   as Claim 1
Source:   same analysis run as Claim 1
Status:   UNVERIFIED
Notes:    Subgroup counts and per-cell intervals were not preserved in the
          project record and would need regeneration. This claim is
          therefore the weakest-provenance item on this page and should be
          re-derived before being relied on.
```
