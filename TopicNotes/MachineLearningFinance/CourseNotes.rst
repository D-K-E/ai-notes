#############################
Machine Learning for Trading
#############################

Reading and Plotting Stock Data
================================

What real stock data looks like:

Date; Open; High; Low; Close; Volume; Adj Close

- Open: is the price that the stock opened at.
- High: is the highest price that the stock had during the day
- Low: is the lowest price that the stock had during the day
- Close: is the price that the stock closed.
- Volume: is the number of the shares of the stock traded during the day
- Adj Close: is the adjusted close, it reflects the adjustment of the data
  provider, which includes stuff like dividend payements and splits, etc.

.. code-block:: python

"""Plot High prices for IBM"""

import pandas as pd
import matplotlib.pyplot as plt

def test_run():
    df = pd.read_csv("data/IBM.csv")
    # TODO: Your code here
    highes = df['High']
    highes.plot()
    plt.show()  # must be called to show plots

For plotting two columns at the same time:

def test_run():
    df = pd.read_csv("data/IBM.csv")
    # TODO: Your code here
    df[['Close', 'AdjClose']].plot()
    plt.show()  # must be called to show plots

Problems to solve in csv data

- Obtain date ranges
- Read in multiple stocks
- Align dates for multiple stocks
- Proper date order

New York Stock Exchange have 252 working days in a year

Global Statistics
=================

We can have the following statistics easily with pandas:

- mean
- median
- std (standard deviation)
- sum
- prod
- mode

All together there is 33 global statistics that one can compute in pandas.

They all concern series.

Rolling Statistics
==================

Global statistics gives the statistic on the entire range of the rows.
Rolling statistics gives the statistic for a window, a limited range of rows
in iterative fashion. For example I take first 20 days between indexed
between 0-19, then another 20 days between 1-20, then another, etc.

One of the hypothesis is to look for areas where the rolling mean crosses
with the day to day price of the stock.
If the day to day price of the stock crosses downward from the rolling mean,
it is probable that it would return to the mean later on, thus crossing downward represent a good buying opportunity. If it crosses upwards then it represents a good selling opportunity.

How do we know that the divergence between the daily price and the rolling
statistic is so far that we should consider a buy or sell opportunity. We
look at the rolling standard deviation between the prices and the rolling
mean

Bollinger Bands
----------------

Basically this is a way to understand if the price of the stock deviates
significantly from the rolling mean.
We add 2 bands, 1 above and 1 below the rolling mean. The distance between
these 2 bands and
the rolling mean is 2 standard deviations.
If something is 2 standard deviations below the rolling mean and
start
moving upwards, then it is time to buy that stock, if it
is above 2 standard deviations band, it is tistock, if it is above 2 standard deviations band, it is time to sell sell
If something isrolling mean2 standard deviations below the thing and start moving upwards, then it is time to buy that thing

They work not so well.

Daily Returns
-------------

How much did the price go up or done on a particular day, and it is
calculated as follows:
:math:`today_return = (priceToday / priceYesterday) -1`

Most of the time we make a chart of daily returns, like on a day, the return
was +10%, etc

Cummulative Returns
--------------------

Cummulative return is the todays price compared to the price at the
beginning.

The formula would be:

:math:`cummalitveReturnToday = (priceToday / priceFirstDay) -1`

Missing Data
-------------

People think financial data is:

- Perfectly recorded at every minute
- Uniform accross all data providers
- No gaps in data

Reality:

- This is not the case, sometimes there are gaps between data
- Sometimes some stocks are not traded in that day.
- There is no single price for a stock in a given time
- Sometimes stock come into existence, sometimes they go out of existence

Why data goes missing?
-----------------------

SPY is S&P 500, one of the most liquid and actively traded ETFs out there.
We typically use it as a reference for time for other stocks

For example a company can be bought by other companies.

How to handle them ?
You can fill the gaps with last known prices.
The filling happens at 2 steps:

- Fill forward the gaps between the data.
- Fill backwards if the beginning is missing for the dataset

Daily Returns and Histograms
-----------------------------

We can create histograms of change from daily returns, that is we can
plot how much a price had changed in a given day in a histogram

Histograms of daily returns look like a gaussian distribution or normal
distribution, that is they look like a bell curve.

With the histogram we can calculate statistics like standard deviation, or
mean, or kurtosis.

Kurtosis is the greek word for curve, it measures the tails/edges of the
distribution, that is how many bins are outside of the tails/edges of the
bell curve if the curve was a gaussian distribution.
Kurtosis measures the likelihood of extreme occurrences, that is, of severely
positive and severely negative returns relative to normal returns.
A curve might have:

- Fat tails: that is lot of bins are outside of the normal distribution curve
  This means the output of the kurtosis function would be positive
- Skinny tails: less bins are outside of the normal distribution curve
  This means the output of the kurtosis function would be negative

Skewness describes the tendency of the strategy to deliver
positive or negative returns. Positive skewness of a return distribution implies
that the strategy is more likely to post positive returns than negative returns.

When the curve is broad on shoulders, the volatility is high, that is the
risk is high, since the standard deviation is larger

Scatter plots
--------------

They are another way to visualize the relationship between the daily returns.

For example we can plot the relationship between 2 stocks on the coordinate
plane by attributing them to x and y axis. We can then use the measures of each
axis to determine a relationship between them. Then these relations would be
materialised using the dots on the coordinate plane. At the very end, we can
fit a line to these dots. The line equation has particular properties.
The slope is called beta, and the intercept is called alpha.

