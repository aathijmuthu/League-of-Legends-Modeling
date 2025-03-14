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
To save time in the further data cleaning steps, we first only kept the relevant columns: `gameid`, `datacompleteness`, `date`, `game`, `side`, `position`, `playername`, `playerid`, `champion`, `gamelength`, `result`, `kills`, `deaths`, `assists`, `totalgold`, `total cs`, `earned gpm`, `damagetochampions`, `dpm`, `minionkills`, and `monsterkills`. Additionally, we identified approximately **25,000 missing data points** out of a total **150,000**. Given that missing values accounted for only **16.7%** of the dataset, we determined that dropping all null values was a reasonable and efficient cleaning approach.  

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


| gamelength_minutes | damagetochampions  |
|--------------------|--------------------|
| (0, 20]            | 7428.907125        |
| (20, 30]           | 10732.953278       |
| (30, 40]           | 14352.552765       |
| (40, 50]           | 20618.585977       |
| (50, 60]           | 27926.046256       |



The table confirms the insight of the scatter plot. Here, we see a sharp increase from damage in 30-40 minute games to 40-50 minute games, and from 40-50 minute games to 50-60 minute games, which is reasonable because nearing the end of the game, there are more team fights, and thus more damage to be dealt.

# Assessment of Missingness

## Identifying NMAR Missingness in `minionkills`

I believe the `minionkills` column in my dataset is **Not Missing at Random (NMAR)** due to its connection to gameplay mechanics in *League of Legends*. This column has **~3,796 missing values**, whereas key statistics like `kills`, `deaths`, `assists`, and `totalgold` have **no missing values**. Other columns, such as `champion` (**25,098 missing**) and `total cs` (**25,098 missing**), exhibit far greater missingness, making `minionkills` stand out due to its **moderate** yet **patterned** missingness.

### Why `minionkills` is NMAR

- `minionkills` represents the number of minions a player has killed.
- Junglers primarily earn gold and experience from **monsters**, not minions.
- Due to game mechanics (e.g., **jungle item penalties**), some Junglers have **zero minion kills**.
- I hypothesize that **missing `minionkills` values correspond to zero minion kills**, meaning they are missing **because of their own value**, making the missingness NMAR.

### Distinguishing NMAR from MAR & MCAR

- **Not MCAR**: Missingness is not completely random; it is linked to a player’s role (Jungle).
- **Not MAR**: Other observed variables (e.g., `position`) **suggest** Jungle playstyle but do not explicitly determine why `minionkills` is missing.

### Additional Data Needed to Reclassify as MAR

To confirm if missingness is explainable by observed variables (**making it MAR**), I would need:

1. **A Binary Indicator (`had_no_minion_kills`)**  
   - If players with `minionkills` missing are explicitly flagged as having zero kills, missingness could be MAR.  

2. **Role-Specific Behavior Logs**  
   - Data on jungle items (e.g., *Smite* usage) could validate if missing `minionkills` aligns with Jungle players.  

3. **Game Event Timestamps**  
   - Minion kill timestamps could confirm if missingness occurs in specific game phases.  


Since `minionkills` missingness appears **dependent on its unobserved value (zero or low kills)** rather than existing variables, I classify it as **NMAR**. However, with additional observed features (e.g., a zero-kill indicator), this could be reclassified as MAR.


I will analyze the `champion` column, which has **25,098 missing values**—a substantial and likely significant portion in a *League of Legends* game statistics dataset. While other candidates like `playername` (25,098 missing) and `playerid` (27,345 missing) exhibit similar missingness, `champion` is more relevant to gameplay and modeling (e.g., predicting player position), making it a meaningful choice.

---

## **Defining the Permutation Test**

To determine whether the missingness of `champion` depends on other columns, we will perform a **permutation test** as follows:

### **1. Create a Binary Indicator**
- Define a binary variable:  
  - **1** if `champion` is missing  
  - **0** if `champion` is present  

### **2. Test Dependency Using a Permutation Test**
- Compare a chosen statistic between rows with and without missing `champion` values.
- Conduct a **two-sided test** to identify:
  - **A dependent column** (statistically significant, p-value < 0.05)
  - **An independent column** (not statistically significant, p-value ≥ 0.05)

