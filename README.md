# 🏡 Real Estate Price Prediction Pipeline with Apache Spark

End-to-end data pipeline for real estate price prediction using Apache Spark and PySpark, covering data engineering, feature transformation, and machine learning regression models applied to structured housing data from Rio de Janeiro.

---

## 🎯 Business Problem

The goal is to build a scalable data pipeline capable of processing raw JSON housing data and predicting apartment prices based on structural features such as:

- living area
- number of rooms
- bathrooms
- parking spaces
- condominium fees
- property tax (IPTU)

The solution simulates a real-world big data environment using Spark.

---

## ⚙️ Data Pipeline Architecture

### 1. Data Ingestion
- Parsing complex nested JSON structures (`struct`)
- Flattening hierarchical fields for analytical processing

### 2. Data Cleaning & Filtering
- Removal of irrelevant or commercial noise data using `.drop()`
- Handling missing values and inconsistent formats

### 3. Feature Engineering
- Type casting (`Integer`, `Double`)
- One-Hot Encoding for categorical variables
- Feature consolidation using `VectorAssembler`

---

## 🤖 Machine Learning Models

Two regression models were evaluated using Spark MLlib:

- Decision Tree Regressor
- Random Forest Regressor (Ensemble Method)

### 📊 Evaluation Metrics
- R² (Coefficient of Determination)
- RMSE (Root Mean Squared Error)
- Cross-validation (K-Fold)

### 🏆 Result
The final model achieved:

- **R² > 0.77**, explaining a significant portion of price variability in the dataset

---

## 🧪 Engineering & Local Infrastructure

This project was executed in a fully local Spark environment, simulating production-level constraints:

- Java 11 configuration (`JAVA_HOME`)
- Hadoop native binaries (`winutils.exe`)
- Local Spark execution on `localhost`
- Windows environment adaptation for distributed processing simulation

A workaround was implemented to bypass serialization conflicts between PySpark and Python 3.12 by exporting intermediate data as JSON and reloading via Spark native reader.

---

## 🚀 How to Run

```bash
# Activate environment
.\.venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Run notebook
jupyter notebook 01_regressao.ipynb