- Beta represents the how reactive is the stock to the market. If beta is 2,
  then if the market goes up 1 percent, the stock would go up 2 percent 
- Alpha represents how good the stock is performing with respect to another
  stock. If it is positive, it is performing better, if it is negative,
  it is performing worse.

It is important not to mix the slope with correlation.
Correlation is how tightly points of the scatter plot fit that line


Portfolios
----------

A portfolio is an allocation of funds to a set of stocks. For now we will
focus on buy and hold strategy, where we buy a stock and observe what's
happening later on.

Daily Portfolio value
----------------------

For example in a portfolio:

given:

- start value: 1 000 000
- start date: 2009 01 01
- end date: 2011 12 31
- symbols: 'SPY', 'XOM', 'GOOG', 'GLD'
- allocation ratios: [ '0,4', '0,4', '0,1', '0,1' ]

How do we calculate the value of portfolio day by day ?

1. We start with a dataframe of prices, indexed by date
2. Normalize prices:normed = prices / prices[0]: this gives cummulative returns
3. Multiply these norm values with each of the equities represented
   with a symbol and their associated allocation ratio:
   normed * allocs = allocated
4. position values = alloced * start_val: essentially it gives how the
   value of the asset changed over time
5. portfolio value = position values.sum(axis=1): That is we sum up each row
   representing the total value of the portfolio on that particular day.

Portfolio Statistics
---------------------

Several important statistics can be acquired from the daily portfolio value
vector:

- Daily returns: Daily return should not include the first row, which
  consists of zero.
- cummulative returns: (port_value[-1] / port_value[0]) - 1
- average daily return: daily_return.mean()
- standard deviation of daily return: daily_return.std()
- Sharp ratio.

Sharp Ratio
-----------

Risk is standard deviation or volatility.

Some rules:

- When two stocks have *same volatility*,
  choose the one with the *greatest return*
- When two stocks have *same return*,
  choose the one with *less volatility*

How to choose between a highly volatile stock with high return, and a stock
with lower volatility and lower return ?

The Sharp ratio comes into play when we are faced with such a question.

Sharp ratio is risk adjusted return.

The value of a portfolio is directly proportional to the return it
generates over some baseline (here risk-free rate), and inversely
proportional to its volatility/standard deviation.

What is risk free rate, how to calculate it ?

it is 252nd root of 1.1 minus 1 :math:`{\sqrt[252]{1.1}} - 1`
Mostly 0 is used though.

Sharp ratio can vary considerably depending on how frequently you sample
the data

Sharp ratio was first thought as an annual measure. However there is an
adjustment factor if we sample our data using different time frames.

SharpRatioWeekSampling = K * SharpRatioFormula

where K is :math:`{\sqrt{52}}` since there are 52 weeks that the portfolio
could have traded.

Beware this does not mean that the portfolio had traded for 52 weeks, it
could have traded for 42 weeks for example or 783. Since the sampling we use
is based on weeks we use the above value

For year we use: :math:`{\sqrt{12}` and for day we use :math:`{\sqrt{252}}`

Optimizers
----------

What optimizers do ?:

- Find minimum values for functions
- Build parametrized models based on data
- Refine allocations to stocks in portfolios

How to use an optimizer ?

- Define a function to minimize
- Provide an initial guess
- Call the optimizer

Convex Problems
-----------------

Convex problems are the easiest to solve.

How to visualise a convex problem:

You make a graph, and draw a line, if line is above graph, then convex. 
For example if the line cuts through the graph in two points, its convex.

Portfolio Optimization
-----------------------

Given a set of assets and a time period find an allocation of funds to assets
that maximizes the performance.
What is performance ?

- Sharp Ratio
- Cummulative Return
- etc

Portfolio optimization is a minimzation problem:

- Provide a function to minimize f(x): x, being the allocation
- provide an initial guess for X
- call the optimizers

If we are using the SharpRatio, the more is the better, so
to use a minimizer we need to multiply it with -1

Ranges and Constraints
-----------------------

Ranges: Limits on values for X 0-1, since I can not provide 200 % of my funds to
allocation

Properties of X that must be true.
For example the total allocation for each dimension of X should be 1, which is
equal to 100% of my funds

Types of Funds
===============

- ETF: Exchange traded funds,
  
  - Buy and Sell them just like stocks
  - Baskets of stocks, sometimes they can include bonds etc
  - Transparent: We know what they are holding

- Mutual Fund,

  - Buy and Sell at the end of the day: They add a net asset value at the end of
    the day
  - Quarterly disclosure, that is they don't disclose what they are holding,
    except once every quarter
  - Accordingly they are less transparent
    - However they are somewhat transparent in the sense that they have stated
      goals that they are trying to achieve

- Hedge fund
  - Buy and sell by agreement
  - No disclosure
  - Not transparent

Couple of terms:

- liquidity represents the ease with which one can buy/sell or share particular
  holding.
  For example ETFs are liquid, because they can be sold easily, they are also
  liquid because there is so much trading going on inside each day.
  ETFs with higher volumes have higher liquidity

- Large capitalization, means: shares * prices is really big


Mostly in stock exchange ETFs are repsented with 3-4 letters
Mostly in stock exchange mutuals are repsented with 5 letters

Incentives for fund managers
-----------------------------

How do the fund managers make money ?

