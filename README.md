# League of Legends Performance Modeling

# 📊 Introduction to the Dataset  

In this project, we explore how a player's **position** in *League of Legends* affects their **in-game performance** and how we can use **performance statistics** to **predict a player’s position**. *League of Legends* is a team-based strategy game where players take on distinct roles—**Top, Jungle, Mid, ADC, and Support**—each with unique responsibilities and playstyles.  

## ❓ Why Does This Matter?  
Understanding how **in-game performance** relates to **player position** has multiple valuable applications:  
- 🔹 **Competitive Insights:** Helps players and coaches analyze role-based performance trends.  
- 🔹 **Automated Role Detection:** Can improve matchmaking and gameplay analysis tools.  
- 🔹 **Esports & Game Analytics:** Supports data-driven strategies in professional and amateur play.  

## 📈 Dataset Overview  
The dataset consists of **123,223 rows**, where each row represents a **player’s performance in a completed game**. The key columns relevant to our analysis include:  

| 🏷️ Column Name  | 📖 Description |  
|------------------|-------------|  
| **position** | The role the player took in the game (**Top, Jungle, Mid, ADC, Support**). This is our **target variable** for prediction. |  
| **kills** | Number of enemy champions eliminated by the player. |  
| **deaths** | Number of times the player was eliminated. |  
| **assists** | Number of times the player helped eliminate an enemy champion. |  
| **monsterkills** | Number of neutral monsters slain (e.g., jungle camps). |  
| **minionkills** | Number of lane minions killed for gold and experience. |  
| **dpm** (*Damage Per Minute*) | Average damage dealt to enemy champions per minute. |  
| **earned gpm** (*Earned Gold Per Minute*) | Rate at which the player accumulates in-game currency. |  
| **champion** | The specific champion the player used in the game, which may influence performance patterns. |  

By analyzing these **performance statistics**, we aim to uncover how **different roles** impact in-game performance and how we can **effectively predict a player’s position** based on these metrics.  

# Data Cleaning and Exploratory Data Analysis

## Data Cleaning


### Selecting Relevant Columns
To save time in the further data cleaning steps, we first only kept the relevant columns: `gameid`, `datacompleteness`, `date`, `game`, `side`, `position`, `playername`, `playerid`, `champion`, `gamelength`, `result`, `kills`, `deaths`, `assists`, `totalgold`, `total cs`, `earned gpm`, `damagetochampions`, `dpm`, `minionkills`, and `monsterkills`. This reduced the dataset size to include only the necessary information for analysis.

| gameid        | datacompleteness | date                | game | side | position   | playername | playerid                                | champion | gamelength | result | kills | deaths | assists | totalgold | total cs | earned gpm | damagetochampions | dpm     | minionkills | monsterkills |
|---------------|------------------|---------------------|------|------|------------|------------|-----------------------------------------|----------|------------|--------|-------|--------|---------|-----------|----------|------------|-------------------|---------|-------------|--------------|
| ESPORTSTMNT01 | complete         | 2022-01-10 07:44:08 | 1    | Blue | top        | Soboro     | oe:player:38e0af7278d6769d0c81d7c4b47ac1e | Renekton | 1713       | 0      | 2     | 3      | 2       | 10934     | 231.0    | 250.9282   | 15768.0           | 552.2942 | 220.0       | 11.0         |
| ESPORTSTMNT01 | complete         | 2022-01-10 07:44:08 | 1    | Blue | jng        | Raptor     | oe:player:637ed20b1e41be1c51bd1a4cb211357 | Xin Zhao | 1713       | 0      | 2     | 5      | 6       | 9138      | 148.0    | 188.0210   | 11765.0           | 412.0841 | 33.0        | 115.0        |
| ESPORTSTMNT01 | complete         | 2022-01-10 07:44:08 | 1    | Blue | mid        | Feisty     | oe:player:d1ae0e2f9f3ac1e0e0cdcb86504ca77 | LeBlanc  | 1713       | 0      | 2     | 2      | 3       | 9715      | 193.0    | 208.2312   | 14258.0           | 499.4046 | 177.0       | 16.0         |
| ESPORTSTMNT01 | complete         | 2022-01-10 07:44:08 | 1    | Blue | bot        | Gamin      | oe:player:998b3e49b01ecc41eacc392477a98cf | Samira   | 1713       | 0      | 2     | 4      | 2       | 10605     | 226.0    | 239.4046   | 11106.0           | 389.0018 | 208.0       | 18.0         |
| ESPORTSTMNT01 | complete         | 2022-01-10 07:44:08 | 1    | Blue | sup        | Loopy      | oe:player:e9741b3a238723ea6380ef2113fae63 | Leona    | 1713       | 0      | 1     | 5      | 6       | 6678      | 42.0     | 101.8564   | 3663.0            | 128.3012 | 42.0        | 0.0          |


