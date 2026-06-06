---
name: gf-dma-health-index
description: Score a stock's current valuation/trend health using the GF-DMA Health Index, combining fundamental growth speed, 20/50/100/200DMA trend speed, price-to-DMA divergence, ATR divergence, escape ratio, and estimate revisions. Use when the user provides a ticker or asks for GF-DMA scoring, valuation health, trend health, healthy momentum, overheated/escape risk, or whether a rising/falling stock is fundamentally supported.
---

# GF-DMA Health Index

## Core Idea

Evaluate whether a stock's current price trend is supported by fundamental speed and moving-average structure.

Use the index to answer:

```text
Is the current price trend supported by revenue growth, profit growth, estimate revisions, and the 20/50/100/200DMA system?
```

Treat results as research analysis, not investment advice. For latest/current scoring, verify data from current sources before calculating.

## Required Inputs

Collect the newest available data before scoring:

- Price/technical data: latest price, 20DMA, 50DMA, 100DMA, 200DMA, ATR20, 5-day price change, and 20/50/100/200-day price changes or historical prices.
- Fundamental data: latest quarterly revenue, EPS, gross margin or gross profit, next-quarter company guidance, consensus revenue/EPS estimates, and 30-day estimate revisions.
- Preferred sources: company IR releases/presentations, earnings calls, Yahoo Finance historical prices/analysis, TradingView technicals/estimates, Barchart technical analysis, Seeking Alpha estimates, Koyfin, FactSet, Bloomberg, TIKR, or Visible Alpha.

If a required field is unavailable, say which field is missing and use the simplified formula only when appropriate.

## Calculation Workflow

### 1. Fundamental Speed

Calculate:

```text
G_f = 0.35G_Revenue + 0.25G_GrossProfit + 0.30G_EPS + 0.10G_Revision
```

Where:

- `G_Revenue = next-quarter revenue guidance / latest-quarter revenue - 1`
- `G_GrossProfit = next-quarter gross profit / latest-quarter gross profit - 1`
- `G_EPS = next-quarter EPS guidance / latest-quarter EPS - 1`
- `G_Revision = 30-day consensus estimate revision`

Fallbacks:

- If gross profit or EPS is missing: `G_f = 0.5G_Revenue + 0.5G_EPS`
- If only revenue guidance is available: `G_f = G_Revenue`

### 2. DMA Speed

Calculate quarterly annualized-equivalent moving-average speed for each DMA:

```text
G_DMAx = ((SMA_x(t) - SMA_x(t-k)) / SMA_x(t-k)) * (63 / k)
```

Use `k = 5` or `10` trading days by default. If only price-change data is available, approximate:

```text
DailySlope_x ~= (P_t - P_t-x) / x
G_DMAx ~= DailySlope_x * 63 / P_t
```

Compute `G_DMA20`, `G_DMA50`, `G_DMA100`, and `G_DMA200`.

### 3. Fundamental-DMA Match

Calculate:

```text
R_x = G_DMAx / G_f
```

Interpret `R_50` and `R_100` first:

| R_x | Status |
| ---: | --- |
| < 0.5 | Trend clearly below fundamental speed |
| 0.5-0.8 | Under-reflected or cheap versus trend |
| 0.8-1.3 | Healthy match |
| 1.3-2.0 | Hot but potentially explainable |
| > 2.0 | Overheated / FOMO escape risk |

Core DMA emphasis:

| Stock type | Key DMA |
| --- | --- |
| Mega-cap growth leaders like NVDA, AVGO, MSFT | 50DMA |
| Memory/cyclical semis like MU, SNDK | 100DMA |
| High-elasticity optical names like LITE, AAOI | 20DMA + 50DMA |
| Industrial AI/power names like ETN, VRT, TEL | 100DMA + 200DMA |
| Small-cap hard-manufacturing names like SIVE, CPSH | 20DMA + ATR divergence |
| Semiconductor ETFs like SOXX, SMH | 50DMA + 100DMA |

### 4. Price-DMA Divergence

Calculate:

