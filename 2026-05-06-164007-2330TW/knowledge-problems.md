# Knowledge Layer Problems — 2330 (台積電)

**Generated**: 2026-05-06

## 1. Missing Expert JSONs (6 experts created)

6 of the 15 requested experts had no JSON persona definition file in `plugin/agents/knowledge/experts/`. Created temporary JSON files based on existing templates:

| Expert | Key | Status |
|--------|-----|--------|
| Ray Dalio | `dalio.json` | ✅ Created (macro/debt cycle framework) |
| Joel Greenblatt | `greenblatt.json` | ✅ Created (magic formula: ROC + earnings yield) |
| Howard Marks | `marks.json` | ✅ Created (risk-first, market cycles, second-level thinking) |
| George Soros | `soros.json` | ✅ Created (reflexivity theory, boom-bust cycles) |
| Jim Simons | `simons.json` | ✅ Created (quantitative momentum/mean reversion) |
| David Tepper | `tepper.json` | ✅ Created (contrarian macro, asymmetric bets) |

**Note**: These JSON files were written to the project's `plugin/agents/knowledge/experts/` directory. They should be reviewed and refined before permanent inclusion.

## 2. Signal Extraction Issues

The `summon_expert.py` script returns LLM output as a raw text field (not structured JSON). Signal and confidence must be extracted via regex from the `verdict` field. For Graham and Cathie Wood, initial regex failed to match signal ("unclear" reported). Manual inspection confirmed:
- Graham: **bearish** (95%)
- Wood: **neutral** (85%)

## 3. Two-Round Discussion

Protocol executed correctly:
- Round 1: 15 experts submitted verdicts (5 bullish, 5 neutral, 5 bearish)
- Round 2: Peer-reviewed discussion round with all verdicts shared
- 3 experts changed stance:
  - Ray Dalio: bullish → neutral (macro headwinds outweighed AI productivity)
  - George Soros: bullish → neutral (reflexivity cycle maturing, foreign selling)
  - Michael Burry: neutral → bearish (confirmed crowded trade + zero margin of safety)

## 4. Data Limitations

The information report noted several data gaps that affected some expert analyses:
- SMA60 unavailable (only 30 days of price data)
- No crude oil data (WTI API empty)
- yfinance fundamentals unavailable in venv
- Some experts requested metrics not available in report (e.g., Taiwan CDS spread, PMI data)

## 5. API Rate and Performance

- All 30 DeepSeek API calls (15 orders + 15 verdicts + 15 discussions = 45 total across 3 phases) completed successfully
- No rate limiting errors encountered
- DeepSeek v4-pro with thinking mode enabled for verdict phases
- Typical token usage: 5,000-8,000 tokens per expert per phase
