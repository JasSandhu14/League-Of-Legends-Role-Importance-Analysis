# League of Legends: Position Importance Analysis
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
<iframe
  src="assets/univariate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
