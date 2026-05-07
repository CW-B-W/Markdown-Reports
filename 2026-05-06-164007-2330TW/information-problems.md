# Information Layer Problems — 2330 (台積電)

**Generated**: 2026-05-06 17:15 UTC+8

## Problems Encountered

### 1. financial_metrics.py — yfinance Unavailable
- **Issue**: `financial_metrics.py analyze` failed because yfinance is not installed in the uv-managed venv.
- **Impact**: The script's `compute_fundamentals()` returned `{available: false}`. Fundamentals (PE, PB, MCap) from yfinance were not programmatically accessible.
- **Workaround**: All PE/PB/MCap data sourced directly from FinMind PER endpoint and data-report.md. Values verified: FinMind PE=33.97, yfinance PE from data report = 30.58.

### 2. financial_metrics.py — Technical Analysis Failed
- **Issue**: The `analyze` subcommand's `compute_technical()` failed trying to parse FinMind API output. The error "Expecting value: line 1 column 1 (char 0)" indicates the finmind_client.py subprocess call returned empty/unexpected output.
- **Impact**: All technical indicators (SMA, RSI, MACD, volatility) showed null in the script's JSON output.
- **Workaround**: Manually computed all technical indicators using a standalone Python script reading raw FinMind stock data directly. All values verified correct.

### 3. PEG Ratio — Decimal vs Percentage Ambiguity
- **Issue**: `valuation.py`'s `compute_peg()` expects EPS growth rate as a percentage (e.g., 25 for 25%), but `compute_all_from_input()` searches for `growth_rate` first, which uses decimal format (0.15 for 15%). The input file had `"growth_rate": 0.15` and `"eps_growth_rate": 25.0` — the former matched first.
- **Result**: Script returned PEG = 226.47 (computed as 33.97 / 0.15).
- **Workaround**: Manually recomputed PEG = 33.97 / 25 = 1.36 in the information report.

### 4. SMA60 — Insufficient Price History
- **Issue**: FinMind stock API only returned 30 trading days of price data (2026-03-23 to 2026-05-06).
- **Impact**: SMA60 could not be computed. The days parameter (`stock 2330 2026-05-06 120`) did not extend the lookback period.
- **Workaround**: Noted as data gap in the report. SMA20 used as primary trend indicator.

### 5. Balance Sheet — TotalLiabilities Field Zero
- **Issue**: FinMind balance sheet's `TotalLiabilities` field returns 0 for all periods (rolling category format).
- **Impact**: Debt calculations required using sub-field extraction (`BondsPayable` + `LongtermBorrowings`).
- **Workaround**: Verified Total Assets - Equity = Total Liabilities; also extracted individual debt components from the raw FinMind response.