AUM: Assets under Management, that is how much money you have at the fund.
This is particularly important because the money you would be bringing in
would be expressed in terms of percentage of your own AUM

The managers of ETFs are compensated according to an expense ratio, which is
simply some percentage of AUM. Expenditure ratios for ETFs are usually pretty
low around 0.01 - 1.00

Mutual fund managers are also compensated according to an expense ratio, which
is again some percentage of AUM. Expenditure ratios for Mutual Funds are pretty
okay, around 0.5 - 3

Hedge fund managers are mostly compensated ifferently. The old model called
"two and twenty" states, the hedge fund manager, takes 2 percent of the AUM, and
20 percent of the profits

How funds attract investors
----------------------------

Who are investors ?

- Individuals
  - Mostly wealthy folk
- Institutions
  - might be non profit foundations, like Fondation deutsch de la Meurth
- Funds of funds

Why do they invest ?

To make money.

What they would consider as a good investment choice ?

If we are talking about a hedge fund case. They would
consider:

- track record
- Simulation + Story: You can show market simulations, and tell a story of why
  your strategy works
- Good portfolio fit: Investors should believe that your strategy fits their
  portfolio

Hedge fund goals and metrics
------------------------------

What are the goals for the hedge fund

Goals:

- Beat a benchmark
- Absolute return:
  - positive return no matter what

How do we know that we are meeting these goals ?

We have seen metrics like:

- Cummulative return
- Volatility
- Risk/Reward, or Sharp ratio

Computational Architecture inside a Hedge Fund
------------------------------------------------

Key computational components of the hedge fund are following:

- Historical Price Data
- Target Portfolio
- Trading Algorithm
- Market
- Live Portfolio
- Orders

Here is how they interact:

Historical Price Data      Orders
           \.              /˙   \.
          Trading Algorithm      Market
           /˙         ˙\        ./
Target Portfolio   Live Portfolio

- Historical price data interacts with trading algorithm
- Target portfolio interacts with trading algorithm
- Live portfolio interacts with trading algorithm
- Trading algorithm interacts with orders
- Orders interacts with market
- Market interacts with live portfolio

Target portfolio is a result of a rather complex process.

Here are its main components:

- Current portfolio: current allocation of funds to stocks
- N'day forcast: results: predicted value of the stocks in "n" days
- Historical Price data: historical data of stocks
- Portfolio Optimizer
- Target Portfolio: target allocation of funds to stocks

Here is how they interact:

        N'day forecast
                      \.
Current portoflio --> Portfolio Optimizer ---> Target Portfolio
                     /˙       |˙
Historical Price Data    Risk Constraints

Basically you feed in everything to an optimizer and it gives you a target
portfolio

What is an Order
------------------

Orders are the manner in which we buy/sell stocks in order to create/update our
portfolio. Most of the time you send those orders through broker, and they
execute the orders for you.

Here is all the information that must go to a well formed order:

- Buy or Sell
- Symbol: identifier of the stock
- number of shares: how many shares I want to buy/sell
- Limit or Market: 
  - Market order is whatever price is the market currently bearing for the stock 
  - Limit order means you don't want to do any worse than a certain price
    That is give me a share of a certain stock but I don't want to pay more than
    the indicated sum
- Price
  - The indicated sum in the limit order

For example:
BUY, IBM, 100, LIMIT, 99.95

- This reads as: buy 100 shares from IBM at a price that is no more than $99.95 

SELL, GOOG, 150, MARKET

The Order Book
----------------

Each stock exchange has an order book,
this is important to know for understanding how stocks are evaluated

The order comes in:

BUY, IBM, 100, LIMIT, 99.95

and let's say so far nothing has been done in the stock exchange

It's added to the order book as:

BID 99.95 100 to IBM

This becomes public knowledge. People can see that, somebody is willing to pay
99.95 $ 100 shares of IBM stock

But since there is noone selling yet, this order can not be executed

Let's say we have another order come in:

SELL, IBM, 1000, LIMIT 100

this goes to order book as:

BID 99.95 100 to IBM
ASK 100 1000 to IBM

Since no one is buying 1000 shares at a price of 100 dollars, this order is also
on hold.

Then an order at market price comes:

BUY, IBM, 20, MARKET

Since there is someone who is selling shares of IBM, this order gets executed,
and the order book for IBM, becomes:

BID 99.95 100 to IBM
ASK 100 980 to IBM

If there are several sellers at different price range, market applies the lowest
price to the buyer

If there is a selling preasure, then the price would likely go down for the
stock

Mechanics of Short Selling
---------------------------

I want to sell 100 shares of IBM, but I don't have it.
Lisa wants to buy 100 shares of IBM.
Joe has 100 shares of IBM.

Basically, I borrow from Joe his 100 shares and sell it to Lisa.
If Joe wants his 100 shares from me, I need to buy it from someone and give him
back.
So I wait for a moment where the stocks of IBM drops in price, then buy 100
stocks of IBM, then give the 100 stocks back to Joe
However if the price goes up in the meantime, you need to buy those stocks at a
higher price, and you can loose money at significant amounts

Why company value matters
---------------------------

In general the value of a company goes up monotonically, that is it increases
over time.
So the true value of a company is distinct than what the market says about the
company, that is the price.

Most of the trading strategies focus on this divergence. How do we estimate the
true value of a company:

