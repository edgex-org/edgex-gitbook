---
hidden: true
---

# edgeX V2

## edgeX V2 Overview <a href="#user-content-edgex-v2-overview" id="user-content-edgex-v2-overview"></a>

edgeX V2 represents the next evolution of our high-performance, orderbook-based decentralized exchange (DEX). Transitioning from our successful V1 foundation, edgeX V2 has been entirely re-architected as a dedicated **Modular Rollup**. By decoupling the high-frequency execution layer from the underlying settlement layer, edgeX V2 delivers an institutional-grade, centralized exchange (CEX) native trading experience without compromising the core ethos of decentralized self-custody.

### Commitment to the Next Generation of Trading <a href="#user-content-commitment-to-the-next-generation-of-trading" id="user-content-commitment-to-the-next-generation-of-trading"></a>

Recognizing that both human traders and algorithmic agents are the foundation of edgeX’s sustainable growth, the V2 platform is built to handle the extreme concurrency and precision demanded by modern financial markets. The guiding principle remains: “Build an infrastructure that professionals and algorithms trust.”

The V2 architecture underscores edgeX’s standard of excellence across several key areas:

* **Modular Rollup Architecture**: By migrating to a modular Rollup framework, edgeX V2 operates an isolated, high-performance execution environment. This completely shields traders from the gas spikes and network congestion typically associated with general-purpose Layer 1 blockchains, ensuring consistent, ultra-low latency order matching.
* **Shared Ethereum Security**: edgeX V2 inherits the uncompromising consensus and economic security of the Ethereum mainnet. The platform batches transactions and anchors definitive state roots directly to Ethereum Layer 1, guaranteeing absolute data integrity and the immutability of every executed trade.
* **Absolute Self-Custody**: In stark contrast to centralized exchanges where collateral is held in opaque company wallets, edgeX V2 enforces strict self-custody via transparent, audited smart contracts. Users retain full cryptographic control over their assets with their private keys, allowing for trustless, permissionless withdrawals at any time—even in the event of an operator sequence failure.
* **Institutional Price Integrity**: To safeguard users from isolated "scam wicks" or localized liquidity crunches, edgeX V2 integrates a native Multi-CEX Oracle aggregation pipeline. By streaming and intelligently weighting real-time data from the world's deepest centralized exchanges, the system guarantees an equitable, manipulation-resistant mark price for all margin and liquidation calculations.
* **Unprecedented Trading Performance**: The heart of edgeX V2 is a newly engineered, Go-based microservice matching engine. Operating entirely independent of slow on-chain block times, this deterministic execution layer processes heavy order flow with microsecond latency, providing the absolute fastest time-to-execution in the decentralized derivatives space.
* **Agent-Native APIs & Advanced Features**: edgeX V2 steps beyond standard UI trading to offer highly optimized, Agent-Native API endpoints for programmatic traders and AI models. Combined with professional-grade features like a unified cross-margin sub-account system, traders can seamlessly manage bi-directional hedge positions and execute complex arbitrage matrixes.
* **Seamless Cross-Chain Onboarding**: Through integrations with cutting-edge MPC wallets and native bridging interfaces, edgeX V2 maintains the easiest onboarding funnel in Web3. Traders can spin up non-custodial wallets via simple social logins and instantly bridge collateral from multiple ecosystems into the Rollup securely.

### The Path Forward <a href="#user-content-the-path-forward" id="user-content-the-path-forward"></a>

edgeX V2 is more than just an upgrade; it is the crucial transitional framework paving the way for the ultimate **EDGE Stack (V3)**—a sovereign AppChain designed specifically as the financial execution layer for the autonomous agent economy.
