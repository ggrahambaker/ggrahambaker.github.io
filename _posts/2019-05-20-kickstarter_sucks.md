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

The main aspects of a campaign that I thought would determine 

The first thing I wanted to look at was the project categories. Which categories were most popular? Which categories had the highest success rate? 


<img src="/../img/cat_count.png" alt="drawing" width="80%"/>

This shows the most popular categories across the platform. 

<img src="/../img/cat_plot.png" alt="drawing" width="80%"/>

In red is the number of successful campaigns, in blue are failed. Film and Video, Music, Games, Comics, Dance, Theater, and Design all have more successul campaigns than not. This is somewhat suprising that the 

This shows a breakdown of categories by success. There are clearly some categories that are more likely to succeed than others. 



<img src="/../img/launched_at_plot.png" alt="drawing" width="80%"/>

I also wanted to see if there was a paticular month that had more sucessful campaigns. March, April, June, October and November all had more successful than failed campaigns, so timing also might be a significant factor in the success of a paticular project.

<img src="/../img/staff_pick.png" alt="drawing" width="80%"/>

A final element of interest is the staff pick, which is simply an endorsement from a staff member of Kickstarter. This often leads to more exposure for a paticular product. As you can see, endoresment of a project often leads to a successful campaign. 



With all of these features in mind, we can start using them to start creating models for predicting if a Kickstarter campaign will be successful or not.



# Model Fitting
I will use a variety of different methods to try to create a good model for prediction. 


## Numeric Values
#### Logistic Regression (Logit)
Logistic Regression is useful because it creates coefficients for each feature, meaning I can see which feature effects the model on the whole more clearly. 
#### Random Forest
I will use a random forest classifier as an alternative to Logistic Regression. Perhaps the random forest can expose trends that Logistic regression missed. 
## Text Values
#### Bayesian Classifier
We will use a bayesian classifier to look for common features among descriptions of the campaigns to see if text features indicate the success of a campaign. Bayesian Classifiers are often used for natural language processing problems like this because they are simple and offer good results. 

#### Random Forest
I will also be using random forest to see if any other patterns emerge from the text values that were not apperent in the bayesian classifier

## Ensemble Method
I will try to encorperate both types text and numeric values together to find if they work better together. 




# Measuring Success
I will use the AUC score to measure how well my model can identify successful and failed kickstarter campaigns. Our sample data has about a 50/50 split of successful and failed campaigns, so our baseline AUC score is 0.5. I will look to simply improve on that measurement.



### Logistic Regression
After running the logistic regression out of the box, 
<img src="/../img/importance_log.png" alt="drawing" width="80%"/>
<img src="/../img/roc_log_before.png" alt="drawing" width="80%"/>
<img src="/../img/roc_log_after.png" alt="drawing" width="80%"/>



### Random Forest Classifier

<img src="/../img/importance_random_forest.png" alt="drawing" width="80%"/>
<img src="/../img/roc_rfc_before.png" alt="drawing" width="80%"/>
<img src="/../img/roc_rfc_after.png" alt="drawing" width="80%"/>

![alt text](/../img/hotline.png "hotline")