- Instrinsic Value: the value of the company as estimated by future dividends,
  in other words, companies pay each year to many companies, not all, or each
  quarter a dividend, so it is a cash payement if you own a share of stock you
  get a certain amount of dividend
  So intrinsic value is based on the following: if own one share of stock, we're
  going to get some amount of dividends over all of the future. 
- Book value: Totality of the assets of the company
- Market cap: The value of the stock on the market, how many shares are
  outstanding

The Value of a future dollar
-----------------------------

The value of a future dollar is less than the value of a current dollar, because
there is a risk factor coming from the fact that the future dollar might not
happen. That is if I promis to give you a dollar in future, I might not fullfill that
promis.

Thus it all boils down to interest rate:

We thus try to calculate the Present Value of a Dollar in Future.
And the formula is the following:
:math:`presentValue=\frac{FutureValue}{(1+InterestRate)^i}`

"i" is the number of periods in the future to which interest rate is applied
In order to attract people you offer higher interest rates, which lowers the value
of the future dollar. That is with high interest rate a dollar in future worth less
now.

If you are certain that the company will pay you the same amount in future than the
interest rate should be lower, if you are uncertain then it is going to be higher.
Interest rate in this context is known as discount rate, where it is higher in
with high risk, and lower with low risk

The intrinsic value of a dollar is the sum of present value overall all futures.
Since the price of a dollar is decreasing, the sum is actually an integral, that is
it is an area under the curve of the decreasing prices over time:
:math:`{\sum_{i=1}^{inf}}{\frac{FutureValue}{(1+InterestRate)^i}}`

Future value is also known as dividens. The result of the equation is:

:math:`\frac{FutureValue}{InterestRate}`

Example: The dividend of a company is 2 dollars per year, with the discount rate of
4%. What is the intrinsic value of the company ?
2 * 100 / 4 = 50$

Book Value
------------

Total assets minus intangible assets and liabilities is the classic definition of
book value.

For example:
A company has 4 factories each worth 10 million dollars.
It also has 4 patents each worth 2 million dollars
It also has a loan it has to pay that is 20 million dollars

Book value of the company is (40 million + 8 million) - (8 million + 20 million)
which is equal to 20 million dollars

Market Capitalization
-----------------------

Market cap= numberOfShares * price

This shows how significant the company is to the market. The price is its
current price of 1 share in market, times the total number of shares that are
outstanding in the market

Why information Matters in Stock Prices
----------------------------------------

Well news affects 1 thing: they reduce the expectation on future dividends, that
is they decrease the credibility of the promise that has been made by the
company on the dividends that it is going to pay in future

Buy Scenario
------------

Let's say you have a company whose:
- book value is 80M
- intrinsic value is 20M
- market capitalization is 75M

Would you buy the company ?

Yes, you should buy it right away!

Ignoring the intrinsic value, if you buy the entire company off the market (for
$75M) and immediately sell it for its book value ($80M), you have a $5M profit
right there!

Even if you are buying some stocks (instead of the whole company), the stock
price is expected to increase (as it is currently undervalued).

We can sum up the values as the following:

- Intrinsic value: the sum of all future dividends if everything stays the same
- Book value: the value of the company if we split it up into pieces and sell it
  that way
- Market capitalization: The value of the percieved value of the stock in the
  market

Trading strategies look for deviations between these values.
For example a sell strategy is:

- If intrinsic value drops significantly and the stock price is say high, it
  maybe worthwile the sell that stock

For example a buy strategy is:

- The dividends are going up and the market capitalization is low, it might be
  good buy opportunity

Similarly the book value kind of provides a lowest price, when stock price
begins to approach book value, you can pretty much assume that the price is not
going to go below book value, or not so much below it, or a predatory buyer,
would buy the company and break it up for parts

Capital Asset Pricing Model
----------------------------

It provides a mathematical framework for hedge fund investing. It explains how
market impacts individual stock prices. It was developped in 1960s

What is a portfolio ?
----------------------

It is a weighted set of assets.
The w_i is the weight of the asset i
We require that sum of the all of all the absoulte values of the weights is
equal to 1.
Let's explain all this in a more formal way:

- :math:`w_i` : is the weight of a particular asset i
- :math:`{\sum_i} abs(w_i)=1.0`: sum of the absolute value of the weights of
  the assets
- :math:`r_p(t) = {\sum_i} {w_i}{\times}{r_i}(t)` : Simply put the return of an
  asset is sum of the weight of the asset times the return on that particular
  day

Market Portfolio
------------------

When someone refers to the Market what they are usually refering to is an index
that broadly covers a large set of stocks:

- US: S&P 500: 500 large companies that traded in the exchanges, it changes in
  accordance with the price of its components
- UK: FTA
- Japan: Topix

Market Portfolio is a combination of those stocks in a certain weighting.

Most of the important indexes are Cap Weighted:

- Individual weight of each stock in the portfolio is set according to that
  stock's market cap: :math:`w_i = {\frac{marketcap_i}{\sum_j marketcaps}}`

Market cap is Market Capitalization, that is the number of shares available for
the stock times its price 

CAPM equation
---------------

Capital asset pricing model equation is simply a linear regression equation:

- :math:`r_j(t)= {\beta}_j {\times} r_m(t) + {\alpha}_j (t)`
  Here is the deal for a particular stock on a market, its return on a day 't'
  is its beta times the return of the market plus the alpha of the stock
  We expect alpha to be 0, beta to be closer to 1, but this is not always the
  case

