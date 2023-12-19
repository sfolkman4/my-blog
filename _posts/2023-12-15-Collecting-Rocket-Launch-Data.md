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

  url = 'https://ll.thespacedevs.com/2.2.0/launch/previous/?status=3&year=2013,2014,2015,2016,2017,2018,2019,2020,2021,2022,2023&mode=normal&limit=100'  #Launch endpoint with dates

```
This specific endpoint /2.2.0/launch is their new launch library. Adding specific years must be done individually like shown in the example above. They also have a few different options for how the data can be returned, but I found that the normal and list modes gave me the information that I wanted. The normal data returns a lot of nested data which can be a pain to filter through but I will show you how I did it!

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
This takes about a minute or two to run, but since I chose to merge data from both the list and normal datasets I initially waited an hour before running my next get request.

# Data Ethics:

One of the things that I realized with this API is that it does not have an API key. This means that if I wanted to, I could use a vpn to make it so that the API can not throttle how many requests I make in an hour. I absolutely took this into consideration when messing up my data frame and wanting to go back and make another request to get more data. I think it would be unethical to go around the throttle, but also believe that API creators should make it so that you have to use an API key for better monitoring of usage.

# Data Cleaning:

After getting the data, I can see that my data frame has a lot of nested information that I need to extract. There are a couple different ways that I had to get the information out. Here are two examples:

```
# Extracting 'name' and 'type' from dictionaries in the column
df['lsp_name'] = df['launch_service_provider'].apply(lambda x: x['name'])
df['lsp_type'] = df['launch_service_provider'].apply(lambda x: x['type'])

df.drop('launch_service_provider', axis=1, inplace=True)
```
This way I was able to use the apply function to get the name of the launch service provider and the type of provider they are. In other columns I had to use the json_normalize function since the data in this column had multiple dictionaries. I am not sure which was is better or more used, but therse are what worked for me!

```
# Extract 'orbit' abbreviation using json_normalize
df['orbit'] = json_normalize(df['mission'], sep='_')['orbit_abbrev']

df.drop('mission', axis=1, inplace=True)

# List of columns you want to remove
columns_to_remove = ["id", "url", "slug", "status", "last_updated", "net", "net_precision", "window_end", "launcher", "infographic", "type"]

# Removing the specified columns
df = df.drop(columns=columns_to_remove)' 
```
There are 4 different columns that all have dates, I chose to use the date of when the rocket window started which I assume was when it was launched or put on the API. I prefer this one because most of the rocket information is updated pretty frequently. I renamed this column as my date. I also changed the format so that it would have year-month-day and the time. Here is what that looks like with my final data frame


```python
#Rename single date column
df = df.rename(columns={'window_start': 'date'})

# Convert string to datetime format
df['date'] = pd.to_datetime(df['date'])

# Convert the datetime to the desired format: 'YYYY-MM-DD HH:MM:SS'
df['date'] = df['date'].dt.strftime('%Y-%m-%d %H:%M:%S')

```

# Data Merging:

I ran the same exact get request with the while loop from up above but changed my url so instead of mode=normal I had mode=list. Once I got that data I merged two columns which tell us information about landings and the type of mission.

```
rocket_df = df.merge(list_df[['landing', 'mission_type']], left_index=True, right_index=True)
```
Here is what the final data frame looks like:

![Photo](https://github.com/sfolkman4/Rocket-Launches/blob/main/images/New%20Rocket%20Data%20Frame.png?raw=true)


# Conclusion: 

Now that I have my cleaned data I am ready to explore all the successful rocket launches of the past decade! There are many exciting discoveries to make with this data and I hope I am able to learn more about what is happening around the world with rocket technology. In the future I would love to go back and be able to look at more data like failed launches or launches from before the 2000's! 

---
If you want to see my EDA of this data, visit my next blog post [Successful Rocket Launches Data Exploration](https://sfolkman4.github.io/my-blog/blog/Successful-Rocket-Launches-EDA)

For my full code and to view the EDA visit my repository: [Rocket Launches](https://github.com/sfolkman4/Rocket-Launches/tree/main)

For an interactive app of this data visit my [Streamlit Dashboard](https://sfolkman4-rocket-launches-streamlitapp-kx3tqt.streamlit.app/)


