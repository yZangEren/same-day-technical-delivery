# API 文档冲刺示例

> 这是一个虚构的订单 API 示例，用来展示交付格式，不代表客户项目或真实生产接口。

## 创建订单

`POST /v1/orders`

### 请求头

| 字段 | 必填 | 说明 |
| --- | --- | --- |
| `Authorization` | 是 | `Bearer <token>` |
| `Content-Type` | 是 | `application/json` |
| `Idempotency-Key` | 建议 | 防止网络重试导致重复订单 |

### 请求体

```json
{
  "items": [{"sku": "starter-001", "quantity": 1}],
  "currency": "USD"
}
```

### 成功响应 `201 Created`

```json
{
  "id": "ord_demo_123",
  "status": "pending_payment",
  "total": "15.00",
  "currency": "USD"
}
```

## 错误和边界情况

| HTTP 状态 | 场景 | 客户端处理 |
| --- | --- | --- |
| `400` | `items` 为空或数量不是正整数 | 展示字段级错误，不重试 |
| `401` | Token 缺失或过期 | 重新认证后再发起请求 |
| `409` | 相同 `Idempotency-Key` 已处理 | 读取原订单结果，不创建新订单 |
| `429` | 请求频率过高 | 按 `Retry-After` 等待并退避 |
| `5xx` | 服务端暂时不可用 | 有上限地重试，并记录 request ID |

## 验收清单

- [ ] 缺失 `Authorization` 时返回 `401`。
- [ ] 空 `items` 返回 `400`，且错误字段可定位。
- [ ] 相同 `Idempotency-Key` 重复请求不会生成第二个订单。
- [ ] `429` 响应包含可用的 `Retry-After` 信息。
- [ ] 成功响应包含订单 ID、状态、金额和币种。
- [ ] 文档中的示例可复制到测试环境运行。

## 发布交接

- 已覆盖：请求字段、成功响应、错误状态、重试和幂等行为。
- 待确认：认证服务的 Token 有效期、金额精度、订单状态转换和正式环境限流值。
- 建议：上线前保存一次成功请求和一次错误请求的 request ID，便于回归和排障。
