# Quant-trading
量化交易
<br>

## Intro

Most scripts inside this repository are technical indicator automated trading. These scripts include various types of momentum trading, opening range breakout and statistical arbitrage strategies. Yet, quantitative trading is not only about technical analysis. It can refer to computational finance to exploit derivative price mismatch, pattern recognition on alternative datasets to generate alphas or low latency order execution in the market microstructure. Hence, there are a few ongoing projects inside this repository. These projects are mostly strange trading ideas I come up with to beat the market (or so I thought). There is no options strategy or HFT strategy simply because option data and ultra high frequency data are very expensive to acquire (even consider platforms like Quantopian or Quandl). Additionally, please note that, all scripts are historical data backtesting (basically via Python, not C++, maybe Julia in the near future). The assumption is that all trades are frictionless so we don't have to worry about slippage or liquidity. 

这个仓库里的大多数脚本涉及到自动化交易的技术方面。这些脚本包括多种类型的动量交易，开盘区间突破和统计套利策略。当然，量化交易不仅仅包括技术方面的分析。他还可以指用来探索衍生品价格不匹配的计算金融，通过在数据集上生成低延迟交易指令的模式识别。所以本仓库里还有很多正在进行的项目。这些项目仅仅是我个人的思考。因为高频数据很难得到，所以我们没有可供选择的策略。另外，需要注意到，所有脚本都使用的是历史数据。我们的基本假设是所有的交易都是无摩擦的，所以我们无需考虑量的下跌和流动性问题。

### Table of Contents

<a href=https://github.com/je-suis-tm/quant-trading/blob/master/README.md#1-macd-oscillator>1.MACD oscillator</a>

<a href=https://github.com/je-suis-tm/quant-trading/blob/master/README.md#2-pair-trading>2.Pair trading</a>

<a href=https://github.com/je-suis-tm/quant-trading/blob/master/README.md#3-heikin-ashi-candlestick>3.Heikin-Ashi candlestick</a>

<a href=https://github.com/je-suis-tm/quant-trading/blob/master/README.md#4-london-breakout>4.London Breakout</a>

<a href=https://github.com/je-suis-tm/quant-trading/blob/master/README.md#5awesome-oscillator>5.Awesome oscillator</a>

<a href=https://github.com/je-suis-tm/quant-trading/blob/master/README.md#6oil-money-project>6.Oil Money project</a>

<a href=https://github.com/je-suis-tm/quant-trading/blob/master/README.md#7dual-thrust>7.Dual Thrust</a>

<a href=https://github.com/je-suis-tm/quant-trading/blob/master/README.md#8parabolic-sar>8.Parabolic SAR</a>

<a href=https://github.com/je-suis-tm/quant-trading/blob/master/README.md#9bollinger-bands-pattern-recognition>9.Bollinger Bands Pattern Recognition</a>

<a href=https://github.com/je-suis-tm/quant-trading/blob/master/README.md#10-relative-strength-index-pattern-recognition>10.Relative Strength Index Pattern Recognition</a>

<a href=https://github.com/je-suis-tm/quant-trading/blob/master/README.md#11monte-carlo-project>11.Monte Carlo project</a>

### Data Source

1.<a href=https://finance.yahoo.com>Yahoo Finance</a>/<a href=https://pypi.org/project/fix-yahoo-finance>fix_yahoo_finance package</a>

2.Bloomberg/Eikon

3.<a href=http://www.histdata.com/>Histdata</a>/<a href=https://fxhistoricaldata.com>fxhistoricaldata</a>

4.<a href=https://stooq.com>Stooq</a>/<a href=https://www.quandl.com>Quandl</a>

<br>

## Strategies:

### 1. MACD oscillator

移动平均震荡指标

MACD oscillator is trading strategy 101. MACD refers to Moving Average Convergence/Divergence. It is a momentum trading strategy which holds the belief that upward/downward momentum has more impact on short term moving average than long term moving average. It only takes 5 minutes for any bloke with no background in finance to trade with MACD signals. Regarding the simplicity of MACD oscillator, it is the most common strategy among the non-professionals in the market. In behavioral economics, the more people believe in the strategy, the more effective the strategy becomes (not always true, e.g. 2008). Therefore, we should not underestimate the power of MACD oscillator.

