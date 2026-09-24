# Case 1 Analysis


## Validation Choice - September 24, 2026

Selected method: Trees



**Which Method had the strongest validation ranking?**
Boosted Trees had the highest validation ranking. It had a 0.8456 AUC, which was slightly higher than logistic regression and much higher than contract rules. 

**Which method produced the most concentrated top-20% contact list?**
Boosted Trees also produced the highest concentrated top-20% list at 65.12 percent. Logistic regression was a little less at 61.21 percent. The lowest was contract rules at 41.64%.

**Does the small difference between trees and logistic regression affect your choice?**
The AUC was a bit higher but Boosted trees also had a higher top 20% churn rate. This makes me favor Boosted trees if validation performance is what you are looking for. Logistics Regression was also good but boosted trees was better in multiple ways.

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

The main cleaning issue was 11 missing Total Charges values, which were replaced with 0 because all of those customers had zero tenure. No rows were removed and standardization was only used for the numeric inputs in Logistic Regression and Boosted Trees.

## Final-Test AUC and 95% Bootstrap Intervals

| Method | Final-test AUC | 95% bootstrap interval |
|---|---:|---:|
| Contract rule | 0.7372652354749543 | 0.7162586607994378 to 0.7557311497055941 |
| Logistic Regression | 0.8471427833320417 | 0.8254145260287559 to 0.8699459652728027 |
| Boosted Trees | 0.8496654524787517 | 0.8282673461945358 to 0.8731642295413213 |

For Final-Test AUC and 95% Bootstrap Intervals:

Boosted Trees had the highest final-test AUC at 0.8497, followed closely by Logistic Regression at 0.8471, while the Contract rule was lower at 0.7373. The bootstrap intervals show the range of AUC values that are reasonably consistent with the test sample.

## Pairwise AUC Comparisons Against the Contract Rule

| Comparison | Exact AUC difference | Paired 95% bootstrap interval | Crosses zero? |
|---|---:|---:|---|
| Logistic Regression minus Contract | 0.10987754785708748 | 0.09313630029071265 to 0.12844992793473903 | No |
| Boosted Trees minus Contract | 0.11240021700379743 | 0.09589134628060791 to 0.13111680606778364 | No |

Both Logistic Regression and Boosted Trees had positive AUC differences compared with the Contract rule, and neither paired 95% interval crossed zero. This means both methods showed a clear ranking improvement over the Contract rule in the bootstrap analysis.

Source files: `outputs/test_metrics.csv` and `outputs/intervals.csv`.

