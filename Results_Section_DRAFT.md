# 3. Results

## 3.1 Field LAI and Sentinel-2 predictor data

Plot-level mean field LAI was available for 59 plots in Rema-Kalenga Wildlife Sanctuary (source domain) and 40 plots in Khadimnagar National Park (KNP; target domain). In Rema-Kalenga, LAI ranged from 0.33 to 4.29 (mean 2.17). A random 80/20 split produced 47 training and 12 held-out test plots with the same mean LAI (2.17) and ranges of 0.35–4.29 and 0.33–3.62, respectively. LAI in KNP spanned a wider range (0.29–5.41; mean 2.44, SD 1.22), and three KNP plots (LAI = 4.71, 4.92 and 5.41) exceeded the maximum LAI of the Rema-Kalenga training set (4.29).

Rema-Kalenga predictors were derived from 15 Sentinel-2 surface-reflectance scenes acquired between 1 January and 27 March 2021 (cloud cover 0–17.2%; 14 of 15 scenes ≤ 3.9%). KNP predictors were derived from 11 scenes acquired between 1 January and 22 March 2022 (cloud cover 0–23.7%). Cloud Score+-masked median composites provided 15 candidate predictors per plot (ten spectral bands and five vegetation indices), averaged over each 15 m × 15 m plot.

## 3.2 Hyperparameter tuning and predictor selection

Hyperparameters were tuned by leave-one-out cross-validation (LOOCV) on the 47 training plots (RF: 108 combinations; XGBoost: 108; KNN: 32; SVR: 48 specified, 36 evaluated after excluding redundant linear-kernel/`auto`-gamma pairs). With all 15 predictors, the best LOOCV RMSE was 0.762 for RF, 0.770 for SVR, 0.778 for KNN and 0.821 for XGBoost (Table 1). Tuning had a limited effect: within each model, the five best configurations differed by at most 0.021 LAI units in LOOCV RMSE. All five best SVR configurations used the linear kernel, and the selected XGBoost configuration lay at the lower edge of the search grid for the number of trees (100), tree depth (2) and learning rate (0.01).

Recursive feature elimination (RFE), run on the training plots only, lowered the LOOCV RMSE of every model relative to its 15-predictor baseline, by 0.032 (XGBoost) to 0.061 (SVR) LAI units (Table 1; Fig. 2). The optimal subsets contained 7 (RF), 4 (KNN), 13 (SVR) and 3 (XGBoost) predictors. The error curves were shallow: across the full elimination sequence, LOOCV RMSE varied between 0.719 and 0.764 for RF, 0.738 and 0.796 for KNN, and 0.789 and 0.821 for XGBoost, and less smoothly between 0.709 and 0.891 for SVR. The stopping rule (an RMSE increase of >0.15) was never triggered; all four runs ended at the three-predictor minimum, and for XGBoost the optimum coincided with that minimum.

The selected subsets differed among models (Table 1). B11 (SWIR1), NDVI and B3 (green) were retained by three of the four models, B4, B5, B6, EVI, GNDVI, MSAVI and SAVI by two, and no predictor was retained by all four. B7 was not retained by any model.

**Table 1.** Tuned hyperparameters, LOOCV RMSE before and after recursive feature elimination (RFE), and selected predictors (Rema-Kalenga training set, n = 47).

| Model | Tuned hyperparameters | LOOCV RMSE, 15 predictors | Selected predictors (n) | LOOCV RMSE, selected |
|---|---|---|---|---|
| RF | 100 trees, max depth 5, min. leaf size 1, max features = √p | 0.762 | B3, B4, B5, B11, B12, NDVI, SAVI (7) | 0.719 |
| KNN | k = 6, uniform weights, Manhattan distance | 0.778 | B3, B6, EVI, MSAVI (4) | 0.738 |
| SVR | Linear kernel, C = 100, ε = 0.2 | 0.770 | B2, B3, B4, B5, B6, B8, B8A, B11, NDVI, EVI, SAVI, GNDVI, MSAVI (13) | 0.709 |
| XGBoost | 100 trees, max depth 2, learning rate 0.01, subsample 1.0 | 0.821 | B11, NDVI, GNDVI (3) | 0.789 |

## 3.3 Model performance in Rema-Kalenga

