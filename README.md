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




## Data Cleaning and Exploratory Data Analysis
<iframe
  src="/RecipeAnalysis/assets/ratingdist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

| name                                 |     id |   minutes |    hours |   rating |   n_steps |   n_ingredients | submitted   |   average_rating | description                                                                                                                                                                                                                                                                                                                                                                       |
|:-------------------------------------|-------:|----------:|---------:|---------:|----------:|----------------:|:------------|-----------------:|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1 brownies in the world    best ever | 333281 |        40 | 0.666667 |        4 |        10 |               9 | 2008-10-27  |                4 | these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you'll ever make.....sereiously! there's no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they're pure heaven!                                                                                                              |
| 1 in canada chocolate chip cookies   | 453467 |        45 | 0.75     |        5 |        12 |              11 | 2011-04-11  |                5 | this is the recipe that we use at my school cafeteria for chocolate chip cookies. they must be the best chocolate chip cookies i have ever had! if you don't have margarine or don't like it, then just use butter (softened) instead.                                                                                                                                            |
| 412 broccoli casserole               | 306168 |        40 | 0.666667 |        5 |         6 |               9 | 2008-05-30  |                5 | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 |
| 412 broccoli casserole               | 306168 |        40 | 0.666667 |        5 |         6 |               9 | 2008-05-30  |                5 | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 |
| 412 broccoli casserole               | 306168 |        40 | 0.666667 |        5 |         6 |               9 | 2008-05-30  |                5 | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 |






