# Introduction

For those that are unaware, **League of Legends**, also called "League" or "LoL" for short, is a 5 vs 5 online game where each player controls a character called "Champions", each with different abilities, roles, and playstyles where the common goal is to defend your team's base whilst trying to destory the other teams'. Maps consists of 3 lanes, seperated by jungles, and scattered with objectives to boost in-game metrics like gold, levels, items, etc, and where gaps in these come from team synergy, game sense, mechanical skills, and overall luck. You can found our more about the dataset here: [League Dataset](https://oracleselixir.com/tools/downloads)

In this project, we will analyze the **2025 E-sports Match Dataset**, which comprises over **15,000 observations** containing stats like kills, gold, deaths, and more for each player in every match.

## Central Question:
*Which role is most valuable in carrying a team to victory based on early game performance?*  

Why do we care you might ask?

This question matters because early game advantages, such as a strong gold lead, can affect overall stats like overall kills or getting objectives and set the tone for the entire match. By understanding which role consistently secures these advantages, coaches, recruiters, fans, and even casual players can gain insights into the dynamics that drive a team's success.

**Relevant Columns in the Dataset:**

- **`position`**:  
  - The position or role the individual played during that match.

  - **`result`**:  
  - A binary indicator of the match outcome for a team, where `1` indicates a win and `0` indicates a loss. This value is consistent across all players on the same team.

- **`golddiffat10`**:  
  - For each player, this column records the gold difference at 10 minutes between that individual and the opposing team's same role player. 

- **`kills`**:  
  - The number of kills for a player

- **Additional Columns** (`killsat10`, `deathsat10`, `assistsat10`, and corresponding kill columns by specific times):  
  - These columns break down early game performance by specific roles, allowing us to perform a detailed role-by-role analysis.

By exploring these columns, this project aims to uncover which role consistently demonstrates an early advantage and how that advantage translates into overall match victory. This analysis is not only insightful for understanding competitive dynamics in League of Legends but also valuable for anyone interested in data-driven decision-making in e-sports.



# Data Cleaning and EDA


## Data Cleaning

For context, each game consists of 12 rows, 10 of it is comprises of each player and their stats, and the 2 leftover comprises of team stat for both team. Since we are looking at individual stats, we can drop the rows that consist of only match stats.

We'll also ensure each column that should be a boolean is a boolean, ensure the names of each column are properly named (contains no spaces), and drop columns that aren't necessary as of now. Some notable columns of the ones we kept are **`position`**, **`result`**, **`killsat10`**, **`golddiffat10`**.

Below is a preview of the first few rows of our cleaned DataFrame:

| playername   | teamname   | champion   | position   | result   |   kills |   golddiffat10 |   killsat10 |   deathsat10 |   assistsat10 |
|:-------------|:-----------|:-----------|:-----------|:---------|--------:|---------------:|------------:|-------------:|--------------:|
| PatkicaA     | IZI Dream  | Gnar       | top        | False    |       1 |           -336 |           0 |            1 |             0 |
| Joinze       | IZI Dream  | Maokai     | jng        | False    |       0 |           -474 |           0 |            0 |             0 |
| Sayn         | IZI Dream  | Hwei       | mid        | False    |       1 |           -405 |           0 |            0 |             0 |
| Shiganari    | IZI Dream  | Jinx       | bot        | False    |       1 |           -422 |           0 |            0 |             0 |
| Lekcyc       | IZI Dream  | Leona      | sup        | False    |       0 |            -85 |           0 |            0 |             0 |


## Univariate Analysis

To understand the distribution of early game performance, we examined the **gold difference at 10 minutes** (`golddoffat10`). This metric reflects the early gold advantage (or disadvantage) for players, a key indicator of early momentum.

<iframe
  src="assets/fig2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

*Interpretation:*  
The histogram indicates that while many teams have a **near-neutral gold difference**, a subset achieves a **large positive** or **negative advantage** early in the game, which could be critical for match outcomes. Differences are mirrored between roles so it essentially **reflects accross the y-axis**. Standard Deviation is around 457


## Bivariate Analysis

Next, we can investigate the relationship between **kills at 10 minutes** and **gold difference at 10 minutes** across each role. The scatter plot is color coded for each role.

<iframe
  src="assets/fig6.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

*Interpretation:*  
The boxplots show that bot lane seems to do best overall when it comes to securing kills with support being on the lower end. Also, the overall shape of the scatter plot seems to lean more left, hinting that positive gold difference leads to more kills at 10 minutes.


## Interesting Aggregates

Finally, we explored aggregate statistics by creating a pivot table that shows the average **kills and deaths at 10 minutes** for each role across games.

| position   |   deathsat10 |   killsat10 |
|:-----------|-------------:|------------:|
| bot        |     0.374175 |    0.652228 |
| jng        |     0.427393 |    0.528053 |
| mid        |     0.377063 |    0.455446 |
| sup        |     0.585809 |    0.230611 |
| top        |     0.448845 |    0.325083 |

   
- **Bot lane** shows the highest average kills at 10 minutes (about 0.65) while maintaining a relatively low average deaths (about 0.37). This suggests bot laners often secure an early edge in kills.  
- **Support** has the highest average deaths at 10 minutes (about 0.59) and the lowest kills (about 0.23), consistent with a role that prioritizes enabling teammates rather than racking up kills.  
- **Jungle**, **mid**, and **top** fall somewhere in between, indicating more moderate early aggression and deaths.  

These differences reflect each role’s responsibilities and risk profiles in the early game. For instance, supports often roam and engage early, leading to more deaths, whereas bot laners can capitalize on assists and positioning to secure more kills. This breakdown highlights how each role’s early performance can influence a team’s overall carry potential and early-game momentum.



# Assessment of Missingness


## NMAR Analysis

When checking for missing or NaN values, we find that there are missing values in columns like **golddiffat10**, **killsat10**, **assistsat10**, and **deathsat10**. I suspect that the missing values in these columns may be **NMAR (Not Missing At Random)** because some of the matches ended prematurely (due to early forfeits, disconnects, or technical issues) before the 10-minute mark was reached. Such missingness are not captured by the other overall performance metrics we observe (like kills or deaths).


## Missingness Dependency

To explore missing values in **golddiffat10**, we lets perform permutation tests to assess the dependency of its missingness on other variables. Our tests will focus on:

- **Kills:**  
  The permutation test comparing the mean kills between rows with missing **golddiffat10** and those with recorded values yielded an observed difference of **0.245** with a p-value of **0.01**. This low p-value indicates that the missingness of **golddiffat10** is significantly associated with kill counts. In other words, players with missing **golddiffat10** tend to have a slightly different number of kills compared to those with complete data.

- **Position:**  
  In contrast, the permutation test for the relationship between missingness in **golddiffat10** and **position** resulted in an observed variance of **0.0** in missing rates across positions, with a p-value of **1.0**. This means that the rate at which **golddiffat10** is missing is identical across all player roles, suggesting no dependency on position.

Below is an interactive Plotly histogram that shows the empirical distribution of the test statistic for the dependency between missingness in **golddiffat10** and **kills**. Observed difference is marked by the rid line difference:

<iframe
  src="assets/fig7.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

  
The histogram shows that the observed difference (0.245) lies in the extreme tail of the permutation distribution, with only about 1% of the random permutations producing a difference as extreme. This confirms that the missingness in **golddiffat10** is significantly associated with kill counts, whereas a similar test for **position** showed no such dependency.
