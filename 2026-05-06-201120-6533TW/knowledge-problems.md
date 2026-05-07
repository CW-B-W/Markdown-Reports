# 6533 晶心科 — Knowledge Agent 問題記錄

> **日期**: 2026-05-06
> **階段**: Expert Panel Analysis (13/15 experts)

---

## ⚠️ 缺失專家

| 專家 | 原因 | 影響 |
|:-----|:-----|:-----|
| **Cathie Wood** | `experts/wood.json` 不存在 | 缺少破壞式創新視角；對 RISC-V 可能偏多 |
| **Paul Tudor Jones** | `experts/jones.json` 不存在 | 缺少宏觀/技術交易視角 |

**建議**: 在 `plugin/agents/knowledge/experts/` 目錄下新增 `wood.json` 和 `jones.json` 定義檔。

---

## 📝 執行記錄

| 步驟 | 狀態 | 耗時 |
|:-----|:----:|:----:|
| STEP 0: 自我質詢 | ✅ | < 1 min |
| STEP 1: @information 委派 | ✅ | ~3 min (agent) |
| STEP 1.5: @information 檢視 | ✅ 無需修正 | < 1 min |
| STEP 2: 專家資料需求 (13) | ✅ | ~5 min (5 批次) |
| STEP 3: 專家初始判決 (13) | ✅ | ~8 min (5 批次) |
| STEP 4: 交叉質詢討論 (13) | ✅ | ~8 min (5 批次) |
| STEP 5: 共識綜合 | ✅ | ~5 min |

---

## 🔧 技術問題

### 1. 路徑拼寫錯誤 (已修復)
- **問題**: Lynch 專家判決批次中，腳本路徑 `/home/untu/...` 誤植為 `/home/ubuntu/...`
- **影響**: 該批次僅完成 Buffett 和 Graham，Lynch 需單獨重新執行
- **解決**: 修正後重跑，無資料遺失

### 2. 討論 JSON 格式差異
- **問題**: 各專家判決輸出的 JSON 結構略有不同（部分專家使用 `ANALYSIS PROCESS` vs `analysis_process` 欄位名，部分將 `verdict` 包裝為物件而非直接欄位）
- **影響**: 需在編譯討論檔案時手動標準化
- **解決**: 使用簡化版摘錄 (signal, confidence, reasoning) 而非完整原始判決

### 3. 缺失資料（上游限制）
- RISC-V 授權/權利金拆分：標準 API 無法取得
- 同業比較 (M31 6643)：yfinance 無資料
- 地區營收拆分：需年報
- 客戶集中度：需年報

---

## 📊 專家轉向摘要

| 專家 | 轉向 | 原因 |
|:-----|:----:|:-----|
| **Dalio** | 😐中立 → 🐻看空 | 交叉質詢後更重視估值極端與成長減速 |
| **Ackman** | 😐中立 → 🐻看空 | 確認無股東行動主義催化劑，估值過高 |
| **Bullish Advocate** | 🐂看多 → 😐中立 | 承認成長減速與極端估值削弱看多論點 |

---

*記錄由 @knowledge 自動產生 — 2026-05-06 UTC+8*
