# Evidence — Execution Cost

The single largest correction in the project. It is an engineering measurement, not a market hypothesis, but it determined the fate of several hypotheses.

---

## Claim 1 — Measured all-in round-trip cost

```
Claim:    All-in round-trip taker cost is ~0.424%, not the 0.08% originally
          assumed.
Tier:     Engineering measurement
Value:    commission 0.099% + execution-vs-midpoint 0.325% = 0.424% all-in
Sample:   7,109 real taker fills
Window:   not preserved in the project record - REQUIRES CONFIRMATION
Source:   fills priced against 1-minute OHLC
Status:   RECORDED
```

**Notes.**
- The original 0.08% assumption was below exchange taker commission alone, which is the clearest signal that it was never measured.
- The correction factor is ~5.3x.
- **The exact fill window is not recorded.** This should be established before the figure is relied on externally; it is the main provenance gap on this page.

---

## Claim 2 — Execution cost scales with volatility

```
Claim:    Execution cost increases with volatility at approximately +0.18%
          per 1% of the fill minute's range.
Tier:     Engineering measurement
Value:    coefficient +0.18% per 1% range, t = 29.5
Sample:   as Claim 1 (7,109 fills)
Window:   as Claim 1
Source:   regression of execution slippage on fill-minute range
Status:   RECORDED
```

**Notes.**
- t = 29.5 is very large; this is not a marginal effect.
- Two design consequences follow. Stops are inherently market orders that trigger during adverse fast moves — the most expensive minutes. And targets placed at estimated liquidation clusters aim the exit at high-volatility zones. Both pay above the average rate.
- A flat two-leg fee model, as used in the original exit replay, captures neither.

---

## Claim 3 — Fill composition is almost entirely taker

```
Claim:    99.44% of recorded fills were taker fills.
Tier:     Engineering measurement
Value:    99.44% taker
Sample:   as Claim 1
Window:   as Claim 1
Source:   fill records
Status:   RECORDED
```

**Notes.**
- A take-profit is naturally a limit order and should earn maker rebate with near-zero execution cost. In practice targets were read off the alert card and exited at market, discarding that advantage.
- This is the largest single fixable line item in execution.
- It does **not** make the system profitable: residual deficit after accounting for all mechanical cost remained approximately −21.5 bps.
- Resting limit orders also do not always fill, and that selection cost is unpriced in the simple maker-versus-taker comparison. The upside is therefore an upper bound.

---

## Claim 4 — Two cost bases were maintained deliberately

```
Claim:    The preregistered cost constant was not retroactively changed; the
          measured cost was reported alongside it.
Tier:     Methodological decision
Value:    preregistered 0.08% fee + 0.15% slippage = 0.23%;
          measured 0.424% reported as a parallel column
Window:   decision taken 2026-07-26
Source:   verdict script configuration
Status:   RECORDED
```

**Notes.**
- Changing a locked constant mid-experiment would have altered a preregistered criterion after seeing data. The measured figure was therefore added as a separate reported column rather than substituted.
- The direction of the correction is one-way safe: **raising assumed cost can only demote a cohort, never promote one.** Nothing previously killed could revive under the corrected cost, so the correction cannot have manufactured a negative result.
- Two display-only code paths that reported expected value to the operator were corrected separately on 2026-08-07, after being verified as non-gating. One cohort's reported figure moved from −0.0101 ("approximately breakeven") to −0.3541 ("consistent loser") on identical data — the difference was entirely the cost basis.

---

## Why this section matters beyond this project

Three hypotheses (3, 4, and the practical reading of 1) turned on this number. An assumed cost that is 5.3x too low does not merely shift results — it inverts conclusions about whether a system is marginally viable.

The general lesson: **assumed costs deserve the same scrutiny as the signal.** They are a modeling choice, they are usually chosen early and casually, and they are rarely revisited.