On the held-out test plots (n = 12), RF performed best (R² = 0.254, RMSE = 0.774, MAE = 0.577, rRMSE = 35.7%), followed by XGBoost (R² = 0.185, RMSE = 0.809, rRMSE = 37.3%; Table 2, Fig. 3). SVR and KNN had negative test R² (−0.057 and −0.159), i.e. they did not outperform a constant prediction at the test-set mean, and had the largest errors (RMSE = 0.920 and 0.964; rRMSE = 42.5% and 44.5%). Test-set bias was small for RF (+0.020), XGBoost (−0.030) and KNN (−0.057) but larger for SVR (−0.381).

In-sample fit differed markedly among models. RF fitted the training plots closely (R² = 0.849, RMSE = 0.342) but its test R² was much lower, whereas the in-sample R² of the other models (0.434–0.556) was closer to their test performance. For all four models, test RMSE was higher than the corresponding LOOCV RMSE in Table 1 (RF 0.774 vs 0.719; XGBoost 0.809 vs 0.789; SVR 0.920 vs 0.709; KNN 0.964 vs 0.738).

Errors were concentrated in a few plots (per-plot residuals are given in Supplementary Table S1). Plot 5 (observed LAI = 0.33, the lowest in the test set) was the largest error for every model and was overpredicted by 1.74 (RF), 1.82 (XGBoost), 1.87 (SVR) and 2.47 (KNN) LAI units. Plots 6 (LAI = 3.62) and 51 (LAI = 3.00) were underpredicted by all four models (residuals −0.86 to −1.60). SVR also produced a negative LAI prediction (−0.21) for plot 46. In a sensitivity check excluding plot 5, test RMSE was 0.614 (RF), 0.643 (XGBoost), 0.678 (KNN) and 0.778 (SVR), and R² was 0.302, 0.235, 0.149 and −0.120, respectively; the ranking of RF and XGBoost as the better models was unchanged.

**Table 2.** Model performance in Rema-Kalenga for the RFE-selected predictors. Training statistics are in-sample. Bias is the mean of (predicted − observed).

| Model | Set | N | R² | RMSE | MAE | rRMSE (%) | Bias |
|---|---|---|---|---|---|---|---|
| RF | Train | 47 | 0.849 | 0.342 | 0.268 | 15.8 | −0.020 |
| RF | Test | 12 | 0.254 | 0.774 | 0.577 | 35.7 | 0.020 |
| KNN | Train | 47 | 0.519 | 0.610 | 0.489 | 28.2 | 0.040 |
| KNN | Test | 12 | −0.159 | 0.964 | 0.684 | 44.5 | −0.057 |
| SVR | Train | 47 | 0.556 | 0.586 | 0.435 | 27.0 | −0.022 |
| SVR | Test | 12 | −0.057 | 0.920 | 0.694 | 42.5 | −0.381 |
| XGBoost | Train | 47 | 0.434 | 0.662 | 0.523 | 30.6 | −0.006 |
| XGBoost | Test | 12 | 0.185 | 0.809 | 0.647 | 37.3 | −0.030 |

## 3.4 Spatial transfer to KNP

Applying the frozen Rema-Kalenga models to the 40 KNP plots without retraining gave the ranking XGBoost > RF > KNN > SVR by RMSE, MAE, rRMSE and R² (Table 3, Fig. 4). XGBoost achieved the lowest error (RMSE = 0.957, MAE = 0.769, rRMSE = 39.2%) and the highest R² (0.373), with a mean bias of +0.111. RF (R² = 0.177, RMSE = 1.096, rRMSE = 44.9%) and KNN (R² = 0.055, RMSE = 1.175, rRMSE = 48.1%) explained little of the variance in KNP LAI. SVR performed worst (R² = −0.900, RMSE = 1.665, rRMSE = 68.2%). All four models overpredicted KNP LAI on average (bias +0.111 to +0.841), in contrast to the near-zero biases in Rema-Kalenga (Tables 2 and 3). Pearson correlations between observed and predicted LAI were 0.711 (XGBoost), 0.672 (RF), 0.552 (SVR) and 0.381 (KNN).

Relative to the Rema-Kalenga test set, rRMSE increased for all four models on transfer, by 1.9 percentage points for XGBoost, 3.6 for KNN, 9.2 for RF and 25.7 for SVR. Although XGBoost's RMSE was higher in KNP than in the Rema-Kalenga test set (0.957 vs 0.809), its R² was higher (0.373 vs 0.185), because the variability of observed LAI was larger in KNP (SD 1.22 vs 0.94 in the test set).

