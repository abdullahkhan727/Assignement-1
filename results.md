# Saved Analysis Results

## Data Cleaning and Preprocessing

- The assigned dataset contained 7,043 rows and 21 columns.
- The only missing values were 11 blank `TotalCharges` entries.
- All 11 rows with blank `TotalCharges` had `tenure = 0`.
- The script converted `TotalCharges` to numeric and replaced the 11 blank values with `0.0`.
- No rows were removed.
- After cleaning, the seven analysis inputs contained no missing values.
- The script checked that all numeric analysis inputs were finite. It found no unresolved nonnumeric, infinite, or otherwise invalid numeric values.
- The supplied script did not perform statistical outlier detection, outlier removal, or outlier capping.
- For Logistic Regression and Boosted Trees, `StandardScaler` standardized the three numeric inputs: `tenure`, `MonthlyCharges`, and `TotalCharges`.
- The scaler and categorical encoder were fit using training rows only. The categorical inputs were one-hot encoded.
- The Contract rule did not use standardized inputs; it used training-row churn rates grouped by `Contract`.

## Final-Test AUC and 95% Bootstrap Intervals

| Method | Final-test AUC | 95% bootstrap interval |
|---|---:|---:|
| Contract rule | 0.7372652354749543 | 0.7162586607994378 to 0.7557311497055941 |
| Logistic Regression | 0.8471427833320417 | 0.8254145260287559 to 0.8699459652728027 |
| Boosted Trees | 0.8496654524787517 | 0.8282673461945358 to 0.8731642295413213 |

## Pairwise AUC Comparisons Against the Contract Rule

| Comparison | Exact AUC difference | Paired 95% bootstrap interval | Crosses zero? |
|---|---:|---:|---|
| Logistic Regression minus Contract | 0.10987754785708748 | 0.09313630029071265 to 0.12844992793473903 | No |
| Boosted Trees minus Contract | 0.11240021700379743 | 0.09589134628060791 to 0.13111680606778364 | No |

Source files: `outputs/test_metrics.csv` and `outputs/intervals.csv`.
