# 🏀 Lakers Player Development Engine (The Consistency Tracker)

An end-to-end data analytics project that cuts through the noise of "streaky" performances to track real developmental growth.
This engine uses Python for automated data extraction and Power BI for advanced visualization and predictive "What-If" modeling.

---

## 🚀 Project Overview

The goal of this project is to move beyond static season averages and evaluate players based on **trajectory**.
By utilizing rolling averages and efficiency metrics, we can identify which players are scaling effectively with increased responsibility.

### Key Features:
* **Automated Roster Sourcing:** Automatically fetches the entire active Los Angeles Lakers roster.
* **The 10-Game Smoothing Filter:** Uses a rolling average to eliminate one-off "outlier" games.
* **Efficiency vs. Volume Analysis:** Visualizes whether a player's shooting efficiency (TS%) holds up as their usage (USG%) increases.
* **"What-If" Minutes Modifier:** A projection tool to estimate a bench player's production if given starter-level minutes.

---

## 🛠️ Tech Stack

| Component | Tool | Purpose |
| :--- | :--- | :--- |
| **Data Sourcing** | `nba_api` (Python) | Live extraction of NBA box scores and roster data. |
| **Processing** | `pandas` | Feature engineering (TS%, AST/TOV ratio). |
| **Storage** | CSV | Intermediate data format for Power BI ingestion. |
| **Visualization** | Power BI | Interactive dashboarding and DAX calculations. |

---

## 📊 Phase 1: Python Data Pipeline

The script (`fetch_lakers_data.py`) performs the following steps:
1.  **Roster Discovery:** Connects to the `commonteamroster` endpoint to identify all current Lakers players.
2.  **Granular Logging:** Iterates through each player to pull game-by-game logs (essential for rolling averages).
3.  **Feature Engineering:** * **True Shooting % (TS%):** Calculated as $$PTS / (2 \times (FGA + 0.44 \times FTA))$$
    * **AST/TOV Ratio:** Includes logic to handle division-by-zero for players with 0 turnovers.
4.  **Rate Limiting:** Implements a `time.sleep` buffer to respect NBA API protocols and prevent IP blacklisting.

---

## 📉 Phase 2: Power BI Intelligence (DAX)

The dashboard is powered by custom DAX measures to provide deeper insights:

### 1. 10-Game Rolling Average
Instead of looking at a single game, we use a window of the last 10 games to see the "true" trend:
```dax
10-Game Rolling Avg = 
AVERAGEX(
    TOPN(10, 
        FILTER(ALL('LakersData'), 'LakersData'[Player] = SELECTEDVALUE('LakersData'[Player]) && 'LakersData'[Date] <= MAX('LakersData'[Date])),
        'LakersData'[Date], DESC
    ),
    'LakersData'[Points]
)
