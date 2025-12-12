# Analyzing What Defines each League of Legends Position

By Zachary Loo (zloo@ucsd.edu)
---
## Abstract
Analyzing What Defines each League of Legends Position is a data science project conducted at UCSD. It walks through many important stages of a Dataset Analysis, starting with Cleaning and Exploratory Data Analysis, Missingness Assessments, Hypothesis Testing, and constructing a Prediction Model based on the available statistics. This project focuses on the presence of distinguished roles and just how much their statistics define them in the playing field. 

Author: Zachary Loo
---

## Introduction
League of Legends, abbreviated as LOL, is an online multiplayer teamwork player versus player (PVP) game that falls under the category of MOBA (Multiplayer Online Battle Arena). League is developed by Riot Games and originally released in 2009. As of 2025, the game still has an active playerbase numbering in the tens of millions. As of 2025, the lowest concurrent playercount its had is 10 Million. Since its release, it has garnered one, if not the largest, presence in the Esport field of entertainment - multiplayer competitive games with professional players. 
<br>
<br>
This analysis uses Oracle's Elixir dataset of professional match data for League of Legends in 2025. Included in this file are many statistics from individual matches and each match's individual players from Esport matches throughout 2025. 
<br>
<br>
This dataset captures essentially all of the numerical and recordable data that can be obtained from LOL matches, providing both information about teams as a whole and individual player statistics, which will prove to be very deterministic about what role that player took on in a match. An essential dynamic to MOBA's is the presence of role based performances. Instead of other genres of games where the performance of a player can be quite similar across the board, asides from executing team formations or decisions, LOL's role dynamic leads to each player on a team fulfilling a very defined position. While some roles share similarities, such as Mid Laners and ADC Bot roles eventually scaling their power to be PVP focused, others such as Jungle are encouraged to damage monsters outside of the game's 3 main lanes. At a basic level, there are positions that play more passively than others, differentiating the "carries," those entrusted to lead the team by power, and the supports. However, stats often overlap in importance. All roles have some involvement and expectation to engage in PVP against the enemy team. For this reason, stats like kills, deaths, assists can vary across game to game from player to player. <br>
<br>
For this analysis, the central question I am interested in is <b>Just how defined are the positions in League of Legends by stats?</b> To explore this question, I isolate many crucial statistics that define a player's performance as well as differentiate them from others. Exploring this question requires us to understand the patterns in individual stats and their correlations to others. In the conclusion of this analysis, I construct a predictive model that attempts to classify a player's position by just their match statistics. Constructing this prediction model may or may not support the idea that there actually are correlations between positions and statistics.

<b>Introduction of Columns</b>
<br>
<br>
This dataset captures essentially every numerical and some categorical metrics of players and teams throughout 2025. Because 2025 has not yet concluded at the time of conducting this analysis, it should be recognized that this dataset is not entirely complete. At the time of this project, the dataset has 118932 rows of data. For this analysis, here are some of the rows that are important to defining an individual player’s performance. It should be recognized that while this is a large quantity of columns, later analyses in the project reduce the amount of columns used or focus on correlations between just two columns. Isolating this columns now highlights what I felt to be important statistics of individual players or were important to the data cleaning and analysis process, such as gameid and league which do not reflect a player’s performance.
<br>
<br>
* gameid : This column captures each match’s unique identifier in which teams played against each other. While this column does not tell us about a player’s performance, it does help us organize the data by matches during analysis
<br>
<br>
* league: This column captures an overarching league that a match was a part of. It is not exactly critical for player performance, but allows filtering and organization during the analysis if needed
<br>
<br>
* side: This column indicates the team of a player in their specific match. For this dataset, ‘blue’ and ‘red’ are the identifiers of which team a player is on in a given match. This is not the same as the player’s official team affiliation, but rather the match specific designation
<br>
<br>
* position: This column indicates the specific coined role of a player on the team. For players, cells in this column contain the designations ‘top’ for Top Laner, ‘jng’ for Jungle, ‘mid’ for Mid Laner, ‘bot’ for Bot Lane or ADC, ‘sup’ for Support positions. In this column are also a position called ‘team’ which indicates a row is the statistics of the team - there are specific statistics in LOL that describe the team as a whole, separate from individual players
<br>
<br>
* kills: This column quantifies how many times a player defeated another player’s champion. A kill is defined as getting the final hit on an enemy champion
<br>
<br>
* deaths: This column quantifies how many times a player is defeated in combat, which does not have to be specifically by another player
<br>
<br>
* assists: This column quantifies how many times a player dealt damage to another champion only for that champion to get defeated by someone else. This recognizes inherently the number of engagements where this player contributed to the defeat of an enemy player
<br>
<br>
* teamkills: This column quantifies how many total kills a team got in a match. This indicator captures the team’s kill count as a whole
<br>
<br>
* damageshare: This column represents a proportion of total damage to enemy champions that a player dealt on their team. In other words, each value represents the individual player’s damage to enemies divided by the team’s overall damage to the enemy team.
<br>
<br>
* damagetotowers: This column captures individual player’s flat damage output against the enemy team’s towers. In LOL, towers are essentially checkpoints that defend a team’s central base, and are necessary to be destroyed for players and their team’s minions to advance in the playing field. This column describes each player’s damage contribution against enemy towers
<br>
<br>
* totalgold: This column quantifies how much in-match currency a player accumulated. Gold is received for basically every action a player can make in-game, so overall its a very general, but non descript quantifier of performance 
<br>
<br>
* minionkills: This column quantifies how many minions a player defeats. In LOL, a minion is a bot like character that is automated by the game to approach towers down the lanes of the battlefield. Minions of the enemy team are able to be defeated to let your own minions advance. This metric specifically captures a player’s involvement in defeating minions, and can be indicative of where in the map these players are positioned. 
<br>
<br>
* monsterkills: This is the number of monsters that a player defeats throughout a game. Similar to minions, monsters are automatically spawning and computer controlled characters that spawn on the map in the jungles for example. Monsters range from small camps to larger ones such as Baron Nashor or the Dragon. Monsters are considered neutral objectives, because technically they are outside of the main objective which is lane control. Therefore, monster kills can help indicate a player’s contribution to controlling the map outside of the main lanes. 
<br>
<br>
* damagetochampions: This statistic is specific to each player and quantifies how much flat damage they have dealt to enemy champions. This can be considered the raw form of damageshare, where this instead describes the total damage a player has dealt to other players, which can be indicative of their role’s contribution to damaging enemy champions.
<br>
<br>
* dpm: the rate at which a player is dealing damage to enemy champions over the duration of the game
<br>
<br>
* firstblood: column that contains values 1 and 0. 1 if the player gets the first blood - the first kill - or contributes to the first kill in the match
<br>
<br>
* firstbloodkill: this column is a more specific indicator of if a player explicitly got the final hit on the first kill in a match 
<br>
<br>
---

