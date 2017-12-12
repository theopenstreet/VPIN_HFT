# VPIN HFT

The market agents can be classified as informed and uninformed traders. As informed traders act on information, growth in their activity are sign of upcoming price change. This activity is called as flow toxicity and can be quantified with VPIN. The VPIN measure is updated in volume time and can be calculated by classifying trades as buys or sells. Along with VPIN we consider Quote Imbalance to develop a trading strategy. We show that VPIN and Quote Imbalance can be used as signals for price movement and can be used to build trading strategy. A comparative study between empirical method and different supervised learning methods is done.

## VPIN and CDF

This paper presents a strategy to take advantage of flow toxicity in the market. The Probability of Informed Trading (PIN) model was developed by Easely and O’Hara (1992) to measure toxicity in the market. Under this model, in a time window there is α chance of an information event happening and δ chance the information event will be good. With these probabilities, the expected price of the stock at the end of the time window is given by following formula.

E[S_t ]  =(1-α) S_0+α(δ_t S_B+(1-δ_t ) S_G)


Under this model the informed trades act on the information event and arrive at the rate of μ. The uninformed traders arrive at the rate of ε irrespective of information event. With this the flow toxicity can be quantified by PIN as,

PIN=  αμ/(αμ+2ε)

While the PIN metric reflects flow toxicity well, it is difficult to calculate in real life. The value depends on lot of unobservable parameters which need to be estimated by making further assumptions.

Easley, Prado and O’Hara (2012) extended the logic behind PIN to Volume Synchronized PIN (VPIN). In high frequency environment, the trades arrive at irregular frequency and the rate of arrival of trades is indicative of an information event. The market will see high trade volume when the information event is more relevant. To divide trades in equal information windows, volume bucketing is used. Instead of using fixed time windows, authors use time windows with fixed trade volume. Whenever the trade volume crosses the barrier, authors start the new time window. This means each window carries equal trade volume and indicates equal amount of information. Since each window carries equal information, it becomes much easier to compare flow toxicity in each window.

With volume bucketing the authors extend PIN to a practical metric Volume Synchronized Probability of Informed Trading (VPIN). VPIN can be calculated using just the trades data. While the overall increase in volume level suggests occurrence of an information event, the direction of volume change indicates the type of the information. If the new information is good, there will be more buy volume compared to sell volume and vice versa. This leads to the next problem of classifying volume as buy volume or sell volume. To estimate buy and sell volume the authors use change in stock price over the window. Increase in the price will indicate higher buy volume and decrease in the price will indicate higher sell volume. The gap between buy and sell volume will be higher if the price change is significant. But since we are using volume bucketing, checking only start and end price will ignore the price path. To handle that the authors divide each window in small time intervals, then divide volume in each small window and then aggregate it. The authors use normal distribution to estimate the split between buy and sell volume.

V_τ^B= ∑_(i=t(τ-1)+1)^t(τ)▒〖V_i  .Z((P_i-P_(i-1))/σ_ΔP ) 〗
V_τ^S=V-V_τ^B

Here Z is the CDF of normal distribution and σ_ΔP is the estimate of standard deviation of the price change between time bars.
The estimate of buy volume and sell volume can be used to calculate VPIN. From formula of PIN, we know αμ represents the informed trading activity. As informed trading is always based on some information, it is directional in nature and difference between buy volume and sell volume reflects it. So the authors quantify VPIN as follows,

VPIN=  αμ/(αμ+2ε)≈  (∑_(τ=1)^n▒〖|V_B^τ-V_S^τ |〗)/nV

Here, the trade imbalance is aggregated over n volume buckets with each bucket comprising of V trades. The VPIN is updated after every new volume bucket with trailing n volume buckets.
While VPIN quantifies the toxicity well, the authors realized that its absolute value does not signify much information. While studying E-mini S&P 500 future around the 2010 flash crash the authors found that the Cumulative Distribution Density (CDF) of VPIN was high hours before the crash. Figure [1] shows the VPIN on that day remained between 0.2 to 0.4, which does not relay much information as it is difficult to understand a new metric like VPIN. But the CDF of VPIN went from 0.40 in morning to 0.95 couple of hours before the crash.

## QUOTE IMBALANCE

VPIN and its CDF are good at quantifying toxicity in the market. But since both are on absolute scale, it fails to give direction of impending market movement. For that we considered Quote Imbalance. Different agents in the market view market differently. All agents place orders at different prices on buy and sell side. As trades happen, these orders are cleared with liquidity from market makers. All the market makes are constantly observing the order flow. Market makers tend to provide more liquidity on the side they expect the market to move. If they expect the price to rise, they will provide more liquidity on the bid side to buy stock before price increases. Quote Imbalance tries to quantify the difference in bid and ask quote volume.
Most of the times, price change takes place across all exchanges. Some exchanges lag others but if price changes on only one exchange, it is mostly because of some glitch. Also, lot of times the stocks in one industry are positively correlated. These stocks move in separate direction only when the news is specific to one stock. Quarterly earnings, changes in leadership are an example of that. But on rest of the days, the stocks move together. When there is similar shift in market across exchanges and across assets, the price change is almost certain.

## CROSS-EXCHANGES and CROSS-ASSET CORRELATION

To measure relation across exchanges and across assets, we looked at toxicity at all exchanges and similar stocks. The VPIN needs to be calculated with different volume buckets for different exchanges and stocks. The VPIN needs to be updated at about same speed. We decided to divide the volume at all exchanges and assets such as to get equal buckets on regular day. Average daily trading volume is very useful to decide volume buckets for each exchange and stock. As Easley, Prado and O’Hara (2012) realized that CDF of VPIN is a better indicator than actual VPIN itself, we also compared the CDF of VPIN at different exchanges and for different stocks. We decided to calculate average rolling correlation of CDF across the exchanges and across the stocks. These two numbers will increase when there is higher correlation between the underlying CDFs.
