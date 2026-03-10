# Stock APIs

整理微信小店库存相关高频查询接口。

## 获取库存

- 中文名：获取库存
- 英文名：`getstock`
- 方法：`POST`
- 路径：`/channels/ec/product/stock/get`
- 类型：只读查询
- 作用：查询某个商品某个 SKU 的当前库存。
- 官方文档：https://developers.weixin.qq.com/doc/store/shop/API/channels-shop-product/stock/api_getstock

### 请求参数

#### Query

- `access_token`: string，必填

#### Body

- `product_id`: string，必填，内部商品 ID
- `sku_id`: string，必填，内部 SKU ID

### 返回重点

- 当前 SKU 库存数量
- 商品与 SKU 的库存关联结果

### 最小请求示例

```json
{
  "product_id": "10000000088178",
  "sku_id": "462966239"
}
```

### 适用场景

- 查询单个 SKU 当前库存
- 商品详情页补充库存信息

### 不适用场景

- 一次查询多个商品库存
- 追踪库存变动过程

## 批量获取库存信息

- 中文名：批量获取库存信息
- 英文名：`batchgetstock`
- 方法：`POST`
- 路径：`/channels/ec/product/stock/batchget`
- 类型：只读查询
- 作用：根据商品 ID 批量获取每个商品下所有 SKU 的库存。
- 官方文档：https://developers.weixin.qq.com/doc/store/shop/API/channels-shop-product/stock/api_batchgetstock

### 请求参数

#### Query

- `access_token`: string，必填

#### Body

- `product_id`: array，必填，商品 ID 列表，上限 50

### 返回重点

- 每个商品下所有 SKU 的库存信息
- 适合批量盘点和后台看板拉数

### 最小请求示例

```json
{
  "product_id": [
    "10000017524246",
    "10000017524247"
  ]
}
```

### 适用场景

- 多商品批量查库存
- 运营盘点
- 做库存看板

### 不适用场景

- 查询单个 SKU 的精确库存流水

## 获取库存流水

- 中文名：获取库存流水
- 英文名：`getstockflow`
- 方法：`POST`
- 路径：`/channels/ec/product/stock/getflow`
- 类型：只读查询
- 作用：查询某商品某 SKU 在指定时间范围内的库存流水。
- 官方文档：https://developers.weixin.qq.com/doc/store/shop/API/channels-shop-product/stock/api_getstockflow

### 请求参数

#### Query

- `access_token`: string，必填

#### Body

- `product_id`: number，必填，内部商品 ID
- `sku_id`: number，必填，内部 SKU ID
- `stock_type`: number，必填，库存类型
- `finder_id`: string，可选，当 `stock_type=1` 时必填
- `begin_time`: number，必填，开始时间，秒级时间戳
- `end_time`: number，必填，结束时间，秒级时间戳
- `op_type_list`: array，可选，库存事件类型列表
- `page_size`: number，必填，每页数量
- `next_key`: string，可选，翻页上下文

### 返回重点

- 库存变动记录
- 变动时间
- 变动类型
- 变动前后数量
- 分页上下文

### 最小请求示例

```json
{
  "product_id": 10000017524246,
  "sku_id": 462966239,
  "stock_type": 0,
  "begin_time": 1735689600,
  "end_time": 1735776000,
  "page_size": 50
}
```

### 适用场景

- 库存异常排查
- 库存变更追踪
- 达人分销或特定库存类型分析

### 不适用场景

- 只想看当前库存数量
- 一次批量拉多个商品静态库存

### 注意事项

- `stock_type` 和 `op_type_list` 的枚举应继续按官方文档补充。
- 查询时间范围过大时，应按时间窗口分段拉取。
