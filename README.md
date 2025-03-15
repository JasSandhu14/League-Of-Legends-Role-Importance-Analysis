# League of Legends: Role Importance Analysis
Author: Jas Sandhu

## Introduction
League of Legends (LoL) is a popular online multiplayer game released by Riot Games. The gameplay consists of two teams of five players, each with a unique role, as they attempt to destroy the enemy team's base, known as the Nexus. With over 150 playable characters, each player has one of five roles: top, jungle, middle, bottom, or support, which all serve different purposes throughout the game. With the mass popularity of the game, debates have sparked for which role is considered the most important, which is what I hope to gain insight into with this project.

### The Dataset
The dataset used for this analysis is the 2024 League of Legends Esports Stats dataset provided by Oracle's Elixir which includes data from professional League of Legends matches across the world. The data includes 117576 rows and 161 columns. The following table shows the most important columns to the analysis.

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
For ease of analysis, only the columns that were relevant to the analysis were kept leaving us with 20 columns. Additionally, rows were split according to the individual players stats and collective team stats for each game. Since I am interested in analyzing individual performance, I removed all rows that were not on player data. In other words, I removed all rows that contained collective team data. The `result` column was also changed to booleans, where `True` means the player won and `False` means the player lost, making it easier to understand. Similar alterations were made to `firstbloodkill`, `firstbloodassist`, and `firstbloodvictim`, making it easier to read first blood data. After cleaning, the dataset contains 97980 rows and 20 columns. 

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
Bottom and Mid Laners have a clear advantage in this regard, with support and jungle having a clear disadvantage. At first glance it seems as though bottom and mid laners are more influential in this regard. I then decided to do another analysis on earned gold per minute, as it seems there is a disparity between the damage certain roles can inflict. 
<iframe
  src="assets/bivariate3.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
As this graph shows, there is a similar trend in the earned gold per minute as there was in the damage per minute. This implies that there is a connection between a role's ability to earn gold and inflict damage, which makes sense as gold is commonly earned by killing champions and minions. Finally, I decided to look at the number of kills + assists per role to see if this trend continues.
<iframe
  src="assets/bivariate1.html"
  width="800"
  height="700"
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

## Assessment of Missingness
### NMAR Analysis
From the original dataset, `ban5` is a column that showed the 5th champion that was banned from the game. In professional Leauge of Legends games, both teams have an opportunity to ban five champions from the other team. I believe that this column is not missing at random (NMAR) as I could not point out a pattern in the missingness of the data. I reason that some teams purposely decided not to ban a fifth champion, which would make the missing data point dependent on the actual value of the missing data point itself. To make this column missing at random (MAR), a new column that checks to see if the total number of bans were used can provide more insight into the missing data point. 

### Missingness Dependency
In the dataset, `damagemitigatedperminute` has a some missing values. I'm going to test if the missingness of these values is dependes on other columns, in particular, `datacompleteness` and `position`. The significance level I chose for both tests is 0.05 and test statistic is Total Variation Distance (TVD).

**Null Hypothesis**: Distribution of `datacompleteness` when `damagemitigated` is missing is the same as the distribution of `datacompleteness` when `damagemitigated` is not missing.

**Alternative Hypothesis**: Distribution of `datacompleteness` when `damagemitigated` is missing is NOT the same as the distribution of `datacompleteness` when `damagemitigated` is not missing.

Below is the observed distribution of `datacompleteness` when `damagemitigatedperminute` is missing and not missing.

| datacompleteness   |   missing=False |   missing=True |
|:-------------------|----------------:|---------------:|
| complete           |       1         |      0         |
| partial            |       0         |      1         |

It is clear that the missingess of `damagemitigatedperminute` depends on `datacompleteness` but I will perform a permutation test to make sure. From our permutation test, the observed TVD is 1.0 and the p-value for the test is 0.0.

<iframe
  src="assets/missing1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Since the p-value is less than the significance level of 0.05, we reject the null hypothesis, meaning that the missingness of `damagemitigatedperminute` is dependent on `datacompleteness`. 

The second permutation test is on the `position` column with the same missingness column `damagemitigatedperminute`.

**Null Hypothesis**: Distribution of `position` when `damagemitigated` is missing is the same as the distribution of `position` when `damagemitigated` is not missing.

**Alternative Hypothesis**: Distribution of `position` when `damagemitigated` is missing is NOT the same as the distribution of `position` when `damagemitigated` is not missing.

Below is the observed distributino of `position` when `damagemitigatedperminute` is missing and not missing.

| position   |   missing=False |   missing=True |
|:-----------|----------------:|---------------:|
| bot        |             0.2 |            0.2 |
| jng        |             0.2 |            0.2 |
| mid        |             0.2 |            0.2 |
| sup        |             0.2 |            0.2 |
| top        |             0.2 |            0.2 |

Once again, off first glance, the data seems to be straightforward but I'll perform permutation test again to ensure this. From the permuation test, the observed TVD is 0.0 and I got a p-value of 1.0. 

<iframe
  src="assets/missing2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Since the p-value is greater than the significance level of 0.05, we fail to reject the null hypothesis, meaning that the missingness of `damagemitigatedperminute` is not dependent on `position`.

