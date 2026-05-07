# Information Agent 問題記錄 — 6533.TW

## 資料取得問題

1. **FinMind 股價僅 30 個交易日**
   - 影響: 無法直接從 data-report 計算 SMA50/SMA200/RSI14/MACD
   - 解決: 使用 yfinance 補足 200 個交易日價量資料，Python 自算技術指標
   - 狀態: ✅ 已解決

2. **yfinance_client.py 路徑不匹配**
   - 問題: financial_metrics.py 在 `plugin/agents/information/scripts/` 目錄尋找 `yfinance_client.py`，但實際上存在於 `plugin/agents/data/scripts/`
   - 影響: `financial_metrics.py analyze` 命令無法正常工作 (fundamentals 與 technical 都回傳 null)
   - 解決: 手動使用 Python + yfinance 直接取得資料並運算
   - 狀態: ✅ 已解決 (繞過)

3. **RISC-V 特定業務指標不可得**
   - 影響: IP 授權 vs 權利金收入拆分、客戶數、客戶集中度等無法從 FinMind/yfinance 取得
   - 狀態: ⚠️ 永久限制 (需年報/法說會資料)

4. **地區營收拆分不可得**
   - 影響: 台灣 vs 國際營收佔比無法從 FinMind/yfinance 取得
   - 狀態: ⚠️ 永久限制 (需年報資料)

5. **同業比較數據不完整**
   - 影響: M31 (6643.TW) 在 yfinance 無資料，無法進行同業估值比較
   - 建議: 後續可手動查詢或使用 FinMind 直接取得

6. **FinMind 月營收未提供年增率**
   - 影響: YoY 欄位全部為空，需手動計算
   - 狀態: ✅ 已計算完成

7. **股價與用戶預期差異**
   - 用戶提到股價 ~NT$182-196，但實際最新收盤為 NT$241.50
   - 原因: 股價在 4 月下旬急漲 (4/22 觸及漲停 253)，月漲幅 +38.4%
   - 狀態: ℹ️ 已於報告反映

## 腳本問題

1. **financial_metrics.py** — `yfinance_client.py` 依賴路徑錯誤，導致 analyze 命令完全失效
2. **valuation.py** — 運作正常，但 Graham Number 因為 EPS 為負而無法計算 (正確行為)
3. **valuation.py DCF** — 使用保守參數 (g=5%, r=10%)，結果為 NT$62.84，遠低於現價

## 備註

- 使用 `uv run --project` 執行腳本時找不到 yfinance (無 .venv)。改用 `python3` 直接執行解決。
- 技術指標為自算 Python 腳本，與標準 TA-lib 可能有微小差異。