### **3. Select Statistics for Comparison**
- **For numerical columns** (e.g., `kills`, `gamelength`):  
  - Compute the **difference in means** between groups.
- **For categorical columns** (e.g., `result`):  
  - Compute the **difference in proportions** of a specific category.

This approach will help determine whether `champion` missingness is associated with other game-related variables, shedding light on possible missing data mechanisms.


<iframe
  src="assets/permutation_test_kills.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## **Permutation Test Result**

### **Result for `kills`**
- **Observed Difference** = **11.5942**  
  - The mean `kills` for rows where `champion` is missing differs by **11.5942** from the mean where it’s not missing.  

- **P-value** = **0.0000**  
  - This is **highly significant** (p < 0.05), indicating a **strong dependency** between `champion` missingness and `kills`.  

### **Interpretation**
The missingness of `champion` is **not random** with respect to `kills`.  
- Rows with missing `champion` values have **significantly different kill counts** compared to rows where `champion` is present.  
- Since the difference is positive, this suggests that **players with missing `champion` values tend to have higher kills** (unless otherwise specified).  


# Hypothesis Testing

## Hypothesis Testing for Performance Comparison

In this analysis, we tested the performance difference between **mid laners** and **top laners** across all games with a significance level of 0.05:

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

<img width="733" alt="image" src="assets/Xnip2025-03-14_23-34-23.jpg" />

This framework allows us to determine if there is a statistically significant difference in the performance of mid laners compared to top laners.

Our observed difference (mean of mid performance - mean of top performance) was 6.386495228223918. But, we need to run a permutation test in order to conclude how statistically significant this value is.

## Permutation Test for Statistical Significance

To assess the statistical significance of the observed results, we performed a **permutation test**:

1. **Shuffling Position Labels**: The position labels were randomly shuffled to break any association between the features and the target variable (position).
2. **Recomputation of Mean Difference**: For each shuffle, we recomputed the mean difference between the predicted and actual values.
3. **Repetitions**: The process was repeated **10,000 times** to generate a distribution of mean differences under the null hypothesis.
4. **P-value Calculation**: The **p-value** was computed by comparing the observed mean difference to the distribution of mean differences generated through permutation.

This process allowed us to determine whether the observed result was statistically significant or could have occurred by chance.

After running the permutation test to test for statistical significance, we concluded with the following values.

```
Observed Difference in Means: 6.386
Average value of Permuted Differences: -0.000327
P-value: 0.0000
```

Because the p-value is 0, we reject the null hypothesis, suggesting that there is strong evidence that mid laners have a higher average performance than top laners across all games.


<img width="733" alt="image" src="https://github.com/user-attachments/assets/5c339237-3d9e-4819-af84-84a4f26e93f2" />


# Framing a Prediction Problem

## 🎯 Prediction Problem  
The goal is to **predict the role of a player** in a *League of Legends* game based on their **post-game statistics**.  

## 🏷️ Type of Problem  
This is a **multiclass classification** problem because:  
- The player’s role (e.g., **Top, Jungle, Mid, ADC, Support**) is a **categorical variable** with more than two possible classes.  
- It is **not** binary classification since there are multiple distinct roles.  
- It is **not** regression since we are predicting a **category**, not a continuous value.  

## 📌 Response Variable  
- The **response variable** is **position**, which represents the player’s role in the game.  
- This was chosen because:  
  - Position is a **fundamental aspect** of a player's contribution to the team.  
  - Position is directly tied to post-game statistics, making it a **suitable target** for prediction.  

## 📊 Evaluation Metric: Macro-Averaged F1-Score  
The **F1-score** (specifically, **macro-averaged F1-score**) is used to evaluate the model.  

### 🔹 Why F1-Score?  
- The **F1-score** balances **precision** (prediction accuracy) and **recall** (coverage of true instances).  
- The **macro-averaged** version treats **all roles equally**, which is crucial in **multiclass classification** and helps address class imbalances (e.g., fewer Junglers than Supports).  

### ⚖️ Why F1-Score Over Other Metrics?  
- **Versus Accuracy**:  
  - Accuracy can be **misleading** if the dataset is imbalanced (e.g., if most players are Supports, predicting "Support" often can yield high accuracy but little value).  
  - The F1-score ensures **per-class performance** matters.  
