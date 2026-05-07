# 資訊層計算問題報告 — 2317.TW (鴻海)

## 計算過程中遇到的問題

| 問題 | 嚴重性 | 說明 |
|------|--------|------|
| **FinMind 股價數據僅30筆** | ⚠️ 中 | `finmind_client.py stock 2317 2026-05-06 500` 只回傳30筆資料(2026-03-23起)，無法計算 SMA(60)、SMA(200) 等需要較長歷史的指標 |
| **financial_metrics.py 分析失敗** | ⚠️ 中 | `analyze` 命令呼叫 finmind_client 時回傳 JSON 解析錯誤(`Expecting value: line 1 column 1`)，只能透過外部呼叫取數據後自行計算 |
| **TAIEX API 回傳空陣列** | ⚠️ 中 | 多次嘗試 (`taiex` 命令搭配不同日期範圍) 均回傳空資料。Beta 僅能以數據報告中 10 筆 TAIEX 日資料粗估 |
| **yfinance 無法使用** | ⚠️ 中 | 環境中未安裝 yfinance，`fundamentals` 命令回傳 unavailable |
| **財報欄位對應問題** | ℹ️ 低 | FinMind 的「營業毛利」欄位數據與鴻海實際毛利率(~6%)偏差極大(顯示60%+)，可能為科目對應錯誤。本報告以自行計算的淨利率(2.34%)取代 |
| **valuation.py 欄位共用問題** | ℹ️ 低 | `growth_rate` 與 `eps_growth_rate` 共用同一 _extract_field，導致 DCF (需小數0.10) 與 PEG (需整數15) 無法同時正確計算。PEG 值 (185.8) 為錯誤輸出，已在報告中以手算補正 |
| **S&P 500 / 美債殖利率缺失** | ℹ️ 低 | FinMind USStockPrice 與 GovernmentBondsYield 均回傳空值，無法做美股關聯分析 |

## 已採取的補救措施

1. 技術指標：自行編寫 Node.js 腳本，從 FinMind 取得的 30 筆價格數據計算 SMA(20)、RSI(14)、MACD、Bollinger Bands、波動率
2. Beta：以數據報告中 10 筆 TAIEX 日資料，對應 30 筆股價資料中重合日期，計算 9 期報酬率 Beta
3. PEG：手動計算三種成長情境 (15%/20%/23.5%) 的 PEG 值
4. 淨利率：以 TTM 歸屬母公司淨利 ($189B) ÷ TTM 營收 ($8.10T) 自行計算
5. Forward P/E：基於 EPS 成長假設情境推算
