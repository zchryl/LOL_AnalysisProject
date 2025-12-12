# Position Analysis In League of Legends

By Zachary Loo (zloo@ucsd.edu)
---
## Abstract
Position Analysis In League of Legends is a data science analysis project conducted at UCSD. It walks through many important stages of a Dataset Analysis, starting with Cleaning and Exploratory Data Analysis, Missingness Assessments, Hypothesis Testing, and constructing a Prediction Model based on the available statistics. This project focuses on the presence of distinguished roles and just how much their statistics define them in the playing field. 

--- 

## Introduction
League of Legends, abbreviated as LOL, is an online multiplayer teamwork player versus player (PVP) game that falls under the category of MOBA (Multiplayer Online Battle Arena). League is developed by Riot Games and originally released in 2009. As of 2025, the game still has an active playerbase numbering in the tens of millions. As of 2025, the lowest concurrent playercount its had is 10 Million. Since its release, it has garnered one, if not the largest, presence in the Esport field of entertainment - multiplayer competitive games with professional players. 
<br>
<br>
This analysis uses **Oracle's Elixir** dataset of professional match data for League of Legends in 2025. Included in this file are many statistics from individual matches and each match's individual players from Esport matches throughout 2025. 
<br>
<br>
This dataset captures essentially all of the numerical and recordable data that can be obtained from LOL matches, providing both information about teams as a whole and individual player statistics, which will prove to be very deterministic about what role that player took on in a match. An essential dynamic to MOBA's is the presence of role based performances. Instead of other genres of games where the performance of a player can be quite similar across the board, asides from executing team formations or decisions, LOL's role dynamic leads to each player on a team fulfilling a very defined position. While some roles share similarities, such as Mid Laners and ADC Bot roles eventually scaling their power to be PVP focused, others such as Jungle are encouraged to damage monsters outside of the game's 3 main lanes. At a basic level, there are positions that play more passively than others, differentiating the "carries," those entrusted to lead the team by power, and the supports. However, stats often overlap in importance. All roles have some involvement and expectation to engage in PVP against the enemy team. For this reason, stats like `kills`, `deaths`, `assists` can vary across game to game from player to player. <br>
<br>
For this analysis, the central question I am interested in is <b>Just how defined are the positions in League of Legends by stats?</b> To explore this question, I isolate many crucial statistics that define a player's performance as well as differentiate them from others. Exploring this question requires us to understand the patterns in individual stats and their correlations to others. In the conclusion of this analysis, I construct a predictive model that attempts to classify a player's position by just their match statistics. The performance of this Prediction Model may or may not support the idea that there actually are correlations between positions and statistics.
<br>
<br>
 
