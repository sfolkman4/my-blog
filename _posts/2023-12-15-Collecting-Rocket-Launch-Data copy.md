---
title: "TheSpaceDevs API: Building a decades worth of rocket success"
layout: post
post-image: "https://revolutionized.com/wp-content/uploads/sites/5/2022/05/rocket-launch-at-sunset.jpg"
description: This post walks you through how I used Thespacedevs API to make a dataset on successful rocket launches from the past decade!
tags:
- Rockets
- SpaceX
- Python
---
### Rocket launches serve as milestones that speak of the great technological advancements that humans have made.

---
# Introduction:

When looking for an end of the semester project for my data science class, I wanted to do something I was super passionate about. I got frustrated with some of the APIs that I was trying to work with and I was dissapointed that I couldn't come up with a good project. I originally wanted to do something related to crypto or nfts, but kept finding that to get the exact data that I wanted I would have to pay. When talking with a close friend he suggested that I look into SpaceX and see if I could find any data on their successes. Although I didn't know much about SpaceX or rockets, I was excited to learn about it!

# Setup:

The API that I used to collect my data is from [TheSpaceDevs](https://thespacedevs.com/) which is a group of space enthusiasts that have the goal to improve public knowledge and accessibility of spaceflight information. 

Their Launch Library API is an entirely free to access database of rocket launches and space events. All API data is available at no cost but is limited to 15 requests per hour. 


```python
  #Imports 
  import requests
  import pandas as pd

  url = 'https://ll.thespacedevs.com/2.2.0/launch/previous/?status=3&year=2013,2014,2015,2016,2017,2018,2019,2020,2021,2022,2023&mode=list&limit=100'  #Launch endpoint with dates

```
This specific endpoint /2.2.0/launch is their new launch library. Adding specific years must be done individually like shown in the example above. They also have a few different options for how the data can be returned, but I found that the list mode gave me the information that I wanted without having to rummage through nested data. 

# API Request:

Since I am not an authenticated user, there is that rate limit of 15 requestes per hour and each request can only get me 100 results. After tinkering around with the API, I decided that I could get the last 10 years of successful launches at one time. To do that, I created the following while loop that calls the API and appends new data while also moving to the next page. This is what that looks like:

```
launch_data = []
while url:
    response = requests.get(url)
    if response.status_code == 200:
        data = response.json()
        launch_data.extend(data['results'])s
        url = data['next']  # Update URL for next page
    else:
        print('Failed to fetch data')
        break

# Creating a DataFrame from all the retrieved launch data
df = pd.DataFrame(launch_data)

```
This takes about a minute to run, but since I chose to use the list data and it is much less complex than the normal data, it is about 4 times as fast! 

# Data Cleaning:

After getting the data, I can see that my data frame has a lot of information. Much of this information will not be necessary when moving forward so I am going to remove it beforehand. 


```
# List of columns you want to remove
columns_to_remove = ["id", "url", "slug", "status", "last_updated", "net", "net_precision", "window_end", "launcher", "infographic", "type"]

# Removing the specified columns
df = df.drop(columns=columns_to_remove)

```

There are 4 different columns that all have dates, I chose to use the date of when the rocket window started which I assume was when it was launched or put on the API. I prefer this one because most of the rocket information is updated pretty frequently. I renamed this column as my date. I also changed the format so that it would have year-month-day and the time. Here is what that looks like with my final data frame.

```python
#Rename single date column
df = df.rename(columns={'window_start': 'date'})

# Convert string to datetime format
df['date'] = pd.to_datetime(df['date'])

# Convert the datetime to the desired format: 'YYYY-MM-DD HH:MM:SS'
df['date'] = df['date'].dt.strftime('%Y-%m-%d %H:%M:%S')

```
![Photo](https://github.com/sfolkman4/Rocket-Launches/blob/main/Rocket%20Data%20Frame.png?raw=true)


# Conclusion: 

Now that I have my cleaned data I am ready to explore all the successful rocket launches of the past decade! There are many exciting discoveries to make with this data and I hope I am able to learn more about what is happening around the world with rocket technology. 