## Data CLeaning and Exploratory Data Analysis
Moving forward, I kept only a subset of the original Dataset by creating a dataframe with only the columns above. An additional step to clean this dataset for the focus of this analysis was to remove the rows in which the ‘position’ column was equal to team. Because my analysis focused on the relationship of individual players statistics to their position in each match, having a row that summarizes team specific qualities is unnecessary for this project. The removal of these rows was done with a mask, keeping only rows of the original dataset where ‘position’ was not equal to ‘team’

Doing this reduced the size of the dataset from 118932 rows to 99110 rows, indicating that we now had a dataset of 99110 player statistics

Some of these columns, such as ‘firstblood’ contained NaN values, but these discrepancies were kept and had unnoticeable impact on the analyses down the line. From here on, the dataframe here contains all of the information that will be used in the coming steps of the analysis. Some rows are added and removed per stage, such as the Hypothesis Test and Prediction Model. The added or transformed rows pertain specifically to the step at the time, and had no relationship to each other. 

Below is the head of the filtered dataframe.
Univariate Analysis

I performed univariate analysis first on individual player’s ‘kills’ in each match by plotting a histogram.

The graph is unimodal but exhibits a heavy right skew, a possible indication that kills are not evenly spread out across players. With a high frequency for low amounts of kills may indicate that there is likely an underlying reason why kills tend to be low, but not much more can be said with this observation alone.

Next, I performed a similar univariate analysis on Player’s DPM across all games by plotting the observed values on a histogram. 

Here, the distribution of dpm is more normal, but still skewed right. The closer appearance to normality does indicate that, while players tend to get less kills, their DPM is more evenly distributed across the board. This leads me to speculate: while players tend to get less kills in their matches, they still contribute evenly to engagements with enemy champions. 
Bivariate Analysis
I used a bivariate analysis scatterplot to visualize the relationship between ‘kills’ and ‘damage per minute.’ By knowing that DPM is a metric about only damage to enemy champions, we can suspect there is a positive correlation

As expected, this graph demonstrates a rather strong correlation between these two statistics - that players with a high amount of kills had a high damage per minute. Some players who had high kills had low DPM, which in context might mean they made a lot of the final hits on enemies or committed only to fights they could win - these are just some of many possible reasons a player can get a lower DPM but high amount of kills

I performed bivariate analysis on the amount of player kills and monster kills using a scatter plot.

From this graph, I found it interesting there existed two almost separate clusters of points. As seen above, a large cluster of points have less than 50 monster kills while having some of the most player kills, meanwhile many had monster kills above 100 but much less player kills than the prior cluster. This could indicate that monster kills and player kills are distinctive stats depending on the player, and perhaps role is an underlying reason why these clusters might exist.


Aggregate Statistics