### Introduction of Columns
This dataset captures essentially every numerical and some categorical metrics of players and teams throughout 2025. Because 2025 has not yet concluded at the time of conducting this analysis, it should be recognized that this dataset is not entirely complete. At the time of this project, the dataset has 118932 rows of data. For this analysis, here are some of the rows that are important to defining an individual player’s performance. It should be recognized that while this is a large quantity of columns, later analyses in the project reduce the amount of columns used or focus on correlations between just two columns. Isolating this columns now highlights what I felt to be important statistics of individual players or were important to the data cleaning and analysis process, such as gameid and league which do not reflect a player’s performance.
<br>
<br>
* `gameid` : This column captures each match’s unique identifier in which teams played against each other. While this column does not tell us about a player’s performance, it does help us organize the data by matches during analysis
<br>
<br>
* `league`: This column captures an overarching league that a match was a part of. It is not exactly critical for player performance, but allows filtering and organization during the analysis if needed
<br>
<br>
* `side`: This column indicates the team of a player in their specific match. For this dataset, ‘blue’ and ‘red’ are the identifiers of which team a player is on in a given match. This is not the same as the player’s official team affiliation, but rather the match specific designation
<br>
<br>
* `position`: This column indicates the specific coined role of a player on the team. For players, cells in this column contain the designations ‘top’ for Top Laner, ‘jng’ for Jungle, ‘mid’ for Mid Laner, ‘bot’ for Bot Lane or ADC, ‘sup’ for Support positions. In this column are also a position called ‘team’ which indicates a row is the statistics of the team - there are specific statistics in LOL that describe the team as a whole, separate from individual players
<br>
<br>
* `kills`: This column quantifies how many times a player defeated another player’s champion. A kill is defined as getting the final hit on an enemy champion
<br>
<br>
* `deaths`: This column quantifies how many times a player is defeated in combat, which does not have to be specifically by another player
<br>
<br>
* `assists`: This column quantifies how many times a player dealt damage to another champion only for that champion to get defeated by someone else. This recognizes inherently the number of engagements where this player contributed to the defeat of an enemy player
<br>
<br>
* `teamkills`: This column quantifies how many total `kills` a team got in a match. This indicator captures the team’s kill count as a whole
<br>
<br>
* `damageshare`: This column represents a proportion of total damage to enemy champions that a player dealt on their team. In other words, each value represents the individual player’s damage to enemies divided by the team’s overall damage to the enemy team.
<br>
<br>
* `damagetotowers`: This column captures individual player’s flat damage output against the enemy team’s towers. In LOL, towers are essentially checkpoints that defend a team’s central base, and are necessary to be destroyed for players and their team’s minions to advance in the playing field. This column describes each player’s damage contribution against enemy towers
<br>
<br>
* `totalgold`: This column quantifies how much in-match currency a player accumulated. Gold is received for basically every action a player can make in-game, so overall its a very general, but non descript quantifier of performance 
<br>
<br>
* `minionkills`: This column quantifies how many minions a player defeats. In LOL, a minion is a bot like character that is automated by the game to approach towers down the lanes of the battlefield. Minions of the enemy team are able to be defeated to let your own minions advance. This metric specifically captures a player’s involvement in defeating minions, and can be indicative of where in the map these players are positioned. 
<br>
<br>
* `monsterkills`: This is the number of monsters that a player defeats throughout a game. Similar to minions, monsters are automatically spawning and computer controlled characters that spawn on the map in the jungles for example. Monsters range from small camps to larger ones such as Baron Nashor or the Dragon. Monsters are considered neutral objectives, because technically they are outside of the main objective which is lane control. Therefore, monster `kills` can help indicate a player’s contribution to controlling the map outside of the main lanes. 
<br>
<br>
* `damagetochampions`: This statistic is specific to each player and quantifies how much flat damage they have dealt to enemy champions. This can be considered the raw form of damageshare, where this instead describes the total damage a player has dealt to other players, which can be indicative of their role’s contribution to damaging enemy champions.
<br>
<br>
* `dpm`: the rate at which a player is dealing damage to enemy champions over the duration of the game
<br>
<br>
* `firstblood`: column that contains values 1 and 0. 1 if the player gets the first blood - the first kill - or contributes to the first kill in the match
<br>
<br>
* `firstbloodkill`: this column is a more specific indicator of if a player explicitly got the final hit on the first kill in a match 

---


## Data Cleaning and Exploratory Data Analysis
Moving forward, I kept only a subset of the original Dataset by creating a dataframe with only the columns above. An additional step to clean this dataset for the focus of this analysis was to remove the rows in which the `position` column was equal to team. Because my analysis focused on the relationship of individual players statistics to their position in each match, having a row that summarizes team specific qualities is unnecessary for this project. The removal of these rows was done with a mask, keeping only rows of the original dataset where `position` was not equal to `team`
<br>
<br>
Doing this reduced the size of the dataset from **118932 rows** to **99110 rows**, indicating that we now had a dataset of **99110 player statistics**
<br>
<br>
Some of these columns, such as `firstblood` contained **NaN** values, but these discrepancies were kept and had unnoticeable impact on the analyses down the line. From here on, the dataframe here contains all of the information that will be used in the coming steps of the analysis. Some rows are added and removed per stage, such as the Hypothesis Test and Prediction Model. The added or transformed rows pertain specifically to the step at the time, and had no relationship to each other. 
<br>
<br>
Below is the head of the filtered dataframe.

