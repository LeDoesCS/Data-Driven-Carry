# Introduction

For those that are unaware, **League of Legends**, also called "League" or "LoL" for short, is a 5 vs 5 online game where each player controls a character called "Champions", each with different abilities, roles, and playstyles where the common goal is to defend your team's base whilst trying to destory the other teams'. Maps consists of 3 lanes, seperated by jungles, and scattered with objectives to boost in-game metrics like gold, levels, items, etc, and where gaps in these come from team synergy, game sense, mechanical skills, and overall luck.

In this project, we will analyze the **2025 E-sports Match Dataset**, which comprises over **15,000 observations** containing stats like kills, gold, deaths, and more for each player in every match.

## Central Question:
*Which role is most valuable in carrying a team to victory based on early game performance?*  

This question matters because early game advantages, such as a strong gold lead, can affect overall stats like kills and set the tone for the entire match, can be a good in. By understanding which role consistently secures these advantages, coaches, recruiters, fans, and even casual players can gain insights into the dynamics that drive a team's success.

**Relevant Columns in the Dataset:**

- **`golddiffat10`**:  
  - For each player, this column records the gold difference at 10 minutes between that individual and the opposing team's same role player. 

- **`kills`**:  
  - The number of kills for a player

- **`result`**:  
  - A binary indicator of the match outcome for a team, where `True` indicates a win and `False` indicates a loss. This value is consistent across all players on the same team.

- **Additional Columns** (e.g., `top_gold`, `jng_gold`, `mid_gold`, `sup_gold`, `bot_gold`, and corresponding kill columns):  
  - *Description*: These columns break down early game performance by specific roles, allowing us to perform a detailed role-by-role analysis.

By exploring these columns, this project aims to uncover which role consistently demonstrates an early advantage and how that advantage translates into overall match victory. This analysis is not only insightful for understanding competitive dynamics in League of Legends but also valuable for anyone interested in data-driven decision-making in e-sports.

# Data Cleaning and Exploratory Data Analysis (EDA)



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