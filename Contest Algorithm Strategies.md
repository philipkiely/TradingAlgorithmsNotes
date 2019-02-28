Twitter and StockTwits Sentiment Analysis
-----------------------------------------

Social Media Mean Reversal Paper

Abstract

* Linear regression analysis shows that extreme sentiment corresponds to higher demand and lower supply of liquidity, with negative sentiment having a much larger effect on demand and supply than positive sentiment.
* After extreme sentiment, prices become more mean-reverting and spreads narrow.
* Keep transaction costs to a minimum
* **Profit by using extreme bullish and bearish emotions in social media as a real-time barometer for the end of momentum and a return to mean reversion.**

Paper

Historically, major intraday and interday crashes have occured when traders relying on mean-reversion had overstayed their liquidity, and panic-sell, driving prices in more extreme directions. Now, those same traders express the strong sentiments associated with these cycles on social media, indicating in near-real-time that the cycle is occuring.

The strategy trades at 30-minute intervals. What is the mean daily turnover? (Specifically, is it between 5 and 65 percent?) Does the strategy hold overnight?

Further reading in survey Dredze et al. (2016), or Mark Dredze, Prabhanjan Kambadur, Gary Kazantsev, Gideon Mann, and Miles Os- borne. 2016\. “How twitter is changing the nature of financial news discovery.” In Proceedings of the Second International Workshop on Data Science for Macro-Modeling. ACM.

Predictive regression from before market open is statistically significant but at a lower r-squared than descriptive regression on the daily aggregates. Doesn’t consider data from evening before trading day.

Platform

Quantopian data set live until 2 months prior. Unsure if it is availible real-time in the contest. Same dataset as used in the paper. Seems complex to access.

Pros:

* Innovative but well-tested strategy, correct implementation should have high returns
* balances multiple strategy types (momentum, mean reversion)
* A good implentation will do extremely well in the contest (already stated, but important)

Cons:

* Difficult to implement (unless supplied with code & permission)
* Potential issues with turnover and inter-day holding
* potential data access issues, though the price says free.
* Not exactly a stock-ranking strategy

CanImplementInWeek = True if Starter Code, else False

USD/EUR Exchange Rate Analysis
------------------------------

Rank stocks by performance under different exchange-rate conditions. Companies moving use-\>britain are positively affected by dollar-\>pound rate, reverse for reverse; reverse for reverse.

`GrossDollarExposure = assets + suppliers + customers + operations`

`GrossPoundExposure = assets + suppliers + customers + operations`

`RelativeDollarValue = standard deviations from mean of dollar-pound spread, and direction?`

`NetDollarExposure = GrossDollarExposure - GrossPoundExposure`

`Rank = normalize(sort(NetDollarExposure * RelativeDollarValue))`

Pros:

* Isolated from style & sector variation
* simple data and model
* Easy to combine with other, more common factors

Cons:

* predictive factor probably not strong
* Not a particularly robust model
* May be difficult to determine each company’s exposure to dollar and pound.

CanImplementInWeek = True if exposure formula, else False

Fundamentals Analysis
---------------------

Ranks stocks by fundamental characteristics regarding return on invested capital. Companies with high multi-year dividend yield and good earnings with high margins perform better.

`StockQuality = 5YearDivYield + 1/PE + scale(profit_margin)`

​

Additional Fundamental: RORC (R&D)

<https://www.investopedia.com/articles/fundamental-analysis/10/research-development-rorc.asp>

Pros:

* Easily adjustable time-tested fundamental factors
* Easy to understand and implement
* Easy to combine with other strategies

Cons:

* May implicitly select heavily in certain sectors by shared characteristics
* Most metrics updated quarterly or less often, may rebalance infrequently
* May favor more static, established corporations that have recently underperformed

CanImplementInWeek = True

General Strategies
------------------

* Any asset pricing model can easily be turned into a ranking method.
* Clone and tweak public algorithms to see if there is alpha left in the strategy
* Use price based technical indicators like momentum and volatility
* Fundamental factors (value based)

A ranking strategy can be inverted between reversion and momentum bets

Crossover Strategy
------------------

As part of another algorithm, use crossover as a boolean passed into the state machine that does the ranking to determine whether it is ranking for momentum or mean-reversion. Major potential flaw: crossover requires long-span historical data, so may not be predictive or fast enough.

Crossover could be its own strategy, but it is unlikely to serve as a properly balanced ranking algorithm, although ranking stocks by their individual crossover percents as a ratio to market crossover would be easy to implement.

CanImplementInWeek = True