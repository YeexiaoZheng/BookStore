# Test Case

## 查看我的所有商铺

#### URL
GET http://[address]/seller/mystores
(GET http://[address]/store/mystores)

#### Request
Headers:

key | 类型 | 描述 | 是否可为空
---|---|---|---
token | string | 登录产生的会话标识 | N

Body:

```json
空
```

#### Test Case

Request Header:

```json
    "token":"$token$",  //由服务器生成后保存在前端，假设此时token为yzm用户在某设备某时登录得到的token
```

## 创建商铺

#### URL

POST http://[address]/seller/create_store

#### Request
Headers:

key | 类型 | 描述 | 是否可为空
---|---|---|---
token | string | 登录产生的会话标识 | N

Body:

```json
{
  "user_id": "$seller id$",
  "store_id": "$store id$"
}
```

key | 类型 | 描述 | 是否可为空
---|---|---|---
user_id | string | 卖家用户ID | N
store_id | string | 商铺ID | N

tips:
(1)store_id不可重复，假设已存在store_id为'648全球连锁店'的店铺

#### Test Case

Request Header:

```json
    "token":"$token$",  //由服务器生成后保存在前端，假设此时token为yzm用户在某设备某时登录得到的token
```

1、user_id与token不对应

Request Body:

```json
{
    "user_id":"stm",
    "store_id":"648"
}
```

2、store_id重复

Request Body:

```json
{
    "user_id":"yzm",
    "store_id":"648全球连锁店"
}
```

3、成功创建店铺

Request Body:

```json
{
    "user_id":"yzm",
    "store_id":"648"
}
```
## 商家添加书籍信息

#### URL：
POST http://[address]/seller/add_book

#### Request
Headers:

key | 类型 | 描述 | 是否可为空
---|---|---|---
token | string | 登录产生的会话标识 | N

Body:

```json
{
  "user_id": "$seller user id$",
  "store_id": "$store id$",
  "book_info": {
    "tags": [
      "tags1",
      "tags2",
      "tags3",
      "..."
    ],
    "pictures": [
      "$Base 64 encoded bytes array1$",
      "$Base 64 encoded bytes array2$",
      "$Base 64 encoded bytes array3$",
      "..."
    ],
    "id": "$book id$",
    "title": "$book title$",
    "author": "$book author$",
    "publisher": "$book publisher$",
    "original_title": "$original title$",
    "translator": "translater",
    "pub_year": "$pub year$",
    "pages": 10,
    "price": 10,
    "binding": "平装",
    "isbn": "$isbn$",
    "author_intro": "$author introduction$",
    "book_intro": "$book introduction$",
    "content": "$chapter1 ...$"
  },
  "stock_level": 0
}

```

属性说明：

变量名 | 类型 | 描述 | 是否可为空
---|---|---|---
user_id | string | 卖家用户ID | N
store_id | string | 商铺ID | N
book_info | class | 书籍信息 | N
stock_level | int | 初始库存，大于等于0 | N

book_info类：

变量名 | 类型 | 描述 | 是否可为空
---|---|---|---
id | string | 书籍ID | N
title | string | 书籍题目 | N
author | string | 作者 | Y
publisher | string | 出版社 | Y
original_title | string | 原书题目 | Y
translator | string | 译者 | Y
pub_year | string | 出版年月 | Y
pages | int | 页数 | Y
price | string | 价格(以分为单位) | N
binding | string | 装帧，精状/平装 | Y
isbn | string | ISBN号 | Y
author_intro | string | 作者简介 | Y
book_intro | string | 书籍简介 | Y
content | string | 样章试读 | Y
tags | array | 标签 | Y
pictures | array | 照片 | Y

tags和pictures：

    tags 中每个数组元素都是string类型  
    picture 中每个数组元素都是string（base64表示的bytes array）类型

tips:
(1)book_info中字段较多，大部分后端设置可以为空，下面不提供其部分为空的测试用例
(2)假设yzm用户拥有store_id为store_id_1的店铺，不拥有store_id_2的店铺
(3)book_id不允许重复，store_id_1店铺中已存在book_id_1的书籍

#### Test Case

Request Header:

```json
    "token":"$token$",  //由服务器生成后保存在前端，假设此时token为yzm用户在某设备某时登录得到的token
```

1、user_id与token不对应

Request Body:

```json
{
  "user_id": "stm",
  "store_id": "store_id_1",
  "book_info": {
    "tags": [
      "tags1",
      "tags2",
      "tags3",
      "..."
    ],
    "pictures": [
      "$Base 64 encoded bytes array1$",
      "$Base 64 encoded bytes array2$",
      "$Base 64 encoded bytes array3$",
      "..."
    ],
    "id": "book_id_2",
    "title": "$book title$",
    "author": "$book author$",
    "publisher": "$book publisher$",
    "original_title": "$original title$",
    "translator": "translater",
    "pub_year": "$pub year$",
    "pages": 10,
    "price": 10,
    "binding": "平装",
    "isbn": "$isbn$",
    "author_intro": "$author introduction$",
    "book_intro": "$book introduction$",
    "content": "$chapter1 ...$"
  },
  "stock_level": 10
}

```

2、商铺不存在

Request Body:

```json
{
  "user_id": "yzm",
  "store_id": "store_id_2",
  "book_info": {
    "tags": [
      "tags1",
      "tags2",
      "tags3",
      "..."
    ],
    "pictures": [
      "$Base 64 encoded bytes array1$",
      "$Base 64 encoded bytes array2$",
      "$Base 64 encoded bytes array3$",
      "..."
    ],
    "id": "book_id_2",
    "title": "$book title$",
    "author": "$book author$",
    "publisher": "$book publisher$",
    "original_title": "$original title$",
    "translator": "translater",
    "pub_year": "$pub year$",
    "pages": 10,
    "price": 10,
    "binding": "平装",
    "isbn": "$isbn$",
    "author_intro": "$author introduction$",
    "book_intro": "$book introduction$",
    "content": "$chapter1 ...$"
  },
  "stock_level": 10
}

```

3、book_id已存在

Request Body:

```json
{
  "user_id": "yzm",
  "store_id": "store_id_1",
  "book_info": {
    "tags": [
      "tags1",
      "tags2",
      "tags3",
      "..."
    ],
    "pictures": [
      "$Base 64 encoded bytes array1$",
      "$Base 64 encoded bytes array2$",
      "$Base 64 encoded bytes array3$",
      "..."
    ],
    "id": "book_id_1",
    "title": "$book title$",
    "author": "$book author$",
    "publisher": "$book publisher$",
    "original_title": "$original title$",
    "translator": "translater",
    "pub_year": "$pub year$",
    "pages": 10,
    "price": 10,
    "binding": "平装",
    "isbn": "$isbn$",
    "author_intro": "$author introduction$",
    "book_intro": "$book introduction$",
    "content": "$chapter1 ...$"
  },
  "stock_level": 10
}

```

4、成功添加书籍信息

Request Body:

```json
{
  "user_id": "yzm",
  "store_id": "store_id_1",
  "book_info": {
    "tags": [
      "tags1",
      "tags2",
      "tags3",
      "..."
    ],
    "pictures": [
      "$Base 64 encoded bytes array1$",
      "$Base 64 encoded bytes array2$",
      "$Base 64 encoded bytes array3$",
      "..."
    ],
    "id": "book_id_2",
    "title": "$book title$",
    "author": "$book author$",
    "publisher": "$book publisher$",
    "original_title": "$original title$",
    "translator": "translater",
    "pub_year": "$pub year$",
    "pages": 10,
    "price": 10,
    "binding": "平装",
    "isbn": "$isbn$",
    "author_intro": "$author introduction$",
    "book_intro": "$book introduction$",
    "content": "$chapter1 ...$"
  },
  "stock_level": 10
}

```

## 商家添加书籍库存


#### URL

POST http://[address]/seller/add_stock_level

#### Request
Headers:

key | 类型 | 描述 | 是否可为空
---|---|---|---
token | string | 登录产生的会话标识 | N

Body:

```json
{
  "user_id": "$seller id$",
  "store_id": "$store id$",
  "book_id": "$book id$",
  "add_stock_level": 10
}
```
key | 类型 | 描述 | 是否可为空
---|---|---|---
user_id | string | 卖家用户ID | N
store_id | string | 商铺ID | N
book_id | string | 书籍ID | N
add_stock_level | int | 增加的库存量 | N

tips:
(1)假设yzm用户拥有store_id为store_id_1的店铺，不拥有store_id_2的店铺
(2)假设store_id_1店铺中已存在id为book_id_1的书籍,不存在id为book_id_2的书籍

#### Test Case

Request Header:

```json
    "token":"$token$",  //由服务器生成后保存在前端，假设此时token为yzm用户在某设备某时登录得到的token
```

1、user_id与token不对应

Request Body:

```json
{
  "user_id": "stm",
  "store_id": "store_id_1",
  "book_id": "book_id_1",
  "add_stock_level": 10
}
```

2、商铺不存在

Request Body:

```json
{
  "user_id": "yzm",
  "store_id": "store_id_2",
  "book_id": "book_id_1",
  "add_stock_level": 10
}
```

3、书本不存在

Request Body:

```json
{
  "user_id": "yzm",
  "store_id": "store_id_1",
  "book_id": "book_id_2",
  "add_stock_level": 10
}
```

4、成功添加库存

Request Body:

```json
{
  "user_id": "yzm",
  "store_id": "store_id_1",
  "book_id": "book_id_1",
  "add_stock_level": 10
}
```

## 查看书店信息(书店info及所包含书的部分info)

#### URL
GET http://[address]/seller/store_info/?store_id=xxx
(GET http://[address]/store/store_info/?store_id=xxx)

#### Request
Headers:

key | 类型 | 描述 | 是否可为空
---|---|---|---
token | string | 登录产生的会话标识 | N

Body:

```json
{
  "store_id": "$store_id$",
  "page": "$page$"
}
```

key | 类型 | 描述 | 是否可为空
---|---|---|---
store_id | string | 商铺ID | N
page | int | 当前页数 | Y(空时默认为1)

tips:
(1)查看书店信息的时候对当前书店上架的图书进行分页显示，前端首次进行页面时默认请求第一页并接收到后端返回的总页数，以此生成分页按钮，每个分页按钮会将请求中page置为相应页数
(2)假设yzm用户有store_id为store_id_1的店铺，没有store_id为store_id_2的店铺
(3)假设store_id_1店铺中的上架图书分页结果有page_num页

#### Test Case

Request Header:

```json
    "token":"$token$",  //由服务器生成后保存在前端，假设此时token为yzm用户在某设备某时登录得到的token
```

1、商铺不存在

Request Body:

```json
{
  "store_id": "store_id_2",
  "page": 1
}
```

2、请求页数超过范围

Request Body:

```json
{
  "store_id": "store_id_2",
  "page": page_num + 1
}
```

3、请求页数不合规范

Request Body:

```json
{
  "store_id": "store_id_2",
  "page": -1
}
```

## 查看书店内某本书籍信息(卖家买家通用)

#### URL
GET http://[address]/seller/book_info/?book_id=xxx
(GET http://[address]/store/book_info/?book_id=xxx)

#### Request
Headers:

key | 类型 | 描述 | 是否可为空
---|---|---|---
token | string | 登录产生的会话标识 | N

Body:

```json
{
  "store_id": "$store_id$",
  "book_id": "$book_id$",
}
```

key | 类型 | 描述 | 是否可为空
---|---|---|---
store_id | string | 商铺ID | N
book_id | string | 书籍ID | N

tips:
(1)假设有store_id为store_id_1的店铺，不存在store_id为store_id_2的店铺
(2)假设store_id_1店铺中有book_id为book_id_1的书，没有book_id为book_id_2的书

#### Test Case

Request Header:

```json
    "token":"$token$",  //由服务器生成后保存在前端，假设此时token为yzm用户在某设备某时登录得到的token
```
1、store_id不存在

Request Body:

```json
{
  "store_id": "store_id_2",
  "book_id": "book_id_1",
}
```

2、book_id不存在

Request Body:

```json
{
  "store_id": "store_id_1",
  "book_id": "book_id_2",
}
```

3、成功查看书籍信息

Request Body:

```json
{
  "store_id": "store_id_1",
  "book_id": "book_id_1",
}
```

## 修改店铺信息

#### URL
POST http://[address]/seller/modify/
(POST http://[address]/store/modify/)

#### Request
Headers:

key | 类型 | 描述 | 是否可为空
---|---|---|---
token | string | 登录产生的会话标识 | N

Body:

```json
{
  "store_id": "$store_id$",
  "address": "$store_address$",
  "info": "$book_info$"
}
```

key | 类型 | 描述 | 是否可为空
---|---|---|---
store_id | string | 店铺ID | N
address | string | 店铺地址 | Y
info | string | 店铺简介 | Y

tips:
(1)假设yzm用户有store_id为store_id_1的店铺，没有store_id为store_id_2的店铺

#### Test Case

Request Header:

```json
    "token":"$token$",  //由服务器生成后保存在前端，假设此时token为yzm用户在某设备某时登录得到的token
```

1、店铺不存在

Request Body:

```json
{
  "store_id": "store_id_2",
  "address": "华东师范大学",
  "info": "简介"
}
```

2、地址为空
Request Body:

```json
{
  "store_id": "store_id_1",
  "address": null,
  "info": "简介"
}
```

3、简介为空

Request Body:

```json
{
  "store_id": "store_id_1",
  "address": "华东师范大学",
  "info": null
}
```

4、修改完整信息

Request Body:

```json
{
  "store_id": "store_id_1",
  "address": "华东师范大学",
  "info": "简介"
}
```

## 查看店铺所有订单

#### URL
GET http://[address]/seller/store_orders/
(GET http://[address]/store/store_orders/)

#### Request
Headers:

key | 类型 | 描述 | 是否可为空
---|---|---|---
token | string | 登录产生的会话标识 | N

Body:

```json
{
  "store_id": "$store_id$"
}
```

key | 类型 | 描述 | 是否可为空
---|---|---|---
store_id | string | 店铺ID | N

tips:
(1)假设yzm用户有store_id为store_id_1的店铺，没有store_id为store_id_2的店铺

tips:
(1)假设yzm用户有store_id为store_id_1的店铺，没有store_id为store_id_2的店铺

#### Test Case

Request Header:

```json
    "token":"$token$",  //由服务器生成后保存在前端，假设此时token为yzm用户在某设备某时登录得到的token
```

1、店铺不存在

Request Body:

```json
{
  "store_id": "store_id_2",
}
```

2、成功查看所有订单

Request Body:

```json
{
  "store_id": "store_id_1",
}
```

## 卖家订单发货

#### URL
POST http://[address]/seller/deliver/
(POST http://[address]/store/deliver/)

#### Request
Headers:

key | 类型 | 描述 | 是否可为空
---|---|---|---
token | string | 登录产生的会话标识 | N

Body:

```json
{
  "order_id": "$order_id$",
}
```

key | 类型 | 描述 | 是否可为空
---|---|---|---
order_id | string | 订单ID | N

tips:
(1)假设yzm用户所属店铺中有order_id为order_id_1的订单状态为未发货，有order_id为order_id_2的订单状态为已发货，order_id的order_id_3的订单状态为已收货，没有order_id_4的订单
(2)店铺订单中只能查看到已付款的订单，所以订单状态可认为只有未发货、已发货、已收货

#### Test Case

Request Header:

```json
    "token":"$token$",  //由服务器生成后保存在前端，假设此时token为yzm用户在某设备某时登录得到的token
```

1、发货已发货的订单

Request Body:

```json
{
  "order_id": "order_id_2",
}
```

2、发货已收货的订单

Request Body:

```json
{
  "order_id": "order_id_3",
}
```

3、发货不存在的订单

Request Body:

```json
{
  "order_id": "order_id_4",
}
```

4、成功发货订单

Body:

```json
{
  "order_id": "order_id_1",
}
```

## 卖家删除店铺

#### URL
POST http://[address]/seller/delete_store/
(POST http://[address]/store/delete_store)

#### Request
Headers:

key | 类型 | 描述 | 是否可为空
---|---|---|---
token | string | 登录产生的会话标识 | N

Body:

```json
{
  "store_id": "$store_id$",
}
```

key | 类型 | 描述 | 是否可为空
---|---|---|---
store_id | string | 店铺ID | N

tips:
(1)假设yzm用户有store_id为store_id_1的店铺，没有store_id为store_id_2的店铺

#### Test Case

Request Header:

```json
    "token":"$token$",  //由服务器生成后保存在前端，假设此时token为yzm用户在某设备某时登录得到的token
```

1、删除不存在的店铺

Request Body:

```json
{
  "store_id": "store_id_2",
}
```

2、成功删除店铺

Request Body:

```json
{
  "store_id": "store_id_1",
}
```

## 卖家上架下架书本

#### URL
POST http://[address]/seller/off_book/
(POST http://[address]/store/off_book/)

#### Request
Headers:

key | 类型 | 描述 | 是否可为空
---|---|---|---
token | string | 登录产生的会话标识 | N

Body:

```json
{
  "store_id": "$store_id$",
  "book_id": "book_id"
}
```

key | 类型 | 描述 | 是否可为空
---|---|---|---
store_id | string | 店铺ID | N
book_id | string | 书本ID | N

tips:
(1)卖家上架、下架书本使用同一接口，后端将数据库中对应书本的off属性反转
(2)假设yzm用户有store_id为store_id_1的店铺，没有store_id为store_id_2的店铺
(3)假设store_id_1店铺中有book_id为book_id_1,book_id_2的书，没有book_id为book_id_3的书
(4)假设book_id_1书的状态为已上架，book_id_2为已下架

#### Test Case

Request Header:

```json
    "token":"$token$",  //由服务器生成后保存在前端，假设此时token为yzm用户在某设备某时登录得到的token
```

1、店铺不存在

Request Body:

```json
{
  "store_id": "store_id_2",
  "book_id": "book_id_1"
}
```

2、书本不存在

Request Body:

```json
{
  "store_id": "store_id_1",
  "book_id": "book_id_3"
}
```

3、下架已上架书本

Request Body:

```json
{
  "store_id": "store_id_1",
  "book_id": "book_id_1"
}
```

4、上架已下架书本

Request Body:

```json
{
  "store_id": "store_id_1",
  "book_id": "book_id_2"
}
```

## 卖家修改书本价格

#### URL
POST http://[address]/seller/modify_price/
(POST http://[address]/store/modify_price)

#### Request
Headers:

key | 类型 | 描述 | 是否可为空
---|---|---|---
token | string | 登录产生的会话标识 | N

Body:

```json
{
  "store_id": "$store_id$",
  "book_id": "book_id",
  "price": "$price$"
}
```

key | 类型 | 描述 | 是否可为空
---|---|---|---
store_id | string | 店铺ID | N
book_id | string | 书本ID | N
price | string | 修改价格 | N

tips:
(1)假设yzm用户有store_id为store_id_1的店铺，没有store_id为store_id_2的店铺
(2)假设store_id_1店铺有book_id为book_id_1的书，没有book_id为book_id_2的书

#### Test Case

Request Header:

```json
    "token":"$token$",  //由服务器生成后保存在前端，假设此时token为yzm用户在某设备某时登录得到的token
```

1、店铺不存在

Request Body:

```json
{
  "store_id": "store_id_2",
  "book_id": "book_id_1",
  "price": "10.00"
}
```

2、书本不存在

Request Body:

```json
{
  "store_id": "store_id_1",
  "book_id": "book_id_2",
  "price": "10.00"
}
```

3、修改价格不合规范

Request Body:

```json
{
  "store_id": "store_id_1",
  "book_id": "book_id_1",
  "price": "-10.00"
}
```

4、成功修改价格

Request Body:

```json
{
  "store_id": "store_id_1",
  "book_id": "book_id_1",
  "price": "10.00"
}
```