| gameid           | league   | side   | position   |   kills |   deaths |   assists |   teamkills |   damageshare |   damagetotowers |   totalgold |   minionkills |   monsterkills | damagetochampions |     dpm |   firstblood |   firstbloodkill |
|:-----------------|:---------|:-------|:-----------|--------:|---------:|----------:|------------:|--------------:|-----------------:|------------:|--------------:|---------------:|--------------------:|--------:|-------------:|-----------------:|
| LOLTMNT03_179647 | LFL2     | Blue   | top        |       1 |        2 |         1 |           3 |     0.40197   |             7451 |       10668 |           234 |              0 |               20156 | 759.648 |            0 |                0 |
| LOLTMNT03_179647 | LFL2     | Blue   | jng        |       0 |        3 |         1 |           3 |     0.0989769 |                0 |        7429 |            11 |            132 |                4963 | 187.048 |            0 |                0 |
| LOLTMNT03_179647 | LFL2     | Blue   | mid        |       1 |        2 |         0 |           3 |     0.278244  |                0 |        9032 |           209 |              0 |               13952 | 525.829 |            0 |                0 |
| LOLTMNT03_179647 | LFL2     | Blue   | bot        |       1 |        3 |         1 |           3 |     0.137567  |              333 |        9407 |           239 |             12 |                6898 | 259.975 |            0 |                0 |
| LOLTMNT03_179647 | LFL2     | Blue   | sup        |       0 |        3 |         2 |           3 |     0.0832419 |                0 |        5719 |            38 |              0 |                4174 | 157.312 |            0 |                0 |

<br>
<br>

### Univariate Analysis
I performed univariate analysis first on individual player’s `kills` in each match by plotting a histogram.
<br>

<iframe
 src = "assets/univariate_kills.html"
 width = "800"
 height = "450"
 frameborder = "0"
></iframe>

The graph is unimodal but exhibits a heavy right skew, a possible indication that `kills` are not evenly spread out across players. With a high frequency for low amounts of kills may indicate that there is likely an underlying reason why kills tend to be low, but not much more can be said with this observation alone.
<br>
<br>
Next, I performed a similar univariate analysis on Player’s DPM across all games by plotting the observed values on a histogram. 
<br>

<iframe
 src = "assets/univariate_dpm.html"
 width = "800"
 height = "450"
 frameborder = "0" 
></iframe>

Here, the distribution of dpm is more normal, but still skewed right. The closer appearance to normality does indicate that, while players tend to get less kills, their DPM is more evenly distributed across the board. This leads me to speculate: while players tend to get less kills in their matches, they still contribute evenly to engagements with enemy champions. 
<br>
<br>

### Bivariate Analysis
I used a bivariate analysis scatterplot to visualize the relationship between `kills` and `damage per minute`. By knowing that DPM is a metric about only damage to enemy champions, we can suspect there is a positive correlation
<br>

<iframe
 src = "assets/bi_kills_dpm.html"
 width = "800"
 height = "450"
 frameborder = "0" 
></iframe>

As expected, this graph demonstrates a rather strong correlation between these two statistics - that players with a high amount of kills had a high damage per minute. Some players who had high kills had low DPM, which in context might mean they made a lot of the final hits on enemies or committed only to fights they could win - these are just some of many possible reasons a player can get a lower DPM but high amount of kills
<br>
<br>
I performed bivariate analysis on the amount of player `kills` and `monster kills` using a scatter plot.
<br>

<iframe
 src = "assets/bi_kills_monster.html"
 width = "800"
 height = "450"
 frameborder = "0" 
></iframe>

From this graph, I found it interesting there existed two almost separate clusters of points. As seen above, a large cluster of points have less than 50 monster kills while having some of the most player kills, meanwhile many had monster kills above 100 but much less player kills than the prior cluster. This could indicate that `monster kills` and `player kills` are distinctive stats depending on the player, and perhaps role is an underlying reason why these clusters might exist.
<br>
<br>

### Aggregate Statistics
A common way to observe the statistics of players is by aggregating every player’s stats when they played a specific position. Below is a table representing the aggregation of stats based on what position the player played.

