# 勤誠興業 (8210.TW) — 數據收集問題報告

## 資料缺口

| 缺失數據 | API/工具 | 錯誤訊息 | 嚴重性 | 替代方案 |
|---------|---------|---------|--------|---------|
| 還原股價 (stock_adj) | FinMind | 需 Backer/Sponsor 付費訂閱 | 低 | 非必要，PE/PB已提供 |
| S&P 500 (^GSPC) | FinMind/USStockPrice | 回傳空資料集 `data: []` | 中 | 可用 yfinance 或 web_fetch |
| NASDAQ (^IXIC) | FinMind/USStockPrice | 回傳空資料集 `data: []` | 中 | 可用 yfinance 或 web_fetch |
| 美債10Y殖利率 | FinMind/GovernmentBondsYield | 回傳空資料集 `data: []` | 中 | 可用 web_fetch Treasury |
| WTI原油 | FinMind/CrudeOilPrices | 回傳空資料集 `data: []` | 低 | 可用 web_fetch |

## 注意事項

1. 所有 FinMind 查詢成功（除上述5項），API 密鑰有效
2. yfinance fundamentals 成功取得市值、PE、PB、殖利率、產業分類
3. yfinance stock 成功取得近20日股價資料（與 FinMind 一致）
4. 黃金價格為5分鐘級別即時報價而非日收盤價
5. 2026年 Q1 財務報表尚未公告（最新為 2025-12-31 年報）
