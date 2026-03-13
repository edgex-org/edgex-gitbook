# Stock Perpetual

Before you start trading Stock Perpetuals, please read the following trading rules carefully. These contracts are linked to underlying equity assets. During weekends and market holidays when the stock markets are closed, the trading mechanisms will be adjusted as follows:



#### 1. Trading Hours & Market Status

* 24/7 Trading: The perpetual market remains open 24/7, but price and volatility are significantly influenced by the status of the underlying stock market.
* Market Status: The system determines the current status as "Market Open" or "Market Closed" in real-time, based on the official trading schedule of the relevant stock markets.



#### 2. Special Rules During Weekends & Holidays

**2.1 Order Types**

To protect your assets from abnormal price fluctuations caused by low liquidity when the stock market is closed, the following rules apply:

* Market Orders Rejected: The system will reject all Market Orders.
* Limit Orders Only: Only Limit Orders are supported, and the limit price must fall within the designated price range.
* Conditional Orders: All Conditional Orders (including Take Profit & Stop Loss (TP/SL), Conditional Limit Orders, and Conditional Market Orders) triggered during market closed period must follow the same rules: Market orders will be rejected, and Limit orders outside the price range will also be rejected.

**2.2 Price Range & Mark Price**

Price range Calculation:

* Formula: Long price upper limit is Last Closing Index Price × (1 + 1 / Max Leverage of the pair); Short price lower limit is Last Closing Index Price × (1 - 1 / Max Leverage of the pair).
* _Example: If the maximum leverage for a perp trading pair is 10x and the last closing Index Price is $100, buy limit orders cannot exceed $110 and sell limit orders cannot be below $90 during market closure._

Mark Price Adjustment:

* During market closure, the Mark Price is also restricted within the price range.
* Clamping Mechanism: The price change is limited to 0.5% every 3 seconds.



#### 3. Risk Disclosure

* Order Execution Risk: Orders placed or triggered during weekends or holidays may be rejected if they do not meet the criteria mentioned above.
* Price Deviation Risk: During market status transitions or periods of extreme volatility, the contract price may temporarily deviate from the external spot price.
* Liquidation Risk: If your margin is insufficient, you may face the risk of forced liquidation
