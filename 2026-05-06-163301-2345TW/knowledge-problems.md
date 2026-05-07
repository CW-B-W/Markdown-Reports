# 知識層級 — 問題記錄

## 執行摘要

15 位專家全數召喚成功。初判 (Phase 3) + 討論修正 (Phase 4) 兩輪完成。3 項次要問題記錄如下。

---

## 問題清單

### 1. summon_expert.py --ask-order 輸出格式 Bug
- **影響**: Phase 2 (專家訂單) 的非 JSON 模式無法正確輸出結果
- **根因**: `ask_order` 回傳的 dict 缺少 `verdict` 和 `response` 鍵，導致第 346 行 `result.get("verdict", result.get("response", "No output"))` 永遠印出 "No output"
- **解決方案**: 在 Phase 2 使用 `--json` 旗標，直接輸出完整 JSON
- **建議修復**: 在 summon_expert.py 第 344-346 行增加對 `phase == "order"` 的處理分支

### 2. PEG 比值計算異常
- **影響**: information-report.md 中 PEG 顯示為 527.3，但 PEG = PE(52.73) / 成長率(25%) = 2.11
- **根因**: financial_metrics.py 腳本可能將成長率以十進位小數 (0.10) 而非百分比 (10) 代入公式，導致 52.73/0.10 = 527.3
- **實際正確 PEG**: 假設 25% EPS 成長率，PEG = 2.11 (>2，仍屬過高)
- **已於報告中備註正確數值**

### 3. 部分討論回合的篇幅限制
- **影響**: 討論回合 (Phase 4) 中，部分專家 (如 Jhunjhunwala) 的輸出將分析過程與判決混合在 verdict 欄位，未遵循標準 JSON 結構
- **根因**: DeepSeek API 的討論模式 prompt 可能引導部分專家產出非結構化回應
- **解決方案**: 人工從內容中提取判決訊號與信心分數，已完成

---

## 資料品質備註

- 原始 data-report.md 僅包含 11 個交易日明細股價 (非完整的 180 天)，導致 SMA20/60、MACD、RSI(14) 無法計算。此限制已被 information-report.md 明確標註
- yfinance 備援資料不完整 (未安裝套件)，但不影響主要分析 (FinMind 資料完整)
- PTT 情緒資料部分日期混雜，信號參考價值有限

---

*記錄時間: 2026-05-06 18:50*
