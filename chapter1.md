---
title: 'Chapter 1. Regression and the Normal Distribution'
description: 'Regression analysis is a statistical method that is widely used in many fields of study, with actuarial science being no exception. This chapter introduces the role of the normal distribution in regression and the use of logarithmic transformations in specifying regression relationships.'
free_preview: true
---

## Course Introduction

```yaml
type: NormalExercise
key: 0f2abcda87
xp: 100
```

<iframe id="kaltura_player" src="https://cdnapisec.kaltura.com/p/1660902/sp/166090200/embedIframeJs/uiconf_id/25916071/partner_id/1660902?iframeembed=true&playerId=kaltura_player&entry_id=0_bj34urmj&flashvars[streamerType]=auto&amp;flashvars[localizationCode]=en&amp;flashvars[leadWithHTML5]=true&amp;flashvars[sideBarContainer.plugin]=true&amp;flashvars[sideBarContainer.position]=left&amp;flashvars[sideBarContainer.clickToClose]=true&amp;flashvars[chapters.plugin]=true&amp;flashvars[chapters.layout]=vertical&amp;flashvars[chapters.thumbnailRotator]=false&amp;flashvars[streamSelector.plugin]=true&amp;flashvars[EmbedPlayer.SpinnerTarget]=videoHolder&amp;flashvars[dualScreen.plugin]=true&amp;&wid=0_tp96kyab" width="649" height="401" allowfullscreen webkitallowfullscreen mozAllowFullScreen allow="autoplay *; fullscreen *; encrypted-media *" frameborder="0" title="Kaltura Player"></iframe>

`@instructions`
In this exercise, you learn how to:

-    Calculate and interpret two basic summary statistics
-    Fit a data set to a normal curve
-    Calculate probabilities under a standard normal curve

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

## Fitting a Normal Distribution

```yaml
type: NormalExercise
key: 48b21f4d40
xp: 100
```

<iframe id="kaltura_player" src="https://cdnapisec.kaltura.com/p/1660902/sp/166090200/embedIframeJs/uiconf_id/25916071/partner_id/1660902?iframeembed=true&playerId=kaltura_player&entry_id=1_e885sczm&flashvars[streamerType]=auto&amp;flashvars[localizationCode]=en&amp;flashvars[leadWithHTML5]=true&amp;flashvars[sideBarContainer.plugin]=true&amp;flashvars[sideBarContainer.position]=left&amp;flashvars[sideBarContainer.clickToClose]=true&amp;flashvars[chapters.plugin]=true&amp;flashvars[chapters.layout]=vertical&amp;flashvars[chapters.thumbnailRotator]=false&amp;flashvars[streamSelector.plugin]=true&amp;flashvars[EmbedPlayer.SpinnerTarget]=videoHolder&amp;flashvars[dualScreen.plugin]=true&amp;&wid=1_tnia11ze" width="649" height="401" allowfullscreen webkitallowfullscreen mozAllowFullScreen allow="autoplay *; fullscreen *; encrypted-media *" frameborder="0" title="Kaltura Player"></iframe>

`@instructions`


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

## Fitting Galton's height data

```yaml
type: NormalExercise
key: 6154f289da
lang: r
xp: 100
skills: 1
```

The Galton data has already been read into a dataset called `heights`. These data include the heights of 928 adult children `child_ht`, together with an index of their parents' height `parent_ht`.  The video explored the distribution of the parents' height; in this assignment, we investigate the distribution of the heights of the adult children.

