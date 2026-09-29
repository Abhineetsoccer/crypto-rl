# Crypto RL Project Structure

```text
crypto_rl/
│
├── data/
│   ├── raw/
│   │   └── btc_15m.csv
│   │
│   └── processed/
│       └── btc_15m_features.csv
│
├── src/
│   ├── __init__.py
│   ├── data_loader.py
│   ├── features.py
│   ├── broker.py
│   ├── strategies.py
│   ├── backtest.py
│   ├── metrics.py
│   └── config.py
│
├── tests/
│   ├── __init__.py
│   ├── test_data_loader.py
│   ├── test_features.py
│   ├── test_broker.py
│   ├── test_strategies.py
│   ├── test_backtest.py
│   └── test_metrics.py
│
├── scripts/
│   ├── download_data.py
│   ├── build_features.py
│   └── run_backtest.py
│
├── results/
│   ├── trades/
│   ├── plots/
│   └── reports/
│
├── models/
│
├── notebooks/
│
├── .gitignore
├── requirements.txt
├── README.md
└── pyproject.toml
```

## What Each Part Is For

### `data/raw/`

Original downloaded market data. **Never modify these files manually.**

### `data/processed/`

Cleaned data plus engineered features.

### `src/`

All reusable trading logic.

### `src/data_loader.py`

Loads CSV or Parquet files and checks timestamps and data quality.

### `src/features.py`

Creates:

- Returns
- Moving averages
- Volatility
- Relative volume
- Momentum

### `src/broker.py`

Simulates:

- Cash
- BTC holdings
- Buying
- Selling
- Fees
- Slippage
- Portfolio value

### `src/strategies.py`

Contains strategies such as:

- `BuyAndHoldStrategy`
- `MovingAverageStrategy`
- `MomentumStrategy`

Later, this can include `RLStrategy`.

### `src/backtest.py`

The backtesting engine:

```text
market data
    ↓
features
    ↓
strategy
    ↓
broker
    ↓
portfolio
```

### `src/metrics.py`

Calculates:

- Total return
- Sharpe ratio
- Maximum drawdown
- Turnover
- Number of trades
- Fees

### `src/config.py`

Central place for settings such as:

```python
INITIAL_CAPITAL = 100.0
FEE_RATE = 0.001
SLIPPAGE = 0.0002
SYMBOL = "BTC/USDT"
TIMEFRAME = "15m"
```

### `tests/`

Contains automated tests only. The backtest test belongs in `tests/test_backtest.py`, not in the main backtesting implementation.

### `scripts/`

Contains executable project workflows:

- `scripts/download_data.py` downloads market data.
- `scripts/build_features.py` creates processed datasets.
- `scripts/run_backtest.py` runs an experiment using `src/backtest.py`.

### `results/`

Contains generated outputs, for example:

```text
results/trades/trades.csv
results/plots/equity_curve.png
results/reports/backtest_summary.json
```

Generated results should not be mixed with source code.

### `models/`

Later, this can contain trained models and related artifacts:

```text
ppo_btc.zip
sac_btc.zip
scaler.pkl
```

### `notebooks/`

For exploration, data inspection, and plots only. Notebooks should not be the main trading system.

## Future RL Structure

When reinforcement learning is added, `src/` can be extended as follows:

```text
src/
├── ...
├── envs/
│   ├── __init__.py
│   └── crypto_env.py
│
├── agents/
│   ├── __init__.py
│   └── ppo_agent.py
│
└── risk/
    ├── __init__.py
    └── risk_manager.py
```

For live trading, it can later include:

```text
src/
├── execution/
│   ├── paper_broker.py
│   └── live_broker.py
│
└── exchanges/
    ├── coinbase.py
    └── kraken.py
```

These future directories should not be created yet.

## Immediate Workflow

```text
scripts/download_data.py
        ↓
data/raw/btc_15m.csv
        ↓
src/features.py
        ↓
src/backtest.py
        ↓
scripts/run_backtest.py
        ↓
results/
```

This provides a clean foundation for building the RL system later.
