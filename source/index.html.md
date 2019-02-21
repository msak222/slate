---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# 介绍

Coinsuper Premium API是一套高性能RESTful JSON端点，专门用于满足应用程序开发人员，数据科学家和企业业务平台的关键任务需求。

此API参考包括开发人员集成第三方应用程序和平台所需的所有技术文档。

# 全局规则

##接口访问前缀

- 接口访问前缀：<aside class="notice">https://api.coinsuper.com</aside>

- 接口链接请求示例(获取个人资产信息链接)：
  <aside class="notice">https://api.coinsuper.com/api/v1/asset/userAssetInfo</aside>

##接口规则

- 采用HTTPS方式访问
- 接口请求均采用 POST 协议
- 接口统一请求数据为 JSON 格式
- 接口统一返回数据为 JSON 格式
- 当某一字段无数据时，该字段不返回数据（若返回类型为数组，则会返回空数组）
- 接口调用参数需加密验签，具体规则及秘钥对由交易所提供
- 所有POST请求接口header中都需要指定Content-Type=application/json

##全局数据格式定义

> 请求数据JSON格式定义

```
{
    "common":{
        "accesskey" : "1900000109",            // 通行证
        "sign":"sdfsdfa1231231sdfsdfsd"        // MD5加密签名
        "timestamp":"1500000000000",        //UTC时间戳(毫秒数)
    },
    "data":{
        ......                                //请求业务数据
    }
}

```

> 响应数据JSON格式定义

```
{
        "code":"1000",    // 状态码
        "msg":"success",    // 返回信息
        "data":{
                    "timestamp":"1500000000000", //系统时间戳(毫秒数)
                    "result":{
                                ....
                             }                  //系统返回结果
                ..... 
               }    // 返回数据
}
```

1.参数说明：

    1.1 请求参数：
   请求参数主要分为两部分，common和data，common和data每次必传。其中common为公有参数，该项每次调用接口
时必传，且common中的子项accesskey、sign、timestamp也为必传项；data项为每个接口的私有业务参数，其中的
子项根据每个接口可能会有不同，若data子项为空，data这个大项也必传。
   
    1.2 响应参数：
   响应参数code、msg每次必回传，请求业务处理成功后，code返回"1000"，返回其它值则表明业务请求有异常，具
体异常说明请参考下方的【全局通用状态码】表。

2.业务参数精度：
   
    2.1 若symbol为币币交易，则价格精度为小数点后8位，数量参数表示精度为4位；
   
    2.2 若symbol为法币交易，则价格精度为小数点后2位，数量参数表示精度为4位；
   
    2.3 在请求参数中，若参数格式不符合要求，系统将根据自行统一精度。

3.签名:
   
    所有请求均需要按本文档中的签名规则传递参数签名，签名生成规则请参考下方的参数签名规范；

4.时间戳:
   
    接口中所有timestamp字符串必须使用UTC时间(格式化时指定时区为0时区)，时间毫秒均采用原子时；

5.当前支持symbol，请通过[可交易的交易对列表]接口查询；

# 用户资产查询管理

## 获取个人资产信息

> 请求示例:

```json
{
    "common":{
        "accesskey" : "1900000109",
        "timestamp": "1500000000000",            
        "sign":"sdfsdfa1231231sdfsdfsd"        
    },
    "data":{

    }
}
```

> 返回示例:

```json
{
    "code":"1000",
    "msg":"success",
    "data":{

            "timestamp":"1500000000000",
            "result":{
                        "userNo":"12345",
                        "email":"demouser@demo.domain",
                        "asset":{
                                "USD":
                                    {
                                        "total":"123",
                                        "available": "12"
                                    },
                                "BTC": 
                                    {
                                        "total":"1234",
                                        "available": "123"
                                    }
                             }
                    }
            }
}
```

获取用户个人资产详细信息。

### 请求路径

`/api/v1/asset/userAssetInfo`

### 请求方式

`POST`

### 请求参数

    使用公有参数

### 响应参数

字段名 | 描述
--------- | -----------
userNo    | 用户的编号
email     | 用户邮箱 
timestamp | 系统时间戳(毫秒数) 
result    | 返回结果       
total     | 总余额        
available | 可用余额       


## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -X DELETE
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

#### 1.用户资产查询管理

##### 1.1 获取个人资产信息

功能描述: 

获取用户个人资产详细信息

请求路径:

```
/api/v1/asset/userAssetInfo
```

请求方式:

```
POST
```

接口请求参数:

 使用公有参数

响应参数:

| 字段名       | 描述         |
| --------- | ---------- |
| userNo    | 用户的编号      |
| email     | 用户邮箱       |
| timestamp | 系统时间戳(毫秒数) |
| result    | 返回结果       |
| total     | 总余额        |
| available | 可用余额       |

请求示例：  

```
{
    "common":{
        "accesskey" : "1900000109",            // 通行证
        "timestamp": "1500000000000",            // 时间戳
        "sign":"sdfsdfa1231231sdfsdfsd"        // MD5加密签名
    },
    "data":{

    }
}
```

  返回示例: 

```
{
    "code":"1000",
    "msg":"success",
    "data":{

            "timestamp":"1500000000000",
            "result":{
                        "userNo":"12345",
                        "email":"demouser@demo.domain",
                        "asset"{
                                "USD":
                                    {
                                        "total":"123",
                                        "available": "12"
                                    },
                                "BTC": 
                                    {
                                        "total":"1234",
                                        "available": "123"
                                    }
                                ...
                             }
                    }
            }
}
```

------

