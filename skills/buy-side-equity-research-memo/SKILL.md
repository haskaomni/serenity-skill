---
name: buy-side-equity-research-memo
description: Generate source-backed buy-side equity research memos from a ticker, starting with investment view, target-price scenarios, SEC and IR-backed financial statement analysis, industry chain, competition, valuation, catalysts, variant perception, risks, and monitoring dashboard. Use when the user asks to analyze a stock or ticker, create a buy-side memo, investment committee note, deep equity research report, SOTP or scenario-driven stock analysis, or fund-manager-style company analysis.
---

# Buy-Side Equity Research Memo

## Core Principle

Start with the investment decision, not company background. Convert a ticker into a buy-side memo that helps a fund manager or investment committee debate whether the stock is mispriced, what could change the view, and what must be tracked next.

Treat outputs as research analysis, not personalized investment advice. For current claims, verify latest filings, prices, market cap, estimates, valuation multiples, earnings calls, investor presentations, and catalysts from reliable current sources. Do not invent unavailable data.

## Defaults And Inputs

Required input: a ticker or clearly identifiable listed company.

Default assumptions unless the user specifies otherwise:

- language: Chinese
- depth: deep version
- horizon: 12-month primary view, with 3-6 month catalyst tracking
- style: buy-side fundamental research memo
- valuation: SOTP when segment structure matters; otherwise relative valuation, DCF, P/E, EV/EBITDA, P/FCF, or industry-specific methods
- scenarios: Bull / Base / Bear with probability weights and implied upside/downside

If the ticker is ambiguous, identify the most likely listed company and state the assumption. Ask only when multiple candidates are genuinely plausible and the wrong choice would change the analysis.

## Source Discipline

Prioritize primary and current sources:

1. SEC filings or local exchange filings
2. company IR website, earnings releases, and investor presentations
3. earnings call transcripts and official guidance
4. exchange announcements and regulatory filings
5. industry associations and credible third-party data
6. reputable financial data providers and mainstream financial media

For every important figure or factual claim, cite the source, document type, document date or filing date, access date, and section/page/path when available. Clearly distinguish:

- reported facts
- management guidance or commentary
- market consensus or third-party estimates
- analyst inference

If a number cannot be verified, mark it as `未核验` and explain what source would be needed to confirm it.

## Optional SEC Data Assist

For U.S.-listed companies, use SEC filings as the factual baseline for reported fundamentals and management disclosure when available. Prioritize latest `10-K`, `10-Q`, `8-K`, and, for foreign private issuers, `20-F` or `6-K`. Use XBRL financial statements when available.

`edgartools` is a useful optional helper because it can retrieve filings, XBRL financial statements, filing text, insider transactions, ownership forms, and recent 8-K disclosures.

If the environment does not already have it, install with `pip install edgartools` or `uv pip install edgartools`. The import package is `edgar`, not `edgartools`. SEC access requires an identity; set `EDGAR_IDENTITY="Name email@example.com"` in the environment or call `from edgar import set_identity; set_identity("name@example.com")` before requests.

Minimal usage pattern:

```python
from edgar import Company

company = Company("AAPL")
filings = company.get_filings(form="10-K")
financials = company.get_financials()
income = financials.income_statement()
balance = financials.balance_sheet()
cashflow = financials.cashflow_statement()
```

Use SEC data to anchor:

- revenue, segment revenue, gross margin, operating margin, EPS, free cash flow, capex, debt, cash, share count, dilution, buybacks, and stock-based compensation
- customer concentration, backlog/order language, pricing, capacity, inventory, MD&A, risk factors, legal/regulatory disclosures, and management's stated demand drivers
- 8-K earnings releases or guidance disclosures when they contain the newest company-reported operating data

Do not use SEC data as a substitute for current market data, forward multiples, consensus revisions, TAM estimates, market share, technical data, or catalyst calendars. Keep SEC-reported facts separate from forecasts and market estimates.

Citation examples:

```text
Source: FY2025 Form 10-K, Item 7, filed 2026-02-21, accessed 2026-06-12.
Source: Q1 FY2026 Form 10-Q, Note 15 Segment Information, filed 2026-05-03, accessed 2026-06-12.
```

If filings cannot be retrieved, say:

```text
未能核验原始 SEC filing，本部分基于可获得资料，需后续人工复核。
```

## Workflow

### 1. Define Company And Research Boundary

Identify the company name, exchange, ticker, fiscal year, reporting currency, primary business, segment structure, latest filings, latest earnings release/call, latest investor presentation, and key disclosure limits.

### 2. Write Executive Investment View First

Open the memo with:

- rating bias: Buy / Hold / Sell / Avoid / Watchlist
- 12-month target-price range: Base / Bull / Bear
- current price and implied upside/downside when current price is available
- core thesis in 3-5 sentences
- key debate and variant perception
- what the market may be missing
- thesis breakpoint that would force downgrade, exit, or re-underwriting

If current price or target inputs are unavailable, state the missing data and provide a qualitative view instead of false precision.

### 3. Map Industry Chain And Profit Pool

Explain where the company sits in the value chain, who suppliers and customers are, who has pricing power, where the profit pool is, what resource is scarce, and whether the company is in a strong or weak value-chain position.

### 4. Analyze Competition And Moat Direction

Compare direct competitors, substitutes, potential entrants, customer/supplier bargaining power, market-share direction, margin differences, brand/channel/scale/network/data/R&D advantages, and whether the moat is widening or narrowing.

### 5. Read Financial Statements Like A Buy-Side Analyst

Cover revenue mix, segment profitability, gross margin, operating margin, R&D, sales and marketing, G&A, operating cash flow, free cash flow, capex, inventory, receivables, deferred revenue, debt, cash, SBC, buybacks, dilution, and management guidance.

Focus on what changes the thesis: acceleration/deceleration, margin quality, working-capital stress, capital intensity, accounting quality, and whether guidance confirms or contradicts the story.

### 6. Identify 3-7 Key Value Drivers

Select the variables that truly drive valuation, such as units, ASP, ARPU, NRR, users, backlog, book-to-bill, utilization, gross margin, take rate, inventory cycle, customer concentration, capex intensity, credit quality, or regulatory cost.

For each driver, state why it matters, past trend, market expectation, management guidance, valuation sensitivity, and the metric that would confirm or falsify the thesis.

### 7. Build Valuation And Scenarios

Use SOTP when business segments deserve different multiples or growth assumptions. For each material segment, include revenue, growth, margin, comparable companies, valuation multiple, value range, and premium/discount rationale.

When SOTP is not suitable, use the most relevant method for the business model: EV/Revenue, EV/EBITDA, P/E, P/FCF, DCF, NAV, normalized earnings, or milestone/option-style valuation.

Always include Bull / Base / Bear scenarios with assumptions, probability, revenue growth, margin, multiple, target price, implied return, and trigger conditions. Report probability-weighted target value and risk/reward when enough data exists.

### 8. Challenge The Thesis

Actively argue against the conclusion. Identify where the thesis can be wrong, what consensus may be too optimistic about, what risks are not priced, which metrics would break the thesis, and what conditions require downgrade or removal from the watchlist.

Cover fundamental, valuation, competition, cycle, regulation, technology substitution, management execution, accounting quality, liquidity, FX, and macro risks when relevant.

### 9. Track Catalysts And Monitoring Dashboard

List the next 3-6 month catalysts: earnings dates, investor days, product launches, regulatory nodes, industry conferences, major contracts, M&A progress, capacity ramps, price changes, guidance updates, index changes, and capital-return policy changes.

For each catalyst, include time window, event, market expectation, likely impact direction, and tracking metrics. End with a dashboard of the most important filings, operating metrics, management comments, and falsification points.

## Serenity Cross-Checks

Use existing Serenity frameworks as lenses when they fit the company or user request:

- `serenity-alpha`: when the thesis depends on news, product launches, procurement signals, supply-chain changes, or market misclassification. Translate news into observable demand and financial-statement impact.
- `bayesian-intrinsic-growth-valuation`: when the key question is whether intrinsic 3-5 year growth probability is above or below market-implied growth.
- `tam-adj-peg`: when valuation depends on high growth, TAM runway, growth duration, and business quality rather than a simple PEG.
- `gf-dma-health-index`: when the user asks whether the current trend or entry point is healthy, or when price/DMA/ATR and estimate-revision data are available.
- `juglar-cycle-stock-stage`: when the thesis depends on industry capex, ASP, inventory, supply-demand, capacity release, customer order behavior, or migration across recovery, expansion, overheating, downturn, and clearing phases.

Do not force every cross-check into every memo. Add only the modules that improve the investment decision.

## Mermaid Visualizations

For a full memo, include 2-4 Mermaid diagrams when they materially improve the investment decision. Prefer 3-4 for a deep memo, while a short or data-limited memo may use fewer. Do not create a diagram merely to meet a quota.

Prioritize these views:

1. A `flowchart` of the value chain, company position, pricing power, and profit pool.
2. An `xychart-beta` for SOTP segment values or Bull/Base/Bear target prices when the values use the same currency and date basis.
3. A `timeline` for the next 3-6 months of catalysts, with the catalyst table retained as the fallback.
4. A compact `flowchart` connecting the core thesis, supporting evidence, variant perception, and thesis breakpoint.

