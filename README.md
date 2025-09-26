# CrossGuard - Reactive Cross-Chain Liquidity Sentinel & Auto-Balancer
CrossGuard watches on-chain liquidity and price events across DEXs and chains and automatically triggers safe rebalancing actions (e.g., move liquidity, rebalance pools, and open hedges) through Reactive Smart Contracts (RSCs) so pools and AMMs remain liquid and arbitrage is reduced — indefinitely, as long as the system is funded with REACT

https://poe.com/CrossGuard

https://gemini.google.com/share/eb902202a5fd

https://vimeo.com/1121830995

Mission of CrossGuard:
Our mission is to protect and stabilize on-chain liquidity by deploying autonomous, reactive smart contracts that monitor, detect, and rebalance in real time.
We aim to eliminate the inefficiencies of centralized keepers, reduce arbitrage extraction, and ensure that every user, trader, and protocol has access to fair and resilient liquidity — across chains, at all times.

Vision of CrossGuard:
CrossGuard envisions a self-sustaining, trustless liquidity defense layer for decentralized finance, where markets remain liquid, efficient, and fair without reliance on centralized keepers or manual interventions.
By embedding Reactive Smart Contracts (RSCs) into the core of liquidity infrastructure, CrossGuard ensures that AMMs, vaults, and cross-chain pools are always protected against volatility, arbitrage extraction, and liquidity drain.
Our long-term vision is to establish autonomous, DAO-governed liquidity sentinels that operate indefinitely, powered by REACT tokens, securing the backbone of DeFi and enabling a future where users and protocols alike can trust liquidity to always be available — safely and fairly.

🛡️ CrossGuard
Reactive Cross-Chain Liquidity Sentinel & Auto-Balancer
🌐 Overview
CrossGuard is an on-chain liquidity defense system that continuously monitors price and liquidity across DEXs and chains. When imbalances occur, it automatically triggers Reactive Smart Contracts (RSCs) to protect liquidity pools by:
Rebalancing pools
Moving liquidity to safer venues
Opening hedges against volatility
Powered by the REACT token, CrossGuard ensures that AMMs and vaults remain liquid indefinitely, reducing arbitrage extraction, slippage, and failed trades.
🚨 Problem
Liquidity shocks cause slippage, MEV extraction, and failed trades.
Current automation relies on off-chain keepers and centralized bots, which are slow, exploitable, and unsustainable.
✅ Solution
CrossGuard introduces Reactive Smart Contracts (RSCs) that:
Continuously watch liquidity + price feeds across DEXs and chains.
Automatically trigger protective actions when risks appear.
Operate indefinitely as long as fueled with REACT tokens.
🔑 Key Features
Autonomous Protection — No centralized keepers.
Cross-Chain Coverage — Multi-chain liquidity defense.
Reduced Arbitrage & MEV — Markets remain fair and efficient.
Composable — Works with AMMs, vaults, LSTs, and LP managers.
Perpetual Operation — As long as funded, it never stops.
🧩 Architecture
Watch — Monitor liquidity and price feeds across chains.
Detect — Identify shocks, imbalances, and arbitrage risk.
React — Trigger RSCs to rebalance or protect liquidity.
Rebalance — Restore safe pool states and reduce risk.
(See included flow diagram in /docs/assets/flow.png)
🔗 Token Utility (REACT)
Fuels RSC execution costs.
Provides governance for liquidity defense strategies.
Aligns incentives for sustainable protection.
🚀 Roadmap
 MVP: Single-chain prototype with RSC monitoring.
 Cross-chain liquidity sentinel deployment.
 DAO-governed strategy guardrails.
 Ecosystem integrations with major AMMs and vault protocols.
🛠️ Getting Started
Prerequisites
Node.js v18+
Hardhat / Foundry
RPC endpoints for EVM-compatible chains
Installation
git clone https://github.com/<your-org>/CrossGuard.git
cd CrossGuard
npm install
Compile & Test Contracts
npx hardhat compile
npx hardhat test
🤝 Contributing
We welcome contributions! Please open issues and PRs, or join our discussions to shape liquidity defense standards in DeFi.
📜 License
MIT License. See LICENSE for details.

