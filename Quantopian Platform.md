Links:

* <https://www.quantopian.com/data?type=free>
* <https://www.quantopian.com/contest/resources>

Use the research platform and notebooks to generate and test ideas.

The Pipeline API accepts a set of calculations across multiple data inputs to analyze a large number of assets. The Pipeline object allows a program to use the Pipeline and is availible through the quantopian.pipeline library.

Use ‘|’ as the union operater and ‘&’ as the intersection operator.

The Algorithm API offers the functionality for implementing an Trading Algorithm.

* `initialize` is called once at algorithm start. It requires `context` as an input. All one-time logic and initialization should go in `initialize`.
* `​context`​ is an augmented Python dictionary that maintains state throughout the entire simulation process. Use `context` as the global namespace. Context variables can be created and updated using dot notation.
* `before_trading_start` runs once before each trading day and requires `context` and `data`. The function uses the output from the pipeline and does any pre-processing to the results before using them for portfolio construction.
* `data` is an object that stores several API functions to look up historical pricing and volume data for any asset.
* `schedule_function``(func, day_rule, time_rule)` schedules the execution of a given function, subject to trading day and time constraints.

Use the `optimize` library to achieve objectives subject to constraints.

Many trading algorithms have the following structure:

1. For each asset in a known (large) set, compute N scalar values for the asset based on a trailing window of data.
2. Select a smaller tradeable set of assets based on the values computed in (1).
3. Calculate desired portfolio weights on the set of assets selected in (2).
4. Place orders to move the algorithm’s current portfolio allocations to the desired weights computed in (3).

There are several technical challenges with doing this robustly. These include:

* efficiently querying large sets of assets
* performing computations on large sets of assets
* handling adjustments (splits and dividends)
* asset delistings

Pipeline exists to solve these challenges by providing a uniform API for expressing computations on a diverse collection of datasets.

A filter is a function that takes an asset and time and returns a boolean. It is usually used for determining sets of assets to include or not include.

A classifier is a function that takes an asset and time and returns a categorical output like an int or string. Classifiers are used for grouping assets.

A factor is a function that takes an asset and time and returns a numerical value. Most measurements are factors.

`from quantopian.pipeline import Pipeline` gives access to the `Pipeline` object. 

A call to `run_pipeline` returns a Pandas Dataframe with the information indexed by date and security.​

Use `​columns={}`​ in the `​Pipeline()`​ constructor to determine what information should be returned. Use `​Pipeline.add()`​ to add factors to an existing pipeline.

Use standard operations to combine multiple factors into a single calculation to input to the pipeline.

Add filters to a pipeline similarly to add a boolean column. Use `​screen`​ in the constructor to limit the assets included in a pipeline by a filter.

Mask specific factors using filters.

Classifiers operate similarly to factors and filters.

Use `​quantopian.pipeline.CustomFactor`​ as the parent class to an extending class to create a custom factor.

`​QTradableStocksUS`​ is a built-in filter that selects a daily universe of stocks. It is used for the Quantopian contest.

Alphalens is a tool for analyzing a given alpha factor’s effectiveness in predicting returns.

quant workflow:

1. Define your trading universe and build an alpha factor using the Pipeline API.
2. Analyze the predictiveness of your alpha factor with Alphalens.
3. Create a trading strategy based on your alpha factor in Quantopian's IDE using the Optimize API.
4. Backtest your trading strategy and analyze the results with Pyfolio.

Alpha factors have an information coefficient (IC). -1\<IC\<1, with anything \>0 considered somewhat predictive, and an IC mean close to .1 probably good.

IC varies over time. To calculate IC for a long period, make sure the pricing information extends sufficiently farther into the future than the pipeline information.

Alphalens allows for grouping by a classifier. Grouping by sector lets the strategy long-short within sectors, isolating it from sector-to-sector varience if that varience is not part of the strategy.

Quantopian says: "Ideally, you want the IC Mean line chart to constantly be above 0\. You also want to see Quantile 1 consistently have the lowest returns, and quantile 5 consistently have the highest returns. Lastly, be sure to take a look at the IC Mean and returns by sector to see if there are any major outliers in terms of performace."

