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


<iframe
  src="assets/teams_clean_head.html"
  width="100%"
  height="400"
  frameborder="0">
</iframe>


### EDA


#### Univariate Analysis

<iframe
  src="assets/objective_score_distribution.html"
  width="800"
  height="600"
  frameborder="0">
</iframe>

<iframe
  src="assets/dragons_distribution.html"
  width="800"
  height="600"
  frameborder="0">
</iframe>

<iframe
  src="assets/barons_distribution.html"
  width="800"
  height="600"
  frameborder="0">
</iframe>

#### Bivariate Analysis