A common way to observe the statistics of players is by aggregating every player’s stats when they played a specific position. Below is a table representing the aggregation of stats based on what position the player played.

By taking the sum of all stats across every position using a groupby() followed by a sum aggregation function, we can see some significant differences in the overall stats of each position. One for example, is that the ‘support’ position has a significantly lower amount of kills compared to every other position, but a significantly higher amount of assists. Support also has a staggeringly low amount of monster kills, less than 6000 when the jungle position has nearly 3.6 million!

The many observations to be made from this table make it likely evidence that positions do have a significantly different playstyles, and thus stats, from each other.
<br>

---

## Assessment of Missingness
NMAR Analysis
The most likely NMAR columns in the original dataset are the ban1, ban2, ban3, ban4, and ban5. A ban is a team's decision to "ban", or prevent a specific champion from being played that game, and is a strategic decision that changes the dynamic of team compositions. In the dataset, it seems very random whenever missing values appear in any of these 5 ban columns. Bans are not tied to any other specific column, because they are an independent decision made by the whole team. There seem to be no other columns that could describe why a whole team decides *not* to ban a character in each ban phase. Contextually, this missingness can reflect the decision to *not* ban a character, which is just as strategic as it is *to* ban a character. This missingness could reflect the team's decision to play risky or unusual - to provoke their competitors, create unpredictability, or mix up their approach to changing the dynamic of the game. Banning a champion reduces the possibilities of team compositions - the possible pairings of champions that could appear. NOT banning a champion retains a greater amount of possible pairings, which could add pressure to the decision making process of which champions to use. Because it seems these columns are not affected by any others, as well as how independent ban decisions are in the context of professional games, I believe these columns’ missingness is NMAR, and dependent solely on the columns themselves.

Missingness Dependency

While the missing values of a column can be explained entirely by the column themselves - the definition of “Not Missing At Random” - the missingness of a column(s) can also be explained by other columns. To demonstrate this, I explored the missingness of the ‘damageshare’ column. The two other columns we will compare to ‘damageshare’ are ‘position’ and ‘teamname.’ From the way this dataset was designed, it is not explicitly stated that ‘damageshare’ values only accompany in-game ‘positions,’ but it is evident that the missingness of ‘damageshare’ can be explained by the values in ‘position’

For both of these tests, I chose to conduct a permutation test with a significance level of 0.05 using the TVD as the test statistic, which measures the difference between two distributions of proportions. 

For the first comparison, we look at the columns of ‘position’ and ‘damageshare’

Null Hypothesis: The distribution of position when damageshare is missing is the same as the distribution of position when damageshare is not missing

Alternative Hypothesis: the distribution of position when damageshare is missing is NOT the same as the distribution of position when damageshare is not missing

Test Statistic: TVD between distribution of position when damageshare is missing and not missing

Below is the observed distribution of position when cells in ‘damageshare’ are and are not missing

After calculating the TVD between these two distributions, we get an observed TVD of 1.0. After conducting permutation tests in which missing cells were randomized based on position, under the null that the distribution of the missing values IS NOT related to role, and plotting the histogram of the test TVDs, the observed p-value is 0. Below is the empirical distribution of the generated test TVDs under the null.

because the p-value is less than the 0.05 significance level, I reject the null hypothesis and favor the alternative. In other words, because the distribution of position when damageshare IS missing IS different from the distribution of position when damageshare IS NOT missing, the missingness in the ‘damageshare’ column depends on the ‘position’ column. 

The second permutation test I performed is on the ‘damageshare’ and ‘teamname’ column. From this test, it will be revealed that the missingness in ‘damageshare’ is not dependent on the ‘teamname’ column.
 
Null Hypothesis: The distribution of ‘teamname’ when ‘damageshare’ is missing is the same as the distribution of ‘teamname’ when ‘damageshare’ is not missing

Alternative Hypothesis: The distribution of ‘teamname’ when ‘damageshare’ is missing is NOT the same as the distribution of ‘teamname’ when ‘damageshare’ is not missing

Test Statistic: TVD between distribution of teamname when damageshare is missing and not missing

Below are the two distributions of values in ‘teamname’ when ‘damageshare’ is missing. 

After producing 3000 test statistics of permutations, the observed tvd of 0.0 has a p-value of 1.0, meaning that all test TVDs were actually greater than the actual TVD! Below is the distribution of the test TVDs compared to the observed TVD.

Because the p-value of the observed statistic is > 0.05, we fail to reject the null hypothesis: that the distributions of ‘teamname’ when ‘damageshare’ is missing is the same as when ‘damageshare’ is not missing. This is strong evidence to conclude that the missing values of ‘damageshare’ are not dependent on the teamname column.

---
 
## Hypothesis Testing

---

## Framing a Prediction Problem

--- 

## Baseline Model 

---

## Final Model

---

## Fairness Analysis