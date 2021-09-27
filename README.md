# Housing-Price-Estimator-
Modeling to predict housing prices based on a number of parameters

Introduction

	The purpose of this project is to build a model that best predicts the sales prices of houses in Ames, Iowa. 79 variables are given that explain the details of each house. Two datasets were also provided, a train set and a test set. The train dataset contains a sales price variable that provides the price of the house, while the test dataset does not contain this variable. The train dataset will be used to create a model that best predicts the sales price of homes in Ames. This model will then be run on the test dataset, that does not contain a sales price variable, to predict the price of those houses. These predictions will be sent in to Kaggle to receive a score based on the accuracy of the predictions. 
  
Data Modeling and Cleaning

	To begin the process of cleaning the data, we combined the train dataset and the test dataset into one dataset. Combining the datasets allowed us to clean both datasets simultaneously. We then looked at the number of NA values for each variable. PoolQC, MiscFeatures, Alley, Fence, and Utilities all had large numbers of NA’s, so we decided to remove these variables from the dataset. To  populate the remaining NA values we used various techniques. For many of the NA values it made sense to use 0 for numeric variables and “none” for character variables. For some of the categorical variables where “none” did not make sense, we used the most common value for the NA values. The other technique used for populating missing values was single imputation. Single imputation uses variable means, medians, or regression predictions to assign a value to the missing value. For example, for the variable MSZoning we used a tree model to predict what the NA values should be. The tree model uses other variables to best predict what the missing value for MSZoning should be. For the variable Lot Frontage, we used the median values for the neighborhood the house was in to populate the null value. 
After taking care of all of the null values, we looked to see if any of the data looked off by running a summary statistics on the data. One of the numbers that looked off was the max value for the variable GarageYrBuilt. The max value was 2207, which is not possible because that year is way in the future. To handle this, we set the value for GarageYrBuilt to the year that house was built. 
We noticed that some variables were similar to each other and could be combined into one variable. One of the variables we created was Total Sqft. We combined the three variables that made up the square footage of the internal house:  Total Basement SF, 1st Floor SF, and 2nd Floor SF. The total sqft variable had a correlation of .76 to Sales Price, which was greater than any of the original variables on their own. We used a similar technique to combine the four bathroom variables. An equation of: [Basement Full Bath + Full Bath + (0.5) * Basement Half Bath + (0.5) * Half Bath] was used to calculate the total number of bathrooms and was stored in a new variable. Again, this total bathroom variable showed greater correlation to the Sales Price then any of the variables on their own. 
	The last thing we did to clean up the data was remove outliers that we believed would have a great effect on the model. The variable we created, Total Sqft, appeared to have two major outliers. In the figure below, you can see the two outliers in the plot circled in red. Figure 2 shows the same relationship between Sales Price and Total Sqft, but with the two outliers removed. You can see how much the regression line changes when removing the outliers. 
 
 

Model and Model Development

This model improves on our previous model mostly in the data that is being fed to it. We have refined, removed, and filtered a lot of the data in order to give us the variables that are the most tied to SalePrice. Often this included combining several data sets into one. As the old adage goes; garbage in, garbage out. 
One of the significant changes is the removal of data sets with high collinearity. By removing one of those variables and keeping the one with a higher correlation with SalePrice it greatly increased the validity of our model. 
There is a slight concern of overfitting inside of this model as we tended to get very high scores when cross-validating on the test data. By manipulating the input into our model so much we may also have tipped the scales towards variance in the bias-variance tradeoff. 
We went with the linear regression model from the caret package. The caret package is designed to do cross-validation every time it fits a model. We are using the linear regression because our outcome is a continuous numeric variable. 
The overall quality seems to be one of the most important variables in predicting price. When we run our model the sales price is driven up by an average of 13K for every score increase on the overall quality. Age is another driver we included with a steady decrease in price as the age of the house goes up (see the chart below). 
 
One of the more interesting variables that is included in our model is the neighborhood variable. You can see the model we ran in Figure 4 below. In this model we binned the neighborhoods so that we could quickly see which neighborhoods contained the higher priced houses and which the lower priced ones. We also included a mean saleprice line for reference. Through this graph you can quickly and easily see the neighborhoods that have high mean sale prices and how many homes from the dataset are in that neighborhood. 
 
Additionally, we also focused on the GarageCars, KitchenQual, BsmtQual, TotRmsAbvGrd, and TotalBaths. These variables appeared to be strong drivers and after handling co-linearity we determined they were fair to include in the model. 
We tried out a few other models to try to optimize our data. We couldn’t get RandomForest to work on our data, and in the end XGBoost looks to have helped the model the most. The package provides a cross validation function, but only determines the optimal number of rounds, and does not support a full grid search of hyperparameters.
Model Performance
●	Train
○	RMSE = 0.0667
○	R^2 = 0.9352
●	Test - X-Validation
○	RMSE = 0.1213
○	R^2 = 0.8745
●	Kaggle Score
○	0.13327
