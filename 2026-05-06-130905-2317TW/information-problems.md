# 資訊代理問題記錄 (Problems Log)

**報告:** information-report.md  
**日期:** 2026-05-06  
**代理:** @information

## 已記錄問題

### 1. `financial_metrics.py analyze` 腳本執行失敗
- **問題:** `financial_metrics.py analyze 2317 2026-05-06` 無法取得技術指標
- **原因:** 
  - `finmind_client.py` 回傳非 JSON 輸出 (路徑問題導致 subprocess 無法正確解析)
  - `yfinance` 未安裝於當前環境
- **處理:** 自行從 data-report.md 提取 OHLCV 數據及 TAIEX 數據，以 Python 計算 SMA20/60、RSI14、MACD、波動率及 Beta。結果已寫入報告第五章。

### 2. Beta 值無法從標準管道取得
- **問題:** FinMind 與 yfinance 均未直接提供 Beta 值
- **處理:** 以 2317.TW 日收盤價與 TAIEX 日收盤價 (約 252 筆) 計算 Covariance/Variance 得出 Beta ≈ 1.0。數據近似但具參考價值。

### 3. PEG 比率腳本參數衝突
- **問題:** `valuation.py` 的 `_extract_field` 函數對 `growth_rate` 和 `eps_growth_rate` 使用相同欄位提取，導致 DCF (需小數) 與 PEG (需百分比) 無法同時正確計算
- **處理:** 手動計算 PEG = PE 18.53 ÷ EPS成長率 15% = 1.24。DCF 採用 `growth_rate: 0.10` (10%)。

### 4. 同業比較數據不完整
- **問題:** 數據報告列出了同業代碼 (2356/2382/3231/6415) 但無實際估值數據
- **處理:** 報告中預留同業比較表格框架，建議後續以 web search 補充。

### 5. PTT 散戶情緒未收集
- **問題:** @data 未執行 PTT 爬蟲
- **處理:** 報告中標記為資料缺口。

### 6. 國際總經數據缺口
- **問題:** FinMind USStockPrice/GovernmentBondsYield/CrudeOilPrices 端點需 Sponsor 付費
- **處理:** 報告中標記為資料缺口，不影響台灣單一個股分析。
