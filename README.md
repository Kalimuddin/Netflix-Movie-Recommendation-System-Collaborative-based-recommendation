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


## Computed User-User Similarity matrices :


## Computed Movie-Movie Similarity matrices :

## Modelling + Featurization :



