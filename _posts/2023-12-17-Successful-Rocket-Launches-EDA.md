---
title: Successful Rocket Launches Data Exploration
layout: post
post-image: "https://ts2.space/wp-content/uploads/2023/10/How-many-rockets-have-crashed-into-the-Moon_652a7d6cc48c3.jpg"
description: In this post I go into detail about my analysis of successful rocket launch data
tags:
- EDA
- MatplotLib
- Rockets
---
Have you ever wondered who is launching rockets and where? SpaceX aren't the only ones sending billions of dollars into space!
---
---

I collected my data for this analysis using an API made by TheSpaceDevs. If you want to see how I got the data here is my previous blog post on [Building a decades worth of rocket success]('https://sfolkman4.github.io/my-blog/blog/Collecting-Rocket-Launch-Data')

To start my EDA, I came up with the following questions:

1. How have rocket launches increased/decreased in the last 10 years?
2. Who is sending up the most rockets and what for?
3. Out of all the rockets going to space, how many land?
4. What are these rockets going up into space for?
5. Where are rockets being launched across the globe?

# Comaparison of Rockets Launched per year

![Test](https://github.com/sfolkman4/Rocket-Launches/blob/main/images/top_10_lsp.png?raw=true)

 Here we can see our top 10 launch service providers of the last decade! SpaceX and China are far in front from the others, but there are some unexpected ones on there for me. I learned that Mitsubishi is more than just an automobile company! It is also crazy to me that SpaceX has passed many nation state. Let's see their launch history compared to China.

# Comparison of China vs SpaceX Launches

![](https://github.com/sfolkman4/Rocket-Launches/blob/main/images/China_vs_SpaceX.png?raw=true)

Even though China has had more launches in the past decade, SpaceX is coming for their #1 spot! This year alone, SpaceX has had over 90 successful launches more than doubling the amount China had this year.

# Launch Types

![](https://github.com/sfolkman4/Rocket-Launches/blob/main/images/mission_types.png?raw=true)

By looking at the type of launch we can see just what these companies and nation states are building rockets for. It is apparent that sattelites for communication have become extremely important, especially as one of SpaceX's products. 

# Launch Orbits

![](https://github.com/sfolkman4/Rocket-Launches/blob/main/images/orbits.png?raw=true)

Most of these communication satellites are going into what is called a Low Earth orbit. This has become very popular since it is within 1200 miles of the earth and warrants a strong connectivity and ease of control. Since these sattelites can not be seen from the earth at all angles, constellations of sattelites are made to improve connectivity. 

# Landings

![](https://github.com/sfolkman4/Rocket-Launches/blob/main/images/landings.png?raw=true)

Since these rockets are so expensive, certain engineers had the idea of being able to reuse rockets instead of spending all of the money to only send up one object. SpaceX has greatly improved in their ability to land rockets and save a substantial amount of money by doing so.

# Top Launch Locations

![](https://github.com/sfolkman4/Rocket-Launches/blob/main/images/top_10_locations.png?raw=true)

Here we can see that Cape Canaveral in Florida is by far the most used location for launches in the world! Let's look at where these other locations are around the globe!

# Launch Location Map

![](https://github.com/sfolkman4/Rocket-Launches/blob/main/images/pad_map.png?raw=true)

It is amazing to see that rockets are being launched all over the world! Both China and the U.S. who lead the world in rocket launches have many different pad locations. The most unexpected location for me was in French Guiana!

# Conclusion

This EDA taught me about the impact that SpaceX has had in the Rocket space. They truly are changing the game when it comes to launches and I won't be surprised if we continue to see them grow year to year!

---
For my full code and to view the EDA visit my repository: [Rocket Launches](https://github.com/sfolkman4/Rocket-Launches/tree/main)

For an interactive app of this data visit my [Streamlit Dashboard]()

If you want to see how I got this data, visit my previous blog post on [TheSpaceDevs API](https://sfolkman4.github.io/my-blog/blog/Collecting-Rocket-Launch-Data)