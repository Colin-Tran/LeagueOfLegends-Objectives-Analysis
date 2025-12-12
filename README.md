# League of Legends Analysis: A look into Objective Control and Match Outcomes

## Introduction
League of Legends (LoL / LOL) is a wildly popular multiplayer online battle arena (MOBA) video game that was released in October 2009 by Riot Games. Since its release, it has grown into one of the largest video games in the world with millions of total players. With its growth has come multiple professional leagues worldwide, blooming the League of Legends Esports scene into one of the largest and most competitive esports ecosystems in the world.

Within League of Legends itself there is so much capturable in game data. Each game produces detailed statistics such as kills, gold earned, vision control, early game advantages, objective control, and overall match outcomes. The dataset used in this project comes from Oracle’s Elixir, a widely used public source for professional League of Legends statistics. Specifically, this analysis focuses on data from the 2025 competitive season, which captures comprehensive in game information across professional matches played in the LOL 2025 season.

This dataset compiles data from over 40 different competitive leagues worldwide, comprising of over 118932 rows and 164 distinct columns. It captures a wide range of in game statistics and match outcomes from professional League of Legends matches, including objectives, combat performance, vision metrics, economy statistics, and overall results. The scale and depth of this dataset make it well suited for analyzing player behaviors, team strategy, gameplay trends, overall dynamics, and so much more.

In the context of League of Legends, objectives play a central role in shaping the flow and outcome of a match. Objectives are neutral map features that teams can contest and secure to gain strategic advantages. These include epic monsters such as Dragons, Rift Heralds, Void Grubs, Barons, and Elder Dragons, as well as structural objectives like towers, turret plates, and inhibitors. Each objective provides lasting buffs, map pressure, or economic advantages that directly influence a team’s ability to control the game.

Beyond immediate rewards, objectives determine a team’s tempo and win conditions. Securing early objectives can establish momentum and map control, while late game objectives such as Baron and Elder Dragon often serve as decisive turning points that enable teams to close out matches because of the buffs and tempo they give. Objectives impact vision control, gold distribution, overall strength, and so much more within the game. 

The question I am interested in is:

**How does objective control influence a team’s chances of winning a League of Legends match?** 

In this project, we will analyze professional League of Legends match data using several techniques to find the impact of objectives on match outcomes. Through exploratory data analysis, hypothesis testing, and predictive modeling, we examine how securing objectives such as Dragons, Heralds, Barons, and structural objectives impact a team’s chance of winning.

## Relevant Columns

This project focuses on team level objective control statistics and match outcomes from the 2025 professional League of Legends season. The following columns are used throughout the analysis:

**Match Outcome**

- result: Binary indicator of match outcome (1 = win, 0 = loss)

**Dragon Objectives**

- dragons: Total number of Dragons secured

- opp_dragons: Total Dragons secured by the opposing team

- firstdragon: Whether the team secured the first dragon

- elementaldrakes: Total elemental drakes secured

- opp_elementaldrakes: Elemental drakes secured by the opponent

- infernals: Infernal dragons secured

- mountains: Mountain dragons secured

- clouds: Cloud dragons secured

- oceans: Ocean dragons secured

- chemtechs: Chemtech dragons secured

- hextechs: Hextech dragons secured

- dragons (type unknown): Dragons taken before type classification

- elders: Elder Dragons secured

- opp_elders: Elder Dragons secured by the opponent

**Rift Herald & Void Grubs**

- firstherald: Whether the team secured the first Rift Herald

- heralds: Total Rift Heralds secured

- opp_heralds: Rift Heralds secured by the opponent

- void_grubs: Void Grubs secured

- opp_void_grubs: Void Grubs secured by the opponent

**Baron & Atakhan**

- firstbaron: Whether the team secured the first Baron Nashor

- barons: Total Barons secured

- opp_barons: Barons secured by the opponent

- atakhans: Atakhans secured

- opp_atakhans: Atakhans secured by the opponent

