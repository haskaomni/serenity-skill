---
name: serenity-alpha
description: Translate market-moving news into investable alpha hypotheses by mapping observed demand changes to revenue lines, supply chains, small-cap financial elasticity, market misclassification, validation metrics, downside risks, and position-sizing conditions. Use when the user shares news, product launches, technology breakthroughs, procurement signals, supply-chain changes, earnings-call clues, or asks for Serenity-style alpha analysis, small-cap beneficiaries, or "news to financial statement" translation.
---

# Serenity Alpha

## Core Principle

Do not ask only whether the news is impressive. Ask whether an already-observable demand change can rewrite a smaller company's financial statements.

Use this skill to convert news into a testable alpha hypothesis:

```text
news -> observed demand change -> revenue/profit transmission -> small-cap elasticity -> validation path
```

Treat outputs as research hypotheses, not investment advice. Verify current prices, market caps, filings, earnings calls, and news from reliable sources before naming securities or making time-sensitive claims.

## Optional SEC Data Assist

For U.S.-listed companies, use SEC filings as the factual base for reported fundamentals and management disclosure when available. `edgartools` is a good optional helper for this step because it can retrieve company filings, XBRL financial statements, filing text, insider transactions, ownership forms, and recent 8-K disclosures.

If the environment does not already have it, install with `pip install edgartools` or `uv pip install edgartools`. The import package is `edgar`, not `edgartools`. SEC access requires an identity; set `EDGAR_IDENTITY="Name email@example.com"` in the environment or call `from edgar import set_identity; set_identity("name@example.com")` before requests.

Minimal usage pattern:

```python
from edgar import Company

company = Company("AAPL")
filings = company.get_filings(form="10-Q")
financials = company.get_financials()
income = financials.income_statement()
```

Use it to support the analysis, not to replace the framework:

- Pull the latest 10-K, 10-Q, and relevant 8-K filings before judging whether a demand change has reached reported numbers.
- Use XBRL financials for historical revenue, gross profit, margin, cash flow, balance sheet, share count, and segment clues.
- Search filing text and MD&A for demand-driver terms, customer concentration, backlog/order commentary, capacity, pricing, supply constraints, and risk-factor changes.
- Check Form 4, 13D/G, or 13F data only as supporting context; do not treat ownership activity as proof of the alpha thesis.
- Keep non-SEC data separate: current price, market cap, valuation multiples, sell-side estimates, TAM, channel checks, and industry pricing usually require other current sources.

When using SEC data, cite the filing form and filing date in the reasoning, and state when the data is stale relative to the news item.

## Answer Shape

Start and end with the company that best fits the alpha hypothesis.

- Open with a direct one-sentence call: `最值得优先验证的是：Company / ticker，原因是...`
- If several names appear, choose a primary candidate and label others as alternatives or supply-chain comparables.
- If no company is investable yet, say so at the top: `暂时没有足够明确的公司，先观察...`
- Close by repeating the same primary company and the single validation condition that would confirm or kill the thesis.

## Workflow

### 1. Separate Talk From Demand

Identify whether the item is merely narrative or a demand inflection. Prioritize evidence that has already happened:

- users are adopting or paying
- enterprises are buying or expanding deployments
- suppliers are shipping, backlogged, or raising prices
- production schedules are tightening
- earnings calls or filings mention the demand
- a niche ecosystem is becoming crowded

If demand is not observable, classify the idea as watchlist-only.

### 2. Translate Phenomenon Into Financial Lines

Use this sentence structure:

```text
Because X is happening, demand for Y is increasing.
That demand can flow to A/B/C companies through revenue line Z.
Company N may matter because its market cap, revenue base, or business purity is small enough for the demand shock to change reported results.
```

For each candidate, map the mechanism to income-statement or balance-sheet items:

- revenue: units, ASP, customer count, usage, take rate, backlog conversion
- gross margin: mix shift, pricing power, utilization, input costs
- operating leverage: fixed-cost absorption, sales efficiency, R&D leverage
- cash flow: working capital, capex needs, inventory turns, prepayments

### 3. Prefer Small, Pure, Misclassified Picks

Look for "small shovels," not only obvious mega-cap winners. Rank candidates higher when they have:

- small market cap or low revenue base versus the size of the demand shock
- high business purity to the specific demand vector
- key supply-chain position or scarcity value
- low current investor attention
- stale market label that misses the new role
- financial verification likely within 1-4 quarters

Use this rough screen:

```text
alpha elasticity ~= incremental demand impact / current company scale
```

### 4. Test Market Misclassification

Ask:

- What does the market currently think this company is?
- What could it actually be becoming if the demand change persists?
- Is the new label large enough, durable enough, and close enough to reported numbers to matter?

Misclassification creates alpha only when the relabeling can be validated by numbers, not just story.

### 5. Build a Verification Chain

Convert the thesis into observable checkpoints. Use the most relevant indicators:

- revenue growth or guidance revisions
- gross-margin improvement or mix disclosure
- backlog, book-to-bill, lead times, or order commentary
- inventory drawdown or channel checks
- utilization, capacity expansion, or capex plans
- ASP changes or pricing commentary
- customer concentration changes
- management mentioning the demand driver unprompted
- competitor/supplier/customer corroboration

Define what would confirm, weaken, or falsify the thesis.

### 6. Score Alpha Strength

Score each candidate qualitatively or on a 1-5 scale:

| Dimension | Question |
| --- | --- |
| Demand certainty | Is this already occurring, or only imagined? |
| Transmission clarity | Can the demand clearly flow to named companies? |
| Business purity | Is the company directly exposed? |
| Market-cap elasticity | Is the company small enough for the impact to matter? |
| Market neglect | Is the market missing or mislabeling it? |
| Verification speed | Can filings or calls verify this in 1-4 quarters? |
| Downside risk | What happens if the thesis is wrong? |

Prioritize ideas with high certainty, clear transmission, high purity, high elasticity, and near-term verification.

### 7. Size By Evidence

Frame position posture as conditional research guidance, not a personalized recommendation:

| Thesis state | Posture |
| --- | --- |
| Demand seems real, transmission unclear | observe / very small exploratory size |
| Demand real, transmission clear, no financial proof yet | small test position if risk fits |
| Financials begin validating and market still underprices | consider adding |
| Thesis becomes consensus and valuation stretches | lower return expectations / trade only |
| Key assumptions are falsified | exit or remove from watchlist |

## Mermaid Visualizations

For a full report, include 2-4 Mermaid diagrams when they materially improve comprehension. A short answer or data-limited analysis may use fewer. Do not create a diagram merely to meet a quota.

Prioritize these views:

1. A `flowchart` from news to observed demand, financial-statement lines, beneficiaries, and the decisive validation condition.
2. A candidate map comparing evidence strength with financial elasticity when at least three candidates have comparable inputs; use `quadrantChart` only with a nearby comparison table.
3. A 1-4 quarter verification path showing confirm, weaken, and falsify branches.

Apply these rules to every diagram:

- Use fenced `mermaid` blocks, match the report language, keep node IDs in simple ASCII, and keep labels short.
- Prefer broadly supported `flowchart`, `pie`, and `stateDiagram` syntax. Use `xychart-beta`, `quadrantChart`, or `timeline` only as progressive enhancement and retain the adjacent Markdown table as the fallback.
- Use only evidence and values already stated in the report. Keep names, numbers, units, and probability totals consistent with the surrounding tables; never fill missing data for visual completeness.
- Place each diagram beside the analysis it explains and follow it with a one-sentence takeaway. Keep citations, URLs, dates, and detailed caveats outside the diagram.
- Keep a diagram focused: normally no more than 12 nodes or 8 plotted values. Diagrams supplement rather than replace the written transmission logic, risks, and source trail.

## Output Template

When analyzing a news item, use this structure:

```markdown
## 结论先行：优先验证的公司
点名最值得优先验证的一家公司 / ticker，并用一句话说明为什么它是本次新闻里最有弹性的候选。

## A. 表层新闻
简述新闻表面信息。

## B. 已发生的需求变化
说明已经可观察的需求、采购、使用、价格、排产或供应链变化；如果没有，明确说只是谈资。

## C. 财务翻译
把需求映射到收入项、成本项、利润项、现金流或资产负债表项目。

在这里或下一节加入需求传导 Mermaid flowchart，展示新闻如何进入财务报表并最终影响候选公司。

## D. 受益链条
列出一阶、二阶、三阶受益者，并说明传导距离。

## E. 小市值高弹性标的
列出可能的小盘/高纯度/低关注候选；标注需要核实的市值、收入基数和业务占比。

如果至少三个候选具有可比数据，可加入证据强度—财务弹性矩阵，并保留候选比较表。

## F. 市场误分类
说明市场现在把公司当什么，它可能正在变成什么。

## G. 验证指标
列出未来 1-4 个季度要看的财报、电话会、供应链和价格指标。

加入验证路径图，明确确认、削弱和证伪分支。

## H. 下行风险
列出需求、传导、竞争、估值、时点和流动性风险。

## I. 仓位建议
给出观察、小仓、加仓、放弃的条件；避免承诺收益或给出个性化投资指令。

## 最后一句
再次点名这家公司，并给出最关键的财报验证条件；如果条件不成立，明确说应放弃或降级为观察。
```

## Quality Bar

- Anchor the analysis in observable demand, not vibes.
- Prefer named revenue mechanisms over broad themes.
- Distinguish first-order beneficiaries from distant second- or third-order stories.
- Use current sourced data for market caps, financials, prices, and recent filings.
- State uncertainty and falsification conditions clearly.

When `references/original-framework.md` is consulted, preserve its analytical intent but follow this SKILL.md's current output and visualization rules when the formats differ.