The quantopian contest requires that algorithms meet certain criteria:

* Positive returns.
* Leverage between 0.8x-1.1x.
* Low position concentration.
* Low beta-to-SPY.
* Mid-range turnover.
* Long and short positions.
* Trades liquid stocks.
* Low exposure to sector risk.
* Low exposure to style risk.
* Place orders with Optimize API.

These criteria are checked over a two-year backtest and re-checked every day that the algorithm is active in the contest.

The contest algorithm must place all orders through the `​order_optimal_portfolio`​ function.

The contest algorithm may only trade the QTradableStocksUS universe.

* Use `​screen=QTradableStocksUS`​ as the base pipeline filter
* Make sure stocks to order are in QTU before ordering

The algorithm cannot invest more than 5% of its capital base in any one asset. 

* Include a `​PositionConcentration`​ constraint in calls to order. Set this substantially beneath 5% to reduce risk of going over.

The long and short positions can differ by no more than 10% of the total capital base.

* Include a `​opt.DollarNeutral()`​ constraint in the order function
* Rebalance frequently, at least every two weeks, to stay between 10% in most cases.

The algorithm must have a mean daily turnover between 5 and 65 percent over a 63-day rolling trading period.

* This means an average asset hold time between 3 and 40 days
* Target alpha factors with a 3 to 40 day prediction horizon
* use `​schedule_function`​ to trade at fixed intervals

The algorithm must maintain between .8 and 1.1 times gross leverage at EOD. Invested capital includes long and short positions.

* set max leverage as 1 using `​opt.MaxGrossExposure`​

The algorithm must have an absolute beta to spy below .3

* make sure the alpha factor is not correlated to the market

The algorithm must be less than 20% exposed to each of the 11 sectors:

* Basic Materials
* Technology
* Real Estate
* Energy
* Communication Services
* Health Care
* Financial Services
* Consumer Cyclical
* Industrials
* Consumer Defensive
* Utilities

The algorithm must be less than 40% exposed to each of the 5 style factors:

* Size
* Value
* Momentum
* Short-Term Reversal
* Volatility

To do this, have an original idea with unique alpha factors. Also:

```js
import quantopian.algorithm as algo
import quantopian.optimize as opt

from quantopian.pipeline.experimental import risk_loading_pipeline  

def initialize(context):
    # Attach the risk loading pipeline to our algorithm.
    algo.attach_pipeline(risk_loading_pipeline(), 'risk_loading_pipeline')

def before_trading_start(context, data):
    # Get the risk loading data every day.
    context.risk_loading_pipeline = pipeline_output('risk_loading_pipeline')

def place_orders(context, data):  
    # Constrain our risk exposures. We're using version 0 of the default bounds
    # which constrain our portfolio to 18% exposure to each sector and 36% to
    # each style factor.
    constrain_sector_style_risk = opt.experimental.RiskModelExposure(  
        risk_model_loadings=context.risk_loading_pipeline,  
        version=0,
    )

    # Supply the constraint to order_optimal_portfolio.
    algo.order_optimal_portfolio(  
        objective=my_objective,  #Fill in with your objective function.
        constraints=[constrain_sector_style_risk],  
    )

constrain_sector_style_risk = opt.experimental.RiskModelExposure(  
    risk_model_loadings=context.risk_loading_pipeline,  
    version=0,  
    min_momentum=-0.1,  
    max_momentum=0.1,  
)
```

Use a specific version of RiskModelExposure to predict its behavior.

Algorithms must have positive returns over a 2-year period (total, not daily).

Use the evaluate backtest notebook with a backtest of at least a 2-year duration to check its contest viability.

Algorithms in the contest will run with a commission model charging .0001 per share and 5 basis points slippage.

When evaluating strategies Quantopian assumes the trading conditions of a prime brokerage (institutional brokerage)

Three fields are in every Quantopian dataset:

* `​sid`​ provides a standard, cross-set stock id
* `​timestamp`​ indicates when the data was added
* `​asof_date`​ indicates what period or time the data refers to

Import `​blaze`​ to do analysis on Quantopian Datasets. It can also split and join datasets.

Want at least 100 positions with average daily turnover under 20% and peak daily turnover under 50%