## Hypothesis Testing
After cleaning and exploring the data, I decided to see if all roles have the same impact.

**Null Hypothesis**: All roles have the same impact

**Alternative Hypothesis**: All roles do not have the same impact

For my hypothesis test, the test statistic I used was Total Variation Distance (TVD) with a significance level of 0.05, which is commonly used to test statistical significance. TVD was used in the scenario since I'm comparing categorical distributions of the different positions. I ran three seperate tests, on `kills + assists`, `damage per minute`, and `earned gold per minute` as the exploratory data analysis showed that different roles specialized in different aspects of the game. I also used per minute statistics for damage and earned gold as some games could've lasted longer than others, allowing players to get more damage and coins. 

**Results for Kills+Assists:**
P-value = 0.0

<iframe
  src="assets/KA_Hypot_Test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Since the p-value is below the significance level of 0.05, we reject the null hypothesis, indicating that all roles do not have the same average kills + assists.

**Results for Damage Per Minute:**
P-value = 0.0

<iframe
  src="C:\Users\Jas\Documents\GitHub\League-Of-Legends-Role-Importance-Analysis\assets\DPM_Hypot_Test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Since the p-value is below the significane level of 0.05, we reject the null hypothesis, indicating that all roles do not have the same average damage per minute.

**Results for Earned Gold Per Minute:**
P-Value = 0.0

<iframe
  src="assets/gpm_test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Since the p-value is below the significance level of 0.05, we reject the null hypothesis, indicating that all roles do not have the same average earned gold per minute.

### Overall Conclusion
From our three hypothesis tests, we can reject the null hypothesis, meaning all roles do not seem to have the same impact overall. 

## Framing a Prediction Problem
From the previous problem, we discovered in our aggregation that roles specialized in different aspects of the game. Recall, bottom and mid lane has the most damage but support had the most assists, while jungle and top had the most damage mitigated. Using this information, I'm going to predict a player's role based off their in-game statistics. Since I'm predicting a role out of five possible options, I'm building a multiclass random forest classifier, in order to do so. The response variable in this case would be `position` and to test its success, I'll be using accuracy as it gives me a generalized view of my model's success. Also since the number of each players for each role is even, there are no imbalances, so accuracy works.

To prevent overfitting, I split the data into 75% training and 25% testing, granting me ample resources to train and test the model. 

## Baseline Model
For my baseline model, I decided to keep the same features that I used in my previous hypothesis test as we concluded there was a difference between each role. These features are `kills`, `assists`, `dpm`, and `earned gpm`. I also decided to add `deaths` as another feature since it's closely tied to kills and assists (KDA). All these features are quantitative and no encodings were necessary. 

After fitting the model, the accuracy score on the training data is 0.49, meaning my model is able to correctly predict the position 49% of the time. Although this is better than guessing a single role everytime (which would be correct 20% of the time), there are still a lot of improvements to be made to increase the accuracy. 

## Final Model
To improve the accuracy of the model, I included the features that were noted in the first aggregation, revealing where each role specialized in. Since some roles specialized in killing minions or monsters, I included `minionkills` and `monsterkills` as new features. Similarly, support performed higher on average in vision score per minute, so `vspm` was also included as a feature. Since `dpm`, `earned gpm`, and `vspm` are statistics per minute, they are already standardized for the length of the game. Thus, I had to use a StandardScaler for `minionkills` and `monsterkills` as they are quantitative features, to normalize them. 

As we continue to add features, I'm confident in the performance of the random forest classifier as it is well suited to handle various features with less risk of overfitting or performance loss. To improve my model performance even more, I used a grid search to find the best hyperparameters to use for my model. The search found the best hyperparameters for my random forest to be criterion as 'gini', a max depth of 10, and a minimum sample split of 5.

After implementing the new features and hyperparameters, the updated model was able to achive an accuracy score of 0.71 on the training data, a roughly 20% increase from the previous model. The new features and hyperparameters proved to be effective in improving model performance, and thus, we have a higher chance of correctly predicting a player's role given their stats.

## Fairness Analysis
To test the fairness of the model, we going to see if the model predicts differently based on different groups. In this section, I'm interested in seeing if the number of deaths influences the models prediction. Group X is this scenario will be players with more than 3 deaths while Group Y will be player with less than or equal to 3 deaths. The line was set at 3 deaths, as the median and mean were 3, so we should have a similar distribution in both groups. 

**Null Hypothesis:** The accuracy of the model is the same for players with more than 3 deaths as it is for players with less than or equal to 3 deaths.

**Alternative Hypothesis:** The accuracy of the model is not the same for players with more than 3 deaths as it is for players with less than or equal to 3 deaths.

The test statistic is the difference in accuracy between the two groups. After performing a permutation test according to these parameters, we get a p-value of 0.0.

<iframe
  src="assets/fairness_test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Since the p-value is less than the significance value of 0.05, we reject the null hypothesis, showcasing a bias in the model according to the number of deaths. This implies that our model is unfair according to this statistic, as the accuracy of the model is not the same between the two groups.