**Structural Objectives**

- firsttower: Whether the team secured the first tower

- firstmidtower: Whether the team secured the first mid-lane tower

- firsttothreetowers: Whether the team was first to destroy three towers

- towers: Total towers destroyed

- opp_towers: Towers destroyed by the opponent

- turretplates: Turret plates destroyed

- opp_turretplates: Turret plates destroyed by the opponent

- inhibitors: Inhibitors destroyed

- opp_inhibitors: Inhibitors destroyed by the opponent

**Contextual Columns**

- gamelength: Total match duration (in seconds)

- datacompleteness: Indicates whether match data is complete or partial or missing completely


## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

#### Filter to Teams
Before we analyzed any of the data we had to clean the raw Oracle's Elixir dataset to esnure the data reflected correct trends. To start we looked at each individual match and the data contained in each match. Each match contains a unique gameid and for each game there were ~12 rows that included one for each of the players as well as one row for each team. Since we are focusing on team objective control, we filtered out for **every** game to ensure that we only contained **team** data. With did this by by filtering for rows where the position column equals "team".

#### Selecting only Relevant Columns
Next, we wanted to select the relevant columns to our analysis. From 164 total unique columns, we cut it down to a total of 36 columns that were relevant to objectives within LOL. This included:
- Epic Monster Objectives (dragons, heralds, void grubs, baron, elder, and atakhan)
- Structural Objectives (towers, inhibiors, turret plates)
- Outcomes / other helpful information (result, game length, data completeness)

#### Converting to all numeric types
Next, we wanted to make sure that all of our data present within the dataframe were converted to numeric types so that we can handle and analyze all of our data without any conflictions within the data types.


#### Handling Missing Values (NaN)
Some objective columns contain missing values. For simplicity sake, we assumed that these missing values indicated that this team didn't secure this objective. Thus, missing entries were replaced with 0. This allowed us to aggregate and analyze match data.


Here is what our cleaned dataset looks like:


|   firstdragon |   dragons |   opp_dragons |   elementaldrakes |   opp_elementaldrakes |   infernals |   mountains |   clouds |   oceans |   chemtechs |   hextechs |   dragons (type unknown) |   elders |   opp_elders |   firstherald |   heralds |   opp_heralds |   void_grubs |   opp_void_grubs |   firstbaron |   barons |   opp_barons |   atakhans |   opp_atakhans |   firsttower |   firstmidtower |   firsttothreetowers |   towers |   opp_towers |   turretplates |   opp_turretplates |   inhibitors |   opp_inhibitors |   result |   datacompleteness |   gamelength |   objective_score |
|--------------:|----------:|--------------:|------------------:|----------------------:|------------:|------------:|---------:|---------:|------------:|-----------:|-------------------------:|---------:|-------------:|--------------:|----------:|--------------:|-------------:|-----------------:|-------------:|---------:|-------------:|-----------:|---------------:|-------------:|----------------:|---------------------:|---------:|-------------:|---------------:|-------------------:|-------------:|-----------------:|---------:|-------------------:|-------------:|------------------:|
|             0 |         0 |             2 |                 0 |                     2 |           0 |           0 |        0 |        0 |           0 |          0 |                        0 |        0 |            0 |             0 |         0 |             1 |            0 |                6 |            0 |        0 |            1 |          0 |              1 |            0 |               0 |                    0 |        3 |            9 |              1 |                  8 |            0 |                2 |        0 |                  0 |         1592 |                 4 |
|             1 |         2 |             0 |                 2 |                     0 |           1 |           0 |        1 |        0 |           0 |          0 |                        0 |        0 |            0 |             1 |         1 |             0 |            6 |                0 |            1 |        1 |            0 |          1 |              0 |            1 |               1 |                    1 |        9 |            3 |              8 |                  1 |            2 |                0 |        1 |                  0 |         1592 |                29 |
|             0 |         3 |             2 |                 3 |                     2 |           0 |           3 |        0 |        0 |           0 |          0 |                        0 |        0 |            0 |             1 |         1 |             0 |            6 |                0 |            1 |        1 |            0 |          1 |              0 |            1 |               1 |                    1 |       11 |            2 |              8 |                  2 |            3 |                0 |        1 |                  0 |         1922 |                33 |
|             1 |         2 |             3 |                 2 |                     3 |           0 |           0 |        0 |        1 |           0 |          1 |                        0 |        0 |            0 |             0 |         0 |             1 |            0 |                6 |            0 |        0 |            1 |          0 |              1 |            0 |               0 |                    0 |        2 |           11 |              2 |                  8 |            0 |                3 |        0 |                  0 |         1922 |                 6 |
|             0 |         0 |             4 |                 0 |                     4 |           0 |           0 |        0 |        0 |           0 |          0 |                        0 |        0 |            0 |             0 |         0 |             1 |            2 |                4 |            0 |        0 |            1 |          0 |              1 |            0 |               1 |                    0 |        3 |           12 |              7 |                 10 |            0 |                3 |        0 |                  0 |         1782 |                12 |

