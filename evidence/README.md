# Evidence and Provenance

Every substantive numerical claim in this repository is recorded here with its source, sample, window, evidence tier, and **verification status**.

## Why this layer exists

A research record that states figures without saying where they came from is asking to be trusted. A record that says where each figure came from — including the ones that have not been re-derived — can be checked.

This is especially important for a null-results repository. The claim "we found nothing" is only as strong as the claim "and here is exactly what we measured, on what sample, with what code."

## Verification status vocabulary

| Status | Meaning |
|---|---|
| **VERIFIED** | The source artifact was opened during preparation of this publication and the figure read directly from it |
| **RECORDED** | The figure comes from contemporaneous project records written at the time of the analysis. The analysis was really run; the artifact was **not** re-opened or re-executed for this publication |
| **UNVERIFIED** | Cited from records, but the artifact's location or existence has not been confirmed |

### Honest summary of current status

**Most numerical results in this repository are `RECORDED`, not `VERIFIED`.**

They were produced by analysis scripts run against the live project database at the time, and written down contemporaneously. They have not been re-executed against the database for this publication. The analysis code exists and is identified below; the database itself is not published (see *What is not published*).

This is stated plainly rather than papered over. A reader should treat `RECORDED` figures as credible-but-unaudited, and `VERIFIED` figures as directly checked.

Upgrading a figure from `RECORDED` to `VERIFIED` requires re-running the named script against the database and attaching the output. The outstanding items are listed in [Verification backlog](#verification-backlog).

## Record format

Each claim is recorded as:

```
Claim:    the assertion, stated numerically
Tier:     preregistered / forward / exploratory / engineering / retrospective
Value:    the figure, with interval where applicable
Sample:   n, and effective n where dependence matters
Window:   exact date range
Source:   script, document, or analysis that produced it
Status:   VERIFIED / RECORDED / UNVERIFIED
Notes:    caveats a reader needs in order to not over-read the claim
```

## Evidence files

| File | Covers |
|---|---|
| [headline_results.md](headline_results.md) | The fixed-horizon BTC-relative return measurement — the decisive result |
| [cost_measurement.md](cost_measurement.md) | Execution cost, maker/taker composition, and the cost basis used in each verdict |
| [regime_test.md](regime_test.md) | The BTC-regime preregistered rule and its forward windows |
| [trapped_long_verdict.md](trapped_long_verdict.md) | The preregistered conditional cut and the 2026-09-01 KILL verdict |
| [rule_screens.md](rule_screens.md) | Hypotheses 6–12, the low-cost promotion screens |

## Documents verified directly

These two source documents were opened and read during preparation of this publication, so claims traced to them are `VERIFIED`:

- **The conditional-cut pre-registration** — registered 2026-06-09, containing H1, H0, the locked trapped-long proxy, the locked bucket definitions, the pass and kill criteria, the data window rule (`ts_fired >= 2026-06-01`) and the decision date (~2026-09-01).
- **The system architecture document** — component responsibilities, data model, and the design decision separating measurement writes from notification gating.

## Analysis code confirmed to exist

The following analysis modules were confirmed present in the project tree during preparation. Their *existence* is `VERIFIED`; the *outputs* attributed to them below are `RECORDED`.

| Module | Role |
|---|---|
| `analysis/conditional_cut.py` | The preregistered verdict script (Hypothesis 1) |
| `analysis/top_ls_cut.py` | Disclosed secondary lens — top-trader positioning |
| `analysis/exit_replay.py` | Exit-policy replay against recorded MFE/MAE (Hypotheses 3, 4) |
| `analysis/rule_scoreboard.py` | Standing report joining shadow predictions to outcomes (Hypotheses 6–11) |
| `analysis/alpha_vs_beta.py` | Decomposition of alert returns against a BTC benchmark |
| `analysis/regime_breaker.py` | Regime circuit-breaker detector and offline calibration replay |
| `tools/btc_regime_killtest.py` | Independent regime-definition kill-test (Hypothesis 2) |

## Verification backlog

To upgrade this repository from `RECORDED` to `VERIFIED`, the following would need to be re-run and their outputs attached under `evidence/outputs/`:

1. **Headline fixed-horizon table** — re-run the horizon measurement over the stated window; attach the four-row output with cluster-robust intervals. *Highest value: this is the decisive claim.*
2. **Cost measurement** — re-derive commission, execution-versus-midpoint, and the volatility coefficient from the fill set; attach fill count and date range.
3. **Conditional-cut verdict** — re-run the locked verdict script with the recorded seed and bootstrap count; attach full output including both raw-n and effective-n intervals.
4. **Design effect and runs test** — re-compute on the pooled cohort; attach.
5. **Rule screen lifts** — regenerate the scoreboard; attach.
6. **Expired-outcome Monte Carlo** — re-run the zero-drift simulation; attach the comparison.

Each item should record the script, its arguments, the database snapshot date, and the output file.

## What is not published

This repository publishes derived evidence and research documentation only. Deliberately excluded:

- the underlying database (it contains identifiers associated with a private notification channel)
- server addresses, hostnames, credentials, environment files, and operational configuration
- notification channel identifiers
- local filesystem paths and usernames

None of these are necessary to evaluate the research claims, and publishing them would create attack surface without adding evidential value.
