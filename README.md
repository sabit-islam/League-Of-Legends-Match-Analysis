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

| | position | champion | kills | deaths | assists | wardsplaced | vspm | earned gpm | total cs | damagetochampions | 
|0| top | Aatrox | 1 | 3 | 1 | 14 | 0.7635 | 221.4210 | 279.0 | 7092 | 
|1| jng | Maokai | 0 | 4 | 3 | 10 | 1.2407 | 143.5737 | 153.0 | 7361 | 
|2| mid | Orianna| 0 | 2 | 0 | 4 | 0.9862 | 210.6045 | 270.0 | 10005 | 
|3| bot | Kalista| 2 | 4 | 0 | 22 | 1.3998 | 257,7200 | 311.0 | 10892 | 
|4| sup | Senna | 0 | 3 | 3 | 47 | 3.5313 | 98.5578 | 30.0 | 6451 | 

### Univariate Analysis

<iframe
  src="assets/univariate_graph.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
