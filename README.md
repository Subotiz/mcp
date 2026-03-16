# Subotiz MCP

Subotiz MCP 是基于 [Model Context Protocol](https://spec.modelcontextprotocol.io/) 的开放工具集服务器。AI 代理可通过标准化 MCP 工具与 Subotiz 支付与订阅能力交互（客户、商品、定价、订阅、交易、退款、发票、Webhook 及开发者文档查询等）。

**了解更多**：了解 Subotiz 产品与能力，请访问 [官网首页](https://www.subotiz.com/)。

## 前置条件

1. 支持 Streamable Http MCP 的宿主（如VS Code 1.101+、Claude Desktop、Cursor、Trae 等）
2. 有效的 Subotiz 访问凭证（Token）

## 对外 MCP 配置

连接官方托管服务时，只需配置 URL 与 `Authorization: Bearer`，**无需其他请求头**。

**Cursor** 配置示例（写入 Cursor 的 MCP 设置即可）：

```json
{
  "mcpServers": {
    "my-remote-server": {
      "url": "https://api.subotiz.com/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_TOKEN_HERE"
      }
    }
  }
}
```

将 `YOUR_TOKEN_HERE` 替换为你的 Subotiz 访问 Token 即可。在 VS Code、Cursor、Claude Desktop 等宿主中，把上述内容合并到各自的 MCP 配置（如 `servers` 或 `mcpServers`）中即可使用。

**API Key 获取**：配置中的 Token 即 Subotiz API Key。获取步骤与认证方式请参阅 [认证文档](https://developer.subotiz.com/v1.0-zh-cn/reference/authentication-1)。

---


## 工具列表

完整工具列表（工具名、类别、path、说明）在单独文档中维护，便于同步更新：[TOOLS.md](TOOLS.md)。更多接口与参数以 [Subotiz 开发者文档](https://developer.subotiz.com/v1.0-zh-cn/reference/over-view) 为准。

---


# Subotiz MCP

Subotiz MCP is an open toolset server based on the [Model Context Protocol](https://spec.modelcontextprotocol.io/). AI agents can interact with Subotiz payment and subscription capabilities through standardized MCP tools (customers, products, pricing, subscriptions, trades, refunds, invoices, webhooks, and developer documentation lookup).

**Learn more**: For an overview of Subotiz products and capabilities, visit the [Subotiz homepage](https://www.subotiz.com/).

## Prerequisites

1. A host that supports Streamable HTTP MCP (e.g. VS Code 1.101+, Claude Desktop, Cursor, Trae, etc.)
2. A valid Subotiz access token

## MCP Configuration

When connecting to the official hosted service, you only need to configure the URL and `Authorization: Bearer`; **no other request headers are required**.

**Cursor** example (add to Cursor’s MCP settings):

```json
{
  "mcpServers": {
    "my-remote-server": {
      "url": "https://api.subotiz.com/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_TOKEN_HERE"
      }
    }
  }
}
```

Replace `YOUR_TOKEN_HERE` with your Subotiz access token. In hosts such as VS Code, Cursor, or Claude Desktop, merge the above into their respective MCP configuration (e.g. `servers` or `mcpServers`) to use it.

**Obtaining an API Key**: The token in the configuration is your Subotiz API Key. For steps to create one and authentication details, see the [Authentication guide](https://developer.subotiz.com/v1.0-zh-cn/reference/authentication-1).

---

## Tool List

The full tool list (name, category, path, description) is maintained in a separate document for easier updates: [TOOLS.md](TOOLS.md). For more endpoints and parameters, see the [Subotiz Developer Documentation](https://developer.subotiz.com/v1.0-en-us/reference/overview-1).