```text
D_x = P_t / SMA_x(t) - 1
Z_x = (P_t - SMA_x) / ATR20
```

Interpretation:

| Signal | Status |
| --- | --- |
| 0%-5% above 20DMA | Healthy close-to-line trend |
| 5%-12% above 20DMA | Strong trend |
| 12%-20% above 20DMA | Hot |
| >20% above 20DMA | Short-term escape |
| >30% above 50DMA | Medium-term overheat |
| >50% above 100DMA | Major repricing |
| >100% above 200DMA | Extreme long-cycle repricing |

ATR divergence: `Z_x` of 0-2 is healthy, 2-3 hot, 3-4 very hot, >4 escape, and <0 means price is below that moving average.

### 5. Trend Parallelism / Escape Ratio

Calculate:

```text
EscapeRatio = 5-day price slope / 50DMA daily slope
```

Interpretation:

| EscapeRatio | Status |
| ---: | --- |
| 0.8-1.2 | Price and 50DMA are parallel; healthy |
| 1.2-1.8 | Short-term acceleration; acceptable |
| 1.8-2.5 | Clearly hot |
| >2.5 | FOMO escape |
| 0-0.5 | Momentum decay |
| <0 | Short-term reversal; trend damage |

### 6. Revision Confirmation

Score estimate revisions:

| Revision state | Score |
| --- | ---: |
| Revenue and EPS estimates rising; company guide above consensus | 85-100 |
| Mild upward revisions; guide slightly above consensus | 70-85 |
| Stable expectations; limited upward revision | 55-70 |
| Revisions starting to fall | 35-55 |
| Guide below consensus; analysts cutting estimates | <35 |

## Final Scoring

Calculate total score out of 100:

```text
HealthScore = 40S_GrowthMatch + 25S_Divergence + 20S_Parallel + 15S_Revision
```

Module scoring:

| Module | Weight |
| --- | ---: |
| Fundamental speed match | 40% |
| Price-DMA divergence | 25% |
| Trend parallelism | 20% |
| Revision confirmation | 15% |

Final interpretation:

| Score | State | Meaning |
| ---: | --- | --- |
| 85-100 | Healthy Momentum | Healthy main uptrend |
| 75-85 | Strong but Watch | Strong trend; continue monitoring |
| 65-75 | Hot but Supported | Hot, but fundamentals can still support it |
| 55-65 | Damaged / Overheated | Trend damage or local overheat |
| 40-55 | High Risk | Risk clearly rising |
| <40 | Broken / Escaping | Broken trend or post-escape pullback |

## Output Format

Use this structure for every ticker:

```text
# TICKER: GF-DMA Health Index 评分

最终评分：XX / 100
状态：Healthy Momentum / Strong but Watch / Hot but Supported / Damaged / High Risk / Broken

一句话判断：
...

1. 基本面速度
- 最新季度营收：
- 下一季度营收指引：
- 营收 QoQ：
- EPS QoQ：
- 毛利润 QoQ：
- Fundamental Speed：

2. 均线速度匹配
| 均线 | 季度化斜率 | 相对基本面速度 | 判断 |
|---|---:|---:|---|
| 20DMA | | | |
| 50DMA | | | |
| 100DMA | | | |
| 200DMA | | | |

3. 股价-均线背离
| 指标 | 当前背离 | 判断 |
|---|---:|---|
| P / 20DMA - 1 | | |
| P / 50DMA - 1 | | |
| P / 100DMA - 1 | | |
| P / 200DMA - 1 | | |

4. 趋势平行度
- Escape Ratio：
- 判断：

5. 预期上修确认
- 公司指引 vs 市场预期：
- 过去 30 天预期变化：
- 判断：

6. 综合评分
| 模块 | 权重 | 分数 |
|---|---:|---:|
| 基本面速度匹配 | 40% | |
| 股价-均线背离 | 25% | |
| 趋势平行度 | 20% | |
| 预期上修确认 | 15% | |

结论：
...
```

## Detailed Reference

Read `references/original-framework.md` when a task needs the full Chinese framework text, examples, source priority list, or scoring tables in their original form.
