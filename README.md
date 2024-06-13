# Investigation on the Relationship Between cooking time and Rating of Recipes

Authors: Xinyi Zhang & Grace Pei

## Overview

This is a data science project for DSC80 at UCSD focusing on exploring the relationship between the cooking time of the given recipe and rating of a recipe.

## Introduction

Food enthusiasts, home cooks, and even professional chefs often seek recipes that balance preparation time with the quality of the final dish. For busy individuals, understanding whether quicker recipes tend to have higher or lower ratings can influence their choices. This dataset provides a comprehensive view of various recipes and their associated user ratings, offering valuable insights into user preferences and the perceived quality of recipes based on their cooking time. Our project focuses on analyzing a dataset from Food.com, which includes recipes and user interactions such as ratings and reviews. The primary goal of this project is to explore the relationship between the cooking time of a given recipe and its rating.

The dataset, `cleaned_df`, contains 234429 rows and 17 colunns, indicating 83628 unique recipes, 587 unique cooking time. The full columns is shown below:

| Column             | Description                                                                                                                                                                                       |
| :----------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `'name'`           | Recipe name                                                                                                                                                                                       |
| `'id'`             | Recipe ID                                                                                                                                                                                         |
| `'minutes'`        | Minutes to prepare recipe                                                                                                                                                                         |
| `'contributor_id'` | User ID who submitted this recipe                                                                                                                                                                 |
| `'submitted'`      | Date recipe was submitted                                                                                                                                                                         |
| `'tags'`           | Food.com tags for recipe                                                                                                                                                                          |
| `'nutrition'`      | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `'n_steps'`        | Number of steps in recipe                                                                                                                                                                         |
| `'steps'`          | Text for recipe steps, in order                                                                                                                                                                   |
| `'description'`    | User-provided description                                                                                                                                                                         |
| `'ingredients'`    | Text for recipe ingredients                                                                                                                                                                       |
| `'n_ingredients'`  | Number of ingredients in recipe                                                                                                                                                                   |

The second dataset, `interactions`, contains 731927 rows and each row contains a review from the user on a specific recipe. The columns it includes are:

| Column        | Description         |
| :------------ | :------------------ |
| `'user_id'`   | User ID             |
| `'recipe_id'` | Recipe ID           |
| `'date'`      | Date of interaction |
| `'rating'`    | Rating given        |
| `'review'`    | Review text         |


## Data Cleaning and Exploratory Data Analysis

1. First, we show the head of two dataframe: df_interactions and df_recipes.

2. Left merge the recipes and interactions datasets on id and recipe_id.

	- This step helps match the unique recipes with their rating and review.

3. Fill all ratings of 0 with np.nan.

	- Since the rating is rate from 1 to 5, so if the rating shows 0, which means it does not have any meaning, and keep 0 will influence our data, so we should replace all the 0 to np.nan. 

4. Create a new column and rename the rating column to avg_rating for clarity

	- Because we are investigating the rating of recipes, we want to calculate the mean ratings. So we Calculate the average rating for each recipe by grouping the processed_recipe dataframe by id and computing the mean of the rating column. 

5. Separate the nutrition column into individual nutrient columns and Give meaningful names to the new columns.

	- By splitting this into separate columns, we make each nutrient easily accessible and usable for analysis.

6. convert all columns in `'nutrition_df'` to the float data type.

	- Converting the nutrient columns to float ensures that we can perform quantitative analysis on these values, such as calculating averages, correlations, or distributions.

7. Drop Unnecessary Columns

	- since we already seperate the nutrition columns into more detailed columns, we don't need to `'nutrition'` columns anymore.

8. Remove outliers in the cooking time
   
   	-  When we attempted to plot the graph, we figured that there are outliers that influence the whole dataset, so we create quartile to establish thresholds beyond which data points are considered outliers. We identified the outliers using the IQR method. 

#### Result
Here are all the columns of the cleaned df.

| Column                  | Description    |
| :---------------------- | :------------- |
| `'name'`                | object         |
| `'id'`                  | int64          |
| `'minutes'`             | int64          |
| `'contributor_id'`      | int64          |
| `'submitted'`           | datetime64[ns] |
| `'tags'`                | object         |
| `'n_steps'`             | int64          |
| `'steps'`               | object         |
| `'description'`         | object         |
| `'ingredients'`         | object         |
| `'n_ingredients'`       | int64          |
| `'user_id'`             | float64        |
| `'recipe_id'`           | float64        |
| `'date'`                | datetime64[ns] |
| `'rating'`              | float64        |
| `'review'`              | object         |
| `'average rating'`      | object         |
| `'calories (#)'`        | float64        |
| `'total fat (PDV)'`     | float64        |
| `sugar (PDV)'`          | float64        |
| `'sodium (PDV)'`        | float64        |
| `'protein (PDV)'`       | float64        |
| `'saturated fat (PDV)'` | float64        |
| `'carbohydrates (PDV)'` | float64        |