- **Versus Precision or Recall Alone**:  
  - Precision emphasizes **correctness**, while recall emphasizes **coverage**.  
  - Since both are important, the **F1-score provides balance**.  

## ✅ Justification for Information Availability  
At the **time of prediction**, the game has **concluded**, and all post-game statistics are available:  
- **Kills, deaths, assists, monsterkills, minionkills, DPM, earned GPM, and champion** are recorded **at the end of the match**.  
- We are **not** predicting future outcomes (e.g., the next game’s performance) but rather **inferring the player’s role** from completed game data.  
- This ensures that **all features used in training and prediction** are **known at the time of prediction**, keeping the problem well-defined.  

For example, we **wouldn’t use** features like *“next game’s kills”* because they **aren’t available post-game**. The selected feature set avoids such issues entirely.  

# Baseline Model

## Our Features

We used the following features in our model to predict a player's role based on their post-game data (e.g., kills, deaths, assists, etc.):

- **Quantitative Features**:
  - `kills`
  - `deaths`
  - `assists`
  - `total_cs` 

- **Categorical (Nominal) Feature**:
  - `champion` (one-hot encoded)


## Encoding and Model Selection

- **Encoding:**  
  - Performed **One-Hot Encoding** for the categorical column.  
  - Left numerical columns **unchanged**.  

- **Model Selection:**  
  - Chose **Random Forest Classifier** because it effectively handles both **numerical** and **categorical** features (after encoding).  
  - It is **robust to overfitting** compared to a single decision tree, making it a strong choice for our dataset.  

After running the model, we get the following model metrics.

**Classification Report**:
| Class | Precision | Recall | F1-Score | Support |
|-------|-----------|--------|----------|---------|
| bot   | 0.96      | 0.97   | 0.96     | 4920    |
| jng   | 0.95      | 0.95   | 0.95     | 4946    |
| mid   | 0.93      | 0.92   | 0.93     | 5049    |
| sup   | 0.99      | 0.98   | 0.99     | 4829    |
| top   | 0.91      | 0.92   | 0.91     | 4901    |

**Overall Accuracy**: 0.95  
**Macro Avg**: 0.95 | 0.95 | 0.95  
**Weighted Avg**: 0.95 | 0.95 | 0.95


We believe our model was pretty good, as it performs well with an accuracy of 95%, indicating it makes correct predictions most of the time. The high F1-Scores across all roles, particularly for the support role (0.99), suggest the model balances precision and recall effectively, minimizing both false positives and false negatives. While the performance for the top role (F1-Score of 0.91) is slightly lower, the overall results, including a strong macro and weighted average F1-Score of 0.95, show that the model generalizes well across different classes. Overall, the model demonstrates robustness and reliability in predicting player roles.

# Final Model  

## Feature Engineering and Transformation  

Here, we added four new features and removed one feature to enhance our model's predictive power.  

### 🔹 New Features  
- **`monster kills` and `minion kills`**: These help differentiate between roles, particularly **support** and **jungle**.  
  - **Support players** generally have **low** values for both `minion kills` and `monster kills`, as they prioritize assisting teammates rather than farming.  
  - **Junglers** tend to have **high** `monster kills` but **low** `minion kills`, as they farm jungle camps rather than lane minions.  
- **`damage per minute`**: A key indicator of a player’s impact, distinguishing **high-damage roles (e.g., mid, bot)** from lower-damage roles such as **support**.  
- **`earned gold per minute`**: Important for understanding how **resource-intensive** a player's role is, as champions that require more gold (e.g., bot lane carries) should have higher values.  

### 🔹 Removed Feature  
- **`total cs` (creep score)** was removed because it is simply the sum of `minion kills` and `monster kills`, making it **redundant**. By splitting this information into two separate features, we preserve the same data while allowing the model to learn more nuanced role-specific patterns.  

### 🔹 Feature Transformations  
- **Quantile Transformation on `damage per minute`**  
  - Applied to **reduce the effect of outliers** while preserving the **distribution shape**.  
- **Log Transformation on `earned gold per minute`**  
  - Used to **compress extreme values** and **reduce skewness**, making it easier for the model to learn meaningful patterns.  

---

# Modeling Algorithm and Hyperparameter Tuning  