CAMP for portfolio
-------------------

Now for individual stocks it is the above mentioned equation.

For portfolios we simply modify the equation as the following:

- :math:`r_{portfolio}(t) = {\sum_j}w_j({\beta}_j {\times} r_{market} + {\alpha}_j)`

  That is we simply multiply the CAPM return with its associated weight and
  sum up the resulting values of each portfolio

Risks for Hedge Funds
----------------------

Typical hedge fund tries to find stocks that would perform well with respect to
market. The stock should go up more than the market if the market goes up, or
goes down less if the market goes down. If this information is reliable, they
can virtually guarantee positive return by using market capitalization

Two Stock Scenario
-------------------

Here is the scenario for two stocks:

- Stock A:
  - Predicted to be +1 % over market
  - its beta is 1.0: b=1.0
  - 50 $

- Stock B:
  - Predicted to be -1 % below market
  - its beta is 2.0: b=2.0
  - 50 $

Our position for stock A is long and stock B is short.
That is we want to have less stock B and more stock A

Let's say the market went up 10 %. What would be our return ?

We simply apply CAPM.
We have 2 stocks, so our allocation is by default 0.5 for each stock

our return for stock A is 10(market return) * 1.0(beta value) + 1(alpha value
predicted value) = 11 %
In dollars: 5,5 $

our return for stock B is 10(market return) * 2.0(beta value) - 1(alpha value
predicted value) = 19 %
However our position is short that is we sell this stock
so we say it is -19 %
In dollars: -9.50 $

total in dollars: - 4 $
total in percent: 11(calculated return for A) * 0.5 (stock A allocation) - 19(
calculated return for B) * 0.5(stock B allocation) = - 4 %

CAPM in Two Stock Scenario
---------------------------

Capm :
:math:`r_{portfolio}(t) = {\sum_j}w_j({\beta}_j {\times} r_{market} + {\alpha}_j)`

Our case:

- A:
  - alpha = +1
  - beta = 1
  - weight = 0.5

- B:
  - alpha = -1
  - beta = 2
  - weight = 0.5

The result of the equation if we plug in these numbers is:
-0.5 * r_m + 1
negative point five times the market return plus 1

We can predict the +1 part with data but we have no control over market return.
So what we try to do is to get rid of it by rearanging the beta for the
portfolio which is equal to -0.5 in the example. We need to make it 0 so that
we can get rid of the market risk. This is done by rearanging the weights that
are associated with betas of each stock
For example in our case the weight of stock B, should be -1/3 and the weight
of the beta of the stock A, should be 2/3.

Technical Analysis
--------------------

Technical analysis has the following characteristics:

- It concerns itself with historical price and historical volume only
- Compute statistics called indicators
- Indicators are heuristics

It is a good trading approach but not necessarily a good investing approach

When is technical analysis effective
-------------------------------------

- Individual indicators are weakly predictive
- Combinations of indicators are stronger
- Look for contrasts stock contrasting to the market 
- Shorter time periods

When trading in shorter periods the fundamental factors don't have the
time to weigh in much

Technical Indicators
---------------------

A lot of them exist:

- momentum: How much has the price changed looking at a period
  - We can have positive momentum for increase in price
  - We can have negative momentum for decrease in price
  - The steepnes or the rate of change in both of these scenarios indicate the
    strength of the momentum
  - formally momentum is :math:`momentum(t)=(price[t]/price[t-n])-1`
    - 'n' number of days for the momentum. So we talk about momentum in n number
      of days 
- sma: simple moving average: we simply take the average of prices for a given
  periods in frames. For example, for a period of two years we take a frame of 7
  days and calculate the average for that frame, and pass on to the next 7 days,
  etc. That gives a moving average. It looks like the smoothed version of prices
  in two years graph
  - Interesting moments are where the sma graph crosses with the prices graph.
    That combined with strong momentum can give a trading signal
  - Also the distance between the price for a given day and its moving average
    gives an arbitrage opportunity, since the price would return to moving
    average at one point,
  - formally sma is :math:`sma[t]=( price[t]/mean(price[t-n:t]) ) -1`

- bolinger bands: We put bands of 2 standard deviations above and below the
  simple moving average. These are called bolinger bands. We look for two
  signals SELL and BUY
  - SELL signal should be given when the price is above the bolinger band and
    starts to get closer to the band.
  - BUY signal should be given when the price is below the bolinger band and
    starts to get closer to the band
  - formally bollinger bands are :math:`bb[t]=(price[t] - sma[t]) / 2 × std[t]`
  - Price above the band would have value greater than 1.0
  - Price below the band would have value less than -1.0

Normalization
----------------

The problem is the technical indicators use different ranges and if want to use
machine learning in our analysis we need to be able to express them in common
unit.
This is done by normalization. We simply use the following formula for
normalizing values. Normalizing in this sense is expressing a distribution in a
given range in another range.
The formula is the following :math:`values - mean / values.std()`. This formula
maps the input range to -1 - 1 range.

How data is aggregated
------------------------

Tick represents a successful by cell match or a successful transaction.
Tick data is usually consolidated on minute by minute or hour by hour chunks

It has several familiar columns:

- open: opening price at the beginning of the chunk
- high: highest price within the chunk
- low: lowest price within the chunk
- close: clcosing price within the chunk
- volume: traded volume, number of shares, within the chunk

Dealing with smaller time periods requires larger databases and more computing
power

