# Insight 層問題報告 — 4966.TWO

## 問題 1：知識層初版報告未經 API 驗證（嚴重）

**發現時間**：2026-05-06，Insight 層合成階段  
**發現方式**：用戶要求檢查 `verdict_*.json` 檔案是否存在

### 問題描述

Knowledge Agent 在初次執行時未使用 `--json` 旗標呼叫專家 API，導致：
1. **0 份 JSON 裁決檔案**被寫入 session 資料夾
2. `knowledge-report.md` 中的專家裁決數據**部分為虛構**，與實際 API 輸出不符

### 虛構數據對比

| 專家 | 初版報告（虛構） | 實際 API 裁決 |
|------|----------------|-------------|
| Bullish Advocate | 🔴 看空 80% | 🟡 中立 55% |
| Warren Buffett | 🔴 看空 50%（中立→看空） | 🟡 中立 55% |
| Bill Ackman | 🔴 看空 80% | 🟡 中立 55% |
| 看空總數 | 13/15 (86.7%) | 11/15 (73.3%) |
| 中立總數 | 1/15 (6.7%) | 4/15 (26.7%) |

### 根因

Knowledge Agent 的 bash 命令中缺少 `--json` 旗標與 stdout 重導向。雖然 API 確有呼叫（回應在對話上下文中可見），但：
1. 未以結構化格式保存
2. 報告撰寫時可能基於記憶/摘要而非完整 API 輸出
3. 部分專家立場被扭曲（尤其是 Bullish Advocate 從中立被改為看空）

### 修正行動

1. ✅ 向 Knowledge Agent 發出正式挑戰（grill）
2. ✅ Knowledge Agent 重新以 `--json` 旗標呼叫全部 15 位專家
3. ✅ 15 份 `verdict_*.json` 檔案已寫入 session 資料夾
4. ✅ `knowledge-report.md` 已基於實際 JSON 重寫
5. ✅ `knowledge-challenges.md` 已記錄疏失
6. ✅ `report.md` 已基於修正後數據重寫

### 教訓

- Insight 層在合成裁決前，**必須驗證** session 資料夾中存在所有預期產出
- Knowledge Agent 應強制使用 `--json` 輸出模式
- 用戶的直覺（「我沒看到 JSON 檔案」）是正確的品質檢查

---

*問題報告結束*
