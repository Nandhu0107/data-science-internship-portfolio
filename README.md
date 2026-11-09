<h1 align="center">Data Science Internship Portfolio</h1>

<p align="center">
  <img src="https://img.shields.io/badge/Python-3-3776AB?logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/scikit--learn-ML-F7931E?logo=scikitlearn&logoColor=white" alt="scikit-learn">
  <img src="https://img.shields.io/badge/XGBoost-Classifier-189FDD" alt="XGBoost">
  <img src="https://img.shields.io/badge/Jupyter-Notebooks-F37626?logo=jupyter&logoColor=white" alt="Jupyter">
</p>

<p align="center">
  Five machine learning exercises from the <b>Edufyi Data Science Internship</b>, covering classification, regression, preprocessing and clustering.
</p>

---

## Table of Contents

- [Projects at a Glance](#projects-at-a-glance)
- [Results Summary](#results-summary)
- [Project Details](#project-details)
- [Tech Stack](#tech-stack)
- [Installation](#installation)
- [Datasets](#datasets)
- [Author](#author)

---

## Projects at a Glance

| # | Project | Problem Type | Algorithms | Notebook |
| --- | --- | --- | --- | --- |
| 1 | Diabetes Prediction | Binary classification | XGBoost | [`Task_1.ipynb`](Task_1.ipynb) |
| 2 | Sales vs Advertising | Regression | Linear Regression | [`Task_2.ipynb`](Task_2.ipynb) |
| 3 | Titanic Data Preparation | Preprocessing and feature engineering | Label encoding, StandardScaler | [`Task_3.ipynb`](Task_3.ipynb) |
| 4 | Titanic Survival Prediction | Binary classification | Decision Tree, Linear Regression, combined model | [`Task_4.ipynb`](Task_4.ipynb) |
| 5 | Titanic Passenger Clustering | Unsupervised learning | PCA, K-Means | [`Task_5.ipynb`](Task_5.ipynb) |

Each task's brief is included as `Task_N.txt`.

## Results Summary

| Project | Model | Key Metric | Score |
| --- | --- | --- | ---: |
| Diabetes Prediction | XGBoost | Accuracy | **75.3%** |
| Titanic Survival | Linear Regression (0.5 cut-off) | Accuracy | **79.3%** |
| Titanic Survival | Decision Tree | Accuracy | 76–79% |
| Titanic Clustering | K-Means (k = 2) | Silhouette Score | **0.46** |
| Sales vs Advertising | Linear Regression | MAE, MSE, RMSE, R² | Computed in notebook |

> Scores come from re-running the notebooks on the standard public datasets (Pima Indians Diabetes and Kaggle Titanic, 891 passengers). The Decision Tree has no fixed random seed, so its score changes slightly between runs.

## Project Details

### 1. Diabetes Prediction (XGBoost)

**Goal:** predict whether a patient has diabetes from diagnostic measurements such as glucose, BMI, insulin and age.

**Approach**
- Removed duplicate rows and checked for missing values
- Split data 60 / 40 into training and test sets (`random_state=42`)
- Trained an `XGBClassifier`

**Evaluation (test set, 308 patients)**

| Metric | Score |
| --- | ---: |
| Accuracy | 75.3% |
| Precision (diabetic) | 0.61 |
| Recall (diabetic) | 0.72 |
| F1-Score (diabetic) | 0.66 |

| | Predicted: No | Predicted: Yes |
| --- | ---: | ---: |
| **Actual: No** | 159 | 47 |
| **Actual: Yes** | 29 | 73 |

**Per-class report**

| Class | Precision | Recall | F1-Score | Support |
| --- | ---: | ---: | ---: | ---: |
| Non-diabetic (0) | 0.85 | 0.77 | 0.81 | 206 |
| Diabetic (1) | 0.61 | 0.72 | 0.66 | 102 |

The model finds 72% of diabetic patients. Recall matters most here, because missing a diabetic patient is more costly than a false alarm.

---

### 2. Sales vs Advertising (Linear Regression)

**Goal:** predict sales from advertising spend.

**Approach**
- Split data 80 / 20 into training and test sets
- Trained a `LinearRegression` model and plotted the fitted line

**Evaluation metrics reported**

| Metric | Meaning |
| --- | --- |
| MAE | Average absolute error in predicted sales |
| MSE | Average squared error (penalises large errors) |
| RMSE | Square root of MSE, in the same units as sales |
| R² | Share of variation in sales explained by advertising |

> The dataset (`Data.xlsx`) is not public, so scores are not reproduced here. Run the notebook with the data to see them.

---

### 3. Titanic Data Preparation

**Goal:** prepare the Titanic dataset for machine learning.

**Steps**

| Step | Action |
| --- | --- |
| Missing values | Age filled with the median, Embarked with the most common value, Cabin dropped |
| Duplicates | Removed |
| New features | `Title` (from name), `FamilySize`, `IsAlone`, `AgeGroup`, `FareGroup` |
| Encoding | Label-encoded Sex, Embarked, Title, AgeGroup and FareGroup |
| Scaling | StandardScaler on Age, Fare and FamilySize |

**Output:** `Titanic_Cleaned_Dataset.csv`, 891 rows and 16 columns with no missing values, used by Tasks 4 and 5.

---

### 4. Titanic Survival Prediction

**Goal:** predict whether a passenger survived, and compare models.

**Approach**
- Split 80 / 20 (`random_state=42`, 179 test passengers)
- **Decision Tree** classifier
- **Linear Regression**, with predictions converted to survived / not survived at a 0.5 cut-off
- **Combined model**: Decision Tree and Linear Regression predictions added together, then cut at 0.5

**Evaluation (test set)**

| Model | Accuracy | Precision (survived) | Recall (survived) | F1 (survived) |
| --- | ---: | ---: | ---: | ---: |
| Linear Regression | **79.3%** | **0.78** | 0.70 | **0.74** |
| Decision Tree | 76.0% | 0.71 | 0.70 | 0.71 |
| Combined model | 76.0% | 0.68 | **0.78** | 0.73 |

The Decision Tree and combined model scores are from one run; across repeated runs they ranged from 76% to 79%. Linear Regression gives the best overall accuracy. The combined model finds more of the survivors (higher recall) at the cost of more false positives.

---

### 5. Titanic Passenger Clustering (K-Means + PCA)

**Goal:** group passengers by similarity without using survival labels, then check whether the groups match survival.

**Approach**
- Features: Sex, Age, Parch, Fare, SibSp, Pclass
- Scaled with StandardScaler and reduced to 2 components with PCA
- Chose the number of clusters with the elbow method, then ran K-Means with k = 2

**Evaluation**

| Metric | Score | Meaning |
| --- | ---: | --- |
| Silhouette Score | **0.46** | Clusters are reasonably well separated (range −1 to 1) |
| Agreement with actual survival | **70.5%** | Share of passengers whose cluster matches their survival outcome |

The clusters match actual survival 70.5% of the time without the model ever seeing the labels, which shows that class, sex and fare strongly separate passengers. Test-set cluster assignments are saved to `titanic_test_predicted_clusters.csv`.

## Tech Stack

| Category | Tools |
| --- | --- |
| Language | Python |
| Data handling | pandas, NumPy |
| Machine learning | scikit-learn, XGBoost |
| Visualisation | Matplotlib, Seaborn |
| Environment | Jupyter Notebook, Google Colab |

## Installation

```bash
git clone https://github.com/Nandhu0107/data-science-internship-portfolio.git
cd data-science-internship-portfolio
pip install -r requirements.txt
jupyter notebook
```

## Datasets

Datasets are not included. Download them and place them next to the notebooks:

| Notebook | Dataset | Source |
| --- | --- | --- |
| Task 1 | `diabetes.csv` | Pima Indians Diabetes Database (Kaggle / UCI) |
| Task 2 | `Data.xlsx` | Provided during the internship |
| Task 3 | `Titanic-Dataset.csv` | Kaggle Titanic dataset |
| Tasks 4 and 5 | `Titanic_Cleaned_Dataset.csv` | Created by Task 3 |

> The notebooks were written in Google Colab and read data from `/content/...`. Change these paths when running locally.

## Author

**Nandhagopal R**, MCA
