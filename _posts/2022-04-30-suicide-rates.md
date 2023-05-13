---
title: A Statistical Analysis of Mental Health in the U.S.
date: 2022-04-29 20:23:54.000000000 -07:00
layout: post
post-image: /assets/images/suicide/suicide.jpg
description: Visualizing Sucidie Rates
keywords: data visualization, sucidie rates, mental health
tags:
- data visualization
author:
  display_name: Johnny Okoniewski
  first_name: Johnny
  last_name: Okoniewski
permalink: "/2022/04/29/mental_health_analysis/"

---

I began this project with an interest in understanding the impact of the recent pandemic on people's mental health. However, due to the lack of easily accessible and reliable data on such a recent event, I shifted my focus to studying the changes in suicidal ideation rates over time. I aimed to identify any trends that could help determine which communities are disproportionately affected, so that effective resource allocation could be implemented to assist those in need.

To achieve this, I utilized the National Center for Health Statistics' National Vital Statistics System dataset labeled "Mortality." Firstly, I cleaned the raw data by removing irrelevant categories. Then, I normalized the data based on a common denominator of 100,000 people in the United States. This normalization helped to facilitate a more accurate comparison between different categories and states. Finally, I created visualizations using various tools such as Pandas, Matplotlib, and Excel. These visualizations presented the data in a more meaningful and accessible way, making it easier for the audience to comprehend the trends and patterns in the data.

![men](/assets/images/suicide/men.png)
![women](/assets/images/suicide/women.png)

The results suggest that there is a clear gender difference in suicide rates, with men being at a higher risk of committing suicide than women. The data shows that as men age, their suicide rates tend to increase, while women's rates drastically decrease once they reach the age of 65. When comparing suicide rates by age group, the highest rate of female suicide is almost half that of the lowest male age group, except for the age group of 10 to 14. The data also indicates that men who are 75 years and older are ten times more likely to commit suicide than women in the same age group. These findings can be used to inform suicide prevention efforts and target interventions to specific age and gender groups.

Although I expected to see a larger increase over time and spikes during times of economic turmoil, such as the housing market crash of 2008, my analysis did not find any evidence of this. However, this could be further explored by analyzing monthly suicide data over the past 20 years to gain a more precise view.

![age](/assets/images/suicide/age_breakdown.png)

The pie chart presents the age groups that are most susceptible to suicide. According to the chart, elderly and middle-aged individuals account for the largest portion of suicides, making them the most vulnerable. This finding was intriguing since I expected young adults to be more affected due to the disproportionate media coverage of their suicides. Moreover, the chart reveals that 25-44 year olds and 65-74 year olds have similar rates of suicide. What struck me the most was the high number of children under 14 years old who have taken their own lives, with around 2000 cases recorded over the last two decades.


![method](/assets/images/suicide/method.png)

Men tend to use firearms as a means of committing suicide, while women are more likely to use poisoning as a method. The graph highlights the difference in the proportion of the most commonly used method between genders. The largest proportion of female suicides is by poisoning, which is almost equal to the smallest proportion of male suicides. This indicates a significant difference in the most commonly used method of suicide between genders.

![non_fatal](/assets/images/suicide/non_fatal.png)

After conducting a further analysis of the data on non-fatal intentional self-harm injuries in Florida, I found that poisoning is a significant means of self-harm when compared to other methods such as suffocation, firearms, and others. This trend is consistent with national data that shows females are more likely to use poisoning as a means of self-harm. It is possible that this preference for poisoning among females is due to a desire to avoid causing physical harm or a perceived effectiveness of this method. However, despite choosing less lethal methods, females remain disproportionately unsuccessful in their suicide attempts, possibly due to factors such as seeking help more often or receiving medical intervention in a timely manner. Further research is needed to investigate the underlying reasons for these patterns and develop effective interventions to reduce the incidence of non-fatal intentional self-harm injuries among all demographics.

The findings from this project can inform suicide prevention efforts and target interventions to specific age and gender groups. Additionally, the data can help focus funding to groups that are at the highest risk to save the greatest number of lives possible. Further research can be conducted, taking into account race and income, to identify niche groups that are even more affected. The importance of this research cannot be overstated, as suicide remains a major public health issue that affects individuals and communities across the US. I am glad to have conducted this project as it highlighted how much more involved suicide is than previously thought. Suicide is a personal issue for me, as I have dealt with it in my own life and am constantly reminded of the growing mental health crisis in society today. I hope to be a small part of addressing this issue and figuring out how to help the most people possible. This project has taught me how data can be used to solve not only industry problems but societal and sustainability issues as well.

References: 
* https://www.flhealthcharts.gov/ChartsReports/rdPage.aspx?rdReport=ChartsProfiles.SuicideProfileDashboard
* https://www.cdc.gov/nchs/products/databriefs/db464.htm

---
