---
name: juglar-cycle-stock-stage
description: Classify a listed company and its core industry into Juglar fixed-asset investment cycle stages with probabilities, evidence, counter-evidence, migration signals, and investment implications. Use when the user provides a ticker/company or asks about 朱格拉周期, Juglar cycle, capex cycle, fixed-asset investment cycle, industry cycle phase, recovery, expansion, overheating, downturn, clearing, inventory cycle, capacity cycle, ASP, or supply-demand cycle positioning.
---

# Juglar Cycle Stock Stage

## Core Principle

Judge the company's Juglar cycle position from the industry fixed-asset investment cycle first, then the company's own operating phase second. Do not classify from share price, PE, or narrative alone.

Use the framework to answer:

```text
Where are the company and its core industry in the fixed-asset investment cycle, and what evidence would move the judgment to the next phase?
```

Treat outputs as research analysis, not personalized investment advice. For latest/current scoring, verify market price, filings, earnings, guidance, industry supply-demand, capex plans, inventory, ASP, backlog, and estimate revisions from current sources before making time-sensitive claims.

## Required Inputs

Required input: a ticker or clearly identifiable listed company.

Optional user inputs:

- market: US, HK, A股, 台股, 日股, 韩股, etc.
- horizon: 6 months, 12 months, 24 months, or 3-5 years
- cost basis or position size for risk framing only; do not let it affect cycle classification

If the ticker is ambiguous, identify the most likely listed company and state the assumption. Ask only when multiple candidates are genuinely plausible and the wrong choice would change the analysis.

## Required Evidence

Collect and separate industry-cycle evidence from company-specific evidence:

- demand: orders, shipments, backlog, book-to-bill, utilization, customer capex, downstream demand
- price / ASP: product pricing, contract prices, commodity prices, discounting, pricing power
- profitability: gross margin, operating margin, EPS, loss/profit inflection, operating leverage
- capex: company capex, peer capex, industry capacity announcements, fab/plant/vessel/equipment orders
- inventory: company inventory, channel inventory, customer inventory, days inventory, destocking/restocking
- capacity pressure: new capacity timing, utilization, supply bottlenecks, lead times, future overcapacity risk
- customer behavior: rush orders, long-term agreements, replenishment, order deferrals, price pressure, cancellations
- capital-market reaction: price trend, valuation, estimate revisions, sentiment, whether stock pricing is ahead of cycle evidence

For U.S.-listed companies, SEC filings can anchor reported revenue, margins, capex, inventory, backlog/order language, capacity plans, customer concentration, and risk factors. Use current market and industry sources for ASP, consensus estimates, stock trend, and competitive capex.

## Five Stages

| Stage | Phase | Typical Signal |
| --- | --- | --- |
| Stage 1 | 复苏期 | Demand improves, prices stop falling, inventory declines, margins repair, capex has not yet surged. |
| Stage 2 | 扩张期 | Orders are strong, revenue/profit accelerate, utilization rises, capex starts increasing. |
| Stage 3 | 过热期 | Prices and margins are high, optimism is broad, companies expand aggressively, future supply risk accumulates. |
| Stage 4 | 衰退期 | Demand growth slows, prices soften, inventory rises, margins fall, capex begins to be cut. |
| Stage 5 | 出清期 | Industry losses or weak profits force capex cuts, destocking, shutdowns, consolidation, layoffs, or restructuring. |

Always distinguish:

- industry cycle stage
- company competitive/operating stage
- stock valuation or market-pricing stage

These can diverge. Example: industry Stage 3, company Stage 2, stock already priced as Stage 3.

## Eight-Dimension Scoring

Score each dimension from `-2` to `+2` and explain the evidence.

| Dimension | +2 | +1 | 0 | -1 | -2 |
| --- | --- | --- | --- | --- | --- |
| Demand cycle | demand surge | demand improving | stable | slowing | sharply declining |
| Price / ASP cycle | sustained price rises | prices bottoming/rebounding | stable | beginning to fall | rapid decline |
| Margin cycle | margins expanding fast | margins repairing | stable | pressured | sharply worse or loss-making |
| Capex cycle | broad aggressive expansion | capex revised up / expansion starts | stable | slowing | sharply cut |
| Inventory cycle | extremely low / tight supply | declining | normal | rising | high / channel clogged |
| Capacity-release pressure | little new supply / bottleneck | manageable new supply | balanced | clear new supply in 6-18 months | concentrated supply release / overcapacity risk |
| Customer behavior | rush orders, LTAs, accepts price hikes | restocking | normal purchasing | delays or price pressure | order cuts or cancellations |
| Capital-market reaction | estimates rising and trend healthy | valuation repair | divided | price weakens ahead of fundamentals | derating and estimate cuts |

