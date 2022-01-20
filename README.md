# Squadstack-Agent-Insights

# Description of files for this repo
#### README.pdf : Problem Statement
#### Agent Insights -Solution.ipynb : The Jupyter Notebook containing all the insights of the agent's performance.

### Dataset
   - agent_followup_data.csv : Input Dataset .
   - glossary : description of all the columns of dataset.

#### Graphical Representations of all the factors considered for agent's performance
  - Number of Follow ups each Agent.png 
  - Percentage of Follow Ups.png
  - Number of Distinct Leads each Agent.png
  - Distribution of Followup Type each Agent.png
  - Distribution of Delay in Follow up.png
  - Median delay in follow up per agent
  - Total number of times an Agent gets a call from a lead

# Real Estate Analysis Report

Please use the following structure as a guide:
- All scripts are in the script folder.
- All visualizations are in the viz folder.
- data folder contains the dataset.
- assets folder contains miscellaneous screenshots for the report. 

### Motivation 

The aim of this project is to analyse the given real estate dataset and provide a scoring criteria to measure an agents performance.
In this analysis, I have tried to wrangle the dataset provided from the CRM platform and have used this dataset to come up with a agent performance scoring criteria and use this algorithm to choose the Top 3 agents in the brokerage firm. 

I have also included suggestions for the brokerage firms to improve their agent's performances.

# Table of Contents

