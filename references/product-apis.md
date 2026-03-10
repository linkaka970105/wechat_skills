# Product APIs

整理微信小店高频商品查询接口。

## 获取商品列表

- 中文名：获取商品列表
- 英文名：`getproductlist`
- 方法：`POST`
- 路径：`/channels/ec/product/list/get`
- 类型：只读查询
- 作用：按商品状态分页获取商品 ID 列表。
- 官方文档：https://developers.weixin.qq.com/doc/store/shop/API/channels-shop-product/shop/api_getproductlist

### 请求参数

#### Query

- `access_token`: string，必填

#### Body

- `status`: number，可选，商品状态；不填默认拉取全部商品，但不包含从未上架的草稿和回收站商品
- `page_size`: number，必填，默认 10，不超过 30
- `next_key`: string，可选，翻页上下文

### 商品状态枚举

- `0`: 初始值
- `5`: 上架
- `6`: 回收站
- `9`: 彻底删除
- `11`: 自主下架
- `13`: 违规下架或风控下架
- `14`: 保证金不足下架
- `15`: 品牌过期下架
- `20`: 商品被封禁

### 返回字段

- `errcode`: number，错误码
- `errmsg`: string，错误信息
- `product_ids`: array，商品 ID 列表
- `next_key`: string，下一页上下文
- `total_num`: number，商品总数

### 最小请求示例

```json
{
  "status": 5,
  "page_size": 20
}
```

### 适用场景

- 拉取在售商品列表
- 分页查看店铺商品池
- 先拿 `product_id`，再逐条查商品详情

### 不适用场景

- 查询单个商品完整字段
- 查询库存

### 注意事项

- 这个接口返回的是商品 ID 列表，不是完整商品详情。
- 获取详情时应继续调用“获取商品”。

## 获取商品

- 中文名：获取商品
- 英文名：`getproduct`
- 方法：`POST`
- 路径：`/channels/ec/product/get`
- 类型：只读查询
- 作用：根据商品 ID 获取单个商品详情。
- 官方文档：https://developers.weixin.qq.com/doc/store/shop/API/channels-shop-product/shop/api_getproduct

### 请求参数

#### Query

- `access_token`: string，必填

#### Body

- `product_id`: string，必填，商品 ID
- `data_type`: number，可选，默认 `1`
  - `1`: 获取线上数据
  - `2`: 获取草稿数据
  - `3`: 同时获取线上和草稿数据

### 返回重点

- 商品基础信息
- 标题、图片、类目、属性、SKU
- 上架态和草稿态相关信息
- 新类目树字段 `cats_v2`

### 最小请求示例

```json
{
  "product_id": "1234567001",
  "data_type": 1
}
```

### 适用场景

- 查看商品完整详情
- 比对线上数据和草稿数据
- 获取商品类目、图片、SKU 等完整字段

### 不适用场景

- 批量分页拉商品列表
- 查询库存流水

### 注意事项

- 已上架过的商品，`data_type=3` 更适合做线上与草稿对比。
- 建议优先关注 `cats_v2`，兼容新类目树。
