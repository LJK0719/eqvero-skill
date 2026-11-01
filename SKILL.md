---
name: eqvero
description: 研究港股、美股上市公司时使用：穿透减持套现、关联交易、IPO 承配、存贷双高、利润与现金流背离等异常操作；查询五年财务、公告原文与持股变动；运行组合风险、VaR、因子归因、DCF、期权定价等量化模型。需要已连接 Eqvero MCP 服务器。
---

# Eqvero 港美股深度研究

通过 `eqvero` MCP 服务器读取港美股上市公司的原始披露与结构化数据。数据来自公司公告与监管申报，结论要能回到原文。

## 先找到公司

`companies_search({ query })` 返回公司 `id`（issuerId）。后续工具都用这个 id，不要用股票代码代替。

## 穿透异常操作的思路

普通的财报摘要谁都会写。用 Eqvero 时，把不同来源放到同一条时间线上对账，回答「谁先拿到钱，谁在承担风险」：

| 怀疑 | 先查 | 再对账 |
|---|---|---|
| 大股东/高管套现 | `ownership_list` 持股变动 | `events_list` 同期公告、`prices_list` 减持前后股价 |
| 关联交易 / 关联并购 | `evidence_search` 搜「关联方」「收购」 | `evidence_section` 读交易对价、业绩承诺、付款安排；`financials_query` 看商誉 |
| 利润含金量 | `financials_query` 净利润、经营现金流、应收、存货 | 多期对比差额来自哪里 |
| 存贷双高 | `financials_query` 现金、有息债务、利息收入 | 利息收入 ÷ 平均现金 是否远低于市场利率 |
| IPO 认购质量 | `evidence_search` 搜配发结果 | 前十大承配人占比、募资用途 |

异常不等于违法。写清证据支持到哪一步、还缺哪一环。

## 读原文

`evidence_search({ issuerId, query })` 只返回片段；引用前用 `evidence_section({ id })` 读完整章节，`evidence_table({ id })` 读表格。

## 量化计算

1. `analytics_methods({})` 查看 15 种模型及默认参数。
2. `analytics_availability({ run })` 检查数据是否够用（不扣次数）。
3. `analytics_preview({ runs })` 确认消耗，`analytics_create({ runs, idempotencyKey })` 提交，`analytics_get({ id })` 取结果。

数值由服务端模型计算，不要自己编造统计值。百分比按小数传（0.05 = 5%）。

## 深度研究

`research_create({ issuerId, question, mode: "deep", idempotencyKey })` 发起后台深度研究，用 `research_get({ id })` 轮询。深度研究消耗套餐次数，用户明确要求时再用。