移动平均震荡指标是101号交易策略。MACD指移动平均收敛或差异。他是一个动量交易策略，理念是向上/向下的动量对短期的移动平均比长期的移动平均影响更大。对于任何没有金融背景的人，只需要5分钟就可以通过MACD信号进行交易。尽管MACD策略很简单，他是市场中不太专业中最普遍的策略。就行为经济而言，越多人相信该策略，该策略越有效（但不是一直是这样，比如2008年）。所以我们不应该低估MACD策略的力量。

For the strategy itself, we compute long term moving average and short term moving average on the close price of a given stock. To generate the trading signal, we implement a comparison between the moving averages of different time horizons. When short term moving average is above long term moving average, we long the given stock accordingly. Vice versa.

对这个策略本身，对于给定的市场，我们计算闭市价格的长期移动平均和短期移动平均。为了生成交易信号，我们比较不同时期的移动平均。当短期移动平均大于长期移动平均，我们相应的持有给定的股票。反之亦然。

![alt text](https://github.com/je-suis-tm/quant-trading/blob/master/preview/macd%20positions.png)

![alt text](https://github.com/je-suis-tm/quant-trading/blob/master/preview/macd%20oscillator.png)

### 2. Pair trading

配对交易

Pair trading is the basic form of statistics arbitrage. It relies on the assumption that two cointegrated stocks would not drift too far away from each other. First step, we select two stocks and run <a href=https://en.wikipedia.org/wiki/Error_correction_model#Engle_and_Granger_2-step_approach>Engle-Granger two step analysis</a>. Once the criteria of cointegration is met, we standardize the residual and set one sigma away (two tailed) as the threshold. After that, we compute the current standardized residual of the selected stocks accordingly. When the standardized residual exceeds the threshold, it generates the trading signal. The simple rule is we always long the cheap stock and short the expensive stock. 

配对交易是统计套利交易的一种基本形式。他基于这样的假设，两个较相关的股票不会彼此偏离太多。第一步，我们选择两只股票，然后运行Engle-Granger 两步分析。一旦这两只股票达到相关的准则，我们对两只股票的差值进行标准化，并设定阈值标准。然后，我们计算两只股票当前的标准差值。当值超过我们之前设定的阈值标准，就产生交易信号。基本准则是我们总是买入便宜的股票，卖出贵的股票。

The core idea of pair trading is <a href=https://en.wikipedia.org/wiki/Cointegration>cointegration</a>. Metaphorically speaking, cointegration is like a couple in a clingy relationship where two parties are crazy-glued together. Yet, most relationships break sooner or later, and only the very few can make it to the marriage (from a statistics perspective, not being pessimistic). Hence, it is important to frequently check on the status quo of cointegration before any pair trading order execution (the same applies to relationships).

配对交易的核心思想是相关程度。打个比方，相关度就像是热恋中的情侣，紧密的联系在一起。然而，大多数关系总会或短或长的破碎，只有少部分能真正走入婚姻的殿堂。所以在真的交易之前需要时常的检查配对的股票状态。

![alt text](https://github.com/je-suis-tm/quant-trading/blob/master/preview/pair%20trading%20eg%20two%20step.PNG)

![alt text](https://github.com/je-suis-tm/quant-trading/blob/master/preview/pair%20trading%20z%20stats.png)

![alt text](https://github.com/je-suis-tm/quant-trading/blob/master/preview/pair%20trading%20positions.png)

### 3. Heikin-Ashi candlestick

平均K线图

Heikin-Ashi, the exotic name actually referring to 'Average Bar' in Japanese, is an alternative style of candlestick chart. The sophisticated rules of Heiki-Ashi are designed to filter out the noise for momentum trading. Hence, Heikin-Ashi shows more consecutive bars in contrast to the standard candlestick, which makes price momentum and reverse points more distinguishable in figures. Arguably it should outperform the standard candlestick in sideways and choppy markets. 

Heikin-Ash，是一个指日本的“Average Bar”的带有异域风情色彩的名字，是K线图的另外一个表现形式。Heiki-Ashi的设计原则就是为了过滤动量交易的噪音。所以，相较于标准的K线图，Heikin-Ashi表现出了更多连续的块。因此可以让价格和相反点在图形中表现的更加清晰。可论证的是，他在波动的市场中优于标准的K线图。

For the strategy itself, initially we make a few transformations on four vital benchmarks - Open, Close, High, Low. The next step is to apply unique Heikin-Ashi rules on Heikin-Ashi Open, Close, High, Low to generate trading signals. The downside of Heikin-Ashi (or any momentum trading strategies) is the slow response. Thus, we should set up the stop loss position accordingly so that we don't get caught up in any flash crash.

对于这个交易策略本身来说，我们在四个基础的指标上做了一些变换-开市，闭市，最高点和最低点。下一步就是将独特的Heikin-Ashi规则用于Heikin-Ashi的高，开，低，闭来生成交易信号。该策略的缺点就是反应较慢。所以我们需要设置止损点。

The rules of Heikin-Ashi can be found in <a href=https://quantiacs.com/Blog/Intro-to-Algorithmic-Trading-with-Heikin-Ashi.aspx>Quantiacs</a>.

![alt text](https://github.com/je-suis-tm/quant-trading/blob/master/preview/heikin-ashi%20positions.png)

![alt text](https://github.com/je-suis-tm/quant-trading/blob/master/preview/heikin-ashi%20asset%20value.png)

### 4. London Breakout

伦敦突破

To one of my favourite cities in the world! Proud to be a Londoner!

London Breakout is an intra daily opening range breakout strategy. Basically, it is a fascinating information arbitrage across different markets in different time zones. FX market runs 24/7 globally. For instance, you cannot long the stock of Ford in ASX simply because Ford is listed in NYSE. As FX market is decentralised, you can long any currency pair in any market as long as the market is open. That leaves a door to take a peek at the activity in a closed foreign FX market before the opening of domestic FX market.

伦敦突破是一种日内开盘范围突破策略。这是一种极好的通过在不同时区，不同市场的套利信息。外汇交易是全球范围内的7*24小时，不间断的。因为外汇市场是去中心化的，只要市场开放，你可以在任何市场持有任何货币。这留下了一道门，可以在国内外汇市场开放前，在外围外汇市场取得最高点。

Back to London Breakout, London and Tokyo are two of the largest FX markets in the world. Tokyo FX trading hour is GMT 0:00 a.m. - GMT 8:59am. London FX trading hour (no summer daylight saving) begins at GMT 8:00 a.m. Even though there is an hour of overlap, the crucial timeframe of London Breakout is GMT 7:00 a.m. - GMT 7:59 a.m. a.k.a. the last trading hour before the opening of London market. The price movement of the crucial timeframe incorporates the information of all the overnight activities of financial market (from the perspective of the current time zone).

For the strategy itself, we establish upper and lower thresholds prior to the high and low of the crucial timeframe. Once London FX market opens, we spend the first couple of minutes to check if the price would breach the preset boundaries. If it is above threshold, we long the currency pair accordingly. Vice versa. Nevertheless, we should set up a limit to prevent us from trading in the case of abnormal opening volatility. Normally, we clear our positions based on our target stop loss or stop profit respectively. By the end of the trading hour (still from the perspective of the current time zone), if there are any open positions, we clear them out.

![alt text](https://github.com/je-suis-tm/quant-trading/blob/master/preview/london%20breakout%20positions.png)

![alt text](https://github.com/je-suis-tm/quant-trading/blob/master/preview/london%20breakout%20thresholds.png)

### 5.Awesome oscillator

Awesome oscillator is an upgraded version of MACD oscillator. It is one of those momentum strategies focusing on the game of moving average. Instead of taking simple moving average on close price, awesome moving average is derived from the mean of high and low price. Similar to MACD oscillator, it takes both short term and long term moving averages to construct the oscillator.

There are various strategies for awesome oscillator to generate signals, such as traditional moving average divergence, twin peaks and saucer. Twin peaks is just one of the many names of bottom W pattern. The pattern recognition will be covered in another chapter so the main focus of this chapter is saucer. Saucer is slightly more complex to implement than the traditional divergence. In return, saucer has the power to beat the slow response of the traditional divergence. Generally speaking, a faster response may sound awesome, but it does not guarantee a less risky outcome or a more profitable outcome. Hence, we will take MACD oscillator as a control group, to test if awesome oscillator can actually outperform MACD oscillator.

The rules of awesome oscillator could be found in <a href=https://www.tradingview.com/wiki/Awesome_Oscillator_(AO)>TradingView</a>.

![alt text](https://github.com/je-suis-tm/quant-trading/blob/master/preview/awesome%20positions.png)

![alt text](https://github.com/je-suis-tm/quant-trading/blob/master/preview/awesome%20oscillator.png)

![alt text](https://github.com/je-suis-tm/quant-trading/blob/master/preview/awesome%20ma.png)

![alt text](https://github.com/je-suis-tm/quant-trading/blob/master/preview/awesome%20asset.png)

### 6.Oil Money project

This project is inspired by an <a href=https://www.bloomberg.com/news/articles/2018-05-20/crude-oil-s-surge-is-putting-the-petro-back-in-petrocurrencies>article</a> on oil-backed foreign exchange. When the oil exits the bear market, the currency exchange of oil producing countries would also bounce back. Does this statement really hold? 

According to the article by Bloomberg (or many other similar research), researchers examine the correlation between petrocurrency and oil price, instead of the causality. But correlation does not equal to causality. Correlation could be a coincidence of a math game. We simply cannot draw the conclusion that oil price moves the currency. Some researchers even use bootstrapping which greatly destroys the autocorrelation of a time series. Thus, it is vital to apply academic analysis and computer simulation on some petrocurrencies to test the causality of oil.  

For more details, please refer to the <a href=https://github.com/je-suis-tm/quant-trading/blob/master/Oil%20Money%20project/README.md>read me page</a> of a separate directory or <a href=https://je-suis-tm.github.io/quant-trading/oil-money>quant trading section</a> on my personal blog.

![alt text](https://github.com/je-suis-tm/quant-trading/blob/master/Oil%20Money%20project/preview/oil%20production%20bubble%20map.png)

### 7.Dual Thrust

If you search dual thrust on google, you will end up with results of rocket engine. Don't panic yet, you can rest assured that dual thrust strategy is nowhere near rocket science. It is just an opening range breakout strategy developed by the founder of Universal Technical Systems. The mathematics involved in this strategy is merely primary school level.

Initially we establish upper and lower thresholds based on previous days' open, close, high and low. When the market opens and the price exceeds certain thresholds, we would take long/short positions prior to upper/lower thresholds. The strategy is quite useful in intra daily trading. However, there is no stop loss/profit position in this strategy. We reverse our positions when the price goes from one threshold to the other. We need to clear all positions by the end of the day.

Rules of dual thrust can be found in <a href=https://www.quantconnect.com/tutorials/dual-thrust-trading-algorithm>QuantConnect</a>.

![alt text](https://github.com/je-suis-tm/quant-trading/blob/master/preview/dual%20thrust%20positions.png)

### 8.Parabolic SAR

Parabolic SAR is an indicator to identify stop and reverse of a trend. Usually, Parabolic SAR is presented as dotted line either above or below the price in charts. When the price is an uptrend, SAR curve would sit below the price. When the price is downtrend, SAR curve would rise above the price. Parabolic SAR is always considered as a symbol of resistance to the price momentum. When SAR curve and the price curve cross over, it is when trade orders are supposed to be executed. 

The building of this strategy seems very simple, but the construction of the indicator is extremely painful due to the involvement of recursive calculation. Illustration on how to compute Parabolic SAR can be found in <a href=https://en.wikipedia.org/wiki/Parabolic_SAR>Wikipedia</a> but it is not very well explained. To get a clear idea of the calculation, my personal recommendation is to take a look at the <a href=https://www.box.com/s/gbtrjuoktgyag56j6lv0>spreadsheet</a> made by joeu2004.

It is worth mentioning that SAR and RSI (which will be featured in a later chapter) shares the same founder, Welles Wilder. The guy is a real legend who used to work as mechanical engineer and real estate developer and later became a technical analyst. His book on technical trading system is a must-read for anyone that wants to elevate quant trading system to the next level.

![alt text](https://github.com/je-suis-tm/quant-trading/blob/master/preview/parabolic%20sar%20positions.png)

### 9.Bollinger Bands Pattern Recognition

Bollinger Bands is a very simple but powerful indicator. There are three bands of this indicator. The mid band is the moving average on the price series (usually takes 20 lags). The upper and lower bands are two moving standard deviations away from the mid band. Bollinfer Bands can be used to test for various types of strategies. 

For volatility trading, contraction and expansion of the band width are crucial elements. Any distinct momentum clustering (it can take form of either upward or downward) would result in a Bollinger Bands expansion. And the oscillation in a horizontal channel would result in a Bollinger Bands contraction. 

For momentum trading, the phenomenon of 'walking the band' indicates the resistance and support level of the underlying asset. In a strong trend, the price constantly attempts to touch or break through the upper/lower band along with Bollinger Bands moving towards the same direction.

For pattern recognition, Bollinger Bands has the capability of testing bottom W, top M, head-shoulder patterns, etc. With upper and lower bands served as an interval, it is easier to identify the hidden pattern in the historical data.

More details of Bollinger Bands can be found in <a href=https://www.tradingview.com/wiki/Bollinger_Bands_(BB)>TradingView</a>.

![alt text](https://github.com/je-suis-tm/quant-trading/blob/master/preview/bollinger%20bands%20positions.png)

### 10. Relative Strength Index Pattern Recognition

RSI (Relative Strength Index) is also a popular indicator. It reflects the current strength/weakness of the stock price momentum. The calculation is pretty straight forward. We use 14 days of smoothed moving average (or other moving average methods) to separately calculate the intra daily uptrend and downtrend. We denote uptrend moving average divided by downtrend moving average as the relative strength. We normalize the relative strength by 100 which becomes an index called RSI. It is commonly believed that RSI above 70 is overbought and RSI below 30 is oversold. This is the simplest way to trade on RSI (as shown in the pictures below). Nonetheless, there could be divergence between RSI momentum and price momentum which will not be covered in the script. The effectiveness of any divergence strategy on RSI is rather debatable.

![alt text](https://github.com/je-suis-tm/quant-trading/blob/master/preview/rsi%20positions.png)

![alt text](https://github.com/je-suis-tm/quant-trading/blob/master/preview/rsi%20oscillator.png)

If you are looking for something slightly more complex, well, we can apply pattern recognition technique to RSI as well. Unlike strategy No.9 Bollinger Bands, we can directly look at the patterns of RSI itself instead of the price. Since we have tested double bottom pattern in Bollinger Bands, we would test head-shoulder pattern on RSI this time.

For details of head-shoulder pattern, please refer to <a href=https://www.investopedia.com/terms/h/head-shoulders.asp>Investopedia</a>.

![alt text](https://github.com/je-suis-tm/quant-trading/blob/master/preview/rsi%20pattern%20positions.png)

![alt text](https://github.com/je-suis-tm/quant-trading/blob/master/preview/rsi%20pattern%20oscillator.png)

### 11.Monte Carlo project

Monte Carlo, my first thought on these two words is the grand casino, where you meet Famke Janssen in tuxedo and introduce yourself, 'Bond, James Bond'. Indeed, the simulation is named after the infamous casino. It actually refers to the computer simulation of massive amount of random events. This unconventional mathematical method is extremely powerful in the study of stochastic process. 

Here comes the argument on Linkedin that caught my eyes the other day. "Stock price can be seemed as a Wiener Process. Hence, we can use Monte Carlo simulation to predict the stock price." said a data science blog. Well, in order to be a Wiener Process, we have to assume the stock price is continuous in time. In reality, the market closes. The overnight volatility exists. But that is not the biggest issue here. The biggest issue is, can we really use Monte Carlo simulation to predict the stock price, even a range or its direction?

For more details, please refer to the <a href=https://github.com/je-suis-tm/quant-trading/blob/master/Monte%20Carlo%20project/README.md>read me</a> page of a separate directory or <a href=https://je-suis-tm.github.io/quant-trading/monte-carlo>quant trading</a> section on my personal blog.

![alt text](https://raw.githubusercontent.com/je-suis-tm/quant-trading/master/Monte%20Carlo%20project/preview/ge%20simulation2.png)

<br>

#### STAY TUNED
