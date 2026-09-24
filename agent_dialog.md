
Earlier messages are available — press ctrl + t to view the full transcript

FILES

Name                   Length LastWriteTime
----                   ------ -------------
compare_run.json          678 9/24/2026 3:25:02 PM
evaluate_run.json         761 9/24/2026 3:45:03 PM
intervals.csv             481 9/24/2026 3:45:03 PM
probability_groups.csv    365 9/24/2026 3:45:03 PM
scenarios.csv             721 9/24/2026 3:45:03 PM
split_rows.csv          89064 9/24/2026 3:44:57 PM
test_metrics.csv          436 9/24/2026 3:45:03 PM
test_predictions.csv   103318 9/24/2026 3:45:03 PM
validation.csv            441 9/24/2026 3:44:57 PM


✓ • 2.33s

The final evaluation completed successfully using boosted trees as the recorded choice. These are explanatory learning notes, not assessed-response or memo text.

## Final-test performance

The final test set contains 1,409 customers. Each method’s top-20% list contains 281 customers because the script rounds down.

| Method | Final-test AUC | Top-20% churn rate | Top-20% mean prediction |
|---|---:|---:|---:|
| Contract rule | 0.7373 | 39.86% | 42.75% |
| Logistic regression | 0.8471 | 69.40% | 64.04% |
| Boosted trees | **0.8497** | **69.40%** | 64.79% |

The overall test-set churn rate was 26.54%.

How to understand these results:

- AUC measures ranking ability. For example, the trees AUC of 0.8497 means the model assigns a higher score to a randomly selected churner than to a randomly selected non-churner about 85% of the time, with half credit for ties.
- Both predictive models rank customers substantially better than the contract rule.
- Trees have the highest AUC, but only slightly higher than logistic regression.
- The trees and logistic lists both have a 69.40% observed churn rate, compared with 39.86% for the contract list and 26.54% overall.
- Therefore, the predictive models produced much more concentrated contact lists than the contract rule.
- Equal top-20% churn rates do not necessarily mean trees and logistic selected exactly the same customers.

## AUC intervals

These are percentile intervals from 1,000 bootstrap samples of the fixed final-test predictions.

| Method | AUC | 95% bootstrap interval |
|---|---:|---:|
| Contract rule | 0.7373 | 0.7163 to 0.7557 |
| Logistic regression | 0.8471 | 0.8254 to 0.8699 |
| Boosted trees | 0.8497 | 0.8283 to 0.8732 |

These intervals describe uncertainty caused by which similar rows happened to appear in the test sample. They do not cover retraining uncertainty, future market changes, or whether an offer actually prevents churn.

## Paired AUC differences

The bootstrap scores both methods on the same resampled rows before calculating each difference.

| Comparison | AUC difference | 95% paired interval |
|---|---:|---:|
| Logistic − contract | +0.1099 | +0.0931 to +0.1284 |
| Trees − contract | **+0.1124** | **+0.0959 to +0.1311** |
| Trees − logistic | +0.0025 | −0.0049 to +0.0103 |

Interpretation:

- Trees outperform the contract rule by approximately 0.112 AUC. The interval remains above zero, providing consistent evidence of better ranking in these bootstrap samples.
- Logistic regression similarly outperforms the contract rule.
- Trees exceed logistic regression by only 0.0025 AUC.
- The trees-versus-logistic interval includes zero, so this test sample does not establish which of those two methods truly ranks better.
- “Includes zero” does not prove equality; it means the observed difference is too small to determine a reliable direction from this evaluation.

## Trees calibration

Calibration compares predicted probabilities with observed churn frequencies.

### Overall and contact-list calibration

| Population | Mean trees prediction | Observed churn rate | Difference |
|---|---:|---:|---:|
| Entire test set | 27.05% | 26.54% | +0.51 percentage points |
| Trees top-20% list | 64.79% | 69.40% | −4.61 percentage points |

Overall, trees are closely calibrated on average. Within the selected contact list, the model underpredicts observed churn by about 4.6 percentage points.

### Probability groups

| Predicted range | Customers | Mean prediction | Observed churn |
|---|---:|---:|---:|
| 0.0–0.2 | 724 | 7.96% | 7.18% |
| 0.2–0.4 | 272 | 29.78% | 25.37% |
| 0.4–0.6 | 236 | 49.63% | 52.12% |
| 0.6–0.8 | 142 | 67.69% | 69.01% |
| 0.8–1.0 | 35 | 83.45% | 91.43% |

What to notice:

- Observed churn generally increases with predicted risk, which is consistent with useful ranking.
- Predictions are relatively close to observed rates in most groups.
- The model overpredicts the 0.2–0.4 group by about 4.4 percentage points.
- It underpredicts the highest-risk group by about 8 percentage points.
- The highest group contains only 35 customers, so its observed rate is comparatively uncertain.
- Good AUC does not guarantee perfect calibration: ranking and probability accuracy are separate properties.

## Value scenarios

These estimates use each method’s fixed top-20% list, a $66 net value per additional retention, and a $6.20 contact cost. They are hypothetical planning calculations—not measured profits.

| Method | Assumed save rate | Net value per 1,000 contacts |
|---|---:|---:|
| Contract | 10% | −$3,569.40 |
| Contract | 15% | −$2,254.09 |
| Contract | 20% | −$938.79 |
| Logistic | 10% | −$1,619.93 |
| Logistic | 15% | +$670.11 |
| Logistic | 20% | +$2,960.14 |
| Trees | 10% | −$1,619.93 |
| Trees | 15% | +$670.11 |
| Trees | 20% | +$2,960.14 |

Trees and logistic have identical scenario values because their top-20% lists have the same observed churn rate, 69.40%. The scenario formula uses that rate rather than AUC or the individual predicted probabilities.

For trees:

- At a 10% save rate, the campaign loses about $1,620 per 1,000 contacts.
- At 15%, it gains about $670.
- At 20%, it gains about $2,960.
- Therefore, the assumed effectiveness of the offer can reverse the financial conclusion.

