# VPIN HFT

The market agents can be classified as informed and uninformed traders. As informed traders act on information, growth in their activity are sign of upcoming price change. This activity is called as flow toxicity and can be quantified with VPIN. The VPIN measure is updated in volume time and can be calculated by classifying trades as buys or sells. Along with VPIN we consider Quote Imbalance to develop a trading strategy. We show that VPIN and Quote Imbalance can be used as signals for price movement and can be used to build trading strategy. A comparative study between empirical method and different supervised learning methods is done.

## VPIN and CDF

This paper presents a strategy to take advantage of flow toxicity in the market. The Probability of Informed Trading (PIN) model was developed by Easely and O’Hara (1992) to measure toxicity in the market. Under this model, in a time window there is α chance of an information event happening and δ chance the information event will be good. Under this model the informed trades act on the information event and arrive at the rate of μ. The uninformed traders arrive at the rate of ε irrespective of information event. With this the flow toxicity can be quantified by PIN as

PIN=  αμ/(αμ+2ε)

While the PIN metric reflects flow toxicity well, it is difficult to calculate in real life. The value depends on lot of unobservable parameters which need to be estimated by making further assumptions.

Easley, Prado and O’Hara (2012) extended the logic behind PIN to Volume Synchronized PIN (VPIN). In high frequency environment, the trades arrive at irregular frequency and the rate of arrival of trades is indicative of an information event. The market will see high trade volume when the information event is more relevant. To divide trades in equal information windows, volume bucketing is used. Instead of using fixed time windows, authors use time windows with fixed trade volume. Whenever the trade volume crosses the barrier, authors start the new time window. This means each window carries equal trade volume and indicates equal amount of information. Since each window carries equal information, it becomes much easier to compare flow toxicity in each window.

With volume bucketing the authors extend PIN to a practical metric Volume Synchronized Probability of Informed Trading (VPIN). VPIN can be calculated using just the trades data. While the overall increase in volume level suggests occurrence of an information event, the direction of volume change indicates the type of the information. If the new information is good, there will be more buy volume compared to sell volume and vice versa. This leads to the next problem of classifying volume as buy volume or sell volume. To estimate buy and sell volume the authors use change in stock price over the window. Increase in the price will indicate higher buy volume and decrease in the price will indicate higher sell volume. The gap between buy and sell volume will be higher if the price change is significant. But since we are using volume bucketing, checking only start and end price will ignore the price path. To handle that the authors divide each window in small time intervals, then divide volume in each small window and then aggregate it. The authors use normal distribution to estimate the split between buy and sell volume.

The estimate of buy volume and sell volume can be used to calculate VPIN. From formula of PIN, we know αμ represents the informed trading activity. As informed trading is always based on some information, it is directional in nature and difference between buy volume and sell volume reflects it. 

The trade imbalance is aggregated over n volume buckets with each bucket comprising of V trades. The VPIN is updated after every new volume bucket with trailing n volume buckets.
While VPIN quantifies the toxicity well, the authors realized that its absolute value does not signify much information. While studying E-mini S&P 500 future around the 2010 flash crash the authors found that the Cumulative Distribution Density (CDF) of VPIN was high hours before the crash. 

## QUOTE IMBALANCE

VPIN and its CDF are good at quantifying toxicity in the market. But since both are on absolute scale, it fails to give direction of impending market movement. For that we considered Quote Imbalance. Different agents in the market view market differently. All agents place orders at different prices on buy and sell side. As trades happen, these orders are cleared with liquidity from market makers. All the market makes are constantly observing the order flow. Market makers tend to provide more liquidity on the side they expect the market to move. If they expect the price to rise, they will provide more liquidity on the bid side to buy stock before price increases. Quote Imbalance tries to quantify the difference in bid and ask quote volume.
Most of the times, price change takes place across all exchanges. Some exchanges lag others but if price changes on only one exchange, it is mostly because of some glitch. Also, lot of times the stocks in one industry are positively correlated. These stocks move in separate direction only when the news is specific to one stock. Quarterly earnings, changes in leadership are an example of that. But on rest of the days, the stocks move together. When there is similar shift in market across exchanges and across assets, the price change is almost certain.

## CROSS-EXCHANGES and CROSS-ASSET CORRELATION

