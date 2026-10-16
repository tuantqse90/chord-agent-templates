# Chord agent templates

Small, standalone trading and research agents built on [Chord](https://getchord.xyz), the trading API and on-chain data index for agents. Each one is its own repository: clone it, run `node index.js`, read the report. No key, nothing to install for paper mode, Node 20.12 or newer.

| Agent | What it does | Chains | Mode |
| --- | --- | --- | --- |
| [Whale Flow](https://github.com/tuantqse90/chord-whale-flow) | Follows tokens that several large wallets are net buying, from Chord's trade tape. | all 8 EVM chains | paper / live |
| [Smart Money Copy](https://github.com/tuantqse90/chord-smart-money-copy) | Copies fresh buys when several wallets with proven realized PnL agree, from Chord's Smart Score board and tape. | Monad, Robinhood, Arc | paper / live |
| [DCA Bot](https://github.com/tuantqse90/chord-dca-bot) | Dollar-cost averages into a token list, buying only below the 24h VWAP. Tokenized stocks on Robinhood by default. | all 8 EVM chains (default: Robinhood stocks) | paper / live |
| [Stable Peg Monitor](https://github.com/tuantqse90/chord-stable-peg-monitor) | Prices every dollar stablecoin on a chain at trade size through Chord's router and buys a depeg under 0.996. | all 8 EVM chains | paper / live |
| [Trend Hunter](https://github.com/tuantqse90/chord-trend-hunter) | Scores momentum on every busy token of a chain from Chord's board and hourly candles and rides the strongest trends. | all 8 EVM chains | paper / live |
| [Stock Rotation](https://github.com/tuantqse90/chord-stock-rotation) | Holds the 3 tokenized stocks with the best volatility-adjusted momentum on Robinhood and rotates once a day, through Chord. | Robinhood | paper / live |
| [Token Risk Scanner](https://github.com/tuantqse90/chord-token-risk-scanner) | Never trades: lists tokens Chord's scam screen hides, checks your watch list for dumps and pulled liquidity, writes a blocklist any bot can read. | all 8 EVM chains (LP checks on Monad, Robinhood, Arc) | read-only |
| [LP Range Planner](https://github.com/tuantqse90/chord-lp-range-planner) | Never trades: backtests liquidity ranges for a pool with Chord's /lp/plan and /lp/simulate and ranks them against simply holding. | Monad, Robinhood, Arc | read-only |

## How they work

- **Data**: Chord's token boards, trade tapes, hourly candles, Smart Score board, wallet PnL and LP index. Every chain-specific path comes from `GET /v1/chains`, so the same code runs on every chain Chord serves.
- **Trades**: paper by default. Every quote is a real route from Chord's router and the fill is simulated at the quoted output. In live mode (`LIVE=1`, your own key and RPC) Chord returns an unsigned transaction, the agent dry-runs it on your RPC, your key signs it and the agent measures what actually arrived. Chord never takes custody of keys or funds.
- **Risk gate** on every entry: size cap, max open positions, price-impact cap, daily loss stop.
- **Limits**: anonymous callers get 60 requests a minute per IP; each agent's client stays under that.

## Let an LLM agent call Chord directly

The same data and the swap tools are on Chord's hosted MCP server, no key needed:

```bash
claude mcp add --transport http chord https://api.getchord.xyz/v1/mcp
```

Docs and OpenAPI: https://getchord.xyz/docs

## Disclaimer

Example code, not financial advice. Paper results do not predict real results; in live mode an agent signs real transactions with your key and can lose money.

## License

MIT
