---
name: bayesian-intrinsic-growth-valuation
description: Use a Bayesian intrinsic-growth valuation model to evaluate whether a company's market value sufficiently, excessively, or insufficiently prices its real 3-5 year growth. Use when the user asks for Bayesian valuation, intrinsic growth rate, implied growth, growth-hypothesis probabilities, FOMO versus fundamentals, or company analysis based on fundamentals, industry cycle, TAM, market share, margin, valuation multiples, and new information.
---

# Bayesian Intrinsic Growth Valuation

## Core Principle

Do not classify company news as simply bullish or bearish. Translate every company-specific data point into a probability update for future 3-5 year revenue growth, margin, TAM, market share, valuation multiple, and market sentiment.

The goal is to estimate the company's true intrinsic growth speed and compare it with the growth already implied by the current market value.

Treat outputs as research hypotheses, not personalized investment advice. Verify current market cap, price, revenue, margins, filings, guidance, peer multiples, and news from reliable current sources before making time-sensitive claims.

## Required Inputs

Use whatever the user provides, and clearly mark missing variables that require verification:

- company fundamentals: revenue scale, margins, free cash flow, ROIC, balance sheet, customers, moat, pricing power
- industry cycle: demand growth, supply-demand gap, inventory cycle, order cycle, price trends, policy, downstream capex
- revenue and growth: historical growth, guidance, backlog, book-to-bill, organic growth, ASP, shipment volume
- TAM and TAM growth: current TAM, future TAM CAGR, penetration, market share, new market expansion
- valuation: EV/Sales, EV/EBITDA, P/E, FCF yield, PEG, historical percentile, peer percentile, implied growth
- share-price trend: 1M/3M/6M/12M and post-earnings returns, drawdown/rebound path, volatility, volume, relative performance versus sector/index, and whether price appreciation is ahead of intrinsic growth
- market FOMO: share-price move, options activity, social heat, analyst revisions, theme crowding, narrative strength
- new information: orders, customers, products, pricing, policy, competition, capacity, earnings, management guidance

### Optional SEC Data Assist

For U.S.-listed companies, use SEC filings as the baseline evidence for reported historical fundamentals. `edgartools` can be used to fetch company filings, XBRL financial statements, filing text, insider transactions, ownership filings, and recent 8-K disclosures.

If the environment does not already have it, install with `pip install edgartools` or `uv pip install edgartools`. The import package is `edgar`, not `edgartools`. SEC access requires an identity; set `EDGAR_IDENTITY="Name email@example.com"` in the environment or call `from edgar import set_identity; set_identity("name@example.com")` before requests.

Minimal usage pattern:

```python
from edgar import Company

company = Company("AAPL")
financials = company.get_financials()
income = financials.income_statement()
balance = financials.balance_sheet()
cashflow = financials.cashflow_statement()
```

Use SEC data to anchor:

- revenue history, gross margin, operating margin, EPS, free cash flow, capex, debt, cash, dilution, and share-count trends
- segment revenue, customer concentration, backlog/order language, risk-factor changes, and management's stated demand drivers
- 10-K and 10-Q trend baselines for the prior, and 8-K/earnings-release data for the latest update
- Form 4, 13D/G, and 13F data as sentiment/ownership context only, not as intrinsic-growth evidence by itself

Do not use SEC data as a substitute for current market data, consensus estimates, forward multiples, TAM estimates, option activity, or real-time price movement. If using edgartools or SEC filings, name the form and filing date, and separate "reported fact" from "analyst/market estimate."

## Growth Hypotheses

Always frame future 3-5 year revenue CAGR as probabilities across these hypotheses:

| Hypothesis | Label | 3-5Y revenue CAGR |
| --- | --- | --- |
| H0 | contraction | <0% |
| H1 | mature slow growth | 0%-5% |
| H2 | steady growth | 5%-12% |
| H3 | high-cycle growth | 12%-25% |
| H4 | structural breakout | 25%-50% |
| H5 | platform expansion | >50% |

## Workflow

### 1. Establish The Prior

Assign initial probabilities to H0-H5 using fundamentals, industry cycle, TAM, historical growth, and competitive position.

Prefer a conservative prior when evidence is incomplete. Do not let market excitement alone justify H4 or H5.