| position   |   kills |   deaths |   assists |   firstblood |   firstbloodkill |         dpm |   damagetotowers |   totalgold |      minionkills |     monsterkills |   damagetochampions |
|:-----------|--------:|---------:|----------:|-------------:|-----------------:|------------:|-----------------:|------------:|-----------------:|-----------------:|--------------------:|
| bot        |   97705 |    54732 |    120992 |         4452 |             2630 | 1.48155e+07 |      8.74836e+07 | 2.79558e+08 |      5.52484e+06 | 225535           |         4.82951e+08 |
| jng        |   67705 |    67122 |    160623 |         5955 |             2927 | 9.16001e+06 |      3.70793e+07 | 2.32486e+08 | 438220           |      3.57988e+06 |         2.94619e+08 |
| mid        |   78176 |    59482 |    133398 |         3540 |             1829 | 1.40537e+07 |      7.27695e+07 | 2.60549e+08 |      5.18706e+06 | 118977           |         4.54773e+08 |
| sup        |   17448 |    77737 |    218467 |         5412 |              951 | 4.35147e+06 |      1.51483e+07 | 1.59417e+08 | 641919           |   5825           |         1.38745e+08 |
| top        |   62012 |    64725 |    111483 |         2630 |             1573 | 1.22767e+07 |      8.4055e+07  | 2.46019e+08 |      4.77908e+06 | 105932           |         3.95402e+08 |

<br>
<br>
By taking the **sum** of all stats across every position using a groupby() followed by a sum aggregation function, we can see some significant differences in the overall stats of each position. One for example, is that the `support` position has a significantly lower amount of kills compared to every other position, but a significantly higher amount of `assists`. Support also has a staggeringly low amount of monster `kills`, less than 6000 when the jungle position has nearly 3.6 million!
<br>
<br>
The many observations to be made from this table make it likely evidence that positions do have a significantly different playstyles, and thus stats, from each other.
<br>

---

## Assessment of Missingness
### NMAR Analysis
The most likely NMAR columns in the original dataset are the `ban1`, `ban2`, `ban3`, `ban4`, and `ban5`. A ban is a team's decision to "ban", or prevent a specific champion from being played that game, and is a strategic decision that changes the dynamic of team compositions. In the dataset, it seems very random whenever missing values appear in any of these 5 ban columns. Bans are not tied to any other specific column, because they are an independent decision made by the whole team. There seem to be no other columns that could describe why a whole team decides *not* to ban a character in each ban phase. Contextually, this missingness can reflect the decision to *not* ban a character, which is just as strategic as it is *to* ban a character. This missingness could reflect the team's decision to play risky or unusual - to provoke their competitors, create unpredictability, or mix up their approach to changing the dynamic of the game. Banning a champion reduces the possibilities of team compositions - the possible pairings of champions that could appear. NOT banning a champion retains a greater amount of possible pairings, which could add pressure to the decision making process of which champions to use. Because it seems these columns are not affected by any others, as well as how independent ban decisions are in the context of professional games, I believe these columns’ missingness is NMAR, and dependent solely on the columns themselves.
<br>
<br>

### Missingness Dependency
While the missing values of a column can be explained entirely by the column themselves - the definition of “Not Missing At Random” - the missingness of a column(s) can also be explained by other columns. To demonstrate this, I explored the missingness of the ‘damageshare’ column. The two other columns we will compare to ‘damageshare’ are ‘position’ and ‘teamname.’ From the way this dataset was designed, it is not explicitly stated that ‘damageshare’ values only accompany in-game ‘positions,’ but it is evident that the missingness of ‘damageshare’ can be explained by the values in ‘position’
<br>
<br>
For both of these tests, I chose to conduct a permutation test with a significance level of 0.05 using the TVD as the test statistic, which measures the difference between two distributions of proportions. 
<br>
<br>
For the first comparison, we look at the columns of `position` and `damageshare`
<br>
<br>
**Null Hypothesis:** The distribution of position when `damageshare` is missing is the same as the distribution of position when `damageshare` is not missing
<br>
<br>
**Alternative Hypothesis:** the distribution of position when `damageshare` is missing is **NOT** the same as the distribution of position when `damageshare` is not missing
<br>
<br>
**Test Statistic:** TVD between distribution of position when `damageshare` is missing and not missing
<br>
<br>
Below is the observed distribution of `position` when cells in `damageshare` are and are not missing

|      |   dmgs_missing = True |   dmgs_missing = False |
|:-----|----------------------:|-----------------------:|
| top  |                   0.2 |                      0 |
| jng  |                   0.2 |                      0 |
| mid  |                   0.2 |                      0 |
| bot  |                   0.2 |                      0 |
| sup  |                   0.2 |                      0 |
| team |                   0   |                      1 |