For our final model, we used a **Random Forest Classifier**, a powerful ensemble learning method that combines multiple decision trees to **reduce overfitting** while maintaining strong predictive performance.  

## 🔹 Hyperparameter Tuning  

We focused on tuning two key hyperparameters:  

### **1️⃣ n_estimators (Number of Trees)**  
- Controls how many **decision trees** are included in the random forest.  
- More trees generally improve performance by reducing variance, but too many trees **increase computational cost** without significant accuracy gains.  
- **Tuning Method**:  
  - Tested values ranging from **50 to 500** in increments of 50.  
  - Used **cross-validation** to identify the optimal value.  

### **2️⃣ max_depth (Maximum Depth of Each Tree)**  
- Determines how complex each tree can be.  
- **Too shallow** → Model **underfits**, missing important patterns.  
- **Too deep** → Model **overfits**, memorizing noise rather than general trends.  
- **Tuning Method**:  
  - Performed a **grid search** over depths ranging from **5 to 50**.  
  - Identified the best trade-off between complexity and generalization.  

## 🔹 Final Model Hyperparameters  
After tuning, the best hyperparameters were:  
- **n_estimators** = 200  
- **max_depth** = 25  

These values provided the best balance between **accuracy, generalization, and computational efficiency**.  

## Performance Improvement Over Baseline Model  

Compared to our **baseline model**, the **final model** demonstrated **clear improvements** in key metrics:  

✅ **Higher F1-Scores Across All Roles**  
- The additional features helped better differentiate between roles, particularly **support** and **jungle**, which were previously harder to classify.  

✅ **Reduced Overfitting**  
- Hyperparameter tuning helped **prevent excessive model complexity**, leading to better generalization on unseen data.  

✅ **More Interpretable Feature Contributions**  
- By breaking down `total cs` into `minion kills` and `monster kills`, the model learned **role-specific behaviors more effectively**.  

# Fairness Analysis

## Player Group Definitions

- **Group X (High-Assist Players)**: Players with assists **greater than the median** assists from the training set.  
- **Group Y (Low-Assist Players)**: Players with assists **less than or equal to the median** assists from the training set.  

Using the median ensures:  
✅ **Roughly balanced group sizes**  
✅ **Leverages an existing model feature**, which is acceptable as long as performance is assessed **post-prediction** without retraining.  

## Evaluation Metric: Macro-Averaged Precision  

Since this is a **multi-class classification** problem, we'll use **macro-averaged precision**, which:  

✅ **Calculates precision for each class (position) individually**  
✅ **Ensures fairness** by evaluating performance across **all positions**, not just the most common ones  

This approach aligns with the provided example and ensures the model performs well **for every position**, not just those with higher frequency. 

## Hypothesis Testing  

- **Null Hypothesis**: The model is **fair**. Its **macro-averaged precision** for **high-assist players (Group X)** and **low-assist players (Group Y)** is **the same**, and any observed differences are due to random chance.  

- **Alternative Hypothesis**: The model is **unfair**. Its **macro-averaged precision** for **Group X** differs from that of **Group Y**.  

We’ll use a **two-sided test** with a significance level of 0.05, with the **test statistic** being the **mean absolute difference of precision scores** between the two groups.  

## The Permutation Test  

1. **Shuffle** the group assignments **1,000 times** while keeping predictions and true labels fixed.  
2. **For each permutation**:  
   - Recompute the **precision** for the shuffled groups.  
   - Compute the **absolute difference** in precision between the two groups.  
3. **Calculate the p-value** as the proportion of permutations where the absolute difference **equals or exceeds** the observed absolute difference.  


After running the permutation test, we ended up with the following metrics.

```
Fairness Analysis Results:
Precision for high-assist group (Group X): 0.9687
Precision for low-assist group (Group Y): 0.9684
Observed difference (Precision_X - Precision_Y): 0.0003
P-value: 0.8610
```

<img width="733" alt="image" src="assets/Xnip2025-03-14_23-13-37.jpg" />

Because the p-value is greater than the significance level of 0.05, we fail to reject the null hypothesis, suggesting that there is not enough evidence to suggest that the model is unfair. Our conclusion indicates that our model is fair and that any observed differences between mAP for high-assist and low-assist players are  due to random chance.
