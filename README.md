# Cooking Up Insights - Analyzing Recipe Ratings and Preparation Times
 

Authors: Esther Chung & Mythri Kishore


## Introduction
In this day and age, food consumes us. We constantly see food in the world around us which drives a passion for food for many people, including us! This dataset is a subset of food recipes and ratings from the website [food.com](https://www.food.com/?ref=nav). The data has been collected since 2008, thus we are using a subset of it.

In our busy lives, we need to prioritize where we dedicate our time for passions and work. Our project is centered around answer the question: **How is rating and cooking time impacted by different aspects of recipes?** It is important to understand ways to manage your time in order to be a well-rounded person. This dataset is important because it can provide insight into these tendencies and help recipe makers and readers alike. Recipe makers can understand what aspects of a recipe can impact the success of their recipe. For readers, it can help them better allocate their time in their schedule.

The `recipe` dataset contains 83782 rows. There are 12 columns: `['name', 'id', 'minutes', 'contributor_id', 'submitted', 'tags', 'nutrition', 'n_steps', 'steps', 'description', 'ingredients', 'n_ingredients']`. The columns that are important to our question are: 

| Column     | Description    |
|-------------|-------------|
| `name`       | Recipe name      |
| `id`             | Recipe ID     |
| `minutes`   | Minutes to prepare recipe      |
| `contributor_id`       | User ID who submitted this recipe     |
| `submitted`       | Date recipe was submitted     |
| `n_steps`       | Number of steps in recipe     |
| `n_ingredients`       | Number of ingredients in recipe     |
| `description`       | User-provided description      |


The `ratings` dataset contains 731927 rows. There are 5 columns: `['user_id', 'recipe_id', 'date', 'rating', 'review']`. The columns relevant to our question are:

| Column     | Description    |
|-------------|-------------|
| `recipe_id`       | Recipe ID     |
| `rating`       | Rating given     |
| `date`       | Date of interaction     |



## Data Cleaning and Exploratory Data Analysis

### Data Cleaning
These were the steps we followed to clean our dataframe:
1. Left merge the `recipe` and `ratings`  on `id` and `recipe_id`.
   - We merged to match the recipes to their corresponding ratings. The left merge preserves all the recipes, even the ones with no rating. 
2. Replace all 0 ratings with np.nan
   - Ratings are generally (and specifically on [food.com](https://www.food.com/?ref=nav)) are on a scale of 1-5, thus it is impossible for a reviewer to give a recipe 0 stars. This was also important because we needed to calculate the `average_rating` for each recipe. If missing values were stored as 0s, this would introduce bias for the mean (pull it down).
3. Create `average_rating` column
   -  This transformation helps provide a more thorough understanding of the ratings for each recipe, especially since we know that one rating is probably not representative of a recipe’s performance.
4. Transform `minutes` column into `hours`
   - We noticed that some recipes had a large number for `minutes`. Having this stored as hours can be easier to interpret for humans viewing the dataframe.
5. Handle missing values
   - There were some columns with missing values. The`'name` column has 1 missing value but we can disregard it since it doesn't relate to our analysis. We can notice a similar thing for the `description` and `user_id` column. We will keep all the null values for `rating`, `review`, and `average_rating` because it could give us more information about the success of a recipe.
6. Removing outliers using IQR
   - There were several recipes in this dataset that had unusually high cooking times, most often due to a recipe that was a [joke](https://www.food.com/recipe/how-to-preserve-a-husband-447963). We removed outliers using the IQR method, but we chose to only exclude values above the upper bound. 


Our cleaned dataframe had 210138 rows and 19 columns. The first 5 rows with some important columns are displayed below: 

| name                                 |     id |   minutes |    hours |   rating |   n_steps |   n_ingredients | submitted   |   average_rating | description                                                                                                                                                                                                                                                                                                                                                                       |
|:-------------------------------------|-------:|----------:|---------:|---------:|----------:|----------------:|:------------|-----------------:|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1 brownies in the world    best ever | 333281 |        40 | 0.666667 |        4 |        10 |               9 | 2008-10-27  |                4 | these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you'll ever make.....sereiously! there's no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they're pure heaven!                                                                                                              |
| 1 in canada chocolate chip cookies   | 453467 |        45 | 0.75     |        5 |        12 |              11 | 2011-04-11  |                5 | this is the recipe that we use at my school cafeteria for chocolate chip cookies. they must be the best chocolate chip cookies i have ever had! if you don't have margarine or don't like it, then just use butter (softened) instead.                                                                                                                                            |
| 412 broccoli casserole               | 306168 |        40 | 0.666667 |        5 |         6 |               9 | 2008-05-30  |                5 | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 |
| 412 broccoli casserole               | 306168 |        40 | 0.666667 |        5 |         6 |               9 | 2008-05-30  |                5 | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 |
| 412 broccoli casserole               | 306168 |        40 | 0.666667 |        5 |         6 |               9 | 2008-05-30  |                5 | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 |`

### Univariate Analysis
We examined the distribution of `rating`. We noticed that it was skewed left, showing that more recipes tended to be rated higher. As `rating` increased, the number of recipes in that rating category increased as well. This could be due to the fact that people are afraid to degrade other people’s hard work. 

<iframe
  src="/RecipeAnalysis/assets/ratingdist.html"
  width="800"
  height="450"
  frameborder="0"
></iframe>

### Bivariate Analysis
We examined the `hours` column as it relates to the number of ingredients(`n_ingredients`). We can notice that there is a general positive trend. As the number of ingredients increases, the cooking time typically increases as well. This follows our intuition that more complicated recipes involve more ingredients and will take longer to complete. Later, we will further explore this relationship.

<iframe
  src="/RecipeAnalysis/assets/hrsingredients.html"
  width="600"
  height="450"
  frameborder="0"
></iframe>

### Pivot Table

This pivot table gives us information about the mean, count, minimum, and maximum for `rating` and `hours` for each rating interaction (indexed by `date`). We can gain insights regarding the time of year’s influence on rating and the length of cooking time required for a recipe. 

| Date       |   ('mean', 'hours') |   ('mean', 'rating') |   ('count', 'hours') |   ('count', 'rating') |   ('max', 'hours') |   ('max', 'rating') |   ('min', 'hours') |   ('min', 'rating') |
|------------|--------------------:|---------------------:|---------------------:|----------------------:|-------------------:|--------------------:|-------------------:|--------------------:|
| 2008-01-02 |            0.833333 |              4.5     |                    2 |                     2 |           1.41667  |                   5 |          0.25      |                   4 |
| 2008-01-03 |            0.491667 |              4.8     |                   10 |                    10 |           1.08333  |                   5 |          0.0833333 |                   4 |
| 2008-01-04 |            0.6625   |              4.5     |                    4 |                     4 |           1.41667  |                   5 |          0.0666667 |                   4 |
| 2008-01-05 |            0.211111 |              4.5     |                    3 |                     2 |           0.416667 |                   5 |          0.0833333 |                   4 |
| 2008-01-06 |            0.786364 |              4.54545 |                   11 |                    11 |           1.5      |                   5 |          0.0666667 |                   2 |
| ...        |                 ... |                 ...  |                  ... |                   ... |                ... |                ... |               ... |                ... |
| 2018-12-15 |            1.01667  |              4.5     |                    5 |                     4 |            1.66667 |                   5 |          0.416667  |                   3 |
| 2018-12-16 |            0.685    |              4       |                   10 |                     9 |            1.58333 |                   5 |          0.0833333 |                   1 |
| 2018-12-17 |            0.75     |              3.25    |                    6 |                     4 |            1.83333 |                   5 |          0.1       |                   1 |
| 2018-12-18 |            0.526389 |              4.41667 |                   12 |                    12 |            1.5     |                   5 |          0.0833333 |                   1 |
| 2018-12-19 |            0.708333 |              4.6     |                    6 |                     5 |            1.08333 |                   5 |          0.5       |                   4 |

## Assessment of Missingness

### NMAR Analysis
A column that we believe is NMAR is `rating`. To be NMAR means that we have to look at the values of the column themselves to understand the missingness. Rating might be NMAR because reviewers may have been more hesitant to leave ratings when they don’t feel any strong emotion regarding the recipe. In other words, reviewers may be more likely to leave a rating if they either hate or love the recipe. 

Some more additional data that could help explain the missingness of the `rating` column could be their comment frequency. This is because it could explain if a user is comfortable with leaving ratings on recipes. 

### Missingness Dependency 
**Null Hypothesis**: The missingness of ratings does not depend on the cooking time of the recipe in minutes.

**Alternate Hypothesis**: The missingness of ratings does depend on the cooking time of the recipe in minutes.

**Test Statistic**: The absolute difference of mean in cooking time of the recipe in minutes of the distribution of the group without missing ratings and the distribution of the group with missing ratings.

**Significance Level**: 0.01
The observed statistic turned out to be 51.452, as shown on the red line. Our p-value of 0.107 was above the significance level of 0.01, so we fail to reject the null hypothesis. This tells us that `ratings` is not MAR dependent on the `minutes` column. 
<iframe
  src="/RecipeAnalysis/assets/missingminutes.html"
  width="800"
  height="450"
  frameborder="0"
></iframe>

**Null Hypothesis**: The missingness of ratings does not depend on the number of steps of the recipe.

**Alternate Hypothesis**: The missingness of ratings does depend on the number of steps of the recipe.

**Test Statistic**: The absolute difference of mean in the number of steps of the recipe of the distribution of the group without missing ratings and the distribution of the group with missing ratings.
	
**Significance Level**: 0.01
The observed statistic turned out to be 1.339, as shown on the red line. Our p-value of 0.0 was below the significance level of 0.01, so we reject the null hypothesis. This tells us that `ratings` is MAR dependent on `n_steps`.
<iframe
  src="/RecipeAnalysis/assets/missingnsteps.html"
  width="800"
  height="450"
  frameborder="0"
></iframe>

## Hypothesis Testing

**Null Hypothesis**: There is no significant difference in the ratings between groups of recipes with short and long cooking times(minutes).

**Alternate Hypothesis**: There is a significant difference in the ratings between groups of recipes with short and long cooking times(minutes).

**Test Statistic**: The absolute difference of mean in the cooking time of the recipe (in minutes) of the distribution of the group without missing ratings and the distribution of the group with missing ratings.

**Significance Level**: 0.01

The relevant columns are 'rating' and 'minutes'. We chose a **permutation test** since we have two clear groups (short vs. long cooking times) that we can shuffle labels between. 

### Conclusion
Our observed statistic was 0.0265. Our p-value was 0.0. Thus, we reject the null hypothesis. This means that we can't say that there is no significant difference in the ratings between groups of recipes with short and long cooking times.

It is important to understand this relationship because it provides insights into people’s sentiment after investing time into short and long recipes. In other words, people who tried out a recipe with a longer cooking time could be more upset than people who tried out a recipe with a shorter cooking time, if the recipe didn’t turn out as good as expected. Using mean absolute difference is a good choice in this instance, because we are just seeing if there is a difference in the distributions and we do not care about directionality. We chose a significance level of 0.01 because this provides a strict framework to evaluate our hypotheses against, while also following typical statistical procedures.

## Framing a Prediction Problem
**We will be predicting the cooking time that a recipe takes(in minutes).** This is a regression problem since cooking time, the response variable, is quantitative. We chose `minutes` as the response variable because it is representative of a recipe. Earlier, we also saw that there was a positive relationship between cooking time and number of ingredients. Cooking time is impacted by several factors in a recipe, so it would be a good response variable to create a prediction problem with.

The metric we are using to evaluate our model with is **root mean squared error (RMSE)**. We chose this because we are performing a linear regression and RMSE is telling of how “accurate” our model is.

At the time of prediction we would not have information about a person’s review and rating. In other words, we only “know” information provided by the recipe and its author.

## Baseline Model
As stated above, our baseline model will be predicting cooking time (in minutes) based on the number of steps and number of ingredients using linear regression. Both of these features are quantitative. To encode both features, we transformed the values by standardizing them using `StandardScaler()`. 

The RMSE of our baseline model was 21.736 minutes. In order to analyze the performance of our model, we must consider the context of the data. There is a large range of values for cooking time, and since we removed outliers our RMSE explains that the model is able to predict cooking time well. However, we can definitely make improvements (reduce RMSE) by using more features. 

## Final Model
In addition to our baseline model which utilized standardized number of ingredients and number of steps to predict minutes, we added two new features.

One of the features we added was `is_december`. This extracted the month from the `submitted` column and one-hot encoded the values (stored a 1 or 0 to represent if the month the recipe was published was December or not). This feature is important for our prediction task of predicting the cooking time in minutes since seasonal trends may affect cooking behaviors. For example, there may be shorter recipes to account for holiday busyness or more time consuming recipes for holiday meals. It is important to account for this when predicting cooking time. 

Our second new feature was `len_description` which stored the length of the description of a recipe. This is a useful feature since the length of a recipe description can provide insights on the complexity of a recipe. For example, more descriptive recipes may involve additional steps or specialized techniques, leading to longer cooking times.

The modeling algorithm we chose was polynomial regression utilizing all of the aforementioned features. To select the best degree hyperparameter we used `GridSearchCV`. Our initial instinct led us to have `GridSearchCV` test degrees 1, 2, and 3. When this was performed it picked degree 3, so we were inclined to add more options greater than degree 3 while still being wary that a higher degree could overfit. So we updated the hyperparameter options to degrees 1, 2, 3, 4, and 5. The hyperparameter that ended up performing the best was degree 4. Its RMSE for cross validation was 21.279 minutes.

Our Final Model improved compared to our Baseline Model’s performance. For the Final Model, the test RMSE was 21.309 minutes. Comparing the RMSEs we see a decrease of 0.427 minutes. This shows that the Final Model did **improve** since the RMSE decreased. 

## Fairness Analysis
Our choice for Group X is recipes with short cooking times, and our choice for Group Y is recipes with long cooking times. We added a column called `time_group` to store `’short’` and `’long’` based on if the cooking time was less than or greater than the median cooking time (minutes). Our evaluation metric is RMSE since our model was a linear regression model.

**Null Hypothesis**: The model is fair. The RMSE of the model for short recipes and long recipes is the same.

**Alternative Hypothesis**: The model is unfair. The RMSE of the model for short recipes is significantly different from the RMSE for long recipes.

**Test Statistic**: Absolute difference in RMSEs between short and long recipes.

**Significance Level**: 0.01

Our observed statistic was 8.402. Our p-value was 0.0. Thus, we reject the null hypothesis that our model is fair. It is important to know how our model behaves differently for two groups.

<iframe
  src="/RecipeAnalysis/assets/fairness.html"
  width="800"
  height="450"
  frameborder="0"
></iframe>