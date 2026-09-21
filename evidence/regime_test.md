# Evidence — BTC Regime Rule (Hypothesis 2)

A preregistered rule with forward windows and a defined KILL condition. It failed, and the failure was well-documented but adjudicated late.

---

## Claim 1 — The preregistered criterion

```
Claim:    The rule's success criterion was Wilson-low net EV clearing fee
          across 3 non-overlapping forward windows of n >= 60 each,
          including at least one non-downtrend BTC window.
Tier:     Preregistered test
Window:   registered 2026-05-31
Source:   the rule's pre-registration, recorded in project documentation
Status:   RECORDED
```

**Notes.**
- The rule fired DUAL shorts only when BTC's trailing 12-hour return was below −1%.
- A KILL condition was defined in advance: failure of the first out-of-sample window, no cohort clearing fee, or worsening autocorrelation, assessed at the 30-day mark.
- The requirement of at least one non-downtrend window was included specifically because the in-sample result came entirely from a single declining-BTC episode.

---

## Claim 2 — In-sample result

```
Claim:    In development, gate-pass DUAL shorts showed 70.0% win rate.
Tier:     Exploratory (development-sample)
Value:    70.0% win rate, net EV +0.487%
Sample:   n = 70
Window:   development sample, within a single BTC decline (~80k -> ~73k)
Source:   development analysis
Status:   RECORDED
```

**Notes.**
- This figure was explicitly flagged as unproven when recorded: n = 70, all within one declining-BTC window, jackknife-thin, with no bull-market data.
- "BTC down implies shorts win" is close to tautological within a downtrend. The real test was always whether the rule caught *future* regime transitions.

---

## Claim 3 — Forward result: 0 of 4 windows

```
Claim:    The rule failed its own preregistered criterion in every forward
          window.
Tier:     Preregistered test (forward)
Value:    W1 +0.607% [-0.085, +1.299]
          W2 +0.282% [-0.261, +0.825]
          W3 -0.888% [-1.746, -0.030]  (significantly negative)
          W4 +0.134% [-0.489, +0.757]
          0 of 4 windows cleared
Sample:   pooled gate-pass n = 284
Window:   forward period from 2026-05-31
Source:   rule scoreboard and forward-window analysis
Status:   RECORDED
```

**Notes.**
- Pooled net EV: **−0.049%** at the preregistered cost basis; **−0.243%** at measured cost.
- The in-sample 70.0% win rate went to **50.0%** out of sample — a clean regression-to-coin-flip.
- W3 is significantly *negative*, not merely non-significant.
- The criterion required 3 passing windows. It obtained 0.

---

## Claim 4 — An independent regime definition initially agreed

```
Claim:    A second, independent regime definition (daily close vs 20-day SMA)
          reproduced the downtrend-favors-shorts pattern in-sample.
Tier:     Exploratory kill-test
Value:    downtrend n = 524, WR 57.3%, net EV +0.268%
          uptrend  n = 215, WR 39.1%, net EV -0.392%
Window:   2026-05-12 .. 2026-06-02
Source:   tools/btc_regime_killtest.py
Status:   RECORDED
```

**Notes.**
- This was run as a cheap attempt to *kill* the hypothesis using a definition with one parameter and no curve-fitting. It did not kill it, and confidence was nudged up at the time.
- **It should not be read as confirmation.** The window covers ~3 weeks and one macro episode. Within it there were 19 downtrend days against only 3 uptrend days, so the 215 "uptrend" alerts cluster into 3 days and their effective n is far smaller than it appears.
- The forward test (Claim 3) subsequently overrode this. An independent definition agreeing on the same episode is weaker evidence than it feels, because both definitions are reading the same one episode.

---

## Claim 5 — The structural weakness: a trailing filter lags the turn

```
Claim:    Because the gate reads trailing BTC return, it rejects shorts at
          the start of a downtrend - the leg that subsequently wins.
Tier:     Retrospective interpretation, with a live observation
Value:    observed early shadow sample: skip (would-reject) n = 10 at 90% WR
Window:   2026-05-31 observation
Source:   shadow rule scoreboard
Status:   RECORDED
Notes:    n = 10 is far too small to support a conclusion. It is recorded
          because it illustrates a structural property predicted in advance
          by the rule's own specification, not because the number is solid.
```

**Notes.**
- The gate was good at staying *out* of sustained hostile regimes and weak at *catching* transitions into favorable ones. This is an inherent property of trailing filters, and was documented as "reaction not prediction" before it was observed.

---

## Claim 6 — Adjudication was late

```
Claim:    The KILL condition was met by 2026-06-30 but not formally
          adjudicated until 2026-08-07.
Tier:     Process record
Value:    ~68 days from the start of the forward period to adjudication
Source:   project records; verdict document written 2026-08-07
Status:   RECORDED
```

**Notes.**
- On adjudication the rule was retired from the active registry, the function retained as callable, and 8,362 accumulated shadow rows kept for future reference.
- Post-retirement, the scoreboard reads: `live = no, n_fire = 284, netEV −0.2425, REJECT`.
- See [../process_failures.md](../process_failures.md) item 4.
