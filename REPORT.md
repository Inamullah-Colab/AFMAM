# Normality and Variance Diagnostics Report

This report summarizes why we run normality and variance checks, what the plots and tables mean, and what the results suggest for each dataset in `Check for the normality`. The summary values below come from `ALL_dataset_summary.csv`.

## Why run these checks

Many causal discovery and SEM algorithms make assumptions about the data distribution:
- **PC** and **NOTEARS** (linear-Gaussian variants) often work best when variables are close to Gaussian.
- **LiNGAM** explicitly assumes **non-Gaussian** noise to identify causal directions.
- **GraNDAG** is flexible for **nonlinear** relationships and can tolerate non-Gaussian data, but is heavier computationally.

These checks help decide which algorithm is a better statistical match before you invest time in model fitting.

## What the tests and plots tell you

We use multiple normality tests because each has different sensitivity:
- **Shapiro-Wilk**: reliable for smaller samples (up to ~5k).
- **D'Agostino K2** and **Jarque-Bera**: sensitive to skew/kurtosis in larger samples.
- **Anderson-Darling**: emphasizes tail behavior.

The report table includes:
- **Pass rates**: fraction of columns with p-value >= 0.05 (higher means more Gaussian-like).
- **Mean absolute skew / kurtosis**: larger values mean stronger deviations from Gaussian.

Visualizations (if you generated them in the notebook):
- **P-value histograms**: a flat distribution suggests normality; many values near 0 suggests non-normality.
- **Skew vs kurtosis scatter**: points near (0,0) are close to Gaussian.
- **QQ plots**: a straight line indicates normality; curves indicate skew or heavy tails.

## Dataset-level summary (from ALL_dataset_summary.csv)

Values are shown with 3 decimals where appropriate.

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

## Interpretation and suggested algorithm choice

Notes:
- Pass rates above ~0.9 with tiny skew/kurtosis are consistent with Gaussian data.
- Large skew/kurtosis means strong non-normality.
- These are **univariate** diagnostics; multivariate normality can still fail.

### HighDim-D_data
- Very low pass rates and extremely high skew/kurtosis.
- **Conclusion**: strongly non-Gaussian.
- **Suggestion**: **LiNGAM** is more appropriate than PC/NOTEARS; GraNDAG if you expect nonlinear relationships.

### HighDim-S_data
- Very high pass rates and near-zero skew/kurtosis.
- **Conclusion**: close to Gaussian.
- **Suggestion**: **PC** or **NOTEARS** (linear-Gaussian) should be a good fit.

### LowDim-D_data
- High pass rates; low skew; moderate kurtosis.
- **Conclusion**: mostly Gaussian, mild tails.
- **Suggestion**: **PC/NOTEARS** likely fine; LiNGAM not required.

### LowDim-L_data
- Excellent pass rates; very low skew/kurtosis.
- **Conclusion**: Gaussian-like.
- **Suggestion**: **PC/NOTEARS**.

### LowDim-N_data
- High pass rates; low skew/kurtosis.
- **Conclusion**: Gaussian-like.
- **Suggestion**: **PC/NOTEARS**.

### LowDim-P_data
- High pass rates; low skew/kurtosis.
- **Conclusion**: Gaussian-like.
- **Suggestion**: **PC/NOTEARS**.

### MidDim-C_data
- High pass rates; low skew/kurtosis.
- **Conclusion**: Gaussian-like.
- **Suggestion**: **PC/NOTEARS**.

### MidDim-D_data
- Lower pass rates; high skew/kurtosis.
- **Conclusion**: non-Gaussian, heavy tails.
- **Suggestion**: **LiNGAM** is more suitable; consider GraNDAG if nonlinearity is expected.

### MidDim-P_data
- Very high pass rates; low skew/kurtosis.
- **Conclusion**: Gaussian-like.
- **Suggestion**: **PC/NOTEARS**.

### MidDim-S_data
- Pass rates are high but skew/kurtosis are elevated.
- **Conclusion**: mixed; some variables likely non-Gaussian.
- **Suggestion**: PC/NOTEARS may still work, but **LiNGAM** could improve directionality if non-Gaussian noise is present.

## Limitations and next steps

- These are univariate tests; multivariate normality may differ.
- With many columns, p-values can show failures due to multiple testing.
- Consider standardization and robust estimation if heavy tails persist.

If you want, I can also generate a PDF or HTML report with plots embedded and add it to the repo.
