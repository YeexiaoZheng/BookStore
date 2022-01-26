# Test Case

## 查看订单详情(买家卖家通用)
#### URL：
GET http://[address]/buyer/order_info/?order_id=xxx
(GET http://[address]/order/order_info/?order_id=xxx)

#### Request

##### Header:

key | 类型 | 描述 | 是否可为空
---|---|---|---
token | string | 登录产生的会话标识 | N

##### Body:
```json
{
  "order_id": "$order_id",
}
```

tips:
(1)假设yzm用户已有order_id为id_1的订单，不存在order_id为id_2的订单

#### Test Case

Request Header:

```json
    "token":"$token$",  //由服务器生成后保存在前端，假设此时token为yzm用户在某设备某时登录得到的token
```

1、订单id不存在

Request Body:

```json
{
    "order_id":"$id_2$"
}
```

2、成功查看订单详细

Request Body:

```json
{
    "order_id":"$id_1$"
}
```


## 买家下单

#### URL：
POST http://[address]/buyer/new_order

#### Request

##### Header:

key | 类型 | 描述 | 是否可为空
---|---|---|---
token | string | 登录产生的会话标识 | N

##### Body:
```json
{
  "user_id": "$buyer_id$",
  "store_id": "$store_id$",
  "books": [
    {
      "id": "$1000067$",
      "count": 1
    },
    {
      "id": "$1000134$",
      "count": 4
    }
  ],
  "address": "$address$"
}
```

##### 属性说明：

变量名 | 类型 | 描述 | 是否可为空
---|---|---|---
user_id | string | 买家用户ID | N
store_id | string | 商铺ID | N
books | class | 书籍购买列表 | N
address | string | 买家收货地址 | N

books数组：

变量名 | 类型 | 描述 | 是否可为空
---|---|---|---
id | string | 书籍的ID | N
count | string | 购买数量 | N

tips:
(1)假设已有store_id为'648全球连锁店'的商铺，其中出售id为book_id_1,book_id_2的书，库存分别为stock_1,stock_2 > 1
(2)不存在store_id为'华东师范大学'的商铺
#### Test Case

Request Header:

```json
    "token":"$token$",  //由服务器生成后保存在前端，假设此时token为yzm用户在某设备某时登录得到的token
```

1、商铺id不存在

Request Body:

```json
{
  "user_id": "yzm",
  "store_id": "华东师范大学",
  "books": [
    {
      "id": "book_id_3",
      "count": 1
    },
    {
      "id": "book_id_4",
      "count": 1
    }
  ],
  "address": "648"   
}
```

2、书籍id不存在

Request Body:

```json
{
  "user_id": "yzm",
  "store_id": "648全球连锁店",
  "books": [
    {
      "id": "book_id_3",
      "count": 1
    },
    {
      "id": "book_id_4",
      "count": 1
    }
  ],
  "address": "648"   
}
```

3、书籍库存不足

Request Body:

```json
{
  "user_id": "yzm",
  "store_id": "648全球连锁店",
  "books": [
    {
      "id": "book_id_1",
      "count": stock_1 + 1
    },
    {
      "id": "book_id_2",
      "count": 1
    }
  ],
  "address": "648"   
}
```

4、user_id与token不对应

Request Body:

```json
{
  "user_id": "stm",
  "store_id": "648全球连锁店",
  "books": [
    {
      "id": "book_id_1",
      "count": 1
    },
    {
      "id": "book_id_2",
      "count": 1
    }
  ],
  "address": "648"   
}
```

## 买家付款

#### URL：
POST http://[address]/buyer/payment 

#### Request

##### Body:
```json
{
  "user_id": "$buyer_id$",
  "order_id": "$order_id$",
  "password": "$password$"
}
```

##### 属性说明：

变量名 | 类型 | 描述 | 是否可为空
---|---|---|---
user_id | string | 买家用户ID | N
order_id | string | 订单ID | N
password | string | 买家用户密码 | N 

tips:
(1)yzm用户有order_id为id_1的订单未支付,order_id为id_2的订单已支付，order_id为id_3的订单已失效，不存在order_id为id_4的订单
(2)yzm用户的密码为aaaaaa
(3)不存在用户名为zyx的用户

#### Test Case

1、支付已付款的订单

Request Body:

```json
{
  "user_id": "yzm",
  "order_id": "$id_2$",
  "password": "aaaaaa"  
}
```

2、支付已失效的订单

Request Body:

```json
{
  "user_id": "yzm",
  "order_id": "$id_3$",
  "password": "aaaaaa"  
}
```

3、支付密码错误

Request Body:

```json
{
  "user_id": "yzm",
  "order_id": "$id_1$",
  "password": "aaaaab"  
}
```

4、成功支付订单

Request Body:

```json
{
  "user_id": "yzm",
  "order_id": "$id_1$",
  "password": "aaaaaa"  
}
```

5、用户不存在

Request Body:

```json
{
  "user_id": "zyx",
  "order_id": "$id_5$",
  "password": "aaaaaa"  
}
```

## 买家充值

#### URL：
POST http://[address]/buyer/add_funds/

#### Request



##### Body:
```json
{
  "user_id": "$user_id$",
  "password": "$password$",
  "add_value": 10
}
```

##### 属性说明：

key | 类型 | 描述 | 是否可为空
---|---|---|---
user_id | string | 买家用户ID | N
password | string | 用户密码 | N
add_value | int | 充值金额，以元为单位 | N

tips:
(1)yzm用户密码为aaaaaa
(2)充值金额在前端通过下拉菜单的形式限制在了1,6,18,30,68,118,198,348,648,后端也对应限制为这几个数字
(3)不存在用户名为zyx的用户
#### Test Case

1、用户不存在

Request Body:

```json
{
  "user_id": "zyx",
  "password": "aaaaaa",
  "add_value": 1  
}
```

2、密码错误

Request Body:

```json
{
  "user_id": "yzm",
  "password": "aaaaab",
  "add_value": 1  
}
```

3、充值金额不合规范

Request Body:

```json
{
  "user_id": "yzm",
  "password": "aaaaaa",
  "add_value": 5  
}
```

4、成功充值

Request Body:

```json
{
  "user_id": "yzm",
  "password": "aaaaaa",
  "add_value": 648  
}
```

## 用户查看所有订单

#### URL：
GET http://[address]/buyer/myorders/
(GET http://[address]/order/myorders/)

#### Request

##### Header:

key | 类型 | 描述 | 是否可为空
---|---|---|---
token | string | 登录产生的会话标识 | N


#### Test Case

Request Header:

```json
    "token":"$token$",  //由服务器生成后保存在前端，假设此时token为yzm用户在某设备某时登录得到的token
```

## 用户取消订单

#### URL：
POST http://[address]/buyer/cancel_order/
(POST http://[address]/order/cancel_order/)

#### Request

##### Header:

key | 类型 | 描述 | 是否可为空
---|---|---|---
token | string | 登录产生的会话标识 | N

##### Body:
```json
{
  "order_id": "$order_id$",
}
```
##### 属性说明：

key | 类型 | 描述 | 是否可为空
---|---|---|---
order_id | string | 订单ID | N

tips:
(1)假设yzm用户有order_id为id_1的订单未被取消且未失效，有order_id为id_2的订单已被取消，有order_id为id_3的订单已失效，没有order_id为id_4的订单

#### Test Case

Request Header:

```json
    "token":"$token$",  //由服务器生成后保存在前端，假设此时token为yzm用户在某设备某时登录得到的token
```

1、订单不存在

Request Body:

```json
{
  "order_id": "id_4"
}
```

2、订单已失效

Request Body:

```json
{
  "order_id": "id_3"
}
```

3、订单已取消

Request Body:

```json
{
  "order_id": "id_2"
}
```


4、成功取消订单

Request Body:

```json
{
  "order_id": "id_1"
}
```

## 用户确认收货，订单完成

#### URL：
POST http://[address]/buyer/complete_order/
(POST http://[address]/order/complete_order/)

#### Request

##### Header:

key | 类型 | 描述 | 是否可为空
---|---|---|---
token | string | 登录产生的会话标识 | N

##### Body:
```json
{
  "order_id": "$order_id$",
}
```
##### 属性说明：

key | 类型 | 描述 | 是否可为空
---|---|---|---
order_id | string | 订单ID | N

tips:
(1)假设yzm用户有order_id为id_1的订单未收货，有order_id为id_2的订单为非未收货状态(未发货、已取消、已失效、未付款等)，没有order_id为id_3的订单

#### Test Case

Request Header:

```json
    "token":"$token$",  //由服务器生成后保存在前端，假设此时token为yzm用户在某设备某时登录得到的token
```

1、订单不存在

Request Body:

```json
{
  "order_id": "id_3"
}
```

2、订单状态为非未收货

Request Body:

```json
{
  "order_id": "id_2"
}
```

3、成功收货

Request Body:

```json
{
  "order_id": "id_1"
}
```