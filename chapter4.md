---
title: 'Chapter 4. Variable Selection'
description: 'This chapter describes tools and techniques to help you select variables to enter into a linear regression model, beginning with an iterative model selection process. In applications with many potential explanatory variables, automatic variable selection procedures are available that will help you quickly evaluate many models. Nonetheless, automatic procedures have serious limitations including the inability to account properly for nonlinearities such as the impact of unusual points; this chapter expands upon the Chapter 2 discussion of unusual points. It also describes collinearity, a common feature of regression data where explanatory variables are linearly related to one another. Other topics that impact variable selection, including out-of-sample validation, are also introduced.'
---

## An iterative approach to data analysis and modeling

```yaml
type: VideoExercise
key: 9da28ad7dd
xp: 50
```

`@projector_key`
9362f318b5a48b39912dd30a85aa2f41

---

## An iterative approach to data modeling

```yaml
type: MultipleChoiceExercise
key: 3c6425abfa
xp: 50
```

Which of the following is not true?

`@possible_answers`
- A. Diagnostic checking reveals symptoms of mistakes made in previous specifications.
- B. Diagnostic checking provides ways to correct mistakes made in previous specifications.
- C. Model formulation is accomplished by using prior knowledge of relationships.
- [D. Understanding theoretical model properties is not really helpful when matching a model to data or inferring general relationships based on the data.]

`@hint`


`@pre_exercise_code`
```{r}

```

`@sct`
```{r}

```

---

## Automatic variable selection procedures

```yaml
type: VideoExercise
key: 8c0358d3d9
xp: 50
```

`@projector_key`
fd8d5708669990bf0b1da87eaf4e45cc

---

## Data-snooping in stepwise regression

```yaml
type: NormalExercise
key: 3c43eee3af
xp: 100
```

Automatic variable selection procedures, such as the classic stepwise regression algorithm, are very good at detecting patterns. Sometimes they are too good in the sense that they detect patterns in the sample that are not evident in the population from which the data are drawn. The detect "spurious" patterns.

This excercise illustrates this phenomenom by using simulation techniques to draw a sample. The simulation is designed so that the outcome variable (*y*) and the explanatory variables are mutually independent. So, by design, there is no relationship between the outcome and the explanatory variables.

