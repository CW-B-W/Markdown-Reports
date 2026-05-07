# Insight Problems Log — 3017.TW

**Session**: 2026-05-06-150521-3017TW  
**Date**: 2026-05-06

---

## Issues Found During Synthesis

### 1. Expert Distribution Counting Error (knowledge-report.md)
- **Severity**: Low (cosmetic)
- **Issue**: Signal distribution table reports 9 看空 + 1 未確定, but individual verdicts clearly show 10 看空 experts (Buffett, Graham, Burry, Pabrai, Lynch, Munger, Druckenmiller, Taleb, Bearish Advocate, Ackman). The "1 未確定" is a phantom entry — no expert has "未確定" signal.
- **Resolution**: Corrected in report.md (10 看空, 66.7%). Not re-delegated to @knowledge — flagged here and corrected downstream.

### 2. Confidence-Weighted Sentiment Discrepancy
- **Severity**: Low
- **Issue**: knowledge-report.md reports sentiment index of -0.575. @insight re-computation yields approximately -0.608 (bear: 810, bull: 137, neutral: 160, total: 1107; (137-810)/1107 = -0.608).
- **Resolution**: Reported as "約 -0.61" in report.md. Minor difference — does not change verdict direction.

### 3. Missing Geopolitical Risk Analysis
- **Severity**: Medium
- **Issue**: Neither knowledge-report.md nor information-report.md addresses Taiwan-China geopolitical risk (台海緊張、美中科技戰、供應鏈去風險化). For a Taiwan large-cap tech stock in the AI supply chain, this is a material omission.
- **Resolution**: Added geopolitical risk section in report.md by @insight. Not re-delegated — addressed in synthesis layer.

### 4. Missing International Macro Data
- **Severity**: High (flagged but unresolved)
- **Issue**: Information report flags missing S&P 500, Nasdaq, 10Y Treasury yield, and SOX index data. FinMind API returned empty for these. DCF valuation and global tech sentiment assessment are incomplete without these.
- **Resolution**: Cannot resolve at @insight layer — would require web search or alternative data source. Flagged in report.md under "資料缺口與限制."

### 5. Expert Substitutions
- **Severity**: Informational
- **Issue**: 4 of 15 experts (Terry Smith, Ray Dalio, George Soros, Jim Simons) not available in expert JSON definitions. Replaced by Ackman, Damodaran, Jhunjhunwala, Taleb respectively.
- **Resolution**: Accepted. Substitutions are reasonable style-matches. Documented in report.md.

### 6. No Peer Comparison Data
- **Severity**: Medium (flagged but unresolved)
- **Issue**: Information report notes that peer comparison data (3324 雙鴻, 2421 建準) was not collected by @data. Relative valuation impossible without this.
- **Resolution**: Cannot resolve at @insight layer. Flagged in report.md.

---

## Decision: Proceed Without Re-Delegation
After grilling myself, I determined these issues are either:
- Cosmetic (counting errors) — corrected in synthesis
- Addressable at insight layer (geopolitical risk) — added directly
- Unresolvable at insight layer (missing macro data, peer data) — flagged for future sessions

A re-delegation to @knowledge would not materially improve the verdict given the one-grill-round constraint. The core signal (bearish, 10/15 experts, PE 49.78x, DCF 30% premium, institutional selling) is robust and consistent across all reports.
