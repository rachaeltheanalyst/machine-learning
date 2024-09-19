<p align="center">
<img src="images/LASSO.png" height="400"/>
</p>

# LASSO ML Model

> Employing LASSO regression, this machine learning model predicts house prices by focusing on key predictor variables, with RMSLE measuring performance.

<a name="toc"/></a>
## Table of Contents

1. [Overview](#overview)

2. [Technologies](#technologies)

3. [Fit Model / Predict](#fit)

4. [GridSearchCV](#gridsearchcv)

5. [Source](#source)

<a name="overview"/></a>
## 1. Overview
[Back to ToC](#toc)

LASSO (Least Absolute Shrinkage and Selection Operator) is a type of regression analysis method in machine learning that performs both variable selection and regularization to enhance the prediction accuracy and interpretability of the model. It is particularly useful when you have a large number of features and suspect that many of them might not be relevant to the model. By shrinking some coefficients to zero, LASSO automatically selects a subset of features, making the model simpler and more interpretable. A larger alpha is equivalent to the simplest model (mean model) and a small alpha is equivalent to the most complex model (multiple regression).

<a name="technologies"/></a>
## 2. Technologies
[Back to ToC](#toc)

Jupyter Notebook<br />
Python 3.11.4 (main, Jul 5 2023, 09:00:44) [Clang 14.0.6]

<a name="fit"/></a>
## 3. Fit Model / Predict
[Back to ToC](#toc)

Below is an example of fitting the LASSO model and predicting the model on test data using a small value of alpha, but this was also done for a large value of alpha as well. This required rescaling and recentering training and test data using the same means and standard deviations. I also transformed the outcome variable to (log + 1) space to avoid negative fitted values. 

```bash
# Instantiate rescaler
scaler = StandardScaler()

# Rescale and recenter training data using the above means and sd's. 
X_train_scaled = scaler.fit_transform(X_train)

# Rescale and recenter test data using the **same** means and sd's of training data.
X_test_scaled = scaler.transform(X_test)

# avoid negative fitted values
training['log_SalePrice'] = np.log1p(training['SalePrice'])

# fit model
model_LASSO_simple = Lasso(alpha = 0)
model_LASSO_simple.fit(X_train_scaled, training['log_SalePrice'])

# predict model
training['log_SalePrice_complex_hat'] = model_LASSO_simple.predict(X_train_scaled)
```

The results showed that the RMSLE for both training and test data were lower for alpha = 0 compared to alpha = 1000000. The model for alpha = 0 is more complex and also addresses potentially overfit models, while the model for alpha = 1000000 is the simplest model and addresses underfit models.<br />

The same process of fitting and predicting was done on a smaller set of data with only 50 observations. When the number of observations (n) is small relative to the number of predictors (p) in the LASSO model, it is more vulnerable to overfitting. In the data, the estimate of the Kaggle score is significantly smaller than the true Kaggle score because of this issue.

<a name="gridsearchcv"/></a>
## 4. GridSearchCV
[Back to ToC](#toc)

I used the GridSearchCV function to find the complexity parameter λ that yields the best out-of-sample predictive error. The resulting alpha was 0.0070548, which returned a Kaggle score of 0.16140.

<a name="source"/></a>
## 5. Source
[Back to ToC](#toc)

The data and RMSLE was retrieved from Kaggle's [House Prices: Advanced Regression Techniques](https://www.kaggle.com/c/house-prices-advanced-regression-techniques/) competition.