<br>
After calculating the TVD between these two distributions, we get an observed TVD of 1.0. After conducting permutation tests in which missing cells were randomized based on position, under the null that the distribution of the missing values IS NOT related to role, and plotting the histogram of the test TVDs, the observed **p-value** is 0. Below is the empirical distribution of the generated test TVDs under the null.
<br>

<iframe
 src = "assets/dmg_perm.html"
 width = "800"
 height = "450"
 frameborder = "0" 
></iframe>

Because the **p-value** is less than the 0.05 significance level, I reject the null hypothesis and favor the alternative. In other words, because the distribution of position when damageshare IS missing IS different from the distribution of position when damageshare IS NOT missing, the missingness in the `damageshare` column depends on the `position` column. 
<br>
<br>
The second permutation test I performed is on the `damageshare` and `teamname` column. From this test, it will be revealed that the missingness in `damageshare` is not dependent on the `teamname` column.
 <br>
<br>
**Null Hypothesis:** The distribution of `teamname` when `damageshare` is missing is the same as the distribution of `teamname` when `damageshare` is not missing
<br>
<br>
**Alternative Hypothesis:** The distribution of `teamname` when `damageshare` is missing is NOT the same as the distribution of `teamname` when `damageshare` is not missing
<br>
<br>
**Test Statistic:** TVD between distribution of teamname when `damageshare` is missing and not missing
<br>
<br>
Below are the two distributions of values in `teamname` when `damageshare` is missing. To reduce clutter, only the first 10 team names are shown. 

|                         |   dmgs_missing = True |   dmgs_missing = False |
|:------------------------|----------------------:|-----------------------:|
| IziDream                |            0.00317829 |             0.00317829 |
| Team Valiant            |            0.00232065 |             0.00232065 |
| Esprit Shōnen           |            0.00383412 |             0.00383412 |
| Skillcamp               |            0.00262335 |             0.00262335 |
| Karmine Corp Blue Stars |            0.00211886 |             0.00211886 |
| Project Conquerors      |            0.00373323 |             0.00373323 |
| Zerance                 |            0.0023711  |             0.0023711  |
| Lille Esport            |            0.00206841 |             0.00206841 |
| Weibo Gaming            |            0.00756735 |             0.00756735 |
| Oh My God               |            0.00161437 |             0.00161437 |

<br>

After producing 3000 test statistics of permutations, the observed tvd of 0.0 has a **p-value** of 1.0, meaning that all test TVDs were actually greater than the actual TVD! Below is the distribution of the test TVDs compared to the observed TVD.
<br>

<iframe
 src = "assets/teamname_perm.html"
 width = "800"
 height = "450"
 frameborder = "0" 
></iframe>

Because the **p-value** of the observed statistic is > 0.05, we fail to reject the null hypothesis: that the distributions of `teamname` when `damageshare` is missing is the same as when `damageshare` is not missing. This is strong evidence to conclude that the missing values of `damageshare` are not dependent on the teamname column.

---
 
## Hypothesis Testing

An important part of analyzing datasets for patterns is the ability to conduct hypothesis tests to validate claims about the data. For this section of the analysis, I will conduct a Hypothesis Test on an aggregate statistic - kill death assist ratio (KDA). `KDA` is a metric calculated by looking at a player’s specific (`kills` + `assists`) / `deaths` in a match. KDA is an important metric because it represents an individual player’s involvement in PVP combat in a specific match. This is especially important because League is a team based game where one’s combat performance against the other team’s players is a critical aspect to ensuring victory. 
<br>
<br>
You may wonder why this metric has `deaths` as its denominator. That is because `deaths` very punishingly sets a player back a great deal of time, as respawning has a long delay. Deaths hold back players from progressing, making it an important task for players to defeat other players. Setting enemies back time can place them at a great disadvantage in a game where it’s a constant race to scale your champion’s power.
<br>
<br>
Here are the first 10 rows of the Dataframe used in this test to demonstrate that KDA column was added

