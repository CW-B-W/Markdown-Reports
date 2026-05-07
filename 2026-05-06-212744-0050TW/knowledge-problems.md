# Knowledge Layer — 執行問題記錄

## 問題清單

### 1. 專家裁決格式不一致
- **問題**: 不同專家的verdict JSON結構不一致
  - Buffett/Munger/Lynch等: `{"analysis_process": "...", "verdict": {"signal": "...", "confidence": N, "reasoning": "..."}}`
  - Dalio: 使用大寫鍵名 `{"ANALYSIS PROCESS": "...", "VERDICT": {...}}`
  - Soros: verdict欄位為純文字而非巢狀JSON `"verdict": "Bullish with 80% confidence. Reasoning: ..."`
- **影響**: 首次萃取Dalio/Soros信號失敗，標記為unknown
- **解決**: 手動修正萃取邏輯後重新解析，Dalio=bullish(75%), Soros=bullish(80%)
- **建議**: 未來應在summon_expert.py中強制統一輸出格式，或加入結構驗證

### 2. 專家討論回合部分失敗
- **問題**: 15位專家中僅部分參與討論回合(如soros從bullish調整為neutral)
  - 多數專家討論後結果與原始裁決相同或未產出討論結果
- **影響**: 討論回合的附加價值有限
- **可能原因**: 討論prompt過長導致模型輸出品質下降；或專家prompt中已包含充足資訊使其無需修改

### 3. API速率限制
- **問題**: 5批次共30次DeepSeek API調用 (15裁決 + 15討論)
- **觀察**: 無明顯速率限制錯誤
- **耗時**: 每批次約2-3分鐘完成

### 4. 專家排序階段輸出格式問題
- **問題**: 專家排序(--ask-order)階段，非JSON模式輸出"No output"
- **原因**: summon_expert.py的print邏輯僅處理"verdict"鍵，排序階段使用"additional_needs"鍵
- **影響**: 需使用--json模式才能取得排序結果
- **不影響裁決品質**: 排序結果僅為診斷用途，不影響後續裁決

### 5. 數據缺口影響
- 僅有30日OHLCV資料，無法計算60日/200日均線和波動率
- 缺少即時NAV數據(僅有推估值)
- 追蹤誤差無法精確計算
- **影響**: 技術面分析完整性受限，但不影響核心估值判斷

### 6. ETF特殊性
- 0050為被動型ETF，非個股
- PEG = 1.64基於代理增長率(15%)，非真實EPS增長
- Graham Number/NCAV/DCF等估值模型不適用
- **專家需自行適應**: 多數專家將其視為「台積電+科技股組合」進行分析