## Break-even save rate

For trees:

\[
\frac{\$6.20}{0.69395 \times \$66}=0.13537
\]

The break-even save rate is approximately **13.54%**. Under these assumptions, the offer must prevent churn for about 13.5% of customers who otherwise would have churned within the selected list.

For comparison, the contract list requires a 23.57% save rate because it contains fewer actual churners.

The dataset does not estimate the save rate. Only a controlled campaign—ideally a randomized pilot with contacted and control groups—could measure whether the offer causes enough additional retention to exceed that threshold.

All final outputs are saved in [outputs](C:/Users/abdul/Downloads/VD1_student_files/outputs).

Worked for 1m 11s

I have recorded boosted trees as my validation-stage choice in analysis.md. Run the final evaluation using trees as my recorded choice. Show me the final-test results, including all three AUCs, top-20% churn rates, AUC intervals and paired differences, calibration results for trees, the 10%, 15%, and 20% value scenarios, and the break-even save rate. Explain what each output means so I can write my own answers.

I’ll rerun the final evaluation with the recorded `trees` choice and summarize the fresh output as explanatory notes for your own writing.

$ Select-String -LiteralPath 'analysis.md' -Pattern 'Selected method:\s*Trees|Boosted Trees' -CaseSensitive:$false; & 'C:\Users\abdul\anaconda3\python.exe' VD1_analysis.py evaluate --csv churn.csv --out outputs --choice trees

analysis.md:6:Selected method: Trees
analysis.md:11:Boosted Trees had the highest validation ranking. It had a 0.8456 AUC, which was slightly higher than
logistic regression and much higher than contract rules.
VALIDATION RESULTS
  method    n      auc  contact_n  top20_churn_rate  top20_mean_prediction  mean_prediction  observed_churn_rate
contract 1409 0.742724        281          0.416370               0.427530         0.258283             0.265436
logistic 1409 0.838402        281          0.612100               0.651202         0.271429             0.265436
   trees 1409 0.845558        281          0.651246               0.662771         0.272776             0.265436
Highest validation AUC: trees
FINAL TEST RESULTS
  method    n      auc  contact_n  top20_churn_rate  top20_mean_prediction  mean_prediction  observed_churn_rate
contract 1409 0.737265        281          0.398577               0.427530         0.268615             0.265436
logistic 1409 0.847143        281          0.693950               0.640393         0.273363             0.265436
   trees 1409 0.849665        281          0.693950               0.647865         0.270473             0.265436

AUC INTERVALS
             comparison  estimate       low     high
               contract  0.737265  0.716259 0.755731
               logistic  0.847143  0.825415 0.869946
                  trees  0.849665  0.828267 0.873164
logistic minus contract  0.109878  0.093136 0.128450
   trees minus contract  0.112400  0.095891 0.131117
   trees minus logistic  0.002523 -0.004862 0.010284

SCENARIOS: hypothetical dollars, not measured savings
  method  historical_list_churn_rate  assumed_save_rate  net_per_1000_contacts  break_even_save_rate
contract                    0.398577               0.10           -3569.395018              0.235687
contract                    0.398577               0.15           -2254.092527              0.235687
contract                    0.398577               0.20            -938.790036              0.235687
logistic                    0.693950               0.10           -1619.928826              0.135369
logistic                    0.693950               0.15             670.106762              0.135369
logistic                    0.693950               0.20            2960.142349              0.135369
   trees                    0.693950               0.10           -1619.928826              0.135369
   trees                    0.693950               0.15             670.106762              0.135369
   trees                    0.693950               0.20            2960.142349              0.135369


✓ • 13.74s

The evaluation reran successfully with `trees` as the recorded choice. The final test set contains 1,409 customers; each top-20% list contains 281 customers.

These are explanatory notes for you to use when writing your own responses.

## Final-test results

The overall final-test churn rate is 26.54%.

| Method | AUC | Top-20% churn rate | Top-20% mean prediction |
|---|---:|---:|---:|
| Contract rule | 0.7373 | 39.86% | 42.75% |
| Logistic regression | 0.8471 | 69.40% | 64.04% |
| Boosted trees | **0.8497** | **69.40%** | 64.79% |

Meaning:

- AUC measures ranking quality, not classification accuracy. Trees’ 0.8497 AUC means a randomly selected churner generally receives a higher risk score than a randomly selected non-churner about 85% of the time, allowing half credit for ties.
- Trees and logistic regression rank customers much better than the contract rule.
- Trees have the highest AUC, but the advantage over logistic regression is very small.
- The trees and logistic top-20% lists each have a 69.40% observed churn rate, compared with 39.86% for the contract list and 26.54% overall.
- Equal list churn rates do not necessarily mean logistic regression and trees selected identical customers.

## AUC intervals

| Method | AUC | 95% bootstrap interval |
|---|---:|---:|
| Contract rule | 0.7373 | 0.7163–0.7557 |
| Logistic regression | 0.8471 | 0.8254–0.8699 |
| Boosted trees | 0.8497 | 0.8283–0.8732 |

These intervals come from 1,000 bootstrap samples of the fixed test predictions. They represent uncertainty associated with the test sample. They do not include uncertainty from retraining, future customer changes, or the effectiveness of a retention offer.

## Paired AUC differences

| Comparison | Difference | 95% paired interval |
|---|---:|---:|
| Logistic − contract | +0.1099 | +0.0931 to +0.1284 |
| Trees − contract | **+0.1124** | **+0.0959 to +0.1311** |
| Trees − logistic | +0.0025 | −0.0049 to +0.0103 |

Meaning:

- Both predictive models show a sizable ranking improvement over the contract rule.
- The paired intervals for logistic-versus-contract and trees-versus-contract remain above zero.
- Trees exceed logistic regression by only 0.0025 AUC.
- Because the trees-minus-logistic interval includes zero, the results do not establish a reliable advantage for either predictive model.
- Including zero does not prove the models are equal; it means the observed difference is not precise enough to determine its direction confidently.

## Trees calibration

