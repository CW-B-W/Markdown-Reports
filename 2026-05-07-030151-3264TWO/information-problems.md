# @information 執行問題記錄 — 3264.TWO

## 問題清單

| 問題 | 嚴重性 | 說明 |
|------|--------|------|
| `financial_metrics.py` 腳本失敗 | 高 | 腳本嘗試從yfinance取得資料但環境未安裝yfinance。返回null值。改用手動從data-report.md讀取OHLCV計算技術指標。 |
| `valuation.py` 正常運作 | 無 | 透過提供JSON輸入檔成功計算Graham Number、DCF、Owner Earnings等估值。 |
| PTT散戶情緒資料未取得 | 中 | @data未從PTT Stock板爬取資料。FinMind新聞API僅提供CMoney/富聯網新聞。建議在data層加入PTT爬蟲。 |
| TPEx櫃買指數未取得 | 低 | 僅取得TAIEX加權指數。未取得櫃買指數(OTC Index)作為市場對比。 |
| 月營收YoY無法計算 | 低 | 月營收資料僅從2025-04開始，無法計算2026-03 vs 2025-03年增率。 |
| 最新月營收為2026-03 | 低 | FinMind月營收延遲約2個月，2026-04營收尚未公告。 |
| 2025年財報尚未公告 | 中 | 最新完整財報為2024年度(2024Q4)，2025年季報尚未發布。EPS參考值為2024全年5.65元。 |
| FinMind PE/PB資料僅至2026-03 | 低 | 引用yfinance PE/PB (41.84/6.12) 做為替代，但yfinance數據可能為TTM估算。 |
| 美國10年期公債殖利率不可用 | 低 | FinMind GovernmentBondsYield API回傳空值。 |
