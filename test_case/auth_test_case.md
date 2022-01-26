
# Test Case

## 注册用户

#### URL：
POST http://$address$/auth/register

#### Request

Body:
```json
{
    "user_id":"$user name$",
    "password":"$user password$"
}
```

变量名 | 类型 | 描述 | 是否可为空
---|---|---|---
user_id | string | 用户名 | N
password | string | 登陆密码 | N

tips:
(1)user_id与password长度要求在6~18
(2)数据库中已有用户名为yzm的账户注册
#### Test Case

1、用户名长度不符

Request Body:

```json
{
    "user_id":"y",
    "password":"aaaaaa"
}
```
2、密码长度不符

Request Body:

```json
{
    "user_id":"yzm_test",
    "password":"a"
}
```
3、用户名已注册

Request Body:

```json
{
    "user_id":"yzm",
    "password":"aaaaaa"
}
```

4、成功注册

Request Body:

```json
{
    "user_id":"yzm_test",
    "password":"aaaaaa"
}
```


## 注销用户

#### URL：
POST http://$address$/auth/unregiser

#### Request

Body:
```json
{
    "user_id":"$user name$",
    "password":"$user password$"
}
```

变量名 | 类型 | 描述 | 是否可为空
---|---|---|---
user_id | string | 用户名 | N
password | string | 登陆密码 | N

tips:
(1)数据库中已有用户名为yzm_test，密码为aaaaaa的账户注册
#### Test Case

1、用户名不存在

Request Body:

```json
{
    "user_id":"y",
    "password":"aaaaaa"
}
```
2、密码不正确

Request Body:

```json
{
    "user_id":"yzm_test",
    "password":"aaaaab"
}
```
3、成功注销

Request Body:

```json
{
    "user_id":"yzm_test",
    "password":"aaaaaa"
}
```

## 用户登录

#### URL：
POST http://$address$/auth/login

#### Request

Body:
```json
{
    "user_id":"$user name$",
    "password":"$user password$",
    "terminal":"$terminal code$"
}
```

变量名 | 类型 | 描述 | 是否可为空
---|---|---|---
user_id | string | 用户名 | N
password | string | 登陆密码 | N
terminal | string | 终端代码 | N

tips:
(1)数据库中已有用户名为yzm_test，密码为aaaaaa的账户注册
#### Test Case

1、用户名不存在

Request Body:

```json
{
    "user_id":"y",
    "password":"aaaaaa",
    "terminal":"127.0.0.1"
}
```
2、密码不正确

Request Body:

```json
{
    "user_id":"yzm",
    "password":"aaaaab",
    "terminal":"127.0.0.1"
}
```
3、成功登录

Request Body:

```json
{
    "user_id":"yzm",
    "password":"aaaaaa",
    "terminal":"127.0.0.1"
}
```

## 用户更改密码

#### URL：
POST http://$address$/auth/password

#### Request

Body:
```json
{
    "user_id":"$user name$",
    "oldPassword":"$old password$",
    "newPassword":"$new password$"
}
```

变量名 | 类型 | 描述 | 是否可为空
---|---|---|---
user_id | string | 用户名 | N
oldPassword | string | 旧的登陆密码 | N
newPassword | string | 新的登陆密码 | N

tips:
(1)数据库中已有用户名为yzm_test，密码为aaaaaa的账户注册
#### Test Case

1、用户名不存在

Request Body:

```json
{
    "user_id":"y",
    "oldPassword":"aaaaaa",
    "newPassword":"aaaaab"
}
```
2、旧密码不正确

Request Body:

```json
{
    "user_id":"yzm",
    "oldPassword":"aaaaab",
    "newPassword":"aaaaac"
}
```
3、新密码长度不符

Request Body:

```json
{
    "user_id":"yzm",
    "oldPassword":"aaaaaa",
    "newPassword":"a"
}
```
4、修改密码成功

Request Body:

```json
{
    "user_id":"yzm",
    "oldPassword":"aaaaaa",
    "newPassword":"aaaaab"
}
```

## 用户登出

#### URL：
POST http://$address$/auth/logout

#### Request

Headers:

key | 类型 | 描述
---|---|---
token | string | 访问token

Body:
```json
{
    "user_id":"$user name$"
}
```

变量名 | 类型 | 描述 | 是否可为空
---|---|---|---
user_id | string | 用户名 | N

tips:
(1)数据库中已有用户名为yzm_test与用户名为stm的账户注册
(2)token在用户登录后由服务器生成
(3)前端实现中，在网页中点击实现用户登出不会出现user_id与token不符的情况，故未向后端发出请求，采用前端window跳转刷新实现登出

#### Test Case

Request Header:

```json
    "token":"$token$",  //由服务器生成后保存在前端，假设此时token为yzm用户在某设备某时登录得到的token
```
1、用户名不存在

Request Body:

```json
{
    "user_id":"y"
}
```

2、用户名与token不符

Request Body:

