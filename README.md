# Fault Point Labeling and Fault Detection in Time Series Data

### A project focused on detecting fault points in real-time within time series data from heat treatment processes using machine learning techniques.

## Table of Contents
1. [Problem Definition](#problem-definition)
2. [Data Description](#data-description)
3. [Data Preprocessing](#data-preprocessing)
4. [Modeling and Prediction](#modeling-and-prediction)
5. [Evaluation and Analysis](#evaluation-and-analysis)

## Problem Definition

### 1. Background
Heat treatment is a critical process in the metal industry, with many companies running 24-hour operations to save on energy costs. However, traditional heat treatment lines lack real-time monitoring and quality control, leading to post-production defects and increased costs. This project aims to address these issues by developing a model that can detect faults in real-time from time series data.

### 2. Objectives
- Develop a model to label and detect defects in the heat treatment process in real time.
- Provide a generalized solution that works across varying data distributions.
- Use explainable AI techniques to identify root causes of defects and improve the production process.

## Data Description

- **data.csv**: Process data collected at 1-second intervals.
  - Shape: (2,939,722, 21)
  - Key Variables:
    - `TAG_MIN`: Timestamp of data collection.
    - `배정번호`: Work order number.
    - `건조 1존~2존 OP`: Output percentage to maintain temperature.
    - `소입로 온도 1~4 Zone`: Inlet zone temperatures.

- **quality.csv**: Quality data for each work order.
  - Shape: (136, 7)
  - Key Variables:
    - `배정번호`: Work order number.
    - `양품수량`: Quantity of good products.
    - `불량수량`: Quantity of defective products.
    - `총수량`: Total quantity produced.

- **train.csv**: Summary statistics and labels for the heat treatment process.
  - Shape: (2,939,722, 21)

## Data Preprocessing
### 1. Handling Missing Values
- Linear interpolation was applied to maintain the time series continuity.
  - The column `Inlet 1 Zone OP` had the most missing values with a total of 6,043 missing entries.
  
### 2. Visualization
Various visualization techniques were used to explore the dataset and understand the distribution and relationships among variables:
  - **Histogram**: Distribution of each variable
    ![Histogram](https://github.com/jeewonkimm2/Fault_Point_Labeling_and_Fault_Detection_in_Time_Series_Data/assets/108987773/120f90d2-523e-4d84-bbd2-69a7f7b994bc)

  - **Box Plot**: Statistical insights such as median, quartiles, and outliers
    ![Box Plot](https://github.com/jeewonkimm2/Fault_Point_Labeling_and_Fault_Detection_in_Time_Series_Data/assets/108987773/b8211ab9-1279-4da9-aa82-f8cc9afd2d3b)

  - **Line Graph**: Trends over time by assignment number
    ![Line Graph](https://github.com/jeewonkimm2/Fault_Point_Labeling_and_Fault_Detection_in_Time_Series_Data/assets/108987773/9eeddcd9-fbbe-41f4-9068-f80a4993a74a)

  - **Correlation Matrix**: Analyzing relationships between variables
    ![Correlation Matrix](https://github.com/jeewonkimm2/Fault_Point_Labeling_and_Fault_Detection_in_Time_Series_Data/assets/108987773/4585a396-f213-47b4-8733-75b538041f43)

### 3. Data Preprocessing
- Highly correlated variables were removed to improve model performance.

## Modeling and Prediction
- **Model**: XGBoost Classifier
- **Process**:
  1. Data split into training, validation, and test sets.
  2. Hyperparameter tuning for optimal model performance.
  3. Evaluating model performance using the optimal threshold.

## Evaluation and Analysis
### 1. Evaluation
The XGBoost model was evaluated using standard classification metrics. Below are some evaluation metrics:
  ![Evaluation](https://github.com/ShawnSon-hub/Fault-Point-Labeling-and-Fault-Detection-in-Time-Series-Data/assets/124177883/76cffe8b-b58c-48ef-ae71-66067be2f028)

### 2. Analysis
  - **Feature Importance**: Analysis was conducted to determine which features had the greatest influence on model predictions.
    ![Feature Importance](https://github.com/ShawnSon-hub/Fault-Point-Labeling-and-Fault-Detection-in-Time-Series-Data/assets/124177883/6ebe2334-a671-49fb-adcd-1cd4e725ab9c)
  
  - **SHAP Summary Plot**: SHAP (SHapley Additive exPlanations) was used to explain the impact of features on the model’s predictions.
    ![SHAP Summary Plot](https://github.com/ShawnSon-hub/Fault-Point-Labeling-and-Fault-Detection-in-Time-Series-Data/assets/124177883/e5746786-23cd-4ac5-b532-40df3fdddf6f)
  
  - **SHAP Dependence Plot**: Explores how a feature affects the model's predictions across the dataset.
    ![SHAP Dependence Plot](https://github.com/ShawnSon-hub/Fault-Point-Labeling-and-Fault-Detection-in-Time-Series-Data/assets/124177883/431ecb4a-f858-4fc9-b7a2-26678812bb13)
