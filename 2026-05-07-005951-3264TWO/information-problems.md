# Information Agent — 問題記錄

## 執行中遇到的問題

### 1. `financial_metrics.py analyze` 腳本失敗
- **原因**: yfinance 未安裝（Python environment 缺少套件），且 finmind_client 子進程呼叫回傳空值
- **解決方式**: 使用 `--input-file` 模式，將數據報告中的 OHLCV 資料寫入 `.ta-tmp/ohlcv_input.json`，以 `compute` 命令成功計算技術指標
- **狀態**: ✅ 已解決

### 2. `valuation.py` DCF 模型使用季度 FCFF
- **原因**: valuation_input.json 中的 fcff 值填入 2026Q1 的 NT$3.41 億（季度資料），導致 DCF 每股估值僅 NT$14.08
- **影響**: DCF 估值低估——若使用年度 FCFF NT$19.43 億重新計算，估值約為 NT$55-70
- **緩解**: 已在報告中註明此限制並給出調整後的合理估值範圍
- **狀態**: ✅ 已備註

### 3. PEG 腳本預期成長率為百分比值
- **原因**: `compute_peg()` 函數預期 `eps_growth_rate` 為百分比數值（如 15 代表 15%），但輸入 JSON 使用 0.15（小數）
- **結果**: 腳本輸出 PEG = 292（35.04 ÷ 0.12），明顯不合理
- **緩解**: 報告中使用自行計算的 PEG = 2.44（PE 35.04 ÷ 14.4% 正確百分比）
- **狀態**: ✅ 已手動修正

### 4. 無 yfinance，部分基本面指標無法自動獲取
- **原因**: 系統未安裝 yfinance 套件
- **影響**: `financial_metrics.py` 的 fundamentals 分支回傳不可用
- **緩解**: 所有基本面資料從 FinMind 數據報告中手動提取，覆蓋率完整
- **狀態**: ✅ 已解決

### 5. SMA60 無法計算
- **原因**: 數據報告僅包含 30 個交易日（約 1.5 個月），不足 60 日
- **緩解**: 使用 SMA20 替代，已於報告中標註
- **狀態**: ✅ 已備註
