# Order APIs

整理微信小店高频订单查询接口。默认使用服务端调用，需携带 `access_token` 或第三方代调用场景下的 `authorizer_access_token`。

## 获取订单列表

- 中文名：获取订单列表
- 英文名：`getorderlist`
- 方法：`POST`
- 路径：`/channels/ec/order/list/get`
- 类型：只读查询
- 作用：按创建时间、更新时间、订单状态、买家 `openid` 查询订单列表。
- 官方文档：https://developers.weixin.qq.com/doc/store/shop/API/channels-shop-order/api_getorderlist

### 请求参数

#### Query

- `access_token`: string，必填，接口调用凭证

#### Body

- `create_time_range`: object，必填，订单创建时间范围
- `create_time_range.start_time`: number，必填，秒级时间戳，和 `end_time` 间隔不可超过 7 天
- `create_time_range.end_time`: number，必填，秒级时间戳，和 `start_time` 间隔不可超过 7 天
- `update_time_range`: object，可选，订单更新时间范围
- `update_time_range.start_time`: number，可选，秒级时间戳
- `update_time_range.end_time`: number，可选，秒级时间戳
- `status`: number，可选，订单状态
- `openid`: string，可选，买家身份标识
- `page_size`: number，可选，每页数量，不超过 100
- `next_key`: string，分页参数；首页可不传，翻页时必须传上次返回值

### 订单状态枚举

- `10`: 待付款
- `12`: 礼物待收下
- `13`: 一起买待成团
- `20`: 待发货
- `21`: 部分发货
- `30`: 待收货
- `100`: 完成
- `250`: 订单取消

### 返回字段

- `errcode`: number，错误码
- `errmsg`: string，错误信息
- `order_id_list`: array，订单号列表
- `next_key`: string，下一页参数
- `has_more`: boolean，是否还有下一页

### 最小请求示例

```json
{
  "create_time_range": {
    "start_time": 1735689600,
    "end_time": 1735776000
  },
  "page_size": 20
}
```

### 适用场景

- 按时间窗口批量拉取订单
- 按订单状态查看待发货、已完成、已取消订单
- 为后续订单详情查询先拿到 `order_id`

### 不适用场景

- 想按商品标题、复杂条件模糊搜索订单
- 想查询单个订单的完整详情

### 注意事项

- 时间范围最多 7 天。
- 翻页时，除 `next_key` 外，其余查询条件必须与上一页一致。
- 这个接口更适合批量拉单，不适合灵活检索。

## 获取订单详情

- 中文名：获取订单详情
- 英文名：`getorder`
- 方法：`POST`
- 路径：`/channels/ec/order/get`
- 类型：只读查询
- 作用：根据 `order_id` 获取单个订单完整详情。
- 官方文档：https://developers.weixin.qq.com/doc/store/shop/API/channels-shop-order/api_getorder

### 请求参数

#### Query

- `access_token`: string，必填

#### Body

- `order_id`: string，必填，订单 ID

### 返回重点

- 订单基础信息
- 商品明细
- 收货与物流信息
- 订单状态与售后相关信息
- 虚拟号或联系方式相关字段可能随权限和场景变化

### 最小请求示例

```json
{
  "order_id": "37423523451235145"
}
```

### 适用场景

- 已知 `order_id`，查看单笔订单完整信息
- 获取订单商品明细、物流信息、状态信息

### 不适用场景

- 批量拉单
- 模糊条件搜索订单

### 注意事项

- 若需要完整收货敏感信息，通常还要结合敏感信息解密类接口，不建议默认直接做。

## 订单搜索

- 中文名：订单搜索
- 英文名：`searchorder`
- 方法：`POST`
- 路径：`/channels/ec/order/search`
- 类型：只读查询
- 作用：按更灵活的条件搜索订单，适合客服或运营按标题、状态、售后条件等查单。
- 官方文档：https://developers.weixin.qq.com/doc/store/shop/API/channels-shop-order/api_searchorder

### 请求参数

#### Query

- `access_token`: string，必填

#### Body

- `search_condition`: object，必填，至少设置一个字段
- `search_condition.title`: string，可选，商品标题相关搜索条件
- `on_aftersale_order_exist`: number，可选
  - 不传：搜索全部订单
  - `0`: 没有正在售后的订单
  - `1`: 正在售后且售后单数量大于等于 1
- `status`: number，可选，订单状态
- `page_size`: number，必填，每页数量，不超过 100
- `next_key`: string，分页参数；首页可不传，翻页时传上次返回值

### 返回字段

- 搜索结果列表或订单号集合
- `next_key`: 下一页上下文
- `has_more`: 是否还有更多结果

### 最小请求示例

```json
{
  "search_condition": {
    "title": "牛奶"
  },
  "page_size": 20
}
```

### 适用场景

- 按商品标题、售后状态、订单状态组合查单
- 客服按模糊条件快速定位订单

### 不适用场景

- 只按时间批量拉取订单
- 已知 `order_id` 的精确详情查询

### 注意事项

- `search_condition` 内必须至少有一个过滤字段。
- 比“获取订单列表”更适合模糊搜索和带售后条件筛选。