To measure relation across exchanges and across assets, we looked at toxicity at all exchanges and similar stocks. The VPIN needs to be calculated with different volume buckets for different exchanges and stocks. The VPIN needs to be updated at about same speed. We decided to divide the volume at all exchanges and assets such as to get equal buckets on regular day. Average daily trading volume is very useful to decide volume buckets for each exchange and stock. As Easley, Prado and O’Hara (2012) realized that CDF of VPIN is a better indicator than actual VPIN itself, we also compared the CDF of VPIN at different exchanges and for different stocks. We decided to calculate average rolling correlation of CDF across the exchanges and across the stocks. These two numbers will increase when there is higher correlation between the underlying CDFs.

## EMPIRICAL STARTEGY

In order to create a trading strategy based on VPIN, we must analyse how different market participants react to toxicity in order flows. As has been discussed in the earlier sections, high VPIN indicates that there is a high probability of informed trading in the market. As such, the VPIN does not provide any information on the direction in which the asset price moves but that there will  be a significant move. 

Let us assume for now that we know that there is a positive news about the asset. The informed traders submit a large number of buy orders which will push the VPIN higher by increasing the flow toxicity. The market makers would then respond by changing the structure of liquidity by providing more liquidity on the bid side (posting higher bid quote volume) in order to secure a net long position. Therefore, looking at high quote imbalance in conjunction with VPIN can provide a good short-term buy/sell signal.
There are a couple of caveats in deriving this signal. 

First, we must make sure that the high VPIN signals are not arising due to liquidity bottlenecks or arbitrage between individual exchanges. Since VPIN calculations are tied to trade volume imbalances in an asset; It could be possibly that we might get high VPIN signals when liquidity flows between exchanges due to their structure and characteristics. These are more precisely captured by models dealing with Market structure and design issues as well as the process of Information and disclosure. 

To make sure that our VPIN signal is indeed a signal for high flow toxicity, we must use the VPIN CDF average correlation metric between exchanges for a particular asset as derived in the previous section. This metric when used with the VPIN CDF tells us that the high flow toxicity is a pan exchange phenomenon. 

Second, we must also eliminate the idiosyncratic effects in the asset leading to high VPIN that again may arise due to structural aspects related to the assets trading activity and are not representative of an increase in flow toxicity. For this we use the multi asset VPIN average correlation metric as described in the earlier section. When used in tandem with the VPIN CDF metric; we would only isolate industry wide information signals while having bestowed greater confidence on VPIN signal.

The directional information of the asset is given by the quote imbalance metric, which reports the average difference between bid volume and ask volume across exchanges. As discussed above, when there is high quote imbalance, the market makers are trying to secure a positive position in the market and therefore, together with a high VPIN metric, this would be buy signal. Since, the updates on the VPIN metric are volume dependent and not time dependent, it also makes sense to create a strategy that whose holding period is dependent on arrival of new information from the VPIN.

Pre-processing: First we calculate the VPIN CDF, exchange VPIN average correlation and VPIN assets VPIN average correlation metrics as described in the previous sections. We then expand this data to 1 min frequency. We filter the data between 8 AM and 4 PM since VPIN metrics are updated very slowly before open and after close. 

We calculate z-scores by taking the rolling average and rolling standard deviation using a window of previous 100 mins making sure that there is no look ahead. We also winsorize the z-scores for each of the metrics.

When the VPIN CDF metric is greater than 0.5 and has high correlation among assets and exchanges, we are fairly certain that it is a flow toxicity signal. We then check if the quote imbalance is greater than 1.5 standard deviations, which is a buy signal. The death count makes sure that the long position expires within 10 periods (10 min) if there are no further buy signals. We treat the short positions in a similar manner. This way the holding period is variable and potentially long term if we get repeating signals in one direction. When the quote imbalance z-score is no longer > 1.5, that means that the market makers are willing to cover their positions by supplying liquidity and posting asks, therefore we will no longer get a buy signal and the algorithm will cover the long position in 10 periods. The logic for the sell signal follows in a similar manner.


## ALGORITHM

1)	Check if the VPIN CDF z-score > 0.5 and (Exchange Corr z-score + Asset Corr z-score) > 4:

            a.	If Quote Imbalance z-score > 1.5:

                  i.	Cover all short positions

                  ii.	Add to long position

                  iii.	Reset long death count to 10 periods

            b.	If Quote Imbalance z-score <- 1.5:

                  i.	Cover all long positions

                  ii.	Add to short position

                  iii.	Reset short death count to 10 periods

2)	If death count reaches 0, exit long/ short position.

