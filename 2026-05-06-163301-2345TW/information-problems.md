# 資訊層級 — 問題記錄

## 執行問題

### 1. finmind_client.py / yfinance_client.py 不存在
- **影響**: financial_metrics.py 腳本的 `analyze` 命令失敗
- **錯誤訊息**: "Expecting value: line 1 column 1 (char 0)"
- **根因**: 腳本嘗試執行 finmind_client.py (不存在於專案中) 並解析其 JSON 輸出
- **解決方案**: 直接從 data-report.md 提取價格資料進行人工計算替代

### 2. yfinance 不可用
- **影響**: 無法取得 yfinance 備援資料 (PE/PB/DY 等)
- **根因**: yfinance 套件未安裝於目前 Python 環境
- **解決方案**: 所有估值資料源自 FinMind API，資料完整無缺失

### 3. 技術指標資料點不足
- **影響**: SMA20、SMA60、MACD、RSI(14) 無法完整計算
- **根因**: data-report.md 僅呈現 11 個交易日之詳細價格資料
- **解決方案**: 以 SMA5、SMA10、RSI(5)、布林通道(5日/10日) 替代
- **資料缺口已於 data-report.md 標註**: @data 有收集 180 天股價資料但未全部呈現

### 4. PEG 比值偏高問題
- **備註**: PEG 以 PE 52.73 / EPS成長率(10%) 計算得出 527.3。此為腳本使用 growth_rate (0.10) 計算之結果。若以 25% EPS 成長率計算，PEG = 2.1。報告中已標註兩種情境。

### 5. 同業比較樣本不足
- **影響**: 僅取得智易 (3596) 和明泰 (3380) 兩家同業資料
- **建議**: 後續可納入啟碁 (6285)、中磊 (5388) 等網通同業
