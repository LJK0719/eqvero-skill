# Eqvero Skill & MCP

让你的 AI 助手（Claude Code、Cursor、Claude Desktop 等）直接调用 Eqvero 的港美股数据、原文检索和量化模型。

## 1. 创建 API Key

在 https://eqvero.com/developers 创建（基础版及以上）。

## 2. 连接 MCP

**Claude Code**

```bash
claude mcp add --transport http eqvero https://eqvero.com/api/mcp \
  --header "Authorization: Bearer <你的 API Key>"
```

**Cursor / Claude Desktop / 其他客户端**

```json
{
  "mcpServers": {
    "eqvero": {
      "type": "http",
      "url": "https://eqvero.com/api/mcp",
      "headers": { "Authorization": "Bearer <你的 API Key>" }
    }
  }
}
```

## 3. 安装 Skill（可选）

Skill 教你的 AI 怎样把公告、持股、财务放在一起对账。

```bash
git clone https://github.com/LJK0719/eqvero-skill ~/.claude/skills/eqvero
```

## 可以这样问

- 用 Eqvero 排查 AAPL 近三年的回购、高管减持与股权激励，算一下真实股东回报。
- 用 Eqvero 找出腾讯最近年报里关于联营公司减值的原文和金额。
- 用 Eqvero 对英伟达做 FF5 因子归因，看它近五年的超额收益来自哪里。
- 用 Eqvero 判断美团现在是否值得买入，并说明什么情况下判断会改变。

## 工具

| 工具 | 作用 |
|---|---|
| `companies_search` / `companies_get` | 检索公司，读取档案 |
| `financials_query` / `financials_metrics` | 查询财务数据 |
| `evidence_search` / `evidence_section` / `evidence_table` | 年报、季报、公告全文检索与原文 |
| `events_list` / `ownership_list` / `prices_list` | 公告事件、持股变动、行情 |
| `analytics_*` | 15 种量化模型 |
| `research_create` / `research_get` | 深度研究与投资判断 |

额度与网页共用。内容仅供研究参考，不构成投资建议。