Stock splits
--------------

When the prices are too high, stocks split.

Why split the stocks ?

Well when the prices are too high, they become less liquid, since they are too
expensive. Hence they become difficult to trade, so the company decides to split
the stock.
This results in seemingly drops in prices, but this is wrong, becausr the value
does not change. For example if you had 10 shares worthing 200 $, after a 2:1
stock split, you would have 20 shares. However in your dataset it would seem as
if the stock price has dropped from 20 to 10 dollars.

It is difficult to take this into account in computer. However the section
adjustedClose in tick data, responds exactly to this problem.

AdjustedClose and the actual close is always same in the data collection day.
If I had collected the data today, than the actual close and adjustedClose
were same today.
This is because the adjustedClose is calculated with respect to data collection
day.

How do we take the splits into account as we are "adjusting" the close?
As we go back in time, we simply follow regular close, and when we come to a
split, we divide the prices before the split in accordance with the ratio
of the split
For example:

I have stock with a share at the closing price of 20 $
5 days before it had 2:1 split, and 10 days before it had 4:1 split

+------+-------------+----------------------+
| Days | Close Value | Adjusted Close Value |
+------+-------------+----------------------+
| 0    | 20 $        | 20 $                 |
+------+-------------+----------------------+
| -1   | 19 $        | 19 $                 |
+------+-------------+----------------------+
| -2   | 18 $        | 18 $                 |
+------+-------------+----------------------+
| -3   | 17 $        | 17 $                 |
+------+-------------+----------------------+
| -4   | 16 $        | 16 $                 |
+------+-------------+----------------------+
| -5   | 18 $        | 18 $                 |
+------+-------------+----------------------+
| -6   | 38 $        | 19 $                 |
+------+-------------+----------------------+
| -7   | 40 $        | 20 $                 |
+------+-------------+----------------------+
| -8   | 41 $        | 20,5 $               |
+------+-------------+----------------------+
| -9   | 43 $        | 21,5 $               |
+------+-------------+----------------------+
| -10  | 44 $        | 22 $                 |
+------+-------------+----------------------+
| -11  | 180 $       | 22,5 $               |
+------+-------------+----------------------+
| -12  | 168 $       | 21 $                 |
+------+-------------+----------------------+

Dividends
-----------
Companies regularly pay their dividends to their owners up to %1, %2

Paying dividends effect significantly the actual price of the stocks
Here is an example:
Company A says that they are going to pay a dividend of 1 $ in 10 days
Its stock's current value is 100 dollars
Since people know that in 10 days you'll have a 1$ the value of the stock
slowly rises to 10 + 1 $. In the 9th day the price of the stock can be
expected to be 11 $, however in the 10th day the value of the stock would
return to 10$, since everyone knows that the dividend has been paid by the
company

Adjusting for the dividends are calculated exactly the same as stock splits.

Note:: Always use Adjusted Close

Survivor Bias Free Data
-------------------------

Here is a general problem. When you develop your algorithm and strategy
and apply to some generic stock like S&P500, and see that your strategy is
reallly performing well on historical tick data. You risk being too optimistic,
because you are basing your strategy on stocks that had survived.
The bias of the stocks that are available today is called survivor bias.
It can be a major source of error. It can be remedied by buying survivor bias
free data.

Efficient Markets Hypothesis
-----------------------------

- Large number of investors
  - They meet at the market to make profit.
  - They have an incentive for opportunities where the price of the stock is
    out of line with its true value
- New information arrives randomly
- Prices adjust quickly
- Prices reflect all available information

Where does information come from ?

- Price volume: public, basis of technical analysis
- Fundemental data: public, published in quarterly, shows companies true value
- Exogenous data: Information about the environmment in which the stock operates
  For example price of oil goes down, the price of airlines goes up, because
  energy is the number 1 cost for airline companies
- Company insiders: You are a ceo, you know that your new piece of software will
  improve the existing software a lot, so you know that your stock will rise,
  so you buy your own stocks before hand based on the information that the
  people outside of the company do not have.

3 forms of Efficient Markets Hypothesis
----------------------------------------

- Weak: Future prices can not be predicted by analysing historical prices
  - You can still do fundamental analysis and rely on insider information
- Semi-strong: Prices adjust immediately to new public information
  - You can still rely on insider information
- Strong: Prices reflect all information public and private
  - You can not make money

Is the hypothesis correct ?
No, at least not the semi-strong and strong

Grinold's Fundamental Law
---------------------------

The problem is how to measure the performance of an investor. Is it the skill,
that is the capacity to pick good stocks, is it the breadth, that is the amount
of opportunities that you can find to apply those skills ?

So Richard Grinold has been thinking about how to relate the skill, breadth, and
performance to each other and he came up with the following equation

:math:`performance=skill {\times} {\sqrt{breadth}}`

- Performance is also called: Information Ratio
  This is very much like the sharp ratio, but it refers to the ratio of excess
  returns, in other words the manner in which the portfolio manager is exceeding
  market's performance
- Skill is summarized in something called: Information Coefficient
- Breadth is how many trading opportunities we have

Reward: expected return
Risk: can be observed as high standard deviation

- Higher alpha generates higher sharp ratio: alpha is the skill
- More execution opportunities provide a higher sharp ratio
- Sharp ratio grows as the square root of breadth

Remember our equation for returns was the following:
:math:`r_{portfolio}(t) = {\sum_j}w_j({\beta}_j {\times} r_{market} + {\alpha}_j)`