Calibration concerns whether predicted probabilities agree with observed frequencies.

### Overall and top-20%

| Population | Mean prediction | Observed churn | Difference |
|---|---:|---:|---:|
| Entire final-test set | 27.05% | 26.54% | +0.51 percentage points |
| Trees top-20% list | 64.79% | 69.40% | −4.61 percentage points |

Trees are close overall but underpredict churn within the selected list by approximately 4.6 percentage points.

### Probability groups

| Predicted range | Customers | Mean prediction | Observed churn |
|---|---:|---:|---:|
| 0.0–0.2 | 724 | 7.96% | 7.18% |
| 0.2–0.4 | 272 | 29.78% | 25.37% |
| 0.4–0.6 | 236 | 49.63% | 52.12% |
| 0.6–0.8 | 142 | 67.69% | 69.01% |
| 0.8–1.0 | 35 | 83.45% | 91.43% |

Observed churn generally rises with predicted risk, supporting useful ranking. Some mismatches remain:

- The 0.2–0.4 group is overpredicted by about 4.4 percentage points.
- The highest group is underpredicted by about 8 percentage points.
- That highest group has only 35 customers, so its observed percentage is relatively uncertain.

A model can have strong AUC while its probabilities remain imperfectly calibrated.

## Value scenarios

These calculations use a $66 net value per additional retention and a $6.20 cost per contact.

| Method | 10% save rate | 15% save rate | 20% save rate |
|---|---:|---:|---:|
| Contract rule | −$3,569.40 | −$2,254.09 | −$938.79 |
| Logistic regression | −$1,619.93 | +$670.11 | +$2,960.14 |
| Boosted trees | **−$1,619.93** | **+$670.11** | **+$2,960.14** |

Amounts are per 1,000 contacts.

Meaning:

- At a 10% save rate, the trees campaign loses about $1,620 per 1,000 contacts.
- At 15%, it gains about $670.
- At 20%, it gains about $2,960.
- The assumed offer effect therefore changes the financial conclusion.
- Trees and logistic have identical scenario values because the calculation uses their identical 69.40% list churn rates.
- These are hypothetical planning estimates, not measured campaign profits. The churn data do not reveal how many customers an offer would actually save.

## Break-even save rate

For boosted trees:

\[
\frac{\$6.20}{0.69395 \times \$66}=0.13537
\]

The break-even save rate is **13.54%**.

That means the offer must retain approximately 13.5% of the selected customers who otherwise would have churned to cover contact costs under the stated assumptions. The corresponding contract-rule threshold is much higher at 23.57%.

A randomized pilot with contacted and control groups would be needed to estimate the actual causal save rate.

The refreshed outputs are available in [outputs](C:/Users/abdul/Downloads/VD1_student_files/outputs).

I have recorded boosted trees as my validation-stage choice in analysis.md. Run the final evaluation using trees as my recorded choice. Show me the final-test results, including all three AUCs, top-20% churn rates, AUC intervals and paired differences, calibration results for trees, the 10%, 15%, and 20% value scenarios, and the break-even save rate. Explain the outputs to me,

I’ll rerun the recorded trees evaluation and present the resulting metrics with plain-language explanations.

$ & 'C:\Users\abdul\anaconda3\python.exe' VD1_analysis.py evaluate --csv churn.csv --out outputs --choice trees
VALIDATION RESULTS
  method    n      auc  contact_n  top20_churn_rate  top20_mean_prediction  mean_prediction  observed_churn_rate
contract 1409 0.742724        281          0.416370               0.427530         0.258283             0.265436
logistic 1409 0.838402        281          0.612100               0.651202         0.271429             0.265436
   trees 1409 0.845558        281          0.651246               0.662771         0.272776             0.265436
Highest validation AUC: trees
FINAL TEST RESULTS
  method    n      auc  contact_n  top20_churn_rate  top20_mean_prediction  mean_prediction  observed_churn_rate
contract 1409 0.737265        281          0.398577               0.427530         0.268615             0.265436
logistic 1409 0.847143        281          0.693950               0.640393         0.273363             0.265436
   trees 1409 0.849665        281          0.693950               0.647865         0.270473             0.265436

AUC INTERVALS
             comparison  estimate       low     high
               contract  0.737265  0.716259 0.755731
               logistic  0.847143  0.825415 0.869946
                  trees  0.849665  0.828267 0.873164
logistic minus contract  0.109878  0.093136 0.128450
   trees minus contract  0.112400  0.095891 0.131117
   trees minus logistic  0.002523 -0.004862 0.010284

SCENARIOS: hypothetical dollars, not measured savings
  method  historical_list_churn_rate  assumed_save_rate  net_per_1000_contacts  break_even_save_rate
contract                    0.398577               0.10           -3569.395018              0.235687
contract                    0.398577               0.15           -2254.092527              0.235687
contract                    0.398577               0.20            -938.790036              0.235687
logistic                    0.693950               0.10           -1619.928826              0.135369
logistic                    0.693950               0.15             670.106762              0.135369
logistic                    0.693950               0.20            2960.142349              0.135369
   trees                    0.693950               0.10           -1619.928826              0.135369
   trees                    0.693950               0.15             670.106762              0.135369
   trees                    0.693950               0.20            2960.142349              0.135369
✓ • 13.35s

The final evaluation completed successfully using boosted trees. The test set has 1,409 customers; each top-20% list contains 281.

## Final-test performance

| Method | AUC | Top-20% churn rate |
|---|---:|---:|
| Contract rule | 0.7373 | 39.86% |
| Logistic regression | 0.8471 | 69.40% |
| Boosted trees | **0.8497** | **69.40%** |

The overall test churn rate is 26.54%.

AUC measures ranking quality: trees and logistic regression rank likely churners substantially better than the contract rule. Trees have the highest AUC, but their advantage over logistic regression is very small. Both predictive models create a much more concentrated contact list than either the contract rule or the full test population.

## AUC intervals

