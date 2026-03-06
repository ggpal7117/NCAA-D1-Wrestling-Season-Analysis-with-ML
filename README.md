# NCAA Division I Wrestling Season Analysis with Machine Learning

## TABLEAU DASHBOARD: https://public.tableau.com/app/profile/gourav.pal/viz/2025-25NCAAD1DualSeason/NCAAD12025-26Season

A full data science project analyzing the **2025–2026 NCAA Division I wrestling dual season** and using **machine learning models to predict match outcomes and conference tournament results**.

This project combines **web scraping, data engineering, feature engineering, visualization, and machine learning** to explore what drives success in college wrestling.

---

# Project Motivation

I wrestled in high school, and the sport taught me many life lessons and gave me lifelong friendships. As a fan of NCAA wrestling today, I’m fascinated by how competitive and unpredictable the sport can be.

This project began with a simple question:

> Can the patterns we observe in wrestling be captured in a statistical or machine learning model?

The goal was **not to "solve" the sport**, but to explore whether measurable statistics can help explain and predict outcomes in NCAA Division I wrestling.

---

# Project Overview

The project has two main components:

1. **Data Collection & Visualization**
   - Scrape match data from WrestleStat
   - Clean and structure the data
   - Explore the 2025–26 dual season using Tableau

2. **Machine Learning Prediction**
   - Engineer features from historical match data
   - Train models to predict match outcomes
   - Simulate predictions for conference tournaments

---

# Wrestling Background

### What is a Wrestling Match?

A match consists of two wrestlers competing head-to-head. Points are awarded for:

- Takedowns
- Escapes
- Reversals
- Turns
- Penalties

---

### What is a Dual Meet?

A **dual meet** is a team vs team format consisting of **10 matches**, one at each NCAA weight class.

The cumulative score of all matches determines the winning team.

---

### Bonus Wins

Some victories award **extra team points**, called **bonus wins**.

| Win Type | Description | Team Points |
|---|---|---|
| Decision | Normal win | 3 |
| Major Decision | Win by 8–14 points | 4 |
| Technical Fall | Win by 15+ points | 5 |
| Pin (Fall) | Opponent pinned | 6 |
| Forfeit / Default / DQ | Opponent cannot compete | 6 |

---

# Tech Stack

- **Python**
- **Jupyter Notebooks**
- **BeautifulSoup**
- **Regular Expressions**
- **Pandas / NumPy**
- **Scikit-learn**
- **XGBoost**
- **Tableau**
- **Excel (minimal formatting)**

---

# Data Collection

The data was scraped from **WrestleStat** using a custom Python scraping program.

The script was executed **weekly throughout the 2025-26 dual season**.

### Tools Used

- **BeautifulSoup**
  - Parses HTML structure
  - Extracts match information

- **Regex**
  - Cleans raw strings
  - Extracts numerical values
  - Formats scores and names

---

# Datasets

The project consists of several structured datasets:

### Duals
Team vs team dual meet results.

### Teams
Team identifiers and names.

### Wrestlers
Unique wrestler identifiers and names.

### Matches
Individual head-to-head match data.

---

# Data Engineering Challenges

Several issues arose during the data cleaning process.

## 1. Nicknames

Some wrestlers competed under **different names**.

Examples:

- Douglas Shipers vs Carter Shipers  
- JD Perez vs Jesse Perez  
- Josh Lange vs Joshua Lange  

These were actually **the same wrestlers**, but were assigned different IDs.

**Solution**

Standardize the names and merge IDs across datasets.

---

## 2. Duplicate Names

Three separate pairs of wrestlers had **identical names** but were:

- Different people
- Different teams
- Different weight classes

**Solution**

Use **team ID + wrestler name** to correctly distinguish identities.

---

## 3. Incorrect Team IDs

Some dual meets contained incorrect team IDs.

Example:

- Virginia vs Virginia Tech
- Central Michigan vs Michigan

**Solution**

Replace joins based on team ID with joins using:

- wrestler name
- weight class
- wrestler class

