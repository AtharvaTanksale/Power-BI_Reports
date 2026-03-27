# Tech Giant M&A Activity Dashboard (1988 - 2021)

## Project Overview
This project is an interactive Power BI dashboard that analyzes historical Mergers and Acquisitions (M&A) data from 14 of the world's largest technology companies, including Microsoft, Google, Apple, and Amazon.
The dashboard is designed to uncover trends in deal volume, target sectors, and geographic acquisition footprints over a 30-year period.

## Dataset
* **Source File:** `acquisitions_update_2021.csv`
* **Size:** 1,455 individual transactions.
* **Key Columns:** Parent Company, Acquired Company, Acquisition Year, Acquisition Month, Business/Sector, Country, and Acquisition Price.
* **Data Cleaning Notes:** The raw dataset contained hyphens (`-`) for undisclosed acquisition prices and missing years.
These were cleaned and transformed into `null` values using Power Query to allow for accurate DAX aggregations and numerical sorting.

## Tools & Technologies Used
* **Microsoft Power BI:** Data visualization and dashboard assembly.
* **Power Query:** Data extraction, cleaning, and type formatting.
* **DAX (Data Analysis Expressions):** Used to calculate aggregate financial metrics.

## Key Visualizations
The dashboard utilizes an "F-Pattern" layout for optimal readability, featuring the following visuals:
* **Average Deal Value (Card):** A dynamic KPI displaying the average acquisition price, which recalculates based on the selected tech giant or sector.
* **Deal Volume Over Time (Line Chart):** Tracks the frequency of acquisitions year-over-year to identify periods of market consolidation.
* **Acquisition Spend by Company (Treemap):** Visualizes the total disclosed capital spent by each Parent Company.
* **Global Acquisition Footprint (Map):** Plots the geographic locations of acquired companies, illustrating international expansion strategies.
* **Top 10 Largest Tech Acquisitions (Matrix/Table):** Highlights the highest-valued mega-deals in the dataset, sortable by Parent Company.

## Custom DAX Measures
```dax
// Calculates the average transaction size for deals with disclosed financial terms
Average Deal Value = AVERAGE('Acquisitions'[Acquisition Price])
