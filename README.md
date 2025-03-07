
# League of Legends

For those that are unaware, **League of Legends**, also called "League" or "LoL" for short, is a 5 vs 5 online game where each player controls a character called "Champions", each with different abilities, roles, and playstyles where the common goal is to defend your team's base whilst trying to destory the other teams'. Maps consists of 3 lanes, seperated by jungles, and scattered with objectives to boost in-game metrics like gold, levels, items, etc, and where gaps in these come from team synergy, game sense, mechanical skills, and overall luck. You can found out more about the dataset here: [League Dataset](https://oracleselixir.com/tools/downloads)

In this project, we will analyze the **2025 E-sports Match Dataset**, which comprises over **15,000 observations** containing stats like kills, gold, deaths, and more for each player in every match. For this, we'll pose a central question that our data analysis will stem from.

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

Results, our primary concern, is a binary column of 1s and 0s, we'll change that so it's True for winners, and False for non-winners. We'll also ensure each column that should be a boolean is a boolean, ensure the names of each column are properly named (contains no spaces), and drop columns that aren't necessary as of now. Some notable columns of the ones we kept are **`position`**, **`result`**, **`killsat10`**, **`golddiffat10`**.

Below is a preview of the first few rows of our cleaned DataFrame:

| gameid           | teamid                                  | playername   | teamname   | champion   | position   | result   |   kills |   golddiffat10 |   killsat10 |   deathsat10 |   assistsat10 |
|:-----------------|:----------------------------------------|:-------------|:-----------|:-----------|:-----------|:---------|--------:|---------------:|------------:|-------------:|--------------:|
| LOLTMNT03_179647 | oe:team:2799e04c7212d3c262467ef25427eda | PatkicaA     | IZI Dream  | Gnar       | top        | False    |       1 |           -336 |           0 |            1 |             0 |
| LOLTMNT03_179647 | oe:team:2799e04c7212d3c262467ef25427eda | Joinze       | IZI Dream  | Maokai     | jng        | False    |       0 |           -474 |           0 |            0 |             0 |
| LOLTMNT03_179647 | oe:team:2799e04c7212d3c262467ef25427eda | Sayn         | IZI Dream  | Hwei       | mid        | False    |       1 |           -405 |           0 |            0 |             0 |
| LOLTMNT03_179647 | oe:team:2799e04c7212d3c262467ef25427eda | Shiganari    | IZI Dream  | Jinx       | bot        | False    |       1 |           -422 |           0 |            0 |             0 |
| LOLTMNT03_179647 | oe:team:2799e04c7212d3c262467ef25427eda | Lekcyc       | IZI Dream  | Leona      | sup        | False    |       0 |            -85 |           0 |            0 |             0 |


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



# Hypothesis Testing


Lets investigate one case more closely, we'll look at whether teams that secure more early kills tend to win. We performed a permutation test comparing **killsat10** (kills at the 10-minute mark) between winning and losing teams.

**Null Hypothesis (H₀):**  
The average number of kills at 10 minutes is the same for winning teams and losing teams.

**Alternative Hypothesis (H₁):**  
Winning teams have a higher average number of kills at 10 minutes than losing teams.

**Test Statistic & Significance Level:**  
We use the **difference in mean killsat10** (winners minus losers) as our test statistic.

**Results:**  
- **Observed difference in mean killsat10:** 0.12 (winners average about 0.12 more kills at 10 minutes than losers)  
- **p-value:** 0.0  

<iframe
  src="assets/fig8.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Because the p-value is well below 0.05, we have strong evidence against the null hypothesis. In other words, it is unlikely that this observed difference in early kills occurred by chance alone, suggesting that winning teams do indeed tend to secure more kills at 10 minutes.

**Conclusion:**  
We fail to confirm that winners and losers have the same early kill counts; instead, the data support the claim that winning teams achieve a higher average killsat10. However, we cannot definitively prove this relationship holds true for every possible match scenario, our findings are based on statistical inference and subject to the limitations of our dataset. Identifying whether winners consistently secure more early kills can help validate the importance of early aggression and lane dominance in League of Legends. We see here that is not just random chance that winners tend to get more early kills, showing some correlation or association between early kills and match outcome.



# Framing a Prediction Problem


Our prediction problem is centered on forecasting match outcomes using only early game performance data. Specifically, we ask:  
*Can we predict whether a team wins or loses based solely on early game metrics?

**Type:**  
Binary Classification

**Response Variable:**  
- **result** – This variable indicates whether a team wins (`True`) or loses (`False`). I chose it because winning is the ultimate goal in competitive play, and understanding which early performance indicators predict victory is crucial for evaluating carry potential.

**Features Available at Prediction Time:**  
- Early gold difference at 10 minutes  
- Early kill counts
- Early death counts
- Early assist counts 
- Additional role-based performance metrics  

**Evaluation Metrics:**  
- **Accuracy:** Measures the overall percentage of correct predictions.  
- **F1-Score:** Balances precision and recall, particularly important if there is any imbalance in match outcomes.

**Justification:**  
By using these early indicators to predict the final match outcome, our model provides actionable insights into which roles demonstrate the highest carry potential. This approach not only aligns with the competitive objectives of League of Legends but also informs strategic decisions on resource allocation and player development.



# Baseline Model

Our baseline model is designed to predict match outcomes using only early game performance metrics. Specifically, we focus on two metrics from the mid lane:

- **mid_gold:** The average gold difference at 10 minutes for mid lane players.
- **mid_killsat10:** The average number of kills at 10 minutes for mid lane players.

**Model Setup:**  
- We aggregated player-level data into a team-level dataset and extracted role-specific metrics.
- For our baseline, we used only **mid_gold** and **mid_killsat10**, which are both quantitative features.
- A single sklearn Pipeline was built that applies a **StandardScaler** to standardize the features, followed by a **Logistic Regression** classifier for binary classification.
- The model was trained on 80% of the data and evaluated on the remaining 20%, using accuracy and F1 score as our performance metrics.

**Performance:**

- **Accuracy:** 58.6%  
- **F1 Score:** 62.0%

Although the baseline performance indicates that our simple model correctly predicts match outcomes around 58.6% of the time with a balanced F1 score of 62.0%, which isn't the best, these results serve as an initial benchmark. They highlight the challenge of predicting match outcomes based solely on mid lane early game metrics, which is only 1 of 5 positions. In our next step, lets engineer additional features—such as the gold difference range across all roles and total kills at 10 minutes—to capture a more comprehensive picture of early game carry potential and improve overall predictive performance.



# Final Model


To improve upon our baseline model, we engineered two new features to capture additional aspects of early game performance:

- **Gold Difference Range:**  
  This feature is the difference between the maximum and minimum gold differences across all roles/positions. It reflects the variability in early game resource advantages within a team, helping to identify if a team has a standout role or if performance is more balanced.

- **Total Kills at 10:**  
  By summing up the kills at 10 minutes across all roles, this feature provides an overall measure of early aggression and impact. This is crucial for evaluating a team's early carry potential.

For our final feature set, we combined these engineered features with our baseline features—**mid_gold** and **mid_killsat10**. The final model is a binary classifier that predicts match outcomes (win or loss) using only early game data, which is available at or before the 10-minute mark.

**Modeling Approach:**  
We used a single sklearn Pipeline that first applies a **StandardScaler** to standardize our features, and then fits a **RandomForestClassifier**. We performed hyperparameter tuning using GridSearchCV, focusing on:
- **n_estimators:** to control the number of trees and stabilize predictions.
- **max_depth:** to balance the complexity and overfitting of each tree.
- **min_samples_split:** to ensure nodes are split only when a sufficient number of samples is available.

The best hyperparameters found were:
- **max_depth:** 5  
- **min_samples_split:** 10  
- **n_estimators:** 100  

**Performance:**

- **Accuracy:** 59.5%  
- **F1 Score:** 63.95%

 While our final model only showed very slight improvements over the baseline, this outcome provides valuable insights. It indicates that simply adding these two engineered features may not be sufficient to capture the carry potential we’re trying to model. Even if very minimal, there is some slight indicators linked to match outcome and carry potential. The incremental gain highlights the inherent difficulty of predicting match outcomes using only early game data. It emphasizes that while early advantages are important, they are just one piece of a more complex puzzle that unfolds throughout the match. Many other nuanced factors can come into play, like a comeback, strategic champion picks, in-game adaptations, and unforeseen events that are not captured in early performance metrics.



## Fairness Analysis

To evaluate whether our final model performs fairly across different groups, we conducted a permutation test using **precision** as our evaluation metric. We defined our groups as follows:

- **Group A:** Teams from matches whose game IDs start with "LOLTMNT01".
- **Group B:** Teams from all other matches.

**Observed Results:**
- The model’s precision for **Group A** was *61.25%*.
- The model’s precision for **Group B** was *55.77%*.
- The observed difference in precision (Group A - Group B) was approximately *5.48%*.

**Permutation Test:**
We shuffled the group labels 1,000 times and recalculated the precision difference for each permutation. The permutation test resulted in a p-value of *0.187*, meaning there is only about a *18.7%* chance that the observed difference in precision could have occurred by random chance alone.

<iframe
  src="assets/fig9.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


**Conclusion:**
Based on the permutation test, we fail reject the null hypothesis that the model's precision is roughly the same across both groups. This suggests that, in terms of precision, our final model does not appear to perform significantly worse for one group compared to the other. While our model is not perfect, these findings provide a basis for further investigation and refinement, ensuring that the model's performance is equitable across different match contexts.