- [Background](#Background)
- [Exploring the Data](#Exploring-the-Data)
- [Analysis](#Analysis)
- [Scoring Criteria](#Scoring-Criteria)
- [Conclusion](#Conclusion)
- [Suggestions](#Suggestions)

## Background

A Real estate agent works under a licensed broker (or brokerage). The brokerage provides the real estate agent with leads (leads are people who are looking to buy, sell or rent), and the agent's job is to help them in their home buying, selling or renting process. 
They do this by regularly following-up with them using calls, text messages or emails. After every follow-up with a lead, the activity is recorded on the CRM(CRM is a Customer Relationship Manager, which is a tool that agents use to log data for a particular lead).

## Exploring the Data

The dataset provided comes from a CRM platform used by the brokerage firm. The dataset has a CSV file format. The following is a screenshot of data along with the definitions of each column in the dataset :

![Example Dataset]

- ***id:*** Unique followup identifier
- ***followup_date:*** Timestamp when the followup happened
- ***lead_created_at:*** Lead creation timestamp on the CRM
- ***leadId:*** Unique id of the lead with whom followup happened
- ***followup_type:*** Type of followup - Call/ Email/ Text
- ***agentId:*** Agent who followed up with the lead
- ***additional_data:*** Additional data associated with the followup 
    - ***duration:*** Time for which call lasted in seconds
    - ***isIncoming:*** False means agent made the Call/ Email/ Text

The columns *follow_up_date* and *lead_created_at* should be **datetime**. Columns *id*, *leadId*, *agentId* are **integers** and rest are **strings**. 

The dataset provided has no null values and hence means it is a clean dataset with no data imputation needed initially. 
But, data has been manipulated as per the analysis performed on it. 

## Analysis

The following libraries have been used for the analysis process :
- Pandas
- Seaborn
- Matplotlib

At first glance of the data, we can see that each row corresponds to a follow up made by an agent to their leads. There can be multiple rows where one agent makes a follow up to the same lead over and over again. This is normal in real estate scenario as an agent keeps going back and forth with their potential customers. 

Thus, the first step in my analysis was to count the number of follow ups done by an agent. On its own, the count of follow ups of each agent doesn't necessarily mean that they are better agents. It could be just that they made multiple calls/mail/text to a single lead. 

![Follow ups](https://github.com/hemantrattey/Squadstack/blob/master/viz/Number%20of%20Follow%20ups%20per%20Agent.png)

We can see that Agent 4 made the most number of follow ups. This also comes out to be around 22% of the total follow ups. On the other hand, Agent 0 made the least number of follow ups which was around 0.17%. 

![perc_follow_ups](https://github.com/hemantrattey/Squadstack/blob/master/assets/perc_follow_ups.JPG)


But, as discussed above, it doesn't make sense to show number of follow ups on its own. Another performance indicator could be the unique number of leads each agent has. 

![Leads](https://github.com/hemantrattey/Squadstack/blob/master/viz/Number%20of%20Distinct%20Leads%20per%20Agent.png)

Again, the graph shows that Agent 4 has the most number of leads and Agent 0 has the least number of leads. This still doesnot guarantee a great performance indicator. Instead, it could just mean that the brokerage has put more trust on Agent 4 as they have provided them with the most leads and Agent 0 might be a new agent hence has least number of leads. 

I went deeper into the data and found that there is a Date when the lead is created and a follow up date. This could provide an idea about how long does each Agent take to follow up to their lead. A higher value means they are taking a lot of days to follow up and the lead might not be satisfied with this agent. 

Since, the follow_up_delay (days) is a highly right skewed distribution, it would make sense to look at the median rather than mean as an indicator because median is not affected by outliers. 

![histogram](https://github.com/hemantrattey/Squadstack/blob/master/viz/Distribution%20of%20Delay%20in%20Follow%20up.png)

The following is the median number of days taken by each agent to follow up to their leads. 

![median](https://github.com/hemantrattey/Squadstack/blob/master/viz/Median%20delay%20in%20follow%20up%20per%20agent.png)

As you can see, Agent 5 takes only 8 days to get back to their leads whereas Agents 3 and 7 take more than a year to get back to their leads. This might mean that the potential customers aren't happy with agents 3 and 7 as they don't want to be kept waiting for such a long time. 

![median_delay](https://github.com/hemantrattey/Squadstack/blob/master/assets/median_delay.JPG)

If the median delay is decreased then surely the potential customers would be satisfied with the agents and thus increasing the profits of the brokerage firm. 

Additionally, the follow up type is a good indicator of performance as leads are more likely to reply to a call immediately rather than an email or a text message. Thus agents calling up their leads might have better success rate than the others. 

![follow_up_type](https://github.com/hemantrattey/Squadstack/blob/master/viz/Distribution%20of%20Followup%20Type%20per%20Agent.png)

It is clear that Agent 4 spends a lot of time calling their leads and texting them whereas Agent 5 relies on waiting for replies on their mails. 

Finally, if an agent receives a call from their lead, this means that the potential customer is interested in doing business with the brokerage firm and this shows a positive sign in agents performance. 

![incoming](https://github.com/hemantrattey/Squadstack/blob/master/viz/Total%20number%20of%20times%20an%20Agent%20gets%20a%20call%20from%20a%20lead.png)

Agent 7 gets a lot of calls from their leads and it might show that he is good at his job and converting leads to customers. On the other hand, agent 3 hardly receives any calls from their leads. 

## Scoring Criteria

For scoring the agents performances, I took the following into consideration :

- Number of follow ups per agent
- Number of leads per agent
- Median number of days delay in following up
- Number of times an agent gets a call from their leads
- Follow up type used by each agent

The idea behind the scoring criteria is to assign values from 0-10 based on each criteria individually where 0 means worst performance and 10 means best performance. After assigning ranks, I will total them up and find the top 3 agents having the most score. 

*For median number of days delay in following up, a value of 10 would mean that the agent followed up quickly whereas a value of 0 would mean that agent took a long long time to follow up.*

*Also, since calls are most likely to be answered immediately by a lead, I assigned a value of 3 to calls and 2 to texts and 1 to emails as leads would take the most time to reply to an email.* 


## Conclusion

Evaluating the agents based on the scoring index described below,  ***Agent 4***, ***Agent 10*** , ***Agent 9*** and ***Agent 5*** were the top 4 agents in that order. The following is a screenshot of the scoring values for each agent :

![rank](https://github.com/hemantrattey/Squadstack/blob/master/assets/rank.JPG)

## Suggestions

This was a quick analysis done on the dataset provided. But some of the following recommendations can be considered to improve: 

- Get a dataset containing details whether or not a lead purchased a property and along with the cost of the property. This could help us understand how much sales an agent is bringing into the brokerage. 

- To improve performance of other agents, the number of follow ups along with the number of distinct leads per agent should be increased as currently a lot of imbalanced data is present and currently Agent 4 has the most number of leads and consequently has the most number of follow ups. 

- Another suggestion to improve performance would be to call the leads up rather than mailing them. Calling is effective as Agent 4 calls the most number of leads and has the highest performance score. Agent 5 should switch from mailing to calling as this could boost up their performance score a lot. 

- Finally, agents should not take a long time to get back to their leads. Potential customers want that agents focus on them and pay attention to them. As a result, if an agent takes a lot of time to get back to their leads, the leads would feel unsatisfied with the agents and the brokerage firm and might end up leaving and not doing business thus decreasing profits for the firm. 


