# Inventory Prediction

**Project for Interlub X Tecnológico de Monterrey Datathon 2025**

Authors: Miguel A. Perez, Mariana León Maldonado, Santiago Gutiérrez González

---

## Table of Contents
1.  [Overview](#overview)
2.  [Problem Statement](#problem-statement)
3.  [Dataset](#dataset)
4.  [Methodology](#methodology)
    *   [Data Cleaning & Preprocessing](#data-cleaning--preprocessing)
    *   [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)
    *   [Time Series Modeling](#time-series-modeling)
    *   [Inventory Optimization](#inventory-optimization)
5.  [Key Findings & Results](#key-findings--results)

---

## 1. Overview

This project presents a comprehensive methodology for predicting product demand and optimizing inventory levels for Interlub, a company specializing in industrial lubricants. Utilizing three years of historical sales order data, we developed a robust system that cleans and preprocesses data, evaluates multiple time series forecasting models, and implements an inventory optimization strategy. The AutoETS model was selected for its superior performance in handling high variability and demand spikes. The optimization strategy leverages confidence intervals and a custom loss function to recommend optimal stock levels, aiming to minimize costs and improve customer satisfaction.

---

## 2. Problem Statement

Efficient inventory management is crucial for minimizing operational costs, preventing stockouts, and maximizing customer satisfaction. Interlub sought a data-driven solution to:
*   Accurately forecast demand for its diverse range of products.
*   Optimize inventory levels to reduce holding costs and avoid lost sales due to stockouts.
*   Provide actionable recommendations for inventory replenishment.

---

## 3. Dataset

The dataset provided by Interlub consists of three years of historical sales order records. Key features utilized include:
*   **Order Creation Date:** Timestamp of the sales order.
*   **Item ID:** Unique identifier for each product (SKU).
*   **Quantity:** Quantity of the product sold in the order.
*   **Sales Unit:** Unit of measure (e.g., PZA, L, KG).

---

## 4. Methodology

Our approach involved several key stages:

### Data Cleaning & Preprocessing
*   **Null Value Handling:** Checked for and addressed missing values (none found in critical columns).
*   **Unit Normalization:** Standardized sales units (e.g., "PZ" to "PZA", "LTS" to "L") and imputed the most common unit per product.
*   **Data Type Conversion:** Converted date columns to `datetime` objects and quantity to numeric.
*   **Outlier Treatment:** Identified and removed outliers in `Cantidad` using the Interquartile Range (IQR) method.
*   **Data Aggregation:** Aggregated sales data daily and then weekly per product (SKU) to reduce noise and capture trends for forecasting.
*   **Product Filtering:** Focused on products with sufficient historical data (>= 200 observations) for reliable modeling.
*   **Time Series Completion:** Ensured a complete time series for each selected product by filling days/weeks with no sales with zero quantity.

### Exploratory Data Analysis (EDA)
Visualized sales trends, seasonality, and demand patterns for key products to inform model selection and feature engineering.

### Time Series Modeling
We evaluated a range of time series forecasting models to predict weekly demand for each SKU:
1.  **Baseline Models:**
    *   Linear Regression
    *   Polynomial Regression (Degrees 2 and 3)
2.  **Classical Time Series Models:**
    *   ARIMA (Autoregressive Integrated Moving Average)
3.  **Modern Time Series Models:**
    *   Prophet (Facebook's forecasting tool)
    *   LSTM (Long Short-Term Memory neural network)
    *   **AutoETS (Exponential Smoothing State Space Model):** Selected as the best-performing model.

Models were evaluated using Root Mean Squared Error (RMSE) through time-series cross-validation (5 folds). AutoETS demonstrated the best ability to handle the high variability and demand spikes present in the data.

### Inventory Optimization
An optimization strategy was developed based on the AutoETS predictions:
*   **Confidence Intervals:** Utilized 95% confidence intervals from AutoETS forecasts to estimate demand uncertainty and calculate safety stock.
*   **Loss Function:** Defined a loss function considering:
    *   **Cost of Storage:** Penalizing excess inventory.
    *   **Cost of Stockout:** Penalizing insufficient inventory (opportunity cost, loss of sales, reputation damage).
*   **Replenishment Lead Time:** Incorporated a 1-week lead time for new stock arrival.
*   **Service Level:** Targeted a 95% service level (probability of not having stockouts).
*   **Simulation:** Simulated weekly demand, inventory levels, and associated costs to generate recommended order quantities.

---

## 5. Key Findings & Results

*   The **AutoETS model** consistently outperformed other models in terms of RMSE, proving most effective for forecasting Interlub's product demand. (See `Model_Comparison.ipynb`.
*   The developed inventory optimization strategy provides practical, data-driven recommendations for weekly order quantities, aiming to maintain a 95% service level.
*   Implementation of this solution has the potential to significantly reduce inventory-related costs (both holding and stockout costs) and improve overall supply chain efficiency for Interlub.
*   Visualizations (e.g. plots in `Inventory_prediction.ipynb`) demonstrate the forecasting accuracy and the dynamics of the optimization model.

![image](https://github.com/user-attachments/assets/62526d33-6219-4bb6-ada6-943a1d798bbf)

