<p align="center">
<img src="images/multiple-regression.png" height="400"/>
</p>

# Multiple Regression ML Model

> A machine learning model leveraging multiple regression techniques to forecast house prices based on a range of predictor variables, with RMSLE used for accuracy evaluation.

<a name="toc"/></a>
## Table of Contents

1. [Overview](#overview)

2. [Technologies](#technologies)

3. [Exploratory Data Analysis](#analysis)

4. [Fit Model / Predict](#fit)

5. [RMSLE](#rmsle)

6. [Source](#source)

<a name="overview"/></a>
## 1. Overview
[Back to ToC](#toc)

This program selects various predictor variables to train a model on house prices, predicts house prices on test data where the actual values are unknown, and calculates RMSLE on both the training and test data to evaluate the model's fit.

<a name="technologies"/></a>
## 2. Technologies
[Back to ToC](#toc)

RStudio Version 2024.04.1+748 (2024.04.1+748)

<a name="analysis"/></a>
## 3. Exploratory Data Analysis
[Back to ToC](#toc)

For this model, 6 predictor variables are chosen to fit the model, 3 of which are categorical variables. The final 6 variables chosen include:

1. totalSF (total square feet)
2. MSZoning
3. Utilities
4. Neighborhood
5. YearBuilt
6. outdoorEntArea

Categorical variables were selected based on the significant differences in sales price distributions among categories, while numerical variables were chosen for their strong correlation with sales price. 

I also considered several other variables, but they were excluded due to insufficient data, weaker correlations with sales price compared to the selected variables, or similar sales price distributions across categories for categorical variables.

<a name="fit"/></a>
## 4. Fit Model / Predict
[Back to ToC](#toc)

I fitted the model on the six predictor variables selected through exploratory data analysis using the following function on the training data.

```bash
SalePrice_model <- lm(SalePrice ~ totalSF + MSZoning + Utilities + Neighborhood + YearBuilt + outdoorEntArea, data = training)
```

After fitting the model, I predicted the sale prices for test data that only included the predictor variables.

```bash
test$SalePrice <- predict(SalePrice_model, newdata = test)
```

<a name="rmsle"/></a>
## 5. RMSLE
[Back to ToC](#toc)

Kaggle uses the true sale price values and the sale price predictions we submitted to calculate the RMSLE below. Below is the formula to calculate RMSLE for the training data, which was also used to calculate RMSLE for the test data. 

```bash
rmsle <- sqrt((sum((log(training$y_hat + 1) - log(training$SalePrice + 1))^2))/(nrow(training)))
```

![Kaggle Test RMSLE](images/score_screenshot.png)

<a name="source"/></a>
## 6. Source
[Back to ToC](#toc)

The data and test RMSLE was retrieved from Kaggle's [House Prices: Advanced Regression Techniques](https://www.kaggle.com/c/house-prices-advanced-regression-techniques/) competition.
