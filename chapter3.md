---
title: 'Chapter 3. Multiple Linear Regression'
description: 'This chapter introduces linear regression in the case of several explanatory variables, known as multiple linear regression (MLR). Many basic linear regression concepts extend directly, including goodness of fit measures such as the coefficient of determination and inference using t-statistics. Multiple linear regression models provide a framework for summarizing highly complex, multivariate data. Because this framework requires only linearity in the parameters, we are able to fit models that are nonlinear functions of the explanatory variables, thus providing a wide scope of potential applications.'
---

## Term life data

```yaml
type: VideoExercise
key: 607ecb673f
xp: 50
```

`@projector_key`
dd5d704d841ca6fae0df18c9b9e020c0

---

## Method of least squares

```yaml
type: VideoExercise
key: 96d425a197
xp: 50
```

`@projector_key`
f251f87ff6f6208b2a3baba08e275eaa

---

## Least squares and term life data

```yaml
type: NormalExercise
key: 7e6008b3b9
xp: 100
```

The prior video introduced the *Survey of Consumer Finances* (SCF) term life data. A subset consisting of only those who purchased term life insurance, has already been read into a dataset `Term2`.
  
Suppose that you wish to predict the amount of term life insurance that someone will purchase but are uneasy about the `education` variable. Your sense is that, for purposes of purchasing life insurance, high school graduates and those that attend college should be treated the same. So, in this exercise, your will create a new variable, say `education1` that is equal to years of education for those with education less than or equal to 12 and is equal to 12 otherwise. The SCF `education` variable is the number of completed years of schooling and so 12 corresponds to completing high school in the US.

