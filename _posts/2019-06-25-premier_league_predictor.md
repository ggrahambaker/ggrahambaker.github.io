---
layout: post
title: "Premier League Predictor"
date: 2019-06-25
---


# Premier League Predictor


As a fan of Liverpool Football Club, a Premier League football team that narrowly came in second place in the 2018-19 season, I wanted to see there was a way to predict how many points a team would earn over the course of a season. I will be looking at squad worth, average attendance of home game, manager history, performance over the course of the last season, as well as other important attibutes of a team. Using this information, I hope to get accuarate predictions for how well a team will do in a premier leage season.


### Collecting Data
This data was wildly available online for me to use, however, it was difficult to find any in csv, api or more managable form. I had to scrape the information from transfermarkt.com in order to get get what I needed. This process was difficult for a variety of reasons. First, the website was created in Germany, meaning all of the tags and notes inside of the webpage were all in German. This made understanding what was going on a little more difficult. Next, the website did not follow the tightest html standards, and this caused me difficulties in a number of situations. Finally, since Germany uses commas to delimit decimals, and periods to show value in large numbers, it took some time to reformat this data to be used appropriately.

A discussion of the process can be found [here](https://ggrahambaker.github.io/blog/2019/06/11/webscraping-transfermarkt). 



##  Exploratory Data Analysis
Once I had put together the data, I wanted to look at some key statistics. I had information from the last 14 premier league seasons, since that was the first year transfermarkt started publishing player values. 



### Total Points Per Club

<div id="container">
	<img src="/../img/total_points_per_club.png" alt="drawing" width="80%"/>
</div>

You can see the classic "Top 6", a term of the perennial top 6 finishing teams. This group is composed of Liverpool, Arsenal, Manchester United, Manchester City, Chelsea and Tottenham. These six teams are the highest performing teams, and have not been relegated in the time period I was looking at. Everton is another team that has not been relegated in this time frame, but does not have the same reputation as the "Top 6."  





### Money Spent on Players Per Club

<div id="container">
	<img src="/../img/total_bought_by_team.png" alt="drawing" width="80%"/>
</div>


I also wanted to see the highest spending clubs during this time frame. I would assume that the clubs that spent the most on players, would have the best team meaning that they would earn the most points over the course of a season. This theory fits with this visualization because the total number of points is similar 



# Data Exploration

The first thing I wanted to look at was the project categories. Which categories were most popular? Which categories had the highest success rate? 




This shows the most popular categories across the platform. 
<div id="container">
	<img src="/../img/cat_plot.png" alt="drawing" width="80%"/>
</div>
In red is the number of successful campaigns, in blue are failed. Film and Video, Music, Games, Comics, Dance, Theater, and Design all have more successful campaigns than not. This is somewhat surprising that the 

This shows a breakdown of categories by success. There are clearly some categories that are more likely to succeed than others. 


<div id="container">
	<img src="/../img/launched_at_plot.png" alt="drawing" width="80%"/>
</div>

I also wanted to see if there was a particular month that had more successful campaigns. March, April, June, October and November all had more successful than failed campaigns, so timing also might be a significant factor in the success of a particular project.

<div id="container">
	<img src="/../img/staff_pick.png" alt="drawing" width="80%"/>
</div>
A final element of interest is the staff pick, which is simply an endorsement from a staff member of Kickstarter. This often leads to more exposure for a particular product. As you can see, endorsement of a project often leads to a successful campaign. 



With all of these features in mind, we can start using them to start creating models for predicting if a Kickstarter campaign will be successful or not.



# Model Fitting
I will use a variety of different methods to try to create a good model for prediction. 


# Numeric Values
### Logistic Regression (Logit)
Logistic Regression is useful because it creates coefficients for each feature, meaning I can see which feature effects the model on the whole more clearly. 
### Random Forest
I will use a random forest classifier as an alternative to Logistic Regression. Perhaps the random forest can expose trends that Logistic regression missed. 
# Text Values
### Bayesian Classifier
We will use a bayesian classifier to look for common features among descriptions of the campaigns to see if text features indicate the success of a campaign. Bayesian Classifiers are often used for natural language processing problems like this because they are simple and offer good results. 

### Random Forest
I will also be using random forest to see if any other patterns emerge from the text values that were not apparent in the bayesian classifier

# Ensemble Method
I will try to incorporate both types text and numeric values together to find if they work better together. 




# Measuring Success
I will use the AUROC or AUC for short, score to measure how well my model can identify successful and failed kickstarter campaigns. This is a way to measure true positive rates (successful campaign), against false positive rates (identifying a failed campaign as successful) The closer the AUC is to 1, the better it is. For more information on AUC curves, check [here] (https://en.wikipedia.org/wiki/Receiver_operating_characteristic)

Our sample data has about a 50/50 split of successful and failed campaigns, so our baseline AUC score is 0.5. I will look to simply improve on that measurement.




### Logistic Regression

<div id="container">
	<img src="/../img/roc_log_before.png" alt="drawing" width="80%"/>
</div>


After running the logistic regression out of the box, I got an AUC score of .61. Since this score is only .10 above the null value we are looking for, we will tune our parameters to fit the logistic classifier better. 




<div id="container"> 
	<img src="/../img/importance_log.png" alt="drawing" width="80%"/>
</div>


I also was interested in the importance of the fields in the classification model. Using a RFE classifier allowed me to expose the most influential fields. 


## Tuning Parameters
<div id="container">
	<img src="/../img/roc_log_after.png" alt="drawing" width="80%"/>
</div>

I used a Grid Search to search for C, a parameter that helps regularize the fit of the regression. After tuning, the AUC score was brought up to .70, which is a pretty good increase from the null AUC.  However the accuracy is only 63.16 percent, so I will try to use a different classifier to get a more accurate model.


## Random Forest Classifier
The random forest classifier has the ability to find harder to find patterns in our data through its randomness. 


<div id="container">
	<img src="/../img/roc_rfc_before.png" alt="drawing" width="80%"/>
</div>


Out of the box, the AUC of the random forest is higher than the Logistic Regression classifier. However, this model had some overfitting problems. The accuracy on the training data is 89.57 percent while the accuracy on the test data is 70.98. Meaning it was too good at classifying the training data set, but had a difficult time making generalizations about the test data set it had never seen. I need to tune the parameters to make a better fitting classifier. 


<div id="container">
	<img src="/../img/importance_random_forest.png" alt="drawing" width="80%"/>
</div>

Using the RFE classifier again, I wanted to see the most important fields in the random forest classifier. The most important fields were the exact opposite of the logistic classifier. This makes sense because the random forest randomly decides which field to make decisions on, making the most important fields pretty much random. 
<div id="container">
	<img src="/../img/roc_rfc_after.png" alt="drawing" width="80%"/>
</div>

After tuning parameters like max depth and number of trees generated, I had greatly reduced overfitting while improving accuracy to 74.08 percent and got an AUC score of .83. This is a pretty good improvement from where we started.


# Natural Language Processing

I will be using the description of the kickstarter project to see if there is useful attributes in predicting success of a campaign. I will use a CountVectorizer, to create a way for classifiers to interpret the text data.


## Count Vectorizer
I will use a count vectorizer to convert the descriptions of the projects into a sparse matrix for me to conduct analysis on. I will be using a "bag of words" technique where I don’t care about order of words, just frequency and relationship to descriptions of other projects. I will have to do a little bit on tuning on the Count Vectorizer to get the most accurate results I can. 
The Count Vectorizer creates a dictionary underneath the sparse matrix that keeps track of the words that are in all the descriptions. Keeping only words that appear a certain amount of descriptions decreases noise in analysis. Also grouping words together in pairs as one dictionary entry helps identify important phrases, adding a little more sophistication to our "bag of words" technique. 



## Naive Bayes
This classifier is often paired with natural language processing and provides accurate results for this type of classification problem. 

Out of the box, our classifier performs decently. It is overfitting the training data more than I would like, so I make it better through parameter tuning for both the Count Vectorizer and the Bayes classifier.
<div id="container">
	<img src="/../img/roc_bayes_before.png" alt="drawing" width="80%"/>
</div>


After tuning, the classifier is not overfitting at all, which is great! However our AUC is still not that great. At only .73, maybe we should try a different classifier to get better results. 

<div id="container">
	<img src="/../img/roc_bayes_after.png" alt="drawing" width="80%"/>
</div>

We also were able to pull out the words that implied the most successful campaigns as well as the ones that implied a failed campaign. 

<div id="container">
	<img src="/../img/good_bad_words.png" alt="drawing" width="80%"/>
</div>

# Random Forest

We will use random forest again to see if it yields more success! 


Using the same vectorized words, got a decent AUC score, but we got significant overfitting. The accuracy on the training data was 98.21 percent, and test data was only 63.75 percent. We need to tune our parameters!
<div id="container">
	<img src="/../img/roc_word_forest_before.png" alt="drawing" width="80%"/>
</div>


After finding the best parameters, our accuracy for test and train came a lot closer to each other, and slightly increased our test data accuracy to 64.13 percent. This is only 15 percent better than randomly choosing, so I think I will stick with the Naive Bayes classifier for the descriptions. 

<div id="container">
	<img src="/../img/roc_word_forest_after.png" alt="drawing" width="80%"/>
</div>



# Voting Classifier

Since I want to use both the numeric and text data to try to predict if a Kickstarter campaign will be successful or not, I will use a voting classifier to blend the two methods together. 'Soft' voting means that the average prediction between the two models will be counted as the prediction for the entire classifier. 


<div id="container">
	<img src="/../img/roc_vote.png" alt="drawing" width="80%"/>
</div>


# Conclusion

Through creating these classifiers, we have achieved a combined score of .81. This is a pretty significant improvement from where we started, but generally still not super accurate. Since the features I was working with have no intrinsic measure about how good a particular project will do in fundraising, the model had to generalize pretty significantly about the attributes. For a larger improvement, more features would be needed to make a better estimate. 






