# League of Legends Position Performace Analysis

This project is meant to serve as an intensive data science project conducted at the University of Michigan. We embark through an intense investigation through our data as we begin with our question, learn to understand the dataset, create visualizations and helpful aggregates, frame a prediction problem, and then finally create our model. Our goal is to understand whether or not a players performance is significantly affected by the position they play. 

Authors: Sabit Islam, Anthony Brunswick 


## Introduction 
### What data is being used? 
Our dataset comes from the game League of Legends, which is a widely popular multiplayer online battle arena video game made in 2009 by Riot Games with over 150 million active monthly players. This set is provided by Oracle's Elixer, which provides information from professional matches which took place in 2024.

The dataset is filled with statistical match data for 9668 matches, and provides very thorough metrics for each team as well as each player. It includes in-game statistics, team performance, as well as player performance throughout the match.

During each match, every team has 5 players, and each player has a specific role that they play. These include **top lane**, **bot lame**, **mid lane**, **support**, and **jungler**. Each role has its own objectives, and they must cooperate well with each other to make sure their team is able to succeed.

- 'Top lane': Primarily a solo lane where players build tanky or damage-focused champions.
- 'Bot lane': the ADC (Attack Damage Carry) where the primary objective is to farm and secure objectives.
- 'Mid lane':  A solo lane that often focuses on damage-dealing champions.
- 'Jungler': Moves around the map to secure neutral objectives and assist other lanes.
- 'Support':   Helps with vision control and assisting the Bottom-lane players in the bot lane.

A players performance in their role is crucial to the outcome of the success for their team. For our analysis our primary goal will be to analyze how the position that a player chooses is able to affect their overall performance in the game. What metrics are certain positions better at? Through our analysis we will set to answer these questions and look to see at what metrics are better served by certain positions. Ultimately creating a model which will help to build on team dynamics and individual performance by understanding each roles ultimate purpose. 

### What data is important? 
The full dataset is complete with 116016 rows, with each row corresponding to either a player or a team for matches played in the 2024 season. While the dataset contains 161 columns of in-game statistics, team performance, and other fun metrics about the match, our focus will remain on those columns which are relavent to the players performance for which position they chose to play. Below you will find the relavent columns and a brief introduction to what they are.


|Column Name                 |Description| 
|---                         |---        |
|''position''                |The role that a player fulfills on their team (top,bot,mid,jungle, or support)| 
|''kills''                   |The number of opponents that the player was able to successfully eliminate throughout the match| 
|''champion''                |The champion chosen by the player for the match| 
|''wardsplaced''             |How many of these wards, which are deployable items that eliminate the fog of war and help to provide vision, they place| 
|''vspm'' 