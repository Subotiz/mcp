# Subotiz MCP 工具列表 / Tool List

本文档单独维护 MCP 工具列表，便于同步更新。更多接口与参数以 [Subotiz 开发者文档](https://developer.subotiz.com/v1.0-zh-cn/reference/over-view) 为准。

---

## 中文

| 工具名 | 类别 | path | 说明 |
|--------|------|------|------|
| `create_customer` | 客户 | POST /api/v1/front/customer/create | 创建客户 |
| `list_customer` | 客户 | GET /api/v1/front/customer | 列表查询客户 |
| `list_products` | 商品 | GET /api/v1/products | 列表查询商品 |
| `create_product` | 商品 | POST /api/v1/products | 创建商品 |
| `list_prices` | 定价 | GET /api/v1/prices | 列表查询定价 |
| `create_price` | 定价 | POST /api/v1/prices | 创建定价 |
| `list_subscription` | 订阅 | GET /api/v1/subscription | 列表查询订阅 |
| `list_trades` | 交易 | GET /api/v1/trades | 列表查询交易 |
| `list_refund` | 退款 | GET /api/v1/trades/{trade_id}/refunds | 按交易查询退款列表 |
| `list_invoice` | 发票 | GET /api/v1/invoices | 列表查询发票 |
| `list_webhook_event_v2` | Webhook | GET /api/v2/webhook/events | 列表查询 Webhook 事件（v2） |
| `get_llm_full_doc` | 开发者文档 | - | 获取完整 LLM 文档（llm-full.txt） |
| `get_llm_doc` | 开发者文档 | - | 获取精简 LLM 文档（llm.txt） |

---

## English

| Tool Name | Category | Path | Description |
|-----------|----------|------|-------------|
| `create_customer` | Customer | POST /api/v1/front/customer/create | Create a customer |
| `list_customer` | Customer | GET /api/v1/front/customer | List customers |
| `list_products` | Product | GET /api/v1/products | List products |
| `create_product` | Product | POST /api/v1/products | Create a product |
| `list_prices` | Pricing | GET /api/v1/prices | List prices |
| `create_price` | Pricing | POST /api/v1/prices | Create a price |
| `list_subscription` | Subscription | GET /api/v1/subscription | List subscriptions |
| `list_trades` | Trade | GET /api/v1/trades | List trades |
| `list_refund` | Refund | GET /api/v1/trades/{trade_id}/refunds | List refunds by trade |
| `list_invoice` | Invoice | GET /api/v1/invoices | List invoices |
| `list_webhook_event_v2` | Webhook | GET /api/v2/webhook/events | List webhook events (v2) |
| `get_llm_full_doc` | Developer docs | - | Get full LLM documentation (llm-full.txt) |
| `get_llm_doc` | Developer docs | - | Get condensed LLM documentation (llm.txt) |

For more endpoints and parameters, see the [Subotiz Developer Documentation](https://developer.subotiz.com/v1.0-en-us/reference/overview-1).
