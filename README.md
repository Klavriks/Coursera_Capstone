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

  
  ### 4. Results


  ### 5. Discussion
  
  
  ### 6. Conclusion

  
 
  
