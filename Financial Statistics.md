Portfolio Return: sigma(weight[i]\*return[i])

Portfolio Variance: sigma(weight[i]\*variance[i]) + sigma(sigma(weight[i]\*weight[j]\*variance[i]\*variance[j]\*correlation[i,j]))

Correlation[i, j] = covariance/(sdeviation[i]\*sdeviation[j])

A covariance matrix holds the variance of each asset along the diagonal and the covariance of each pair of assets in the i, j row/column with i != j, only the upper or lower triangle is needed, as the values are repeated.

Number of columns of the matrix on the left must be equal to the number of rows of the matrix on the right.

(m x n) \* (n x p) = (m x p) as n = n

An arithmatic mean is the standard mean, but a weighted arithmatic mean is the same method, but applying weights to each component in the sum.

A median is the central number in a series, and is less affected by extreme values

A mode is the most frequently occuring value. The built in mode function in python returns 1 value no matter what, even if each data point is unique or there are two or more elements with a tied count.

The Geometric mean of n numbers is the nth root of the cumulative multiple of the n numbers. It is always less than or equal to the arithmatic mean. Because of the root, there can be issues with negative numbers in this mean. Geometric mean shows the average daily change in multiplicitave return, that is, show compound interest.

The harmonic mean is equal to n divided by the sum of reciprocals of the data. Harmonic means are used for calculating things expressed by ratios, such as dollar-cost averaged purchases. The inverse of the harmonic mean is equal to the arithmatic mean of the inverses of the numbers.

Mean absolute deviation is the sum of abs(deviations) from the mean divided by the length of the series.

Variance is calculated as the sum of squared difference between each point divided by the length of the series. It is sign-agnostic as Mean Absolute Deviation, but is differentiable as a square not an absolute value.

Standard Deviation is the square root of the variance, which re-adjusts for the Variance’s weight towards extreme values.

SemiVariance works the same, but with the sum of values less than mean, divided by that number of values, alternately greater than.

SemiDeviation is the square root of SemiVariance

A moment is a specific quantitative measure of the shape or nature of a function or distribution. Mean, deviation, skewedness, and kurtosis are all moments.

A probability distribution without skew is like the normal distribution. Skewedness is a measure away from symmetry. For unimodal distributions, mean \> median \> mode is positive skew, mean \< median \< mode is negative skew.

Kurtosis is a measurement of how peaked a distribution is. Excess kurtosis is the amount of kurtosis in a distribution greater than the normal distribution. A leaptokurtic distribution has a positive kurtosis, sharper and higher peak, and fatter tails. A mesokurtic distribution is a normal distribution, and a platykurtic distribution has a negative kurtosis, a rounder and lower peak, and thinner tails.

A Jarque-Bera (zhark-beara) test tells how similar a distribution is to a normal distribution. The null hypothesis is that the distribution is normal. If p \< .05, data not normal, else data is normal. Check that the library has the same Ho and Ha.

The correlation coefficient of two series is the covariance of the two series divided by the product of those series standard deviation. You can correlate any two variables and will get a number between -1, perfect inverse correlation, and 1, perfect correlation. Covariance is a measure of how much of the variance in one variable is explained by the variance of the other variable. By itself, covariance isn’t very meaningful. Correlation normalizes covariance into the -1 to 1 range so that it is actually useful.

Correlation always returns a NxN matrix for comparing N variables. The diagonal, the correlation between a variable and itself, is always 1s.

Correlation assumes normality and a linear model.

Spearman Rank Correlation is a type of correlation analysis that defends against outliers, scale, and other issues by adding the concept of rank. Ranked data is a datapoint’s position in a sorted list, with ties averaged by mean. In data such as a poisson distribution, Spearman Rank will consistently return higher, more accurate correlation values, but will be equally likely to report a lack of correlation when that correlation does not exist.

Neither method will show a correlation with time lag (and a time-lag correlation is gold to discover in modeling). However, time-lag calculations give more comparisons, which can lead to a reported correlation by chance rather than by actual correlation, so to do this analysis requires a corrective measure, usually the conservative Bonferroni correction.

A parameter is anything a model uses to constrain its predictions, so any form of used input is a parameter. For example, the mean is a parameter. Each parameter is limited by its construction: its time period, measurement frequency, error, and other factors of computation. The error may not even be a normal distribution. Make sure to do a normality test.

When taking an estimate, take that estimate at multiple points in time and consider the variance of the estimate.

The sharpe ratio is one of the best single measures of algorithm performance, but it is still an estimate with variance and different methods of calculation. The ratio indicates the average return minus risk-free return over standard deviation for an investment, higher is better. A Sharpe ratio on a rolling basis will often change greatly.

Linear regression predicts a dependent variable by an independent variable. Y = a + bX. To calculate this in Python requires an explicit term for calculating a. The F-statistic is the probability that the linear regression is predictive, lower is better, general cutoff at \>=.05\.

Use ordinary least squares regression to find the hyperplane of best fit in multiple regression.

Use stepwise regression to determine which variables to include in the model. If it is sufficiently important and the time and resources are availible, test all combinations.

The validity of these statistics depends on whether or not the assumptions of the linear regression model are satisfied. These are:

* The independent variable is not random.
* The variance of the error term is constant across observations. This is important for evaluating the goodness of the fit.
* The errors are not autocorrelated. The Durbin-Watson statistic reported by the regression detects this. If it is close to 2, there is no autocorrelation.
* The errors are normally distributed. If this does not hold, we cannot use some of the statistics, such as the F-test.

Multiple linear regression also requires an additional assumption:

* There is no exact linear relationship between the independent variables. Otherwise, it is impossible to solve for the coefficients βi uniquely, since the same linear equation can be expressed in multiple ways.

Overfitting is arguably the biggest problem in quantitative finance. It is always there, and can only be minimized, not eliminated. An overfitted model is overly reliant on the noise and idiosyncrocities of the sample data set, rather than the actual predictive signal. It is better to have a simple model that understands some things than a complex model that purports to know a lot. Embrace the noise, reject perfect fit.

* Have an out-of-sample test as a final check for the algorithm, but don’t overfit on the out-of-sample data, only use it once.
* Use cross-validation to split up data set.
* Use an information criterion test on additional parameters to see how many parameters should be in a model.

Hypothesis testing is fundamental to statistically rigorous strategy construction.

The null hypothesis is the generally accepted idea, or at least the opposite of the assertion. The alternate hypothesis is the claim under examination.

1. State the hypothesis and the alternative to the hypothesis
2. Identify the appropriate test statistic and its distribution. Ensure that any assumptions about the data are met (stationarity, normality, etc.)
3. Specify the significance level, α
4. From α and the distribution compute the 'critical value'.
5. Collect the data and calculate the test statistic
6. Compare test statistic with critical value and decide whether to accept or reject the hypothesis.

Hypothesis tests may be 1 or 2 sided depending on the hypothesis, which refers to if it considers one or both tails as part of the alternate hypothesis.

True Situation**Decision**H0 TrueH0 FalseDo not reject H0Correct DecisionType II ErrorReject H0 (accept HA)Type I ErrorCorrect Decision

A z-distribution, or standard normal distribution, is an essential probability distribution in finance. However, in cases where the assumption of normality is in question, algorithms should use a t-distribution, which has fatter tails and a lower peak.