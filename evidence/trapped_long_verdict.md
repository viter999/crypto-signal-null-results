# Evidence — Trapped-Long Conditional Cut (Hypothesis 1)

The project's primary preregistered test, and the only one that ran to a formal verdict on a date fixed in advance.

---

## Claim 1 — The pre-registration

```
Claim:    H1, the trapped-long proxy, the bucket definitions, the pass and
          kill criteria, the data window and the decision date were all
          locked in writing before any inference was run.
Tier:     Preregistered test
Window:   registered 2026-06-09
Source:   docs/CONDITIONAL_CUT_PREREGISTER.md
Status:   VERIFIED (document opened and read during preparation)
```

**The locked content, verbatim in substance:**

- **H1** — Among DUAL-short alerts fired in a BTC downtrend, net EV (after 0.08% fee + 0.15% slippage) is materially higher in the HIGH trapped-long-density bucket than the LOW bucket, and the HIGH bucket's effective-n CI floor is > 0. *The edge is the interaction (density × short), not the marginal short.*
- **H0** — EV is flat across density buckets, so there is no liquidation-driven edge and DUAL-short is directional beta. Recorded in advance as **the expected base case**.
- **Trapped-long proxy** — `ls_ratio` ≥ cohort median **AND** `oi_delta_pct` > 0. Taken from context stored at fire time.
- **Locked buckets** — `ls_ratio` terciles; `oi_delta_pct` sign; combined TRAPPED vs NOT. Explicitly "no post-hoc redefinition."
- **Pass criterion** — TRAPPED bucket slippage-adjusted EV > 0 with effective-n CI floor > 0, AND materially higher than NOT.
- **Kill criterion** — any of: flat EV across buckets; TRAPPED EV ≤ 0 or its effective-n CI floor ≤ 0; or the interaction existing only on raw n rather than effective n.
- **Data window** — `ts_fired >= 2026-06-01` only, because pre-June data had laptop-sleep contamination *and* was the over-analyzed sample.
- **Decision date** — ~2026-09-01, fixed in advance.
- **Standing caveat, recorded in advance** — "If the TRAPPED-bucket sample is too thin by the decision date, extend the window rather than lower the bar — never relax the criteria to manufacture a pass."

---

## Claim 2 — The verdict

```
Claim:    All four decision clauses passed on their faces, but the result
          failed the fragility requirement and was therefore killed.
Tier:     Preregistered test
Value:    EVslip_TRAPPED +0.219pp against a floor of +0.007
          delta            +0.208pp against a floor of +0.038
          EVmeas           +0.025pp
Sample:   effective n (TRAPPED) = 127, above the 100 evaluability threshold
Window:   clean forward window opening 2026-06-01; verdict executed 2026-09-01
Source:   analysis/conditional_cut.py, run under the locked configuration
          with a recorded seed and 10,000 bootstrap resamples
Status:   RECORDED
```

**Notes.**
- The verdict ran against a fresh database pull whose counts were cross-checked against a server-side printout, with the prior pull retained.
- A pre-check step passed before the locked verdict was executed.

---

## Claim 3 — The fragility failure

```
Claim:    Five individual days each independently flip the cost-floor clause
          when removed.
Tier:     Preregistered test (robustness clause)
Value:    dropping any one of Jun 9, Jun 14, Jun 24, Jun 25, or Jul 27
          individually takes the result below the 0.194pp cost floor
Sample:   as Claim 2
Window:   as Claim 2
Source:   leave-one-day-out check within the verdict run
Status:   RECORDED
```

**Notes.**
- The fragility clause was added by the operator on **2026-08-25** — after the pre-registration, before the verdict, and without having seen the result. This timing matters and is disclosed deliberately: adding a clause mid-experiment is legitimate only if it *tightens* the criterion and is committed before the data is examined. Here it could only make a PASS harder.
- This clause is what converted a face-value pass into a KILL. A margin that five different single days can each individually erase is not a margin.

---

## Claim 4 — The precise scope of the KILL

```
Claim:    The failure was tradeability under measured cost, NOT the
          "flat buckets = beta" branch of the preregistration.
Tier:     Preregistered test (interpretation of the verdict)
Value:    the interaction cleared its own CI floor; effective n was above
          the evaluability threshold, so this is a true kill rather than a
          thin-data non-result
Status:   RECORDED
```

**Notes.**
- This distinction is the most easily over-read item in the whole project, in both directions.
- It is **not** the case that trapped-long density showed nothing. It is the case that what it showed could not afford the cost floor robustly.
- Under the declared rule the trapped-long alpha claim is **retired**. Re-reading other cuts or windows does not constitute a verdict.

---

## Claim 5 — The distribution shape

```
Claim:    The concentration shape is real and asymmetric: median effect
          substantially exceeds mean effect.
Tier:     Preregistered test (disclosed post-verdict observation)
Value:    TRAPPED median EVslip +0.589 vs mean +0.219
          median delta approximately 5x mean delta
Sample:   as Claim 2
Status:   RECORDED
```

**Notes.**
- The mean is dragged down by a left tail of large losses; the median is far stronger than the mean.
- Recorded because it is the honest disclosure: the *shape* the hypothesis predicted is visible in the data even though the tradeable claim failed.
- Any future hypothesis in this family should start from this observation — **under a fresh declaration**, not as a continuation of the killed one.

---

## Claim 6 — The secondary lens seeds nothing

```
Claim:    The disclosed top-trader secondary lens showed win rate monotone
          in large-account long exposure but EV-after-slippage non-monotone.
Tier:     Disclosed secondary lens (not confirmatory)
Source:   analysis/top_ls_cut.py
Status:   RECORDED
```

**Notes.**
- A monotone win rate with a non-monotone EV is the same trap described in the headline result: win rate is not the economic quantity.
- Recorded as descriptive. It does not support a follow-on hypothesis.
