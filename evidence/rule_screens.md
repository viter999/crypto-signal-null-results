# Evidence — Rule Screens (Hypotheses 6–12)

Prospectively-scored candidate rules, evaluated as **promotion screens** rather than as definitive tests.

---

## How these were produced

```
Tier:     Forward test
Source:   analysis/rule_scoreboard.py - a standing report joining shadow
          predictions to recorded alert outcomes, per rule
Method:   Each rule was implemented as a "shadow" that recorded what it
          WOULD have fired, without affecting live behaviour. The scoreboard
          reports n_fire, win rate of the fired cohort vs the skipped
          cohort, lift, Wilson lower bound, fired-cohort net EV, and a
          status label.
Status:   RECORDED (scoreboard outputs from contemporaneous project records)
```

**The decision standard was asymmetric and is stated in the scoreboard's own output:** a screen pass was explicitly marked *necessary but not sufficient*, with a footnote citing the runs-test result (Z = −10.68) as evidence that the lift and Wilson calculations assume an independence that does not hold.

That caveat is why these are reported as "not promoted" rather than "disproven."

---

## Claim 1 — Screen results

```
Claim:    None of the five screened rules produced a positive forward lift
          sufficient to justify adoption.
Tier:     Forward test
Status:   RECORDED
```

| # | Rule | Lift | n | Decision |
|---|---|---:|---:|---|
| 6 | Engine agreement | −6.6 pp | 122 | Not promoted |
| 7 | CVD divergence | −8.3 pp | 41 | Not promoted |
| 8 | Breakout z-score | −11.7 pp | 58 | Not promoted |
| 9 | Recommended-take | −31.8 pp | 24 | Not promoted |
| 10 | B-long edge | −31.7 pp | 23 | Not promoted (confounded) |

**Notes on individual entries.**

- **#7 (CVD divergence)** has a complicated history worth recording honestly. At an earlier reading it was the single live candidate that looked worth growing — fire win rate 61.9%, lift +14.1 pp, net EV +0.50% — but was held at WATCH because n was only 21. The later reading at n = 41 is negative. A candidate that reversed sign as n doubled is a useful illustration of how little a 21-sample lift means.
- **#10 (B-long edge)** is materially confounded: **21 of the 23 fires came from quarantined symbols**, which were independently identified as anti-predictive. This is not a clean test of the underlying idea.
- **#9** at n = 24 carries an interval wide enough that the point estimate should not be read as an effect size.

---

## Claim 2 — Persistence (Hypothesis 11)

```
Claim:    Signal persistence showed negative forward lift, but the failure
          tracked a market-wide regime collapse rather than rule decay.
Tier:     Forward test
Value:    fire cohort 26.7% vs skip cohort 40.8%; lift -14.1 pp;
          fired-cohort net EV -0.51%
Source:   analysis/rule_scoreboard.py
Status:   RECORDED
```

**Notes.**
- A backfill over historical data had indicated roughly 56%, against 26.7% forward. That gap is itself the finding.
- The forward failure was traced to a market-wide collapse in the short book, not to a defect in the rule: across the same period, quartile-level win rates were 44.7 / 49.3 / 52.7 / 48.7, while the trailing 48 hours read 15.0% (n = 60) and the trailing 24 hours 0% (n = 8).
- The decision taken at the time was **not** to patch the rule, on the grounds that it was functioning as a thermometer of regime rather than as a failing predictor. Reacting to 48 hours of data would have been overfitting.
- Retained conceptually as a regime diagnostic; rejected as directional edge.

---

## Claim 3 — Long alerts (Hypothesis 12)

```
Claim:    Long-side alerts were a confirmed net-negative cohort.
Tier:     Forward test
Value:    ~35% win rate; net EV ~-0.175%; fails its own breakeven
Sample:   n = 117
Source:   cohort analysis; subsequently gated in the notification path
Status:   RECORDED
```

**Notes.**
- Notification for long alerts was blocked following this result. **Database recording continued**, so the cohort remained measurable and the block did not create a blind spot.
- A corresponding shadow rule's skip cohort read 5.0% win rate, which independently supported the block.
- This is the clearest-cut rejection in the set: the largest n among the screens, and an effect size well outside plausible noise.

---

## Claim 4 — Retired rules validated the screening process

```
Claim:    Every previously-retired rule read REJECT on the standing
          scoreboard.
Tier:     Forward test
Value:    4 of 4 retired rules confirmed REJECT
Source:   analysis/rule_scoreboard.py first-run output
Status:   RECORDED
```

**Notes.**
- This is weak evidence — the rules were retired *because* they looked bad, so confirming they look bad is close to circular.
- It is recorded only as a sanity check that the scoreboard machinery reproduced prior decisions rather than contradicting them.

---

## What should not be concluded from this page

These screens do **not** establish that engine agreement, CVD divergence, breakout statistics, or persistence carry no information in crypto markets.

They establish that **these specific constructions, on this venue, over this window, at these sample sizes, gave no reason to add complexity to the system.**

At n = 23–58 with a measured design effect above 10, the honest statement about most of these rules is "insufficient evidence to adopt," not "shown to be false."