As part of the code set-up, we have *n* = 100 observations generated of the outcome *y* and 50 explanatory variables, `xvar1` through `xvar50`. A few steps show that collections of explanatory variables are not statistically significant. However, with the [stepwise()](https://www.rdocumentation.org/packages/Rcmdr/versions/2.0-4/topics/stepwise) command, you will find some statistically significant relationships. This is because the `stepwise` procedure is repeatedly using *t*-tests - hypothesis testing procedures that are design to falsely detect a relationship $\alpha$ fraction of the time (typically 5\%). For example, if you run a *t*-test 50 times (for each explanatory variable), you can expect to get two or three "statistically significant" explanatory variables even for unrelated variables (because $50 \times 0.05 = 2.5$).

`@instructions`
- Fit a basic linear regression model and MLR model with the first ten explanatory variables. Compare the models via an *F* test.
- Fit a multiple linear regression model with all fifty explanatory variables. Compare this model to the one with ten variables via an *F* test.
- Use the `stepwise` function to find the best model starting with the fitted model containing all fifty explanatory variables.
- Fit the model identified by the stepwise regression algorithm and summarize the fit.

`@hint`


`@pre_exercise_code`
```{r}
set.seed(1237)
X <- as.data.frame(matrix(rnorm(100*50, mean = 0, sd = 1), ncol = 50))
colnames(X) <- paste("xvar", 1:50, sep = "")
X$y <- with(X, matrix(rnorm(100*1, mean = 0, sd = 1), ncol = 1))
```

`@sample_code`
```{r}
modelStep1 <- lm(y ~ xvar1, data = X)
summary(modelStep1)

modelStep2 <- lm(y ~ xvar1 + xvar2 + xvar3, data = X)
#summary(modelStep2)
anova(modelStep1,modelStep2)

modelStep3 <- lm(y ~ xvar1 + xvar2 + xvar3 + xvar4 + xvar5 + xvar6 + xvar7 + xvar8 + xvar9 + xvar10, data = X)
#summary(modelStep3)
anova(modelStep2,modelStep3)

modelStep4 <- lm(y ~ xvar1 + xvar2 + xvar3 + xvar4 + xvar5 + xvar6 + xvar7 + xvar8 + xvar9 + xvar10 + xvar11 + xvar12 + xvar13 + xvar14 + xvar15 + xvar16 + xvar17 + xvar18 + xvar19 + xvar20 + xvar21 + xvar22 + xvar23 + xvar24 + xvar25 + xvar26 + xvar27 + xvar28 + xvar29 + xvar30 + xvar31 + xvar32 + xvar33 + xvar34 + xvar35 + xvar36 + xvar37 + xvar38 + xvar39 + xvar40 + xvar41 + xvar42 + xvar43 + xvar44 + xvar45 + xvar46 + xvar47 + xvar48 + xvar49 + xvar50, data = X)
#summary(modelStep4)
anova(modelStep4)
anova(modelStep3,modelStep4)

#library(Rcmdr)
#temp <- stepwise(modelStep4, direction = 'backward/forward')
#summary(temp)

#  The function "stepwise" is in the Rcmdr library. Alternatively, you can use "step" function as below:
#  k CONTROLS THE COMPLEXITY OF THE MODEL
#  step(modelStep4, k = log(length(y)))
#  You can check the model of those three explanatory variables to see whether all of them are significant
modelStep5 <- lm(y ~ xvar27 + xvar29 + xvar32, data = X)
summary(modelStep5)
```

`@solution`
```{r}
modelStep1 <- lm(y ~ xvar1, data = X)
summary(modelStep1)

modelStep2 <- lm(y ~ xvar1 + xvar2 + xvar3, data = X)
#summary(modelStep2)
anova(modelStep1,modelStep2)

modelStep3 <- lm(y ~ xvar1 + xvar2 + xvar3 + xvar4 + xvar5 + xvar6 + xvar7 + xvar8 + xvar9 + xvar10, data = X)
#summary(modelStep3)
anova(modelStep2,modelStep3)

modelStep4 <- lm(y ~ xvar1 + xvar2 + xvar3 + xvar4 + xvar5 + xvar6 + xvar7 + xvar8 + xvar9 + xvar10 + xvar11 + xvar12 + xvar13 + xvar14 + xvar15 + xvar16 + xvar17 + xvar18 + xvar19 + xvar20 + xvar21 + xvar22 + xvar23 + xvar24 + xvar25 + xvar26 + xvar27 + xvar28 + xvar29 + xvar30 + xvar31 + xvar32 + xvar33 + xvar34 + xvar35 + xvar36 + xvar37 + xvar38 + xvar39 + xvar40 + xvar41 + xvar42 + xvar43 + xvar44 + xvar45 + xvar46 + xvar47 + xvar48 + xvar49 + xvar50, data = X)
#summary(modelStep4)
anova(modelStep4)
anova(modelStep3,modelStep4)

#library(Rcmdr)
#temp <- stepwise(modelStep4, direction = 'backward/forward')
#summary(temp)

#  The function "stepwise" is in the Rcmdr library. Alternatively, you can use "step" function as below:
#  k CONTROLS THE COMPLEXITY OF THE MODEL
#  step(modelStep4, k = log(length(y)))
#  You can check the model of those three explanatory variables to see whether all of them are significant
modelStep5 <- lm(y ~ xvar27 + xvar29 + xvar32, data = X)
summary(modelStep5)
```

`@sct`
```{r}
success_msg("Excellent! The step procedure repeatedly fits many models to a data set. We summarize each fit with hypothesis testing statistics like t-statistics and p-values. But, remember that hypothesis tests are designed to falsely detect a relationship a fraction of the time (typically 5%). For example, if you run a t-test 50 times (for each explanatory variable), you can expect to get two or three statistically significant explanatory variables even for unrelated variables (because 50 times 0.05 = 2.5).")
```

---

## Residual analysis

```yaml
type: VideoExercise
key: 37c4ebf2d6
xp: 50
```

`@projector_key`
1a22b8308fcc14590ae2bdb95030c2a2

---

## Residual analysis and risk manager survey

```yaml
type: NormalExercise
key: d62cb0bd86
xp: 100
```

This exercise examines data, pre-loaded in the object `survey`, from a survey on the cost effectiveness of risk management practices. Risk management practices are activities undertaken by a firm to minimize the potential cost of future losses, such as the event of a fire in a warehouse or an accident that injures employees. This exercise develops a model that can be used to make statements about cost of managing risks.

A measure of risk management cost effectiveness, `logcost`, is the outcome variable. This variable is defined as total property and casualty premiums and uninsured losses as a proportion of total assets, in logarithmic units. It is a proxy for annual expenditures associated with insurable events, standardized by company size. Explanatory variables include `logsize`, the logarithm of total firm assets, and `indcost`, a measure of the firm's industry risk.

`@instructions`
- Fit and summarize a MLR model using `logcost` as the outcome variable and `logsize` and `indcost` as explanatory variables.
- Plot residuals of the fitted model versus `indcost` and superimpose a locally fitted line using the `R` function [lowess()](https://www.rdocumentation.org/packages/stats/versions/3.5.0/topics/lowess).
- Fit and summarize a MLR model of `logcost` on `logsize`, `indcost` and a squared version of `indcost`.
- Plot residuals of the fitted model versus `indcost' and superimpose a locally fitted line using [lowess()](https://www.rdocumentation.org/packages/stats/versions/3.5.0/topics/lowess).

`@hint`


`@pre_exercise_code`
```{r}
survey <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/dc1c5bce43ef076aa77169a242118e2e58d01f82/Risk_survey.csv", header=TRUE)
survey$logcost <- log(survey$firmcost)
```

`@sample_code`
```{r}
mlr.survey1 <- lm(logcost ~ logsize + indcost, data = survey)
summary(mlr.survey1)

plot(survey$indcost, mlr.survey1$residuals)
lines(lowess(survey$indcost,mlr.survey1$residuals))

mlr.survey2 <- lm(logcost ~ logsize + poly(indcost,2), data = survey)
summary(mlr.survey2)

plot(survey$indcost, mlr.survey2$residuals)
lines(lowess(survey$indcost,mlr.survey2$residuals))
```

`@solution`
```{r}
mlr.survey1 <- lm(logcost ~ logsize + indcost, data = survey)
summary(mlr.survey1)

plot(survey$indcost, mlr.survey1$residuals)
lines(lowess(survey$indcost,mlr.survey1$residuals))

mlr.survey2 <- lm(logcost ~ logsize + poly(indcost,2), data = survey)
summary(mlr.survey2)

plot(survey$indcost, mlr.survey2$residuals)
lines(lowess(survey$indcost,mlr.survey2$residuals))
```

`@sct`
```{r}
success_msg("Excellent! In this exercise, you examined residuals from a preliminary model fit and detected a mild quadratic pattern in a variable. This suggested entering the squared term of that variable into the model specification. The refit of this new model suggests that the squared term has important explanatory information. The squared term is a nonlinear alternative that is not available in many automatic variable selection procedures.")
```

---

## Added variable plot and refrigerator prices

```yaml
type: NormalExercise
key: ecd90f8bcd
xp: 100
```

What characteristics of a refrigerator are important in determining its price (`price`)? We consider here several characteristics of a refrigerator, including the size of the
refrigerator in cubic feet (`rsize`), the size of the freezer compartment in cubic feet (`fsize`), the average amount of money spent per year to operate the refrigerator (`ecost`, for energy cost), the number of shelves in the refrigerator and freezer doors (`shelves`), and the number of features (`features`). The features variable includes shelves for cans, see-through crispers, ice makers, egg racks and so on.

Both consumers and manufacturers are interested in models of refrigerator prices. Other things equal, consumers generally prefer larger refrigerators with lower energy costs that have more features. Due to forces of supply and demand, we would expect consumers to pay more for these refrigerators. A larger refrigerator with lower energy costs that has more features at the similar price is considered a bargain to the consumer. How much extra would the consumer be willing to pay for this additional space? A model of
prices for refrigerators on the market provides some insight to this question.

To this end, we analyze data from *n* = 37 refrigerators.

`@instructions`
Nothing yet

`@hint`


`@pre_exercise_code`
```{r}

```

`@sample_code`
```{r}

```

`@solution`
```{r}

```

`@sct`
```{r}

```

---

## Unusual observations

```yaml
type: VideoExercise
key: f26dac9523
xp: 50
```

`@projector_key`
3958fa9f4bc063310c17d368a7ad400d

---

## Outlier example

```yaml
type: NormalExercise
key: a61e86392e
xp: 100
```

In chapter 2, we consider a fictitious data set of 19 "base" points plus three different types of unusual points. In this exercise, we consider the effect of one unusal point, "C", this both an outlier (unusual in the "y" direction) and an influential point (usual in the x-space). The data have been pre-loaded in the dataframe `outlrC`.

`@instructions`
- Fit a basic linear regression model of `y` on `x` and store the result in an object.
- Use the function [rstandard()](https://www.rdocumentation.org/packages/stats/versions/3.5.0/topics/influence.measures) to extract the standardized residuals from the fitted regression model object and summarize them. 
- Use the function [hatvalues()](https://www.rdocumentation.org/packages/stats/versions/3.5.0/topics/influence.measures) to extract the leverages from the model fitted and summarize them. 
- Plot the standardized residuals versus the leverages to see the relationship between these two measures that calibrate how unusual an observation is.

`@hint`


`@pre_exercise_code`
```{r}
outlr <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/7a38912e544c31fc6f5fca12b9a2eb645f2bcd32/Outlier.csv", header = TRUE)
outlrC <-outlr[-c(20,21),c("x","y")]
```

`@sample_code`
```{r}
plot(outlrC)
model_outlrC <- lm(y ~ x, data = outlrC)
ri <- rstandard(model_outlrC)
summary(ri)
hii <- hatvalues(model_outlrC)
summary(hii)
plot(hii,ri)
```

`@solution`
```{r}
plot(outlrC)
model_outlrC <- lm(y ~ x, data = outlrC)
ri <- rstandard(model_outlrC)
summary(ri)
hii <- hatvalues(model_outlrC)
summary(hii)
plot(hii,ri)
```

`@sct`
```{r}
success_msg("Excellent! With only two variables, we could argue graphically that observations were unusual. In this exercise, we showed how certain statistics could be used to identify usual observations. Although not really necessary in basic linear regression, the main advantage of the statistics is that they work readily in a multivariate setting.")
```

---

## High leverage and risk manager survey

```yaml
type: NormalExercise
key: e072807cbe
xp: 100
```

In a prior exercise, we fit a regression model of `logcost` on `logsize`, `indcost` and a squared version of `indcost`. This model is summarized in the object `mlr_survey2`. In this exercise, we examine the robustness of the model to unusual observations.

`@instructions`
- Use the `R` functions [rstandard()](https://www.rdocumentation.org/packages/stats/versions/3.5.0/topics/influence.measures) and [hatvalues()](https://www.rdocumentation.org/packages/stats/versions/3.5.0/topics/influence.measures) to extract the standardized residuals and leverages from the model fitted. Summarize the distributions graphically.
- You will see that there are two observations where the leverages are high, numbers 10 and 16. On looking at the dataset, these turn out to be observations in a high risk industry. Create a histogram of the variable `indcost` to corroborate this.
- Re-run the regression omitting observations 10 and 16. Summarize this regression and the regression in the object  `mlr_survey2`, noting differences in the coefficients.

`@hint`


`@pre_exercise_code`
```{r}
survey <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/dc1c5bce43ef076aa77169a242118e2e58d01f82/Risk_survey.csv", header=TRUE)
survey$logcost <- log(survey$firmcost)
mlr.survey2 <- lm(logcost ~ logsize + poly(indcost,2), data = survey)
```

`@sample_code`
```{r}
#summary(mlr.survey2)
ri <- rstandard(mlr.survey2)
hii <- hatvalues(mlr.survey2)
mean(hii)

par(mfrow=c(1, 2))
hist(ri, nclass=16, main="", xlab="Standardized Residuals")
hist(hii, nclass=16, main="", xlab="Leverages")
par(mfrow=c(1, 1))
hist(survey$indcost, nclass=16)
mlr.survey3 <- lm(logcost ~ logsize + poly(indcost,2), data =  survey, subset =-c(10,16))
summary(mlr.survey3)
```

`@solution`
```{r}
#summary(mlr.survey2)
ri <- rstandard(mlr.survey2)
hii <- hatvalues(mlr.survey2)
mean(hii)

par(mfrow=c(1, 2))
hist(ri, nclass=16, main="", xlab="Standardized Residuals")
hist(hii, nclass=16, main="", xlab="Leverages")
par(mfrow=c(1, 1))
hist(survey$indcost, nclass=16)
mlr.survey3 <- lm(logcost ~ logsize + poly(indcost,2), data =  survey, subset =-c(10,16))
summary(mlr.survey3)
```

`@sct`
```{r}
success_msg("Excellent! You will have noted that after removing these two influential observations from a high risk industry, the variable associated with the `indcost` squared became less statistically significant. This illustrates a general phenomena; sometimes, the 'signicance' of a variable may actually due to a few unusual observations, not the entire variable.")
```

---

## Collinearity

```yaml
type: VideoExercise
key: 1e651c4018
xp: 50
```

`@projector_key`
3f319d9bce5db7f2d39df9e2e14f3c46

---

## Collinearity and term life

```yaml
type: NormalExercise
key: 4d112f601d
xp: 100
```

We have seen that adding an explanatory variable $x^2$ to a model is sometimes helpful even though it is perfectly related to $x$ (such as through the function $f(x)=x^2$). But, for some data sets, higher order polynomials and interactions can be approximately linearly related (depending on the range of the data). 

This exercise returns to our term life data set `Term1` (preloaded) and demonstrates that collinearity can be severe when introducing interaction terms.

`@instructions`
- Fit a MLR model of `logface` on explanatory variables `education`, `numhh` and `logincome`
- Use the function [vif()](https://www.rdocumentation.org/packages/car/versions/3.0-0/topics/vif) from the `car` package (preloaded) to calculation variance inflation factors.
- Fit a MLR model of `logface` on explanatory variables `education` , `numhh` and `logincome` with an interaction between `numhh` and `logincome` and extract variance inflation factors.

`@hint`


`@pre_exercise_code`
```{r}
Term <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/efc64bc2d78cf6b48ad2c3f5e31800cb773de261/term_life.csv", header = TRUE)
Term1 <- subset(Term, subset = face > 0)
```

`@sample_code`
```{r}
#library(car)

# Solution
Term_mlr <- lm(logface ~ education + numhh + logincome, data = Term1)
#vif(Term_mlr)
Term_mlr1 <- lm(logface ~ education + numhh*logincome , data = Term1)
summary(Term_mlr1)
#vif(Term_mlr1)
```

`@solution`
```{r}
#library(car)

# Solution
Term_mlr <- lm(logface ~ education + numhh + logincome, data = Term1)
#vif(Term_mlr)
Term_mlr1 <- lm(logface ~ education + numhh*logincome , data = Term1)
summary(Term_mlr1)
#vif(Term_mlr1)
```

`@sct`
```{r}
success_msg("Excellent! This exercise underscores that approximately colinearity among explanatory variables can be induced when introducing higher order terms such as interactions. Note that in the interation model the variable 'numhh' does not appear to be statistically effect. This is one of the big dangers of collinearity - it can mask important effects.")
```

---

## Selection criteria

```yaml
type: VideoExercise
key: fbe7bc54a3
xp: 50
```

`@projector_key`
5ee9a1ceca54fdf811fb1ed24ed36673

---

## Cross-validation and term life

```yaml
type: NormalExercise
key: 40e9eec496
xp: 100
```

Here is some sample code to give you a better feel for cross-validation.

The first part of the random re-orders ("shuffles") the data. It also identifies explanatory variables `explvars`.

In the function starts with pulling out only the needed data into `cvdata`. Then, for each subsample, a model is fit based on all the data except for the subsample, in `train_mlr` with the subsample in `test`. 

```
# Randomly re-order data - "shuffle it"
n <- nrow(Term1)
set.seed(12347)
shuffled_Term1 <- Term1[sample(n), ]
explvars <- c("education", "numhh", "logincome")

crossvalfct <- function(explvars){
  cvdata   <- shuffled_Term1[, c("logface", explvars)]
  crossval <- 0
  k <- 5
  for (i in 1:k) {
    indices <- (((i-1) * round((1/k)*nrow(cvdata))) + 1):((i*round((1/k) * nrow(cvdata))))
    # Exclude them from the train set
    train_mlr <- lm(logface ~ ., data = cvdata[-indices,])
    # Include them in the test set
    test  <- data.frame(cvdata[indices, explvars])
    names(test)  <- explvars
    predict_test <- exp(predict(train_mlr, test))
    # Compare predicted to held-out and summarize
    predict_err  <- exp(cvdata[indices, "logface"]) - predict_test
    crossval <- crossval + sum(abs(predict_err))
  }
  crossval/1000
}
```

`@instructions`
- Calculate the cross-validation statistic using only logarithmic income, `logincome`.
- Calculate the cross-validation statistic using `logincome`, `education` and `numhh`.
- Calculate the cross-validation statistic using `logincome`, `education`, `numhh` and `marstat`.

The best model has the lowest cross-validation statistic.

`@hint`


`@pre_exercise_code`
```{r}
#Term <- read.csv("CSVData\\term_life.csv", header = TRUE)
Term <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/efc64bc2d78cf6b48ad2c3f5e31800cb773de261/term_life.csv", header = TRUE)
Term1 <- subset(Term, subset = face > 0)
Term1$marstat <- as.factor(Term1$marstat)

crossvalfct <- function(explvars){
  cvdata   <- shuffled_Term1[, c("logface", explvars)]
  crossval <- 0
  k <- 5
  for (i in 1:k) {
    indices <- (((i-1) * round((1/k)*nrow(cvdata))) + 1):((i*round((1/k) * nrow(cvdata))))
    # Exclude them from the train set
    train_mlr <- lm(logface ~ ., data = cvdata[-indices,])
    # Include them in the test set
    test  <- data.frame(cvdata[indices, explvars])
    names(test)  <- explvars
    predict_test <- exp(predict(train_mlr, test))
    # Compare predicted to held-out and summarize
    predict_err  <- exp(cvdata[indices, "logface"]) - predict_test
    crossval <- crossval + sum(abs(predict_err))
  }
  crossval/1000000
}
```

`@sample_code`
```{r}
# Randomly re-order data - "shuffle it"
n <- nrow(Term1)
set.seed(12347)
shuffled_Term1 <- Term1[sample(n), ]
## Cross - Validation
explvars <- c("logincome")
crossvalfct(explvars)
explvars <- c("education", "numhh", "logincome")
crossvalfct(explvars)
explvars <- c("education", "numhh", "logincome", "marstat")
crossvalfct(explvars)
```

`@solution`
```{r}
# Randomly re-order data - "shuffle it"
n <- nrow(Term1)
set.seed(12347)
shuffled_Term1 <- Term1[sample(n), ]
## Cross - Validation
explvars <- c("logincome")
crossvalfct(explvars)
explvars <- c("education", "numhh", "logincome")
crossvalfct(explvars)
explvars <- c("education", "numhh", "logincome", "marstat")
crossvalfct(explvars)
```

`@sct`
```{r}

```
