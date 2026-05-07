# Data Collection Problems — 2330 (台積電)

**Generated**: 2026-05-06 16:47 UTC+8

## Problems Encountered

### 1. stock_adj — FinMind Sponsor Required
- **Command**: `finmind_client.py stock_adj 2330 2026-05-06`
- **Result**: `{"available": false}`
- **Impact**: Adjusted prices (還原股價) unavailable. Minor — unadjusted prices work for most analysis.
- **Workaround**: yfinance backup provides raw prices. Manual dividend adjustment possible.

### 2. US Stock Data — FinMind Empty
- **Commands**: `finmind_client.py us_stock "^GSPC"`, `us_stock "^IXIC"`
- **Result**: Both returned `{"data": []}`
- **Impact**: S&P 500 and Nasdaq data not available from primary source.
- **Workaround**: Used yfinance backup successfully. Complete data obtained.

### 3. Oil/WTI — FinMind Empty
- **Command**: `finmind_client.py oil WTI 2026-05-06`
- **Result**: `{"data": []}`
- **Impact**: WTI crude price unavailable. Removed from report section.
- **Workaround**: None yet. Could use playwright-cli to scrape alternative source.

### 4. Income Statement Field Name Mismatch
- **Symptoms**: `IncomeAfterTax` field returned 0 for all periods. Actual net income in `IncomeAfterTaxes`.
- **Impact**: Initial TTM calculation showed NT$0 net income. Fixed by using correct field name.
- **Root Cause**: FinMind schema uses `IncomeAfterTaxes` (plural) for the income statement endpoint.

### 5. Balance Sheet TotalLiabilities = 0
- **Symptoms**: `TotalLiabilities` field showed 0 for all periods.
- **Workaround**: Computed as `TotalAssets - Equity`.
- **Root Cause**: Balance sheet uses rolling category fields. Total liabilities embedded in sub-categories.

### 6. yfinance May 6 Close = NaN
- **Symptoms**: yfinance returned NaN for 2026-05-06 close price.
- **Impact**: Used FinMind price (NT$2,250) as primary source for May 6.
- **Root Cause**: yfinance likely hadn't finalized the day's close at time of query.

### 7. Institutional Data Date Gaps
- **Note**: FinMind institutional data dates may skip non-trading days. Merge not performed.
- **Impact**: Minor — trading day data is complete.
