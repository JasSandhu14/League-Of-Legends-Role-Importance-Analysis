# League of Legends: Role Importance Analysis
Author: Jas Sandhu

## Introduction
League of Legends (LoL) is a popular online multiplayer game released by Riot Games. The gameplay consists of two teams of five players, each with a unique role, as they attempt to destroy the enemy team's base, known as the Nexus. With over 150 playable characters, each player has one of five roles: top, jungle, middle, bottom, or support, which all serve different purposes throughout the game. With the mass popularity of the game, debates have sparked for which role is considered the most important, which is what I hope to gain insight into with this project.

### The Dataset
The dataset used for this analysis is the [2024 League of Legends Esports Stats dataset]() provided by Oracle's Elixir which includes data from professional League of Legends matches across the world. The data includes 117576 rows and 161 columns. The following table shows the most important columns to the analysis.

| **Column Name**     | **Definition**                                               |
| ------------------- | ------------------------------------------------------------ |
| `gameid`            | game ID                                                      |
| `datacompleteness`  | indicates if the data for this match was complete or partial |
| `result`            | result of the game (0 for loss, 1 for victory)               |
| `champion`          | playable character used by player                            |
| `position`          | player’s role in the team                                    |
| `kills`             | total kills per player                                       |
| `deaths`            | total deaths per player                                      |
| `assists`           | total assists per player                                     |
| `doublekills`       | the number of double kills performed by player               |
| `triplekills`       | the number of triple kills performed by player               |
| `quadrakills`       | the number of quadra kills performed by player               |
| `pentakills`        | the number of penta kills performed by player                |
| `firstbloodkill`    | whether the player got the first kill of the game            |
| `firstbloodassist`  | whether the player assisted in the first kill of the game    |
| `firstbloodvictim`  | whether the player was the first death of the game           |
| `damagetochampions` | total damage dealt to other champions                        |
| `dpm`               | total damage dealt to other champions per minute             |
| `damagemitigatedperminute` | total damage mitigated per player per minute          |
| `visionscore`       | vision granted and denied                                    |
| `vspm`              | vision granted and denied per minute                         |
| `earnedgold`        | gold earned per player                                       |
| `earned gpm`        | gold earned per player per minute                            |
| `minionkills`       | total minions killed per player                              |
| `monsterkills`      | total monsters killed per player                             |

## Data Cleaning and Exploratory Data Analysis
### Data Cleaning
For ease of analysis, only the columns that were relevant to the analysis were kept leaving us with 23 columns. Additionally, rows were split according to the individual players stats and collective team stats for each game. Since I am interested in analyzing individual performance, I removed all rows that were not on player data. In other words, I removed all rows that contained collective team data. The `result` column was also changed to booleans, where `True` means the player won and `False` means the player lost, making it easier to understand. Similar alterations were made to `firstbloodkill`, `firstbloodassist`, and `firstbloodvictim`, making it easier to read first blood data. After cleaning, the dataset contains 97980 rows and 23 columns. 

### Univariate Analysis
A univariate analysis on the distribution of champions across all games. 
<iframe
  src="assets/univariate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
It should be noted that some characters are used more often than others, which may result in some skewed data, as some champions may perform better at certain statistics.

### Bivariate Analysis
Bivariate analysis on the distribution of kills + assists, damage per minute, and earned gold per minute.
<iframe
  src="assets/bivariate2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
Bottom and Mid Laners have a clear advantage in this regard, with support and jungle having a clear disadvantage. At first glance it seems as though bottom and mid laners are more influential in this regard. 

I then decided to do another analysis on earned gold per minute, as it seems there is a disparity between the damage certain roles have inflict. 
<iframe
  src="assets/bivariate3.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
As this graph shows, there is a similar trend in the earned gold per minute as there was in the damage per minute. This implies that there is a connection between a role's ability to earn gold and inflict damage, which makes sense as gold is commonly earned by killing champions and minions.

Finally, I decided to look at the number of kills + assists per role to see if this trend continues.
<iframe
  src="assets/bivariate1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
However, it seems as though all roles are relatively similar in this regard. This implies that the roles that were unable to inflict a lot of damage makes up for it by assisting and participating in kills.

### Interesting Aggregates
According to the trends spotted in the bivariate analysis, I decided to calculate the mean of various statistics grouped according each role.
| position   |    kills |   deaths |   assists |     dpm |   damagemitigatedperminute |    vspm |   earned gpm |   minionkills |   monsterkills |
|:-----------|---------:|---------:|----------:|--------:|---------------------------:|--------:|-------------:|--------------:|---------------:|
| bot        | 4.604    |  2.57981 |   5.73648 | 682.659 |                    310.231 | 1.207   |      303.178 |      265.932  |      12.7384   |
| jng        | 2.78205  |  3.17381 |   7.98122 | 407.086 |                    999.555 | 1.41025 |      218.78  |       24.44   |     163.637    |
| mid        | 3.93876  |  2.7154  |   5.9729  | 683.593 |                    419.706 | 1.10782 |      290.626 |      267.454  |       9.42131  |
| sup        | 0.920851 |  3.63411 |   9.88982 | 204.696 |                    619.754 | 3.28185 |      126.039 |       42.5094 |       0.291029 |
| top        | 2.87513  |  3.0494  |   5.20407 | 552.316 |                   1010.86  | 0.95972 |      256.543 |      239.654  |       6.71928  |

From the aggregated table, our suspicion was correct as jungle and support have a higher average assists than every other role. Our analysis also shows that these roles have lower average minion kills, possibly influencing the earned gold per minute from before. At first glance, while it may seem that bottom and mid lane may be the most important, the other positions influence the game in different ways. Top lane and jungle have the highest mean damage mitigated per minute, support has the highest average vision score per minute, and jungle has a large advantage for average monster kills.