`@instructions`
-  Define the height of an adult child as a local variable
-  Use the function [mean()](https://www.rdocumentation.org/packages/base/versions/3.5.0/topics/mean/) to calculate the mean and the function [sd()](https://www.rdocumentation.org/packages/base/versions/3.5.0/topics/sd/) to calculate the standard deviation 
-  Use the normal approximation and the function [pnorm()](https://www.rdocumentation.org/packages/stats/versions/3.5.0/topics/Normal/)  determine the probability that a child's height is less than 72 inches

`@hint`
Remember that we can reference a variable, say `var`, from a data set such as `heights`, as `heights$var`.
hint

`@pre_exercise_code`
```{r}
heights <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/c85ede6c205d22049e766bd08956b225c576255b/galton_height.csv", header = TRUE)
```

`@sample_code`
```{r}
#Define the variable
ht_child <- ___

#Calculate the mean height
mchild <- ___
mchild

#Calculate the standard deviation of heights
sdchild <- ___
sdchild

#Determine the probability that a child's height is less than 72 inches
pr=___(72, mean=mchild, sd=sdchild)
pr
```

`@solution`
```{r}
ht_child <- heights$child_ht
mchild <- mean(ht_child)
sdchild <- sd(ht_child)
pr=pnorm(72,mean=mchild, sd=sdchild)
```

`@sct`
```{r}
ex() %>% check_object("ht_child",undefined_msg="Make sure you assign the children's hight to ht_child") %>% check_equal(incorrect_msg="Remember that in order to call a specific column from a dataframe, use the $ operator")
ex() %>% check_object("mchild",undefined_msg="Make sure you assign the average of the children's heights to mchild")  %>% check_equal()
ex() %>% check_object("sdchild",undefined_msg="Make sure you assign the standard deviation of the children's heights to sdchild") %>% check_equal()
ex() %>% check_object("pr", undefine_msg="Make sure that you assign the probability of a child being less tha 72 inches to pr") %>% check_equal(incorrect_msg="Make sure to use mchild and sdchild in your calculation of pr")
success_msg("Excellent! With this procedure, you can now calculate probabilities for any distribution using a normal curve approximation.")
```

---

## Visualizing child's height distribution

```yaml
type: NormalExercise
key: 7dc98dc1ff
xp: 100
```

As in the prior exercise, from the Galton dataset `heights`, the heights of 928 adult children have been used to create a local variable called `ht_child`. We also have basic summary statistics, the mean height `mchild` and the standard deviation of heights in `sdchild`. In this exercise, we explore the fit of the normal curve to this distribution.

`@instructions`
-  To visualize the distribution, use the function [hist()](https://www.rdocumentation.org/packages/graphics/versions/3.5.0/topics/hist/) to calculate the histogram. Use the `freq = FALSE` option to give a histogram with proportions instead of counts.
-  Use the function [seq()](https://www.rdocumentation.org/packages/base/versions/3.5.0/topics/seq) to determine a sequence that can be used for plotting. Then, function [lines()](https://www.rdocumentation.org/packages/graphics/versions/3.5.0/topics/lines/) to superimpose a normal curve on the histogram.
-  Determine the probability that a son's height is greater than 72 inches.

`@hint`
No hints for now. No hints for now. No hints for now.

`@pre_exercise_code`
```{r}
heights <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/c85ede6c205d22049e766bd08956b225c576255b/galton_height.csv", header = TRUE)
ht_child <- heights$child_ht
mchild <- mean(ht_child)
sdchild <- sd(ht_child)
```

`@sample_code`
```{r}
#Visualize the Distribution
___(___, freq = FALSE)

#Determine a sequence. Then, graph a histogram with a normal curve superimposed
x <- seq(60, 80,by = 0.1)
___(x, dnorm(x,mean = mchild, sd = sdchild), col = "blue")

# Determine the probability that a son's height is greater than 72 inches
pr=pnorm__
```

`@solution`
```{r}
hist(ht_child, freq = FALSE)
x <- seq(60, 80,by = 0.1)
lines(x, dnorm(x,mean = mchild, sd = sdchild), col = "blue")
pr=pnorm(72, mean = mchild , sd = sdchild,lower.tail=FALSE)
```

`@sct`
```{r}
ex() %>% check_function("hist",not_called_msg="Use the hist command to create a histogram of the children's heights.") %>% check_arg("x") %>% check_equal(incorrect_msg="Make sure to create a histogram of the children's heights.")
ex() %>% check_function("lines",not_called_msg="Please use the lines function to overlay a normal curve on your histogram")
ex() %>% check_object("pr", undefined_msg="Make sure to assign the probability of a child's height being greater than 72 inches to pr.") %>% check_equal(incorrect_msg="Make sure to find the probability of a child's height being GREATER than 72 inches.")
success_msg("Excellent! Visualizing the distribution, especially with reference to a normal, is important for communicating results of your analysis.")
```

---

## Visualizing distributions

```yaml
type: NormalExercise
key: 6b823fe809
xp: 100
```

<iframe id="kaltura_player" src="https://cdnapisec.kaltura.com/p/1660902/sp/166090200/embedIframeJs/uiconf_id/25916071/partner_id/1660902?iframeembed=true&playerId=kaltura_player&entry_id=1_p7gt98qk&flashvars[streamerType]=auto&amp;flashvars[localizationCode]=en&amp;flashvars[leadWithHTML5]=true&amp;flashvars[sideBarContainer.plugin]=true&amp;flashvars[sideBarContainer.position]=left&amp;flashvars[sideBarContainer.clickToClose]=true&amp;flashvars[chapters.plugin]=true&amp;flashvars[chapters.layout]=vertical&amp;flashvars[chapters.thumbnailRotator]=false&amp;flashvars[streamSelector.plugin]=true&amp;flashvars[EmbedPlayer.SpinnerTarget]=videoHolder&amp;flashvars[dualScreen.plugin]=true&amp;&wid=1_04w1xjbe" width="649" height="401" allowfullscreen webkitallowfullscreen mozAllowFullScreen allow="autoplay *; fullscreen *; encrypted-media *" frameborder="0" title="Kaltura Player"></iframe>

`@instructions`


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

## Visualizing injury claims with density plots

```yaml
type: NormalExercise
key: 2b2ea998af
xp: 100
```

In the prior video, you got somebackground information on the Massachusetts bodily injury dataset. This dataset, `injury`, has been read in and local variables `claims` has been created. This assignment reviews the  [hist()](https://www.rdocumentation.org/packages/graphics/versions/3.5.0/topics/hist/) function for visualizing the distribution and allows you to explore density plotting, a smoothed version of the histogram.

`@instructions`
-  Use the function [log()](https://www.rdocumentation.org/packages/base/versions/3.5.0/topics/log/) to create the logarithmic version of the claims variable
-  Calculate a histogram of logarithmic with 40 bins using an option in the [hist()](https://www.rdocumentation.org/packages/graphics/versions/3.5.0/topics/hist/) function,  `breaks = `.
-  Create a density plot of logarithmic claims using the functions [plot()](https://www.rdocumentation.org/packages/graphics/versions/3.5.0/topics/plot/) and [density()](https://www.rdocumentation.org/packages/stats/versions/3.5.0/topics/density/).
-  Repeat the density plot, this time using a more refined bandwidth equal to 0.03. Use an option in the  [density()](https://www.rdocumentation.org/packages/stats/versions/3.5.0/topics/density/) function, `bw = `.

`@hint`
No hints for now. No hints for now. No hints for now.

`@pre_exercise_code`
```{r}
injury <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/8cca19d0503fcf6e9d30d9cb912de5ba95ecb9c1/MassBI.csv", header = TRUE)
claims <- injury$claims
```

`@sample_code`
```{r}
#Create the logarithmic claims variable
logclaims <- ___

#Create a histogram usins 40 bins
___(logclaims, breaks = 40,freq = FALSE)
box()

# Create a density plot of logarithmic claims
plot(___(logclaims))

# Create a density plot of logarithmic claims with a binwidth of 0.03
___
```

`@solution`
```{r}
logclaims <- log(claims)
hist(logclaims , breaks = 40,freq = FALSE)
box()
plot(density(logclaims))
plot(density(logclaims, bw = 0.03))
```

`@sct`
```{r}
ex() %>% check_object("logclaims") %>% check_equal(incorrect_msg = "You made an error in the definition of the logarithmic claims. Check out the definition of the log() function.")
ex() %>% check_function("hist",not_called_msg="Make sure to use `hist` to create a histogram.") %>% {
  check_arg(., "x") %>% check_equal(incorrect_msg="Please create a histogram of logclaims.")
  check_arg(., "freq") %>% check_equal(incorrect_msg="Please create a density histogram instead of a frequency histogram.")
}
ex() %>% check_function("plot",index=1) %>% check_arg("x") %>% check_equal(incorrect_msg="Use the density function to plot the density of logclaims.")
ex() %>% check_function("plot",index=2,not_called_msg="Create another plot using `plot` that displays the density of logarithmic claims with a binwidtch of 0.03.") %>% check_arg("x") %>% check_equal()
success_msg("Excellent! Visualizing the distribution is important and smoothing techniques allow viewers to see important patterns without being distracted by random fluctations.")
```

---

## Summarizing Distributions

```yaml
type: NormalExercise
key: ef0f4110ea
xp: 100
```

<iframe id="kaltura_player" src="https://cdnapisec.kaltura.com/p/1660902/sp/166090200/embedIframeJs/uiconf_id/25916071/partner_id/1660902?iframeembed=true&playerId=kaltura_player&entry_id=1_4yk0k2m4&flashvars[streamerType]=auto&amp;flashvars[localizationCode]=en&amp;flashvars[leadWithHTML5]=true&amp;flashvars[sideBarContainer.plugin]=true&amp;flashvars[sideBarContainer.position]=left&amp;flashvars[sideBarContainer.clickToClose]=true&amp;flashvars[chapters.plugin]=true&amp;flashvars[chapters.layout]=vertical&amp;flashvars[chapters.thumbnailRotator]=false&amp;flashvars[streamSelector.plugin]=true&amp;flashvars[EmbedPlayer.SpinnerTarget]=videoHolder&amp;flashvars[dualScreen.plugin]=true&amp;&wid=1_n2n42jwy" width="649" height="401" allowfullscreen webkitallowfullscreen mozAllowFullScreen allow="autoplay *; fullscreen *; encrypted-media *" frameborder="0" title="Kaltura Player"></iframe>

`@instructions`
In this section, you learn how to:

-    Calculate and interpret basic summary statistics
-    Calculate and interpret distributions using boxplots
-    Calculate and interpret distributions using qq plots

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

## Summarizing injury claims with box and qq plots

```yaml
type: NormalExercise
key: 70a5a3a60b
xp: 100
```

The Massachusetts bodily injury data has already been read and used to create the local variable `claims` representing bodily injury claims. The previous video showed how to present the distribution of logarithmic claims which appeared to be approximately normally distributed. However, users are not really interested in log dollars but want to know about a unit of measurement that is more intuitive, such as dollars. 

So this assignment is based on claims, not the logarithmic version. You will review the functions  [boxplot()](https://www.rdocumentation.org/packages/graphics/versions/3.5.0/topics/boxplot/) and [qqnorm()](https://www.rdocumentation.org/packages/stats/versions/3.5.0/topics/qqnorm/) for visualizing the distribution through boxplots and quantile-quantile, or qq-, plots. But, because we are working with such a skewed distribution, do not be surprised that it is difficult to interpret results readily.

`@instructions`
-  Produce a box plot for claims
-  Determine the 25th empirical percentile for claims using the  [quantile()](https://www.rdocumentation.org/packages/stats/versions/3.5.0/topics/quantile/) function.
-  Determine the 25th percentile for claims based on a normal distribution using the  [qnorm()](https://www.rdocumentation.org/packages/stats/versions/3.5.0/topics/Normal/) function.
-  Produce a qq plot for claims. The [qqline()](https://www.rdocumentation.org/packages/stats/versions/3.5.0/topics/qqnorm/) function is handy for producing a reference line.

`@hint`
No hints for now. No hints for now. No hints for now.

`@pre_exercise_code`
```{r}
injury <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/8cca19d0503fcf6e9d30d9cb912de5ba95ecb9c1/MassBI.csv", header = TRUE)
claims <- injury$claims
```

`@sample_code`
```{r}
#Produce a box plot for claims
___(claims)

#Determine the 25th empirical percentile for claims
___(claims, probs = ___)

#Determine the 25th percentile for claims based on a normal distribution
pct.25=___(p = ___, mean = mean(claims), sd = sd(claims))
pct.25

#Produce a qq plot for claims
___(claims)
___(claims)
```

`@solution`
```{r}
boxplot(claims)
quantile(claims, probs = 0.25)
pct.25=qnorm(p = 0.25, mean = mean(claims), sd = sd(claims))
qqnorm(claims)
qqline(claims)
```

`@sct`
```{r}
ex() %>% check_function("boxplot") %>% check_arg("x") %>% check_equal(incorrect_msg="Please create a boxplot of `claims`.")
ex() %>% check_function("quantile") %>% check_arg("probs") %>% check_equal(incorrect_msg="If we want to find the Yth percentile, make sure to set probs equal to Y in decimal format.")
ex() %>% check_object("pct.25",undefined_msg="Make sure to assign the normal value associated with the 25th percentile to pct.25") %>% check_equal(incorrect_msg="J.D.E.M.")
ex() %>% check_function("qqnorm") %>% check_arg("y") %>% check_equal(incorrect_msg="Make sure that you are creating a qq-plot for `claims`.")
ex() %>% check_function("qqline") %>% check_arg("y") %>% check_equal(incorrect_msg="Make sure that you are adding a qq-line for `claims`.")
success_msg("Congratulations on learning about box and qq plots. Although you are unlikely to show these plots to consumers of your analysis, you will find them useful tools as we explore multivariate aspects of data.")
```

---

## Effects on distributions of removing the largest claim

```yaml
type: NormalExercise
key: 127a1f92ec
xp: 100
```

The Massachusetts bodily injury dataset `injury` has been read in; our focus is on the `claims` variable in that dataset. 

In the previous exercise, we learned that the Massachusetts bodily injury `claims` distribution was not even close to approximately normal (as evidence by the box and qq- plots). Non-normality may be induced by skewness (that we will handle via transformations in the next section). But, seeming non-normality can also be induced by one or two very large observations (that we will defined as an *outlier* later in the course). So, this exercise examines the effects on the distribution of removing the largest claims.

`@instructions`
-  Use the function [tail()](https://www.rdocumentation.org/packages/utils/versions/3.5.0/topics/head) to examine the `injury` dataset and identify the largest claim
-  Use the function  [subset()](https://www.rdocumentation.org/packages/base/versions/3.5.0/topics/subset) to create a subset omitting the largest claim
-  Compare the summary statistics of the omitted claim distribution to the full distribution
-  Compare the two distributions visually via histograms plotted next to another. `par(mfrow = c(1, 2))` is used to organize the plots you create. Do not alter this code.

`@hint`
No hints for now. No hints for now. No hints for now.

`@pre_exercise_code`
```{r}
injury <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/8cca19d0503fcf6e9d30d9cb912de5ba95ecb9c1/MassBI.csv", header = TRUE)
claims <- injury$claims
```

`@sample_code`
```{r}
#View the last 6 entries in injury
___(injury)

#create a subset that excludes the largest claim
injury2 <- ___(injury, ___ < ___ )

#get summary stats on this new subset
___(injury2)
sd(injury2$claim)

#plot both graphs against eachother
par(mfrow = c(1, 2))
hist(claims, freq = FALSE,  main = "Full Data")
hist(injury2$claims, freq = FALSE,  main = "Largest Claim Omitted")
```

`@solution`
```{r}
tail(injury)
injury2 <- subset(injury, claims < 25000 )
summary(injury2)
sd(injury2$claim)
par(mfrow = c(1, 2))
hist(claims, freq = FALSE,  main = "Full Data")
hist(injury2$claims, freq = FALSE,  main = "Largest Claim Omitted")
```

`@sct`
```{r}
ex() %>% check_function("tail") %>% check_arg("x") %>% check_equal(incorrect_msg="Make sure to use tail to see the las 6 entries in `injury`.")
ex() %>% check_object("injury2") %>% check_equal(incorrect_msg="Make sure that `injury2` is the same as `injury` but without the largest claim. Try and think of creative ways to remove that observation from the data!")
ex() %>% check_function("subset")
ex() %>% check_function("summary") %>% check_arg("object") %>% check_equal(incorrect_msg="Make sure to get summary statistics of `injury2`.")
ex() %>% check_function("sd") %>% check_result() %>% check_equal(incorrect_msg="Make sure that you have found the standard deviation of `claims` in the dataframe `injury2`.")
ex() %>% check_function("par") %>% check_arg("mfrow") %>% check_equal(incorrect_msg="Please dont change this part. it should read `par(mfrow=c(2,1))`")
ex() %>% check_function("hist",index=1) %>% {
  check_arg("x") %>% check_equal(incorrect_msg="Create the first histogram using all of the observed claims.")
  check_arg("freq") %>% check_equal(incorrect_msg="Make sure to create a density histogram instead of a frequency histogram.")
}
ex() %>% check_function("hist",index=2) %>% {
  check_arg("x") %>% check_equal(incorrect_msg="Make sure to create the second histogram based on claims with the largest one removed.")
  check_arg("freq") %>% check_equal(incorrect_msg="Make sure to create a density histogram instead of a frequency histogram.")
}
success_msg("Congratulations! The goal of predictive modeling is to discover patterns in the data. However, sometimes seeming 'patterns' are the result of one or two unusual observations. Unusual observations may be due to incorrect data gathering procedures or just due to wild fluctuations in a process of interest but are common in predictive modeling.")
```

---

## Transformations

```yaml
type: NormalExercise
key: 219d7f5214
xp: 100
```

<iframe id="kaltura_player" src="https://cdnapisec.kaltura.com/p/1660902/sp/166090200/embedIframeJs/uiconf_id/25916071/partner_id/1660902?iframeembed=true&playerId=kaltura_player&entry_id=1_5yfuwrbc&flashvars[streamerType]=auto&amp;flashvars[localizationCode]=en&amp;flashvars[leadWithHTML5]=true&amp;flashvars[sideBarContainer.plugin]=true&amp;flashvars[sideBarContainer.position]=left&amp;flashvars[sideBarContainer.clickToClose]=true&amp;flashvars[chapters.plugin]=true&amp;flashvars[chapters.layout]=vertical&amp;flashvars[chapters.thumbnailRotator]=false&amp;flashvars[streamSelector.plugin]=true&amp;flashvars[EmbedPlayer.SpinnerTarget]=videoHolder&amp;flashvars[dualScreen.plugin]=true&amp;&wid=1_6h3ur8eu" width="649" height="401" allowfullscreen webkitallowfullscreen mozAllowFullScreen allow="autoplay *; fullscreen *; encrypted-media *" frameborder="0" title="Kaltura Player"></iframe>

`@instructions`
In this exercise, you learn how to:

-    Symmetrize a skewed distribution using a logarithmic transformation

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

## Distribution of transformed bodily injury claims

```yaml
type: NormalExercise
key: 2127303de1
xp: 100
```

We have now examined the distributions of bodily injury claims and its logarithmic version. Grudgingly, we have concluded that to fit a normal curve the logarithmic version of claims is a better choice (again, we really do not like log dollars but you'll get used to it in this course). But, why logarithmic and not some other transformations?

A partial response to this question will appear in later chapters when we describe interpretation of regression coefficients. Another partial response is that the log transform seems to work well with skewed insurance data sets, as we demonstrate visually in this exercise.

`@instructions`
Use the code `par(mfrow = c(2, 2))` so that four graphs appear in a 2 by 2 matrix format for easy comparisons. Plot the density of

-  claims
-  square root of claims
-  logarithmic claims
-  negative reciprocal of claims

`@hint`
You can use basic `R` calculation functions to transform data. For example, `-claims^{-1}` can be used to code the negative reciprocal of claims.

`@pre_exercise_code`
```{r}
injury <- read.csv("https://assets.datacamp.com/production/repositories/2610/datasets/8cca19d0503fcf6e9d30d9cb912de5ba95ecb9c1/MassBI.csv", header = TRUE)
claims <- injury$claims
```

`@sample_code`
```{r}
#This code helps to organize the four graphs into a 2 by 2 format
par(mfrow = c(2, 2))
#Plot the density of claims
plot(density(___))
#Plot the density of square root of claims
plot(density(___)) 
#Plot the density of logarithmic claims
plot(density(___))
#Plot the density of the negative reciprocal of claims
plot(density(___))
```

`@solution`
```{r}
par(mfrow = c(2, 2))
plot(density(claims))    
plot(density(claims^(0.5)))  
plot(density(log(claims)))  
plot(density(-claims^(-1)))
```

`@sct`
```{r}
ex() %>% check_function("par") %>% check_arg("mfrow") %>% check_equal("Please do not change this part of the code. It should read `par(mfrow=c(2,2))`")
ex() %>% check_function("plot",index=1,not_called_msg="Did you plot the density of claims?") %>% check_arg("x") %>% check_equal(incorrect_msg="Make sure to create the first histogram using `claims`")
ex() %>% check_or(
check_function(.,"plot") %>% check_arg(., "x") %>% check_equal(),
  override_solution(.,"plot(density(sqrt(claims)))") %>% check_function("plot") %>% check_arg(., "x") %>% check_equal()
)
ex() %>% check_function("plot",index=3,not_called_msg="Did you plot the density of logarithmic claims?") %>% check_arg("x") %>% check_equal(incorrect_msg="Make sure to create the third histogram based on the natural log of `claims`.")
ex() %>% check_function("plot",index=4,not_called_msg="Did you plot the density of the negative reciprocal of claims?") %>% check_arg("x") %>% check_equal(incorrect_msg="Make sure to create the fourth histogram based on the negative reciprocal () of `claims`.")
success_msg("Excellent! Transformations of data is a tool that incredibly expands potential applicability of (linear) regression techniques.")
```