| Method | AUC | 95% bootstrap interval |
|---|---:|---:|
| Contract rule | 0.7373 | 0.7163–0.7557 |
| Logistic regression | 0.8471 | 0.8254–0.8699 |
| Boosted trees | 0.8497 | 0.8283–0.8732 |

These intervals measure test-sample uncertainty for the fixed predictions. They do not account for retraining, future population changes, or whether an offer causes customers to stay.

## Paired AUC differences

| Comparison | Difference | 95% paired interval |
|---|---:|---:|
| Logistic − contract | +0.1099 | +0.0931 to +0.1284 |
| Trees − contract | **+0.1124** | **+0.0959 to +0.1311** |
| Trees − logistic | +0.0025 | −0.0049 to +0.0103 |

Both predictive models show a clear ranking improvement over the contract rule because their paired intervals remain above zero. The trees-versus-logistic interval includes zero, so the data do not establish which of those two models ranks better. This does not prove that they are equal.

## Trees calibration

| Population | Mean prediction | Observed churn |
|---|---:|---:|
| Entire test set | 27.05% | 26.54% |
| Trees top-20% list | 64.79% | 69.40% |

Trees are close overall but underpredict churn in the selected list by approximately 4.6 percentage points.

| Predicted range | Customers | Mean prediction | Observed churn |
|---|---:|---:|---:|
| 0.0–0.2 | 724 | 7.96% | 7.18% |
| 0.2–0.4 | 272 | 29.78% | 25.37% |
| 0.4–0.6 | 236 | 49.63% | 52.12% |
| 0.6–0.8 | 142 | 67.69% | 69.01% |
| 0.8–1.0 | 35 | 83.45% | 91.43% |

Observed churn generally increases with predicted risk, showing useful ranking. The highest-risk group is underpredicted, but it has only 35 customers, making its observed rate less precise. Strong AUC does not guarantee perfectly accurate probabilities.

## Value scenarios

The following are hypothetical net values per 1,000 contacts:

| Method | 10% save rate | 15% save rate | 20% save rate |
|---|---:|---:|---:|
| Contract rule | −$3,569.40 | −$2,254.09 | −$938.79 |
| Logistic regression | −$1,619.93 | +$670.11 | +$2,960.14 |
| Boosted trees | **−$1,619.93** | **+$670.11** | **+$2,960.14** |

For trees, the campaign loses money at a 10% save rate but becomes positive at 15% and 20%. Logistic and trees have identical scenario values because their selected lists have the same observed churn rate.

These values are not measured profits. The CSV shows churn risk but does not measure how many customers would be retained because of an offer.

## Break-even save rate

For trees:

\[
\$6.20 \div (0.69395 \times \$66)=0.13537
\]

The break-even save rate is **13.54%**. Under the assignment’s assumptions, the offer must prevent churn among approximately 13.5% of selected customers who otherwise would have churned.

The refreshed files are in [outputs](C:/Users/abdul/Downloads/VD1_student_files/outputs).

pastes the results for question 1 in "C:\Users\abdul\Downloads\VD1_student_files\analysis.md"

I can add the factual run results to Q1, but I’ll keep them as raw results and reproducibility details—not assessed interpretation—because the handout requires you to write the explanatory prose yourself. I’ll inspect the current file before editing.

$ Get-Content -LiteralPath 'C:\Users\abdul\Downloads\VD1_student_files\analysis.md'
\# Case 1 Analysis


\## Validation Choice - September 24, 2026

Selected method: Trees



**Which Method had the strongest validation ranking?**
Boosted Trees had the highest validation ranking. It had a 0.8456 AUC, which was slightly higher than logistic regression and much higher than contract rules.

**Which method produced the most concentrated top-20% contact list?**


**Does the small difference between trees and logistic regression affect your choice?**
✓ • 1.97s

I found the existing validation-choice notes. I’m adding a separate Q1 section containing the command, dataset/split facts, shared inputs, method definitions, and raw validation table, leaving the interpretation prompts for you.

• Edited Downloads\VD1_student_files\analysis.md (+43 -1)
    15
    16 -**Does the small difference between trees and logistic regression affect your choice?**
    16 +**Does the small difference between trees and logistic regression affect your choice?**
    17 +
    18 +
    19 +## Q1 — Generate: Run record and results
    20 +
    21 +Command used:
    22 +
    23 +```text
    24 +python VD1_analysis.py compare --csv churn.csv --out outputs
    25 +```
    26 +
    27 +Dataset and split facts:
    28 +
    29 +- Source data: 7,043 rows and 21 columns
    30 +- Outcomes: 1,869 Churn = Yes and 5,174 Churn = No
    31 +- Training rows: 4,225
    32 +- Validation rows: 1,409
    33 +- Final-test rows: 1,409
    34 +- Cleaning recorded by the script: 11 blank `TotalCharges` values at zero tenure were filled with 0; no rows were removed
    35 +
    36 +All three methods use the same seven inputs: `tenure`, `MonthlyCharges`, `TotalCharges`, `Contract`, `InternetService`, `PaperlessBilling`, and `PaymentMethod`.
    37 +
    38 +Methods supplied by the script:
    39 +
    40 +- Contract rule: assigns churn probabilities from training-row churn rates within each contract category.
    41 +- Logistic regression: scales numeric inputs, one-hot encodes categorical inputs, and fits logistic regression with `C=1`.
    42 +- Boosted trees: uses the same inputs and fits 100 trees with depth 2 and learning rate 0.1.
    43 +
    44 +Validation results:
    45 +
    46 +| Method | n | AUC | Contact n | Top-20% churn rate | Top-20% mean prediction | Mean prediction | Observed churn rate |
    47 +|---|---:|---:|---:|---:|---:|---:|---:|
    48 +| Contract rule | 1,409 | 0.742724 | 281 | 0.416370 | 0.427530 | 0.258283 | 0.265436 |
    49 +| Logistic regression | 1,409 | 0.838402 | 281 | 0.612100 | 0.651202 | 0.271429 | 0.265436 |
    50 +| Boosted trees | 1,409 | 0.845558 | 281 | 0.651246 | 0.662771 | 0.272776 | 0.265436 |
    51 +
    52 +Generated comparison files:
    53 +
    54 +- `outputs/validation.csv`
    55 +- `outputs/split_rows.csv`
    56 +- `outputs/compare_run.json`
    57 +
    58 +Add your own explanation of what the three methods do, what AUC measures, and why the top-20% result matters for the business decision.

