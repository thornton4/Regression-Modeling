---
title: 'Chapter 5. Interpreting Regression Results'
description: 'An application, determining an individual''s characteristics that influence its health expenditures, illustrates the regression modeling process from start to finish. Subsequently, the chapter summarizes what we learn from the modeling process, underscoring the importance of variable selection.'
---

## Case study - MEPS health expenditures

```yaml
type: VideoExercise
key: 04ae7f9572
xp: 50
```

`@projector_key`
9f660c963794c20e534c811dcee7577e

---

## Summarizing data

```yaml
type: NormalExercise
key: 8b8b4db5e8
xp: 100
```

With a complex dataset, you will probably want to take a look at the structure of the data. You are already familiar with taking a *summary* of a dataset which provides summary statistics for many variables. You will see that several variables in this dataset are categorical, or factor, variables. We can use the `R` [table()](https://www.rdocumentation.org/packages/base/versions/3.5.0/topics/table) function to summarize them.

After getting a sense of the distributions of explanatory variables, we want to take a deeper dive into the distribution of the outcome variable, `expendop`. We will do this by comparing the histograms of the variable to that of its logarithmic version.

To examine relationships of the outcome variable visually, we look to scatterplots for continuous variables (such as `age`) and boxplots (such as `phstat`).

`@instructions`
- Examine the structure of the `meps` dataset using the [str()](https://www.rdocumentation.org/packages/utils/versions/3.5.0/topics/str/) function.
- Examine the distribution of the `race` variable using the [table()](https://www.rdocumentation.org/packages/base/versions/3.5.0/topics/table) function.
- Compare the expenditures distribution to its logarithmic version visually via histograms plotted next to another. `par(mfrow = c(1, 2))` is used to organize the plots you create.
- Examine the distribution of logarithmic expenditures in terms of levels of `phstat` visually using the 
[boxplot()](https://www.rdocumentation.org/packages/graphics/versions/3.5.0/topics/boxplot/) function.
- Examine the relationship of age versus logarithmic expenditures using a scatter plot. Superimpose a local fitting line using the [lines()](https://www.rdocumentation.org/packages/graphics/versions/3.5.0/topics/lines) and
[lowess()](https://www.rdocumentation.org/packages/stats/versions/3.5.0/topics/lowess) functions.

`@hint`


`@pre_exercise_code`
```{r}
meps <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/7b7dab6d0c528e4cd2f8d0e0fc7824a254429bf8/HealthMeps.csv", header = TRUE)
```

`@sample_code`
```{r}
str(meps)
summary(meps)
table(meps$race)
par(mfrow = c(1, 2))
hist(meps$expendop, main = "", xlab = "outpatient expenditures")
hist(log(meps$expendop), main = "", xlab = "log expenditures")
par(mfrow = c(1, 1))
meps$logexpend <- log(meps$expendop)
boxplot(logexpend ~ phstat, data = meps, main = "boxplot of log expend")
plot(meps$age,meps$logexpend, xlab = "age", ylab = "log expend")
lines(lowess(meps$age, meps$logexpend), col="red")
```

`@solution`
```{r}
str(meps)
summary(meps)
table(meps$race)
par(mfrow = c(1, 2))
hist(meps$expendop, main = "", xlab = "outpatient expenditures")
hist(log(meps$expendop), main = "", xlab = "log expenditures")
par(mfrow = c(1, 1))
meps$logexpend <- log(meps$expendop)
boxplot(logexpend ~ phstat, data = meps, main = "boxplot of log expend")
plot(meps$age,meps$logexpend, xlab = "age", ylab = "log expend")
lines(lowess(meps$age, meps$logexpend), col="red")
```

`@sct`
```{r}
success_msg("Excellent! Summarizing data, without reference to a model, is probably the most time-consuming part of any predictive modeling exercise. Summary statistics are also a key part of any report as the illustrate features of the data that are accessible to a broad audience.")
```

---

## Fit a benchmark multiple linear regression model

```yaml
type: NormalExercise
key: d8f858af77
xp: 100
```

As part of the pre-processing for the model fitting, we will split the data into training and test subsamples. For this exercise, we use a 75/25 split although other choices are certainly suitable. Some analysts prefer to do this splitting before looking at the data. Another approach, adopted here, is that the final report typically contains summary statistcs of the entire data set and so it makes sense to do so when examining summary statistics.

We start by fitting a benchmark model. It is common to use all available explanatory variables with the outcome on the original scale and so we use this as our benchmark model. This exercise shows that when you [plot()](https://www.rdocumentation.org/packages/graphics/versions/3.5.0/topics/plot) a fitted linear regression model in `R`, the result provides four graphs that you have seen before. These can be useful for identifying an appropriate model.

`@instructions`
- Randomly split the data into a training and a testing data sets. Use 75\% for the training, 25\% for the testing.
- Fit a full model using `expendop` as the outcome and all explanatory variables. Summarize the results of this model fitting.
- You can [plot()](https://www.rdocumentation.org/packages/graphics/versions/3.5.0/topics/plot) the fitted model to view several diagnostic plots. These plots provide evidence that expenditures may not be the best scale for linear regression.
- Fit a full model using `logexpend` as the outcome and all explanatory variables. Use the [plot()]() function for evidence that this variable is more suited for linear regression methods than expenditures on the original scale.

`@hint`


`@pre_exercise_code`
```{r}
meps <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/7b7dab6d0c528e4cd2f8d0e0fc7824a254429bf8/HealthMeps.csv", header = TRUE)
meps$logexpend <- log(meps$expendop)
```

`@sample_code`
```{r}
# Split the sample into a `training` and `test` data
n <- nrow(meps)
set.seed(12347)
shuffled_meps <- meps[sample(n), ]
train_indices <- 1:round(0.75 * n)
train_meps    <- shuffled_meps[train_indices, ]
test_indices  <- (round(0.25 * n) + 1):n
test_meps     <- shuffled_meps[test_indices, ]

meps_mlr1 <- lm(expendop ~ gender + age + race + region + educ + phstat + mpoor + anylimit + income + insure + usc + unemploy + managedcare, data = train_meps)
summary(meps_mlr1)
par(mfrow = c(2, 2))
plot(meps_mlr1)

meps_mlr2 <- lm(logexpend ~ gender + age + race + region + educ + phstat + mpoor + anylimit + income + insure + usc + unemploy + managedcare, data = train_meps)
plot(meps_mlr2)
summary(meps_mlr2)
par(mfrow = c(1, 1))
```

`@solution`
```{r}
# Split the sample into a `training` and `test` data
n <- nrow(meps)
set.seed(12347)
shuffled_meps <- meps[sample(n), ]
train_indices <- 1:round(0.75 * n)
train_meps    <- shuffled_meps[train_indices, ]
test_indices  <- (round(0.25 * n) + 1):n
test_meps     <- shuffled_meps[test_indices, ]

meps_mlr1 <- lm(expendop ~ gender + age + race + region + educ + phstat + mpoor + anylimit + income + insure + usc + unemploy + managedcare, data = train_meps)
summary(meps_mlr1)
par(mfrow = c(2, 2))
plot(meps_mlr1)

meps_mlr2 <- lm(logexpend ~ gender + age + race + region + educ + phstat + mpoor + anylimit + income + insure + usc + unemploy + managedcare, data = train_meps)
plot(meps_mlr2)
summary(meps_mlr2)
par(mfrow = c(1, 1))
```

`@sct`
```{r}
success_msg("Excellent! You may have compared the four diagnostic graphs from the MLR model fit of 'expend' to those created using the same procedure but with logarithmic expenditures as the outcome. This provides another piece of evidence that log expenditures are more suitable for regression modeling. Using logarithmic outcomes is a common feature of actuarial applications but can be difficult to diagnose and interpret without practice.")
```

---

## Variable selection

```yaml
type: NormalExercise
key: a41d08acf3
xp: 100
```

Modeling building can be approached using a "ground-up" strategy, where the analyst introduces a variable, examines residuls from a regression fit, and then seeks to understand the relationship between these residuals and other available variables so that these variables might be added to the model.

Another approach is a "top-down" strategy where all available variables are entered into a model and unnecessary variables are pruned from the model. Both approaches are helpful when using data to specify models. This exercise illustrates the latter approach, using the stepwise function in `R` to help narrow our search for the best fitting model.

`@instructions`
From our prior work, the training dataset `train_meps` has already been loaded in. A multiple linear regression model fit object `meps_mlr2` is available that summarizes a fit of `logexpend` as the outcome variable using all 13 explanatory variables.

- Use the [Rcmdr::stepwise()](https://www.rdocumentation.org/packages/RcmdrMisc/versions/1.0-10/topics/stepwise) function to drop unnecessary variables from the full fitted model summarized in the object `meps_mlr2`.
- Refit the recommended model.
- As an alternative, use the explanatory variables in the recommended model and add the varibles `phstat`. Summarize the fit using the [anova()](https://www.rdocumentation.org/packages/stats/versions/3.5.0/topics/anova) function and note that statistical significance of the new variable.
- You have been reminded by your boss that use of the variable `gender` is unsuitable for actuarial pricing purposes. As an another alternative, drop `gender` from the recommended model (still keeping `phstat`). Note the statistical significance of the variable `usc`with this fitted model.

`@hint`


`@pre_exercise_code`
```{r}
meps <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/7b7dab6d0c528e4cd2f8d0e0fc7824a254429bf8/HealthMeps.csv", header = TRUE)
meps$logexpend <- log(meps$expendop)
n <- nrow(meps)
set.seed(12347)
shuffled_meps <- meps[sample(n), ]
train_indices <- 1:round(0.75 * n)
train_meps    <- shuffled_meps[train_indices, ]
test_indices  <- (round(0.25 * n) + 1):n
test_meps     <- shuffled_meps[test_indices, ]
```

`@sample_code`
```{r}
meps_mlr2 <- lm(logexpend ~ gender + age + race + region + educ + phstat + mpoor + anylimit + income + insure + usc + unemploy + managedcare, data = train_meps)
#library(Rcmdr)
#temp <- stepwise(meps_mlr2, direction = 'backward/forward')
#summary(temp)

meps_mlr3 <- lm(logexpend ~ gender + age + mpoor + anylimit + insure + usc , data = train_meps)
summary(meps_mlr3)

par(mfrow = c(2, 2))
plot(meps_mlr3)

meps_mlr4 <- lm(logexpend ~ gender + age + mpoor + anylimit + insure + usc  + phstat, data = train_meps)
summary(meps_mlr4)
anova(meps_mlr4)

meps_mlr5 <- lm(logexpend ~ age  + anylimit + mpoor + insure  + usc  + phstat, data = train_meps)
summary(meps_mlr5)
anova(meps_mlr4, meps_mlr5)
```

`@solution`
```{r}
meps_mlr2 <- lm(logexpend ~ gender + age + race + region + educ + phstat + mpoor + anylimit + income + insure + usc + unemploy + managedcare, data = train_meps)
#library(Rcmdr)
#temp <- stepwise(meps_mlr2, direction = 'backward/forward')
#summary(temp)

meps_mlr3 <- lm(logexpend ~ gender + age + mpoor + anylimit + insure + usc , data = train_meps)
summary(meps_mlr3)

par(mfrow = c(2, 2))
plot(meps_mlr3)

meps_mlr4 <- lm(logexpend ~ gender + age + mpoor + anylimit + insure + usc  + phstat, data = train_meps)
summary(meps_mlr4)
anova(meps_mlr4)

meps_mlr5 <- lm(logexpend ~ age  + anylimit + mpoor + insure  + usc  + phstat, data = train_meps)
summary(meps_mlr5)
anova(meps_mlr4, meps_mlr5)
```

`@sct`
```{r}
success_msg("Excellent! Sometimes variables may have good predictive power but are unacceptable for public policy purposes - in insurance, ethnicity and sometimes sex are good examples. This implies that model interpretation can be just as important as the ability to predict.")
```

---

## Model comparisons using cross-validation

```yaml
type: NormalExercise
key: 587248a912
xp: 100
```

To compare alternative models, you decide to experiment with cross-validation. For this exercise, you split the training sample into six subsamples of approximately equal size.

In the sample code, the cross-validation procedure has been summarized into a function that you can call. The input to the function is a list of variables that you select as your model explanatory variables. With this function, you can readily test several candidate models.

```
crossvalfct <- function(explvars){
  cvdata   <- train_meps[, c("logexpend", explvars)]
  crossval <- 0
  for (i in 1:6) {
    indices <- (((i-1) * round((1/6)*nrow(cvdata))) + 1):((i*round((1/6) * nrow(cvdata))))
    # Exclude them from the train set
    train_mlr <- lm(logexpend ~ ., data = cvdata[-indices,])
    # Include them in the test set
    test  <- data.frame(cvdata[indices, explvars])
    names(test)  <- explvars
    predict_test <- exp(predict(train_mlr, test))
    # Compare predicted to held-out and summarize
    predict_err  <- exp(cvdata[indices, "logexpend"]) - predict_test
    crossval <- crossval + sum(abs(predict_err))
  }
```

`@instructions`
- Run the cross validation (`crossvalfct`) function using the explanatory variables suggested by the stepwise function.
- Run the function again but adding the `phstat` variable
- Run the function again but omitting the `gender` variable
- Note which model is suggested by the cross validation function.

`@hint`


`@pre_exercise_code`
```{r}
meps <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/7b7dab6d0c528e4cd2f8d0e0fc7824a254429bf8/HealthMeps.csv", header = TRUE)
meps$logexpend <- log(meps$expendop)
# Split the sample into a `training` and `test` data
n <- nrow(meps)
set.seed(12347)
shuffled_meps <- meps[sample(n), ]
train_indices <- 1:round(0.75 * n)
train_meps    <- shuffled_meps[train_indices, ]
test_indices  <- (round(0.25 * n) + 1):n
test_meps     <- shuffled_meps[test_indices, ]

crossvalfct <- function(explvars){
  cvdata   <- train_meps[, c("logexpend", explvars)]
  crossval <- 0
  for (i in 1:6) {
    indices <- (((i-1) * round((1/6)*nrow(cvdata))) + 1):((i*round((1/6) * nrow(cvdata))))
    # Exclude them from the train set
    train_mlr <- lm(logexpend ~ ., data = cvdata[-indices,])
    # Include them in the test set
    test  <- data.frame(cvdata[indices, explvars])
    names(test)  <- explvars
    predict_test <- exp(predict(train_mlr, test))
    # Compare predicted to held-out and summarize
    predict_err  <- exp(cvdata[indices, "logexpend"]) - predict_test
    crossval <- crossval + sum(abs(predict_err))
  }
  crossval/1000000
}
```

`@sample_code`
```{r}
explvars <- c("gender", "age", "race", "mpoor", "anylimit", "income", "insure", "usc")
crossvalfct(explvars)
explvars <- c("gender", "age", "race", "mpoor", "anylimit", "income", "insure", "usc", "phstat")
crossvalfct(explvars)
explvars <- c("gender", "age", "mpoor", "anylimit", "income", "insure", "usc", "phstat")
crossvalfct(explvars)
```

`@solution`
```{r}
explvars <- c("gender", "age", "race", "mpoor", "anylimit", "income", "insure", "usc")
crossvalfct(explvars)
explvars <- c("gender", "age", "race", "mpoor", "anylimit", "income", "insure", "usc", "phstat")
crossvalfct(explvars)
explvars <- c("gender", "age", "mpoor", "anylimit", "income", "insure", "usc", "phstat")
crossvalfct(explvars)
```

`@sct`
```{r}
success_msg("Excellent! ")
```

---

## Out of sample validation

```yaml
type: NormalExercise
key: f86e97aa0f
xp: 100
```

From our prior work, the training `train_meps` and test `test_meps` datasets have already been loaded in. We think our best model is based on logarithmic expenditures as the outcome and the following explanatory variables:

```
explvars3 <- c("gender", "age", "race", "mpoor", "anylimit", "income", "insure", "usc")
```
We will compare this to a benchmark model that is based on expenditures as the outcome and all 13 explanatory variables

```
explvars4 <- c(explvars3, "region", "educ", "phstat", "unemploy", "managedcare")
```

The comparisons will be based on expenditures in dollars using the held-out validation sample.

`@instructions`
- Use the training sample to fit a linear model with `logexpend` and explanatory variables listed in `explvars3`
- Predict expenditures (not logged) for the test data and summarize the fit using the sum of absolute prediction errors.
- Use the training sample to fit a benchmark linear model with `expendop` and explanatory variables listed in `explvars4`
- Predict expenditures for the test data and summarize the fit for the benchmark model using the sum of absolute prediction errors.
- Compare the predictions of the models graphically.

`@hint`


`@pre_exercise_code`
```{r}
meps <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/7b7dab6d0c528e4cd2f8d0e0fc7824a254429bf8/HealthMeps.csv", header = TRUE)
meps$logexpend <- log(meps$expendop)
# Split the sample into a `training` and `test` data
n <- nrow(meps)
set.seed(12347)
shuffled_meps <- meps[sample(n), ]
train_indices <- 1:round(0.75 * n)
train_meps    <- shuffled_meps[train_indices, ]
test_indices  <- (round(0.25 * n) + 1):n
test_meps     <- shuffled_meps[test_indices, ]
explvars3 <- c("gender", "age", "race", "mpoor", "anylimit", "income", "insure", "usc")
explvars4 <- c(explvars3, "region", "educ", "phstat", "unemploy", "managedcare")
```

`@sample_code`
```{r}
meps_mlr3 <- lm(logexpend ~ gender + age + mpoor + anylimit + income + insure + usc , data = train_meps)
predict_meps3 <- test_meps[,explvars3]
predict_mlr3  <- exp(predict(meps_mlr3, predict_meps3))
predict_err_mlr3 <- test_meps$expendop - predict_mlr3
sape3     <- sum(abs(predict_err_mlr3))/1000

meps_mlr4 <- lm(expendop ~ gender + age + race + region + educ + phstat + mpoor + anylimit + income + insure + usc + unemploy + managedcare, data = train_meps)
predict_meps4 <- test_meps[,explvars4]
predict_mlr4  <- predict(meps_mlr4, predict_meps4)
predict_err_mlr4 <- test_meps$expendop - predict_mlr4
sape4     <- sum(abs(predict_err_mlr4))/1000

sape3;sape4
```

`@solution`
```{r}
meps_mlr3 <- lm(logexpend ~ gender + age + mpoor + anylimit + income + insure + usc , data = train_meps)
predict_meps3 <- test_meps[,explvars3]
predict_mlr3  <- exp(predict(meps_mlr3, predict_meps3))
predict_err_mlr3 <- test_meps$expendop - predict_mlr3
sape3     <- sum(abs(predict_err_mlr3))/1000

meps_mlr4 <- lm(expendop ~ gender + age + race + region + educ + phstat + mpoor + anylimit + income + insure + usc + unemploy + managedcare, data = train_meps)
predict_meps4 <- test_meps[,explvars4]
predict_mlr4  <- predict(meps_mlr4, predict_meps4)
predict_err_mlr4 <- test_meps$expendop - predict_mlr4
sape4     <- sum(abs(predict_err_mlr4))/1000

sape3;sape4
```

`@sct`
```{r}
success_msg("Excellent! ")
```

---

## What the modeling procedure tells us a

```yaml
type: VideoExercise
key: 19a5897edf
xp: 50
video_link: //player.vimeo.com/video/289885887
```

`@projector_key`
a4e48282b4a5c9b11a4ff473d05854dc

---

## What modeling procedures tell us

```yaml
type: PureMultipleChoiceExercise
key: b2c9c6ef17
xp: 50
```

Which of the following is not important when interpreting the effects of individual variables?

`@hint`


`@possible_answers`
- A. Substantive effect
- B. Statistical significance
- [C. The amount of effort that it took to gather the data and do the analysis]
- D. Role of causality

`@feedback`
```{r}
msg1 = "Incorrect"
msg2 = "Incorrect"
msg3 = "Correct!"
msg4 = "Incorrect"
test_mc(3, feedback_msgs = c(msg1, msg2, msg3, msg4))
```

---

## Test of Vimeo

```yaml
type: VideoExercise
key: fc2d95ba73
xp: 50
video_link: https://player.vimeo.com/video/289885887
```

`@projector_key`
c77b3818c12540e69df74a4571d458dd
