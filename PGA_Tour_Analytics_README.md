# ⛳ PGA Tour Performance Analytics – Scoring Average Prediction

> **Identifying the strongest performance predictors of professional golf scoring using Multiple Linear Regression in R**

---

## 📌 Project Overview

What separates a great PGA Tour player from a good one? Is it driving distance, accuracy, putting, or scrambling ability? This project answers that question using statistical modeling on data from the **top 125 PGA Tour players by earnings in 2008** — analyzing which performance metrics are the strongest predictors of a player's scoring average, and building a robust regression model to predict it.

The project follows a rigorous analytical workflow: EDA → model building → variable selection → transformation → train/test validation → final model.

---

## 🎯 Objectives

- Understand which PGA Tour performance metrics most strongly predict scoring average
- Build a Multiple Linear Regression model with high explanatory power (R²)
- Apply proper statistical methodology: EDA, multicollinearity detection, variable selection, transformation, and validation
- Evaluate and compare multiple model specifications to identify the best-performing one

---

## 🛠️ Tech Stack

| Category | Tools |
|----------|-------|
| **Language** | R |
| **Packages** | `moderndive`, `ggplot2`, `dplyr`, `GGally`, `car`, `MASS`, `skimr` |
| **Statistical Methods** | Multiple Linear Regression, Log Transformation, Polynomial Transformation, 80/20 Train-Test Split |

---

## 📊 Dataset

- **Source**: PGA Tour 2008 season statistics
- **Observations**: 125 players (top earners by total prize money)
- **Target Variable**: `Scoring Average` — average number of strokes per completed round

| Variable | Description |
|----------|-------------|
| `Money` | Total earnings in PGA Tour events ($) |
| `DrDist` | Average driving distance (yards per measured drive) |
| `DrAccu` | Driving accuracy — % of tee shots landing in the fairway |
| `GIR` | Greens in Regulation — % of holes where the putting surface was reached in regulation |
| `Sand Saves` | % of greenside bunker attempts resulting in par or better |
| `PPR` | Putts Per Round — average number of putts per completed round |
| `Scrambling` | % of missed greens where the player still made par or better |
| `Bounce Back` | % of bogey holes followed by a birdie or better on the next hole |

---

## 🔬 Methodology

### Step 1 – Exploratory Data Analysis
- Computed summary statistics (mean, SD, min/max, quartiles) for all variables using `skim()`
- Built a correlation matrix to detect multicollinearity — found **GIR and PPR correlation > 0.73**, signaling potential multicollinearity risk in the model
- Created a scatter plot matrix (`scatterplotMatrix`) to visualize the relationship between Scoring Average and each predictor
- Visualized the `Money` variable using a histogram and boxplot — identified **right skew and high-value outliers**, motivating a log transformation

### Step 2 – Initial Regression Model (All Variables)
- Fit MLR using all 8 predictors (excluding Rank and Player Name)
- **R² = 0.84**, Adjusted R² = 0.829, F-statistic p-value < 2.2e-16
- Identified statistically insignificant variables (p > 0.05): `DrDist`, `DrAccu`, `Sand.Saves`, `Bounce_Back`

### Step 3 – Reduced Model (Significant Variables Only)
- Re-fit using only the significant predictors: `Money`, `GIR`, `PPR`, `Scrambling`
- **R² = 0.83** — nearly identical explanatory power with a more parsimonious model
- RMSE improved slightly: 0.1815 vs. 0.1845 in the full model

### Step 4 – Model Transformations (8 Models Tested)

| Model | Transformation Applied | R² | RMSE |
|-------|------------------------|-----|------|
| Mod 1 | All variables, no transformation | 0.8089 | 0.1845 |
| Mod 2 | Significant variables only | 0.8089 | 0.1815 |
| Mod 3 | Polynomial on GIR, Money, PPR, Scrambling (all vars) | 0.807 | 0.1855 |
| Mod 4 | Polynomial on significant variables only | 0.8067 | 0.1859 |
| Mod 5 | Log(Money) with significant variables | 0.8046 | 0.1751 |
| Mod 6 | Log(Scoring Avg) with all variables | 0.8087 | 0.0026 |
| Mod 7 | Log(Scoring Avg) + Log(Money) with all variables | 0.8061 | 0.0025 |
| **Mod 8** | **Log(Scoring Avg) + Log(Money) with significant variables only** | **0.8044** | **0.0025** |

### Step 5 – Train / Test Validation (80/20 Split)
- Split data: 80% training (~100 players), 20% test (~25 players) using `set.seed(123)`
- Trained Model 8 on the training set, evaluated predictions on the held-out test set
- **Final Test RMSE = 0.002467**, R² = 0.8044

### Step 6 – Final Model
Combined training and test data to build the final production model:

```r
log(Scoring.Average) = 4.25 - 0.002 × log(Money) - 0.002 × GIR - 0.001 × Scrambling + 0.008 × PPR
```

All four coefficients statistically significant at p < 0.001.

---

## 📈 Key Findings

- **GIR is the strongest negative predictor** of scoring average — hitting more greens in regulation dramatically lowers scores (coefficient = −0.002, t = −17.1). Every additional % point in GIR meaningfully reduces a player's average score.
- **PPR is the strongest positive predictor** — more putts per round directly inflates scoring average (coefficient = +0.008, t = +9.22). Putting efficiency is critical.
- **Scrambling matters** — players who recover effectively from missed greens consistently post lower scores, even when their ball striking is imperfect.
- **Driving distance and accuracy are NOT statistically significant predictors** — directly contradicting conventional golf wisdom. What happens on and around the green matters far more than how far or straight a player drives.
- **Money (earnings) as a skill proxy** — even after controlling for specific performance metrics, total earnings adds predictive power, capturing overall competitive quality not fully reflected in individual stats.
- **Log transformations were decisive** — Model 8's RMSE of 0.0025 represents a dramatic improvement over the untransformed Model 1 RMSE of 0.1845, confirming that non-linear relationships exist between predictors and scoring average.

---


---

## 📬 Contact

**Prateeksha Mehta** | pratu2930@gmail.com | [LinkedIn](https://linkedin.com/in/prateeksha29) | [GitHub](https://github.com/Prateeksha2930)
