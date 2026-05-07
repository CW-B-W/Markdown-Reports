# 資料收集問題報告 — 3008.TW (大立光)

## 無法取得的資料

### 1. 還原股價 (Adjusted Price)
- **API**: `finmind_client.py stock_adj 3008 2024-01-01 2026-05-06`
- **錯誤**: `{available: false, error: "This dataset requires FinMind Backer/Sponsor subscription"}`
- **影響**: 無法取得除權息調整後的歷史股價，技術分析可能略有偏差
- **建議**: 若需要可使用 yfinance 的 adjusted close

### 2. 美股 S&P 500
- **API**: `finmind_client.py us_stock "^GSPC" 2026-05-06`
- **錯誤**: FinMind 回傳空陣列 `{data: []}`
- **影響**: 無法提供 S&P 500 指數數據
- **建議**: 使用 yfinance 或其他來源

### 3. 美股 Nasdaq
- **API**: `finmind_client.py us_stock "^IXIC" 2026-05-06`
- **錯誤**: FinMind 回傳空陣列 `{data: []}`
- **影響**: 無法提供 Nasdaq 指數數據
- **建議**: 使用 yfinance 或其他來源

### 4. 美債10年殖利率
- **API**: `finmind_client.py bonds_yield "United States 10-Year" 2026-05-06`
- **錯誤**: FinMind 回傳空陣列 `{data: []}`
- **影響**: 無法提供無風險利率基準
- **建議**: 使用 web scraping 或 FRED API

### 5. 原油價格 WTI
- **API**: `finmind_client.py oil WTI 2026-05-06`
- **錯誤**: FinMind 回傳空陣列 `{data: []}`
- **影響**: 無法提供原油價格

### 6. CNYES 新聞參數錯誤 (已修正)
- **初始錯誤**: 使用日期參數 `2026-05-06` 但 script 預期為 limit (整數)
- **修正**: 改用 `cnyes_scraper.py 3008 20`

## 成功取得的資料 (共26項)

1. ✅ FinMind stock (OHLCV 400日)
2. ✅ FinMind monthly_revenue (28個月)
3. ✅ FinMind income_statement (9季)
4. ✅ FinMind balance_sheet (9季)
5. ✅ FinMind cash_flow (9季)
6. ✅ FinMind dividends (12次配息)
7. ✅ FinMind margin (融資融券)
8. ✅ FinMind institutional (三大法人)
9. ✅ FinMind short_sale (融券借券)
10. ✅ FinMind per (PE/PB/殖利率歷史)
11. ✅ FinMind taiex (加權指數)
12. ✅ FinMind info (公司基本資料)
13. ✅ FinMind foreign_holding (外資持股歷史)
14. ✅ FinMind total_institutional (全市場法人)
15. ✅ FinMind futures_daily (台指期)
16. ✅ FinMind futures_institutional (期貨法人)
17. ✅ FinMind exchange_rate (USD/TWD)
18. ✅ FinMind gold (黃金價格)
19. ✅ yfinance stock (備份股價)
20. ✅ yfinance fundamentals (備份基本面)
21. ✅ PTT scraper 7 days (鄉民情緒)
22. ✅ PTT scraper 30 days (鄉民情緒)
23. ✅ CNYES news (鉅亨新聞)
24. ❌ FinMind stock_adj (無法取得)
25. ❌ FinMind us_stock ^GSPC (無法取得)
26. ❌ FinMind us_stock ^IXIC (無法取得)
27. ❌ FinMind bonds_yield (無法取得)
28. ❌ FinMind oil WTI (無法取得)

**成功率**: 24/28 = 85.7% (主要缺失為需要進階訂閱的國際數據)
