Uber-Demand-Prediction
==============================

**A machine learning pipeline to forecast Uber pickup demand at 15-minute intervals across New York City using spatiotemporal clustering and advanced CI/CD deployment.**

---

## 📌 Table of Contents

* [Project Overview](#project-overview)
* [Key Highlights](#key-highlights)
* [Business Objective](#business-objective)
* [Problem Statement](#problem-statement)
* [Data Description](#data-description)
* [Project Architecture](#project-architecture)
* [Modeling Approach](#modeling-approach)
* [Model Performance](#model-performance)
* [Deployment Strategy](#deployment-strategy)
* [Tech Stack](#tech-stack)
* [Setup Instructions](#setup-instructions)
* [Potential Improvements](#potential-improvements)
* [License](#license)

---

## 🚀 Project Overview

This project develops a predictive system to estimate Uber pickup demand across various zones in **New York City** at 15-minute granularity. It utilizes historical pickup data, spatial clustering with **MiniBatchKMeans**, time-series smoothing, and a **Linear Regression** model to forecast regional demand with high temporal accuracy. The solution is fully automated and production-ready, using **GitHub Actions** for CI/CD and **Blue-Green deployment** via **AWS Auto Scaling Groups**.

---

## ✨ Key Highlights

* Forecasts Uber pickup demand every **15 minutes** for **dynamically clustered zones** across NYC.
* Clustering of geo-coordinates using **MiniBatchKMeans** for regional segmentation.
* Applied **Exponentially Weighted Moving Average (EWMA)** to denoise time-series signals and stabilize input data.
* Achieved **7.93% MAPE** on test data with a lightweight **Linear Regression model**, demonstrating high accuracy with low complexity.
* Deployed using a fully automated **CI/CD pipeline** on AWS with **Blue-Green deployment** strategy for zero-downtime releases.
* Integrated **GitHub Actions** for seamless version control, testing, and deployment triggers.

---

## 🎯 Business Objective

Uber's dynamic marketplace thrives on proactive resource allocation. This project aims to empower Uber with a demand forecasting system that:

* Improves driver-to-rider matching accuracy
* Reduces wait times during peak hours
* Optimizes supply-side deployment and incentives
* Supports operational decisions for city-wide logistics

---

## ❓ Problem Statement

How can we accurately forecast Uber pickup demand across **multiple NYC sub-regions** at **15-minute intervals**, using historical trends and spatial data? The solution must be:

* Scalable across urban regions
* Efficient for real-time inference
* Deployable in production environments
* Adaptable to fluctuating temporal patterns

---

## 🔮 Data Description

* **Source**: Uber pickup data (aggregated historical rides from NYC)
* **Features**:

  * Pickup timestamps
  * Latitude & Longitude
  * Aggregated ride counts per region & time bucket
* **Preprocessing**:

  * Timestamp rounding to 15-min intervals
  * Spatial clustering using MiniBatchKMeans
  * EWMA smoothing applied to time-series pickup counts

---

## 🧠 Project Architecture
------------

    ├── LICENSE
    ├── Makefile           <- Makefile with commands like `make data` or `make train`
    ├── README.md          <- The top-level README for developers using this project.
    ├── data
    │   ├── external       <- Data from third party sources.
    │   ├── interim        <- Intermediate data that has been transformed.
    │   ├── processed      <- The final, canonical data sets for modeling.
    │   └── raw            <- The original, immutable data dump.
    │
    ├── docs               <- A default Sphinx project; see sphinx-doc.org for details
    │
    ├── models             <- Trained and serialized models, model predictions, or model summaries
    │
    ├── notebooks          <- Jupyter notebooks. Naming convention is a number (for ordering),
    │                         the creator's initials, and a short `-` delimited description, e.g.
    │                         `1.0-jqp-initial-data-exploration`.
    │
    ├── references         <- Data dictionaries, manuals, and all other explanatory materials.
    │
    ├── reports            <- Generated analysis as HTML, PDF, LaTeX, etc.
    │   └── figures        <- Generated graphics and figures to be used in reporting
    │
    ├── requirements.txt   <- The requirements file for reproducing the analysis environment, e.g.
    │                         generated with `pip freeze > requirements.txt`
    │
    ├── setup.py           <- makes project pip installable (pip install -e .) so src can be imported
    ├── src                <- Source code for use in this project.
    │   ├── __init__.py    <- Makes src a Python module
    │   │
    │   ├── data           <- Scripts to download or generate data
    │   │   └── make_dataset.py
    │   │
    │   ├── features       <- Scripts to turn raw data into features for modeling
    │   │   └── build_features.py
    │   │
    │   ├── models         <- Scripts to train models and then use trained models to make
    │   │   │                 predictions
    │   │   ├── predict_model.py
    │   │   └── train_model.py
    │   │
    │   └── visualization  <- Scripts to create exploratory and results oriented visualizations
    │       └── visualize.py
    │
    └── tox.ini            <- tox file with settings for running tox; see tox.readthedocs.io


--------

<p><small>Project based on the <a target="_blank" href="https://drivendata.github.io/cookiecutter-data-science/">cookiecutter data science project template</a>. #cookiecutterdatascience</small></p>


---

## ⚙️ Modeling Approach

* **Clustering**: Used **MiniBatchKMeans** to segment NYC into zones with high geographical coherence.
* **Smoothing**: Applied **EWMA (α = 0.3)** to reduce variance in pickup trends while retaining signal responsiveness.
* **Regression**: Trained a **Linear Regression** model per zone with lag features (prior intervals) to forecast demand.
* **Validation**: Employed time-based cross-validation to simulate forward-looking generalization.

---

## 📊 Model Performance

| Metric      | Value                 |
| ----------- | --------------------- |
| MAPE (Test) | **7.93%**             |
| MAE         | Low, stable           |
| R² Score    | High (Zone-dependent) |

> The model exhibited strong generalization across diverse regions and peak traffic intervals, making it suitable for production deployment.

---

## 🚀 Deployment Strategy

* **Containerized** the model using **Docker** for cloud-native scalability.
* Implemented **CI/CD pipeline** via **GitHub Actions** for:

  * Automated testing
  * Model packaging
  * Deployment triggering
* **Blue-Green Deployment** via **AWS CodeDeploy + Auto Scaling Group** ensures:

  * Zero downtime
  * Easy rollback
  * Seamless user experience during rollout

---

## 🧰 Tech Stack

* **Languages**: Python, Bash
* **ML Libraries**: scikit-learn, pandas, NumPy, matplotlib
* **CI/CD**: GitHub Actions, AWS CodeDeploy
* **Deployment**: Docker, EC2, Auto Scaling Group (Blue-Green)
* **Visualization**: Seaborn, Plotly
* **Infrastructure as Code**: YAML, AWS CLI

---

## 🛠️ Setup Instructions

### 1. Clone the repository

```bash
git clone https://github.com/your-username/uber-pickup-demand-prediction.git
cd uber-pickup-demand-prediction
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Run training pipeline

```bash
python src/train_model.py
```

### 4. Build Docker image

```bash
docker build -t uber-demand-predictor .
```

### 5. Trigger deployment (CI/CD)

Push to `main` branch. GitHub Actions will:

* Run tests
* Build and tag Docker image
* Push image to ECR (if configured)
* Trigger Blue-Green deployment on AWS Auto Scaling Group

---

## 🔮 Potential Improvements

* Replace Linear Regression with **Gradient Boosting** or **Temporal CNNs**
* Implement **Zone-to-Zone interaction modeling** (spatial spillover effects)
* Integrate **real-time data pipelines** via Kafka or AWS Kinesis
* Monitor and alert for **model drift** using AWS CloudWatch & Lambda

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).

