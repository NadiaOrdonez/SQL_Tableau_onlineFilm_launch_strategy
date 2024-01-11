# SQL_onlineFilm_launch_strategy
This repository serves as a demonstration of the refined SQL syntax applied in combination with visuals crafted in Tableau in the context of a comprehensive data analysis project. The analysis presented herein is instrumental in assisting Rockbuster Stealth LLC, a film company, as it formulates a strategic plan for the successful launch of an online video service [^1]. 
## Project Overview
### Motivation
In response to fierce competition from streaming giants like Netflix and Amazon Prime, Rockbuster Stealth is leveraging its extensive film licenses to launch a cutting-edge online video rental service.
### Objective
Address business questions posed by the Rockbuster Stealth Management Board, guiding the 2020 launch company strategy. This includes customer demographics, film preferences, and sales figures.
#### Business questions 
1. Which films contributed the most/least to sales?
2. What was the average rental duration for all films?
3. Which countries are Rockbuster customers based in?
4. Where are customers with a high lifetime value based?
5. Do sales figures vary between geographic regions?
### Scope
The data-driven insights on customer behavior and sales trends are extracted from the company database and will aid in shaping the 2020 launch strategy for an online service.
## Database overview
The entity relationship diagram (ERD) for our company database was constructed using DbVisualiser [see ERD](ERD_database_NadiaOrdonez.png). This illustrates the links between the tables in our available relational database used for the data analysis. A detailed description of the tables, primary keys, foreigner keys and how our tables are linked please refer to the [data dictionary](Data_dictionary_NadiaOrdonez.pdf) 
## SQL commands
* Main SQL syntax
  * Statistical descriptions were achieved using main SQL commands in combination with aggregations and operators, e.g. [see Data Overview](Data_overview_NadiaOrdonez.md).
* Subqueries & CTE
  * A CTE was created and applied in the `FROM` command in the [Data overview](Data_overview_NadiaOrdonez.md) file. 
  * Subqueries and CTEs were applied to answer business questions 4 and 5 in the `FROM` and `WHERE` command , see [Business questions](Business_questions_NadiaOrdonez.md) file.  
* Create tables
  * A continent table was created to address business question 5 about sales per geographic region, see [Business questions](Business_questions_NadiaOrdonez.md) file.   
* Joints
  * The joints commands were frequently used to address [business questions](Business_questions_NadiaOrdonez.md) as well as in the [data overview](Data_overview_NadiaOrdonez.md) analyses.

ChatGPT 3.5 was used to facilitate and speed the data analysis process. 
## Tableau visuals
After executing SQL queries, I utilized Tableau to create compelling visualizations that not only illustrate our findings but also provide robust support for the recommendations outlined in our launch strategy. Within Tableau, I seamlessly integrated individual graphs using dashboards and story features for a comprehensive presentation. Use this [linkk](https://public.tableau.com/app/profile/nadia.ordonez/viz/Rockbuster_tableau/Rockbusterdataanalyses?publish=yes) to access the visuals.
## Launch strategy recommendations 
Details about the project brief summarizing the data analyses, business questions and recommendations are compiled in the [Rockbuster ppt](Rockbuster_ppt_NadiaOrdonez.pdf) file. These recommendations are supported by our data analysis process.
* Stop film licenses for films with none and low sales and instead invest in:
  * High-selling films:
    * Top 3 categories: Drama, New, Games
    * Top rating: PG-13
    * Top 3 actors: Gina Degeneres, Matthew Carrey, and Mary Kaitel
  * Consider including more thrillers to offer a well-balanced mix of categories.
* The Asia market has high customer engagement and sales, making it a prime candidate for a pilot online video rental service launch.
* Analyze customer film views at the country/customer level to satisfy their preferences.
* Explore how the popularity of certain types of content (categories, ratings, actors, etc.) changes over time after our launch strategy to align our film content acquisition strategy accordingly.
* Analyze the strategies and content offerings of competitors in the online video rental space to identify potential tactics that attract new customers.
  
[^1]: Database provided by CareerFoundry in the Data Analytics program. Data and company details are of fictional nature. 