Apply these rules to every diagram:

- Use fenced `mermaid` blocks, match the report language, keep node IDs in simple ASCII, and keep labels short.
- Prefer broadly supported `flowchart`, `pie`, and `stateDiagram` syntax. Use `xychart-beta`, `quadrantChart`, or `timeline` only as progressive enhancement and retain the adjacent Markdown table as the fallback.
- Use only evidence and values already stated in the memo. Keep currency, valuation date, units, scenario probabilities, and target prices consistent with the surrounding tables; never fill missing data for visual completeness.
- Place each diagram beside the analysis it explains and follow it with a one-sentence investment takeaway. Keep citations, URLs, dates, and detailed caveats outside the diagram.
- Keep a diagram focused: normally no more than 12 nodes or 8 plotted values. Diagrams supplement rather than replace analysis, valuation tables, risks, and source trails.
- When a Serenity cross-check is used, import only the one diagram with the highest decision value. Do not repeat the same data in both the parent memo and a cross-check chart, and keep the whole memo within the 2-4 diagram budget.

## Output Template

Use this default structure:

```markdown
# [Company / Ticker] Buy-Side Equity Research Memo

## 0. Executive Investment View
- Rating Bias:
- 12M Target Price Range:
- Current Price:
- Implied Upside / Downside:
- Base / Bull / Bear Case:
- Core Thesis:
- Key Debate:
- What the Market May Be Missing:
- Thesis Breakpoint:

## 1. Company Snapshot
- Business Overview
- Revenue / Segment / Geography Mix
- Customer Profile
- Reporting Currency And Fiscal Year
- Latest Source Set

## 2. Industry Chain Position
- Value Chain Map
- Profit Pool
- Bargaining Power
- Secular Drivers
- Cyclical / Structural Risks

Add a Mermaid value-chain flowchart here and state the profit-pool takeaway immediately after it.

## 3. Competitive Landscape
- Key Competitors
- Market Share Direction
- Moat Assessment
- Margin Comparison
- Threats And Substitutes

## 4. Financial Statement Deep Dive
- Revenue And Segment Trends
- Gross Margin And Operating Margin
- Cash Flow, Capex, And Working Capital
- Balance Sheet, Debt, Cash, Dilution, SBC
- Guidance And Management Commentary

## 5. Key Value Drivers
- Driver 1
- Driver 2
- Driver 3
- Valuation Sensitivity

## 6. SOTP / Valuation
- Segment Or Business-Line Valuation
- Comparable Companies And Multiple Assumptions
- Net Cash / Debt And Dilution Adjustment
- Implied Equity Value And Target Price

When segment values are comparable, add a Mermaid xychart and retain the SOTP table.

## 7. Bull / Base / Bear Scenarios
| Scenario | Probability | Core Assumptions | Target Price | Implied Return |
| --- | ---: | --- | ---: | ---: |
| Bull | | | | |
| Base | | | | |
| Bear | | | | |

When target prices share the same currency and valuation date, add a Mermaid xychart and retain this scenario table.

## 8. Variant Perception
- Where Consensus May Be Wrong
- What Is Underappreciated
- What Would Break The Thesis

## 9. Catalysts: Next 3-6 Months
- Event
- Timing
- Market Expectation
- Tracking Metrics

Add a Mermaid timeline when event windows are known; retain the catalyst table as the compatibility fallback.

## 10. Key Risks
- Fundamental Risks
- Valuation Risks
- Competitive Risks
- Regulatory Risks
- Thesis Breakpoints

## 11. Monitoring Dashboard
- Metrics To Track
- Filing Items To Review
- Management Commentary To Watch

## 12. Source List
- SEC / Exchange Filings
- Earnings Calls And IR Materials
- Industry And Market Sources
- Access Dates
```

## Quality Bar

- Lead with the investment conclusion.
- Support each core judgment with data, source documents, or explicit reasoning.
- Do not replace analysis with news summaries.
- Do not extrapolate historical growth mechanically.
- Show valuation assumptions, not only a target price.
- Include Bull / Base / Bear scenarios and reverse thesis testing.
- Make all unverifiable data and missing inputs explicit.
- Keep the memo decision-useful, professional, and concise enough for buy-side discussion.

## Detailed Reference

The original Chinese framework is stored in `references/original-framework.md`. Read it when you need the full source draft, exact prompt language, section-by-section checklist, or industry-specific driver examples.

When the reference format differs, preserve its analytical intent but follow this SKILL.md's current output and visualization rules.
