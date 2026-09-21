# Process Failures

The project became more rigorous over time. It did not begin that way.

These are recorded because each one materially changed a conclusion, and because a research record that reports only its hypotheses — and not the ways its own machinery misled it — is incomplete.

---

## 1. Transaction costs were assumed rather than measured

The original round-trip cost assumption was **0.08%**.

Measured from 7,109 real taker fills, actual all-in cost was approximately **0.424%** — commission 0.099% plus execution-versus-midpoint 0.325%. The assumption was roughly **5.3× too low**, and below exchange taker commission alone.

**What it changed:** the exit-optimization thesis (Hypothesis 3) was built entirely on the wrong number. At measured cost, every pessimistic cell in the exit grid turned negative. Several other results moved from "marginally acceptable" to "clearly negative."

**Lesson:** an assumed cost is a modeling choice, not a constant. It deserves the same scrutiny as the signal.

---

## 2. Statistical dependence was badly underestimated

Earlier analyses assumed a day-clustered design effect around **1.9–2.5**. The measured value was **11.1** — a 4–6× miss.

**What it changed:** intervals computed assuming independence were roughly 3.3× too narrow; even those using the earlier assumption were still 2.1–2.4× too narrow. Effective sample size on the pooled DUAL-short cohort was approximately 330, not 3,692. A runs test gave Z = −10.68.

**Lesson:** thousands of alerts are not thousands of independent experiments. In a market where signals fire in regime clusters, raw counts flatter the analyst.

---

## 3. Win rate was treated as evidence of predictive skill

Early evaluation leaned on whether price reached the target or the stop first.

**What it changed:** barrier geometry produced attractive-looking classifications that did not correspond to directional information. Some categories were directionally correct ~55% of the time at four hours and still produced approximately zero mean return.

**Lesson:** a metric that depends on where you placed your exits cannot tell you whether your entry was right.

---

## 4. A failed preregistration was allowed to sit uncalled

The BTC-regime rule's 30-day KILL condition was met by **2026-06-30**. It was not formally adjudicated until **2026-08-07** — roughly 68 days after the forward period began.

**What it changed:** nothing in the final verdict, because the rule failed either way. But the delay is the problem regardless of outcome.

**Lesson:** a preregistration does not constrain researcher discretion if nobody is obliged to call the result when its decision rule is reached. An uncalled failed preregistration drifts back into being an exploratory hypothesis. Future experiments need explicit adjudication dates, not merely written criteria.

---

## 5. The verdict script was biased toward PASS

The preregistered criterion required a confidence floor computed on **effective n**. The adjudication script initially computed and printed a **raw-n** Wilson interval — approximately **1.4× too narrow**.

**What it changed:** the error ran in precisely the dangerous direction, making a PASS easier to obtain. It was identified and corrected before the final verdict was run. The corrected interval widened the relevant figure substantially; the conclusion did not change, because the result was already leaning against the hypothesis.

**Lesson:** a preregistration is only as trustworthy as the code that adjudicates it. The verdict implementation deserves independent review, and ideally a check that it can actually fail.

---

## 6. Cost corrections did not propagate cleanly through the codebase

After the fee correction, `tests/test_exit_replay.py` remained red for **42 days** because it still contained a hardcoded expectation based on the old fee assumption.

The failure delta matched the fee change itself.

This was not a statistical problem, but it exposed a software-governance weakness:

> A corrected research assumption is not fully corrected until every dependent test, script, and report has been updated and independently verified.

The fix was to derive the expectation from the module constant — so the test checks its actual invariant — and to pin the constant's *value* in a separate test, so a change of cost basis can never drift in silently again.

---

## 7. Data collection was treated as cleaner than it was

The dataset contains a documented **14.5-hour outage** on 2026-07-01 (02:34–17:06 UTC), caused by a stale PID file that sent the service into a crash-loop repeated **4,825 times**. Earlier data additionally suffered laptop-sleep contamination.

**What it changed:** the clean forward window was moved to begin **2026-06-01**, for both data-quality and research-integrity reasons — the pre-June sample had also been heavily explored during development.

**Lesson:** "it ran continuously" is a claim about the apparatus and needs the same verification as a claim about markets. A silent failure that reports success is worse than one that crashes loudly.

---

## 8. Many attractive ideas were tested

Roughly 81 tests were run against this dataset over the project's life.

**What it changed:** it made isolated nominally significant findings — such as the 13:00–21:00 UTC result at t = 2.17 — far less convincing than their p-values suggest. At that test count, family-wise false-positive risk is extreme.

**Lesson:** the number of hypotheses tried is itself a statistic, and it should be reported. A finding's credibility depends on how many opportunities there were to find something.

---

## The common thread

Five of these eight failures — 1, 2, 5, 6 and 7 — share a structure: **a component of the measuring apparatus was wrong in the direction that made results look better.**

None of them were detected by looking harder at the market data. Each was found by turning the same skepticism on the instrument that was being applied to the hypothesis.

That is the transferable lesson of this project. The signal research produced a null result. The methodology research produced the actual findings.
