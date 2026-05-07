# 資訊層執行問題記錄

## 問題 1: financial_metrics.py 腳本失效

**問題**: `financial_metrics.py analyze 8299.TW 2026-05-06` 輸出中 `price_data.current = 0`、所有技術指標為 null。腳本內部使用 yfinance 擷取股價資料，但 **8299.TW 為上櫃股，yfinance 不支援** (回傳 404)。

**處理方式**: 從 data-report.md 中手動提取 30 個交易日 OHLCV 資料，以 JavaScript 自行計算:
- SMA20 (20日均線)
- RSI(14) 
- MACD 線/訊號線/柱狀圖
- 年化波動率
- 價格統計 (高低點、漲跌幅)

**影響**: 指標正確計算完成。無資料遺失。

## 問題 2: valuation.py PEG 公式偏差

**問題**: `valuation.py all` 回傳 PEG = 369.2 (公式為 PE / eps_growth_rate_decimal)。標準 PEG 公式應為 PE / (eps_growth_rate_pct)，即 55.38 / 15 = 3.69。

**處理方式**: 在報告中使用正確的 PEG 3.69 並加註計算方式。

**影響**: 已修正，報告中呈現正確數值。

## 問題 3: FinMind 國際資料缺失

**問題**: 4 個 FinMind 端點無返回資料 (S&P 500、Nasdaq、美債 10Y 殖利率、WTI 原油)，需 Sponsor 訂閱方案。

**處理方式**: 在 data-report 中已標記為缺失。資訊報告中未使用國際資料。

**影響**: 不影響台灣市場分析。

## 問題 4: 缺少足量價格資料計算 SMA60

**問題**: data-report 僅提供 30 個交易日價格表，無法計算 60 日均線 (需 60 個資料點)。data-report 雖然標註「60 日回看」但僅列出 30 日明細。

**處理方式**: 在報告中標註 SMA60 無法計算，以 SMA20 替代作為均線參考。

**影響**: 低 — SMA60 可從 FinMind 歷史資料補足，但已足夠進行短中期分析。
