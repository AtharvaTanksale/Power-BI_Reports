# Macroeconomic Impact Tracker (Power BI)

## Project Overview
This Power BI dashboard is an interactive analytical tool designed to correlate major macroeconomic indicators with historical stock market performance.
By tracking the S&P 500 index against the Federal Funds Rate, this project visualizes the historical inverse relationship between equity valuations and borrowing costs to identify systemic market vulnerabilities.

## Data Architecture & Pipeline
This project utilizes a hybrid approach, combining Python for data extraction with Power BI for advanced relational modeling and visualization.
* **Data Sourcing (Python):** A custom Python script utilizes `yfinance` and `pandas_datareader` to scrape live, historical financial data directly from Yahoo Finance and the St. Louis Fed (FRED) API.
* The script forward-fills monthly macroeconomic data to match daily market trading days and outputs a clean CSV.
* **Data Modeling (Power BI):** The raw financial data is connected to a custom-built, continuous `Master Calendar` table via a one-to-many relationship.
* This Star Schema ensures that DAX time-intelligence functions calculate perfectly across weekends and market holidays.

## Key Power BI Features & Analytics
* **Advanced DAX (Time-Intelligence):** Custom measures calculate standard investment banking metrics, including a 365-day Year-over-Year (YoY) Index Return and a 90-Day Rolling Moving Average to smooth out market volatility.
* **Parameter-Driven "What-If" Scenarios:** Features an interactive numeric parameter allowing users to simulate hypothetical market shocks (e.g., "What happens to the portfolio value if inflation spikes by 4%?"). The dashboard dynamically recalculates and displays the exact point drawdown.
* **Dual-Axis Visualization:** Utilizes clean, executive-facing dual-axis charting to directly compare index price action (columns) against interest rate hikes (trend lines).
* **Error Handling:** Division calculations are wrapped in `DIVIDE()` functions to gracefully handle zero-value trading days without returning infinity errors.

## Featured DAX Calculation
```dax
// Calculates the Year-over-Year percentage change in the S&P 500
YoY Index Return = 
DIVIDE (
    SUM('FinancialData'[SP500_Price]) - CALCULATE(SUM('FinancialData'[SP500_Price]), SAMEPERIODLASTYEAR('Calendar'[Date])),
    CALCULATE(SUM('FinancialData'[SP500_Price]), SAMEPERIODLASTYEAR('Calendar'[Date]))
)
