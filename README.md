# NBA Shot Outcome Prediction

## Overview

This project develops a machine learning model to predict the probability
that an NBA shot attempt is made based on shot characteristics, court
location, defensive pressure, and game context.

The project explores the complete modeling process, including exploratory
data analysis, feature engineering, logistic regression, gradient boosting,
hyperparameter tuning, and model evaluation.

The final XGBoost model achieved a validation log-loss of **0.6269**,
improving upon a baseline logistic regression log-loss of **0.6448**.

## Project Goals

The primary goals of this project were to:

- Explore factors associated with NBA shot success
- Identify useful offensive and defensive predictors
- Engineer features from player-tracking data
- Establish a logistic regression baseline
- Model nonlinear relationships using XGBoost
- Optimize predictive performance using validation log-loss

## Exploratory Data Analysis

The training data contained NBA shot attempts with a binary outcome
indicating whether each shot was made or missed.

Approximately **46% of shots were made and 54% were missed**.

Several relationships were explored during EDA.

### Shot Distance

Shot success generally decreased as distance from the basket increased.
The relationship also appeared nonlinear, suggesting that distance alone
would not be adequately represented by a simple linear effect.

### Shot Type

Different shot types showed substantially different success rates. Dunks,
for example, had a considerably higher observed make rate than jump shots
and other shot types.

### Defensive Pressure

Defender distance and the number of contesting defenders were investigated
as measures of defensive pressure.

### Court Location

Shot outcomes were also examined spatially using X and Y court coordinates.
Shooting efficiency varied across the court, providing additional evidence
that nonlinear relationships were present in the data.

## Feature Engineering

An **80/20 training-validation split** with a fixed random seed was used
throughout model development.

Features considered included:

- Shot distance
- Two-point vs. three-point attempt
- Shot type
- Closest defender distance
- Number of contesting defenders
- Game state
- Shot clock
- Dribbles before the shot
- Shooter speed
- X and Y court coordinates

I also engineered a **defender approach** feature using changes in defender
distance shortly before the shot attempt. Negative values represented a
defender moving closer to the shooter.

Although this feature provided an interesting measure of defensive movement,
it did not meaningfully improve validation performance and was excluded from
the final model.

## Model Development

### Logistic Regression

A logistic regression model was first developed as a baseline.

| Model | Validation Log-Loss |
|---|---:|
| Baseline Logistic Regression | 0.64475 |
| Nonlinear Logistic Regression | 0.64245 |
| Nonlinear Logistic + Contest Information | ~0.64033 |

Adding nonlinear relationships and defensive information improved upon the
initial baseline, but the largest improvement came from moving to a
tree-based model.

### XGBoost

XGBoost was selected to better capture nonlinear relationships and
interactions among shot, location, and defensive variables.

The initial XGBoost model achieved a validation log-loss of approximately
**0.6278**.

Tree depths of 3, 4, 5, and 6 were evaluated, with a maximum depth of
**6** producing the best validation performance.

Additional defender-distance variables were then incorporated, further
improving validation log-loss to approximately **0.62718**.

## Hyperparameter Tuning

The final tuning process evaluated combinations of:

- Learning rate (`eta`)
- Minimum child weight
- Number of boosting iterations

The best-performing configuration was:

| Parameter | Value |
|---|---:|
| Maximum tree depth | 6 |
| Learning rate (`eta`) | 0.03 |
| Minimum child weight | 10 |
| Boosting iterations | 359 |
| Validation log-loss | **0.6269139** |

Overall, model development reduced validation log-loss from **0.6447523**
for the baseline logistic regression to **0.6269139** for the final
XGBoost model.

## Model Interpretation

Feature importance from the XGBoost model indicated that **shot distance**
was the dominant predictor.

Other important predictors included:

- Closest defender distance
- Number of contesting defenders
- Shot type
- Shooter speed
- Shot clock
- Court location

The tree-based model was particularly useful because it could capture
nonlinear relationships and interactions between these variables.

## Challenges

One challenge involved the defensive tracking variables, which contained
substantial missing data. Rather than removing observations with missing
defender measurements, the modeling workflow preserved these observations
and allowed XGBoost to handle the missing values.

Another challenge involved converting multiple defender-distance
measurements into a useful representation of defender movement. This led
to the defender-approach feature, which was evaluated but ultimately
excluded because it did not materially improve predictive performance.

## Limitations and Future Work

Hyperparameter tuning was intentionally limited to a targeted set of
parameters rather than an exhaustive search.

Future improvements could include:

- More systematic hyperparameter optimization
- Additional feature engineering
- Cross-validation
- More detailed model interpretation
- Investigation of nonlinear feature interactions
- Comparison with additional machine learning algorithms

## Technologies

- R
- XGBoost
- Logistic Regression
- Data Visualization
- Feature Engineering
- Predictive Modeling

## Repository Contents

- `project_code.Rmd` — exploratory analysis, feature engineering, model
  development, tuning, and prediction workflow
- `project_writeup.pdf` — detailed explanation of the analysis and modeling
  process
- `ai_prompts.txt` — documentation of AI assistance used during development

## Results

The final model achieved a validation log-loss of **0.6269139**, compared
with **0.6447523** for the baseline logistic regression.

The project demonstrates how exploratory analysis, feature engineering,
and nonlinear machine learning methods can improve prediction of NBA
shot outcomes.