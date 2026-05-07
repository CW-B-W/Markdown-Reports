# 數據問題報告 — 2317.TW (鴻海)

## 資料缺口

| 問題 | 嚴重性 | 說明 |
|------|--------|------|
| S&P 500 指數 (^GSPC) | ⚠️ 中 | FinMind USStockPrice 回傳空陣列，無法獲取美股資料 |
| Nasdaq 指數 (^IXIC) | ⚠️ 中 | 同上，FinMind 無資料回傳 |
| 美債10年期殖利率 | ⚠️ 中 | FinMind GovernmentBondsYield 回傳空陣列 |
| 還原股價 | ℹ️ 低 | 需要 FinMind Sponsor 方案（付費） |
| 台指期貨數據 | ℹ️ 低 | 未在此報告中擷取，可用 futures_daily 補充 |
| 技術指標 (SMA/RSI/MACD) | ℹ️ 低 | 原始數據已提供，可由 information agent 自行計算 |

## 已嘗試但失敗

- `us_stock ^GSPC 2026-05-06 500` → 回傳空陣列
- `us_stock ^IXIC 2026-05-06 500` → 回傳空陣列  
- `bonds_yield "United States 10-Year" 2026-05-06` → 回傳空陣列

## 建議

1. 使用 yfinance 補美股指數資料（但環境中可能有 pandas 相容性問題）
2. 使用 web_fetch 直接從 Treasury.gov 或 Yahoo Finance 抓美債殖利率
3. 期貨數據可透過 FinMind `futures_daily TX` 命令補齊