`@instructions`
- Use the [pmin()](https://www.rdocumentation.org/packages/mc2d/versions/0.1-17/topics/pmin) function to create the `education` variable as part of the `Term2` dataset and check your work by examining summary statistics for the revised `Term2` dataset.
- Examine correlations for the revised dataset.
- Using the method of least squares and the function [lm()](https://www.rdocumentation.org/packages/stats/versions/3.5.0/topics/lm), fit a MLR model using `logface` as the dependent variables and using `education`, `numhh`, and `logincome` as explanatory variables.
- With this fitted model and the function [predict()](https://www.rdocumentation.org/packages/stats/versions/3.5.0/topics/predict), predict the face amount of insurance that someone with income of 40,000, 11 years of education, and 4 people in the household would purchase.

`@pre_exercise_code`
```{r}
Term <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/efc64bc2d78cf6b48ad2c3f5e31800cb773de261/term_life.csv", header = TRUE)
Term1 <- subset(Term, subset = face > 0)
Term2 <- Term1[, c("education", "face", "income", "logface", "logincome", "numhh")]
#library(Rcmdr)
```

`@sample_code`
```{r}
Term2$education1 <- pmin(12, Term2$education)
#summvar <- Rcmdr::numSummary(Term2, statistic = c("mean", "sd", "quantiles"), quantiles = c(0, .5, 1))
#summvar
summary(Term2)
round(cor(Term2), digits=3)
model_mlr1 <- lm(logface ~ education1 + numhh + logincome, data = Term2)
newdata <- data.frame(logincome = log(40000), education1 = 11, numhh = 4)
exp(predict(model_mlr1, newdata))
```

`@solution`
```{r}
Term2$education1 <- pmin(12, Term2$education)
#summvar <- Rcmdr::numSummary(Term2, statistic = c("mean", "sd", "quantiles"), quantiles = c(0, .5, 1))
#summvar
summary(Term2)
round(cor(Term2), digits=3)
model_mlr1 <- lm(logface ~ education1 + numhh + logincome, data = Term2)
newdata <- data.frame(logincome = log(40000), education1 = 11, numhh = 4)
exp(predict(model_mlr1, newdata))
```

`@sct`
```{r}
success_msg("Excellent! You now have experience fitting a regression plane and using this plane for predictions, extending what Galton did when he used parents' heights to predict the height of an adult child. Well done!")
```

---

## Interpreting coefficients as proportional changes

```yaml
type: NormalExercise
key: 9d20cb1de4
xp: 100
```

In a previous exercise, you fit a MLR model using `logface` as the outcome variable and using `education`, `numhh`, and `logincome` as explanatory variables; the resulting fit is in the object `Term_mlr`. It turned out that the coefficient associated with `education` was 0.2064. We now wish to interpret this regression coefficient.

The typical interpretation of coefficients in a regression model is as a partial slope. That is, for the coefficient $b_1$ associated with $x_1$, we interpret $b_1$ to be amount that the expected outcome changes per unit change in $x_1$, holding the other explanatory variables fixed. For the term life example, the units of the outcome are in logarithmic dollars. So, for small values of $b_1$, we can interpret this to be a *proportional* change in dollars.

`@instructions`
- Determine least square fitted values for several selected values of `education`, holding other explantory variables fixed. For this part of the demonstration, we used their mean values.
- Determine the proportional changes. Note the relation between these values from a discrete change approximation to the regression coefficient for `education` equal to 0.2064.

`@hint`


`@pre_exercise_code`
```{r}
Term <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/efc64bc2d78cf6b48ad2c3f5e31800cb773de261/term_life.csv", header = TRUE)
Term1 <- subset(Term, subset = face > 0)
Term2 <- Term1[, c("education", "face", "income", "logface", "logincome", "numhh")]
Term_mlr <- lm(logface ~ education + numhh + logincome, data = Term2)
#summary(model_mlr)$coefficients[,1]
```

`@sample_code`
```{r}
educ_predict <- c(14,14.1,14.2,14.3)
newdata1 <- data.frame(logincome=mean(Term2$logincome), education=educ_predict, numhh=mean(Term2$numhh))
lsfits1 <- predict(model_mlr, newdata1)
lsfits1
lsfits1[2:4] - lsfits1[1:3]
pchange_fits1 <- exp(lsfits1[2:4] - lsfits1[1:3])
pchange_fits1
```

`@solution`
```{r}
educ_predict <- c(14,14.1,14.2,14.3)
newdata1 <- data.frame(logincome=mean(Term2$logincome), education=educ_predict, numhh=mean(Term2$numhh))
lsfits1 <- predict(Term_mlr, newdata1)
lsfits1
lsfits1[2:4] - lsfits1[1:3]
pchange_fits1 <- exp(lsfits1[2:4] - lsfits1[1:3])
pchange_fits1
```

`@sct`
```{r}
success_msg("Congratulations! From calculus, small changes in logarithmic values can be interpreted as proportional changes. This is the reason for using natural logarithms.")
```

---

## Interpreting coefficients as elasticities

```yaml
type: NormalExercise
key: 13219eeb75
lang: r
xp: 100
skills: 1
```

In a previous exercise, you fit a MLR model using `logface` as the outcome variable and using `education`, `numhh`, and `logincome` as explanatory variables; the resulting fit is in the object `Term_mlr`. It turned out that the coefficient associated with `logincome` was 0.4935. We now wish to interpret this regression coefficient. 

The typical interpretation of coefficients in a regression model is as a partial slope. When both $x_1$ and $y$ are in logarithmic units, then we can interpret $b_1$ to be ratio of two percentage changes, known as an *elasticity* in economics. Mathematically, we summarize this as
$$
\frac{\partial \ln y}{\partial \ln x} = \left(\frac{\partial y}{y}\right) ~/ ~\left(\frac{\partial x}{x}\right) .
$$

`@instructions`
- For several selected values of `logincome`, determine the corresponding proportional changes.
- Determine least square fitted values for several selected values of `logincome`, holding other explantory variables fixed.
- Determine the corresponding proportional changes for the fitted values. 
- Calculate the ratio of proportional changes of fitted values to those for income. Note the relation between these values (from a discrete change approximation) to the regression coefficient for `logincome` equal to 0.4935.

`@hint`


`@pre_exercise_code`
```{r}
Term <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/efc64bc2d78cf6b48ad2c3f5e31800cb773de261/term_life.csv", header = TRUE)
Term1 <- subset(Term, subset = face > 0)
Term2 <- Term1[, c("education", "face", "income", "logface", "logincome", "numhh")]
Term_mlr <- lm(logface ~ education + numhh + logincome, data = Term2)
```

`@sample_code`
```{r}
logincome_pred <- c(11,11.1,11.2,11.3)
pchange_income <- 100*(exp(logincome_pred[2:4])/exp(logincome_pred[1:3])-1)
pchange_income
newdata2 <- data.frame(logincome=logincome_pred,education=mean(Term2$education), numhh=mean(Term2$numhh))
lsfits2 <- predict(Term_mlr, newdata2)
pchange_fits2 <- 100*(exp(lsfits2[2:4])/exp(lsfits2[1:3])-1)
pchange_fits2
pchange_fits2/pchange_income
```

`@solution`
```{r}
logincome_pred <- c(11,11.1,11.2,11.3)
pchange_income <- 100*(exp(logincome_pred[2:4])/exp(logincome_pred[1:3])-1)
pchange_income
newdata2 <- data.frame(logincome=logincome_pred,education=mean(Term2$education), numhh=mean(Term2$numhh))
lsfits2 <- predict(Term_mlr, newdata2)
pchange_fits2 <- 100*(exp(lsfits2[2:4])/exp(lsfits2[1:3])-1)
pchange_fits2
pchange_fits2/pchange_income
```

`@sct`
```{r}
success_msg("Congratulations! When both $x_1$ and $y$ are in logarithmic units, then we can interpret $b_1$ to be ratio of two percentage changes, known as an *elasticity* in economics.")
```

---

## Statistical inference and multiple linear regression

```yaml
type: VideoExercise
key: cfa3072c7c
xp: 50
```

`@projector_key`
c77074330c0754662a1dbb56fbca3e43

---

## Statistical inference and term life

```yaml
type: NormalExercise
key: 81c84d6420
xp: 100
```

In later chapters, we will learn how to specify a model using diagnostics techniques; these techniques were used to specify face in log dollars for the outcome and similarly income in log dollars as an explanatory variable. Just to see how things work, in this exercise we will create new variables `face` and `income` that are in the original units and run a regression with these. We have already seen that rescaling by constants do not affect relationships but can be helpful with interpretations, so we define both  `face` and `income` to be in thousands of dollars. A prior video introduced the term life dataset `Term2`.

`@instructions`
- Create `Term2$face` by exponentiating `logface` and dividing by 1000. For convenience, we are storing this variable in the data set `Term2`. Use the same process to create `Term2$income`.
- Run a regression using `face` as the outcome variable and `education`, `numhh`, and `income` as explanatory variables.
- Summarize this model and identify the residual standard error ($s$) as well as the coefficient of determination ($R^2$) and the version adjusted for degrees of freedom ($R_a^2$).

`@pre_exercise_code`
```{r}
Term <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/efc64bc2d78cf6b48ad2c3f5e31800cb773de261/term_life.csv", header = TRUE)
Term1 <- subset(Term, subset = face > 0)
Term2 <- Term1[, c("education", "face", "income", "logface", "logincome", "numhh")]
```

`@sample_code`
```{r}
Term2$face <- exp(Term2$logface)/1000
Term2$income <- exp(Term2$logincome)/1000
Term_mlr1 <- lm(face ~ education + numhh + income, data = Term2)
summary(Term_mlr1)
```

`@solution`
```{r}
Term2$face <- exp(Term2$logface)/1000
Term2$income <- exp(Term2$logincome)/1000
Term_mlr1 <- lm(face ~ education + numhh + income, data = Term2)
summary(Term_mlr1)
```

`@sct`
```{r}
success_msg("Congratulations! Compare these goodness of fit measures to those where income and face are in logarithmic units. Although not the only indicators, you will see that the proportion of variability explained (R square) and the statistical significance of coefficients are strikingly higher in the model with variables in logged units.")
```

---

## Binary variables

```yaml
type: VideoExercise
key: ff359ef406
xp: 50
```

`@projector_key`
a27f0e9bfd20f2e7d3c30b25a5acb7a8

---

## Binary variables and term life

```yaml
type: NormalExercise
key: 2f0001026b
xp: 100
```

In the prior video, we saw how the variable `single` can be used with logarithmic income to explain logarithmic face amounts of term life insurance that people purchase. The coefficient associated with this variable turns out to be negative which is intuitively appealing; if an individual is single, then that person may not have the strong need to purchase financial security for others in the event of unexpected death. In this exercise, we will extend this by incorporating `single` into our larger regression model that contains other explanatory varibles, `logincome`, `education` and `numhh`.

`@instructions`
- Calculate a table of correlation coefficients to examine pairwise linear relationships among the variables `numhh`, `education`, `logincome`, `single`, and  `logface`.
- Fit a MLR model of `logface` using explanatory variables `numhh`, `education`, `logincome`, and `single`. Examine the residual standard deviation $s$, the coefficient of determination $R^2$, and the adjusted version $R_a^2$. Also note the statistical significance of the coefficient associated with `single`.
- Repeat the MLR model fit while adding the interaction term  `single*logincome`.

`@pre_exercise_code`
```{r}
Term <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/efc64bc2d78cf6b48ad2c3f5e31800cb773de261/term_life.csv", header = TRUE)
Term1 <- subset(Term, subset = face > 0)
Term4 <- Term1[,c("numhh", "education", "logincome", "marstat", "logface")]
Term4$single <- 1*(Term4$marstat == 0)
```

`@sample_code`
```{r}
round(cor(Term4[,c("numhh", "education", "logincome", "single", "logface")]), digits = 3)
Term_mlr3 <- lm(logface ~ education + numhh + logincome + single, data = Term4)
summary(Term_mlr3)
Term_mlr4 <- lm(logface ~ education + numhh + logincome + single + single*logincome, data = Term4)
summary(Term_mlr4)
```

`@solution`
```{r}
round(cor(Term4[,c("numhh", "education", "logincome", "single", "logface")]), digits = 3)
Term_mlr3 <- lm(logface ~ education + numhh + logincome + single, data = Term4)
summary(Term_mlr3)
Term_mlr4 <- lm(logface ~ education + numhh + logincome + single + single*logincome, data = Term4)
summary(Term_mlr4)
```

`@sct`
```{r}
success_msg("Excellent! From a correlation table, you saw that there are relationships with among explanatory variables and so it is not clear whether adding `single` to the model would be helpful. You explored this by first fitting a model by just adding the binary variable single, examined summary statistics, and checked the significance of the variable. Then, you explored the utility of the interaction of `single` with logarithmic income. Well done!")
```

---

## Categorical variables

```yaml
type: VideoExercise
key: ec71e48d7d
xp: 50
```

`@projector_key`
a96ac5c8eed3af533c4c8897c2b57542

---

## Categorical variables and Wisconsin hospital costs

```yaml
type: NormalExercise
key: 29d2203fad
xp: 100
```

This exercise examines the impact of various predictors on hospital charges. Identifying predictors of hospital charges can provide direction for hospitals, government, insurers and consumers in controlling these variables that in turn leads to better control of hospital costs. The data, from 1989, are aggregated by: 

- `drg`, diagnostic related groups of costs, 
- `payer`, type of health care provider (Fee for service, HMO, and other), and 
- `hsa`, nine major geographic areas.

Some preliminary analysis of the data has already been done. In this exercise, we will analyze `logcharge`, the logarithm of total hospital charges per number of discharges, in terms of `log_numdschg`, the logarithm of the number of discharges. In the dataset `Hcost` which has been loaded in advance, we restrict consideration to three types of drgs, numbers 209, 391, and 431.

`@instructions`
- Fit a basic linear regression model using logarithmic number of discharges to predict logarithmic hospital costs and superimposed the fitted regression line on the scatter plot.
- Produce a scatter plot of logarithmic number of discharges to predict logarithmic hospital costs. Allow plotting symbols and colors to vary by diagnostic related group.
- Fit a MLR model using logarithmic number of discharges to predict logarithmic hospital costs, allowing intercepts and slopes to vary by diagnostic related groups.
- Superimpose the fits from the MLR model on the scatter plot of logarithmic number of discharges to predict logarithmic hospital costs.

`@pre_exercise_code`
```{r}
Hcost <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/2cc1e2739bf827093db31d7c4e6dcdc348ac984e/WiscHcosts.csv", header = TRUE)
Hcost1 <- subset(Hcost, drg == 209|drg == 391|drg == 430)
```

`@sample_code`
```{r}
hosp_blr <- lm(logcharge~log_numdschg, data=Hcost1)
plot(logcharge~log_numdschg, data=Hcost1, xlab = "log number discharges", ylab = "log charge")
abline(hosp_blr, col="red")

plot(logcharge~log_numdschg, data=Hcost1, xlab = "log number discharges", ylab = "log charge",
    pch= as.numeric(as.factor(Hcost1$drg)), 
    col = c("red", "black", "blue")[as.factor(Hcost1$drg)])
legend("left", legend=c("drg 209","drg 391", "drg 430"), col=c("red", "black", "blue"), pch = c(1,2,3))

hosp_mlr <- lm(logcharge~log_numdschg + as.factor(drg)*log_numdschg, data=Hcost1)
#summary(hosp_mlr)$coefficients[,1]
plot(logcharge~log_numdschg, data=Hcost1, xlab = "log number discharges", ylab = "log charge",
         pch= as.numeric(as.factor(Hcost1$drg)), 
    col = c("red", "black", "blue")[as.factor(Hcost1$drg)])
xseq <- seq(0,10,length.out=100)
coef <- summary(hosp_mlr)$coefficients[,1]
fit209 <- coef[1] + coef[2]*xseq
lines(xseq,fit209, col="red")
fit391 <- coef[1] + coef[3] + (coef[2] + coef[5])*xseq
lines(xseq,fit391, col="black")
fit430 <- coef[1] + coef[4] + (coef[2] + coef[6])*xseq
lines(xseq,fit430, col="blue")
```

`@solution`
```{r}
hosp_blr <- lm(logcharge~log_numdschg, data=Hcost1)
plot(logcharge~log_numdschg, data=Hcost1, xlab = "log number discharges", ylab = "log charge")
abline(hosp_blr, col="red")

plot(logcharge~log_numdschg, data=Hcost1, xlab = "log number discharges", ylab = "log charge",
    pch= as.numeric(as.factor(Hcost1$drg)), 
    col = c("red", "black", "blue")[as.factor(Hcost1$drg)])
legend("left", legend=c("drg 209","drg 391", "drg 430"), col=c("red", "black", "blue"), pch = c(1,2,3))

hosp_mlr <- lm(logcharge~log_numdschg + as.factor(drg)*log_numdschg, data=Hcost1)
#summary(hosp_mlr)$coefficients[,1]
plot(logcharge~log_numdschg, data=Hcost1, xlab = "log number discharges", ylab = "log charge",
         pch= as.numeric(as.factor(Hcost1$drg)), 
    col = c("red", "black", "blue")[as.factor(Hcost1$drg)])
xseq <- seq(0,10,length.out=100)
coef <- summary(hosp_mlr)$coefficients[,1]
fit209 <- coef[1] + coef[2]*xseq
lines(xseq,fit209, col="red")
fit391 <- coef[1] + coef[3] + (coef[2] + coef[5])*xseq
lines(xseq,fit391, col="black")
fit430 <- coef[1] + coef[4] + (coef[2] + coef[6])*xseq
lines(xseq,fit430, col="blue")
```

`@sct`
```{r}
success_msg("Congratulations! When you superimposed the fits from the MLR model on the scatter plot of logarithmic number of discharges to predict logarithmic hospital costs, note how slopes differ dramatically from the slope from the basic linear regression model.")
```

---

## Hypothesis testing

```yaml
type: VideoExercise
key: 156ef40aaf
xp: 50
```

`@projector_key`
df2187184761fab4d774149af7725157

---

## Hypothesis testing and term life

```yaml
type: NormalExercise
key: f80245771a
xp: 100
```

In the context of our `Term life` data, let us compare model based on the binary variable that indicates whether a survey respondent is single versus the more complex marital status, `marstat`. In principle, the more detailed information the better but it may be that the additional information in `marstat`, compared to `single`, does not help fit the data in a significantly better way. 

As part of the preparatory work, the dataset `Term4` is available that includes the binary variable `single` and the factor `marstat`. Moreover, the object `Term_mlr` contains information in a multiple linear regression fit of `logface` on the base explanatory variables 'logincome`, `education`, and `numhh`.

`@instructions`
- Fit a MLR model using the base explanatory variables plus `single` and another model using the base variables plus `marstat`.
- Use the F test to decide whether the additional complexity `marstat` is warranted by calculating the p-value associated with this test.
- Fit a MLR model using the base explanatory variables plus `single` interacted with `logincome` and another model using the base variables plus `marstat` interacted with `logincome`.
- Use the F test to decide whether the additional complexity `marstat` is warranted by calculating the p-value associated with this test.

`@pre_exercise_code`
```{r}
Term <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/efc64bc2d78cf6b48ad2c3f5e31800cb773de261/term_life.csv", header = TRUE)
Term1 <- subset(Term, subset = face > 0)
Term4 <- Term1[,c("numhh", "education", "logincome", "marstat", "logface")]
Term4$single <- 1*(Term4$marstat == 0)
Term4$marstat <- as.factor(Term4$marstat)
Term_mlr <- lm(logface ~ logincome + education + numhh , data = Term4)
anova(Term_mlr)
```

`@sample_code`
```{r}
Term_mlr3 <- lm(logface ~ logincome + education + numhh + single*logincome, data = Term4)
anova(Term_mlr3)
Fstat <- (anova(Term_mlr)$`Sum Sq`[4] - anova(Term_mlr3)$`Sum Sq`[6])/(2*anova(Term_mlr3)$`Mean Sq`[6])
Fstat
cat("p-value is", 1 - pf(Fstat, df1 = 2 , df2 = anova(Term_mlr3)$Df[6]))

Term_mlr4 <- lm(logface ~ logincome + education + numhh +marstat*logincome, data = Term4)
anova(Term_mlr4)
Fstat <- (anova(Term_mlr3)$`Sum Sq`[6] - anova(Term_mlr4)$`Sum Sq`[6])/(2*anova(Term_mlr4)$`Mean Sq`[6])
Fstat
cat("p-value is", 1 - pf(Fstat, df1 = 2 , df2 = anova(Term_mlr4)$Df[6]))
```

`@solution`
```{r}
Term_mlr3 <- lm(logface ~ logincome + education + numhh + single*logincome, data = Term4)
anova(Term_mlr3)
Fstat <- (anova(Term_mlr)$`Sum Sq`[4] - anova(Term_mlr3)$`Sum Sq`[6])/(2*anova(Term_mlr3)$`Mean Sq`[6])
Fstat
cat("p-value is", 1 - pf(Fstat, df1 = 2 , df2 = anova(Term_mlr3)$Df[6]))

Term_mlr4 <- lm(logface ~ logincome + education + numhh +marstat*logincome, data = Term4)
anova(Term_mlr4)
Fstat <- (anova(Term_mlr3)$`Sum Sq`[6] - anova(Term_mlr4)$`Sum Sq`[6])/(2*anova(Term_mlr4)$`Mean Sq`[6])
Fstat
cat("p-value is", 1 - pf(Fstat, df1 = 2 , df2 = anova(Term_mlr4)$Df[6]))
```

`@sct`
```{r}
success_msg("Congratulations! Hypothesis testing is a type of 'statistical inference' in that it is one of the main ways in which we can summarize what a model is 'inferring' about the real world (in contrast to mathematical 'deduction'.) Moreover, as we will see in the next chapter, it can also be used as a tool to develop a model.")
```

---

## Hypothesis testing and Wisconsin hospital costs

```yaml
type: NormalExercise
key: 25140024df
xp: 100
```

In a previous exercise, you were introduced to a dataset with hospital charges aggregated by:

- `drg`, diagnostic related groups of costs, 
- `payer`, type of health care provider (Fee for service, HMO, and other), and 
- `hsa`, nine major geographic areas.

We continue our analysis of the outcome variable  `logcharge`, the logarithm of total hospital charges per number of discharges, in terms of `log_numdschg`, the logarithm of the number of discharges, as well as the three categorical variables used in the aggregation. As before, we restrict consideration to three types of drgs, numbers 209, 391, and 431 that has been preloaded in the dataset `Hcost1`.

`@instructions`
- Fit a basic linear regression model using logarithmic hospital costs as the outcome variable and explanatory variable logarithmic number of discharges.
- Fit a MLR model using logarithmic hospital costs as the outcome variable and explanatory variables logarithmic number of discharges and the categorical variable diagnostic related group. Identify the *F* statistic and *p* value that tests the importance of diagnostic related group. 
- Fit a MLR model using logarithmic hospital costs as the outcome variable and explanatory variable logarithmic number of discharges interacted with diagnostic related group. Identify the *F* statistic and *p* value that tests the importance of diagnostic related group interaction with logarithmic number of discharges.
- Calculate a coefficient of determination, $R^2$, for each of these models as well as for a model using logarithmic number of discharges and categorical variable `hsa` as predictors.

`@hint`


`@pre_exercise_code`
```{r}
Hcost <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/2cc1e2739bf827093db31d7c4e6dcdc348ac984e/WiscHcosts.csv", header = TRUE)
Hcost1 <- subset(Hcost, drg == 209|drg == 391|drg == 430)
```

`@sample_code`
```{r}
hosp_blr <- lm(logcharge ~ log_numdschg , data=Hcost1)
anova(hosp_blr)
hosp_mlr1 <- lm(logcharge ~ log_numdschg + as.factor(drg), data=Hcost1)
anova(hosp_mlr1)
hosp_mlr2 <- lm(logcharge ~ log_numdschg + as.factor(drg)*log_numdschg, data=Hcost1)
anova(hosp_mlr2)
summary(hosp_blr)$r.squared
summary(hosp_mlr1)$r.squared
summary(hosp_mlr2)$r.squared

hosp_mlr3 <- lm(logcharge ~ log_numdschg + as.factor(hsa)*log_numdschg, data=Hcost1)
summary(hosp_mlr3)$r.squared
```

`@solution`
```{r}
hosp_blr <- lm(logcharge ~ log_numdschg , data=Hcost1)
anova(hosp_blr)
hosp_mlr1 <- lm(logcharge ~ log_numdschg + as.factor(drg), data=Hcost1)
anova(hosp_mlr1)
hosp_mlr2 <- lm(logcharge ~ log_numdschg + as.factor(drg)*log_numdschg, data=Hcost1)
anova(hosp_mlr2)
summary(hosp_blr)$r.squared
summary(hosp_mlr1)$r.squared
summary(hosp_mlr2)$r.squared

hosp_mlr3 <- lm(logcharge ~ log_numdschg + as.factor(hsa)*log_numdschg, data=Hcost1)
summary(hosp_mlr3)$r.squared
```

`@sct`
```{r}
success_msg("Congratulations! By examining the coefficients of determination, $R^2$, for each of these models, you see that this provides one piece of evidence that the `hsa` is a far poorer predictor of costs than `drg`.")
```

---

## Hypothesis testing and auto claims

```yaml
type: NormalExercise
key: 92b61ebeba
xp: 100
```

As an actuarial analyst, you are working with a large insurance company to help them understand their claims distribution for their private passenger automobile policies. You have available claims data for a recent year, consisting of:

- `state`: codes 01 through 17 used, with each code randomly assigned to an actual individual state
- `class`: rating class of operator, based on age, gender, marital status, use of vehicle, as coded in a separate PDF file
- `gender`: operator gender 
- `age`: operator age
- `paid`: amount paid to settle and close a claim.

You are focusing on older drivers, 50 and higher, for which there are *n* = 6,773 claims available.

`@instructions`
a. Run a regression of `logpaid` on `age`. Is `age` a statistically significant variable? To respond to this question, use a formal test of hypothesis. State your null and alternative hypotheses, decision-making criterion, and your decision-making rule. Also comment on the goodness of fit of this variable.

b. Consider using class as a single explanatory variable. Use the one factor to estimate the model and respond to the following questions.

    b (i). What is the point estimate of claims in class C7, drivers 50-69, driving to work or school, less than 30 miles per week with annual mileage under 7500, in natural logarithmic units?

    b (ii). Determine the corresponding 95\% confidence interval of expected claims, in natural logarithmic units.

    b (iii). Convert the 95\% confidence interval of expected claims that you determined in part b(ii) to dollars.

c. Run a regression of `logpaid` on `age`, `gender` and the categorical variables `state` and `class`.

    c (i). Is `gender` a statistically significant variable? To respond to this question, use a formal test of hypothesis. State your null and alternative hypotheses, decision-making criterion, and your decision-making rule.

    c (ii). Is `class` a statistically significant variable? To respond to this question, use a formal test of hypothesis. State your null and alternative hypotheses, decision-making criterion, and your decision-making rule.

    c (iii). Use the model to provide a point estimate of claims in dollars (not log dollars) for a male age 60 in STATE 2 in `class` C7.

    c (iv). Write down the coefficient associated with `class` C7 and interpret this coefficient.

`@hint`


`@pre_exercise_code`
```{r}
#AutoC <- read.csv("CSVData\\Auto_claims.csv", header = TRUE)
```

`@sct`
```{r}
success_msg("Excellent! ")
```
