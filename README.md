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