### 2. Classify New Information By Variable

When new information appears, identify which variables it affects:

- revenue growth
- margin
- TAM
- market share
- competitive structure
- cash flow
- valuation multiple
- FOMO sentiment

If information mainly affects market attention, update valuation multiple and FOMO, not intrinsic growth.

### 3. Bayesian Update

Ask how likely the new information is under each growth hypothesis:

- If the information is more consistent with H3/H4/H5, raise those probabilities.
- If it looks cyclical, one-off, or backlog timing, avoid over-updating long-term growth.
- If it only strengthens narrative or trading enthusiasm, raise FOMO and multiple risk rather than intrinsic growth.
- If it contradicts high growth, shift probability toward H0-H2.

Show the update as prior -> likelihood interpretation -> posterior.

### 4. Calculate Weighted Intrinsic Growth

Estimate weighted intrinsic 3-5 year revenue CAGR from the posterior probabilities. Use midpoint assumptions unless better evidence is available:

| Hypothesis | Suggested midpoint |
| --- | --- |
| H0 | -5% |
| H1 | 2.5% |
| H2 | 8.5% |
| H3 | 18.5% |
| H4 | 37.5% |
| H5 | 60% or scenario-specific |

Report a range, not false precision.

### 5. Reverse-Engineer Market-Implied Growth

Infer the growth rate embedded in current valuation using market cap or enterprise value, revenue, margin, FCF margin, valuation multiple, and discount-rate assumptions.

If exact data is unavailable, state the missing inputs and provide a qualitative implied-growth bracket instead of inventing numbers.

### 6. Compare Intrinsic Growth With Implied Growth

Classify valuation state:

| Comparison | Valuation state |
| --- | --- |
| intrinsic growth > implied growth | undervalued |
| intrinsic growth roughly equals implied growth | fair value |
| implied growth > intrinsic growth, but cycle still accelerating | expensive but tradable |
| implied growth far above intrinsic growth and FOMO is extreme | bubble-like |

### 7. Measure Price-Growth Divergence

Separately judge whether the share-price trend has moved faster or slower than the intrinsic-growth update.

Use current data where possible:

- compare recent share-price return, market-cap expansion, and multiple expansion with changes in revenue CAGR, guidance, backlog, margins, and posterior probabilities
- separate rerating driven by fundamentals from rerating driven by liquidity, theme crowding, short squeeze, index flows, or FOMO
- classify the divergence as `price lagging fundamentals`, `price aligned with fundamentals`, `price ahead of fundamentals`, or `severe price-growth divergence`
- when price is ahead of intrinsic growth, reduce confidence in long-term margin of safety even if the company remains high quality
- when price lags intrinsic growth, identify the catalyst needed for the market to close the gap

Suggested qualitative thresholds:

| Price move versus intrinsic-growth update | Divergence signal |
| --- | --- |
| price return materially below improved posterior growth / implied growth still below intrinsic growth | price lagging fundamentals |
| price return and multiple expansion roughly match posterior growth improvement | aligned |
| price return or multiple expansion exceeds posterior growth improvement | price ahead of fundamentals |
| rapid price rise, multiple rerating, and little/no posterior intrinsic-growth improvement | severe divergence / FOMO risk |

### 8. Build A Verification Path

Define the time window and concrete indicators that will validate or falsify the model:

- revenue growth and guidance revisions
- backlog, book-to-bill, orders, lead times
- ASP, shipment volume, utilization, capacity expansion
- gross margin, operating leverage, FCF conversion
- TAM expansion evidence and penetration change
- market-share gain or loss
- peer/customer/supplier corroboration
- analyst revision breadth and narrative crowding

## Mermaid Visualizations

For a full report, include 2-4 Mermaid diagrams when they materially improve comprehension. A short answer or data-limited analysis may use fewer. Do not create a diagram merely to meet a quota.

Prioritize these views:

1. A `pie` chart of the H0-H5 posterior probabilities after confirming they match the probability table and sum to roughly 100%.
2. A `flowchart` showing prior, new evidence, likelihood interpretation, posterior, implied growth, and valuation state.
3. An `xychart-beta` comparison of weighted intrinsic growth versus market-implied growth, or price/multiple change versus the intrinsic-growth update, only when the values and units are genuinely comparable.