Prediction spread differed among models. The SD of predicted LAI in KNP was 0.44 (XGBoost), 0.48 (KNN) and 0.84 (RF), all well below the observed SD of 1.22, so predictions were compressed towards the mean. KNN, RF and XGBoost overpredicted all five KNP plots with observed LAI < 1.0 (residuals +0.72 to +2.16) and underpredicted all five plots with observed LAI > 4.0 (residuals −0.07 to −2.50). The largest XGBoost underprediction was for the highest-LAI plot (plot 14, observed 5.41, predicted 3.05). In contrast, SVR predictions were more dispersed than the observations (SD 1.71), with several implausibly high values (plot 37: 7.54; plot 24: 7.08; plot 29: 6.34), all above the maximum observed KNP LAI (5.41) and the maximum of the training data (4.29); these three plots account for its largest errors (+4.60, +3.62 and +4.35).

**Table 3.** Spatial transfer performance of the Rema-Kalenga models applied to KNP (n = 40, no retraining). Bias is the mean of (predicted − observed); r is the Pearson correlation between observed and predicted LAI.

| Model | R² | RMSE | MAE | rRMSE (%) | Bias | r |
|---|---|---|---|---|---|---|
| XGBoost | 0.373 | 0.957 | 0.769 | 39.2 | 0.111 | 0.711 |
| RF | 0.177 | 1.096 | 0.911 | 44.9 | 0.632 | 0.672 |
| KNN | 0.055 | 1.175 | 0.974 | 48.1 | 0.364 | 0.381 |
| SVR | −0.900 | 1.665 | 1.279 | 68.2 | 0.841 | 0.552 |

## 3.5 Predictor importance

Predictor importance was model-specific (Table 4). In RF, importance was spread almost evenly across the seven retained predictors (0.09–0.16), with B4, B12, B3, NDVI and SAVI within 0.013 of each other. In XGBoost, the SWIR band B11 had the highest importance (0.445), followed by GNDVI (0.322) and NDVI (0.233). For the linear SVR, the largest absolute standardised coefficient belonged to MSAVI (11.67), followed by EVI (4.99), NDVI (4.94) and B4 (4.65); because these indices are derived from the same bands, individual coefficients should not be read as independent effects. KNN has no native importance measure, so none is reported.

**Table 4.** Predictor importance in the final models (RFE-selected predictors). RF and XGBoost: gain-/impurity-based importance (sums to 1); SVR: absolute coefficient of the linear kernel on standardised predictors.

| Model | Importance (highest to lowest) |
|---|---|
| RF | B4 0.164, B12 0.159, B3 0.156, NDVI 0.155, SAVI 0.151, B11 0.125, B5 0.091 |
| XGBoost | B11 0.445, GNDVI 0.322, NDVI 0.233 |
| SVR | MSAVI 11.67, EVI 4.99, NDVI 4.94, B4 4.65, GNDVI 3.61, B3 3.51, B8 2.71, SAVI 2.45, B2 1.44, B5 1.09, B11 0.73, B6 0.71, B8A 0.42 |
| KNN | Not available |

## 3.6 Summary of model comparison

No model explained more than 40% of the variance in KNP LAI (highest R² = 0.373) or more than 26% in the Rema-Kalenga test set (highest R² = 0.254). RF was the best model in the Rema-Kalenga test set and XGBoost was the best on transfer to KNP; XGBoost had the smallest transfer bias and the smallest increase in rRMSE. The differences between RF and XGBoost in the test set (ΔRMSE = 0.035) are small relative to the limited number of test plots (n = 12), and no uncertainty intervals were estimated for the metrics. SVR was the least reliable under transfer, and KNN, which showed the weakest correlation with observed LAI (r = 0.381), was also the least accurate in the Rema-Kalenga test set.

---
*Placeholders: Fig. 2 = RFE curves (LOOCV RMSE vs number of predictors) for the four models; Fig. 3 = observed vs predicted LAI, Rema-Kalenga test set; Fig. 4 = observed vs predicted LAI, KNP transfer. Supplementary Table S1 = per-plot residuals (printed by each notebook). Renumber to match your manuscript.*
