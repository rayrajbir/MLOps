# Data Pipeline with DVC and MLflow for Machine Learning

## Overview
This project demonstrates how to build an end-to-end machine learning pipeline using **DVC (Data Version Control)** for data and model versioning and **MLflow** for experiment tracking. The pipeline focuses on training a **Random Forest Classifier** on the **Pima Indians Diabetes Dataset**, with structured stages for **data preprocessing, model training, and evaluation**.

## Key Features

### **Data Version Control (DVC):**
- Tracks and versions datasets, models, and pipeline stages to ensure reproducibility.
- Automates pipeline execution when dependencies (data, scripts, parameters) change.
- Supports remote data storage (e.g., **DagsHub, S3**) for handling large datasets and models.

### **Experiment Tracking with MLflow:**
- Logs **hyperparameters** (e.g., `n_estimators`, `max_depth`) and **performance metrics** like accuracy.
- Tracks different model runs, making it easy to compare and optimize the pipeline.
- Stores experiment artifacts for reproducibility.

## **Pipeline Stages**

### **1. Preprocessing:**
- The `preprocess.py` script reads raw data (`data/raw/data.csv`), processes it (e.g., renaming columns), and saves it to `data/processed/data.csv`.
- Ensures consistent preprocessing across runs.

### **2. Training:**
- The `train.py` script trains a **Random Forest Classifier** on the preprocessed data.
- The trained model is saved as `models/random_forest.pkl`.
- Hyperparameters and model metrics are logged in MLflow.

### **3. Evaluation:**
- The `evaluate.py` script loads the trained model and assesses its performance.
- Accuracy and other evaluation metrics are logged to MLflow.

## **Goals**
- **Reproducibility:** DVC ensures that the same dataset, parameters, and code yield consistent results.
- **Experimentation:** MLflow helps track experiments with varying hyperparameters for optimization.
- **Collaboration:** DVC and MLflow enable seamless teamwork, ensuring everyone works with the latest data and model versions.

## **Use Cases**
- **Data Science Teams:** Track datasets, models, and experiments in an organized and reproducible manner.
- **Machine Learning Research:** Quickly iterate over different experiments, manage data versions, and evaluate results.

## **Technology Stack**
- **Python**: Core language for data processing, model training, and evaluation.
- **DVC**: For data and model versioning.
- **MLflow**: For logging and tracking experiments.
- **Scikit-learn**: For training the **Random Forest Classifier**.

---

## **How to Add Pipeline Stages with DVC**

### **Preprocessing Stage**
```sh
dvc stage add -n preprocess \
    -p preprocess.input,preprocess.output \
    -d src/preprocess.py -d data/raw/data.csv \
    -o data/processed/data.csv \
    python src/preprocess.py
```

### **Training Stage**
```sh
dvc stage add -n train \
    -p train.data,train.model,train.random_state,train.n_estimators,train.max_depth \
    -d src/train.py -d data/raw/data.csv \
    -o models/model.pkl \
    python src/train.py
```

### **Evaluation Stage**
```sh
dvc stage add -n evaluate \
    -d src/evaluate.py -d models/model.pkl -d data/raw/data.csv \
    python src/evaluate.py
```

---

## **How to Run the Pipeline**
1. **Clone the Repository:**
   ```sh
   git clone <repo_url>
   cd <repo_name>
   ```

2. **Install Dependencies:**
   ```sh
   pip install -r requirements.txt
   ```

3. **Initialize DVC:**
   ```sh
   dvc init
   dvc remote add origin <remote_url>
   ```

4. **Run the Pipeline:**
   ```sh
   dvc repro
   ```

5. **Track and Compare Experiments in MLflow:**
   ```sh
   mlflow ui
   ```
   Open `http://localhost:5000` in your browser to visualize and compare runs.

---

## **Conclusion**
This project showcases best practices for managing an ML pipeline using **DVC** and **MLflow**, ensuring version control, experiment tracking, and reproducibility. Whether working solo or in a team, this setup enables efficient and structured model development.