| gameid           | position   | side   |   kills |   assists |   deaths |       kda |
|:-----------------|:-----------|:-------|--------:|----------:|---------:|----------:|
| LOLTMNT03_179647 | top        | Blue   |       1 |         1 |        2 |  1        |
| LOLTMNT03_179647 | jng        | Blue   |       0 |         1 |        3 |  0.333333 |
| LOLTMNT03_179647 | mid        | Blue   |       1 |         0 |        2 |  0.5      |
| LOLTMNT03_179647 | bot        | Blue   |       1 |         1 |        3 |  0.666667 |
| LOLTMNT03_179647 | sup        | Blue   |       0 |         2 |        3 |  0.666667 |
| LOLTMNT03_179647 | top        | Red    |       3 |         4 |        2 |  3.5      |
| LOLTMNT03_179647 | jng        | Red    |       1 |        10 |        1 | 11        |
| LOLTMNT03_179647 | mid        | Red    |       1 |         8 |        1 |  9        |
| LOLTMNT03_179647 | bot        | Red    |       8 |         3 |        1 | 11        |
| LOLTMNT03_179647 | sup        | Red    |       0 |        11 |        1 | 11        |


<br>
Here I must preface that KDA is not an overall metric of performance in League. Rather, KDA is a metric that captures mainly a player’s combat performance against the enemy players. It is important to measure this because the game builds itself around player versus player combat. Unfortunately this stat is not deterministic of a player’s overall performance because the roles / positions turn out to have such a significant impact on a playstyle and thus one’s stats - that many other metrics have just as much importance in determining what role a player is.
<br>
<br>
For my Hypothesis test, my test statistic was the signed difference in proportion (Mid - Bot) with a significance level of 0.05. These are the two hypotheses that I used:
<br>
<br>
**Null Hypothesis:** The proportion of times that "Bot" position has the highest KDA on their team per game is equal to the proportion of times that "Mid" position has the highest KDA on their team, regardless of winning or losing
<br>
<br>
**Alternative Hypothesis:** ADC positions more often have the highest KDAs than do Mid Laners on their teams.
<br>
<br>
**Test Statistic:** Different in Proportion 
<br>
<br>
Below is the plot for the empirical distribution of test statistics:

<iframe
 src = "assets/hyp_kda_carry.html"
 width = "800"
 height = "450"
 frameborder = "0" 
></iframe>

With my observed value having a **p-value** of 0.0514, which is < 0.05, fail to reject the Null Hypothesis 
<br>
<br>
While I took samples of the 50/50 with the size equivalent to the number of times that Mid and Bot positions carried their team’s KDA. I took 100,000 of these samples, so by the Law of Large Numbers, the empirical distribution would more accurately represent the true distribution of proportion differences.
<br>
<br>
For clarity, I approached this hypothesis as: given the Bot position and Mid position, observe how many times one of these two positions had the highest KDA on their team in a match, whether their team won or lost. This is because “carry” is an ambiguous term that is applied relative to teams, not the overall game. For this hypothesis, I established this as my definition of “carry.” In other words, I looked at the amount of times either a Mid or Bot position “carried” their team’s KDA in a match. Seeing the amount of times this happened, I then looked at the proportion that Mid and Bot made up these occurrences. Because Mid positions made up a greater percentage of times when either of these two positions carried by KDA, the alternative hypothesis would claim that Mid positions carried a greater proportion of times than Mid positions. 
<br>
<br>
Looking at just this subsample of either Mid or Bot positions carrying, under the null, the amount of times these two groups appeared total would remain constant. Under the null hypothesis however, these two groups would appear, or carry, the same proportion of times, so an equal 0.5 split between both groups. 
<br>
<br>
The **p-value** of this hypothesis test heavily depended on the number of test statistics calculated, so leaning on the more extreme amount would better represent the true distribution under the null. From these findings, I am inclined to believe that both Bot and Mid positions carry their teams by KDA - the same proportion of times. We can only be inclined to believe, and cannot absolutely conclude this.  

---


## Framing a Prediction Problem

Given datasets this extensive, we can construct a prediction problem to train a machine learning model to attempt to answer. Using predictive models in these instances can provide us further evidence patterns exist.
<br>
<br>
For this analysis, my prediction problem is: **"What role is a player based on their post game statistics?"** This is a multiclass classification situation which will predict `position` as its response variable. I chose this prediction problem because I am interested in determining whether or not LOL post game statistics, at least what statistics are provided, are deterministic enough of a player’s position. In other words, how defined are the playstyles of each position just by each player’s statistics? For this prediction problem I choose accuracy as the main metric of evaluation, because each position appears an almost equal amount in the dataset once I removed rows for `team` positions (as I am looking solely at player positions). Nevertheless, I will also provide F1-scores to highlight individual performances of the prediction model on each class.
<br>
<br>
When constructing prediction models on datasets like this, it is important to address the time in which you would be making predictions, also known as the “time of prediction.” When predicting some things, there are columns of data you should not use to predict off of because those statistics would only be available after your “time of prediction.” For this circumstance, because my prediction problem happens explicitly after each match and looks backwards after all of the data is obtained, my “time of prediction” is technically after all of this information is already collected. In other words, there are no columns in the dataset that can come after I make my prediction, because I am basing my prediction completely on data that has already been documented. 

