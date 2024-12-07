# League of Legends Position Performace Analysis

This project is meant to serve as an intensive data science project conducted at the University of Michigan. We will investigate through our data and begin with our question, learn to understand the dataset, create visualizations and helpful aggregates, frame a prediction problem, and then finally create our model. Our goal is to understand whether or not a players performance is significantly affected by the position they play. 

Authors: Sabit Islam, Anthony Brunswick 


## Introduction 
### What data is being used? 
Our dataset comes from the game League of Legends, which is a widely popular multiplayer online battle arena video game made in 2009 by Riot Games with over 150 million active monthly players. This set is provided by Oracle's Elixer, which provides information from professional matches which took place in 2024.

The dataset is filled with statistical match data for 9668 matches, and provides very thorough metrics for each team as well as each player. It includes in-game statistics, team performance, as well as player performance throughout the match.

During each match, every team has 5 players, and each player has a specific role that they play. These include **top lane**, **bot lane**, **mid lane**, **support**, and **jungler**. Each role has its own objectives, and they must cooperate well with each other to make sure their team is able to succeed.

- `Top lane`: Primarily a solo lane where players build tanky or damage-focused champions.

- `Bot lane`: the ADC (Attack Damage Carry) where the primary objective is to farm and secure objectives.

- `Mid lane`:  A solo lane that often focuses on damage-dealing champions.

- `Jungler`: Moves around the map to secure neutral objectives and assist other lanes.

- `Support`:   Helps with vision control and assisting the Bottom-lane players in the bot lane.

A players performance in their role is crucial to the outcome of the success for their team. For our analysis our primary goal will be to analyze how the position that a player chooses is able to affect their overall performance in the game. What metrics are certain positions better at? Through our analysis we will set to answer these questions and look to see at what metrics are better served by certain positions. Ultimately creating a model which will help to build on team dynamics and individual performance by understanding how each role contributes to the team. 

### What data is important? 
The full dataset is complete with 116,016 rows, each row corresponding to either a player or a team for matches played in the 2024 season. While the dataset contains 161 columns of in-game statistics, team performance, and other fun metrics about the match. Our focus will remain on those columns which are relavent to the players performance for which position they chose to play. 

Below you will find the relavent columns and a brief introduction to what they are.


|Column Name                 |Description| 
|---                         |---        |
|`'position'`                |The role that a player fulfills on their team (top,bot,mid,jungle, or support)| 
|`'kills'`                   |The number of opponents that the player was able to successfully eliminate throughout the match| 
|`'assists'`                 |The number of assists a player contributes during the match.|
|`'champion'`                |The champion chosen by the player for the match| 
|`'wardsplaced'`             |How many of these wards, which are deployable items that eliminate the fog of war and help to provide vision, they place| 
|`'vspm'`                    |Vision score per minute, which serves as an indicator of vision coverage as well as map control| 
|`'total cs'`                |A score calculated based on the amount of monsters and minions the player eliminates|
|`'earned gpm'`              |How much gold the player earns every minute| 
|`'damagetochampions'`       |Total damage player inflicts on enemy champions during the match| 


## Data Cleaning and Exploratory Data Analysis
### Data Cleaning 
Since we are only concerned with individual player statistics throughout the match, the first part of our cleaning process consists of removing all the rows which contain the summarative team information for the match. This will remove 2 rows for every match in our dataset, leaving us with a dataframe consisting of only individual player data. Next, we make sure to only include the columns which contain relevant information about how a player performs with their position. Therefore, we decided to keep the following columns: `position`, `kills`, `assists`, `champion`, `wardsplaced`, `vspm`, `total cs`, `earned gpm`, and `damagetochampions`. 

Thankfully, for the dataset provided and the columns that we have chosen, the dataset is able to provide values for every row in our relevant dataframe. There will be no need to handle any missing values. 

Below is a head of our clean_players dataframe. 

