# Regression Model for Weekly 1RM Change Prediction

## 1. Goal

Develop and evaluate a regression model to predict the weekly change in estimated 1-Rep Max (1RM) for a specified target exercise using historical structured workout data. The model should quantify the influence of training parameters on strength progression at a weekly level, enabling analysis of training efficacy and identification of plateaus.

## 2. Functional Requirements

- **Data Ingestion & Preprocessing:** Accept a structured DataFrame (`df`) with workout set logs, timestamps, exercise identification, weight, and repetitions.
- **Temporal Aggregation:** Aggregate set-level data into weekly periods based on workout timestamps.
- **Feature Engineering:**
  - Compute descriptive statistics and aggregated metrics for each week (e.g., total weekly volume, workout count, average session duration).
  - Derive the target variable: estimated 1RM change for the target exercise within each week (difference between earliest and latest 1RM).
  - Engineer additional features, including **lagged features** (total volume and average weight from the previous 1-2 weeks).
- **Data Partitioning:** Use a time-based split of weekly data into training and testing sets (training set precedes testing set).
- **Model Selection:** Use supervised regression algorithms (e.g., Linear Regression, Random Forest).
- **Model Training:** Train the regression model(s) on the training set.
- **Prediction:** Predict weekly 1RM changes for the test set.
- **Model Evaluation:** Assess performance using MAE, RMSE, and R-squared.
- **Feature Importance Analysis:** Quantify feature importance (especially for tree-based models).
- **Insight Generation:** Interpret results to understand drivers of 1RM progression and plateaus.

## 3. MVP Scope

- **Target Variable:** Weekly change in estimated 1RM for a single, configurable target exercise.
- **Time Granularity:** Weekly aggregation.
- **Input Features:** Weekly metrics (total volume, workout count, avg. duration), time-based features, and **lagged features** (total volume and avg. weight from previous 1-2 weeks).
- **Models:** At least Linear Regression and Random Forest Regressor.
- **Output:** Evaluation metrics, actual vs. predicted plots, and feature importance ranking.

## 4. Out of Scope (Future Iterations)

- Multi-exercise prediction.
- Advanced time series models (ARIMA, Prophet, LSTM).
- Automated plateau detection.
- Training recommendation engine.
- Real-time data pipeline or deployment.
- Integration with external data (sleep, nutrition).
- Web service or app deployment.

## 5. Data Input Schema

Assume `df` contains (after cleaning and feature creation):

- `title` (string): Workout session title
- `start_time` (datetime): Workout start timestamp
- `end_time` (datetime): Workout end timestamp
- `exercise_title` (string): Standardized exercise name
- `set_index` (int): Set index within exercise
- `weight_kg` (float): Weight used (kg)
- `reps` (float): Repetitions
- `set_volume` (float): `weight_kg * reps`
- `estimated_1rm` (float): Estimated 1RM (e.g., Epley formula)

## 6. Technical Steps

### 6.1 Data Transformation & Feature Engineering

- Create a weekly period identifier from `start_time`.
- Aggregate weekly features (total volume, workout count, avg. duration, etc.).
- For the target exercise, aggregate weekly stats (volume, sets, avg. reps, avg. weight).
- Compute weekly 1RM change for the target exercise (last - first 1RM in week).
- **Add lagged features:** For each week, include total volume and avg. weight from previous 1 and 2 weeks.
- Drop rows with missing target or lagged features.

### 6.2 Data Splitting

- Sort by week.
- Use a time-based split (e.g., 80% train, 20% test) to avoid data leakage.

### 6.3 Model Training

- Train Linear Regression and Random Forest Regressor on the training set.

### 6.4 Model Evaluation

- Predict on the test set.
- Compute MAE, RMSE, R-squared.
- Analyze feature importance (Random Forest) and coefficients (Linear Regression).
- Visualize actual vs. predicted 1RM change over time and as a scatter plot.

### 6.5 Interpretation & Insights

- Use evaluation metrics to assess predictive power.
- Use feature importance and coefficients to interpret which variables drive weekly 1RM change.
- Use predictions to identify potential plateaus and hypothesize interventions.

---

**Next Steps:**  
You will implement each step interactively. After each step, pause to ensure you understand the process and results before moving on. If you have questions or want to discuss any aspect in depth, let me know before proceeding to the next step.