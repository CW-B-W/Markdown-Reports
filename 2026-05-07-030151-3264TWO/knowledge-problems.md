# 知識層問題報告 — 3264.TWO (2026-05-07)

## 1. 專家缺失

### Paul Tudor Jones 專家定義檔不存在
- **問題**: 用戶要求召喚 Paul Tudor Jones，但 `plugin/agents/knowledge/experts/` 目錄中無 `jones.json`、`paul_tudor_jones.json` 或類似檔案
- **影響**: 實際召喚14位而非15位專家
- **解決方案**: 需創建 Paul Tudor Jones 專家定義檔（參考現有 druckenmiller.json 或 soros.json 模板，因其風格偏向宏觀/交易）

## 2. 專家裁決格式不一致

- **問題**: 各專家返回的 JSON 結構不完全一致。部分專家將 `verdict` 欄位嵌套為物件（含 signal/confidence/reasoning），部分則直接平鋪
- **影響**: 合成共識時需手動解析不同結構
- **狀態**: 已在報告中統一處理

## 3. 腳本輸出問題

- **問題**: `summon_expert.py` 在 `--ask-order` 模式下未使用 `--json` 旗標時，輸出欄位對應錯誤（`result.get("verdict")` 在 ask-order 階段不存在）
- **解決方案**: 統一使用 `--json` 旗標

## 4. API 呼叫成本

- 14位專家 × 2輪（裁決+討論）= 28次 DeepSeek API 呼叫
- 每次呼叫使用 thinking mode (effort: max)，tokens 用量約 7,000-10,000 per call
- 總 tokens 用量約 200,000-250,000

## 5. 資料缺口

- PTT 散戶情緒資料未能取得（API 限制或爬取失敗）
- 部分 FinMind 資料僅更新至2026年3月，yfinance 為主要資料來源
- 無 insider trading（內部人交易）資料

## 6. 時間延遲

- 總處理時間約 25-35 分鐘（28次 API 呼叫，每次約60-120秒）
- 建議: 對於單純的每日展望，可考慮減少討論輪次以加速流程