### EDA


#### Univariate Analysis

<iframe
  src="assets/objective_score_distribution.html"
  width="800"
  height="600"
  frameborder="0">
</iframe>

<p>
The objective score distribution summarizes the total number of objectives secured by teams in a single match.
This histogram shows that the distribution of team objective score is relatively normal and most teams cluster around a moderate amount of total objectives. There are very few teams that are able to secure more than 30 total objectives. 
</p>

<iframe
  src="assets/dragons_distribution.html"
  width="800"
  height="600"
  frameborder="0">
</iframe>

<p>
This distribution shows the number of dragons secured by teams in a match.
The distribtuion appears to be relatively normal with most teams securing around 2-3 dragons within a LOL match. Very few teams are able to secure 4 or more dragons.
</p>

<iframe
  src="assets/barons_distribution.html"
  width="800"
  height="600"
  frameborder="0">
</iframe>

<p>
This histogram shows the number of Barons taken by teams in a match.
Most teams secure zero or one Baron, with multiple Barons occurring rarely, highlighting the fact that Baron Nashor
is a late-game objective that typically helps teams decide or conclude games.
</p>

#### Bivariate Analysis
<h3>Objective Score vs Match Result</h3>

<iframe
  src="assets/objective_score_vs_result.html"
  width="800"
  height="300"
  frameborder="0">

  
</iframe>

<p>
This box plot compares the distribution of total objectives secured by teams that won versus teams that lost.
The winning teams generally secure significantly more objectives, highlighting the strong relationship between
objective control and match outcome.
</p>


<h3>Win Rate by Number of Dragons Taken</h3>

<iframe
  src="assets/dragons_vs_winrate.html"
  width="800"
  height="600"
  frameborder="0">
</iframe>

<p>
This bar chart shows how a team’s win rate changes as the number of dragons secured increases.
Teams that secure more dragons tend to have a way higher probability of winning with the win chance plateauing when a team secures 4 dragons (this is when a team secures a soul which provides a significant buff).
</p>


#### Interesting Aggregates

The table below summarizes average objective statistics grouped by whether a team secured **First Dragon**.

|   firstdragon |   result |   dragons |   heralds |   void_grubs |   barons |   towers |   inhibitors |   objective_score |
|--------------:|---------:|----------:|----------:|-------------:|---------:|---------:|-------------:|------------------:|
|             0 | 0.424682 |   1.60597 |  0.488405 |      2.4274  | 0.486993 |  5.50151 |     0.766284 |           14.7609 |
|             1 | 0.575424 |   2.84178 |  0.509491 |      1.79867 | 0.572597 |  6.51313 |     0.97395  |           16.8874 |

This grouped table highlights the impact of early objective control by comparing teams that did and did not secure
First Dragon. Teams that obtained First Dragon show a substantially higher win rate and consistently higher averages
across key objectives such as dragons, barons, towers, and total objective score. This suggests that early objective (such as first dragon) provides a significant advantage, increasing a team’s likelihood of winning.


