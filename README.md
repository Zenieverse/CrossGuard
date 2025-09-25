# CrossGuard
CrossGuard watches on-chain liquidity and price events across DEXs and chains and automatically triggers safe rebalancing actions (e.g., move liquidity, rebalance pools, and open hedges) through Reactive Smart Contracts (RSCs) so pools and AMMs remain liquid and arbitrage is reduced — indefinitely, as long as the system is funded with REACT



CrossGuard — Reactive Cross-Chain Liquidity Sentinel & Auto-Balancer
One-sentence: CrossGuard watches on-chain liquidity and price events across DEXs and chains and automatically triggers safe rebalancing actions (e.g., move liquidity, rebalance pools, and open hedges) through Reactive Smart Contracts (RSCs) so pools and AMMs remain liquid and arbitrage is reduced — indefinitely, as long as the system is funded with REACT for callbacks.
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
