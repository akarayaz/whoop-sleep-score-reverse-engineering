# Reverse-Engineering WHOOP's Sleep Performance Score

A single-subject (n=91 nights) case study that reconstructs WHOOP's official Sleep
Performance formula from its 4 documented components, using linear regression validated
with leave-one-out cross-validation.

## Motivation

WHOOP publishes *what* goes into its Sleep Performance score (Hours vs. Needed, Sleep
Efficiency, Sleep Consistency, Sleep Stress) but not the exact model behind it. This project
asks a simple question: **how closely can a transparent linear model reconstruct that
black-box score using only the documented ingredients?**

It's also a small working prototype of the kind of wearable-data modeling pipeline used in
sports science for athlete load and readiness monitoring — the broader area this project is
meant to build toward.

## Methodology

1. **Data cleaning** — 93 nights of WHOOP data (9 Apr – 11 Jul 2026), 2 dropped due to
   missing Sleep Consistency (needs a 4-day rolling history), leaving 91 clean nights.
2. **EDA** — distribution and correlation analysis of all 4 components against Sleep
   Performance.
3. **Model A** — standardized multiple linear regression (all 4 components), evaluated
   both in-sample and with Leave-One-Out Cross-Validation for an honest out-of-sample
   estimate.
4. **Efficiency deep dive** — Sleep Efficiency showed no significant linear effect, so this
   is investigated further with a threshold (high vs. low efficiency) comparison and a
   statistical power analysis to check whether the null result reflects a real absence of
   effect or simply insufficient data.

## Key findings

- The 4 components explain **95.5%** of the variance in Sleep Performance in-sample, and
  **92.2%** out-of-sample (LOOCV) — average prediction error of 1.91 points vs. 6.97 points
  for guessing the mean.
- `Hours vs. Needed` and `Sleep Consistency` are the strongest positive drivers;
  `Sleep Stress` has a clear negative effect. All three are significant at p < 0.001.
- `Sleep Efficiency` showed no significant linear effect (p = 0.899) — but this isn't the
  full story. A threshold test found a difference in the expected direction (3.57 points,
  Cohen's d = 0.332), and a power analysis showed that reliably detecting an effect that
  size would require ~144 low-efficiency nights per group; this dataset had only 10. The
  metric is very likely real but statistically under-powered here, not irrelevant.

## Running it

pip install -r requirements.txt
jupyter notebook whoop_sleep_analysis.ipynb

All cell outputs (tables, plots) are already saved in the notebook, so it can also just be
read directly on GitHub without running anything.

## Limitations

- n=1 (single subject, 91 nights) — findings describe this individual, not a general
  population.
- Sleep Efficiency stayed in a narrow, high range (91–96%) for most of the tracking period,
  limiting what could be detected — quantified directly via the power analysis above.
- Residuals show mild non-normality, likely driven by a handful of high-Sleep-Stress
  outlier nights.

## Author

**Alper Karayaz** — Data Science & Engineering, Politecnico di Torino
