# DSA210-Team-Profile-Market-Analysis
# Motivation
My motivation for this project is to test "Efficient Market Hypothesis" which states that all publicly available information is already fully and accurately reflected in the betting odds, making it impossible to systematically "beat" the market. This project aims to explore whether a machine learning model, which considers underlying factors like playing styles that go beyond traditional statistics, systematically identify valuable opportunities that the market has missed. This project combines my interest in football with market analysis to apply data science methodologies to a practical, real world problem.
# Data Source
The project will make use of publicly available datasets:  

- **football-data.co.uk** - Contains detailed match-level statistics (shots, corners, cards, etc.) for past seasons in major European leagues.  
- **FBref.com** - Provides detailed match and team statistics such as expected goals (xG), possession, and pressing metrics.  
- **oddsPortal.com** - Contains historical bookmaker odds across multiple markets, allowing comparison between model's and market-implied odds to identify potential value bets.

# Data Collection
The dataset used in this project (`fbref_data.csv`) is a comprehensive compilation of match statistics from the **English Premier League** over the last 5 seasons (2019-2024).

### Collection Strategy
To overcome API rate limits and gather deep tactical metrics, a custom Python script was developed to:
1.  Aggregate data from multiple statistical tables:
    * **Standard Stats:** Goals, xG, xGA, Possession.
    * **Shooting:** Shot distance, Shots on Target.
    * **Passing Types:** Corners, Crosses, Switch plays.
    * **Defense:** Tackles, Pressing, Clearances.
    * **Miscellaneous:** Aerial duels, Fouls, Cards.
2.  **Merge & Clean:** The script merges these disparate tables based on match ID and date, creating a unified dataset with over 150+ tactical features per match. Which will be narrowed down to statistics merely used for this project.

The raw data collection script can be found in the scripts/ folder.

# Data Analysis
I plan to analyze seasonal team statistics and group teams that have similar statistical profiles. Next, I will train a model based on these profiles to predict match outcomes. Finally, I will compare the model's predictions against the market's odds and conduct a backtesting process to see if a profitable strategy exists.
