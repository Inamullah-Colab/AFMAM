# AFMAM

Normality diagnostics report and datasets.

Python script (reproduce diagnostics and plots):
https://github.com/Inamullah-Colab/Inamullah-Colab.github.io/blob/master/assets/normality-report/normality_tests.py


# A Friendly Report on Normality, Variance, and What It Means for Causal Discovery

This is a short blog-style report that explains **why** we check normality and variance, **what the plots mean**, and **how each dataset behaves**. The numbers below come from `ALL_dataset_summary.csv`, and the plots were generated into `Check for the normality/plots/`.

---

## Why we do this

Causal discovery algorithms often make assumptions about the data distribution.

- **PC** and **NOTEARS** (linear-Gaussian) usually work best when variables are close to Gaussian.
- **LiNGAM** depends on **non-Gaussian noise** to recover directions.
- **GraNDAG** is more flexible for **nonlinear** patterns, but heavier to train.

So before we run any model, we first check whether each dataset looks Gaussian or not. That gives us a practical, data-driven choice of method.

---

## Quick global view (all datasets together)

### Pass-rate overview (normality tests)

This plot shows the fraction of columns that pass each normality test (p >= 0.05). Higher is more Gaussian-like.

![Pass rates by dataset](plots/pass_rate_by_dataset.png)

### Skew vs kurtosis (all columns)

Points near (0, 0) are close to Gaussian. Large deviations mean heavy tails or skew.

![Skew vs kurtosis scatter](plots/skew_kurtosis_scatter.png)

---

## Summary table (dataset level)

| Dataset         | n_cols | Shapiro pass | K2 pass | JB pass | AD pass | mean |skew| | mean |kurt| |
|----------------|--------|--------------|---------|---------|---------|-----------|-----------|
| HighDim-D_data | 200    | 0.415        | 0.415   | 0.415   | 0.460   | 20.345    | 1520.341  |
| HighDim-S_data | 200    | 0.945        | 0.950   | 0.940   | 0.955   | 0.026     | 0.047     |
| LowDim-D_data  | 20     | 0.900        | 0.900   | 0.900   | 0.950   | 0.053     | 0.535     |
| LowDim-L_data  | 20     | 1.000        | 0.900   | 0.950   | 1.000   | 0.018     | 0.063     |
| LowDim-N_data  | 20     | 0.900        | 0.950   | 0.950   | 0.950   | 0.024     | 0.105     |
| LowDim-P_data  | 20     | 0.900        | 0.900   | 0.900   | 0.950   | 0.024     | 0.047     |
| MidDim-C_data  | 50     | 0.920        | 0.920   | 0.920   | 0.960   | 0.029     | 0.050     |
| MidDim-D_data  | 100    | 0.780        | 0.780   | 0.780   | 0.820   | 1.099     | 70.508    |
| MidDim-P_data  | 100    | 0.970        | 0.970   | 0.970   | 0.980   | 0.024     | 0.048     |
| MidDim-S_data  | 100    | 0.920        | 0.940   | 0.940   | 0.920   | 0.582     | 27.523    |

How to read this table:
- **Pass rate** near 1.0 = many columns look Gaussian.
- **Mean |skew|** and **mean |kurtosis|** large = strong non-Gaussian behavior.

---

## Dataset-by-dataset notes (with plots)

Each dataset below includes:
- **p-value histograms** (Shapiro, K2, JB)
- **QQ + histogram samples** (3 random columns)
- A short practical recommendation

### HighDim-D_data

Strong non-Gaussian behavior (very low pass rates, extreme skew/kurtosis).
- **Suggested**: LiNGAM or GraNDAG (if nonlinear).

![HighDim-D p-values](plots/pvalues_HighDim-D_data.png)
![HighDim-D QQ + hist](plots/qq_hist_HighDim-D_data.png)

### HighDim-S_data

Very Gaussian-like (high pass rates, tiny skew/kurtosis).
- **Suggested**: PC or NOTEARS.

![HighDim-S p-values](plots/pvalues_HighDim-S_data.png)
![HighDim-S QQ + hist](plots/qq_hist_HighDim-S_data.png)

### LowDim-D_data

Mostly Gaussian; mild tail behavior in some columns.
- **Suggested**: PC/NOTEARS.

![LowDim-D p-values](plots/pvalues_LowDim-D_data.png)
![LowDim-D QQ + hist](plots/qq_hist_LowDim-D_data.png)

### LowDim-L_data

Very Gaussian-like.
- **Suggested**: PC/NOTEARS.

![LowDim-L p-values](plots/pvalues_LowDim-L_data.png)
![LowDim-L QQ + hist](plots/qq_hist_LowDim-L_data.png)

### LowDim-N_data

Gaussian-like.
- **Suggested**: PC/NOTEARS.

![LowDim-N p-values](plots/pvalues_LowDim-N_data.png)
![LowDim-N QQ + hist](plots/qq_hist_LowDim-N_data.png)

### LowDim-P_data

Gaussian-like.
- **Suggested**: PC/NOTEARS.

![LowDim-P p-values](plots/pvalues_LowDim-P_data.png)
![LowDim-P QQ + hist](plots/qq_hist_LowDim-P_data.png)

### MidDim-C_data

Gaussian-like.
- **Suggested**: PC/NOTEARS.

![MidDim-C p-values](plots/pvalues_MidDim-C_data.png)
![MidDim-C QQ + hist](plots/qq_hist_MidDim-C_data.png)

### MidDim-D_data

Non-Gaussian, heavy tails (skew/kurtosis high, lower pass rates).
- **Suggested**: LiNGAM; GraNDAG if nonlinear.

![MidDim-D p-values](plots/pvalues_MidDim-D_data.png)
![MidDim-D QQ + hist](plots/qq_hist_MidDim-D_data.png)

### MidDim-P_data

Gaussian-like.
- **Suggested**: PC/NOTEARS.

![MidDim-P p-values](plots/pvalues_MidDim-P_data.png)
![MidDim-P QQ + hist](plots/qq_hist_MidDim-P_data.png)

### MidDim-S_data

Mixed: pass rates are high, but skew/kurtosis are elevated.
- **Suggested**: PC/NOTEARS may still work, but LiNGAM can help if non-Gaussian noise is present.

![MidDim-S p-values](plots/pvalues_MidDim-S_data.png)
![MidDim-S QQ + hist](plots/qq_hist_MidDim-S_data.png)

---

## Limitations (quick honesty section)

- These are **univariate** tests; multivariate normality can still fail.
- Many columns means many tests; some failures are expected.
- Always combine this with domain knowledge and downstream model checks.

---

## Files you can reuse

- `ALL_dataset_summary.csv`: dataset-level summary.
- `ALL_univariate_summary.csv`: column-level metrics.
- `plots/`: all visualizations used in this report.

