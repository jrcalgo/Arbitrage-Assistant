## Token Arbitrage Assistant

An exploratory LangGraph agent that analyzes crypto token pairs across centralized exchanges to surface potential arbitrage opportunities. The notebook orchestrates LLM-driven research with market data from CoinGecko, synthesizing risk, liquidity, and spread signals into concise, user-facing reports.

### What it aims to achieve
- **Identify** where a token pair trades across exchanges and quantify buy/sell price differentials.
- **Prioritize** opportunities by potential profit, liquidity, spreads, risk, and exchange trust.
- **Generate** concise arbitrage summaries and an overall profit assessment, with iterative refinement for clarity and rigor.

### What it does
- **Ticker resolution**: Converts coin names to tickers with an LLM and maps them to CoinGecko IDs.
- **Market ingestion**: Fetches live tickers from CoinGecko, then performs combinatorial analysis across exchanges and pairs to pick the optimal buy/sell venues per symbol.
- **Scoring**: Ranks opportunities using a heuristic combining profit percentage, average volume, trust scores, and spread penalties.
- **Risk signals**: Aggregates web signals (via Tavily) for exchange risk assessment and folds that into the report.
- **Reporting loop**: Iteratively drafts, scores, and refines an arbitrage report until it meets a quality threshold or hits safety caps.
- **Routing**: Non-arbitrage user queries are answered via a normal response path.

### Key components
- **Research subgraph**: token parsing, market/ticker ingestion, opportunity extraction, web risk summary, report drafting and refinement.
- **Main graph**: query routing (arbitrage vs normal), final profit assessment synthesis.
- **Models/data**: `ChatGroq` (`llama-3.1-8b-instant`), `CoinGeckoAPI` tickers, optional `Tavily` web search.

### Example outcome (from the included run)
- **Input**: “Assess arbitrage opportunities: TON, PEPE, WLD”
- The agent:
  - Resolved tickers and mapped CoinGecko IDs.
  - Retrieved 100+ WLD tickers (fewer for TON/PEPE).
  - Identified a top opportunity for `WLD/USDT`: buy on Binance → sell on BIT with ~1.34% price differential (subject to volume/spread/trust checks).
  - Produced a concise arbitrage report and a user-facing final summary; risk labeled medium in the sample run.
- Non-arbitrage queries (e.g., general knowledge) are handled by a normal response path.

### Notes and limitations
- Relies on CoinGecko tickers (snapshot, not full order book depth) and heuristic scoring.
- Trust, fees, slippage, and venue constraints are approximated; real execution viability requires deeper due diligence.

- Generated artifacts: narrative arbitrage report, final profit assessment, and graph visualizations of both the main and research subgraphs.
