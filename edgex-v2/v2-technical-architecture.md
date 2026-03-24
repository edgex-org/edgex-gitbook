# V2 Technical Architecture

### **Evolution from V1 Legacy**

The foundational edgeX V1 platform utilized a StarkEx-based validium engine, which successfully validated our core matching model and processed billions in initial trading volume. As the decentralized exchange landscape evolved to demand higher concurrency, lower latency, and deeper liquidity aggregation, our architecture naturally transitioned into an advanced, dedicated rollup design to serve professional and algorithmic traders effectively.

### **V2 Modular Rollup Architecture**

Currently, edgeX operates as a high-performance, modular Rollup trading protocol. By decoupling the execution layer from the settlement layers, edgeX delivers a centralized-exchange-like orderbook experience while remaining deeply rooted in the decentralized ethos of self-custody.

#### **Shared Ethereum Security**

At its core, the V2 protocol inherits the robust consensus and economic security guarantees of the Ethereum mainnet. Financial states and bulk transaction batches are securely bridged and anchored back to Ethereum. This ensures tamper-proof trade finality, total asset self-custody, and the elimination of single-point-of-failure counterparty risk, without forcing users to bear the burden of high mainnet gas costs for every trade execution.

#### **High-Performance Business Layer**

To accommodate the immense throughput required by our sophisticated matching engine, edgeX employs a modular execution layer powered by a high-concurrency microservice cluster. This isolated computational environment is dedicated entirely to high-frequency order routing, risk validation, and complex financial clearing. It effectively removes the performance bottlenecks associated with global, general-purpose smart contract networks, shielding traders from the volatility and gas spikes caused by unrelated block space congestion.

#### **The Transitional Implementation Framework**

In its current iteration, edgeX has temporarily adopted the Arbitrum technology stack as a transitional framework for our modular rollup operations. Using this mature technology stack allows us to immediately benefit from extensive EVM compatibility, established reliable bridging solutions, and a battle-tested shared layer while we aggressively fine-tune the high-performance proprietary trading infrastructure that lives above it.\
However, this architecture represents a crucial stepping stone toward our ultimate sovereign vision, equipping our engines to handle the coming age of global algorithmic execution.<br>
