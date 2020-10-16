---
layout: post
title: "PD Tracker"
date: 2020-10-10
---




# [PD Tracker](https://pd-tracker1.herokuapp.com/)
[github](https://github.com/ggrahambaker/product_tracker)
# What is this thing?

I wanted to learn more about flask as well as the ecosystem that surrounds the python web development world, so I decided to make an app that would introduce and solidify my understanding. The goal of the project was to create a "product tracker", or essentially a platform for people to discuss and present research regarding a paticular product. Originally, the idea was for specifically finacial instuments like stocks, bonds or anything else you can invest in, but potentially can be used for anything! It is intended for a closed group of users, where an instance of the application is only available to a select few instead of the internet at large. I opted to use heroku to host the site, leveraging a bunch of the easy to use plugins to add all the functionality I wanted. 


# What were the Requirements?

### User Authentication
A basic requirement but a good way to flex some of those authentication muscles. Flask has a bunch of handy user authentication tracking add ons, but I used [Flask-Login](https://flask-login.readthedocs.io/en/latest/). Most of the site is behind a user authentication check, so if you actually want see what kind of stuff it does, make sure you make an account. I added password recovory and some other convienice functions. 




### Posting Assets
I wanted users to be able to create an asset easily with a name, a short description, as well as be able to upload relevant documents. I used the Amazon S3 service to host the uploaded docuements. 


### Searching
The ability to search for assets is crucial for this type of application. While a user can just scroll through all of the assets on their home page, its a lot easier just to be able to quickly get results from the search bar. I used elasticsearch to add this functionality to the site. 






### Future Features
I would like to add an export asset feature, where a user can get the asset discussion including the description, attached documents, as well as the comments from other users sent to eamil.




# Acknowledgments

I based this project off the wonderful series created by Miguel Grinberg that you can find [here](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world). 