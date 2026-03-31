# 🔥 Heat Check ECLIPSE: Quantifying the Hot Hand in the NBA

![Python](https://img.shields.io/badge/Python-3.12-blue?style=flat-square&logo=python)
![XGBoost](https://img.shields.io/badge/XGBoost-Enabled-orange?style=flat-square&logo=xgboost)
![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-Machine%20Learning-F7931E?style=flat-square&logo=scikit-learn)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458?style=flat-square&logo=pandas)
![Data](https://img.shields.io/badge/Data-NBA_API-green?style=flat-square)

> *Some of basketball's greatest moments emerge from incredible shooting sequences—often termed the "Heat Check." This project rigorously investigates whether the "hot hand" phenomenon provides a measurable, statistical advantage in the NBA, or if it is merely a cognitive illusion fueled by volume.*

---

## 📖 Table of Contents
1. [Project Overview](#-project-overview)
2. [Data Architecture](#-data-architecture)
3. [Methodology & Feature Engineering](#-methodology--feature-engineering)
4. [Exploratory Data Analysis (EDA)](#-exploratory-data-analysis-eda)
5. [Predictive Modeling](#-predictive-modeling)
6. [Key Findings & Conclusion](#-key-findings--conclusion)
7. [Installation & Setup](#-installation--setup)
8. [Authors](#-authors)

---

## 🎯 Project Overview
In basketball culture, a **"Heat Check"** is a high-difficulty shot taken by a player who has recently made several subsequent shots, attempting to gauge if their elite shooting efficiency will persist. 

**Our Scientific Definition of a Heat Check:**
- The shot is taken **at least 15 feet** from the basket.
- The player's **previous two shots** were both made (`prev_1_make = 1`, `prev_2_make = 1`).
- Both makes occurred within the same game and within a **3-minute window**.

**Core Objectives:**
- Determine if post-streak confidence leads to statistically significant changes in shot selection risk.
- Assess if "heat" genuinely improves shooting efficiency or merely correlates with higher shot volume.
- Build a robust predictive framework to forecast shot success utilizing spatiotemporal data and player momentum indicators.

---

## 📊 Data Architecture
We curated an expansive, highly-granular dataset encompassing over **100,000 shots** from the NBA's Top 15 Scorers spanning the 2019-2020 through 2023-2024 seasons.

- **Sources:** `nba_api` and `nba_on_court` packages.
- **Features Captured:** Exact court coordinates (`LOC_X`, `LOC_Y`), shot distance, absolute game time elapsed, and period mechanics.
- **Pipeline:** Custom requests securely fetch Top 50 scorer IDs via league leaders, then sequentially download and concatenate comprehensive shot charts, handling API throttling and timeouts safely.

---

## 🛠 Methodology & Feature Engineering
To elevate predictive power beyond basic geographical data, we engineered deterministic and contextual features:
- **Momentum Indicators:** Extracted localized streaks (`prev_1_make`, `prev_2_make`).
- **Spatiotemporal Context:** Computed `game_seconds_elapsed` to normalize shot timing across all periods (including OT).
- **Interaction Terms:** Generated features like `SHOT_DISTANCE_x_IS_HEAT_CHECK` and `PERIOD_x_IS_HEAT_CHECK` to capture the multiplicative effects of fatigue, distance, and confidence.

---

## 📈 Exploratory Data Analysis (EDA)
Our visual analysis pipeline uncovered profound insights into how elite scorers operate during a hot streak. Key visualizations generated include:
- **Hexbin Spatial Maps:** Identifying high-frequency shooting zones (e.g., Stephen Curry, Luka Dončić, and Zach LaVine's optimal heat check spots).
- **Shot Distance Distributions (KDE):** Comparing the density of regular shots versus heat checks to confirm that players tend to shoot from further away and with higher variance when "hot."
- **Efficiency vs. Minutes Remaining:** Time-decay line graphs demonstrating how shooting accuracy fluctuates as the quarter progresses.
- **Volume Trajectories:** Highlighting the total volume vs. makes and misses during designated heat check scenarios.

---

## 🧠 Predictive Modeling
We established a robust machine learning pipeline to predict the binary outcome of a shot (`SHOT_MADE_FLAG`). Since missed shots naturally outweigh makes (class imbalance), we incorporated advanced resampling techniques.

### Models Evaluated
1. **XGBoost Classifier (Primary Model)**
   - Utilized as the flagship model, achieving stable ROC-AUC and interpretability.
   - Further tuned using `GridSearchCV` to optimize `max_depth`, `learning_rate`, and `n_estimators`.
2. **Random Forest Classifier**
   - Captured non-linear spatial boundaries well but slightly underperformed XGBoost in handling sparse interaction features.
3. **Logistic Regression**
   - Served as our linear baseline, solved via `liblinear` with L1/L2 regularization to baseline linear feature independence.

### Data Balancing (SMOTE)
We utilized **Synthetic Minority Over-sampling Technique (SMOTE)** on the training data to perfectly balance the binary classes (16,370 Makes to 16,370 Misses). This drastically improved the model's recall on predicting successful shots.

### Feature Importance
Across all tree-based algorithms, the most predictive indicators were:
1. `prev_1_make`
2. `prev_2_make`
3. `is_heat_check`
4. `SHOT_DISTANCE`

---

## 🏆 Key Findings & Conclusion
- **The Hot Hand Exists (Conditionally):** A player's immediate momentum (`prev_1_make`, `prev_2_make`) is the strongest statistical predictor of their next shot's success, empirically validating the "heat check" to a measurable degree.
- **Risk vs. Reward:** While efficiency occasionally drops on the heat check itself (due to heightened shot difficulty and deeper distances), the threat of the hot hand forces defensive adjustments, creating an unquantifiable gravitational advantage on the court.
- **Variance Rules:** Basketball inherently involves chaotic human variations (defensive pressure, fatigue, off-ball movement). While our model successfully captures the signal of momentum, the noise of external factors preserves the game's unpredictability.

---

## 🚀 Installation & Setup

1. **Clone the Repository:**
   ```bash
   git clone https://github.com/prs-016/Hot_Streaks_ECLIPSE.git
   cd Hot_Streaks_ECLIPSE
   ```

2. **Install Dependencies:**
   Ensure you have Python 3.10+ installed, then run:
   ```bash
   pip install -r requirements.txt
   ```
   *(Or manually install: `numpy`, `pandas`, `matplotlib`, `scikit-learn`, `statsmodels`, `seaborn`, `pbpstats`, `nba_on_court`, `xgboost`, `imblearn`)*

3. **Data Acquisition:**
   Upload or ensure `nba_shot_data.csv` is present in the working directory (or linked via Google Drive if running on Colab).

4. **Run the Analysis:**
   Execute the core notebook linearly:
   ```bash
   jupyter notebook HeatCheckAlgorithm.ipynb
   ```

---

## 👥 Authors
- **Prakhar Shah**
- **Anay Takkapalli**
- **Alex Dai**
- **Nikhil Ganpule**

*Built for ECLIPSE.*