# Marketing APIs

整理赠品与限时抢购相关高频查询接口。

## 获取赠品列表

- 中文名：获取赠品列表
- 英文名：`getgiftproductlist`
- 方法：`POST`
- 路径：`/channels/ec/product/gift/list/get`
- 类型：只读查询
- 作用：按状态分页获取赠品列表。
- 官方文档：https://developers.weixin.qq.com/doc/store/shop/API/channels-shop-product/gift/api_getgiftproductlist

### 请求参数

#### Query

- `access_token`: string，必填

#### Body

- `status`: number，可选，赠品状态；不填默认拉全部赠品，不包含回收站
- `page_size`: number，必填，默认 10，不超过 30
- `next_key`: string，可选，分页上下文

### 返回重点

- 赠品 ID 列表
- 下一页参数
- 总量信息

### 最小请求示例

```json
{
  "page_size": 20
}
```

### 适用场景

- 拉取赠品清单
- 后续逐条获取赠品详情

### 不适用场景

- 查询单个赠品完整字段

## 获取赠品

- 中文名：获取赠品
- 英文名：`getgiftproduct`
- 方法：`POST`
- 路径：`/channels/ec/product/gift/get`
- 类型：只读查询
- 作用：根据赠品 ID 获取赠品详情。
- 官方文档：https://developers.weixin.qq.com/doc/store/shop/API/channels-shop-product/gift/api_getgiftproduct

### 请求参数

#### Query

- `access_token`: string，必填

#### Body

- `product_id`: string，必填，赠品 ID
- `data_type`: number，可选，默认 `1`
  - `1`: 获取线上数据
  - `2`: 获取草稿数据
  - `3`: 同时获取线上和草稿数据

### 返回重点

- 赠品基础信息
- 类目、图片、属性、SKU
- 草稿与线上数据

### 最小请求示例

```json
{
  "product_id": "1234567890",
  "data_type": 1
}
```

### 适用场景

- 查看赠品完整详情
- 核对赠品属性和 SKU

### 不适用场景

- 批量拉赠品清单

## 获取限时抢购任务列表

- 中文名：获取限时抢购任务列表
- 英文名：`getlimiteddiscounttasklist`
- 方法：`POST`
- 路径：`/channels/ec/product/limiteddiscounttask/list/get`
- 类型：只读查询
- 作用：分页获取限时抢购任务。
- 官方文档：https://developers.weixin.qq.com/doc/store/shop/API/channels-shop-product/limiteddiscounttask/api_getlimiteddiscounttasklist

### 请求参数

#### Query

- `access_token`: string，必填

#### Body

- `status`: number，可选，按任务状态筛选；不填则获取全部状态
- `page_size`: number，必填，默认 10，不超过 50
- `next_key`: string，可选，翻页上下文

### 返回重点

- 抢购任务列表
- 任务状态
- 分页信息

### 最小请求示例

```json
{
  "page_size": 20
}
```

### 适用场景

- 活动巡检
- 运营查看抢购排期
- 排查当前是否有有效抢购任务

### 不适用场景

- 创建、停止或删除抢购任务

### 注意事项

- 正式接入时建议补全 `status` 枚举，方便 agent 根据状态精确筛选。
