# Write-Up: Trader Performance vs Market Sentiment
**Analyst:** Haseeba | **Role:** Data Science Intern — Primetrade.ai

---

#Methodology 

Two datasets were merged at a daily level: the Bitcoin Fear/Greed Index (daily sentiment classification) and Hyperliquid historical trade executions. After basic cleaning (removing null PnL rows, deduplicating records, parsing unix-millisecond timestamps, and normalizing trading fees), sentiment classifications were grouped into three regimes — Fear, Neutral, and Greed — to ensure adequate sample sizes for comparison.

Daily trader-level metrics were engineered, including net PnL (after fees), win rate, trade count, average position size (USD), long/short ratio, and a drawdown proxy defined as the largest single-trade loss per day. Traders were segmented in three ways: by position size (median split), by trading frequency (median split), and by historical performance tier. Consistent Winners were defined as traders with both a top-quartile win rate and positive cumulative PnL, while Consistent Losers were traders with bottom-quartile win rates or negative cumulative PnL.

Differences between sentiment regimes were evaluated using Welch’s t-tests to account for unequal variance between groups. To explore predictive signals, a Gradient Boosting classifier was trained to estimate next-day profitability using trader behavior features. The modeling pipeline used imputation, scaling, and feature selection to maintain consistency. Finally, K-Means clustering (k = 4 selected via the elbow method) was applied to identify behavioral trader archetypes, with PCA used for visualization.

#Insights 
Insight 1 — Trader performance improves during Fear regimes

Median daily PnL during Fear regimes is approximately $83,311, compared with $37,160 during Greed regimes, indicating roughly 2.2× higher profitability during Fear conditions. Win rate also increases substantially (88.7% during Fear vs 54.6% during Greed). Trading activity is similarly elevated, with Fear regimes averaging 2,016 trades per day compared to 731 during Greed.

These patterns suggest that volatility-rich Fear environments create more trading opportunities, allowing experienced traders to capitalize on price dislocations.

Insight 2 — Position size amplifies sentiment sensitivity

High-size traders show significantly stronger performance variation across sentiment regimes. During Fear periods, high-size traders achieve median profits near $140,000 per day, while performance drops to roughly $8,000 during Greed regimes. In contrast, low-size traders show relatively stable performance across regimes (approximately $42–48K daily median PnL).

This suggests that larger position sizes amplify exposure to sentiment-driven market volatility, making performance more sensitive to market conditions.

Insight 3 — Trading activity is strongly associated with next-day profitability

In the Gradient Boosting model, trade count emerges as the most influential feature for predicting next-day profitability relative to other behavioral metrics. Fear regimes also correspond with significantly higher trading activity, averaging 5.7× more trades than Neutral regimes.

This relationship suggests that market sentiment influences trader activity levels, and elevated activity may be associated with short-term profitability patterns.

#Strategy Recommendations
S1 — Sentiment-adjusted position sizing

Trigger: sentiment ∈ {Fear, Extreme Fear}

During Fear regimes, limit position sizes for lower-confidence traders to ≤ 50% of their trailing 30-day average exposure. Traders classified as Consistent Winners may operate without this cap.

Rationale: High-size traders generate the largest PnL dispersion during volatile Fear environments. Moderating exposure for less consistent traders can reduce downside risk while allowing skilled traders to capture volatility-driven opportunities.

S2 — Activity moderation during high-volatility regimes

Trigger: Fear sentiment regimes

Although Fear regimes show the highest aggregate profitability, they also produce the largest variance in outcomes. Limiting excessive trade frequency for lower-performing traders may reduce losses caused by impulsive trading behavior during volatile periods.

Consistent Winners may continue trading at normal activity levels.

S3 — Opportunistic contrarian entries during extreme fear

Trigger: sentiment = Extreme Fear

Consistent Winners may selectively increase long exposure during Extreme Fear regimes with strict stop-loss controls. Other traders should maintain conservative positioning.

Rationale: Extreme Fear environments often correspond with oversold market conditions, creating potential mean-reversion opportunities for disciplined traders.



Limitations

This analysis is based solely on historical trade execution data and sentiment classification. Other factors such as macroeconomic news, market liquidity changes, and cross-asset volatility were not incorporated. Future work could integrate additional market indicators to better capture broader trading conditions.