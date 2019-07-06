---
layout: post
title: "Web Scraping Transfermarkt"
date: 2019-06-11
---


I was working on a project that predicted the success of Premier League teams depending on several variables. I figured that transfer statistics would be a good indication of success in the league. Buying expensive players indicates that you are buying good players, which will help a team win more points. To get this information, I wanted to scrape [transfermarket](https://www.transfermarkt.co.uk/) for this information. 



## Setting Up Environment

I used [Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/), a python based web scraping package to crawl through the pages of transfermarkt. I was using a Jupyter notebook because I wanted to convert the dataset into a human-readable csv file once I was finished. 



## Starting
```python
import pandas as pd
from bs4 import BeautifulSoup
import requests
```
These were the main packages I used during this exercise. 

My starting point for the web scraping was going to be the [2018 season homepage](https://www.transfermarkt.co.uk/premier-league/startseite/wettbewerb/GB1/plus/?saison_id=2018) where I could get most of the transfer information. I also had to fool transfermarkt into allowing my access by passing in header information that I was coming from a web browser. 

```python
headers = {'User-Agent': 
           'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/47.0.2526.106 Safari/537.36'}

page = 'https://www.transfermarkt.co.uk/premier-league/startseite/wettbewerb/GB1/plus/?saison_id=2018'

tree = requests.get(page, headers = headers)
#print(tree)
soup = BeautifulSoup(tree.content, 'html.parser')
```



## Pulling in Data by Season

Now that I had a beautiful soup object that contained all of the html from the yearly homepage, I need to search for the table containing the information I needed. I wanted to just get the url for each team, so I would be able to use it for future queries. 

```python
def build_by_year(year):
    page = 'https://www.transfermarkt.co.uk/premier-league/startseite/wettbewerb/GB1/plus/?saison_id=' + year
    tree = requests.get(page, headers = headers)
    soup = BeautifulSoup(tree.content, 'html.parser')
    spending_table = soup.select("tbody")[1]
    names = soup.select("td.hide-for-pad > a.vereinprofil_tooltip")
    season = []
    for name in names:
        temp = []
        temp_name = name.text.rstrip('FC').lower().rstrip()
        temp.append(temp_name)
        temp.append(name.get('href'))
        season.append(temp)
    return season
```
This function took a year, and used to to look up the teams in the particular season. I was able to find the name of the team as well as the link for a teams home page. 


With that link, I could query-specific teams transfer history. 


```python
def get_all_transfer_info_year(url, year):
    date_dict = {
    '18/19': '2018',
    '17/18': '2017',
    '16/17': '2016',
    '15/16': '2015',
    '14/15': '2014',
    '13/14': '2013',
    '12/13': '2012',
    '11/12': '2011',
    '10/11': '2010',
    '09/10': '2009',
    '08/09': '2008',
    '07/08': '2007',
    '06/07': '2006',
    '05/06': '2005',
    '04/05': '2004'}
    year_to_ret = ''
    for dash, whole in date_dict.items():
        if year == whole:
            year_to_ret = dash
    fixed_url = url.replace("startseite", "alletransfers")[1:]
    full_url = 'https://www.transfermarkt.co.uk/' + fixed_url 
    tree = requests.get(full_url, headers = headers)
    s = BeautifulSoup(tree.content, 'html.parser')
    incoming = s.select("td.redtext")
    in_and_out = []
    for idx, spent in enumerate(incoming):
        for parent in incoming[idx].parents:
            if parent.get('class') == ['box']:
                split = parent.text.split()
                if split[0] == 'Arrivals' and split[1] == year_to_ret:
                    in_and_out = [spent.text, '']
                    break
            
        # if we got here, we didnt find it!
    if not bool(in_and_out):
        in_and_out = ['0', '']
    outgoing = s.select("td.greentext")    
    for idx, sale in enumerate(outgoing):
        for parent in outgoing[idx].parents:
            if parent.get('class') == ['box']:
                split = parent.text.split()
                if split[0] == 'Departures' and split[1] == year_to_ret:
                    in_and_out[1] = sale.text
    
    if in_and_out[1] == '':
        in_and_out[1] =  '0'
    return in_and_out
```

This function went through all the years of a particular clubs transfer history and got how much the team spent and sold per year. I had to replace some of the url to get the appropriate location of the transfer information. One difficulty pulling this information in was that if a team sold or spent nothing, the information was the opposite color that you would expect. So I needed to just go year by year, asking for the specific year in the table, and getting both spent and sold values. This approach takes significantly more time than just getting all the team's transfer history in one url request, but since the data set was only for about 15 years, this wasn’t very expensive in my case. Once I had this information, I wanted to combine it with the point table at the end of the season. 



# Combining Tables

I had gotten the season tables for each year from a different source, so, unfortunately, the team names were slightly different from one another. For example, I could have Brighton and Hove Albion, Brighton & Hove Albion, or simply Brighton. This made matching team names together impossible just using string equality. I turned to a python package called [fuzzywuzzy](https://github.com/seatgeek/fuzzywuzzy). This package takes two string together and gives a similarity score out of 100. Since I knew all the possible names, I created a list of names I wanted to use for each club and matched them against the incompatible team names from Transfermarkt and my season tables.


``` python
def team_lookup(name):
    team_list = [
        'bournemouth',
        'arsenal', 
        'aston villa', 
        'birmingham city', 
        'blackburn rovers',
        'blackpool',
        'bolton wanderers', 
        'brighton hove albion', 
        'burnley', 
        'cardiff city', 
        'charlton athletic', 
        'chelsea', 
        'crystal palace', 
        'derby county', 
        'everton', 
        'fulham', 
        'huddersfield town',
        'hull city',
        'leicester city', 
        'liverpool', 
        'manchester city', 
        'manchester united', 
        'middlesbrough', 
        'newcastle united', 
        'norwich city', 
        'portsmouth', 
        'queens park rangers', 
        'reading', 
        'sheffield united', 
        'southampton', 
        'stoke city', 
        'sunderland',
        'swansea city',
        'tottenham hotspur', 
        'watford', 
        'west bromwich albion', 
        'west ham united', 
        'wigan athletic', 
        'wolverhampton wanderers'
    ]
    max_score = 0
    name_to_ret = ''
    for team in team_list:
        temp_score = fuzz.ratio(name, team)
        if temp_score > max_score:
            max_score = temp_score
            name_to_ret = team
        
    return name_to_ret
```


Using this function, I was able to combine all of the relevant data together into one pandas dataframe object. You can find the full table of data that I put together [here](https://www.kaggle.com/grahamcrbaker/premier-league-player-values). Let me know what you think!











