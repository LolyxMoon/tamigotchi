## TAMI GOTCHI

<a href="https://freeimage.host/"><img src="https://iili.io/fO1KVus.png" alt="fO1KVus.png" border="0" /></a>

TAMI / TamaHer (TamiGochi)
TamaHer is a community tamagotchi experience where each wallet has its own Tami.
Tami's stats decay over time, and the decay rate is dynamically affected by your real Solana trading performance (read-only wallet analytics).
Token: TAMI.

Core Idea
Each wallet has a personal Tami.
Stats decay every 30 seconds (base decay).
Good trading → decay 50% slower
Bad trading → decay 3x faster
If the average stat hits 0 → Game Over (wallet is out)

We do not require signing transactions to "read trades". Wallet data is public and processed in read-only mode.

Features
Read-only wallet input / optional wallet connect for convenience
Real-time stat decay engine (30s ticks)
Trading-performance modifier (good/bad)
Care actions (spend TAMI): feed / comfort / rest / dress
Pixel-art character states (8-bit sprites, transparent PNG)

Tech Stack (suggested)
Frontend: Next.js (or Vite + React)
Backend: Node.js API (read-only indexing)
Data: Redis/Postgres for snapshots + scoring
Solana: public RPC + indexer (Helius/Shyft/etc)

Local Development
1) Install deps
```bash
pnpm install
Core loop
Tick interval: 30s

State per wallet:
mood, energy, affection, intimacy (0..100)
lastTickAt
performanceMultiplier (0.5 | 1.0 | 3.0)

Tick:
baseDecay = X (per stat)
decay = baseDecay * performanceMultiplier
stats = max(0, stats - decay)

Game Over:
avg(stats) == 0  => status = "dead"

Performance:
computed from last N trades / last 24h window
score classified into: good / neutral / bad
maps to multiplier: good=0.5, neutral=1, bad=3
Wallet
We support:
1) Manual wallet address input (read-only).
2) Optional wallet connect to auto-fill address.
We never request signatures to read trading history.