- the alpha here again is about the skill of the portfolio manager
- The information ratio can be defined as:
  :math:`IR={\frac{mean(\alpha (t)_p)}{std(\alpha (t)_p)}}`

- IC: information correlation is simply the correlation of forecasts to returns.
  Forecasts are done by the manager

- BR, Breadth: number of trading opportunities per year

Portfolio Optimization
------------------------

Given a set of equities and a target return, find an allocation that minimizes
the risk

What is risk ?
Volatility of the prices of the stock, that is the standard deviation of
historical daily returns

A good way to visualize the stock prices is risk vs return
Risk is on the x axis, and return is on the y axis

Covariance, correlation is really important
High correlation means, stock A goes up, stock B goes up as well
Low or negative correlation means stock A goes up, stock B goes down

What we try to do is to blend in stocks with negative correlation because it
provides very low volatility, since as one goes up the other goes down, it
protects the portfolio's balance

But we also want them to have returns, that is if a stock is raising while the
other is falling all the time then there is no point in buying the failing stock
Thus the stocks we have should have negative correlation in short term but
positive correlation in long term

Mean variance optimizer
-------------------------

Inputs:
- Expected return: what do we expect as return for the stock
- Volatility, standard deviation over historical daily prices
- Covariance: this is a matrix showing the correlation between each assets
- Target return

Output:
- Asset weights for portfolio that minimizes the risk


Efficient frontier is a line that is between the lowest risk/return point
until the highest risk/return point.

Any portfolio below the line is suboptimal
When you draw a tangent line to the efficient frontier, you get the max sharp
ratio point

Reinforcement Learning
------------------------

Every agent has the following cycle in robotics:

- Sense
- Think
- Act

That is the agent senses the environment, then evaluates the environment, and
finally acts upon the environment which affects the environment

Some form of description of the environment comes into the agent,
let's call this

State S,

the agent then effects the state S with a policy P

P(S)

this process outputs and action A

P(S) -> A

Action simply changes the state S creating S' by creating a reaction in
terms of a transition function of an environment

