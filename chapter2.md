---
title: 'Chapter 2. Basic Linear Regression'
description: 'This chapter considers regression in the case of only one explanatory variable. Despite this seeming simplicity, many deep ideas of regression can be developed in this framework. By limiting ourselves to the one variable case, we can illustrate the relationships between two variables graphically. Graphical tools prove to be important for developing a link between the data and a predictive model.'
---

## Correlation

```yaml
type: NormalExercise
key: a83ceb1fad
xp: 100
```

<iframe id="kaltura_player" src="https://cdnapisec.kaltura.com/p/1660902/sp/166090200/embedIframeJs/uiconf_id/25916071/partner_id/1660902?iframeembed=true&playerId=kaltura_player&entry_id=1_qlpe86qi&flashvars[streamerType]=auto&amp;flashvars[localizationCode]=en&amp;flashvars[leadWithHTML5]=true&amp;flashvars[sideBarContainer.plugin]=true&amp;flashvars[sideBarContainer.position]=left&amp;flashvars[sideBarContainer.clickToClose]=true&amp;flashvars[chapters.plugin]=true&amp;flashvars[chapters.layout]=vertical&amp;flashvars[chapters.thumbnailRotator]=false&amp;flashvars[streamSelector.plugin]=true&amp;flashvars[EmbedPlayer.SpinnerTarget]=videoHolder&amp;flashvars[dualScreen.plugin]=true&amp;&wid=1_w18zht77" width="649" height="401" allowfullscreen webkitallowfullscreen mozAllowFullScreen allow="autoplay *; fullscreen *; encrypted-media *" frameborder="0" title="Kaltura Player"></iframe>

`@instructions`
In this module, you learn how to:

- Calculate and interpret a correlation coefficient
- Interpret correlation coefficients by visualizing scatter plots

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

## Correlations and the Wisconsin lottery

```yaml
type: NormalExercise
key: a485262f71
xp: 100
```

The Wisconsin lottery dataset has already been read into a dataset `Wisc_lottery`.

Like insurance, lotteries are uncertain events and so the skills to work with and interpret lottery data are readily applicable to insurance. It is common to report sales and population in thousands of units, so this exercise gives you practice in rescaling data via linear transformations.

