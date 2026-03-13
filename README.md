##Trader Performance vs Market Sentiment

##Data Science Analysis — Primetrade.ai Internship Evaluation
Author: Haseeba

#Project Objective

This project analyzes how Bitcoin market sentiment influences trader behavior and profitability on Hyperliquid. By combining sentiment indicators with historical trade execution data, the analysis identifies behavioral patterns, evaluates sentiment-driven performance differences, and proposes sentiment-aware trading strategies.

#The workflow includes:
data preparation and feature engineering
exploratory and statistical analysis
trader segmentation
predictive modeling
strategy recommendations based on observed patterns

## 🚀 Quick Start

### 1. Clone / Download this repo
```bash
git clone <-repo-url>
cd 
```

### 2. Install dependencies
```bash
pip install pandas numpy matplotlib seaborn scipy scikit-learn jupyter
```

### 3. Download the datasets
| Dataset | Link |
|---------|------|
| Bitcoin Sentiment (Fear/Greed) | [Google Drive](https://drive.google.com/file/d/1PgQC0tO8XN-wqkNyghWc_-mnrYv_nhSf) |
| Hyperliquid Trader Data | [Google Drive](https://drive.google.com/file/d/1IAfLZwu6rJzyWKgBToqwSmmVYU6VbjVs) |

Place them in the `data/` folder and rename:
- Sentiment file → `data/sentiment.csv`
- Trader file → `data/trades.csv`

### 4. Run the notebook
```bash
jupyter notebook notebooks/trader_sentiment_analysis.ipynb
Kernel → Restart & Run All
Python 3.8+ recommended.
All analysis runs on CPU (no GPU required).

##Repository Structure
|--charts
|--notebooks/
├── trader_sentiment_analysis.ipynb   # Full analysis notebook
├── README.md                         # Project documentation
├── writeup.md                        # 1-page summary (methodology + insights)
|--data/
├── fear_greed_index.csv              # Bitcoin Fear/Greed sentiment dataset
└── historical_data.csv               # Hyperliquid trader execution dataset

##Datasets
Dataset	                       Description	                            Key Columns
fear_greed_index.csv	       Daily Bitcoin sentiment classification	date, value, classification
historical_data.csv	           Hyperliquid trade execution history	    account, closed_pnl, side, size_usd, timestamp, fee

##Analysis Workflow
#Data Preparation

The datasets were cleaned and aligned at the daily level. Steps included:
timestamp parsing and normalization
removal of null PnL values and duplicate rows
fee normalization
merging sentiment classifications with trader activity

Trader-level metrics were then engineered, including:
net daily PnL
win rate
trade frequency
average position size
long/short ratio
drawdown proxy

#Performance Analysis

Trader performance was compared across Fear, Neutral, and Greed sentiment regimes using summary statistics and Welch’s t-tests to evaluate regime differences.

Behavioral Analysis

Trader behavior was examined to understand how sentiment affects:
trading frequency
directional bias (long vs short)
position sizing

#Trader Segmentation

Three segmentation approaches were used:

Position Size Segmentation
High-size vs low-size traders

Trade Frequency Segmentation
High-frequency vs low-frequency traders

Performance Segmentation
Consistent Winners vs Consistent Losers

These segments help identify which trader profiles benefit most from different sentiment environments.

##Predictive Modeling

A Gradient Boosting classifier (using a Scikit-learn pipeline) was trained to explore whether trader behavior and sentiment features could predict next-day profitability.

#Features included:
trade count
average position size
sentiment regime
directional bias
win rate

Results indicate that trader activity levels are strongly associated with short-term profitability patterns.

##Behavioral Clustering
K-Means clustering (k=4, selected using the elbow method) was used to identify distinct trader archetypes. PCA visualization was applied to interpret cluster separation and behavioral characteristics.

#Key Findings

Fear regimes show higher overall trader profitability, with median PnL approximately 2.2× higher than Greed regimes.

Position size amplifies sentiment sensitivity — high-size traders experience much larger performance swings across sentiment regimes.

Trader activity is strongly linked to short-term profitability, suggesting that sentiment-driven trading intensity plays an important role in outcomes.

Consistent Winners maintain positive performance across regimes, while weaker traders are more vulnerable to sentiment-driven volatility.

##Strategy Recommendations
Strategy	                        Trigger	            Rule
Sentiment-Based Position Size Cap	Fear / Extreme Fear	Limit position size for lower-performing traders to ≤50% of trailing 
                                                        exposure
Activity Moderation	                Fear regimes	    Reduce excessive trade frequency for weaker traders
Contrarian Long Strategy	        Extreme Fear	    Consistent Winners may selectively deploy long positions with strict                                                           risk controls
Full reasoning is detailed in writeup.md.

#Limitations

This analysis is based on historical trade execution data and sentiment classification. External factors such as macroeconomic news, liquidity changes, and broader market volatility were not incorporated. Future work could include additional market indicators to provide a more comprehensive model of trader behavior.

Author
Haseeba
Data Science Student