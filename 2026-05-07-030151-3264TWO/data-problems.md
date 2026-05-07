# 資料收集問題報告 — 3264.TWO (欣銓科技)

**報告日期**: 2026-05-07 03:01:51 UTC  
**生成者**: @data (LEAF node — no further delegation)

---

## 問題清單

### P1 — 資料完全不可用

| 問題 | API端點 | 影響 |
|------|---------|------|
| **美國10年期公債殖利率** | FinMind GovernmentBondsYield ("United States 10-Year") | 回傳空陣列 `{data: []}`。無法取得美債殖利率 |
| **全市場法人彙總** | FinMind TaiwanStockTotalInstitutionalInvestors | 回傳空陣列 `{data: []}`。無法取得三大法人大盤買賣超 |

### P2 — 資料延遲或不完整

| 問題 | API端點 | 影響 |
|------|---------|------|
| **FinMind股價資料較舊** | FinMind TaiwanStockPrice (3264) | 僅提供至2026年3月-4月初，不包含4月中旬後的強漲行情 |
| **FinMind PE/PB資料延遲** | FinMind TaiwanStockPER (3264) | 最新資料約2026年3月，不反映近期股價急漲(PE從20→41.8) |
| **FinMind財務報表未更新** | FinancialStatements/BalanceSheet/CashFlows | 最新完整財年為2024年，2025年年報尚未公告 |
| **月營收延遲2個月** | TaiwanStockMonthRevenue | 最新月份為2026年3月 |

### P3 — 資料格式問題

| 問題 | 說明 |
|------|------|
| **期貨法人資料缺少名稱欄位** | FuturesInstitutionalInvestors 的 name 欄位為空字串，無法直接區分外資/投信/自營商。必須透過成交量及未平倉特徵推估。 |
| **黃金資料為5分鐘頻率** | GoldPrice API返回日內高頻資料(5分鐘bar)，非日收盤。需要自行彙整為日資料。 |
| **FinMind stock命令僅回傳30筆** | fetch_stock()函數限制回傳最近30筆，資料量不足完整60日分析。 |

### P4 — 解決方案

| 問題 | 解決方案 |
|------|---------|
| 股價資料延遲 | ✅ 使用 yfinance 3264.TWO 補齊完整資料 |
| PE/PB資料延遲 | ✅ 使用 yfinance fundamentals 補齊最新估值數據 |
| 財務報表延遲 | ⚠️ 無替代來源，等待財報公告 |
| 債券殖利率 | ⚠️ 無替代來源(需付費API或web scraping) |
| 期貨法人推估 | ⚠️ 根據交易量特徵分組推估法人類別 |

---

## API 調用摘要

| 腳本 | 調用次數 | 成功 | 失敗/空值 |
|------|---------|------|----------|
| finmind_client.py | 18 | 16 | 2 (bonds_yield, total_institutional) |
| yfinance_client.py | 2 | 2 | 0 |
| **總計** | **20** | **18** | **2** |

---

*資料收集完成。報告已寫入 data-report.md。*  
*由 @data 生成 — 原始採集節點，不可再委派。*
