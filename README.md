# League of Legends Position Performace Analysis

This project is meant to serve as an extensive data science project conducted at the University of Michigan. We will investigate through our data and begin with our question, learn to understand the dataset, create visualizations and helpful aggregates, frame a prediction problem, and then finally create our model. Our goal is to understand whether or not a player's performance is significantly affected by the position they play. 

Authors: Sabit Islam, Anthony Brunswick 


## Introduction 
### What data is being used? 
Our dataset comes from the game League of Legends, which is a widely popular multiplayer online battle arena video game made in 2009 by Riot Games with over 150 million active monthly players. This set is provided by Oracle's Elixer, which provides information from professional matches which took place in 2024.

The dataset is filled with statistical match data for 9,668 matches, and provides very thorough metrics for each team as well as each player. It includes in-game statistics, team performance, as well as player performance throughout the match.

During each match, both teams have 5 players with each player having a specific role that they play. These include **top lane**, **bot lane**, **mid lane**, **support**, and **jungler**. Each role has its own objectives, and they must cooperate well to ensure success.

- `Top lane`: A solo lane with the objective of farming, capture objectives, and building a presence. They often take a 'tank' role, focusing on taking damage for the team rather than getting kills.   

- `Bot lane`: The ADC (Attack Damage Carry) with the primary objective of farming and capturing objectives alongside the Support. Their distinguishing focus is collecting gold as they depend on buying items instead of leveling up; this is due to sharing XP with the Support.

- `Mid lane`:  Another solo lane with the primary objective of farming and capturing objectives. They are similar to Bot lane, however not sharing XP means they are able to focus on leveling instead of collecting gold.

- `Jungler`: The only role without a dedicated lane; they travel across the map farming 'jungle objectives' and jumping between all three lanes to support their teammates.

- `Support`:   Shares a lane alongslide Bot lane, their objective is to heal and mitigate enemy damage; while they do not directly kill, they are characterized by a high amount of assists. Additionally, they focus on building vision across the map using wards to find and track enemies.  

A player's ability to fulfill their specific role requirements is crucial; thus their statistics and role are intertwined. For our analysis, our primary goal will be analyzing how the position that a player chooses affects their overall performance in specific categories within the game. What metrics do certain positions excel at? Which metrics do certain positions not emphasize? Through our analysis we will set to answer these questions and look to see at what metrics are characterized by certain positions. Ultimate, we will create a model which will help to build on team dynamics and individual performance by understanding how each role contributes to the team. 

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

The plot above shows a rather normal dataset which skews to the right. As we know one role specifically focuses on `assists`, `Support`, a right skew makes perfect sense. Due to this skew, it is proven mathematically how the `assists` of `Supports` distinguish themselves from other roles, placing them outside the normal distribution and allowing prediction.

### Bivariate Analysis 

Below is a box plot showing how each `position` differs in `creep score`.

<iframe
  src="assets/bivariate_graph.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Here we can see some *pretty cool* trends in our data. While `Top`, `Mid`, and `Bot` lane all have equal spawn rate of creeps, we can see how different each lane prioritizes farming them. Heavily dependent upon getting gold and XP, both of which come from creeps, `Mid` and `Bot` lanes have higher median scores. While `Top` also farms creeps, their priority of becoming a tank lessens their creep score. `Jungler` farms a different kind of creep - jungle objectives - which are harder to kill, slower to spawn, and stationary, requiring the `Jungler` themself to move. This results in a the 2nd lowest median score comparitively. Lastly, `Support` is not intended to kill creeps, instead having their partner, `bot lane` farm, thus, we can characterize them by the lowest creep score. From these roles and differing objectives, we realize we can also use creep score to predict which role every player was.


Below is another plot which shows the 10 most played `champion` and what `position` they were
<iframe
  src="assets/champ_graph.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

League of Legends does not bind certain champions to certain roles: any champion is allowed to be picked in any role desired. However, that is not to say certain champions are not more *efficient* in certain roles. This is known in part as the game's "meta", where certain champions are strategically played in only certain roles. For example, `Nautilus` has low damage output, yet can stun enemies; due to this, he is played nearly exlusively in the `Support` role. Across the 10 most played champions, this pattern of a standout role is seen in each one, creating a connection that can be used in prediction. 


### Interesting Aggregates

Here is a table of aggregated mean data grouped by each position in our dataset: 

