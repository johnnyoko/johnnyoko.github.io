---
title: Using Machine Learning to Optimize Impact of Election Donations
layout: post
post-image: /assets/images/campaign_fin/campaign_finance.jpg
description: Demonstrating the impact of campaign donations on the House of Representatives.
keywords: machine learning, data visualization, campaign finance
tags:
- Machine Learning
- data visualization
author:
  display_name: Johnny Okoniewski
  first_name: Johnny
  last_name: Okoniewski
permalink: "/2023/04/28/campaign_finance_predictions/"

---
The purpose of this [project](https://github.com/johnnyoko/campaign_finance_donation_optimization.git) is to address candidates who are struggling to fund their campaigns, even if they have strong beliefs and policies that align with voters' interests by providing a platform that helps users to make informed decisions when voting for candidates. Specifically, the project aims to highlight candidates who are struggling to fund their campaigns and would benefit the most from donations. By doing so, the project seeks to level the playing field and ensure that candidates with strong beliefs and policies have a better chance of winning, regardless of their financial resources.

I helped develop a [platform](https://pavlicag-campaign-finance-donation-optimiz-streamlitmain-iuzsxf.streamlit.app/) that used a machine learning model to predict the winners of the 2022 House of Represenatives election only using campaign finance data in conjunction with Thao Nguyen, Vivian Pavlica, Ben Ramsey, and Gregory Zavalnitskiy.

![flowchart](/assets/images/campaign_fin/flowchart.jpg)

The study utilized data from the Federal Election Commission (FEC), FiveThirtyEight, and OpenSecrets to construct a comprehensive understanding of the numerous factors that contribute to election outcomes. The analysis focused on the 2022 House of Representatives elections, given the high availability and significant correlation between campaign finance and electoral success. Key variables, such as total funds raised, total expenditures, incumbency, and predicted win percentages, were integrated into a predictive model to rank candidates based on their potential impact. Ideological alignment data collected from FiveThirtyEight was also incorporated to facilitate user selection of candidates. The findings were visualized using standard Python data science libraries, providing users with an intuitive and user-friendly interface.

The analysis revealed a strong correlation between campaign financing and electoral outcomes, with 93% of candidates who raised more money emerging victorious in contested races. This underscores the need for greater transparency and accountability in campaign finance, which can be seen looking into the total distribution of money versus win percentage.

![graph](/assets/images/campaign_fin/combined_disbursement.png)

This project successfully created a platform to address the challenges related to political participation and decision-making by highlighting struggling candidates who would benefit the most from donations based on their beliefs and state. The project's methodology utilized a predictive model created from data on the 2022 House of Representatives elections to identify key candidates and was visualized using standard Python data science libraries. The analysis revealed a strong correlation between campaign financing and electoral outcomes and underscored the need for greater transparency and accountability in campaign finance. However, the project encountered several challenges such as technical issues, effective communication, collaboration, leadership skills, ethical considerations, time management, and project organization. Future work could involve expanding the scope of the platform to include other elections like local elections or developing additional features to facilitate greater user engagement and understanding of the political process.

---
