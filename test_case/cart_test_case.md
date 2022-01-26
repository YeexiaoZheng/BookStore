# Test Case

#### URL：
POST http://[address]/cart/add/

#### Request

##### Header:

key | 类型 | 描述 | 是否可为空
---|---|---|---
token | string | 登录产生的会话标识 | N

##### Body:
```json
{
  "user_id": "buyer_id",
  "store_id": "store_id",
  "book_id": "1000067",
  "count": 1,  
}
```

##### 属性说明：

变量名 | 类型 | 描述 | 是否可为空
---|---|---|---
user_id | string | 买家用户ID | N
store_id | string | 商铺ID | N
bookid | string | 书籍的ID | N
count | string | 购买数量 | N

tips:
(1)假设有store_id为store_id_1的店铺，不存在store_id为store_id_2的店铺
(2)假设store_id_1店中有book_id为book_id_1的书，库存为stock_1，没有book_id为book_id_2的书
(3)假设yzm用户已注册,stm用户未注册

#### Test Case

Request Header:

```json
    "token":"$token$",  //由服务器生成后保存在前端，假设此时token为yzm用户在某设备某时登录得到的token
```

1、用户不存在

Request Body:

```json
{
  "user_id": "stm",
  "store_id": "store_id_1",
  "book_id": "book_id_1",
  "count": 1,  
}
```

2、店铺不存在

Request Body:

```json
{
  "user_id": "yzm",
  "store_id": "store_id_2",
  "book_id": "book_id_1",
  "count": 1,  
}
```

3、书本不存在

Request Body:

```json
{
  "user_id": "yzm",
  "store_id": "store_id_1",
  "book_id": "book_id_2",
  "count": 1,  
}
```

4、书本库存不足

Request Body:

```json
{
  "user_id": "yzm",
  "store_id": "store_id_1",
  "book_id": "book_id_1",
  "count": stock_1 + 1,  
}
```

5、成功加入购物车

Request Body:

```json
{
  "user_id": "yzm",
  "store_id": "store_id_1",
  "book_id": "book_id_1",
  "count": 1,  
}
```

## 查看购物车

#### URL：
GET http://[address]/cart/mycart/

#### Request

##### Header:

key | 类型 | 描述 | 是否可为空
---|---|---|---
token | string | 登录产生的会话标识 | N

##### Body:
```json
{}
```

#### Test Case

Request Header:

```json
    "token":"$token$",  //由服务器生成后保存在前端，假设此时token为yzm用户在某设备某时登录得到的token
```

## 删除购物车记录

#### URL：
POST http://[address]/cart/delete/

#### Request

##### Header:

key | 类型 | 描述 | 是否可为空
---|---|---|---
token | string | 登录产生的会话标识 | N

##### Body:
```json
{
  "user_id": "$buyer_id$",
  "stores": [
      {
          "store_id": "$store_id$",
          "books": ["bookID","bookID",...]
      },
  ],
}
```

##### 属性说明：

变量名 | 类型 | 描述 | 是否可为空
---|---|---|---
user_id | string | 买家用户ID | N
store_num | int | 涉及到的商铺数量 | N
stores | class | 商铺信息 | N

stores数组：

变量名 | 类型 | 描述 | 是否可为空
---|---|---|---
store_id | string | 商铺id | N
book_num | int | 商铺中书的bookID数量 | N
books | class | 购物车里商品信息 | N

books数组：bookID则是书籍的ID

tips:
(1)前端页面中全部删除和选择删除都是调用同一接口
(2)删除购物车分两种情况，一种是直接在页面中点击删除，一种是对购物车中的记录进行下单后删除购物车记录。由于买家下单部分接口中限定了只有一个store_id，所以，上述两种情况中后者只能选中一个店铺所以出于设计简单起见，在前端设置为只能对某一家店铺里的购物车记录进行操作，即前端发来的请求中stores只有一个元素，但后端支持多个元素。
(3)假设yzm用户在store_id为store_id_1的店铺中加入了book_id_1,book_id_2的购物车记录，在store_id为store_id_2的店铺中没有购物车记录

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
  "stores": [
      {
          "store_id": "store_id_1",
          "books": ["book_id_1","book_id_2"]
      },
  ],
}
```

2、商铺无购物车记录

Request Body:

```json
{
  "user_id": "yzm",
  "stores": [
      {
          "store_id": "store_id_2",
          "books": ["book_id_3","book_id_4"]
      },
  ],
}
```

3、书本不在购物车记录中

Request Body:

```json
{
  "user_id": "yzm",
  "stores": [
      {
          "store_id": "store_id_1",
          "books": ["book_id_3","book_id_4"]
      },
  ],
}
```

4、删除购物车成功

Request Body:

```json
{
  "user_id": "yzm",
  "stores": [
      {
          "store_id": "store_id_1",
          "books": ["book_id_1","book_id_2"]
      },
  ],
}
```