| position   |   kills |   deaths |   assists |   wardsplaced |   vspm |   total cs |   earned gpm |
|:-----------|--------:|---------:|----------:|--------------:|-------:|-----------:|-------------:|
| bot        |    4.59 |     2.57 |      5.72 |         14.14 |   1.21 |     278.64 |       303.16 |
| jng        |    2.77 |     3.17 |      7.96 |         11.15 |   1.41 |     187.99 |       218.62 |
| mid        |    3.93 |     2.71 |      5.95 |         12.96 |   1.11 |     276.94 |       290.68 |
| sup        |    0.92 |     3.63 |      9.86 |         54.66 |   3.28 |      42.88 |       126.03 |
| top        |    2.87 |     3.04 |      5.19 |         11.66 |   0.96 |     246.45 |       256.6  |


First, if we look in the `assists`, `total cs`, and `earned gpm` we notice strong similarites between the `Top`, `Mid`, and `Bot` lane positions and their means. As they share similar goals of farming creeps in their lane in order to collect gold, this is a logical similarity. Additionally, none focus on supporting another teammate secure a kill, like `Support` and `Jungler`, thus they share a lower mean `assists` as well.

We also notice how dominant of a role the `Support` plays in columns such as `assists` and `wardsplaced`. While `Support` does not show their contribution in `kills` or `total cs` as the other lanes might, we can see their standout contribution in their high means in `assists` and `wardsplaced`. Their role prioritizes helping their teammates win fights and giving vision and map knowledge through wards; again showing how role ultimately affects final player statistics. 


## Framing a Prediction Problem

Based on all the information we have been able to gather so far about this dataset, we were able to notice trends about how different positions have a correlation on certain in-game metrics that a player will attain. Would it then be possible to reverse this and accurately predict what role a player had on a team based on certain in-game statitistics? 

By integrating a machine learning classification algorithm we can attempt to see how well these metrics servre to predict a players role during a match.This will be a multiclass classification which will take metrics known at the end of a match, and try to predict whether a player was designated to be a `Top`,`Bot`,`Mid`,`Jungler`, or `Support`. Since we know every team has to assign each of these positions to a player, we can conclude that our dataset will be well balanced with each position making up exactly 20% of our dataset. Therefore, we can evaluate our model based on accuracy to see how well it is predicting. 


## Baseline Model 

For our baseline model, we chose to implement a `RandomForestClassifier`, making use of 3 of the columns in our cleaned dataset. `champion`, `kills`, and `assists`. Out of these columns 2 are quantitative (`kills`, `assists`), we chose not to transform these columns and leave them as is. Our third column happens to be nominal data (`champion`); since it is a nominal categorical, we chose to use a `OneHotEncoder` to handle it properly. This is done to convert our categorical data into a numerical format so that the model is able to learn from it. This addition added about 160 columns into our dataframe. 

After we made sure to split our training set into a training set and a validation set, we were able to train our model and then test its accuracy against unseen test data. Just with these 3 simple columns, and one simple encoding our model is able to predict with an accuracy of 90.29%! This means that for every 10 predictions it makes, it will only make one wrong decision. This accuracy might seem high, but in our final model after including other columns and some more transformations. We believe that our model will be able to predict with more accuracy.

## Final Model 
In our final model, it remained as a `RandomForestClassifier` and included the columns in our dataset which showed a significant distribution amongst each position which was played. These are all quantitative columns which include: `earned gpm`, `vspm`,`wardsplaced`,`total cs`. The significance of these columns is represented earlier in our analysis. We chose to add a `StandardScalar` to two of these columns (`wardsplaced`, `total cs`) as each game they came from were completed in different lengths, by adding this transformer we are able to standardize these columns to make sure each game is represented fairly. Due to complications throughout our process, and the size of our dataset with all the columns added with our `OneHotEncode` there were some issues with finding the best hyperparameters to integrate into our `RandomForestClassifier` to see how we could improve our `n_estimators` or our `max_depth`. So instead of being able to run them concurrently and check as many points as we could (We tried to let it run for hours, with no success), our best option was to pick and choose static points and move in the direction of our data. It was noticed that the numbers would just keep getting larger with there ultimately being no noticable improvement on our model. It ultimately would suggest to keep our parameters as the default.

While our model was unable to improve significantly, we were able to get our model to predict with about 94.27% accuracy which is about a 4% increase from our baseline model. The primary prediction problem that arises is the similarity between the lane positions (`bot`,`top`,`mid`). Represented in our Confusion Matrix shown below it can be seen that the majority of our prediction mistakes took place between these 3 columns. However, this difference is not enough to deem our model unsuccessful. 
<iframe
  src="assets/confusion.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Ultimately, throughout this entire data analysis we were able to start with a question and live through the Data Science Lifecycle and end up with a properly functioning model that is able to take a players in game statistics and predict the role that they played throughout the match.

Thank you for joining us on this exploration!