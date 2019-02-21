---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - java
toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# 介绍

Welcome to the Kittn API! You can use our API to access Kittn API endpoints, which can get information on various cats, kittens, and breeds in our database.

We have language bindings in Shell, Ruby, Python, and JavaScript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

This example API documentation page was created with [Slate](https://github.com/lord/slate). Feel free to edit it and use it as a base for your own API's documentation.

# 认证

> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# 用户资产查询管理

## 获取个人资产信息

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
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

### 接口请求参数

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

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