--- 

## Baseline Model 

For my baseline model, I decided to use a DecisionTreeClassifier on the following features: `kills`, `deaths`, `assists`, `teamkills`, `firstblood`, `firstbloodkill`, `dpm`, `damagetowers`, `totalgold`, `minionkills`, `monsterkills`, and `damagetochampions`. 
<br>
<br>
Below is the filtered DataFrame of just the feature columns that I used for prediction. The position column was saved as a separate series as it was the target column to predict.

|    |   kills |   deaths |   assists |   teamkills |   firstblood |   firstbloodkill |     dpm |   damagetotowers |   totalgold |   minionkills |   monsterkills |   damagetochampions |
|---:|--------:|---------:|----------:|------------:|-------------:|-----------------:|--------:|-----------------:|------------:|--------------:|---------------:|--------------------:|
|  0 |       1 |        2 |         1 |           3 |            0 |                0 | 759.648 |             7451 |       10668 |           234 |              0 |               20156 |
|  1 |       0 |        3 |         1 |           3 |            0 |                0 | 187.048 |                0 |        7429 |            11 |            132 |                4963 |
|  2 |       1 |        2 |         0 |           3 |            0 |                0 | 525.829 |                0 |        9032 |           209 |              0 |               13952 |
|  3 |       1 |        3 |         1 |           3 |            0 |                0 | 259.975 |              333 |        9407 |           239 |             12 |                6898 |
|  4 |       0 |        3 |         2 |           3 |            0 |                0 | 157.312 |                0 |        5719 |            38 |              0 |                4174 |

<br>

At this stage of the modeling process, it is important to identify the quantitative and qualitative features. For this specific subset of columns, all but one feature are quantitative. firstbloodkill is a nominal categorical feature, but the dataset already converted this feature with `one-hot encoding`, making its values either 1 or 0. Therefore, no further encoding on this column is necessary. All quantitative features have an inherent ordinal nature to them, because each numerical value’s size is correlated to greater importance. For example, 15 in the `kills` column has more importance or ranking than a value of 5. Likewise, 15 deaths is numerically greater than 5 deaths.
<br>
<br>
For this baseline model, I applied a `StandardScaler` on the `kills`, `assists`, and `deaths` column because each of these statistics range in values depending on other columns (that are not part of these hand-picked features), such as match length. Longer matches naturally allow more time for these particular columns to accumulate larger values, so standardizing felt necessary in order to normalize the scale of these features. 
<br>
<br>
After fitting this pipeline to the training data, it returns an accuracy score of **68.474%** on the training data and **68.456%** on the test data. This small dip in accuracy is always expected because the model does not train on the test data. To further analyze this baseline model, I examined the F1-Score of each individual class because of this problem’s multiclass nature. When comparing the predictions of the test data with the actual test classes. When checking the F1-Scores of the baseline model, in the order of `bot`, `jng`, `mid`, `sup`, `top`, the scores are `0.52, 1.0, 0.43, 1.0, 0.47`.
<br>
<br>
From these initial scores, it is possible to say the model is “passable.” It accurately predicts greater than 50% of the time, but the F1-scores tell us the model struggles most differentiating the `bot`, `mid`, and `top` positions. Meanwhile, it has maximum F1-score for `jng` and `sup`. What stands out the most from this current model is that the training accuracy is **68.474%**, when Decision Trees often overfit. In tandem with a low test accuracy, I am led to believe that this baseline model is actually **underfitting** the data. Therefore, there is some room for improvement.
---

