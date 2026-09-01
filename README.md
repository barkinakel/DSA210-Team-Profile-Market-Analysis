# Advanced Tactical Analysis & Betting Strategy
## DSA 210 Term Project - Fall 2025-2026

> [!WARNING]
> **Archived — results are not reliable.**
>
> This repository is the original Fall 2025–26 submission, kept for reference.
> A later review of the pipeline identified methodological issues. The win rates, ROI and Safety Score figures reported below are in-sample
> and should not be treated as validated performance or used for any decision.
>
> A rebuilt version with proper train/validation/test separation and out-of-sample
> testing is in development. *(link to be added)*

### Motivation
The primary motivation for this project is to test the **Efficient Market Hypothesis (EMH)** in the context of sports betting. EMH suggests that all publicly available information is already reflected in betting odds, making it impossible to systematically "beat" the market. 

This project challenges this by exploring whether a machine learning model that incorporates **tactical profiling** rather than just traditional win/loss records can find "value bets" which represents the situations where the model's calculated probability differs from the bookmaker's implied odds.
### Data Sources
The project utilizes data from the following sources:
*   **[FBref.com](https://fbref.com/en/)**: Used for detailed match and team statistics, including Expected Goals (xG), Possession, Touches in Attacking Penalty Area, and Defensive Actions.
*   **[TotalCorner.com](https://www.totalcorner.com/)**: Used for historical corner kick odds to enable backtesting and value identification.

### Project Scope
While the initial research considered multiple leagues and betting markets, this project specifically narrows its focus to the **English Premier League** and the **Corner Kick Market**.
*   **Why Premier League?** It has the most reliable advanced metrics (e.g., "Touches in Attacking Third") needed to define tactical styles.
*   **Why Corner Market?** Unlike the popular Match Result (1X2) market, the corner market receives less attention from oddsmakers, creating more opportunities to find pricing errors using statistical models.

### Methodology

The project follows a structured pipeline:

1.  **Data Collection & Preprocessing**:
    *   Scraped match statistics from the English Premier League (2019-2024).
    *   Scraped betting odds.
    *   Merged tactical metrics with historical betting odds.
    *   Cleaned data and engineered features.

2.  **Exploratory Data Analysis (EDA)**:
    *   Conducted Paired T-Tests to quantify Home vs. Away performance bias.
    *   Validated the correlation between specific tactical metrics like Box Touches and Corner Kicks.

3.  **Tactical Profiling (Unsupervised Learning)**:
    *   Applied **K-Means Clustering** to categorize teams into distinct playing styles (e.g. *High Pressing*, *Deep Block*).
    *   Used Silhouette Analysis to determine the optimal number of clusters (K=6).
    *   Performed clustering dynamically using only prior match data to prevent data leakage.

4.  **Predictive Modeling**:
    *   Developed a weighted 3-component prediction model combining:
        *   **Tactical Matchup History** (80% weight)
        *   **Season Averages** (15% weight)
        *   **Recent Form** (5% weight)
    *   Optimized weights using Grid Search to minimize Brier Score and MAE.

5.  **Betting Strategy & Optimization**:
    *   **Probabilistic Modeling**: Used the **Poisson Distribution** to calculate the "Fair Probability" of corner outcomes based on predicted totals.
    *   **Risk Management**: Introduced a **Safety Score** metric (Z-Score based) to filter bets where the edge exceeds historical variance.
    *   **Staking Strategy**: Simulated a staking plan to optimize capital allocation (Edge Power, Confidence Threshold, Volatility Penalty).

### Key Findings
*   **Tactical Correlations**: Specific matchups, such as a "Box Siege" team playing against a "Deep Block" team, produce a statistically significant surplus of corners compared to the league average.
*   **Market Inefficiency**: The model identified that bookmakers often underprice these specific tactical mismatches, creating a "Value Bet" opportunity.
*   **Profitable Thresholds**: Backtesting revealed that a **Safety Score > 0.22** for "Over" bets yielded a profitable win rate (>52.7%) with a robust sample size. Similarly, a **Safety Score > 0.11** for "Under" bets achieved a **55.3% Win Rate** across 338 matches.

### Files in the Repository

| File/Folder | Description |
| :--- | :--- |
| `Tactical_analysis_and_strategy.ipynb` | The main notebook containing the entire pipeline: Data Collection, EDA, Clustering, Modeling, and Strategy Optimization. |
| `data/` | Directory containing the datasets (`premier_league_metrics.csv`, `PL_metrics_plus_odds.csv`). |
| `requirements.txt` | List of Python dependencies required to run the analysis. |
| `README.md` | Project documentation, methodology, and findings. |

### Installation & Usage

1.  **Clone the Repository**:
    ```bash
    git clone <your-repo-url>
    cd <your-repo-name>
    ```
2.  **Install Dependencies**:
    ```bash
    pip install -r requirements.txt
    ``
Ensure the `premier_league_metrics.csv` and `PL_metrics_plus_odds.csv` files are located in the `Data/` directory.

4.  **Run the Analysis**:
    *   Open `Tactical_analysis_and_strategy.ipynb` in Jupyter Notebook or VS Code.

### Limitations & Future Work
*   **Game State**: The current model uses aggregate match stats and does not account for game state (e.g., a team stopping attacks after leading 3-0).
*   **Data Quality**: Detailed tactical data is less reliable for lower-tier leagues, limiting immediate expansion.
*   **Future Expansion**: Plans include testing the framework on other major European leagues (La Liga, Bundesliga etc.) and incorporating "Corner Handicap" markets once reliable historical data is sourced.
*   **Market Diversification**: Expanding the model to cover not just Corner markets, but also general markets such as **Over/Under** (e.g., Total Goals) and to broaden the strategy's scope.
*   **Forward Testing**: While the backtesting results are promising, in order to test how sound the model is, I will deploy the strategy on the ongoing 2025-2026 EPL season data to verify if the simulated ROI holds up.

  ### Academic Integrity
*   **AI Usage**: Generative AI tools were utilized extensively throughout the entire development process. This includes assistance with writing data scraping scripts, implementing complex logic, debugging code, optimizing model parameters, formatting. While AI accelerated the coding process, conceptual frameworks, the core ideas and the analytical decisions are entirely my own.
*   **Citations**: Data sources (FBref, TotalCorner) and external libraries are properly credited.