$ Get-Content -LiteralPath 'C:\Users\abdul\Downloads\VD1_student_files\analysis.md' | Select-Object -Last 55
\## Validation Choice - September 24, 2026

Selected method: Trees



**Which Method had the strongest validation ranking?**
Boosted Trees had the highest validation ranking. It had a 0.8456 AUC, which was slightly higher than logistic regression and much higher than contract rules.

**Which method produced the most concentrated top-20% contact list?**


**Does the small difference between trees and logistic regression affect your choice?**


## Q1 â€” Generate: Run record and results

Command used:

```text
python VD1_analysis.py compare --csv churn.csv --out outputs
```

Dataset and split facts:

- Source data: 7,043 rows and 21 columns
- Outcomes: 1,869 Churn = Yes and 5,174 Churn = No
- Training rows: 4,225
- Validation rows: 1,409
- Final-test rows: 1,409
- Cleaning recorded by the script: 11 blank `TotalCharges` values at zero tenure were filled with 0; no rows were removed

All three methods use the same seven inputs: `tenure`, `MonthlyCharges`, `TotalCharges`, `Contract`, `InternetService`, `PaperlessBilling`, and `PaymentMethod`.

Methods supplied by the script:

- Contract rule: assigns churn probabilities from training-row churn rates within each contract category.
- Logistic regression: scales numeric inputs, one-hot encodes categorical inputs, and fits logistic regression with `C=1`.
- Boosted trees: uses the same inputs and fits 100 trees with depth 2 and learning rate 0.1.

Validation results:

| Method | n | AUC | Contact n | Top-20% churn rate | Top-20% mean prediction | Mean prediction | Observed churn rate |
|---|---:|---:|---:|---:|---:|---:|---:|
| Contract rule | 1,409 | 0.742724 | 281 | 0.416370 | 0.427530 | 0.258283 | 0.265436 |
| Logistic regression | 1,409 | 0.838402 | 281 | 0.612100 | 0.651202 | 0.271429 | 0.265436 |
| Boosted trees | 1,409 | 0.845558 | 281 | 0.651246 | 0.662771 | 0.272776 | 0.265436 |

Generated comparison files:

- `outputs/validation.csv`
- `outputs/split_rows.csv`
- `outputs/compare_run.json`

Add your own explanation of what the three methods do, what AUC measures, and why the top-20% result matters for the business decision.
✓ • 1.90s

• Edited Downloads\VD1_student_files\analysis.md (+1 -1)
    18
    19 -## Q1 — Generate: Run record and results
    19 +## Q1 - Generate: Run record and results
    20

Added the Q1 factual results to [analysis.md](C:/Users/abdul/Downloads/VD1_student_files/analysis.md), including:

- Command used
- Dataset and split facts
- Cleaning rule
- Seven shared inputs
- Brief factual descriptions of all three methods
- Complete validation-results table
- Generated output filenames

I left a prompt for you to write the assessed interpretation yourself, in accordance with the assignment’s AI-use policy.

can you do that again i think i mesed up

I’ll inspect the current `analysis.md` and restore the Q1 factual-results section without overwriting your own writing.

$ Get-Content -LiteralPath 'C:\Users\abdul\Downloads\VD1_student_files\analysis.md' -Raw
\# Case 1 Analysis


\## Validation Choice - September 24, 2026

Selected method: Trees



**Which Method had the strongest validation ranking?**
Boosted Trees had the highest validation ranking. It had a 0.8456 AUC, which was slightly higher than logistic regression and much higher than contract rules.

**Which method produced the most concentrated top-20% contact list?**


**Does the small difference between trees and logistic regression affect your choice?**


## Q1 - Generate: Run record and results

Command used:

```text
python VD1_analysis.py compare --csv churn.csv --out outputs
```

Dataset and split facts:

- Source data: 7,043 rows and 21 columns
- Outcomes: 1,869 Churn = Yes and 5,174 Churn = No
- Training rows: 4,225
- Validation rows: 1,409
- Final-test rows: 1,409
- Cleaning recorded by the script: 11 blank `TotalCharges` values at zero tenure were filled with 0; no rows were removed

All three methods use the same seven inputs: `tenure`, `MonthlyCharges`, `TotalCharges`, `Contract`, `InternetService`, `PaperlessBilling`, and `PaymentMethod`.

Methods supplied by the script:

- Contract rule: assigns churn probabilities from training-row churn rates within each contract category.
- Logistic regression: scales numeric inputs, one-hot encodes categorical inputs, and fits logistic regression with `C=1`.
- Boosted trees: uses the same inputs and fits 100 trees with depth 2 and learning rate 0.1.

Validation results:

| Method | n | AUC | Contact n | Top-20% churn rate | Top-20% mean prediction | Mean prediction | Observed churn rate |
|---|---:|---:|---:|---:|---:|---:|---:|
| Contract rule | 1,409 | 0.742724 | 281 | 0.416370 | 0.427530 | 0.258283 | 0.265436 |
| Logistic regression | 1,409 | 0.838402 | 281 | 0.612100 | 0.651202 | 0.271429 | 0.265436 |
| Boosted trees | 1,409 | 0.845558 | 281 | 0.651246 | 0.662771 | 0.272776 | 0.265436 |

Generated comparison files:

- `outputs/validation.csv`
- `outputs/split_rows.csv`
- `outputs/compare_run.json`

Add your own explanation of what the three methods do, what AUC measures, and why the top-20% result matters for the business decision.

✓ • 1.68s

The Q1 results are still intact. The visible problem is that the first two headings were escaped (`\#`), so I’m repairing those Markdown headings and leaving the results and your wording unchanged.