## Final Model
To attempt to improve the accuracy of the model, I next encoded the `minionkills` and `monsterkills` feature with a `StandardScaler` on top of the original `kills`, `deaths`, and `assists`, features. I did this because, just like `kills`, `deaths`, and `assists`, the values of `minionkills` and `monsterskills` may vary depending on factors like the length of the game. Standardizing these features would likely place them on an easier scale for readability. Additionally, I also encoded `damagetochampions` and `damagetotowers` with a `QuantileTransformer`, because these features ranged in the thousands between player to player. Instead of having these large differences, encoding each value to instead be on a Distribution’s CDF, much like standardizing, would change their units to be much smaller and instead now relative to each other. 
<br>
<br>
Using a `QuantileTransformer` would remove a great deal of the variability in the two damage columns with a non-linear transformation. These features would now be placed on a much more relative scale between its values, whereas before they would be vastly different.
<br>
<br>
For this improved model, I chose to train a **RandomForestClassifier**, searching for the best hyperparameters with **GridSearchCV**.  Using a Grid Search, I trained models with a max_depth from 0 to 200 at increments of 10 as well as n_estimators from 100 to 200 at increments of 30. From this search, the best parameters identified were `max_depth = 160` and `n_estimators = 190.` In other words, 190 decision trees with a depth of 160 each - was found to be the best parameters.
<br>
<br>
Evaluating this model now, it yielded an accuracy of **100%** (1.0) on the training data, and an accuracy of **74.206%** (0.742069) on the test data. Looking at the individual F1-Scores, the model now scored `0.6, 1.0, 0.47, 1.0, 0.63`. Thus, this new model had about a **6%** increase in accuracy over the baseline on the test data. The 100% accuracy on the training data is a positive sign as well, because we expect having that many decision trees in a Random Forest to overfit the data. The F1-scores for `bot`, `mid`, and `top` positions also improved, which is a positive sign, although still rather low. 
<br>
<br>
Something I recognized about this model is that the ‘best’ chosen n_estimators hyperparameter was the largest value I allowed it to be. 100 to 200 with increments of 30 has a max value of 190, which is what the Grid Search concluded as the best parameter. What this means, given the still low F1-Scores, is that the model could likely improve by increasing the number of estimators - even more. On the other hand, the depth of each estimator was much less than the provided maximum it could have been, so this is a good sign that 160 is the sweet spot for this dataset. 
<br>
<br>
A possibility that may explain why the F1-Scores are still low while the RandomForestClassifier is overfitting the training data is that the current features being predicted off of are not sufficient enough to distinguish the differences between the `bot`, `mid`, and `top` positions. Meanwhile, `jng` and `sup` positions are being classified at 100% accuracy, which likely indicates the current features are enough to distinguish these two positions' playstyles from the others! In a future improvement step to this model, including more defining features will likely make the F1-scores greater. 

---

## Fairness Analysis
When creating a prediction model, it is important to address how different groups yield different performances from the model. In this section, I will assess if my model is fair among two specific groups in the dataset. For this Fairness Analysis, I asked the question: **does my model perform worse for players who have less than or equal to 10 kills than it does for players who have more than 10 kills?** To answer this question, I used a **permutation test** with a significance value level of 0.05.
<br>
<br>
The group x represents the players who have less than or equal to 10 kills, and group y represents players with more than 10 kills in a match. The evaluation metric compared between these two groups is accuracy subtracting group `x’s` accuracy from group `y`.
<br>
<br>
The Hypotheses for this test are:
<br>
<br>
**Null Hypothesis:** The accuracy of the model for player positions when that player had less than or equal to 10 kills in a match is equal to the accuracy of the model for players who had over 10 kills in a match
<br>
<br>
**Alternative Hypothesis:** The accuracy of the model for player positions when that player had less than or equal to 10 kills in a match is LESS THAN to the accuracy of the model for players who had over 10 kills in a match
<br>
<br>
**Test Statistic:** difference in accuracy between players who have less than or equal to 10 kills and players who have over 10 kills

<iframe
 src = "assets/fairness.html"
 width = "800"
 height = "450"
 frameborder = "0" 
></iframe>

From the permutation test, the observed value has a **p-value** of 0.265, which is much greater than the significance level of 0.05. Thus, I fail to reject the null hypothesis and am inclined to believe that this model does not perform more accurately for players with greater than 10 kills. Maybe kills are not as important of a metric after all! Failing to reject the null in this test is a good sign because that indicates the model is fair regarding groups derived from the kills feature. Further tests can be conducted to identify if this model is fair in the other feature categories.