Do not mechanically map the total score to a stage. Use the four core variables first: margin, capex, inventory, and price.

## Stage Mapping Rules

Use these patterns as anchors:

| Stage | Pattern |
| --- | --- |
| Stage 1 复苏期 | demand +1, price +1, margin +1, capex 0/+1, inventory +1, market starts repairing |
| Stage 2 扩张期 | demand +2, price +1/+2, margin +2, capex +1, inventory +1/+2, estimates rising |
| Stage 3 过热期 | demand +2, price +2, margin +2, capex +2, inventory still low, future supply pressure rising, valuation/story very optimistic |
| Stage 4 衰退期 | demand -1/-2, price -1/-2, margin -1/-2, inventory -1/-2, capex rolls over, customers pressure prices or cut orders |
| Stage 5 出清期 | demand weak, prices low, margins poor or loss-making, inventory begins falling, capex -2, industry consolidation or shutdowns appear |

Output stage probabilities across all five stages. The probabilities must sum to roughly 100%, and the most likely stage should include a confidence level.

## Mermaid Visualizations

For a full report, include 2-4 Mermaid diagrams when they materially improve comprehension. A short answer or data-limited analysis may use fewer. Do not create a diagram merely to meet a quota.

Prioritize these views:

1. A five-stage `flowchart` showing the Juglar cycle and visibly highlighting the current industry stage; annotate a different company stage only when it diverges.
2. A `pie` chart of Stage 1-5 probabilities after confirming they match the probability list and sum to roughly 100%.
3. An `xychart-beta` of the eight dimension scores on the common -2 to +2 scale, with the scoring table retained.
4. A compact `flowchart` of the signals required to enter the next stage and the evidence that would invalidate the current classification.

Apply these rules to every diagram:

- Use fenced `mermaid` blocks, match the report language, keep node IDs in simple ASCII, and keep labels short.
- Prefer broadly supported `flowchart`, `pie`, and `stateDiagram` syntax. Use `xychart-beta`, `quadrantChart`, or `timeline` only as progressive enhancement and retain the adjacent Markdown table as the fallback.
- Use only evidence and values already stated in the report. Keep stage names, probabilities, score scales, and horizons consistent with the surrounding tables; never fill missing data for visual completeness.
- Place each diagram beside the analysis it explains and follow it with a one-sentence takeaway. Keep citations, URLs, dates, and detailed caveats outside the diagram.
- Keep a diagram focused: normally no more than 12 nodes or 8 plotted values. Diagrams supplement rather than replace scoring evidence, counter-evidence, migration conditions, and source trails.

## Output Format

Use Chinese by default unless the user requests another language.

```markdown
# TICKER：朱格拉周期阶段判断

【结论】
股票代码：
公司：
核心行业：
行业朱格拉周期阶段：
公司自身阶段：
股票定价阶段：
置信度：
未来 6-12 个月方向：

【阶段概率】
Stage 1 复苏期：xx%
Stage 2 扩张期：xx%
Stage 3 过热期：xx%
Stage 4 衰退期：xx%
Stage 5 出清期：xx%

紧接概率列表加入 Stage 1-5 Mermaid pie；数值必须与列表一致。

【周期位置图】
加入五阶段 Mermaid flowchart，并高亮当前行业阶段；公司阶段不同则单独标注。

【一句话判断】
...

【8维评分表】
| 维度 | 分数 | 证据 | 解释 |
| --- | ---: | --- | --- |
| 需求周期 | | | |
| 价格周期 / ASP | | | |
| 利润率周期 | | | |
| 资本开支周期 | | | |
| 库存周期 | | | |
| 产能释放压力 | | | |
| 客户行为 | | | |
| 资本市场反应 | | | |

如八维数据完整，在表后加入 -2 到 +2 同量纲的 Mermaid xychart，并保留评分表。

【核心证据】
1.
2.
3.

【反证与风险】
1.
2.
3.

【阶段迁移信号】
- 进入下一阶段需要看到：
- 当前判断被推翻需要看到：

加入 Mermaid flowchart 展示迁移条件与证伪分支。

【投资含义】
1. 短期交易含义：
2. 中期持仓含义：
3. 最大风险：

【最终评级】
周期位置：
风险收益比：
适合策略：左侧买入 / 右侧持有 / 趋势加仓 / 逢高减仓 / 等待出清 / 只观察
```

## Detailed Reference

Read `references/original-framework.md` when a task needs the full Chinese source framework, exact stage definitions, complete scoring tables, or the original prompt wording.

When the reference format differs, preserve its analytical intent but follow this SKILL.md's current output and visualization rules.