• Edited Downloads\VD1_student_files\analysis.md (+2 -2)
    1 -\# Case 1 Analysis
    1 +# Case 1 Analysis
    2
    3
    4 -\## Validation Choice - September 24, 2026
    4 +## Validation Choice - September 24, 2026
    5

$ Select-String -LiteralPath 'C:\Users\abdul\Downloads\VD1_student_files\analysis.md' -Pattern '^#|^## Q1|^\| Contract|^\| Logistic|^\| Boosted' | ForEach-Object { $_.Line }
# Case 1 Analysis
## Validation Choice - September 24, 2026
## Q1 - Generate: Run record and results
| Contract rule | 1,409 | 0.742724 | 281 | 0.416370 | 0.427530 | 0.258283 | 0.265436 |
| Logistic regression | 1,409 | 0.838402 | 281 | 0.612100 | 0.651202 | 0.271429 | 0.265436 |
| Boosted trees | 1,409 | 0.845558 | 281 | 0.651246 | 0.662771 | 0.272776 | 0.265436 |
✓ • 2.14s

Fixed [analysis.md](C:/Users/abdul/Downloads/VD1_student_files/analysis.md). The Q1 results table was already intact; I repaired the malformed Markdown headings so they render correctly. Your existing wording was preserved.

Review my existing churn.csv and VD1_analysis.py for Q2 of the assignment. Do not change any files or code and do not rerun or tune any models unless necessary. Report the data checks and cleaning performed by the supplied script, including:

number of rows and columns
number of Churn = Yes and Churn = No
any missing values found
specifically what happened with missing TotalCharges values
whether any rows were removed
any other important data-quality checks performed by the script

I’ll perform a read-only data audit against the checks coded in `VD1_analysis.py`. I won’t execute the analysis stages, tune models, or modify files.

$ $path='C:\Users\abdul\Downloads\VD1_student_files\churn.csv'; $d=Import-Csv -LiteralPath $path; Write-Output ('Rows: ' + $d.Count); Write-Output ('Columns: ' + @($d[0].PSObject.Properties).Count); Write-Output ('Churn Yes: ' + @($d | Where-Object Churn -eq 'Yes').Count); Write-Output ('Churn No: ' + @($d | Where-Object Churn -eq 'No').Count); Write-Output ('Unique customerID: ' + @($d.customerID | Sort-Object -Unique).Count); Write-Output 'Blank fields by column:'; foreach($p in $d[0].PSObject.Properties.Name){ $n=@($d | Where-Object { [string]::IsNullOrWhiteSpace($_.$p) }).Count; if($n -gt 0){ Write-Output ($p + ': ' + $n) } }; Write-Output ('Blank TotalCharges at tenure 0: ' + @($d | Where-Object { [string]::IsNullOrWhiteSpace($_.TotalCharges) -and [int]$_.tenure -eq 0 }).Count)
Rows: 7043
Columns: 21
Churn Yes: 1869
Churn No: 5174
Unique customerID: 7043
Blank fields by column:
TotalCharges: 11
Blank TotalCharges at tenure 0: 11
✓ • 4.93s

No files were changed, and no models were rerun or tuned.

## Data checks

| Check | Result |
|---|---:|
| Rows | 7,043 |
| Columns | 21 |
| `Churn = Yes` | 1,869 |
| `Churn = No` | 5,174 |
| Unique customer IDs | 7,043 |

The script requires the dataset to match these exact dimensions and outcome counts. Otherwise, it stops with an error indicating that it is not the assigned extract.

## Missing values and cleaning

A scan of all 21 columns found only:

- `TotalCharges`: 11 blank values
- Every other column: no blank values

All 11 blank `TotalCharges` records have `tenure = 0`.

The script:

1. Converts `TotalCharges` to numeric, turning nonnumeric or blank entries into missing numeric values.
2. Confirms that exactly 11 entries are blank.
3. Confirms that those 11 blanks are the only entries that failed numeric conversion.
4. Confirms that all 11 affected customers have zero tenure.
5. Replaces the 11 missing `TotalCharges` values with `0.0`.

This is consistent with customers whose service had just begun and therefore had no accumulated charges.

No rows were removed. The dataset remains at 7,043 rows after cleaning.

## Other data-quality checks

The supplied script also verifies that:

- The CSV file exists locally.
- All required columns are present:
  - `customerID`
  - `Churn`
  - The three numeric inputs
  - The four categorical inputs
- Every `customerID` is present.
- Every `customerID` is unique.
- The outcome counts exactly match the assigned extract.
- After fixing `TotalCharges`, none of the seven analysis inputs contains a missing value.
- All numeric analysis inputs contain finite numbers—no positive infinity, negative infinity, or unresolved nonnumeric values.
- `TotalCharges` does not contain unexpected invalid values beyond the 11 documented blanks.

The seven checked analysis inputs are:

- Numeric: `tenure`, `MonthlyCharges`, `TotalCharges`
- Categorical: `Contract`, `InternetService`, `PaperlessBilling`, `PaymentMethod`

One useful distinction: the script explicitly checks missingness in these seven analysis inputs, plus customer IDs. My read-only scan of the complete CSV additionally confirmed that the 11 `TotalCharges` blanks are the only blank fields anywhere in the 21-column file.

Update my analysis.md by adding the factual results for Q2 - Validate the Analysis under the existing ## Q2 heading.

For now, add only the Data Checks and Cleaning subsection. Include:

7,043 rows and 21 columns
1,869 Churn = Yes and 5,174 Churn = No
7,043 unique customer IDs
the only missing values were 11 blank TotalCharges values
all 11 had tenure = 0
the script converted TotalCharges to numeric and replaced those 11 blanks with 0
no rows were removed
after cleaning, the seven analysis inputs had no missing or invalid numeric values

Keep this section factual only. Do not write any interpretation or conclusions for me.

IMPORTANT: Do not delete, rewrite, edit, reformat, or replace any existing content in analysis.md. Only append this new subsection underneath the existing ## Q2 heading.

