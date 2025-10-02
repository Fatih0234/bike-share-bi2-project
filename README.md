# Cyclistic Bike Share Analysis

This project analyzes Cyclistic's April 2021–March 2022 bike-share rides to contrast **casual** riders with **annual members** and surface actions that improve conversion. Exploratory analysis, feature engineering, and classification models were developed in the Jupyter notebook `1-vibing.ipynb` (HTML export included) and summarized in the accompanying presentation.

## Repository Snapshot
- `1-vibing.ipynb` / `1-vibing.html`: end-to-end data preparation, EDA, and modelling workflow.
- `An Analysis of Cyclist Bike ShareDataset.pdf`: stakeholder-facing slide deck.
- `dataset/`: 12 monthly CSVs from the Kaggle Cyclistic archive (Apr 2021–Mar 2022).
- `weather_cache_openmeteo/`: cached hourly weather pulls used to augment rides.
- `EDA/`, `EDA-intro/`, `Preprocessing/`, `Modelling/`: exported figures for reuse outside the notebook.

## Data Sources
- **Cyclistic trip data**: [Kaggle – Cyclistic Bike Share](https://www.kaggle.com/datasets/evangower/cyclistic-bike-share). Each CSV contains all rides for a month, with bike type, timestamps, and station metadata.
- **Weather enrichment**: Hourly temperature, precipitation, wind, humidity, and cloud cover fetched from the [Open-Meteo archive API](https://open-meteo.com/) for a ~1 km grid covering Chicago. Responses are cached locally to avoid repeat API calls.

## Data Preparation Highlights
1. **Unification & typing**: Combined the 12 monthly CSVs (~5.7M rides) into a single dataframe, standardised column schemas, and converted timestamp fields to `datetime`.
2. **Station gaps**: Missing `*_station_*` fields (7–22% per month, largely dockless e-bikes) were retained by labelling as `"Unknown"`, preserving ride counts while flagging the operational difference.
3. **Feature engineering**:
   - Trip metrics: distance via the Haversine formula, duration in minutes, average speed.
   - Calendar flags: start hour/day/month, ISO week, weekend indicator, rush-hour flag, categorical duration buckets, and meteorological seasons.
   - Weather bins: continuous variables plus coarse buckets (e.g., `temp_bin`, `precip_bin`, `wind_bin`) for non-linear effects.
4. **Outlier handling**:
   - Removed negative/zero durations and rides >24 hours (likely logging errors).
   - Filtered implausible speeds above 33 km/h and a lone location outlier.
   - Applied log+MAD analysis to document the long leisure tail without over-pruning.
5. **Weather join**: Rounded start coordinates to a grid, merged hourly weather, then forward/back-filled per cell and imputed any residual gaps with column means.
6. **Train/test design**: Reserved the most recent months (Feb–Mar 2022) for testing, using Apr 2021–Jan 2022 for training. Both splits were down-sampled to 20% to stay within local compute limits.

## Exploratory Insights
- **Weekend vs. weekday mix**: Members ride predominantly on weekdays, while casual riders skew to weekends—validating the commute vs. leisure hypothesis.
- **Hourly patterns**: Members show sharp 8 AM and 5 PM peaks on weekdays (commuters). Casual usage peaks mid-day, especially on weekends, pointing to leisure/tourist behaviour.
- **Trip length**: Median casual ride ≈16 minutes vs. ≈9.5 minutes for members, and the casual distribution has a much wider tail.
- **Seasonality**: Casual volumes are highly elastic to weather (summer spike, winter drop), whereas member usage remains steadier year-round.
- **Bike type preference**: Casual riders over-index on electric and docked bikes, whereas members favour classic bikes for predictable commutes.

Supporting visuals for these findings live under `EDA/` and `EDA-intro/`.

## Modelling Approach & Results
- **Goal**: Predict whether a ride is taken by a member (1) or casual rider (0) using trip characteristics and weather.
- **Feature pipeline**: `ColumnTransformer` that scales numeric fields, passes boolean flags, and one-hot encodes categorical bins so downstream estimators receive a sparse matrix.
- **Models tested**:
  1. Logistic Regression (`saga`, class-weight balanced) with ROC-AUC tuned `C`.
  2. Random Forest (`class_weight='balanced_subsample'`) with depth/min-split grid search.
  3. XGBoost (hist tree method) with tuned depth and learning rate plus `scale_pos_weight` for the member-heavy mix.
- **Evaluation**: Hold-out test set (Feb–Mar 2022) and grid-search CV on training folds. Key metrics below:

| Model                | Accuracy | Casual Recall | Member Recall | Macro F1 |
|----------------------|---------:|--------------:|--------------:|---------:|
| Logistic Regression  | 0.766    | 0.36          | 0.92          | 0.655    |
| Random Forest        | 0.779    | 0.38          | 0.93          | 0.673    |
| XGBoost              | **0.786**| 0.35          | **0.95**      | 0.671    |

- **Interpretation**:
  - All models strongly identify members but struggle with the rarer casual class, underscoring class imbalance and overlapping behaviour.
  - Feature-importance charts (`Modelling/*.png`) highlight time-of-day, ride duration/speed, bike type, and temperature as the strongest signals—matching EDA narratives about commute vs. leisure usage.
  - Business implication: focus interventions where signals align (e.g., warm-weekend, long leisure rides) and consider cost-sensitive training or threshold tuning to boost casual recall when conversion outreach is the goal.

## Reproducing the Analysis
1. **Environment**: Python ≥3.10 with packages `pandas`, `numpy`, `scikit-learn`, `xgboost`, `matplotlib`, `seaborn`, `folium`, and `requests`. Install them with `pip install pandas numpy scikit-learn xgboost matplotlib seaborn folium requests`.
2. **Data**: Place the monthly `*divvy-tripdata.csv` files under `dataset/` (already included for this project).
3. **Weather cache** (optional): Keep `weather_cache_openmeteo/` to skip refetching. Delete it to re-download from Open-Meteo (mind API rate limits).
4. **Run notebook**: Open `1-vibing.ipynb` in Jupyter Lab/Notebook, execute cells in order. The HTML export provides a static reference if you only need the results.
5. **Exports**: Plots and model diagnostics are written to the `EDA*`, `Preprocessing`, and `Modelling` folders for use in reports.

## Next Steps
- Improve casual recall with rebalancing strategies (e.g., focal loss, SMOTE, or threshold tuning aligned with marketing costs).
- Incorporate station-level supply/demand features (dock availability, nearby amenities) for finer-grained targeting.
- Deploy the best model as a lightweight scoring service to flag high-conversion casual rides in near real time.
