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

<div id="container">
	<img src="/../img/cat_count.png" alt="drawing" width="80%"/>
</div>


This shows the most popular categories across the platform. 
<div id="container">
	<img src="/../img/cat_plot.png" alt="drawing" width="80%"/>
</div>
In red is the number of successful campaigns, in blue are failed. Film and Video, Music, Games, Comics, Dance, Theater, and Design all have more successul campaigns than not. This is somewhat suprising that the 

This shows a breakdown of categories by success. There are clearly some categories that are more likely to succeed than others. 


<div id="container">
	<img src="/../img/launched_at_plot.png" alt="drawing" width="80%"/>
</div>

I also wanted to see if there was a paticular month that had more sucessful campaigns. March, April, June, October and November all had more successful than failed campaigns, so timing also might be a significant factor in the success of a paticular project.

<div id="container">
	<img src="/../img/staff_pick.png" alt="drawing" width="80%"/>
</div>
A final element of interest is the staff pick, which is simply an endorsement from a staff member of Kickstarter. This often leads to more exposure for a paticular product. As you can see, endoresment of a project often leads to a successful campaign. 



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
I will also be using random forest to see if any other patterns emerge from the text values that were not apperent in the bayesian classifier

# Ensemble Method
I will try to encorperate both types text and numeric values together to find if they work better together. 




# Measuring Success
I will use the AUC score to measure how well my model can identify successful and failed kickstarter campaigns. Our sample data has about a 50/50 split of successful and failed campaigns, so our baseline AUC score is 0.5. I will look to simply improve on that measurement.



### Logistic Regression

<div id="container">
	<img src="/../img/roc_log_before.png" alt="drawing" width="80%"/>
</div>


After running the logistic regression out of the box, I got an AUC score of .61. Since this score is only .10 above the null value we are looking for, we will tune our parameters to fit the logistic classifier better. 






<div id="container"> 
	<img src="/../img/importance_log.png" alt="drawing" width="80%"/>
</div>


I also was interested in the importance of the fields in the classification model. Using a RFE classifier allowed me to expose the most influencial fields. 


## Tuning Parameters
<div id="container">
	<img src="/../img/roc_log_after.png" alt="drawing" width="80%"/>
</div>

I used a Grid Search to search for C, a parameter that helps regularize the fit of the regression. After tuning, the AUC score was brought up to .70, which is a pretty good increase from the null AUC. I thought I would try to use a different classifier to get a more accurate model.


## Random Forest Classifier
The random forest classifier has the ability to find harder to find patterns in our data through its randomness. 


<div id="container">
	<img src="/../img/roc_rfc_before.png" alt="drawing" width="80%"/>
</div>


Out of the box, the AUC of the random forest is higher than the Logistic Regression classifier. However, this model had some overfitting problems. Meaning it was too good at classifieying the training data set, but couldn't make generalizations about the test data set it had never seen. I need to tune the parameters to make a better fitting classifier. 


<div id="container">
	<img src="/../img/importance_random_forest.png" alt="drawing" width="80%"/>
</div>

Using the RFE classifier again, I wanted to see the most important fields in the random forest classifier. The most important fields were the exact opposite of the logistic classifier. This makes sense because the random forest randomly decides which field to make decisions on, making the most important fields pretty much random. 
<div id="container">
	<img src="/../img/roc_rfc_after.png" alt="drawing" width="80%"/>
</div>

After tuning parameters like max depth and number of trees generated, I had greatly reduced overfitting and got an AUC score of .83. This is a pretty good improvement from where we started.


# Natural Language Processing

I will be using the description of the kickstarter project to see if there is useful attibutes in predicting success of a campaign. I will use a CountVectorizer, to create a way for classifiers to interpret the text data.


## Count Vectorizer
I will use a count vectorizer to convert the discriptions of the projects into a sparse matrix for me to conduct analysis on. I will be using a "bag of words" technique where I dont care about order of words, just frequency and relationship to discriptions of other projects. I will have to do a little bit on tuning on the Count Vectorizer to get the most accurate results I can. 
The Count Vectorizer creates a dictionary underneath the sparse matrix that keeps track of the words that are in all the descriprions. Keeping only words that appear a certain amount of discriptions decreases noise in analysis. Also grouping words together in pairs as one dictionary entry helps identify important phrases, adding a little more sophistication to our "bag of words" technique. 



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


Using the same vectorized words, got a decent AUC score, but we got significant overfitting. The accuarcy on the training data was 98.21 percent, and test data was only 63.75 percent. We need to tune our parameters!
<div id="container">
	<img src="/../img/roc_word_forest_before.png" alt="drawing" width="80%"/>
</div>


<div id="container">
	<img src="/../img/roc_word_forest_after.png" alt="drawing" width="80%"/>
</div>






