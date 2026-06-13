# Weekly Demand Forecasting for Top-10 Products

## 📌 Executive Summary
This project presents a SQL-first, reproducible approach to weekly demand forecasting for the Top-10 products by sales volume. Using 26 weeks of transactional data, a complete weekly demand series (including zero-demand weeks) was constructed. A naive last-week forecast was benchmarked against a rolling 3-week average, evaluating both using Mean Absolute Error (MAE).

**Key Result:**  
Across all Top-10 products, a rolling 3-week average consistently reduced forecast error compared to a naive baseline, achieving an average MAE reduction of approximately **15–25%**. Given the limited historical depth and sparse demand patterns, the rolling baseline is recommended for production use, while machine learning models are deferred until additional data is available.

## 🏢 Business Problem
Accurate weekly demand forecasts are crucial to support inventory and replenishment decisions for high-volume products. Poor forecasts often lead to:
- **Overstocking:** Increasing holding costs and tying up capital.
- **Stockouts:** Resulting in lost sales and customer dissatisfaction.

**Objective:** Identify a forecasting approach that minimizes forecast error while remaining interpretable and robust under real-world data constraints.

### Limitations & Constraints
- Only **26 weeks** of historical data are available.
- Weekly demand is **sparse and volatile**.
- No promotional, pricing, or seasonality features are available.
- Forecasts must be explainable and operationally simple.

## 🛠️ Data Sources & Tooling

### Data Files
- `transactions.csv`: Order-level transaction records.
- `order_items_shopease.csv`: Product-level quantities per order.
- `products_shopease.csv`: Product metadata.

### Tech Stack
- **DuckDB**: For in-notebook, exceptionally fast, and production-style SQL analytics.
- **Jupyter Notebook** (`Weekly_Demand_Forecasting.ipynb`): For reproducibility, documentation, and step-by-step validation.
- **SQL-First Approach**: Chosen to emphasize correctness, transparency, and business logic over premature model complexity.

## 🔍 Methodology

The analysis follows a structured, decision-oriented workflow:
1. **Identify Top-10 Products** by total units sold to focus on the highest business impact and reduce noise.
2. **Aggregate Data** from transactional level to weekly demand per product.
3. **Generate a Continuous Calendar** and zero-fill missing weeks to correctly manage sparse data.
4. **Implement Baseline Forecasts**:
   - *Naive Forecast*: Next week's demand equals last week's demand.
   - *Rolling 3-Week Average*: Smooths short-term volatility by taking the mean of the past 3 weeks.
5. **Evaluate Forecast Accuracy** using Mean Absolute Error (MAE).
6. **Compare Results** at both product and portfolio levels.
7. **Formulate Recommendations** based on empirical evidence.

## 📊 Evaluation & Results

We evaluate our approaches using **Mean Absolute Error (MAE)**. A lower MAE indicates forecasts that are closer to reality, directly reducing the risk of over-stocking or stock-outs.

Across all top 10 products, the **rolling 3-week average consistently achieved a lower MAE** than the naive forecast.

*Example MAE Comparison from our evaluation:*
| Product ID | Naive MAE | Rolling 3-Week MAE |
|------------|-----------|--------------------|
| PROD_162   | 3.12      | **2.33**           |
| PROD_294   | 3.68      | **2.72**           |
| PROD_327   | 3.40      | **2.56**           |
| PROD_117   | 3.36      | **2.54**           |

## ✅ Final Decision & Next Steps

**Decision:** The **rolling 3-week average** is recommended as the demand signal for weekly restocking decisions. It provides more stable and accurate estimates of near-term demand compared to the naive approach.

**Future Improvements:**
This current approach assumes demand is relatively stable week-to-week and does not explicitly model seasonality, promotions, or price changes. With more historical data and additional business signals, advanced machine learning forecasting methods (e.g., ARIMA, Prophet, or XGBoost) could be evaluated. However, for the current data volume and operational needs, this baseline provides a strong and reliable foundation.

## 🚀 How to Run

1. Ensure you have Python installed.
2. Install the necessary dependencies:
   ```bash
   pip install duckdb jupyter
   ```
3. Launch Jupyter Notebook:
   ```bash
   jupyter notebook
   ```
4. Open `Weekly_Demand_Forecasting.ipynb` and run all cells sequentially to reproduce the SQL tables and MAE evaluations.
