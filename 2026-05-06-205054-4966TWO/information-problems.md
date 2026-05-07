# 資訊層問題紀錄：譜瑞-KY (4966.TWO)

**報告日期**: 2026-05-06  
**對應報告**: information-report.md

---

| # | 問題 | 嚴重性 | 說明 | 解決方式 |
|---|------|--------|------|---------|
| 1 | `financial_metrics.py` 執行失敗 | 中 | script 依賴 yfinance，但解析 data-report 資料時報錯 `Expecting value: line 1 column 1 (char 0)`，無法正確讀取技術指標 | 改由手動呼叫 yfinance API 計算 SMA/RSI/MACD/波動率 |
| 2 | `valuation.py` PEG 計算異常 | 低 | PEG 公式使用 `EPS_Growth_Rate` 原始值 (0.05) 而非百分比倍數 (5)，導致 PEG = 354.6 (應為 17.73/5=3.55) | 手動重新計算 PEG，報告中使用修正值 3.55x |
| 3 | FinMind 模組未於 `.venv` 安裝 | 低 | `finmind` / `finsentinel` 套件未安裝於專案虛擬環境，但 @data 成功收集資料（可能使用獨立安裝） | 使用 yfinance (已安裝於 .venv) 補技術指標計算 |
| 4 | 美股/美債資料不可用 | 低 | FinMind USStockPrice 及 GovernmentBondsYield 回傳空陣列，無法進行國際比較 | 報告中省略跨市場對比 |
| 5 | 期貨三大法人持倉僅 2018 年 | 低 | FinMind futures_institutional 僅回傳 2018 年歷史資料 | 報告中未納入期貨法人數據 |
| 6 | PTT 討論度過低 | 低 | 近30天僅 1 篇提及，情緒樣本不具統計意義 | 報告中保留但備註樣本不足 |
| 7 | 借券賣出數據特異 (4/27) | 中 | 2026-04-27 外資持股比率驟降 6.4pp（45.45%→26.88%），疑為 GDR 轉換或大額轉讓，非正常賣壓 | 報告中明確標註此異常並提出可能原因 |
