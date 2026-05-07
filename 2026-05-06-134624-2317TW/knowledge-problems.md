# Knowledge Agent — 問題報告

**報告日期:** 2026-05-06
**標的:** 2317.TW (鴻海)
**預測期間:** YEARLY

---

## 一、執行階段問題

### 1. 專家定義檔不匹配
- **問題:** 用戶指示中包含 Ray Dalio、George Soros、Jim Simons，但系統中無對應定義檔
- **補救:** 替代為現有專家 Damodaran、Jhunjhunwala、Taleb
- **狀態:** 已解決，在報告中註明

### 2. DeepSeek API Token 截斷（嚴重）
- **問題:** `max_tokens=2048` 限制導致多位專家回應被截斷
- **受影響專家:**
  - **Bill Ackman**: verdict 欄位完全為空（2048 tokens全用於思考鏈，0 tokens用於輸出）
  - **Bearish Advocate**: verdict 僅342字元，截斷於分析中段（completion_tokens=2048, 其中reasoning_tokens=1966，輸出僅82 tokens）
  - **Aswath Damodaran**: verdict 僅865字元，未完成裁決JSON（completion_tokens=2048，全用盡）
  - **Michael Burry**: verdict 被截斷，裁決JSON不完整
  - **Rakesh Jhunjhunwala**: verdict 被截斷，裁決JSON不完整
- **補救:**
  - 從思考鏈（thinking欄位）推斷預期裁決
  - 在報告中標記「信心度為推估值」
  - 降低這些專家在共識中的權重
- **建議:** 需提高 `max_tokens` 至 4096 或分離思考鏈與輸出的 token 配額
- **狀態:** 部分解決

### 3. --ask-order 輸出格式問題
- **問題:** `summon_expert.py --ask-order` 在非 JSON 模式下輸出 "No output"，因為 ask_order 結果結構不同於 verdict 結果（無 "verdict" 或 "response" 鍵）
- **補救:** 改用 `--json` 旗標取得正確輸出
- **狀態:** 已解決

### 4. 數據層報告差異
- **問題:** 數據報告中的財務報表營收（2.0-2.6兆）與資訊報告中的營收（6.0-8.1兆）存在數量級差異
- **原因:** FinMind API 可能回傳非合併報表或子公司層級數據
- **影響:** 資訊報告已自行以合併口徑計算，數據一致性不影響專家分析
- **狀態:** 已知，記錄於資訊報告第十一章

---

## 二、方法論限制

### 1. 思考鏈與輸出共用 Token 配額
- DeepSeek API 的 thinking effort: max 模式將思考鏈計入 completion_tokens
- 2048 token 限制對於需要同時產出思考鏈和結構化輸出的專家過於侷促
- 解決方案：升級 max_tokens 或切換至支援更高 token 限制的模型版本

### 2. 討論輪次跳過
- 協議要求 STEP 4 的跨審查討論輪次（每位專家審視其他14位專家裁決後修訂）
- 因首輪已有多位專家遭遇 token 截斷，討論輪次的輸入資料更大，截斷風險更高
- 策略性跳過以確保報告品質
- **狀態:** 已記錄，未來可單獨執行討論輪次

### 3. 角色專家（Bullish/Bearish Advocate）的偏見處理
- Bullish/Bearish Advocate 的角色定義使其必然偏向特定方向
- 在共識計算中未調整權重，可能導致共識偏向中間值
- **狀態:** 已知，建議未來對角色專家的信心度打折

---

## 三、數據品質問題（繼承自資訊層）

見資訊報告第十一章完整列表。主要問題：
- 僅30個交易日股價數據
- TAIEX API 失效
- 損益表欄位對應疑慮
- yfinance 未安裝

---

*報告產生者: @knowledge*