| position   | champion   |   kills |   deaths |   assists |   wardsplaced |   vspm |   earned gpm |   total cs |   damagetochampions |
|:-----------|:-----------|--------:|---------:|----------:|--------------:|-------:|-------------:|-----------:|--------------------:|
| top        | Aatrox     |       1 |        3 |         1 |            14 | 0.7635 |     221.421  |        279 |                7092 |
| jng        | Maokai     |       0 |        4 |         3 |            10 | 1.2407 |     143.574  |        153 |                7361 |
| mid        | Orianna    |       0 |        2 |         0 |             4 | 0.9862 |     210.605  |        270 |               10005 |
| bot        | Kalista    |       2 |        4 |         0 |            22 | 1.3998 |     257.72   |        311 |               10892 |
| sup        | Senna      |       0 |        3 |         3 |            47 | 3.5313 |      98.5578 |         30 |                6451 |

### Univariate Analysis

Below is a histogram representing the distribution of `assists` in our dataset. 

<iframe
  src="assets/uni_graph.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
This plot that the distribution of assists appears to be normal with a slight skew to the right, this can be confirmed by our knowledge of the game since we know that those in more support roles tend to have much higher assists than those in other positions. 

### Bivariate Analysis 

Below is a box plot showing how each `position` differs in `creep score`

<iframe
  src="assets/bivariate_graph.html"
  width="800"
  height="500"
  frameborder="0"
></iframe>

Here we can see some *pretty cool* trends in our data. We know that there is a high spawn rate of creeps in our bottom lane, and our plot validates this as we can see that those who farm in the bottom lane, or near it (mid-laners) have a generally higher creep score than the rest. We also notice that `junglers` and `support` have much lower scores, since they are not in the lanes where these usually spawn.


### Interesting Aggregates

Here is a table of aggregated mean data grouped by each position in our dataset: 

| position   |   kills |   deaths |   assists |   wardsplaced |   vspm |   total cs |   earned gpm |
|:-----------|--------:|---------:|----------:|--------------:|-------:|-----------:|-------------:|
| bot        |    4.59 |     2.57 |      5.72 |         14.14 |   1.21 |     278.64 |       303.16 |
| jng        |    2.77 |     3.17 |      7.96 |         11.15 |   1.41 |     187.99 |       218.62 |
| mid        |    3.93 |     2.71 |      5.95 |         12.96 |   1.11 |     276.94 |       290.68 |
| sup        |    0.92 |     3.63 |      9.86 |         54.66 |   3.28 |      42.88 |       126.03 |
| top        |    2.87 |     3.04 |      5.19 |         11.66 |   0.96 |     246.45 |       256.6  |


First, if we look in the `assists`, `total cs`, and `earned gpm` we can notice some strong connection between the top, mid, and bot positions. This makes sense because these 3 positions are more involved in the central aspects of the map as they try to gain control over the map. This means that they ultimately share similar objectives, causing there in-game statistics to build a sort of connection to each other. 

We can also notice how dominant of a role the support plays in columns such as in the `assists` and `wardsplaced`. Support players while they might not be engaging in as much direct conflict are still one of the biggest contributors on the team. Helping the team to gain more map control, and their impact does not go unnoticed. 


## Framing a Prediction Problem

Based on all the information we have been able to gather so far about this dataset, we were able to notice trends about how different positions can have a correlation on certain in-game metrics that a player will attain. Would it be possible to accurately predict what role a player had on a team based on certain in-game statitistics? 

By integrating a machine learning classification algorithm we can attempt to see how well these metrics servre to predict a players role during a match.This will be a multiclass classification which will take metrics known at the end of a match, and try to predict whether a player was designated to be a `top`,`bot`,`mid`,`jng`, or `sup`. Since we know every team has to assign each of these positions to a player, we can conclude that our dataset will be well balanced with each position making up exactly 20% of our dataset. Therefore, we can evaluate our model based on accuracy to see how well it is predicting. 


## Baseline Model 





