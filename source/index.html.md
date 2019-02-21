---
title: Coinsuper Premium API Documentation

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

# 委托和交易

## 买入委托

> 请求示例:

```json
{
    "common":{
        "accesskey" : "1900000109",           
        "timestamp": "1500000000000",            
        "sign":"sdfsdfa1231231sdfsdfsd"        
    },
    "data":{
        "orderType":"LMT",
        "symbol":"ETC/BTC",
        "priceLimit":"12345.67", 
        "amount":"0",
        "quantity":"2.34"
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
                        "orderNo":"2134123412" 
                     }
            }
}
```

用户委托买入(挂单)。

### 请求路径

`/api/v1/order/buy`

### 请求方式

`POST`

### 请求参数

| 字段名        | 填写类型 | 描述                  |
| ---------- | ---- | ------------------- |
| symbol     | 必填   | 交易对                 |
| priceLimit | 必填   | 买入价格(字符串类型)         |
| orderType  | 必填   | 委托单类型 MKT-市价，LMT-限价 |
| quantity   | 必填   | 买入数量(字符串类型)         |
| amount     | 必填   | 金额(字符串类型)           |  

### 响应参数

| 字段名       | 描述         |
| --------- | ---------- |
| timestamp | 系统时间戳(毫秒数) |
| result    | 返回结果       |
| orderNo   | 委托单号       |     


## 卖出委托

> 请求示例:

```json
{
    "common":{
        "accesskey" : "1900000109",           
        "timestamp": "1500000000000",            
        "sign":"sdfsdfa1231231sdfsdfsd"        
    },
    "data":{
            "orderType":"LMT",
            "symbol":"ETC/BTC",
            "priceLimit":"12345.67", 
            "amount":"0",
            "quantity":"2.34"
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
                        "orderNo":"2134123412"
                     }
           }
}
```

用户委托卖出(挂单)。

### 请求路径

`/api/v1/order/sell`

### 请求方式

`POST`

### 请求参数

| 字段名        | 填写类型 | 描述                  |
| ---------- | ---- | ------------------- |
| symbol     | 必填   | 交易对                 |
| priceLimit | 必填   | 卖出价格(字符串类型)         |
| orderType  | 必填   | 委托单类型：MKT-市价，LMT-限价 |
| quantity   | 必填   | 卖出数量(字符串类型)         |
| amount     | 必填   | 金额(字符串类型)           |

### 响应参数

| 字段名       | 描述         |
| --------- | ---------- |
| timestamp | 系统时间戳(毫秒数) |
| result    | 返回结果       |
| orderNo   | 委托单号       |


## 取消委托

> 请求示例:

```json
{
    "common":{
        "accesskey" : "1900000109",           
        "timestamp": "1500000000000",            
        "sign":"sdfsdfa1231231sdfsdfsd"        
    },
    "data":{
            "orderNo":"12341235123412"
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
                        "operate":"success"
                     }
            }
}
```

用户取消委托单。

### 请求路径

`/api/v1/order/cancel`

### 请求方式

`POST`

### 请求参数

| 字段名     | 填写类型 | 描述       |
| ------- | ---- | -------- |
| orderNo | 必填   | 要取消的委托单号 |

### 响应参数

| 字段名       | 描述                           |
| --------- | ---------------------------- |
| timestamp | 系统时间戳(毫秒数)                   |
| result    | 返回结果                         |
| operate   | 操作结果 : success-成功，failure-失败 |


## 批量取消委托

> 请求示例:

```json
{
    "common":{
        "accesskey" : "1900000109",           
        "timestamp": "1500000000000",            
        "sign":"sdfsdfa1231231sdfsdfsd"        
    },
    "data":{
            "orderNoList":"1612930403329875969,1612930403329875970"
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
                        "failResultList": [
                            {
                                "errMsg": "orderNo is invalid",
                                "orderNo": "1612930403329875969"
                            }
                        ],
                        "successNoList": ["1612930403329875970"]
                     }
            }
}
```

用户批量取消委托单。

### 请求路径

`/api/v1/order/batchCancel`

### 请求方式

`POST`

### 请求参数

| 字段名     | 填写类型 | 描述       |
| ------- | ---- | -------- |
| orderNoList | 必填   | 要取消的委托单号|

### 响应参数

| 字段名       | 描述                           |
| --------- | ---------------------------- |
| timestamp | 系统时间戳(毫秒数)                   |
| result    | 返回结果                         |
| operate   | 操作结果 : success-成功，failure-失败 |


## 查询委托单状态(根据委托单号)

> 请求示例:

```json
{
    "common":{
        "accesskey" : "1900000109",           
        "timestamp": "1500000000000",            
        "sign":"sdfsdfa1231231sdfsdfsd"        
    },
    "data":{
            "orderNoList":"1612930403329875969,1612930403329875970"
    }
}
```

> 返回示例:

```json
{
    "code":"1000",
    "msg":"success",
    "data":{
            "timestamp":1500000000123,
            "result":[ 
                        {
                            "orderNo":"1612930403329875969",                 
                            "action":"SELL",                     
                            "orderType":"MKT",                  
                            "priceLimit":"0.00000000",           
                            "symbol":"ETC/BTC",                  
                            "quantity" :"0.00010000",            
                            "quantityRemaining":"0.00010000",    
                            "amount":"0.00000000",               
                            "amountRemaining":"0.00000000",      
                            "fee":"0.00000000",                  
                            "utcUpdate":"1500000000000",         
                            "utcCreate":"1500000000000",         
                            "state":"CANCEL"                  
                        },
                        {
                            "orderNo":"1612930403329875970",                 
                            "action":"SELL",                     
                            "orderType":"MKT",                  
                            "priceLimit":"0.00000000",           
                            "symbol":"ETC/BTC",                  
                            "quantity" :"0.00010000",            
                            "quantityRemaining":"0.00010000",    
                            "amount":"0.00000000",               
                            "amountRemaining":"0.00000000",      
                            "fee":"0.00000000",                  
                            "utcUpdate":"1500000000000",         
                            "utcCreate":"1500000000000",         
                            "state":"CANCEL"                  
                        }
                    ]
            }
}
```

获取用户委托单状态(单次查询最大限50条记录)。用户可以传入委托单号来获取相对应的委托单状态列表。

### 请求路径

`/api/v1/order/list`

### 请求方式

`POST`

### 请求参数

| 字段名         | 填写类型 | 描述                                       |
| ----------- | ---- | ---------------------------------------- |
| orderNoList | 必填   | 要查询的委托单号列表(将委托单号拼接为字符串，中间用逗号分隔开)单次最大查询单号为50条 |

### 响应参数

| 字段名               | 描述                                       |
| ----------------- | ---------------------------------------- |
| timestamp         | 时间戳(毫秒数)                                 |
| result            | 返回结果                                     |
| orderNo           | 委托单号                                     |
| action            | 买卖类型 SELL-卖，BUY-买                        |
| orderType         | 委托单类型 MKT-市价，LMT-限价                      |
| priceLimit        | 委托价格                                     |
| state             | 委托单状态  UNDEAL:未成交，PARTDEAL:部分成交，DEAL:完全成交，CANCEL: 已撤销，PROCESSING：处理中 |
| quantity          | 委托数量                                     |
| quantityRemaining | 剩余数量                                     |
| amount            | 委托金额                                     |
| amountRemaining   | 剩余金额                                     |
| fee               | 手续费                                      |
| symbol            | 交易对                                      |
| utcUpdate         | 最后成交时间戳(毫秒级)                             |
| utcCreate         | 委托时间戳(毫秒级)                               |
