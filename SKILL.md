---
name: eqvero
description: 使用Eqvero查询港美股公司资料、分析关联交易与财务风险，或进行组合、因子、估值、债券期权等金融计算。需要已连接Eqvero MCP服务器。
---

# Eqvero 公司研究

使用已连接的 `eqvero` MCP 服务器完成用户的金融问题，不另建Agent循环。先确认当前服务器实际提供的工具和权限。

## 公司与资料

`companies_search({query})`返回公司ID，后续使用真实`issuerId`，不以股票代码代替UUID。按问题读取`financials_metrics`、`financials_query`、`events_list`、`ownership_list`和`prices_list`，保持主体、期间、币种和调整口径一致。

需要原文时，先`evidence_search({issuerId,query})`定位，再`evidence_section({id})`读章节、`evidence_table({id})`读表格。搜索片段不是全文，不编造没有读到的证据。

## 研究表达

先说明哪家公司发生了什么、异常可能怎样形成，再解释它对公司价值、股东权益、偿债或退出流动性的影响。标题和段落用完整叙述，不写“谁在接盘”等口号，不用孤立数字制造悬念。

区分实际交易、拟议安排、监管怀疑和已认定事实。关联并购不自动等于利益输送；单一比率只能提供核查方向。保留真正影响结论的缺项和改判条件，不把每段写成免责声明。

## 研究与计算任务

用户明确要求公司研究时，`research_create({issuerId,question,mode:"deep",idempotencyKey})`创建后台任务。说明使用的额度，保存返回的任务ID，用`research_get`读取状态与结果；追问使用`research_followup`。

金融计算先用`analytics_methods`确认模型和字段，用`analytics_quote`查看费用，再经用户确认调用`analytics_create`。实际输入由服务端准备和核验，通过`analytics_get`取得数值结果，不自行编造或修改统计量。比例使用小数（0.05表示5%）。

不确定参数集中说明一次，给有依据的建议；能从真实资料取得的主动读取。缺少决定性参数时明确说缺什么，不填零、不反复追问。Skill说明不代表工具已执行成功。

## 断线、取消与重试

- 页面或MCP连接断开，不取消已经接受的任务；继续读取原ID，不再次创建。
- 同一提交重发保留原`idempotencyKey`。响应丢失时用`research_submission({operation,idempotencyKey,taskId?})`查询；暂未查到不自动换key收费。
- 等待补充时，给出具体问题后续接原任务。只有用户明确要求停止才调用`research_cancel`或`analytics_cancel`。
- `status=cancelled`且`executionActive=true`表示执行仍在退出；不要报告已经物理停止。
- 失败恢复沿`research_retry`使用原taskId和独立稳定重试标识；不循环重试，也不重新运行已交付结果。

私人资料只在用户授权范围内使用。API Key不写入对话、URL、日志或共享文件。安装与连接的权威说明见 https://eqvero.com/developers/setup.md ，不要猜客户端配置。
