# Information Agent — 問題日誌

## 3008.TW (大立光) | 2026-05-06

### 已解決問題

| 問題 | 原因 | 處理方式 |
|------|------|---------|
| `financial_metrics.py` 技術指標計算失敗 | 該腳本依賴 yfinance 取得股價數據，但 yfinance 未安裝於環境中 | 直接透過 FinMind API 獲取246個交易日股價，自行計算 SMA/RSI/MACD/Bollinger |
| FinMind client `stock` 指令僅回傳30筆 | `finmind_client.py:243` 硬編碼 `data[-30:]` 限制 | 改用 FinMind REST API 直接查詢 |
| yfinance client 僅回傳21筆 | yfinance default period 僅30天 | FinMind API 成功獲取完整1年數據 |

### 持續限制

| 限制 | 影響 |
|------|------|
| 同業比較數據不足 | 未取得玉晶光(3406)等競爭對手的及時財務數據，同業比較欄位較簡略 |
| 還原股價不可用 | 需 FinMind Backer 方案，無法計算完全精確的技術指標 |
| SMA200 僅勉強足夠 | 取得246個交易日，剛好超過200日線所需的數據量 |
| DCF估值對成長率敏感 | 採用保守3%成長率，若實際成長更高則低估程度更大 |

### 注意事項

1. MACD信號線計算使用簡化算法（9筆MACD值的SMA），與標準EMA算法有微小差異
2. EV/EBITDA為概算值，未逐項調整現金等價物與長短期投資
3. PTT情緒數據反映散戶觀點，不代表機構法人立場
