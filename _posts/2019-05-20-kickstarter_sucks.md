---
layout: post
title: "Does Your Kickstarter Suck?"
date: 2019-05-20
---
# A Study In Blind Ambition


Recently, some Kickstarter campaigns have been brought to my attention via the [Your Kickstarter Sucks Podcast](https://soundcloud.com/ykspod). I have been blown away by the sheer volume of bizarre and unnessary kickstarter campaigns that people sink valuable time and money into developing. Some of my favorites include [the Smart Spinner : The Most Unique and Versatile Fidget Spinner](https://www.kickstarter.com/projects/270360067/smart-spinner-the-most-unique-and-versatile-fidget?ref=discovery), [The Guac-e Talk-e](https://www.kickstarter.com/projects/638032122/the-guac-e-talk-e), and the classic [Mokase](https://www.kickstarter.com/projects/mokase/mokase-your-mobile-phone-cover-makes-even-coffee). I am setting out to analyze several features of Kickstarter campaigns, and create a prediction model to determine if the campaign will be a success or not. I hope to offer a way for kickstarter campaign managers to see if their idea is worth launching on the platform.


# Data Wrangling and Cleaning

I got my data from [webrobots.io](https://webrobots.io/kickstarter-datasets/) who have been scraping Kickstarter data since 2015. The data set was almost 50 gb of csv files, making analysis on the entire set unmanagable, so I selected a smaller set to make generalizations on the set as a whole. I also just included projects from the United States, to ensure that I can create a viable model without the confusion of comparing projects from different countries. 


# Data Exploration

The first thing I wanted to look at was the project categories. Which categories were most popular? Which categories had the highest success rate? 


![alt text](/../img/cat_counts.png "cat_counts")

This shows the most popular categories across the platform. 

![alt text](/../img/cat_plot.png "cat_ratio")

This shows a breakdown of categories by success. There are clearly some categories that are more likely to succeed than others. 




![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 1")







I have been interested in kickstarters campaigns, I wanted to see what if there was a pattern in what types of projects raise their crowdfunding goal or not, and try to create a model for predicting if your kickstarter will succeed. There was a small amount of data wrangling that needed to be done. Converting data to the correct data type, tossing out or reclassifying stray categories of campaigns, and splitting off active campaigns from the dataset. Ultimately, there are 323749 campaigns that we have in our dataset that we will use to explore what makes a campaign successful. I also have an auxiliary data set that includes information like description, whether or not the offering was “featured” or a “staff pick,”, as well as a direct link for the kickstarter. I can connect both sets of data with a commonly shared offering id. This auxiliary set of data will be useful in later stages of my analysis. 





-- without me:(  
4.02 Oakland CA / Octopus Literary Salon  
4.03 Fullerton CA / Programme Skate Shop  
4.04 Los Angeles CA / Rec Center  
4.05 San Diego CA / Terros Gallery  
4.06 Tucson AZ / Club Congress  
4.07 Santa Fe NM / Ghost  
4.08 Oklahoma City OK / House show  
4.10 St. Louis MO / Pizza Head  
4.11 Chicago IL / Bohemian Grove  
4.12 Madison WI / University of Wisconsin  
4.13 Milwaukee WI / Ground Zero  
4.17 Minneapolis MN / Moon Palace   

![alt text](/../img/hotline.png "hotline")
