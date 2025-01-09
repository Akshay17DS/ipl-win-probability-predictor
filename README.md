# IPL Win Probability Predictor

## Project Overview
The IPL Win Predictor is a machine learning-based project that predicts the winner of an IPL match based on various match-related factors. It leverages historical IPL data, player performance metrics, and match conditions to provide accurate predictions. This project is designed to enhance fan engagement and offer valuable insights for cricket enthusiasts.

---

## Project Flow
1. **Dataset Details**  
2. **Data Preprocessing**  
3. **Model Building**
4. **Match Progression Simulation**

---

## Dataset Details

### 1. Matches Dataset
This dataset contains summary-level information about IPL matches. It includes details such as the teams playing, match results, and player performance.

Key Columns:
- `id`: Unique identifier for each match.
- `Season`: The IPL season the match belongs to (e.g., IPL-2017).
- `city`: City where the match was played.
- `date`: Match date in DD-MM-YYYY format.
- `team1`/`team2`: Names of the two teams playing.
- `toss_winner`: Team that won the toss.
- `toss_decision`: Decision taken by the toss winner (`bat` or `field`).
- `result`: Type of result (`normal` or `tie`).
- `dl_applied`: Whether Duckworth-Lewis method was applied (`1` for yes, `0` for no).
- `winner`: The winning team of the match.
- `win_by_runs`/`win_by_wickets`: Margin of victory (either by runs or wickets).
- `player_of_match`: The player awarded Man of the Match.
- `venue`: Stadium where the match was played.
- `umpire1`/`umpire2`/`umpire3`: Names of the match officials.

### 2. Deliveries Dataset
This dataset provides ball-by-ball details of IPL matches, capturing granular information about every delivery bowled.

Key Columns:
- `match_id`: Unique identifier linking deliveries to the respective match in the matches dataset.
- `inning`: The innings number (`1` or `2`).
- `batting_team`/`bowling_team`: Teams batting and bowling in the delivery.
- `over`/`ball`: Over and ball number within the over.
- `batsman`/`non_striker`: Names of the batsman and non-striker.
- `bowler`: Name of the bowler.
- `is_super_over`: Indicates whether the delivery is part of a Super Over (`1` for yes, `0` for no).
- `batsman_runs`: Runs scored by the batsman on the delivery.
- `extra_runs`: Additional runs conceded (wide, no-ball, etc.).
- `total_runs`: Total runs scored on the delivery (batsman + extras).
- `player_dismissed`: Name of the player dismissed (if any).
- `dismissal_kind`: Type of dismissal (e.g., bowled, caught).
- `fielder`: Name of the fielder involved in the dismissal (if applicable).

---

## Data Preprocessing
Key steps for preprocessing the dataset include:

1. **Current Score Calculation**  
   - Calculated the cumulative sum of runs for each match and inning.

2. **Runs and Balls Left**  
   - `Runs Left`: Subtracting the current score from the target score.
   - `Balls Left`: Calculated as `120 - (over * 6 + ball)`.

3. **Player Dismissal Handling**  
   - Missing values in `player_dismissed` were replaced with "0".
   - Transformed to indicate whether a player was out (`0` = not out, `1` = out).
   - Used cumulative sum to determine wickets taken and wickets left (`10 - cumulative dismissals`).

4. **Run Rates**  
   - **Current Run Rate (CRR)**: `(current_score * 6) / balls_faced`.
   - **Required Run Rate (RRR)**: `(runs_left * 6) / balls_left`.

5. **Match Outcome**  
   - Added a new column `result` indicating win (`1`) or loss (`0`).

6. **Final Dataset Creation**  
   - Selected relevant features: `batting_team`, `bowling_team`, `city`, `runs_left`, `balls_left`, `wickets`, `total_runs`, `CRR`, `RRR`, `result`.
   - Removed missing values and rows where `balls_left = 0`.

Sample Processed Data:

| batting_team             | bowling_team            | city    | runs_left | balls_left | wickets_left | total_runs | crr      | rrr       | result |
|--------------------------|--------------------------|---------|-----------|------------|--------------|------------|----------|-----------|--------|
| Kings XI Punjab          | Rajasthan Royals        | Durban  | 208       | 108        | 8            | 211        | 1.500000 | 11.555556 | 0      |
| Royal Challengers Bangalore | Mumbai Indians        | Mumbai  | 91        | 66         | 9            | 141        | 5.555556 | 8.272727  | 1      |

---

## Model Building

### Data Splitting
- Features (`X`) and target (`y`) were split, where `result` is the target variable.
- Dataset was divided into training and testing sets using an 80-20 split.

### Data Transformation
- Used OneHotEncoding for categorical features (`batting_team`, `bowling_team`, `city`) with `drop='first'`.

### Model Pipeline
- Built a pipeline:
  - Step 1: OneHotEncoder for categorical features.
  - Step 2: Logistic Regression model with `solver='liblinear'`.

### Model Training and Evaluation
- Achieved an **accuracy of 80.31%** on the test dataset.

---

## Match Progression Simulation
- Created a simulation of match progression over time.
- Visualized progression using line plots for `runs`, `wickets`, and `win/lose progression`.

Sample Progression Table:

| **End of Over** | **Runs After** | **Wickets in Over** | **Win Percentage** | **Lose Percentage** |
|-----------------|----------------|---------------------|---------------------|---------------------|
| 1               | 4              | 0                   | 45.9%               | 54.1%               |
| 2               | 8              | 0                   | 51.1%               | 48.9%               |
| 3               | 1              | 0                   | 44.2%               | 55.8%               |
| 4               | 7              | 1                   | 67.8%               | 32.2%               |
| 5               | 12             | 0                   | 57.5%               | 42.5%               |
| 6               | 13             | 0                   | 54.9%               | 45.1%               |
| 7               | 9              | 0                   | 60.7%               | 39.3%               |
| 8               | 15             | 0                   | 74.2%               | 25.8%               |
| 9               | 17             | 0                   | 87.2%               | 12.8%               |
| 10              | 17             | 0                   | 91.8%               | 8.2%                |
| 11              | 9              | 1                   | 82.1%               | 17.9%               |
| 12              | 9              | 0                   | 85.4%               | 14.6%               |
| 13              | 8              | 0                   | 87.4%               | 12.6%               |
| 14              | 8              | 0                   | 89.2%               | 10.8%               |
| 15              | 5              | 1                   | 81.1%               | 18.9%               |
| 16              | 8              | 1                   | 72.9%               | 27.1%               |
| 17              | 8              | 2                   | 47%                 | 53%                 |
| 18              | 6              | 1                   | 31.6%               | 68.4%               |
| 19              | 8              | 2                   | 11.7%               | 88.3%               |

---

## Visualizations
![download](https://github.com/user-attachments/assets/a2c72116-0e74-42d5-b2d1-57d35713dc02)


The table and graph demonstrates how the model predicts win/loss probabilities as the game progresses, highlighting each overs and changes in the match scenario.
---
