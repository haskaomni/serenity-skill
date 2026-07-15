---
name: tam-adj-peg
description: Evaluate a stock's valuation using TAM-Adj-PEG, adjusting traditional PEG by growth runway and quality. Use when the user provides a ticker or asks whether a growth stock's valuation is cheap, expensive, TAM-supported, runway-supported, quality-adjusted, or suitable as core growth, high-beta growth, turnaround, option-like, or cyclical exposure.
---

# TAM-Adj-PEG

## Core Idea

Traditional PEG asks:

```text
Is the current valuation expensive relative to future EPS growth?
```

TAM-Adj-PEG asks a broader question:

```text
How long can this growth last, is the TAM large enough, and can the company convert TAM growth into durable profits?
```

Use this framework for AI infrastructure, semiconductors, healthcare, SaaS, payment networks, high-growth manufacturers, bottleneck suppliers, early turnarounds, and option-like equities.

Treat results as research analysis, not investment advice. For latest/current scoring, verify valuation, estimates, TAM, margins, and company-specific data from current sources before calculating.

## Required Inputs

Collect the newest available data before scoring:

- Valuation: current PE or TTM PE, forward PE, and traditional PEG if available.
- Growth: expected 2-3 year EPS CAGR, revenue CAGR, TAM CAGR, and current revenue / TAM penetration.
- Profit quality: gross margin, EBIT margin, free cash flow profile, capex intensity, and dilution risk.
- Business quality: competitive position, pricing power, customer concentration, technology iteration risk, cyclicality, and key milestones.
- Preferred sources: company IR releases/presentations, earnings calls, SEC filings, consensus estimate providers, industry TAM reports, and reputable financial data sources.

For U.S.-listed companies, use SEC filings as the baseline for reported fundamentals. `edgartools` is an optional helper for retrieving latest 10-K, 10-Q, 8-K, XBRL financial statements, filing text, insider transactions, and ownership filings.

If the environment does not already have it, install with `pip install edgartools` or `uv pip install edgartools`. The import package is `edgar`, not `edgartools`. SEC access requires an identity; set `EDGAR_IDENTITY="Name email@example.com"` in the environment or call `from edgar import set_identity; set_identity("name@example.com")` before requests.

Minimal usage pattern:

```python
from edgar import Company

company = Company("AAPL")
financials = company.get_financials()
income = financials.income_statement()
cashflow = financials.cashflow_statement()
```

Use SEC data to support:

- revenue scale, segment mix, gross margin, EBIT margin, free cash flow, capex intensity, debt/cash, dilution, and share-count trends
- customer concentration, pricing language, backlog/order commentary, supply constraints, technology risk, and cyclicality disclosures
- 8-K earnings releases or guidance disclosures when they provide the newest company-reported inputs

Do not rely on SEC data alone for forward PE, EPS CAGR, consensus revisions, current valuation, TAM size, TAM CAGR, or competitive market-share estimates. Keep reported SEC facts separate from forecasts and industry assumptions, and cite the filing form/date when using SEC evidence.

If PE or EPS CAGR is not meaningful because the company is loss-making or earnings are highly volatile, mark PE/PEG as distorted and use normalized earnings, EV/Sales, milestone scenarios, or an option-style framework.

## Core Formula

Calculate:

```text
TAM-Adj-PEG = Forward PE / (EPS CAGR x TAM Runway Factor x Quality Factor)
Adjusted Growth = EPS CAGR x TAM Runway Factor x Quality Factor
```

Use EPS CAGR as a percentage number in the denominator. Example: if forward PE is 40 and adjusted growth is 50%, then `TAM-Adj-PEG = 40 / 50 = 0.8`.

Do not directly add TAM CAGR to EPS CAGR. EPS CAGR usually already reflects part of TAM expansion; TAM should mainly adjust growth duration and certainty.

## TAM Runway Factor

Use TAM Runway Factor to correct for how long high growth can continue:

```text
TAM Runway Factor = sqrt(Growth Duration / 5)
```

Rough scoring:

| High-growth duration | Factor | Interpretation |
| ---: | ---: | --- |
| 2 years | 0.6 | Short-cycle growth |
| 3 years | 0.75 | Growth exists, but runway is short |
| 5 years | 1.0 | Standard growth stock |
| 8 years | 1.25 | Long-runway compounder |
| 10 years | 1.4 | High-quality long runway |
| 15 years | 1.7 | Super long-cycle opportunity |
| 20+ years | 2.0 cap | Rare supercycle, platform, or monopoly-like asset |

Do not assign runway by sector label alone. AI-driven semiconductor bottlenecks can deserve a 15-20+ year runway when demand is structurally expanding, the company controls a hard-to-replicate choke point, and each technology generation reinforces its position. Conversely, SaaS or platform companies should not automatically receive a long runway if frontier AI models can compress workflow value, commoditize features, weaken seat-based pricing, or shift the profit pool to model/infrastructure providers.

## Quality Factor

Use Quality Factor to correct for whether growth can remain in the company's income statement:

| Quality Factor | Company Type |
| ---: | --- |
| 0.3-0.5 | Early-stage, loss-making, unproven orders, or high dilution risk |
| 0.5-0.7 | Cyclical, customer-concentrated, or high execution risk |
| 0.7-0.9 | High growth but competitive, with unstable margins |
| 0.9-1.1 | Normal high-quality growth company |
| 1.1-1.3 | Strong moat, pricing power, and customer stickiness |
| 1.3-1.5 | Monopoly-like, platform, or ecosystem asset |
| 1.5+ | Rare super-platform or AI-era bottleneck asset; use cautiously |

