# A/B Test Statistical Validation

Rigorous hypothesis testing on an e-commerce landing-page A/B test, using four independent
statistical lenses to answer one question: **should the new page ship?**

**Dataset:** [A/B Testing — Kaggle](https://www.kaggle.com/datasets/zhangluyuan/ab-testing) (294,478 sessions). Download `ab_data.csv` from there and place it next to the notebook before running.

## TL;DR

Ran chi-square, Welch's t-test, one-way ANOVA, and 10,000-iteration bootstrap resampling on the
control/treatment conversion split. All four agree: **no statistically or practically significant
difference**. Don't ship the new page on conversion grounds.

| Test | Statistic | p-value | Decision (α=0.05) |
|---|---|---|---|
| Chi-square (independence) | χ² = 0.025 | 0.874 | Fail to reject H₀ |
| Welch's t-test | t = -0.165 | 0.869 | Fail to reject H₀ |
| One-way ANOVA (day of week) | F = 2.699 | 0.013 | Reject H₀ (see note below) |
| Bootstrap resampling | diff = -0.02% | 0.870 | Fail to reject H₀ |

The ANOVA flags real day-to-day variation in conversion — but a balance check confirms the
control/treatment split stayed ~50/50 every day, so it doesn't bias the group comparison. Full
reasoning is in the notebook.

## Why four tests instead of one

Any single test rests on its own assumptions. Running chi-square (categorical association),
a t-test (mean difference), ANOVA (multi-group variance), and a model-free bootstrap side by side
means the conclusion isn't an artifact of one method's quirks — if they'd disagreed, that
disagreement would itself be the finding worth digging into.

## Repo structure

```
TASK_04_A_B_TESTING/
├── ab_testing_hypothesis_validation.ipynb   # full analysis, runs end to end
├── AB_Test_Stakeholder_Report.md            # non-technical summary (GitHub-rendered)
├── images/                                  # charts used in the stakeholder report
└── README.md
```

## Running it

```bash
pip install pandas numpy scipy matplotlib seaborn jupyter
jupyter nbconvert --to notebook --execute --inplace ab_testing_hypothesis_validation.ipynb
```

Every random operation is seeded (`np.random.default_rng(42)`), so the numbers above reproduce
exactly on re-run.

## What's inside the notebook

1. Business problem framing — translating "should we ship the new page" into four testable
   hypotheses.
2. Data cleaning — dropping known group/landing_page mismatch rows and de-duplicating users
   (skipping this step silently corrupts every test downstream).
3. EDA — conversion rate by group with 95% CIs, daily and day-of-week trends.
4. Four hypothesis tests, each with a plain-English interpretation of what the p-value actually
   means for the business decision — not just "significant" or "not significant."
5. A summary table and a stakeholder-facing recommendation.

For the non-technical version of the findings, see [`AB_Test_Stakeholder_Report.md`](AB_Test_Stakeholder_Report.md).

## Stack

`pandas` · `numpy` · `scipy.stats` · `matplotlib` / `seaborn` for EDA and bootstrap visualization.