## Assessment of Missingness

### NMAR Analysis
Not missing at random (NMAR)
Is there a good reason why the missingness depends on the values themselves?


In our data, I believe that these columns can be NMAR:

**Chance for certain drakes to appear within a game**
- infernals, mountains, clouds, oceans, chemtechs, hextechs, dragons (type unknown) 

For the drakes/dragons, each LOL match follows a fixed elemental dragon spawn pattern. The first two dragons are the same element followed by two dragons that are two different elements from the previous ones. This provides a chance at getting 3 different elemental dragons meaning at least 3 of the possible elemental dragons don't spawn every match. Basically, a subset of dragon types are selected each game allowing for the possibility of certain dragon types to be left out. This could account for the missingess within these columns as this missingness depends on the  unobserved elemental configuration of the match, rather than being missing at random. Thus, these elemental dragon columns are NMAR, since whether a value is missing depends on an unobserved mechanism (the elemental dragon configuration), not purely on observed variables / existing columns.

Some data that might help us better understand the missingness would be information about the underlying elemental dragon selection process, such as which dragon elements were eligible to spawn in each match or the predetermined elemental sequence for that game. Having explicit indicators for which elemental dragons were possible would allow us to explain why certain dragon types never appear, potentially converting this missingness from NMAR to MAR.

### Missingness Dependency

In this section, I will test if the missingness of the **firstbaron** column is dependent on other columns. More specifically, we will be looking at the **game length** and **result** columns to see whether or not missingness is dependent on these two. 

First we will perform a permutation test on the **firstbaron** and **game length** columns.

- Null Hypothesis: Distribution of gamelength when firstbaron is missing is the same as the distribution of gamelength when firstblood is not missing.

- Alternative Hypothesis: Distribution of gamelength when firstbaron is missing is **NOT** the same as the distribution of gamelength when firstblood is not missing.

firstbaron missing in 1634 out of 19822 rows (8.24%)

Our statistic to perform this test will be the Total Variance Distance (TVD).

<iframe
  src="assets/firstbaron_missing_vs_gamelength_tvd.html"
  width="800"
  height="600"
  frameborder="0">
</iframe>


Here is the plot showing the distribution of TVDs when permutated using firstbaron missingness versus the gamelength. Overall we found that the 
- Observed TVD (gamelength vs firstbaron_missing): 0.030018447338675973
- Permutation p-value for dependency on gamelength(0-25, 25-35, >35 mins): 0.012

This means that we reject our null hypothesis, meaning that the missingness of firstbaron does depend on game length. This also does match our previous knowledge about the baron spawning process, which is that sometimes the game length results in barons not being spawned at all if a match finished before the spawn timer of 25 minutes. As a result, shorter games are more likely to have missing values for firstbaron, explaining the observed dependency. This is Missing at Random (MAR) because the missingness of our firstbaron column can be explained by the game length column.


- Null Hypothesis: Distribution of result when firstbaron is missing is the same as the distribution of result when firstblood is not missing.

- Alternative Hypothesis: Distribution of result when firstbaron is missing is **NOT** the same as the distribution of result when firstblood is not missing.


<iframe
  src="assets/firstbaron_missing_vs_result_tvd.html"
  width="800"
  height="600"
  frameborder="0">
</iframe>


Here is the plot showing the distribution of TVDs when permutated using firstbaron missingness versus the result. Overall we found that the 
- Observed TVD (result vs firstbaron_missing): 0
- Permutation p-value for dependency on result: 1.0

We fail to reject the null hypothesis, indicating that the missingness of firstbaron does not
depend on match outcome // result. This suggests that missingness is unrelated to whether a team won
or lost, and is instead driven by other factors.


## Hypothesis Testing


## Framing a Prediction Problem

## Baseline Model

## Final Model

## Fairness Analysis