P(S) -> A -> T(S') ->

Each action is associated with a reward or cost

In terms of trading:

- our environment is market
- our actions are buying, selling, holding
- our states are factors about our stocks that we might observe and know about 
- our reward is profit

Let's formalize a little:

- There are a set of States, that comes into the agent

- There are a set of Actions that the agent can take:
  - BUY
  - SELL
  - HOLD

- There is a transition function of the following form:
  - Transition[State, Action, State']
    - Basically it is a 3d object, it records the following information:
      - Given that we are at the state S,
      - If we take the action A,
      - The probability that we are going to have a State' is some value
        - The sum of all the next states that can occur as the result of our
          action has to be equal to 1

- Reward function R(S,A)
  - If we take the action A on state S, we should have a Reward value

Policy P, is the one that maximizes the output of our reward function

There are two algorithms that can help:

- Policy iteration
- Value iteration

However since we don't know the reward and transition what do we do ?

Well we interact with the environment.
The interaction takes place as the following:
- <s_1, a_1, s'_1, r_1>
- <s_2, a_2, s'_2, r_2>
- <s_3, a_3, s'_3, r_3>
.
.
.

With these we can use two approaches:
- We can deduce a model for transition function and reward statistically from
  data and interactions
- We can use a model free learning approach called q learning we'll see it in a
  bit

Now reward is being in a certain state,
each state is arrived in a series of actions,
each action has a cost,
however there is also an important phenomenon unique to finance in this
formulation value of the dollar decays
That is when you formulate your cost as loss of money, your initial money,
also looses value over time, because a dollar today is more valuable than a
dollar in future

However we can incorporate this to our reward function by using discounted
reward function which basically multiplying our reward function with a ratio
indicating the decrease in percentage of the money within the time period

for example our reward function in finance would be like the following:

:math:`objective = max(Expactation[{\sum_{t=0}^{infinity}({\gamma}^t)R_t}])`

Q Learning
----------

What is Q ?

Q is a function that takes two arguments Q(action, state) it returns a value
which represents the utility value of the action in that state that is the
immediate reward + the future rewards.

How to use Q(state, action) ?

When we want to know the policy for a state s we simply take the argmax_a for
Q(s,a). So

policy P(state) = argmax_a(Q(state, a))

**When we run q learning long enough we converge to the OPTIMAL POLICY**

Big picture in q learning:

- Select training data
- Iterate over time <s,a,s',r>
- test policy P
- repeat until converge, converge means more iterations does not make the policy
  better

Details:

- Set start time, initialize Q() or Q table, most of the time we initialize the
  Q table with some small random numbers
- compute state, that is features of our stocks, from those we build up our
  state
- select action
- observe reward, and state'
- update Q table

Update Rule
-----------

Agent chooses an action and applies that to a state, which results in a different
state. This stuation is associated with a reward and based on this reward we need
to update our Q table

The update rule for the q table is
:math:`(1-{\alpha}) \times Q[state,action]+{\alpha} {\times}IME`

- alpha is our learning rate between 0-1 which is usually about 0.2
- IME: is our improved estimate which is:
  - :math:`reward + gamma {\times} laterRewards`
    - gamma is the discount rate between 0-1, low value means, we value
      later rewards less

We simply multiply our old q value with a portion of learnin rate

What is `laterRewards` ?

Let's say that we had arrived to state s'. We ask ourselves,
what would be the appropriate action to take in order to maximize the q value,
that is we find the best action that maximizes the reward of the s'

So our update rule in its totality is the following:

.. math::

   `(1-{\alpha}) \times Q[state,action]+
   {\alpha} {\times} (
   reward + gamma {\times} Q[state', argmax_{a'}(Q[state', action'])]
   )

Update Rule

The formula for computing Q for any state-action pair <s, a>, given an experience tuple <s, a, s', r>, is:
Q'[s, a] = (1 - α) · Q[s, a] + α · (r + γ · Q[s', argmaxa'(Q[s', a'])])

Here:

    r = R[s, a] is the immediate reward for taking action a in state s,
    γ ∈ [0, 1] (gamma) is the discount factor used to progressively reduce the value of future rewards,
    s' is the resulting next state,
    argmaxa'(Q[s', a']) is the action that maximizes the Q-value among all possible actions a' from s', and,
    α ∈ [0, 1] (alpha) is the learning rate used to vary the weight given to new experiences compared with past Q-values.

Two finer points
-----------------

- Sucess depends on exploration
  - choose random action with probability c
    - In most cases we begin with probability 0.3 and we decrease it
      at each iteration

Trading Problem Elements:
---------------------------

- Reward: Daily Returns on Stocks
- Actions: Buy, Sell, Nothing
- State should include:
  - adjustedClose / simple moving average
  - Bollinger band values
  - P/E ratio
  - Holding Stock
  - return since entry

Creating state:

- State is an integer since we are going to be using q learning
  - In order to transform our factors to state, we need to:
    - Discretize each factor
    - Combine each of them

Discretizing is important. It simply means we need to find a way to represent
a real number in an integer scale

We can use the following algorithm:

.. python::

   stepsize = size(data)/steps
   data.sort()
   for i in range(0, steps):
       threshold[i] = data[(i++1) * stepsize]

Advantages

The main advantage of a model-free approach like Q-Learning over model-based
techniques is that it can easily be applied to domains where all states and/or
transitions are not fully defined.
As a result, we do not need additional data structures to store transitions
T(s, a, s') or rewards R(s, a).
Also, the Q-value for any state-action pair takes into account future rewards.
Thus, it encodes both the best possible value of a state (maxa Q(s, a)) as well
as the best policy in terms of the action that should be taken (argmaxa Q(s, a)).

Issues

The biggest challenge is that the reward (e.g. for buying a stock) often comes
in the future - representing that properly requires look-ahead and careful
weighting.
Another problem is that taking random actions (such as trades) just to learn a
good strategy is not really feasible (you'll end up losing a lot of money!).
In the next lesson, we will discuss an algorithm that tries to address this
second problem by simulating the effect of actions based on historical data.

Dyna Q learning
----------------

It is a blend of model free and model based learning:

Regular q learning which is expensive in terms of money
because it requires interaction with the real world:

1. initiate Q table
2. observe state S
3. execute action a
4. observe resulting state S'
5. calculate reward R
6. update Q table with <state S, action a, state S', R>

Then starts the dynamic part:

7. Learn T and R
   - T is our transition matrix which shows how state S transforms with the
     action A into state S'
   - R is our expected award

Learning T

Transition matrix T simply shows how state S is transformed into state S'.
It has the following form:
- We fill out the matrix by simple probability value:
  - We start with state S, then we apply an action and obtain a state S'
  - Then count how many S' we have had and attribute a probability value
    to the S' with respect to S in the light of action a

More concretely it goes out as the following:

initialize T_{count} table values with 0.00001 to avoid 0 division
while executing Q learning:
observe state S, action a, state S'
increment the corresponding T_{count} table value with respect to observed
parameters

Transition matrix value is simply the following formula:
:math:`T(s,a,s') = \frac{T_c(s,a,s')}{{\sum}_i T_c(s,a,i)}`

That is simply the T_c value of the parameters over the sum of all the resulting
state S' in T_c which occured with action a imposed upon state S.

Learning R

R is the expected reward for state S and action a
r is the immediate reward for state S and action a

We update the R with the following:

R'[S,a] = (1-learningRate) × R[S,a] + learningRate × r

8. Update Q table

Basically the whole algorithm is:

Real world:

1. initiate Q table
2. observe state S
3. execute action a
4. observe resulting state S'
5. calculate reward R
6. update Q table with <state S, action a, state S', R>

Simulation:
7. Update T[s,a,s']
8. Update R[s,a]
9. initialize a random state 
10. intialize a random action
11. infer state' from transition matrix T
12. get immediate rewar r: R[s,a]
13. update Q table with these new parameters

Summary

The Dyna architecture consists of a combination of:

direct reinforcement learning from real experience tuples gathered by acting in an environment,
updating an internal model of the environment, and,
using the model to simulate experiences.

Trading Strategies
-------------------

Jump diffusion model: reproduces the 6 sigma event:
It is used for risk management 

You have a current portfolio, you do a monte carlo simulation to see how risky
that portfolio is

Yield curve arbitrage:
model the swap 

These trading strategies work in highly volatile environments

1. Look for therotically sound ideas, no black box neural nets etc, it should
   make sense on a white board
2. Empiracly test this
3. Beware of complexity, if it does not stay simple, you are probably
   overfitting and trapping yourself to somewhere

DON'T:

- Endlessly iterate on same data, your learner creates a bias