---

# Tableau Visualizations

The cleaned datasets were combined and visualized in **Tableau**.

Key insights explored:

### Season Overview
- Dual meet results
- Team performance

### Match Statistics
- Score distributions
- Most common match score (4-1)
- Median points scored per match

### Wrestler Performance
Grouped by:

- Weight class
- Win rate
- Bonus rate
- Pins
- Matches wrestled

### Team Dominance

One notable observation:

> **Penn State’s dominance during the dual season.**

---

# Machine Learning

Machine learning models were used to predict **match outcomes**.

### Dataset Size

- **Training matches:** 3974  
- **Validation matches:** 1154  

Matches with incomplete rolling statistics were removed.

---

# Feature Engineering

The most important feature engineering step was computing **rolling statistics**.

### Rolling Statistics

Rolling statistics use **only past information**.

Example:

| Match | Result |
|---|---|
| Match 1 | Win |
| Match 2 | Win |
| Match 3 | Loss |
| Match 4 | ? |

When predicting **Match 4**, the model only sees:

- Results from matches 1–3
- Historical statistics

This prevents **data leakage**.

---

# Data Leakage

Data leakage occurs when the model accidentally uses **future information**.

Example mistake:

Predicting a match while including statistics **that already contain the match result**.

This produces unrealistically high accuracy.

Rolling statistics prevent this by using **lagged historical data only**.

---

# Machine Learning Models

The following models were trained and evaluated.

### Logistic Regression

A classification model predicting the probability of a wrestler winning.

**Validation Accuracy**

~70%

Despite its simplicity, it performed surprisingly well.

---

### Decision Tree

A rule-based model that predicts outcomes through sequential decisions.

Example logic:



---

### XGBoost

An ensemble model that builds **multiple trees sequentially**, each correcting the mistakes of previous trees.

Initially, the model **overfitted heavily**.

Example:

| Model | Train Accuracy | Validation Accuracy |
|---|---|---|
| Initial XGBoost | 97.2% | 69.2% |
| Tuned XGBoost | 75.2% | 71.4% |

Hyperparameter tuning improved generalization.

---

# Inference Pipeline

A prediction pipeline was built to simulate **future matchups**.

Steps:

1. Build a **season summary database** of wrestler statistics
2. Generate **all possible matchups**
3. Compute head-to-head features
4. Run predictions using trained models

This pipeline was used to predict outcomes for:

- **ACC Tournament**
- **Ivy League Tournament**
- **Big Ten Tournament**

---

# Example Prediction Output

Example: **Big Ten Tournament Predictions**

| Weight | Logistic | Decision Tree | XGBoost | Agreement |
|---|---|---|---|---|
| 125 | Luke Lilledahl | Luke Lilledahl | Luke Lilledahl | High |
| 133 | Marcus Blaze | Marcus Blaze | Marcus Blaze | High |
| 149 | Shayne Van Ness | Shayne Van Ness | Shayne Van Ness | High |
| 174 | Levi Haines | Levi Haines | Levi Haines | High |

Higher agreement between models increases confidence.

---

# Verifying AI-Generated Code

Much of the scraping and feature engineering code was generated with AI assistance.

Tools used:

- Claude
- DeepSeek

To verify correctness:

- Manual test cases were created
- Rolling statistics were computed by hand and compared to code output
- Dataset joins were validated

This ensured correctness despite the pipeline's complexity.

---

# Limitations

### 1. Dual Matches Only

The model only uses **dual match data**, but conference tournaments have different dynamics.

Tournament data would improve predictions.

---

### 2. Limited Data

Although the dataset contains **5000+ matches**, wrestling outcomes are highly volatile.

More seasons of data would improve model stability.

---

# Future Improvements

Potential future extensions include:

- Store data in a **SQL database**
- Train **neural network models**
- Predict **type of victory**
  - Pin
  - Tech fall
  - Major decision
  - Decision
- Incorporate **tournament match data**
- Expand to **multiple seasons**

---

# Repository Structure

