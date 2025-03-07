# Introduction

For those that are unaware, **League of Legends**, also called "League" or "LoL" for short, is a 5 vs 5 online game where each player controls a character called "Champions", each with different abilities, roles, and playstyles where the common goal is to defend your team's base whilst trying to destory the other teams'. Maps consists of 3 lanes, seperated by jungles, and scattered with objectives to boost in-game metrics like gold, levels, items, etc, and where gaps in these come from team synergy, game sense, mechanical skills, and overall luck.

In this project, we will analyze the **2025 E-sports Match Dataset**, which comprises over **15,000 observations** containing stats like kills, gold, deaths, and more for each player in every match.

## Central Question:
*Which role is most valuable in carrying a team to victory based on early game performance?*  

Why do we care you might ask?

This question matters because early game advantages, such as a strong gold lead, can affect overall stats like kills and set the tone for the entire match. By understanding which role consistently secures these advantages, coaches, recruiters, fans, and even casual players can gain insights into the dynamics that drive a team's success.

**Relevant Columns in the Dataset:**

- **`golddiffat10`**:  
  - For each player, this column records the gold difference at 10 minutes between that individual and the opposing team's same role player. 

- **`kills`**:  
  - The number of kills for a player

- **`result`**:  
  - A binary indicator of the match outcome for a team, where `True` indicates a win and `False` indicates a loss. This value is consistent across all players on the same team.

- **Additional Columns** (e.g., `top_gold`, `jng_gold`, `mid_gold`, `sup_gold`, `bot_gold`, and corresponding kill columns):  
  - These columns break down early game performance by specific roles, allowing us to perform a detailed role-by-role analysis.

By exploring these columns, this project aims to uncover which role consistently demonstrates an early advantage and how that advantage translates into overall match victory. This analysis is not only insightful for understanding competitive dynamics in League of Legends but also valuable for anyone interested in data-driven decision-making in e-sports.


## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

For context, each game consists of 12 rows, 10 of it is comprises of each player and their stats, and the 2 leftover comprises of team stat for both team. Since we are looking at individual stats, we can drop the rows that consist of only match stats.

We'll also ensure each column that should be a boolean is a boolean, ensure the names of each column are properly named (contains no spaces), and drop columns that aren't necessary as of now. Columns we'll keep are **`playername`**, **`teamname`**, **`champion`**, **`position`**, **`result`**, **`kills`**, **`golddiffat10`**.

Below is a preview of the first few rows of our cleaned DataFrame:

| playername   | teamname   | champion   | position   | result   |   kills |   golddiffat10 |
|:-------------|:-----------|:-----------|:-----------|:---------|--------:|---------------:|
| PatkicaA     | IZI Dream  | Gnar       | top        | False    |       1 |           -336 |
| Joinze       | IZI Dream  | Maokai     | jng        | False    |       0 |           -474 |
| Sayn         | IZI Dream  | Hwei       | mid        | False    |       1 |           -405 |
| Shiganari    | IZI Dream  | Jinx       | bot        | False    |       1 |           -422 |
| Lekcyc       | IZI Dream  | Leona      | sup        | False    |       0 |            -85 |

### Univariate Analysis

To understand the distribution of early game performance, we examined the **mid lane gold difference** (`mid_gold`). This metric reflects the early gold advantage (or disadvantage) for mid lane players—a key indicator of early momentum. The histogram below shows that most teams cluster around a small gold difference, though some teams achieve a significant early advantage.

<iframe src="assets/mid_gold_hist.html" width="800" height="600" frameborder="0"></iframe>

*Interpretation:*  
The histogram indicates that while many teams have a near-neutral gold difference in the mid lane, a subset achieves a large positive or negative advantage early in the game, which could be critical for match outcomes.

### Bivariate Analysis

Next, we investigated the relationship between **mid lane gold difference** (`mid_gold`) and **mid lane kills** (`mid_kills`). The scatter plot below reveals a positive association—teams with a higher mid lane gold advantage tend to have higher mid lane kill counts. This suggests that early gold advantage may contribute to greater aggression and performance in the mid lane.

<iframe src="assets/mid_gold_vs_mid_kills.html" width="800" height="600" frameborder="0"></iframe>

*Interpretation:*  
The scatter plot shows a clear trend: as mid lane gold difference increases, so do mid lane kills. This relationship supports the idea that early advantages might translate into more impactful performance metrics.

### Interesting Aggregates

Finally, we explored aggregate statistics by creating a pivot table that shows the average **mid lane gold difference** for each team across games. This table helps identify teams that consistently secure an early advantage in the mid lane, highlighting potential trends in performance at the team level.

<iframe src="assets/pivot_mid_gold.html" width="800" height="400" frameborder="0"></iframe>

*Interpretation:*  
The pivot table underscores differences across teams, indicating which teams consistently outperform or underperform in early mid lane gold difference. This aggregated view is significant for understanding long-term trends and the potential impact of early game performance on match outcomes.


<iframe
  src="assets/fig1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/fig2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/fig3.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/fig4.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>