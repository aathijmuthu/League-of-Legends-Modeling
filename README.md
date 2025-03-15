# League-of-Legends-Performance-Modeling

# 📊 Introduction to the Dataset  

In this project, we explore how a player's **position** in *League of Legends* affects their **in-game performance** and how we can use **performance statistics** to **predict a player’s position**. *League of Legends* is a team-based strategy game where players take on distinct roles—**Top, Jungle, Mid, ADC, and Support**—each with unique responsibilities and playstyles.  

## ❓ Why Does This Matter?  
Understanding how **in-game performance** relates to **player position** has multiple valuable applications:  
- 🔹 **Competitive Insights:** Helps players and coaches analyze role-based performance trends.  
- 🔹 **Automated Role Detection:** Can improve matchmaking and gameplay analysis tools.  
- 🔹 **Esports & Game Analytics:** Supports data-driven strategies in professional and amateur play.  

## 📈 Dataset Overview  
The dataset consists of **[X] rows**, where each row represents a **player’s performance in a completed game**. The key columns relevant to our analysis include:  

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