<img width="1536" height="1024" alt="IMG_1284" src="https://github.com/user-attachments/assets/0cff5141-f4bc-4617-8f9a-09945b1afe29" />


What it does:
CrossGuard continuously monitors liquidity and price conditions across decentralized exchanges (DEXs) and chains. When volatility or imbalance is detected, it triggers Reactive Smart Contracts (RSCs) to automatically execute protective actions:
Rebalance pools to prevent depletion
Move liquidity to safer venues
Open hedges against exposure
Why it matters:
Keeps pools and AMMs liquid at all times
Reduces arbitrage extraction and MEV
Minimizes slippage and failed trades
Operates indefinitely, as long as fueled with REACT tokens
Core Innovation:
CrossGuard is the first on-chain, self-sustaining liquidity defense system. Instead of relying on centralized keepers or manual interventions, it uses RSCs that “react” instantly and autonomously to market conditions.

CrossGuard watches on-chain liquidity and price events across DEXs and chains and automatically triggers safe rebalancing actions (e.g., move liquidity, rebalance pools, and open hedges) through Reactive Smart Contracts (RSCs) so pools and AMMs remain liquid and arbitrage is reduced — indefinitely, as long as the system is funded with REACT for callbacks.
Why this use case needs Reactive Smart Contracts
Liquidity imbalances and oracle price shocks are time-sensitive. Traditional smart contracts are passive; external keepers or bots must poll and act. This centralizes execution and increases latency.
Reactive Smart Contracts let you subscribe to on-chain events (e.g., liquidity changed, price crossed threshold) and have the RSC call back to a rebalancer contract deterministically and automatically, paying for gas via REACT. That gives low-latency, decentralized, incentiveed automation with on-chain accountability — impossible to do robustly with passive contracts and off-chain bots.
Primary beneficiaries: DEXs, LPs, cross-chain bridges, and DeFi infrastructure teams that want automated, trust-minimized rebalancing and liquidity preservation.
Why it’s durable: The RSC architecture can run indefinitely if funded with REACT, which pays for callback gas. Incentives (small fee on rebalances / sponsor staking) keep the process funded.
High-level architecture
Origins — Onchain events come from DEXs and oracles (e.g., Uniswap pool events, Chainlink price feed events). Example origin txs: liquidity add/remove, large swap, TWAP breach.
Reactive Smart Contract (RSC) — Subscribes to origin events and triggers callbacks when conditions meet rules (thresholds, TVL % change, price delta). The RSC is deployed on Reactive network and pays for callbacks.
Destination (Rebalancer) contract(s) — Destination contracts receive callback transactions from the RSC and execute safe rebalancing strategies:
Move small portion of collateral to deficit chain/pool via approved bridge
Rebalance pool positions with multi-step atomic actions
Trigger hedges in derivatives protocol
Offchain operator / UI — Dashboard showing alerts, history, manual override (signed txs) for emergency operations. Optional: front-end dApp for sponsors to top-up REACT and monitor health.
Funds & Economics — Sponsors deposit REACT to a “callback fund” or the project holds REACT to pay for callbacks. Small fees on rebalancing transactions go to the funding pool.
Reactive-specific behavior (how RSC is used)
Subscriptions: RSC subscribes to Origin contracts’ events (e.g., Sync in Uniswap V2, Transfer events on LP tokens, Chainlink price feed AnswerUpdated) via Reactive network subscription API.
Conditions & Filters: RSC contains logic to only trigger on significant events (>= X% TVL drop or price moves beyond Y% within window).
Callbacks: When triggered, RSC calls back into the Destination contract’s entrypoint onReactiveTrigger(bytes calldata payload) with details.
Payment for callbacks: The RSC pays REACT for each callback (Reactive economic model). A sponsor address funds the RSC; the RSC keeps a credit balance or withdraws from a funding contract.
Dual-state ReactVM: Use ReactVM to read both blocks and RSC internal state to compute threshold actions in a gas-efficient way.