Our cleaned dataframe ended up with 234429 rows and 24 columns. Here are the first 5 rows of ~unique recipes of our cleaned dataframe for illustration. Since there is a lot of columns for the merged dataframe, we selected the columns that are most relevant to our questions for display. Scroll right to view more columns.

| name                                 |     id |   minutes | submitted           |   rating |   average rating |   calories (#) |   sugar (PDV) |
|:-------------------------------------|-------:|----------:|:--------------------|---------:|-----------------:|---------------:|--------------:|
| 1 brownies in the world    best ever | 333281 |        40 | 2008-10-27 00:00:00 |        4 |                4 |          138.4 |            50 |
| 1 in canada chocolate chip cookies   | 453467 |        45 | 2011-04-11 00:00:00 |        5 |                5 |          595.1 |           211 |
| 412 broccoli casserole               | 306168 |        40 | 2008-05-30 00:00:00 |        5 |                5 |          194.8 |             6 |
| millionaire pound cake               | 286009 |       120 | 2008-02-12 00:00:00 |        5 |                5 |          878.3 |           326 |
| 2000 meatloaf                        | 475785 |        90 | 2012-03-06 00:00:00 |        5 |                5 |          267   |            12 |

### Univariate Analysis

For this analysis, we examine the distribution of cooking time.

<iframe
  src="assets/fig1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/fig2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analysis

<iframe
  src="assets/fig3.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/fig4.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Interesting Aggregates

For this section, we investigated the relationship between the cooking time in minutes and ratings of the recipes. First, we created a small dataframe, `'pivot_table'` to store the cooking time in minutes without outliers. 

| cooking_time_range | avg_rating |
| -----------------: | ---------: |
|       0-20         |   4.705120 |
|       20-40        |   4.674040 |
|       40-60        |   4.663482 |
|       60-80        |   4.679378 |
|       80-100       |   4.667133 |
|     100-120        |   4.677711 |

## Assessment of Missingness

If users chose not to rate a recipe, possibly due to dissatisfaction or indifference, and this resulted in ratings being recorded as np.nan, the data could be considered Not Missing At Random (NMAR).

Conversely, the "review" column could provide additional context. If users encountered issues with the recipe and failed to complete it, they might not provide a rating but could leave a comment explaining their difficulties. In such cases, the missing ratings could be considered Missing At Random (MAR).

### Missingness Dependency

Distribution of Minutes by Missingness of Rating

**Null Hypothesis:**  The missingness of ratings does not depend on the minutes in the recipe.

**Alternative Hypothesis:** The missingness of ratings does depend on the minutes in the recipe.

**Test Statistic:** difference in means of minutes between the two groups (rating missing and not missing)

**Significance Level:** 0.05

<iframe
  src="assets/fig5.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We ran a permutation test by shuffling the missingness of rating for 1000 times to collect 1000 simulating mean differences in the two distributions. 

<iframe
  src="assets/fig6.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Distribution of Calories by Missingness of Rating

**Null Hypothesis:**  The missingness of ratings does not depend on the calories in the recipe.

**Alternative Hypothesis:** The missingness of ratings does depend on the calories in the recipe.

**Test Statistic:** difference in means of calories between the two groups (rating missing and not missing)

**Significance Level:** 0.05

<iframe
  src="assets/fig7.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

Result: 
{'minutes': {'Observed Statistic': 51.45237039852127, 'P-Value': 0.118},
 'calories (#)': {'Observed Statistic': 69.00722806375853, 'P-Value': 0.0}}

The permutation test results suggest that the missingness of the "rating" column may not depend significantly on the "minutes" column (P-value > 0.05). However, the missingness appears to depend on the "total fat (PDV)" column (P-value = 0.0).

## Hypothesis Testing

**Null Hypothesis:**  more cooking time of the recipe in minutes tend to have higher rating.

**Alternative Hypothesis:** more cooking time of the recipe in minutes tend to have same ratings.

**Test Statistic:** The difference in mean between high rating of recipes and same rating of recipes.

**Significance Level:** 0.05

To run the test, we first split the data points into two groups: recipes with shorter cooking times (less than or equal to the median cooking time) and recipes with longer cooking times (greater than the median cooking time).

Observed Statistic: The difference in mean ratings between the two groups is 0.0266.
Next, we shuffled the ratings 1000 times to collect 1000 simulated mean differences between the two distributions as described in the test statistic.

#### Conclusion of Permutation Test

P-value: We obtained a p-value of 0.0, indicating that there is a significant difference in how users rate recipes with different cooking times.

## Framing a Prediction Problem

For this project, we aim to predict the average rating of recipes using their characteristics. We have chosen the sugar (PDV) column as a key feature for our prediction model for the following reasons:

The sugar (PDV) column is numerical, making it suitable for regression models.

Sugar content significantly affects both the taste and healthiness of recipes, which likely impacts user ratings.

The sugar (PDV) column is consistently available in the dataset, ensuring a robust training set.

## Baseline Model

For this project, we aim to predict the average rating of recipes using their characteristics. We have chosen the sugar (PDV) column as a key feature for our prediction model for the following reasons:

Numerical Suitability: The sugar (PDV) column is numerical, making it suitable for regression models.
Impact on Ratings: Sugar content significantly affects both the taste and healthiness of recipes, which likely impacts user ratings.
Data Availability: The sugar (PDV) column is consistently available in the dataset, ensuring a robust training set.

## Final Model

Features:
'n_ingredients':

Reason for Inclusion: The number of ingredients in a recipe can impact its complexity and the perceived effort required to prepare it. Recipes with more ingredients might be seen as more complex and could influence user ratings.
'calories (#)':

Reason for Inclusion: The total calorie content of a recipe is a significant factor for many users, especially those who are health-conscious. Recipes with lower calories might be rated higher due to health benefits. We standardized this feature using StandardScaler to handle outliers and ensure comparability.
'total fat (PDV)':

Reason for Inclusion: Similar to calories, the fat content of a recipe is crucial for health-conscious users. Standardizing this feature ensures that it is on a comparable scale with other numerical features.
'protein (PDV)':

Reason for Inclusion: Protein content is an important nutritional aspect, especially for those focusing on muscle building or weight management. Standardizing this feature helps in making it comparable with other nutritional values.
'minutes':

Reason for Inclusion: The cooking time of the recipe in minutes is an essential feature as it can indicate the complexity and effort required for a recipe. Recipes that take longer to cook might receive lower ratings due to the time investment required. We used StandardScaler to standardize this feature to ensure comparability across different recipes.
Modeling Algorithm and Hyperparameters:
Algorithm: Linear Regression
Hyperparameters Tuned: Linear Regression has limited hyperparameters, so our focus was on feature engineering and preprocessing.
Method for Hyperparameter Tuning: Not applicable due to the nature of Linear Regression.
Improvement over Baseline Model:
The final model's performance shows a slight improvement over the baseline model in terms of R² and RMSE:

Baseline Model Performance:

RMSE: 107.18
R²: 0.73
Final Model Performance:

RMSE (Test): 110.32
R² (Test): 0.70

## Fairness Analysis

We categorized the dataset into two groups based on the value of n_steps:

Less Steps Group: This group includes observations where the number of steps (n_steps) is less than or equal to the median number of steps.
More Steps Group: This group consists of observations where the number of steps is greater than the median.

**Null Hypothesis**: there is no difference in the predictability (as measured by R²) of the outcome between the two groups. This would suggest that the number of steps does not influence the fairness of the predictions.

**Alternative Hypothesis**: there is a significant difference in R² between the two groups, indicating that the number of steps affects the predictability of the outcome and, consequently, the fairness of the predictions.

**Test Statistic**: Difference in precision (low sugar - high sugar)

**Significance Level**: 0.05

1. Determine the median number of steps in the recipes. This value is used to split the dataset into two groups: recipes with steps less than or equal to the median and recipes with steps greater than the median.
2. Evaluate the model on the two groups and calculate the observed difference in RMSE between the two groups.
   - less and more represent the models trained on the subsets of the data.
   - obs_rmse_diff stores the absolute difference in RMSE between the two groups.
3. Randomly shuffle the n_steps column and calculate the difference in RMSE for each permutation.

The p-value of 0.04 indicates that there is a 4% chance that the observed difference in RMSE between the two groups (recipes with fewer steps vs. recipes with more steps) could have occurred by random chance. Since the p-value is less than the commonly used significance level of 0.05, we can conclude that the difference in RMSE is statistically significant.

This permutation test shows that the number of steps in a recipe has a significant effect on the RMSE of the prediction model. The observed RMSE difference between recipes with fewer steps and those with more steps is unlikely to have occurred by chance, as indicated by the low p-value of 0.04. This suggests that the complexity of a recipe, as measured by the number of steps, plays a significant role in the model's prediction accuracy.