Do not rerun or tune any models.

I’ll locate the existing `## Q2` heading, then insert only the requested factual subsection directly beneath it. I won’t alter other content or run any models.

$ $p='C:\Users\abdul\Downloads\VD1_student_files\analysis.md'; Select-String -LiteralPath $p -Pattern '^## Q2' -Context 0,12 | ForEach-Object { $_.ToString() }; Write-Output '--- headings ---'; Select-String -LiteralPath $p -Pattern '^#{1,3} ' | ForEach-Object { ($_.LineNumber.ToString() + ': ' + $_.Line) }
> C:\Users\abdul\Downloads\VD1_student_files\analysis.md:72:## Q2
--- headings ---
1: # Case 1 Analysis
4: ## Validation Choice - September 24, 2026
19: ## Q1 - Generate: Run record and results
55: ### Explanation of Results
72: ## Q2
✓ • 1.86s

• Edited Downloads\VD1_student_files\analysis.md (+11 -0)
    72  ## Q2
    73 +
    74 +### Data Checks and Cleaning
    75 +
    76 +- The dataset contained 7,043 rows and 21 columns.
    77 +- The outcome counts were 1,869 `Churn = Yes` and 5,174 `Churn = No`.
    78 +- The dataset contained 7,043 unique customer IDs.
    79 +- The only missing values were 11 blank `TotalCharges` values.
    80 +- All 11 records with blank `TotalCharges` had `tenure = 0`.
    81 +- The script converted `TotalCharges` to numeric and replaced the 11 blank values with `0`.
    82 +- No rows were removed.
    83 +- After cleaning, the seven analysis inputs contained no missing values or invalid numeric values.

$ Get-Content -LiteralPath 'C:\Users\abdul\Downloads\VD1_student_files\analysis.md' | Select-Object -Last 18
Generated comparison files:

- `outputs/validation.csv`
- `outputs/split_rows.csv`
- `outputs/compare_run.json`

## Q2

### Data Checks and Cleaning

- The dataset contained 7,043 rows and 21 columns.
- The outcome counts were 1,869 `Churn = Yes` and 5,174 `Churn = No`.
- The dataset contained 7,043 unique customer IDs.
- The only missing values were 11 blank `TotalCharges` values.
- All 11 records with blank `TotalCharges` had `tenure = 0`.
- The script converted `TotalCharges` to numeric and replaced the 11 blank values with `0`.
- No rows were removed.
- After cleaning, the seven analysis inputs contained no missing values or invalid numeric values.
✓ • 1.83s

Added the factual `### Data Checks and Cleaning` subsection directly beneath the existing `## Q2` heading in [analysis.md](C:/Users/abdul/Downloads/VD1_student_files/analysis.md).

No other content was changed, and no models were run.

Continue Q2 in analysis.md by adding a factual subsection called ### Data Splitting and Separation. Report:

the number of training, validation, and final-test rows
how the data was split
whether the same source row appears in more than one partition
which partition was used to fit the models and preprocessing
which partition was used to choose the model
when the final-test partition was used

I’ll append the factual splitting-and-separation subsection within Q2, using the supplied code and saved partition assignments. I won’t modify existing text or run models.

$ $p='C:\Users\abdul\Downloads\VD1_student_files\outputs\split_rows.csv'; $d=Import-Csv -LiteralPath $p; $d | Group-Object partition | Select-Object Name,Count | Format-Table -AutoSize; Write-Output ('Total rows: ' + $d.Count); Write-Output ('Unique source rows: ' + @($d.source_row | Sort-Object -Unique).Count)

Name       Count
----       -----
validation  1409
train       4225
test        1409


Total rows: 7043
Unique source rows: 7043
✓ • 2.38s

• Edited Downloads\VD1_student_files\analysis.md (+11 -0)
    83  - After cleaning, the seven analysis inputs contained no missing values or invalid numeric values.
    84 +
    85 +### Data Splitting and Separation
    86 +
    87 +- The training partition contained 4,225 rows.
    88 +- The validation partition contained 1,409 rows.
    89 +- The final-test partition contained 1,409 rows.
    90 +- The script created a stratified 60% training, 20% validation, and 20% final-test split. It first separated 60% of the rows for training and then divided the remaining 40% equally between validation and final test. Both split steps used random seed 0 and preserved similar churn proportions.
    91 +- Each of the 7,043 source rows appeared in exactly one partition; no source row appeared in more than one partition.
    92 +- Only the training partition was used to fit the contract-group churn rates, numeric scaling, categorical encoding, logistic regression, and boosted-trees model.
    93 +- The validation partition was used to compare the three methods and record the method choice.
    94 +- The final-test partition was used only after the validation-stage choice had been recorded, during the final evaluation of that choice and the other fixed methods.

$ Get-Content -LiteralPath 'C:\Users\abdul\Downloads\VD1_student_files\analysis.md' | Select-Object -Last 26
- `outputs/split_rows.csv`
- `outputs/compare_run.json`

## Q2

### Data Checks and Cleaning

- The dataset contained 7,043 rows and 21 columns.
- The outcome counts were 1,869 `Churn = Yes` and 5,174 `Churn = No`.
- The dataset contained 7,043 unique customer IDs.
- The only missing values were 11 blank `TotalCharges` values.
- All 11 records with blank `TotalCharges` had `tenure = 0`.
- The script converted `TotalCharges` to numeric and replaced the 11 blank values with `0`.
- No rows were removed.
- After cleaning, the seven analysis inputs contained no missing values or invalid numeric values.

### Data Splitting and Separation

- The training partition contained 4,225 rows.
- The validation partition contained 1,409 rows.
- The final-test partition contained 1,409 rows.
- The script created a stratified 60% training, 20% validation, and 20% final-test split. It first separated 60% of the rows for training and then divided the remaining 40% equally between validation and final test. Both split steps used random seed 0 and preserved similar churn proportions.
- Each of the 7,043 source rows appeared in exactly one partition; no source row appeared in more than one partition.
- Only the training partition was used to fit the contract-group churn rates, numeric scaling, categorical encoding, logistic regression, and boosted-trees model.
- The validation partition was used to compare the three methods and record the method choice.
- The final-test partition was used only after the validation-stage choice had been recorded, during the final evaluation of that choice and the other fixed methods.
✓ • 1.85s

