# 資訊層執行問題記錄

**報告**: 勤誠興業 (8210.TW)  
**日期**: 2026-05-06

## 已發現並處理之問題

### 1. financial_metrics.py analyze 命令失敗
- **問題**: `financial_metrics.py analyze 8210 2026-05-06` 嘗試透過 finmind_client 自行抓取資料，但回傳 JSON 解析錯誤
- **處理**: 改用 `--input-file` 模式，從 data-report.md 提取 30 日 OHLCV 資料寫入 price_data.json 後計算
- **影響**: 無 — 技術指標計算成功

### 2. yfinance 基本面資料無法取得
- **問題**: yfinance 未安裝於目前 venv，fundamentals 回傳 unavailable
- **處理**: 所有基本面資料從 data-report.md (FinMind 來源) 取得
- **影響**: 無 — FinMind 已提供完整 PE/PB/殖利率歷史資料

### 3. SMA 60 無法計算
- **問題**: 僅有 30 個交易日價格資料 (2026-03-23 ~ 2026-05-06)，不足 60 日
- **處理**: 報告中標示為「資料不足」
- **影響**: 60 日均線指標遺缺

### 4. PEG 計算之growth_rate輸入格式
- **問題**: valuation.py 的 PEG 函式預期 EPS 成長率為整數百分比 (如 20 表示 20%)，但 DCF 函式預期小數 (如 0.20 表示 20%)。同一個 growth_rate 欄位無法同時滿足兩種格式
- **處理**: 報告中手動計算正確 PEG 值 (PE 46.65 ÷ 20% = 2.33)，而非使用腳本原始輸出

### 5. Net Debt (淨負債) 無法精確計算
- **問題**: 資產負債表未明確區分短期借款與長期借款餘額，僅有現金流量表中之「變動數」
- **處理**: 報告中省略淨負債指標，標示資料未提供
- **影響**: 無法計算 EV/EBITDA 等進階估值指標

## 已知資料缺口 (轉引自 data-report)

| 缺口 | 原因 | 影響 |
|------|------|------|
| PTT 輿情 | 未執行 scraper | 無零售情緒資料 |
| 同業比較 | FinMind 無直接提供 | 無法進行產業對比 |
| 還原股價 | FinMind 需付費訂閱 | 無還原股價技術指標 |
| 美股 (S&P 500, NASDAQ) | FinMind USStockPrice 無資料 | 缺國際市場關聯分析 |
| 美國 10Y 公債殖利率 | FinMind 無此資料 | 缺無風險利率基準 |
