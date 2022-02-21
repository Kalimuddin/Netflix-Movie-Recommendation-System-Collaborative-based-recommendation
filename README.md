# Netflix-Movie-Recommendation-System-Collaborative-based-recommendation :

## Problem Definition and Data Requirements :
- Problem Statement is to predict the rating that a user would give to a movie that he has not yet rated so as to recommend movies which have higher ratings.
- Primary goal is to minimize the difference between predicted and actual rating (Minimize RMSE).
- After doing EDA, Computed User-User and Movie-Movie Similarity matrices, Featurization and Experimenting so many model's we achieved RMSE = 1.0726 and MAPE = 34.4873 
- Required Data available at : https://drive.google.com/drive/folders/19rCdufTRLLaPZtYaUJxkoMdORbiGPSnG?usp=sharing
- Here, values that u will be predicting is rating between 1 and 5 so it can also be as regression problem.
- MovieIDs range from 1 to 17770 sequentially.
- CustomerIDs range from 1 to 2649429, with gaps. There are 480189 users.
- Ratings are on a five star (integral) scale from 1 to 5.
- Dates have the format YYYY-MM-DD.
- The first line of each file contains the movie id followed by a colon, Each subsequent line in the file : CustomerID,Rating,Date
- The given problem is a Recommendation problem (Similarity, Matrix Factorisation)
- Performance metric : RMSE, MAPE

## EDA :
- No of Nan values in our dataframe :  0
- There are 0 duplicate rating entries in the data.
- Total data 
  - Total no of ratings : 100480507
  - Total No of Users   : 480189
  - Total No of movies  : 17770

-  Distribution of ratings :

![1](https://user-images.githubusercontent.com/54996809/154888902-4c28a121-31bb-4aa0-8fe9-8a27b1f16516.png)

- Analysis on the Ratings given by user :

![2](https://user-images.githubusercontent.com/54996809/154889131-228babed-9869-49a9-b33a-1233c10ab7b3.png)

![3](https://user-images.githubusercontent.com/54996809/154889141-6adc4f9c-1bf5-4d1b-b8ff-39e044f11ab4.png)

- Number of ratings on each day of the week : this graph is not usefull.

![4](https://user-images.githubusercontent.com/54996809/154889241-43ca7dba-7d07-4e07-8321-e254ceb01463.png)

- Creating sparse matrix from data frame :
  - Sparsity Of Train matrix : 99.8292709259195 % 
  - Sparsity Of Test matrix : 99.95731772988694 % 

- Finding Global average of all movie ratings, Average rating per user, and Average rating per movie :
  - finding global average of all movie ratings : 
  - finding average rating per user :
  - finding average rating per movie :

- PDF's & CDF's of Avg.Ratings of Users & Movies (In Train Data) :

![5](https://user-images.githubusercontent.com/54996809/154889642-5c828b82-4afa-47d1-b98d-7d2bd8ce6407.png)

## Computed User-User Similarity matrices :
- It is not easy task, Computing User-User Similarity matrix :- It is not easy task unless you have huge Computing Power and lots of time, system could crash or the program stops with Memory Error

![6](https://user-images.githubusercontent.com/54996809/154890125-4d121fcb-dcd8-4f53-9ddb-3ac3b66d62bc.png)

- We have 405,041 users in out training set and computing similarities between them..( 17K dimensional vector..) is time consuming..
- From above plot, It took roughly 8.88 sec for computing simlilar users for one user
- 405041×8.88=3596764.08sec=59946.068min=999.101133333 hours=41.629213889 days... 
- Even if we run on 4 cores parallelly (a typical system now a days), It will still take almost 10 and 1/2 days.

- Is there any other way to compute user user similarity..??
    - An alternative is to compute similar users for a particular user, whenenver required (ie., Run time) 
    - We maintain a binary Vector for users, which tells us whether we already computed or not. 
    - If not : Compute top (let's just say, 1000) most similar users for this given user, and add this to our datastructure, so that we can just access it(similar users) without recomputing it again.
    - If It is already Computed: Just get it directly from our datastructure, which has that information.
    - In production time, We might have to recompute similarities, if it is computed a long time ago. Because user preferences changes over time. 
    - If we could maintain some kind of Timer, which when expires, we have to update it ( recompute it ).
    - Which datastructure to use: It is purely implementation dependant.
    - One simple method is to maintain a Dictionary Of Dictionaries.
    - key : userid - value: Again a dictionary - key : Similar User - value: Similarity Value
- there are many u_i, u_j combination that we don't need at all

## Computed Movie-Movie Similarity matrices :
- Even though we have similarity measure of each movie, with all other movies, We generally don't care much about least similar movies.
- Most of the times, only top_xxx similar items matters. It may be 10 or 100.
- We take only those top similar movie ratings and store them in a saperate dictionary.

- Does movie-movie similarity work :-
  - Let's pick some random movie and check for its similar movies
  - movie details are in 'netflix/movie_titles.csv'
  - Similar Movies for 'Vampire Journals', mv_id = 67
  - Top 10 similar movies :

  ![7](https://user-images.githubusercontent.com/54996809/154891076-8997f3c4-0982-4ceb-a932-a9acb459dd29.png)

  - Got good results, Here, we didn't use the name, we just use movie-movie similarity matrix by user who just rated these movies

## Modelling + Featurization :
- Featurizing data for regression problem :
  - GAvg : Average rating of all the ratings
  - Similar users rating of this movie:
    - sur1, sur2, sur3, sur4, sur5 ( top 5 simiular users who rated that movie.. )
  - Similar movies rated by this user:
    - smr1, smr2, smr3, smr4, smr5 ( top 5 simiular movies rated by this movie.. )
  - UAvg : User AVerage rating
  - MAvg : Average rating of this movie
  - rating : Rating of this movie by this user.
  - user | movie | GAvg | sur1 | sur2 | sur3 | sur4 | sur5 | smr1 | smr2 | smr3 | smr4 | smr5 | UAvg | MAvg | rating



