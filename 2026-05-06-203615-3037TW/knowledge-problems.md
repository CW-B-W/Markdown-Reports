# Knowledge Agent — 問題追蹤

## 已辨識問題

### P1 — Paul Tudor Jones 專家缺失
- 使用者要求召喚 Paul Tudor Jones（宏觀戰術／動能），但 `plugin/agents/knowledge/experts/` 目錄中無此專家定義檔
- 可用專家共 21 位（ackman, bearish, buffett, bullish, burry, dalio, damodaran, druckenmiller, fisher, graham, greenblatt, jhunjhunwala, lynch, marks, munger, pabrai, simons, soros, taleb, tepper, wood）
- 最終召喚 14 位（15 位中扣除 PTJ）
- **建議**: 新增 paul_tudor_jones.json 或 ptj.json 定義檔

### P2 — --ask-order 輸出格式問題
- 不帶 `--json` 旗標時，`ask_expert_order()` 回傳的 dict 不含 `verdict` 或 `response` key，導致 CLI 輸出 "No output"
- 解決方案: 使用 `--json` 旗標並將 stdout 重定向至檔案

### P3 — George Soros 討論回合判決解析問題
- Soros 的討論回合回應使用大寫 key（ANALYSIS PROCESS、VERDICT）而非小寫，導致自動解析失敗
- 內容仍可確認其看空立場，但結構化解析需要處理大小寫變體
- 已在最終報告中手動確認 Soros 為看空

### P4 — 部分專家判決格式不一致
- 多數專家使用 `analysis_process + verdict.signal/confidence/reasoning`（小寫）
- George Soros 使用 `ANALYSIS PROCESS + VERDICT.Signal/Confidence/Reasoning`（大寫）
- **建議**: 在 prompt 中更嚴格規範 JSON 格式

### P5 — 專家召喚執行時間長
- 每批次 3 位專家約需 3-5 分鐘（DeepSeek API + thinking mode）
- 14 位專家 × 2 回合 = 28 次 API 呼叫，總耗時約 30-40 分鐘
- 原因: thinking mode (effort=max) 產生大量 reasoning tokens

### P6 — 討論回合輸出大小寫問題
- `discussion_result_soros.json` 中判決使用大寫 key，自動解析腳本未處理此變體
- 已手動確認，不影響最終報告準確性

---

> **報告時間**: 2026-05-06 21:35 (UTC+8)
