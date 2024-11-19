# Analyzing Recipe Ratings and Preparation Times
 

Authors: Esther Chung & Mythri Kishore


## Introduction
In this day and age, food consumes us. We constantly see food in the world around us which drives a passion for food for many people, including us! This dataset is a subset of food recipes and ratings from the website [food.com](https://www.food.com/?ref=nav). The data has been collected since 2008, thus we are using a subset of it.

In our busy lives, we need to prioritize where we dedicate our time for passions and work. Our project is centered around answer the question: **How is cooking time impacted by different aspects of recipes?** It is important to understand ways to manage your time in order to be a well-rounded person. This dataset is important because it can provide insight into these tendencies and help recipe makers and readers alike. Recipe makers can understand what aspects of a recipe can impact the success of their recipe. For readers, it can help them better allocate their time in their schedule.

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
| 412 broccoli casserole               | 306168 |        40 | 0.666667 |        5 |         6 |               9 | 2008-05-30  |                5 | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 |

### Univariate Analysis
We examined the distribution of `rating`. We noticed that it was skewed left, showing that more recipes tended to be rated higher. As `rating` increased, the number of recipes in that rating category increased as well. This could be due to the fact that people are afraid to degrade other people’s hard work. 

<iframe
  src="/RecipeAnalysis/assets/ratingdist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


### Bivariate Analysis
We examined the `hours` column as it relates to the number of ingredients(`n_ingredients`). We can notice that there is a general positive trend. As the number of ingredients increases, the cooking time typically increases as well. This follows our intuition that more complicated recipes involve more ingredients and will take longer to complete. Later, we will further explore this relationship.

<iframe
  src="/RecipeAnalysis/assets/hrsingredients.html"
  width="600"
  height="600"
  frameborder="0"
></iframe>

### Pivot Table