`@instructions`
- From the available population and sales variables, create new variables, say, `pop_1000` and `sales_1000` that are in thousands (of people and of dollars, respectively).
- Create summary statistics for the new variables.
- Plot `pop_1000` versus `sales_1000`.
- Calculate the correlation between `pop_1000` versus `sales_1000` using the function [cor()](https://www.rdocumentation.org/packages/stats/versions/3.5.0/topics/cor). How does this differ between the correlation between population and sales in the original units?

`@hint`
No hints for now. No hints for now. No hints for now.

`@pre_exercise_code`
```{r}
Lot <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/a792b30fb32b0896dd6894501cbab32b5d48df51/Wisc_lottery.csv", header = TRUE)
#library(Rcmdr)
```

`@sample_code`
```{r}
Lot$pop_1000 <- ___/1000
Lot$sales_1000 <- ___/1000
#(as.data.frame(psych::describe(Lot)))[,c(2,3,4,5,8,9)]
```

`@solution`
```{r}
Lot$pop_1000 <- Lot$pop/1000
Lot$sales_1000 <- Lot$sales/1000
#numSummary(Lot[,c("pop_1000", "sales_1000")], statistics = c("mean", "sd", "quantiles"), quantiles = c(0,.5,1))
#(as.data.frame(psych::describe(Lot)))[,c(2,3,4,5,8,9)]
plot(Lot$pop_1000, Lot$sales_1000)
cor(Lot$pop_1000, Lot$sales_1000)
```

`@sct`
```{r}
ex() %>% check_object("Lot") %>% check_column("pop_1000") %>% check_equal()
ex() %>% check_object("Lot") %>% check_column("sales_1000") %>% check_equal()
ex() %>% check_function("plot") %>% check_arg("x") %>% check_equal()
ex() %>% check_function("plot") %>% check_arg("y") %>% check_equal()
ex() %>% check_function("cor") %>% check_result() %>% check_equal()
success_msg("Congratulations! We will rescale data using 'linear' transformations regularly. In part we do this for communicating our analysis to others. Also in part, this is for our own convenience as it can allow us to see patterns more readily.")
```

---

## Method of least squares

```yaml
type: NormalExercise
key: 92d9c32078
xp: 100
```

<iframe id="kaltura_player" src="https://cdnapisec.kaltura.com/p/1660902/sp/166090200/embedIframeJs/uiconf_id/25916071/partner_id/1660902?iframeembed=true&playerId=kaltura_player&entry_id=1_6finvcp7&flashvars[streamerType]=auto&amp;flashvars[localizationCode]=en&amp;flashvars[leadWithHTML5]=true&amp;flashvars[sideBarContainer.plugin]=true&amp;flashvars[sideBarContainer.position]=left&amp;flashvars[sideBarContainer.clickToClose]=true&amp;flashvars[chapters.plugin]=true&amp;flashvars[chapters.layout]=vertical&amp;flashvars[chapters.thumbnailRotator]=false&amp;flashvars[streamSelector.plugin]=true&amp;flashvars[EmbedPlayer.SpinnerTarget]=videoHolder&amp;flashvars[dualScreen.plugin]=true&amp;&wid=1_5pgz5f8p" width="649" height="401" allowfullscreen webkitallowfullscreen mozAllowFullScreen allow="autoplay *; fullscreen *; encrypted-media *" frameborder="0" title="Kaltura Player"></iframe>

`@instructions`
In this module, you learn how to:

- Fit a line to data using the method of least squares
- Predict an observation using a least squares fitted line

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

## Least squares fit using housing prices

```yaml
type: NormalExercise
key: 97ec81af2a
xp: 100
```

The prior video analyzed the effect that a zip code's population has on lottery sales. Instead of population, suppose that you wish to understand the effect that housing prices have on the sale of lottery tickets. In the Wisconsin lottery dataset  `Wisc_lottery` is the variable `medhome` which is the median house price for each zip code, in thousands of dollars. In this exercise, you will get a feel for the distribution of this variable by examining summary statistics, examining its relationship with sales graphically and via correlations, fitting a basic linear regression model and using this model to predict sales.

`@instructions`
- Summarize the distributions of `medhome` and `sales`.
- Plot `medhome` versus `sales`. Summarize this relationship by calculating the corresponding correlation coefficient using the function [cor()](https://www.rdocumentation.org/packages/stats/versions/3.5.0/topics/cor).
- Using the function [lm()](https://www.rdocumentation.org/packages/stats/versions/3.5.0/topics/lm), regress `medhome`, the explanatory variable, on `sales`, the dependent variable.
- Use the function [predict()](https://www.rdocumentation.org/packages/stats/versions/3.5.0/topics/predict) and the fitted regression model to predict sales assuming that the median house price for a zip code is 50 (in thousands of dollars).

`@hint`
No hints for now. No hints for now. No hints for now.

`@pre_exercise_code`
```{r}
Lot <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/a792b30fb32b0896dd6894501cbab32b5d48df51/Wisc_lottery.csv", header = TRUE)
```

`@sample_code`
```{r}
#(as.data.frame(psych::describe(Lot[,c("medhome", "sales")])))[,c(2,3,4,5,8,9)]
cor(Lot$medhome,Lot$sales)
plot(Lot$medhome,Lot$sales)
model_blr1 <- lm(sales ~ medhome, data = Lot)
round(coefficients(model_blr1), digits=4)
newdata <- data.frame(medhome = 50)
predict(model_blr1, newdata)
```

`@solution`
```{r}
#(as.data.frame(psych::describe(Lot[,c("medhome", "sales")])))[,c(2,3,4,5,8,9)]
cor(Lot$medhome,Lot$sales)
plot(Lot$medhome,Lot$sales)
model_blr1 <- lm(sales ~ medhome, data = Lot)
round(coefficients(model_blr1), digits=4)
newdata <- data.frame(medhome = 50)
predict(model_blr1, newdata)
```

`@sct`
```{r}
ex() %>% check_function("cor") %>% check_result() %>% check_equal()
ex() %>% check_function("plot") %>% {
  check_arg(., "x") %>% check_equal()
  check_arg(., "y") %>% check_equal()
}
ex() %>% check_object("model_blr1") %>% check_equal()
ex() %>% check_function("lm") %>% {
  check_arg(., "formula") %>% check_equal()
  check_arg(., "data") %>% check_equal()
}
ex() %>% check_function("round") %>% {
  check_arg(., "x") %>% check_equal()
  check_arg(., "digits") %>% check_equal()
}
ex() %>% check_object("newdata") %>% check_equal()
ex() %>% check_function("predict") %>% {
  check_arg(., "object") %>% check_equal()
  check_arg(., "newdata") %>% check_equal()
}
success_msg("Congratulations! You now have experience fitting a regression line and using this line for predictions, just as Galton did when he used parents' heights to predict the height of an adult child. Well done!")
```

---

## Understanding Variability

```yaml
type: NormalExercise
key: 55a24164a1
xp: 100
```

<iframe id="kaltura_player" src="https://cdnapisec.kaltura.com/p/1660902/sp/166090200/embedIframeJs/uiconf_id/25916071/partner_id/1660902?iframeembed=true&playerId=kaltura_player&entry_id=1_a6j1eyc0&flashvars[streamerType]=auto&amp;flashvars[localizationCode]=en&amp;flashvars[leadWithHTML5]=true&amp;flashvars[sideBarContainer.plugin]=true&amp;flashvars[sideBarContainer.position]=left&amp;flashvars[sideBarContainer.clickToClose]=true&amp;flashvars[chapters.plugin]=true&amp;flashvars[chapters.layout]=vertical&amp;flashvars[chapters.thumbnailRotator]=false&amp;flashvars[streamSelector.plugin]=true&amp;flashvars[EmbedPlayer.SpinnerTarget]=videoHolder&amp;flashvars[dualScreen.plugin]=true&amp;&wid=1_x5oq2i5k" width="649" height="401" allowfullscreen webkitallowfullscreen mozAllowFullScreen allow="autoplay *; fullscreen *; encrypted-media *" frameborder="0" title="Kaltura Player"></iframe>

`@instructions`
In this module, you learn how to:

- Visualize the ANOVA decomposition of variability
- Calculate and interpret $R^2$, the coefficient of determination
- Calculate and interpret $s^2$ the mean square error
- Explain the components of the ANOVA table

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

## Summarizing measures of uncertainty

```yaml
type: NormalExercise
key: e97a0cfbd0
xp: 100
```

In a previous exercise, you developed a regression line to fit the variable `medhome`, the median house price for each zip code, as a predictor of lottery sales. The regression of `medhome` on `sales` has been summarized in the `R` object `model_blr`.

How reliable is the regression line? In this excercise, you will compute some of the standard measures that are used to summarize the goodness of this fit.

`@instructions`
- Summarize the fitted regression model in an ANOVA table.
- Determine the size of the typical residual, $s$.
- Determine the coefficient of determination, $R^2$.

`@hint`
No hints for now. No hints for now. No hints for now.

`@pre_exercise_code`
```{r}
Lot <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/a792b30fb32b0896dd6894501cbab32b5d48df51/Wisc_lottery.csv", header = TRUE)
```

`@sample_code`
```{r}
model_blr <- lm(sales  ~ medhome, data = Lot)
anova(model_blr)
sqrt(anova(model_blr)$Mean[2])
summary(model_blr)$r.squared
```

`@solution`
```{r}
model_blr <- lm(sales  ~ medhome, data = Lot)
anova(model_blr)
sqrt(anova(model_blr)$Mean[2])
summary(model_blr)$r.squared
```

`@sct`
```{r}
ex() %>% check_object("model_blr") %>% check_equal()
ex() %>% check_function("lm") %>% {
  check_arg(., "formula") %>% check_equal()
  check_arg(., "data") %>% check_equal()
}
ex() %>% check_function("anova") %>% check_arg("object") %>% check_equal()
ex() %>% check_function("sqrt") %>% check_result() %>% check_equal()
ex() %>% check_function("summary") %>% check_result() %>% check_equal()
success_msg("Congratulations! It will be helpful if you compare the results of this exercise to the regression of `pop` on `sales` from the prior video. We have seen that `pop` is more highly correlated with `sales` than `medhome`, so we are expecting greater uncertainty in this regression fit.")
```

---

## Effects of linear transforms on measures of uncertainty

```yaml
type: NormalExercise
key: 1f57405e27
xp: 100
```

Let us see how rescaling, a linear transformation, affects our measures of uncertainty. As before, the Wisconsin lottery dataset  `Wisc_lottery` is available and this dataset contains `sales_1000`, sales in thousands of dollars, and `pop_1000`, zip code population in thousands. How do measures of uncertainty change when going from the original units to thousands of those units?

`@instructions`
- Run a regression of `pop` on `sales_1000` and summarize this in an ANOVA table.
- For this regression, determine the $s$ and the coefficient of determiniation, $R^2$.  
- Run a regression of `pop_1000` on `sales_1000` and summarize this in an ANOVA table.
- For this regression, determine the $s$ and the coefficient of determination, $R^2$.

`@hint`
No hints for now. No hints for now. No hints for now.

`@pre_exercise_code`
```{r}
Lot <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/a792b30fb32b0896dd6894501cbab32b5d48df51/Wisc_lottery.csv", header = TRUE)
Lot$pop_1000 <- Lot$pop/1000
Lot$sales_1000 <- Lot$sales/1000
```

`@sample_code`
```{r}
model_blr1 <- lm(sales_1000  ~ pop, data = Lot)
anova(model_blr1)
sqrt(anova(model_blr1)$Mean[2])
summary(model_blr1)$r.squared
model_blr2 <- lm(sales_1000  ~ pop_1000 , data = Lot)
anova(model_blr2)
sqrt(anova(model_blr2)$Mean[2])
summary(model_blr2)$r.squared
```

`@solution`
```{r}
model_blr1 <- lm(sales_1000  ~ pop, data = Lot)
anova(model_blr1)
sqrt(anova(model_blr1)$Mean[2])
summary(model_blr1)$r.squared
model_blr2 <- lm(sales_1000  ~ pop_1000 , data = Lot)
anova(model_blr2)
sqrt(anova(model_blr2)$Mean[2])
summary(model_blr2)$r.squared
```

`@sct`
```{r}
ex() %>% check_object("model_blr1") %>% check_equal()
ex() %>% check_function("lm",index=1) %>% {
  check_arg(., "formula") %>% check_equal()
  check_arg(., "data") %>% check_equal()
}
ex() %>% check_function("anova",index=1) %>% check_result() %>% check_equal()
ex() %>% check_function("sqrt",index=1) %>% check_result() %>% check_equal()
ex() %>% check_function("anova",index=2) %>% check_result() %>% check_equal()
ex() %>% check_function("summary",index=1) %>% check_result() %>% check_equal()
ex() %>% check_object("model_blr2") %>% check_equal()
ex() %>% check_function("lm",index=2) %>% {
  check_arg(., "formula") %>% check_equal()
  check_arg(., "data") %>% check_equal()
}
ex() %>% check_function("anova",index=3) %>% check_result() %>% check_equal()
ex() %>% check_function("sqrt",index=2) %>% check_result() %>% check_equal()
ex() %>% check_function("anova",index=4) %>% check_result() %>% check_equal()
ex() %>% check_function("summary",index=2) %>% check_result() %>% check_equal()
success_msg("Congratulations! In this exercise, you have seen that rescaling does not affect our measures of goodness of fit in any meaningful way. For example, coeffcient of determinations are completely unaffected. This is helpful because we will rescale variables extensively in our search for patterns in the data.")
```

---

## Statistical inference

```yaml
type: NormalExercise
key: f24edc35f9
xp: 100
```

<iframe id="kaltura_player" src="https://cdnapisec.kaltura.com/p/1660902/sp/166090200/embedIframeJs/uiconf_id/25916071/partner_id/1660902?iframeembed=true&playerId=kaltura_player&entry_id=0_s9a4jr3s&flashvars[streamerType]=auto&amp;flashvars[localizationCode]=en&amp;flashvars[leadWithHTML5]=true&amp;flashvars[sideBarContainer.plugin]=true&amp;flashvars[sideBarContainer.position]=left&amp;flashvars[sideBarContainer.clickToClose]=true&amp;flashvars[chapters.plugin]=true&amp;flashvars[chapters.layout]=vertical&amp;flashvars[chapters.thumbnailRotator]=false&amp;flashvars[streamSelector.plugin]=true&amp;flashvars[EmbedPlayer.SpinnerTarget]=videoHolder&amp;flashvars[dualScreen.plugin]=true&amp;&wid=1_quj4d5r0" width="649" height="401" allowfullscreen webkitallowfullscreen mozAllowFullScreen allow="autoplay *; fullscreen *; encrypted-media *" frameborder="0" title="Kaltura Player"></iframe>

`@instructions`
In this module, you learn how to:

- Conduct a hypothesis test for a regression coefficient using either a rejection/acceptance procedure or a p-value
- Calculate and interpret a confidence interval for a regression coefficient
- Calculate and interpret a prediction interval at a specific value of a predictor variable

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

## Statistical inference and Wisconsin lottery

```yaml
type: NormalExercise
key: 7f691f8ace
xp: 100
```

In a previous exercise, you developed a regression line to fit the variable `medhome`, the median house price for each zip code, as a predictor of lottery sales. The regression of `medhome` on `sales` has been summarized in the `R` object `model_blr`.
  
This exercise will give you practice in the standard inferential tasks: hypothesis testing, confidence intervals, and prediction.

`@instructions`
- Summarize the regression model and identify the $t$-statistic for testing the importance of the regression coefficient associated with `medhome`.
- Use the function [confint()](https://www.rdocumentation.org/packages/stats/versions/3.5.0/topics/confint) to provide a 95\% confidence interval for the regression coefficient associated with `medhome`.
- Consider a zip code with a median housing price equal to 50 (in thousands of dollars). Use the function [predict()](https://www.rdocumentation.org/packages/stats/versions/3.5.0/topics/predict) to provide a point prediction and a 95\% prediction interval for sales.

`@hint`
No hints for now. No hints for now. No hints for now.

`@pre_exercise_code`
```{r}
Lot <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/a792b30fb32b0896dd6894501cbab32b5d48df51/Wisc_lottery.csv", header = TRUE)
```

`@sample_code`
```{r}
model_blr1 <- lm(sales ~ medhome, data = Lot)
summary(model_blr1)
#Rcmdr::Confint(model_blr1, level = .95)
confint(model_blr1, level = .95)
NewData1 <- data.frame(medhome = 50)
predict(model_blr1, NewData1, interval = "prediction", level = .95)
```

`@solution`
```{r}
model_blr1 <- lm(sales ~ medhome, data = Lot)
summary(model_blr1)
#Rcmdr::Confint(model_blr1, level = .95)
confint(model_blr1, level = .95)
NewData1 <- data.frame(medhome = 50)
predict(model_blr1, NewData1, interval = "prediction", level = .95)
```

`@sct`
```{r}
ex() %>% check_object("model_blr1") %>% check_equal()
ex() %>% check_function("lm") %>% {
  check_arg(., "formula") %>% check_equal()
  check_arg(., "data") %>% check_equal()
}
ex() %>% check_function("summary") %>% check_result() %>% check_equal()
ex() %>% check_function("confint") %>% {
  check_arg(., "object") %>% check_equal()
  check_arg(., "level") %>% check_equal()
}
ex() %>% check_object("NewData1") %>% check_equal(eq_condition="identical")
ex() %>% check_function("predict") %>% {
  check_arg(., "object") %>% check_equal()
  check_arg(., "newdata") %>% check_equal()
  check_arg(., "interval") %>% check_equal()
  check_arg(., "level") %>% check_equal()
}
ex() %>% check_function("predict") %>% check_result() %>% check_equal()
success_msg("Congratulations! Much of what we learn from a data modeling exercise can be summarized using standard inferential tools: hypothesis testing, confidence intervals, and prediction.")
```

---

## Diagnostics

```yaml
type: NormalExercise
key: 3ae81a2fb8
xp: 100
```

<iframe id="kaltura_player" src="https://cdnapisec.kaltura.com/p/1660902/sp/166090200/embedIframeJs/uiconf_id/25916071/partner_id/1660902?iframeembed=true&playerId=kaltura_player&entry_id=0_c6l4p07j&flashvars[streamerType]=auto&amp;flashvars[localizationCode]=en&amp;flashvars[leadWithHTML5]=true&amp;flashvars[sideBarContainer.plugin]=true&amp;flashvars[sideBarContainer.position]=left&amp;flashvars[sideBarContainer.clickToClose]=true&amp;flashvars[chapters.plugin]=true&amp;flashvars[chapters.layout]=vertical&amp;flashvars[chapters.thumbnailRotator]=false&amp;flashvars[streamSelector.plugin]=true&amp;flashvars[EmbedPlayer.SpinnerTarget]=videoHolder&amp;flashvars[dualScreen.plugin]=true&amp;&wid=1_4rvf8rp1" width="649" height="401" allowfullscreen webkitallowfullscreen mozAllowFullScreen allow="autoplay *; fullscreen *; encrypted-media *" frameborder="0" title="Kaltura Player"></iframe>

`@instructions`
In this module, you learn how to:

- Describe how diagnostic checking and residual analysis are used in a statistical analysis
- Describe several model misspecifications commonly encountered in a regression analysis

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

## Assessing outliers in lottery sales

```yaml
type: NormalExercise
key: 496a3a7c29
xp: 100
```

In an earlier video, we made a scatter plot of population versus sales. This plot exhibits an outlier; the point in the upper left-hand side of the plot represents a zip code that includes Kenosha, Wisconsin. Sales for this zip code are unusually high given its population.  

This exercise summarizes the regression fit both with and without this zip code in order to see how robust our results are to the inclusion of this unusual observation.

`@instructions`
- A basic linear regression fit of population on sales has already been fit in the object `model_blr`. Fit this same model to the data, omiting Kenosha (observation number 9).
- Plot these two least squares fitted lines superimposed on the full data set.
-  What is the effect on the distribution  of residuals by removing this point? Calculate a qq plot with and without Kenosha.

`@hint`
No hints for now. No hints for now. No hints for now.

`@pre_exercise_code`
```{r}
Lot <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/a792b30fb32b0896dd6894501cbab32b5d48df51/Wisc_lottery.csv", header = TRUE)
```

`@sample_code`
```{r}
model_blr <-lm(sales ~ pop, data = Lot)
summary(model_blr)
model_Kenosha <- lm(sales ~ pop, data = Lot, subset = -c(9))
summary(model_Kenosha)

plot(Lot$pop, Lot$sales, xlab = "population", ylab = "sales")
text(5000, 24000, "Kenosha")
abline(reg=model_blr, col="blue")
abline(reg=model_Kenosha, col="red")

par(mfrow = c(1, 2))
qqnorm(residuals(model_blr), main = "")
qqline(residuals(model_blr))
qqnorm(residuals(model_Kenosha), main = "")
qqline(residuals(model_Kenosha))
```

`@solution`
```{r}
model_blr <-lm(sales ~ pop, data = Lot)
summary(model_blr)
model_Kenosha <- lm(sales ~ pop, data = Lot, subset = -c(9))
summary(model_Kenosha)

plot(Lot$pop, Lot$sales, xlab = "population", ylab = "sales")
text(5000, 24000, "Kenosha")
abline(reg=model_blr, col="blue")
abline(reg=model_Kenosha, col="red")

par(mfrow = c(1, 2))
qqnorm(residuals(model_blr), main = "")
qqline(residuals(model_blr))
qqnorm(residuals(model_Kenosha), main = "")
qqline(residuals(model_Kenosha))
```

`@sct`
```{r}
ex() %>% check_object("model_blr") %>% check_equal()
ex() %>% check_function("summary",index=1) %>% check_result() %>% check_equal()
ex() %>% check_object("model_Kenosha") %>% check_equal()
ex() %>% check_function("summary",index=2) %>% check_arg("object") %>% check_equal()
ex() %>% check_function("plot") %>% {
  check_arg(., "x") %>% check_equal()
  check_arg(., "y") %>% check_equal()
  check_arg(., "xlab")
  check_arg(., "ylab")
}
ex() %>% check_function("text")
ex() %>% check_function("abline",index=1) %>% check_arg(., "reg") %>% check_equal()
ex() %>% check_function("abline",index=2) %>% check_arg(., "reg") %>% check_equal()
ex() %>% check_function("par") %>% check_arg(., "mfrow") %>% check_equal()
ex() %>% check_function("qqnorm",index=1) %>% check_arg(., "y") %>% check_equal()
ex() %>% check_function("qqline",index=1) %>% check_arg(., "y") %>% check_equal()
ex() %>% check_function("qqnorm",index=2) %>% check_arg(., "y") %>% check_equal()
ex() %>% check_function("qqline",index=2) %>% check_arg(., "y") %>% check_equal()
success_msg("Excellent! Just because an observation is unusual does not make it bad or noninformative. Kenosha is close to the Illinois border; residents from Illinois probably participate in the Wisconsin lottery thus effectively increasing the potential pool of sales in Kenosha. Although unusual, there is interesting information to be learned from this observation.")
```