Added `### Data Splitting and Separation` under Q2 in [analysis.md](C:/Users/abdul/Downloads/VD1_student_files/analysis.md).

It includes the partition sizes, stratified splitting procedure, non-overlap check, training-only fitting, validation-stage selection, and final-test timing. No existing content was changed and no models were rerun.

Create a new file called results.md. Do not modify or delete any of my other files.

Add the factual results required by the instructor:

Data cleaning decisions, including missing values, outliers or invalid values, and whether/where standardization was used.
Final-test AUC for Contract rule, Logistic Regression, and Boosted Trees.
The 95% bootstrap interval for each method's AUC.
Pairwise AUC comparisons using Contract rule as the baseline:
Logistic Regression minus Contract
Boosted Trees minus Contract
Include the exact difference, paired 95% bootstrap interval, and whether the interval crosses zero.

Use the existing saved analysis outputs. Do not rerun, tune, or change any models unless the required saved results cannot be found. Keep the file factual and do not write my interpretation for me.

I’ll read the saved output tables and confirm `results.md` does not already exist. Then I’ll create only that new factual file, without running models or changing any existing file.

$ $dir='C:\Users\abdul\Downloads\VD1_student_files'; Write-Output ('results.md exists: ' + (Test-Path -LiteralPath (Join-Path $dir 'results.md'))); Write-Output 'test_metrics.csv'; Get-Content -LiteralPath (Join-Path $dir 'outputs\test_metrics.csv'); Write-Output 'intervals.csv'; Get-Content -LiteralPath (Join-Path $dir 'outputs\intervals.csv')
results.md exists: False
test_metrics.csv
method,n,auc,contact_n,top20_churn_rate,top20_mean_prediction,mean_prediction,observed_churn_rate
contract,1409,0.7372652354749543,281,0.398576512455516,0.4275300171526587,0.26861482637510287,0.2654364797728886
logistic,1409,0.8471427833320417,281,0.693950177935943,0.6403929393736062,0.2733632328180993,0.2654364797728886
trees,1409,0.8496654524787517,281,0.693950177935943,0.6478654375314628,0.2704734491434011,0.2654364797728886
intervals.csv
comparison,estimate,low,high
contract,0.7372652354749543,0.7162586607994378,0.7557311497055941
logistic,0.8471427833320417,0.8254145260287559,0.8699459652728027
trees,0.8496654524787517,0.8282673461945358,0.8731642295413213
logistic minus contract,0.10987754785708748,0.09313630029071265,0.12844992793473903
trees minus contract,0.11240021700379743,0.09589134628060791,0.13111680606778364
trees minus logistic,0.002522669146709955,-0.0048617134175355375,0.01028416683712332
✓ • 1.92s

• Added Downloads\VD1_student_files\results.md (+32 -0)
     1 +# Saved Analysis Results
     2 +
     3 +## Data Cleaning and Preprocessing
     4 +
     5 +- The assigned dataset contained 7,043 rows and 21 columns.
     6 +- The only missing values were 11 blank `TotalCharges` entries.
     7 +- All 11 rows with blank `TotalCharges` had `tenure = 0`.
     8 +- The script converted `TotalCharges` to numeric and replaced the 11 blank values with `0.0`.
     9 +- No rows were removed.
    10 +- After cleaning, the seven analysis inputs contained no missing values.
    11 +- The script checked that all numeric analysis inputs were finite. It found no unresolved nonnumeric, infinite, or otherwise invalid numeric values.
    12 +- The supplied script did not perform statistical outlier detection, outlier removal, or outlier capping.
    13 +- For Logistic Regression and Boosted Trees, `StandardScaler` standardized the three numeric inputs: `tenure`, `MonthlyCharges`, and `TotalCharges`.
    14 +- The scaler and categorical encoder were fit using training rows only. The categorical inputs were one-hot encoded.
    15 +- The Contract rule did not use standardized inputs; it used training-row churn rates grouped by `Contract`.
    16 +
    17 +## Final-Test AUC and 95% Bootstrap Intervals
    18 +
    19 +| Method | Final-test AUC | 95% bootstrap interval |
    20 +|---|---:|---:|
    21 +| Contract rule | 0.7372652354749543 | 0.7162586607994378 to 0.7557311497055941 |
    22 +| Logistic Regression | 0.8471427833320417 | 0.8254145260287559 to 0.8699459652728027 |
    23 +| Boosted Trees | 0.8496654524787517 | 0.8282673461945358 to 0.8731642295413213 |
    24 +
    25 +## Pairwise AUC Comparisons Against the Contract Rule
    26 +
    27 +| Comparison | Exact AUC difference | Paired 95% bootstrap interval | Crosses zero? |
    28 +|---|---:|---:|---|
    29 +| Logistic Regression minus Contract | 0.10987754785708748 | 0.09313630029071265 to 0.12844992793473903 | No |
    30 +| Boosted Trees minus Contract | 0.11240021700379743 | 0.09589134628060791 to 0.13111680606778364 | No |
    31 +
    32 +Source files: `outputs/test_metrics.csv` and `outputs/intervals.csv`.

$ Get-Content -LiteralPath 'C:\Users\abdul\Downloads\VD1_student_files\results.md'
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
✓ • 1.73s

Created [results.md](C:/Users/abdul/Downloads/VD1_student_files/results.md) using the existing saved outputs.

It contains:

- Factual cleaning and preprocessing decisions
- Invalid-value and outlier handling
- Standardization details
- All three final-test AUCs and bootstrap intervals
- Both requested comparisons against the contract baseline
- Exact differences, paired intervals, and whether each crosses zero

No models were rerun, and no other files were modified.

• Raw output mode on: transcript text is shown for clean terminal selection.