```json
{
    "user_id":"stm"
}
```

3、成功登出

Request Body:

```json
{
    "user_id":"yzm"
}
```

## 用户个人信息查询

#### URL：
GET http://$address$/auth/info

#### Request

Headers:

key | 类型 | 描述
---|---|---
token | string | 访问token

Body:
```json
{}
```

#### Test Case

Request Header:

```json
    "token":"$token$",  //由服务器生成后保存在前端，假设此时token为yzm用户在某设备某时登录得到的token
```

## 用户个人信息修改(含头像)

#### URL：
POST http://$address$/auth/modify/

#### Request

Headers:

key | 类型 | 描述
---|---|---
token | string | 访问token

Body:
```json
{
    "avatar":"$avatar file$",
    "gender": "$gender$",
    "phone_number": "$phone_number$",
    "email": "$email$",
}
```

变量名 | 类型 | 描述 | 是否可为空
---|---|---|---
avatar | 文件 | 用户头像 | Y
gender | string | 用户性别 | Y
phone_number| string | 手机号 | Y
email | string | 邮箱 | Y

tips:
(1)后端要求gender字段取值为'男'、'女'、'未设置'
(2)后端(数据库)要求phone_number字段长度最大为11
(3)avatar字段由前端input[type='file']上传，格式为file,下面测试用例中分别假定为用户上传的某张图片
#### Test Case

Request Header:

```json
    "token":"$token$",  //由服务器生成后保存在前端，假设此时token为yzm用户在某设备某时登录得到的token
```
1、gender不合规范

Request Body:

```json
{
    "avatar":"$avatar file$",
    "gender": "男生",
    "phone_number": "17767325243",
    "email": "10195501426@stu.ecnu.edu.cn",
}
```
2、phone_number不合规范

Request Body:

```json
{
    "avatar":"$avatar file$",
    "gender": "男",
    "phone_number": "177673252431",
    "email": "10195501426@stu.ecnu.edu.cn",
}
```

3、avatar为空

Request Body:
```json
{
    "avatar":null,
    "gender": "男",
    "phone_number": "17767325243",
    "email": "10195501426@stu.ecnu.edu.cn",
}
```

4、gender为空

Request Body:
```json
{
    "avatar":"$avatar file$",
    "gender": null,
    "phone_number": "17767325243",
    "email": "10195501426@stu.ecnu.edu.cn",
}
```

5、phone_number为空

Request Body:
```json
{
    "avatar":"$avatar file$",
    "gender": "男",
    "phone_number": null,
    "email": "10195501426@stu.ecnu.edu.cn",
}
```

6、email为空

Request Body:
```json
{
    "avatar":"$avatar file$",
    "gender": "男",
    "phone_number": "17767325243",
    "email": null,
}
```

7、完整信息修改

Request Body:
```json
{
    "avatar":"$avatar file$",
    "gender": "男",
    "phone_number": "17767325243",
    "email": "10195501426@stu.ecnu.edu.cn",
}
```

## 添加收货地址

#### URL：
POST http://$address$/auth/add_address/

#### Request

Headers:

key | 类型 | 描述
---|---|---
token | string | 访问token

Body:
```json
{
    "name": "name",
    "address": "address",
    "phone_number": "phone_number",
}
```

变量名 | 类型 | 描述 | 是否可为空
---|---|---|---
name | string | 收货人姓名 | N
phoneNumber | string | 收货人电话 | N
address | string | 收货地址 | N

tips:
(1)由于未对用户个人进行实名认证，未存储个人姓名等信息，故name字段通过用户名来填充
(2)yzm用户已有地址'648'
(3)允许添加重复地址
(4)添加地址个数无上限
#### Test Case

Request Header:

```json
    "token":"$token$",  //由服务器生成后保存在前端，假设此时token为yzm用户在某设备某时登录得到的token
```

1、成功添加地址

Request Body:

```json
{
    "name": "yzm",
    "address": "华东师范大学中山北路校区第五宿舍",
    "phone_number": "17767325243",
}
```

## 删除收货地址

#### URL：
POST http://$address$/auth/delete_address/

#### Request

Headers:

key | 类型 | 描述
---|---|---
token | string | 访问token

Body:
```json
{
    "address_id": "address_id"
}
```

变量名 | 类型 | 描述 | 是否可为空
---|---|---|---
address_id | string | 收货地址id | N

tips:
(1)假设yzm用户已添加address_id为id_1的地址，未添加address_id为id_2的地址,数据库中未存储address_id为id_3的地址
#### Test Case

Request Header:

```json
    "token":"$token$",  //由服务器生成后保存在前端，假设此时token为yzm用户在某设备某时登录得到的token
```

1、地址id不存在

Request Body:

```json
{
    "address_id": "$id_3$"
}
```

2、用户地址不匹配

Request Body:

```json
{
    "address_id": "$id_2$"
}
```

3、成功删除地址

Request Body:

```json
{
    "address_id": "$id_1$"
}
```