Apply these rules to every diagram:

- Use fenced `mermaid` blocks, match the report language, keep node IDs in simple ASCII, and keep labels short.
- Prefer broadly supported `flowchart`, `pie`, and `stateDiagram` syntax. Use `xychart-beta`, `quadrantChart`, or `timeline` only as progressive enhancement and retain the adjacent Markdown table as the fallback.
- Use only evidence and values already stated in the report. Keep names, numbers, units, and probability totals consistent with the surrounding tables; never fill missing data for visual completeness.
- Place each diagram beside the analysis it explains and follow it with a one-sentence takeaway. Keep citations, URLs, dates, and detailed caveats outside the diagram.
- Keep a diagram focused: normally no more than 12 nodes or 8 plotted values. Diagrams supplement rather than replace the probability table, assumptions, uncertainty, and source trail.

## Output Template

Use this format for company analysis:

```markdown
## 1. 公司一句话定位
说明公司到底是什么，以及增长由什么驱动。

## 2. 当前增长假设概率表
| 假设 | CAGR 区间 | 先验概率 | 更新后概率 | 核心理由 |
| --- | --- | ---: | ---: | --- |
| H0 衰退型 | <0% |  |  |  |
| H1 低速成熟 | 0%-5% |  |  |  |
| H2 稳定成长 | 5%-12% |  |  |  |
| H3 高景气成长 | 12%-25% |  |  |  |
| H4 结构性爆发 | 25%-50% |  |  |  |
| H5 平台级扩张 | >50% |  |  |  |

紧接概率表加入 posterior 概率 Mermaid pie；图中数值必须与表格一致。

## 3. 加权内在增长速度
给出未来 3-5 年收入 CAGR 的加权区间和关键假设。

## 4. 市场隐含增长速度
反推当前市值/估值倍数隐含的增长率；若数据不足，列出需要补齐的数据。

## 5. 股价走势与内在增速背离
比较 1M/3M/6M/12M 股价、相对行业/指数表现、市值和估值倍数变化，与收入增速、指引、订单、利润率和 posterior 增长概率变化是否匹配。
给出结论：股价落后基本面 / 股价基本匹配基本面 / 股价领先基本面 / 严重背离且 FOMO 风险上升。

数据同口径时，可加入内在增长、隐含增长与价格/倍数变化的 Mermaid xychart，并保留原始数据表。

## 6. 新信息的贝叶斯更新
说明信息影响的变量、在各增长假设下的相容性，以及 posterior 变化。

加入先验→证据→似然解释→后验→估值判断的 Mermaid flowchart。

## 7. 估值状态
在 低估 / 合理 / 高估但可交易 / 泡沫化 中选择一个，并解释为什么。

## 8. 上行空间
说明需要哪些收入、利润率、TAM、市占率或倍数条件才有上行。

## 9. 下行风险
列出增长、利润率、竞争、周期、估值、FOMO 和流动性风险。

## 10. 验证周期
说明应在几个季度内验证，以及每个阶段看什么。

## 11. 关键跟踪指标
列出最重要的财报、订单、价格、产能、客户、股价相对表现、成交量、波动率、估值分位和情绪指标。

## 12. 仓位建议
用观察 / 小仓试错 / 验证后加仓 / 只交易不投资 / 降级或退出 等条件化表述，避免个性化投资指令。

## 13. 一句话结论
用一句话总结内在增长、市场隐含增长与股价走势之间的差异。
```

## Style Rules

- Start from observable demand changes, not surface narrative.
- Translate demand into revenue, profit, TAM, and valuation impact.
- Look for underpriced shovels, bottlenecks, second-position winners, hard manufacturing, and critical supply-chain nodes.
- Separate intrinsic growth updates from FOMO and multiple expansion.
- Explicitly measure whether share-price movement is leading, matching, or lagging the intrinsic-growth update.
- Distinguish structural growth from cyclical rebound or one-time order timing.
- State uncertainty, missing data, and falsification conditions clearly.

## Source Reference

The original Chinese framework is stored in `references/original-framework.md`. Read it when you need to preserve the exact wording or rebuild the model structure.

When the reference format differs, preserve its analytical intent but follow this SKILL.md's current output and visualization rules.