Evaluate nine questions:

1. Can TAM growth actually accrue to this company?
2. Does the company have pricing power?
3. Is customer concentration high?
4. Does frequent technology iteration require repeated requalification?
5. Are gross margin and EBIT margin sustainable?
6. Does growth require heavy capex?
7. Can competitors quickly catch up or become second/third sources?
8. Does growth depend on financing or share issuance?
9. Can frontier AI models erode the product's workflow value, pricing model, or distribution moat?

## Result Interpretation

| TAM-Adj-PEG | Valuation View |
| ---: | --- |
| < 0.5 | Very cheap, but verify forecasts are not overly optimistic |
| 0.5-0.8 | Clearly attractive |
| 0.8-1.2 | Reasonable to slightly cheap |
| 1.2-1.8 | Reasonable to slightly expensive; execution must continue |
| 1.8-2.5 | Expensive unless the company has a super-long runway |
| > 2.5 | Very expensive, or EPS/PE inputs are distorted |
| Not applicable | Loss-making, early-stage, or earnings too volatile; use option framework |

## Special Cases

### Loss-Making Companies

- Do not directly use PE.
- Mark PE/PEG as distorted.
- Use normalized EPS, EV/Sales, milestone scenarios, or option-style analysis.
- Focus on key milestones and financing/dilution risk.

### Cyclical Companies

- Do not use peak-cycle EPS mechanically.
- Use normalized EPS.
- Discount Quality Factor for cyclicality.
- TAM runway can add support, but supply/demand cycle risk must be deducted.
- For AI semiconductor supercycles, separate structural demand runway from inventory, capacity, and margin-cycle risk. A semiconductor leader can receive a long TAM Runway Factor if it is a durable bottleneck; the cyclical penalty should mainly flow through normalized EPS and Quality Factor, not an automatic short-runway cap.

### Turnarounds

Show two versions:

| Scenario | Treatment |
| --- | --- |
| Base case | Score current earnings power |
| Turnaround success | Score normalized profit 2-3 years out |

This avoids misclassifying a turnaround as a normal growth stock.

## Position-Type Framework

| Type | TAM-PEG Traits | Position Framing |
| --- | --- | --- |
| Core compounder | TAM-PEG 0.5-1.2 with high Quality Factor | Candidate for long-term core exposure |
| High-beta growth | TAM-PEG 0.8-1.5 with high growth and volatility | Medium exposure, track results closely |
| Turnaround | Current TAM-PEG high, success-case TAM-PEG lower | Small to medium exposure, milestone-driven |
| Option-like | PE/PEG distorted, large TAM, early execution | Small exposure, accept binary outcomes |
| Cyclical | Low PEG but discounted Quality Factor | Trade supply/demand cycle, avoid linear extrapolation |

## Mermaid Visualizations

For a full report, include 2-4 Mermaid diagrams when they materially improve comprehension. A short answer or data-limited analysis may use fewer. Do not create a diagram merely to meet a quota.

Prioritize these views:

1. A `flowchart` showing how EPS CAGR, TAM Runway Factor, and Quality Factor produce adjusted growth and TAM-Adj-PEG.
2. A `quadrantChart` comparing runway and quality for the company and peers only when at least three entities use comparable evidence and scales; retain the comparison table.
3. An `xychart-beta` comparing raw EPS growth with adjusted growth, or base and turnaround-success valuation outputs, only when the values share a clear unit.

Apply these rules to every diagram:

- Use fenced `mermaid` blocks, match the report language, keep node IDs in simple ASCII, and keep labels short.
- Prefer broadly supported `flowchart`, `pie`, and `stateDiagram` syntax. Use `xychart-beta`, `quadrantChart`, or `timeline` only as progressive enhancement and retain the adjacent Markdown table as the fallback.
- Use only evidence and values already stated in the report. Keep percentages, factor scales, valuation units, and scenario labels consistent with the surrounding tables; never fill missing data for visual completeness.
- Place each diagram beside the analysis it explains and follow it with a one-sentence takeaway. Keep citations, URLs, dates, and detailed caveats outside the diagram.
- Keep a diagram focused: normally no more than 12 nodes or 8 plotted values. Diagrams supplement rather than replace formulas, assumptions, risk adjustments, and source trails.

## Output Format

Use this structure for every ticker:

```markdown
# TICKER: TAM-Adj-PEG 估值分析

公司：XXX
股票代码：XXX

1. 当前估值
- 当前 PE：
- Forward PE：
- 传统 PEG：

2. 增长拆解
- 未来 EPS CAGR：
- Revenue CAGR：
- TAM CAGR：
- 当前收入 / TAM：
- 高速增长 runway：

3. TAM Runway Factor
- 估计值：
- 原因：

4. Quality Factor
- 估计值：
- 加分项：
- 扣分项：

5. TAM-Adj-PEG
公式：
TAM-Adj-PEG = Forward PE / (EPS CAGR x TAM Runway Factor x Quality Factor)

计算：
- 修正后增长率：
- TAM-Adj-PEG：

加入 Mermaid flowchart 展示三个输入如何传导到修正后增长率和最终估值；保留完整公式与计算。

如有可比公司或双情景的同口径数据，可加入 runway—quality 矩阵或增长对比 xychart，并保留数据表。

6. 结论
- 估值档位：
- 主要上行驱动：
- 主要下行风险：
- 适合的仓位类型：
```

## Detailed Reference

Read `references/original-framework.md` when a task needs the full Chinese framework text, all scoring tables, or the original explanation of TAM-Adj-PEG logic.

When the reference format differs, preserve its analytical intent but follow this SKILL.md's current output and visualization rules.