## Univariate Analysis

We performed our univariate analysis on the `totalgold` column of the dataset, plotting its distribution with a histogram.

<iframe
  src="assets/total_gold_distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The histogram shows that the distribution of `totalgold` is approximately normal with a slight right skew, meaning  most players earn amounts near the average gold, with fewer players earning much lower or much higher amounts. 

We also plotted the distribution of `xxxxxx`.


## Bivariate Analysis

We performed our bivariate analysis by plotting a scatter plot to describe the relationship between the `damagetochampions` column and the `gamelength` column.

<iframe
  src="assets/damage_vs_gamelength.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The scatter plot shows a somewhat positively linear relationship between the two variables. In shorter game lengths, the scatter plot is more densely packed, and there are players who deal lots and little damage in these shorter games. However, when you look players who are dealing a lot of damage, these are almost always occuring in longer games.

We also created a box plot comparing the distributions of the `earned gpm` column within the `result` column. That is, we plotted the distribution of `earned gpm` across both wins and losses.

<iframe
  src="assets/gpm_by_result.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The box plot shows a shift in the two box plots. When looking at `earned gpm` in wins, all values of the box plot (min, q1, median, q3, max) are all shifted to the right, meaning that all these values are greater in wins. This is reasonable because players who earn more gold can buy better items, which ultimately helps them win the game.

## Interesting Aggregates

In order to confirm the scatter plot, we were curious to see how total damage differed between different game lengths. To do so, we created a pivot table and created five different buckets for the game lengths.

<div align="center">

| gamelength_minutes | damagetochampions  |
|--------------------|--------------------|
| (0, 20]            | 7428.907125        |
| (20, 30]           | 10732.953278       |
| (30, 40]           | 14352.552765       |
| (40, 50]           | 20618.585977       |
| (50, 60]           | 27926.046256       |

</div>


The pivot table confirms the scatterplot, and we see that the total damage mean damage is increasing as the game length also increases. Particularly, we see a steeper increase in total damage from 30-40 minute games to 40-50 minute games, and from 40-50 minute games to 50-60 minute games, which is reasonable because there are a lot of team fights at the end of the game. 

# Hypothesis Testing

## Hypothesis Testing for Performance Comparison

In this analysis, we tested the performance difference between **mid laners** and **top laners** across all games:

1. **Null Hypothesis**: 
   - There is **no difference** in the average performance between mid laners and top laners across all games.
   - Performance is quantified by a **weighted average** of the following metrics:
     - **Kills**
     - **Deaths**
     - **Assists**
     - **Total CS**

2. **Alternative Hypothesis**: 
   - **Mid laners** have a **higher average performance** than top laners across all games.

3. **Test Statistic**:
   - The test statistic used to evaluate the hypothesis is the **difference in the mean performance** between the two groups (mid laners and top laners).

We defined `performance` to be the following:
<img width="733" alt="image" src="https://github.com/user-attachments/assets/5c339237-3d9e-4819-af84-84a4f26e93f2" />


$$
(5 \times \text{kills}) + (2 \times \text{assists}) - (2 \times \text{deaths}) + \frac{4 \times \text{total cs}}{100}
$$

This framework allows us to determine if there is a statistically significant difference in the performance of mid laners compared to top laners.
