# Figures

Currently empty. This directory is for charts that carry information the tables cannot.

## Worth making

Three figures would materially strengthen the record. Each is listed with the point it needs to make, because a figure that does not make a point should not be drawn.

**1. The headline interval against the cost floors.**
Four horizons on one axis, each showing the point estimate and its cluster-robust interval, with the 0.23% preregistered and 0.424% measured cost floors drawn as vertical lines. The entire argument of this repository is visible in one image: the intervals sit well inside the floors. This is the single highest-value figure.

**2. The trapped-long fragility check.**
The verdict statistic with each of the five identified days removed in turn, against the 0.194 pp cost floor. Shows at a glance that five separate single-day removals each cross the line — which is what "fragile" means, and is much harder to convey in prose.

**3. The expired-outcome survivorship artifact.**
Observed positive-outcome rate against the zero-drift Monte Carlo (61.8% vs 61.2%), with the asymmetric barrier geometry drawn alongside. Demonstrates that the apparent effect requires no market edge. This one can be generated entirely from simulation with no private data.

## Deliberately not worth making

- **Equity curves.** There is no strategy to curve. Drawing one would imply a tradeable construction that the research concluded does not exist.
- **Win-rate-over-time charts.** The whole finding is that win rate was the misleading metric; giving it a chart re-privileges it.
- **Anything with an unlabeled y-axis or an undeclared sample window.** Every figure must state n and its window, or it repeats the error the repository documents.

## Conventions

Any figure added here should carry: sample size, exact date window, evidence tier, and whether the underlying figure is `VERIFIED` or `RECORDED` (see [../evidence/README.md](../evidence/README.md)). A figure that outruns the provenance of its source is a claim outrunning its measurement.
