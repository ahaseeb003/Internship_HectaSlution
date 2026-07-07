    # Task 02 — Advanced Data Cleaning & Preprocessing

**Student:** Abdul Haseeb ([@ahaseeb003](https://github.com/ahaseeb003))
**Program:** 2-Month ML Internship — Month 1
**Dataset:** [NYC Airbnb Open Data 2019](https://www.kaggle.com/datasets/dgomonov/new-york-city-airbnb-open-data) (`AB_NYC_2019.csv`)

---

## 📌 Overview

This task builds a **reusable, production-grade data cleaning pipeline** for a real, messy
dataset — NYC Airbnb listings from 2019. The goal wasn't just to "remove NaNs," but to
diagnose *why* each problem exists in the data and choose a defensible, documented fix for
each one, the way a data/ML engineer would on the job.

The dataset was loaded directly from Google Drive (`/content/drive/MyDrive/dataset/AB_NYC_2019.csv`)
rather than an API, and processed entirely in Google Colab.

---

## 🗂️ Repository Structure

```
Task_02_Advanced_Data_Cleaning_and_Preprocessing/
│
├── README.md                                          ← this file
├── Task_02_Advanced_Data_Cleaning_and_Preprocessing.ipynb   ← main task notebook
├── Task_02_Bonus_Challenge.ipynb                      ← bonus: sklearn transformer
├── REPORT.md                                          ← findings, decisions, visuals (if included)
└── Screenshots/                                       ← screenshots of notebook outputs
    ├── 01_missing_values_heatmap_raw.png
    ├── 02_missing_values_barchart_raw.png
    ├── 03_price_boxplot_before_after.png
    ├── 04_minimum_nights_boxplot_before_after.png
    ├── 05_missing_values_heatmap_cleaned.png
    ├── 06_price_by_neighbourhood_group.png
    ├── 07_listings_map_scatter.png
    ├── 08_bonus_pipeline_output.png
    └── 09_cleaned_dataframe_head.png
```

---

## 🧭 My Approach

I broke the problem into four stages, matching how I'd approach a real dataset handed to me
at work with no prior context.

### 1. Diagnose before touching anything (EDA)

Before writing a single cleaning function, I profiled the raw data to understand what was
actually wrong with it:

- Missing-value heatmap and per-column missing counts
- Duplicate-row check
- Boxplots for `price` and `minimum_nights` to visually confirm outliers
- Cross-checked whether missingness in `reviews_per_month` / `last_review` correlated with
  `number_of_reviews == 0`

This step mattered because it changed my strategy. For example, I initially assumed
`reviews_per_month` was "just missing" — but the data showed it was missing **only** for
listings with zero reviews. That's not random noise, it's a structural fact about the listing,
so it needed a different fix than ordinary imputation (see below).

### 2. Design the pipeline as small, single-purpose functions

Rather than one large cleaning script, I split the logic into independently testable
functions, each with one job:

| Function | Responsibility |
|---|---|
| `fix_types()` | Converts `last_review` to datetime; casts categorical text columns to `category` dtype |
| `validate_schema()` | Enforces realistic business ranges (no `price ≤ 0`, no `minimum_nights < 1`, coordinates must fall inside NYC's bounding box) — invalid values become `NaN` rather than being silently kept |
| `handle_structural_missingness()` | Explicitly encodes the "zero reviews → no review data" pattern instead of treating it as random missingness |
| `handle_outliers()` | Caps (Winsorizes) `price` and `minimum_nights` using the IQR method, so extreme values are contained without deleting rows |
| `handle_missing_values()` | Imputes remaining gaps — median for numeric columns, `'Unknown'` for missing text fields |
| `remove_duplicates()` | Drops exact duplicate rows |
| `clean_pipeline()` | Composes all of the above in the correct order |

**Why this order matters:** type-fixing and schema validation run *first*, so that later
steps (outlier capping, imputation) are working with genuinely valid numbers — not strings,
not impossible values. Missing-value imputation runs *last*, after outliers are already
capped, so a handful of extreme values don't distort the median used for imputation.

### 3. Make every fix defensible with a reason, not just a default

Every cleaning decision is backed by a specific reason tied to what the data actually showed,
not a blanket "fill NaN with 0" approach:

- **`price == 0`** → converted to `NaN`, then imputed with the median (a free nightly rate
  isn't realistic, and price is heavily right-skewed, so median beats mean).
- **`minimum_nights` outliers (1000+ nights)** → capped via IQR rather than dropped, so no
  listings are lost, but extreme values can't distort downstream models.
- **`reviews_per_month` missing when `number_of_reviews == 0`** → filled with `0`, plus a new
  `has_reviews` flag — because this is *meaningful* missingness, not random.
- **`last_review` missing** → deliberately left as `NaT`, not imputed. Fabricating a review
  date for a listing that's never been reviewed would introduce false information.
- **`name` / `host_name` missing** → filled with `'Unknown'`, since these are descriptive text
  fields, not used numerically.
- **Invalid latitude/longitude** (outside NYC's bounding box) → treated as data-entry errors,
  set to `NaN`, then imputed with the median.
- **Duplicate rows** → dropped, keeping the first occurrence.

### 4. Validate the result, don't just trust it

After running the pipeline, I re-checked the same diagnostics from step 1 (missing-value
heatmap, duplicate count, boxplots) on the cleaned data to visually and numerically confirm
the fixes actually worked, rather than assuming success.

---

## 🎁 Bonus Challenge

The same pipeline logic is re-implemented as `AirbnbCleaningTransformer`, a class inheriting
from scikit-learn's `BaseEstimator` and `TransformerMixin`, so it can be dropped into a
standard `sklearn.pipeline.Pipeline`.

The key engineering decision here was keeping `fit` and `transform` cleanly separated:
`fit()` learns statistics (medians, IQR bounds) **only from the training data**, and
`transform()` applies those already-learned values to any new data. This avoids **data
leakage** — a validation/test set is never allowed to influence the statistics used to clean
the training set.

Full write-up of what worked, what didn't, and what I'd improve is in `REPORT.md` under
`## Bonus`.

---

## ▶️ How to Run

1. Open either notebook in **Google Colab**.
2. Run the first cell to mount Google Drive.
3. Make sure `AB_NYC_2019.csv` exists at:
   ```
   /content/drive/MyDrive/dataset/AB_NYC_2019.csv
   ```
   (Edit the `DATA_PATH` variable near the top of the notebook if your file lives elsewhere.)
4. Run all cells top to bottom (`Runtime → Run all`).
5. Cleaned outputs are saved back to Drive under:
   ```
   /content/drive/MyDrive/Task_02_Advanced_Data_Cleaning_and_Preprocessing/
   ```

---

## 🛠️ Tools & Libraries

- `pandas`, `numpy` — data manipulation
- `matplotlib`, `seaborn` — visualization
- `scikit-learn` (`BaseEstimator`, `TransformerMixin`, `Pipeline`, `StandardScaler`) — bonus challenge
- Google Colab + Google Drive — execution environment and storage

---

## 🖼️ Screenshots

The `Screenshots/` folder contains captured images of key notebook outputs (missing-value
heatmaps, before/after boxplots, the cleaned DataFrame, and the bonus pipeline output) for
quick visual reference without needing to re-run the notebooks. See
`Screenshots/README.md` for the full list and naming convention.

---

## 📈 Key Results

| Metric | Before Cleaning | After Cleaning |
|---|---|---|
| Rows with missing values | Several columns affected | Only `last_review` retains `NaT` (by design) |
| Duplicate rows | Present | 0 |
| `price` outliers | Extreme right-skew, unrealistic 0-values | Capped via IQR, 0-values imputed |
| `minimum_nights` outliers | Values of 1000+ nights | Capped via IQR |
| Invalid coordinates | A small number outside NYC bounds | Corrected via schema validation + imputation |

*(See `REPORT.md` for the full visual before/after comparisons.)*

---

## 👤 Author

**Abdul Haseeb** — [@ahaseeb003](https://github.com/ahaseeb003/Internship_HectaSlution)
Part of the [ML-Internship-Tasks](https://github.com/musagithub1/ML-Internship-Tasks) repository.