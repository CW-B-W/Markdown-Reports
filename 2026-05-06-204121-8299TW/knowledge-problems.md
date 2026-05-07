# Knowledge Layer 執行問題記錄

**報告日期**: 2026-05-06  
**層級**: @knowledge — 問題記錄

---

## 問題清單

### 1. ⚠️ 專家名冊不匹配（已解決）
- **問題**: 使用者要求的 15 位專家中，bill_miller、paul_tudor_jones、george_soros、ray_dalio 的 JSON 定義文件不存在
- **解決方案**: 基於現有專家格式（buffett.json, druckenmiller.json 等）建立 4 個新的專家定義文件
- **檔案位置**: `plugin/agents/knowledge/experts/{miller,jones,soros,dalio}.json`
- **風險**: 新建立的專家 persona 未經完整校準，系統提示詞為通用模板

### 2. ⚠️ STEP 2 輸出顯示問題（不影響功能）
- **問題**: `--ask-order` 階段輸出顯示「No output」，因為 CLI 格式化代碼尋找 `verdict` 欄位但訂單階段返回 `additional_needs`
- **影響**: API 呼叫本身成功（無錯誤），僅終端輸出格式不完整
- **腳本位置**: `summon_expert.py` 第 341-347 行
- **建議修復**: 在 CLI 輸出格式中為 `phase == "order"` 添加專門的格式化分支

### 3. ⚠️ 語言管線不一致（接受）
- **問題**: 所有專家系統提示詞為英文，專家輸出為英文。最終 knowledge-report.md 為繁體中文
- **解決方案**: Knowledge agent 在 STEP 5 合成階段將英文裁決翻譯為繁體中文
- **風險**: 翻譯可能損失部分語義精確度，但對專家共識的整體判斷影響有限

### 4. ✅ API 穩定性
- 共執行 45 次 DeepSeek API 呼叫（15 專家 × 3 輪）
- 零次 API 失敗或錯誤
- DeepSeek v4-pro 模型回應時間約 30-90 秒/次
- Thinking mode 啟用，所有專家提供了完整的推理鏈

### 5. ✅ 專家討論回合有效性
- 5/15 專家在討論後修改了立場：
  - Philip Fisher: 中立→看多（被前瞻 EPS 論點說服）
  - Stanley Druckenmiller: 看多→中立（被估值風險說服）
  - George Soros: 看空→中立（承認短期反身性）
  - Ray Dalio: 信心度 40%→60%（同儕共識強化判斷）
  - Bullish Advocate: 信心度 85%→65%（過熱技術面降低確信度）
- 10/15 專家維持原立場——顯示討論回合不是形式主義

---

## 執行統計

| 階段 | 呼叫次數 | 成功 | 失敗 | 耗時 |
|------|---------|------|------|------|
| STEP 2: 訂單 | 15 | 15 | 0 | ~2 分鐘 |
| STEP 3: 裁決 | 15 | 15 | 0 | ~8 分鐘 |
| STEP 4: 討論 | 15 | 15 | 0 | ~8 分鐘 |
| **合計** | **45** | **45** | **0** | **~18 分鐘** |

---

*記錄時間: 2026-05-06 22:35 (UTC+8)*
