# Decentralized Oracle Pricing

### The Power of Native Multi-CEX Aggregation

edgeX utilizes a proprietary, natively aggregated **Multi-CEX Oracle framework** to obtain independent and ultra-precise price data. This system powers our Index Prices, calculates margin requirements, and establishes exact liquidation triggers.If the calculated Oracle Price reaches an account's liquidation threshold, the position will undergo forced liquidation. Maintaining an absolutely impenetrable oracle system is crucial for securing the integrity of the trading platform and ensuring that edgeX traders are resistant to flash crashes and market manipulation.

#### The Challenge of Malicious Flash Crashes

In the cryptocurrency market, centralized exchanges (CEXs) and simplistic on-chain AMMs can occasionally suffer from malicious "flash crashes" or “scam wicks.” These are sudden, abnormal surges or drops in asset prices unique to a single trading platform caused by thin liquidity, algorithmic errors, or targeted manipulation.If a single platform's price feed is used blindly, traders’ stop-loss orders can trigger unexpectedly, leading to unwarranted forced liquidations and financial loss.

#### How the edgeX Native Oracle Prevents Manipulation

Unlike older decentralized architectures that rely on slow third-party publisher networks (like Chainlink or [Stork](https://www.stork.network/) with varying on-chain latency), edgeX has integrated the oracle pricing actively into its core high-performance infrastructure.

1. **Institutional Multi-Exchange Data Sourcing**

To ensure precision, edgeX maintains redundant, low-latency WebSocket connections directly to the top 5 centralized exchanges globally (e.g., Binance, OKX, Bybit, Gate, MEXC). By aggregating order book and tick data from the world's deepest liquidity pools simultaneously, edgeX drastically reduces the risk of relying on a single, potentially compromised platform.

2. **Stake-Weighted Median Algorithm**

Raw prices are not just averaged; they pass through our deterministic Stake-Weighted Median algorithm.

* Exchanges with deeper liquidity and higher historical reliability are assigned heavier weighting parameters (e.g., Binance: 3, OKX/Bybit: 2, etc.).
* If a localized flash crash occurs on a single exchange, the mathematical median automatically filters out the anomaly, protecting edgeX traders from being liquidated by another exchange's lack of liquidity.

3. **High-Performance Backend Aggregation**

Unlike decentralized oracle networks that wait for periodic on-chain contract updates, the edgeX Multi-CEX prices are synthesized natively within our high-performance backend architecture. A dedicated Go-based microservice continually aggregates and verifies the WebSocket feeds, streaming the finalized Oracle Price directly into the core matching engine's memory. This ensures the pricing engine operates with the precise sub-second real-time latency of the exchange itself, providing the fairest possible cross-margin liquidations without the bottleneck of on-chain data propagation.

#### edgeX’s Commitment to Fair Trading

By internalizing a highly-sophisticated Multi-CEX data aggregation pipeline and filtering out market noise through weighted medians, edgeX ensures accurate, tamper-proof pricing. This guarantees a level playing field for both retail and institutional traders, completely eliminating opportunities for isolated price manipulation.<br>
