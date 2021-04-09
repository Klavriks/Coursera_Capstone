# Coursera - Capstone Project

## The Battle of Neighborhoods


### Part A 

  ### 1. Introduction 

New York City is the nation's most attractive destination for visitors. In 2018, New York City welcomed 65.1 million visitors, an all-time high. International visitors comprised 13.6 million of the total, but their economic importance to the City outweighs their numbers because they stay longer and spend more money than domestic visitors. New York City attracts almost one-third of all foreign visitors to the United States. 

My client is international travel company who seeks to investigate what type of restaurants are prevailing in one or the other neighborhood of New York. Coulinary tourism is a growing travel trend. Not just exploration of new cuisines and local food delicates what attracts tourists, but very often variety of local restaurant options play a pivotal role in their holiday eperience. Also, type of local restaurants quite often provides a tip on wellbeing and security of the area, points to ethnic diversity of the place and even gives a prompt to local overnight stay costs. This is especialy applicable to large megalopolises such as New York. Retrieved infromation on what type of restaurants are dominant in one or the other area will help travel agents to provide a weighted and well-informed opinion for their clients about their best place to stay. 
  
   ### 2. Data Section
   
  This project depends on the following data:
  
  **1. New York neighborhoods**
  
  - *Data source:* [Wikipedia page](https://en.wikipedia.org/wiki/Neighborhoods_in_New_York_City)
  - *Description:* Data is scraped from Wikipedia page which presents all community areas in New York and lists all neighborhoods they cover. 

  **2. Location of neighborhoods**
  
  - *Data sorce:* GeoPy geocoding web services
  - *Description:* Using GeoPy services long/lat data extracted for each neighborhood.
  
  **3. Restaurant data**
  
  - *Data source:* Foursquare API
  - *Description:* Once neighborhood locations are defined, Foursquaere API is used to identify nearby restaurants and retrieve required information about the venue. 
  
  ### Part B
  
  ### 3. Methodology
  
  #### Data preparation
  
  Initially neighborhood data is scraped from "New York Community Areas" table available in Wikipedia. 
  
  ![image](https://user-images.githubusercontent.com/46403847/114102314-febdec80-98be-11eb-817d-3e8110eecd31.png)
  
  Since more than one neighborhood stated against most community areas, those were split and concatenated with community areas in order to create an approprite address line. Relevant address data is paramaunt for obtaining lat/long information from GeoPy locatior. Once geolocation data obtained, the data is cleansed over duplicates and erroneous locations indentified. 
  
  ![image](https://user-images.githubusercontent.com/46403847/114102481-4e041d00-98bf-11eb-9800-e33edba7cc8f.png)

  Using Folium library we map our neighborhoods to ensure the locations are defined correctly. 
  
  ![image](https://user-images.githubusercontent.com/46403847/114159550-d0232e80-991d-11eb-9410-22a17a838439.png)
  

  #### Restaurant information (Foursquare API)
  
  Using Foursquare API we first select random neighborhood and check if we can spot required restaurant information. Random neighborhood selected is "Melrose"
  
  ![image](https://user-images.githubusercontent.com/46403847/114103301-d0d9a780-98c0-11eb-864b-375c96b64762.png)

  There were few restaurants identified in top 10 nearby venues found in this neighborhood. "Categories" column contains precise information which we need for identifying type of restaurant. Foursquare API can be used now for all neighborhoods and there will be top 100 nearby venues searched (this is max limit defined by Foursquare API for search request). Retrieved venues are filtered on "Venue Category" filed that contains a word "Restaurant". 
  
  ![image](https://user-images.githubusercontent.com/46403847/114104397-b0125180-98c2-11eb-82bb-69c1493d8df8.png)
  
  Further, to support quality of further analysis, lines with not defined restaurant type are cleared away and only neighborhoods with at least 5 restaurant types are selected.  
  #### Machine learnings (K-means Clustering)
  
  To analyse dominant restaurants in each area, one hot encoding will be applied for getting dummies of the restaurant types. 

  ![image](https://user-images.githubusercontent.com/46403847/114105513-ce794c80-98c4-11eb-877f-fa5d53faeb1c.png)

  In that way, by grouping data and calculating mean for each restaurant type we obtain desired ranking.
  
  ![image](https://user-images.githubusercontent.com/46403847/114105729-362f9780-98c5-11eb-9a9b-9542d5be8a1c.png)

  ![image](https://user-images.githubusercontent.com/46403847/114105793-5c553780-98c5-11eb-9b02-2fb58d14f023.png)
  
  Now, when data is ready for clustering we will test the data using k-means estimator to find best value for k parameter.
  
  ![image](https://user-images.githubusercontent.com/46403847/114165178-14b1c880-9924-11eb-8335-9333970d709b.png)

  From squared errors plotted above it's barely identifiable that elbow of the curve points at 5 value of k parameter. We will use this value in our clustering process. 
  The resulted clusters are mapped below. 

  ![image](https://user-images.githubusercontent.com/46403847/114165471-6e19f780-9924-11eb-9e38-71997b6f203d.png)

  #### Exploratory Data Analysis
  
  **Cluster 0 - (red pointers on the map)**
  
  ![image](https://user-images.githubusercontent.com/46403847/114232031-2bcbd700-9973-11eb-8a0d-967e9b31dab5.png)
  ![image](https://user-images.githubusercontent.com/46403847/114232065-371f0280-9973-11eb-94e4-bf037765b259.png)
  
  We can clearly see Mexican being dominant type of restaurant. Latin American restaurants can be also regarder as dominant as they earned 2nd place in both charts.
  Neighborhoods are identified in all areas of New York apart from Staten Island and avoiding congested NY city locations. 


  **Cluster 1 - (purple pointers on the map)**  
  
  ![image](https://user-images.githubusercontent.com/46403847/114232266-8402d900-9973-11eb-8fd9-3b6f085b69ad.png)
  ![image](https://user-images.githubusercontent.com/46403847/114232279-89f8ba00-9973-11eb-9285-6179cf153558.png)

  Italian cuisine is most popular in cluster 1 with American restaurants earning confident 2nd place.
  Neighborhoods are identified in all areas of New York without an exception with majority concentrated in Downtown and north-west part of Brooklyn. 
  
  
  **Cluster 2 - (light blue pointers on the map)** 
  
  ![image](https://user-images.githubusercontent.com/46403847/114232481-db08ae00-9973-11eb-9fc9-d7b3a2dd121a.png)
  ![image](https://user-images.githubusercontent.com/46403847/114232504-e2c85280-9973-11eb-9fee-5a38999927bd.png)

  This cluster covers a small number of neighborhoods which are mainly popular with their fast food chain restaurants.
  Location of neighborhoods: Staten Island & north part of Bronx.
  
  
  **Cluster 3 - (light green pointers on the map)**   
  
  ![image](https://user-images.githubusercontent.com/46403847/114232598-02f81180-9974-11eb-882f-e820f714ecaa.png)
  ![image](https://user-images.githubusercontent.com/46403847/114232624-08edf280-9974-11eb-8044-5675fd5ae020.png)

  Though clear dominance belongs to Mexican as the most common restaurant (remember, Mexican selected as top type for cluster 0 also), it's important to notice that neighborhood is rich with its Far East restaurants (Japanese, Chinese, Thai, Korean). Especially popular are Japanese restaurants, cause Sushi places belong to Japanese cuisine
  Location of neighborhoods: Manhattan, Brooklyn and Queens
  
  
  **Cluster 4 - (orange pointers on the map)**   
  
  ![image](https://user-images.githubusercontent.com/46403847/114232684-2a4ede80-9974-11eb-9738-bec0f5793aed.png)
  ![image](https://user-images.githubusercontent.com/46403847/114232693-3044bf80-9974-11eb-844a-634ae7360237.png)

  In cluster 4 Chinese (and Cantonese, which is basically the same cuisine) is clearly top type of restaurant in the neighborhoods.
  Neighborhoods are identified in all areas of New York apart from Staten Island. There are no locations identified in Manhattan apart from two neighborhoods in obvious Chinatown area.
  
  
  ### 4. Results

  - It goes without saying that New York has no shortage of restaurants in its area. 
  
  - 111 neighborhoods have at least five type of restaurants in radius of 500m from its center point. 
  
  - Majority of neighborhoods are dominated by Mexican, Far East, Italian and American restaurants. These 4 also dominate most parts of Manhattan, especially central and city locations. 
  
  - Quite a few neighborhoods have clear dominance of Chinese and Cantonese restaurants.
   
  - Only a few neighborhoods located in Staten Island and north Bronx have fast food chains indicated as the most popular restaurants. 
  

  ### 5. Discussion
  
  #### Observations
  
  Though it's widely known that New York is crammed with numerous fast food stores, only a few neighborhoods identified to have fast food restaurants being dominant in the area. That is likely to be explained by the fact that not all fast food places qualify for a "restaurant" title, and hence could be marked under different venue category in Foursquare API. 
  
  A small number of restaurant type instances selected for each neighborhood (5) together with a great variety of different types of restaurants in theory cause clustering process to be fairly inefficient. Though obtained clusters quite clearly defined dominant places in most neighborhoods that are close to be true. 
  
  It was a surprise not to see any neighborhood where Indian restaurants would dominate the area. It could be either because those are spread evenly across New York neighborhoods and are not concentrated in any specific area, or due to data incompleteness retrieved from Foursquare API. 
  
  #### Recommendations
  
  This project can certainly benefit from split-rank logic which is not applied for cases when number of most common restaurant places is identified the same. For example, if a neighborhood area has 2 of each of 5 restaurant types, any restaurant can be selected as a dominant.
  
  New York neighborhoods can be very different in size and venue search radius could have been modified for each area according to its size to cover more restaurants
  
  Quite a few neighborhoods were identified to have only a few number of restaurants and were removed from the analysis. Since data relies on limited 100 venues that can be extracted from Foursquare API in a single search request, more restaurant locations could have been captured if a workaround solution found to bypass this limit. 
 
 
  ### 6. Conclusion

  To conclude, this project has served well for outlining restaurant type dominance in various neighborhoods of New York and will be a great help to travel agents for forming informed advise on restaurant locations to their clients. The analysis provides a good glimpse into what cuisines can be tried out in one or the other neighborhood of New York, however there are certain aspects that can be improved in order to improve quality of outputs presented. As a final note, the analysis carried out in this project is dependent on the accuracy of Four Square data. A more comprehensive analysis would benefit from incorporating data from other external databases.
 
