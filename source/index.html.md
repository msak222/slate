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


# 开通API

##签名描述

交易所将对请求数据的内容进行验签，以确定携带的信息是否未经篡改，因此定义生成 sign 字符串的方法。

a. 将所有传入的业务参数同公钥(accesskey)、秘钥(secretkey)一起按照参数名的ASCII码从小到大排序（字母序）
排序，使用URL键值对的格式（即key1=value1&key2=value2…）拼接成原始字符串string。
注意：值为空的参数不参与签名;

b. 对string进行md5运算，最终得到32位小写的sign值XXXXXXXXXX...;

##签名生成方法示例

假设有一个调用请求，

业务参数如下：
symbol : BTC/USD
num    : 50

公共请求参数如下:
accesskey : zhangsan
secretkey : zhangsan
timestamp : 1500000000000

i：经过 a 过程排序后的字符串 string 为：
accesskey=zhangsan&num=50&secretkey=zhangsan&symbol=BTC/USD&timestamp=1500000000000

ii：经过 b 过程后得到 sign 为 :
signValue = md5(string);

完整请求数据格式 :

{	
  "common":{	
    "accesskey":"zhangsan",
    "sign":"acd1761b47fb5d65c1ef9e644adba4fc",
    "timestamp":"1500000000000"
  },
  "data":{
    "symbol":"BTC/USD",
    "num"   :"50"
  }
}

##全局通用状态码

| 状态码  | 状态描述                                     |
| ---- | ---------------------------------------- |
| 1000 | success                                  |
| 1001 | asynchronous success                     |
| 2000 | system upgrading                         |
| 2001 | system internal error                    |
| 2002 | interface is unavailability              |
| 2003 | request is too frequently                |
| 2004 | fail to check sign                       |
| 2005 | parameter is invalid                     |
| 2006 | request failure                          |
| 2007 | accesskey has been forbidden             |
| 2008 | user not exist                           |
| 3001 | account balance is not enough            |
| 3002 | orderNo is not exist                     |
| 3003 | price is invalid                         |
| 3004 | symbol is invalid                        |
| 3005 | quantity is invalid                      |
| 3006 | ordertype is invalid                     |
| 3007 | action is invalid                        |
| 3008 | state is invalid                         |
| 3009 | num is invalid                           |
| 3010 | amount is invalid                        |
| 3011 | cancel order failure                     |
| 3012 | create order failure                     |
| 3013 | orderList is invalid                     |
| 3014 | symbol not trading                       |
| 3015 | order amount or quantity less than min setting |
| 3016 | price greater than max setting           |
| 3017 | user account forbidden                   |
| 3018 | order has execute                        |
| 3019 | orderNo num is more than the max setting |
| 3020 | price out of range                       |
| 3021 | order has canceled                       |
| 3027 | this symbol's API trading channel is not available |

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


## 查询订单交易详情(根据委托单号)

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
    "code":1000,
    "msg":"success",
    "data":{
            "timestamp":"1500000000",
            "result":[
                        { 
                                    "orderNo":"1612930403329875969",                     
                                    "priceLimit": "0.00000000",                
                                    "quantity": "1.00000000",                  
                                    "symbol":"ETC/BTC",                        
                                    "action":"SELL",                           
                                    "orderType":"MKT",                         
                                    "fee":"0.23",                              
                                    "quantityRemaining": "0.00000000",         
                                    "amount": "0.00000000",                    
                                    "amountRemaining": "0.00000000",           
                                    "state":"DEAL",                           
                                    "detail":[
                                                {
                                                    "matchType":"MAKER",        
                                                    "price": "0.00213640",       
                                                    "volume": "0.42220000",       
                                                    "utcDeal":"123123412",       
                                                    "fee":"0.01",              
                                                    "feeCurrency":"BTC"        
                                                },
                                               {
                                                 "matchType":"MAKER",        
                                                 "price": "0.00213640",       
                                                 "volume": "0.42220000",       
                                                 "utcDeal":"123123412",       
                                                 "fee":"0.01",              
                                                 "feeCurrency":"BUC"        
                                               } 
                                            ]    
                        },
                        { 
                        "orderNo":"1612930403329875969",                     
                        "priceLimit": "0.00000000",                
                        "quantity": "1.00000000",                  
                        "symbol":"ETC/BTC",                        
                        "action":"SELL",                           
                        "orderType":"MKT",                         
                        "fee":"0.23",                              
                        "quantityRemaining": "0.00000000",         
                        "amount": "0.00000000",                    
                        "amountRemaining": "0.00000000",           
                        "state":"DEAL",                           
                        "detail":[
                                    {
                                        "matchType":"MAKER",        
                                        "price": "0.00213640",       
                                        "volume": "0.42220000",       
                                        "utcDeal":"123123412",       
                                        "fee":"0.01",              
                                        "feeCurrency":"BTC"        
                                    },
                                   {
                                     "matchType":"MAKER",        
                                     "price": "0.00213640",       
                                     "volume": "0.42220000",       
                                     "utcDeal":"123123412",       
                                     "fee":"0.01",              
                                     "feeCurrency":"BUC"        
                                   } 
                                ]    
                        }
                    ]
            }
}
```

获取用户已成交委托单详细交易信息(一次最大查询50条已成交委托单的交易详情信息)

### 请求路径

`/api/v1/order/details`

### 请求方式

`POST`

### 请求参数

| 字段名         | 填写类型 | 描述                               |
| ----------- | ---- | -------------------------------- |
| orderNoList | 必填   | 要查询的委托单号列表(将委托单号拼接为字符串，中间用逗号分隔开) |

### 响应参数

| 字段名               | 描述                                       |
| ----------------- | ---------------------------------------- |
| timestamp         | 时间戳(毫秒数)                                 |
| result            | 返回结果                                     |
| orderNo           | 委托单号                                     |
| priceLimit        | 委托价格                                     |
| price             | 交易价格                                     |
| quantity          | 委托数量                                     |
| quantityRemaining | 委托剩余数量                                   |
| amount            | 委托金额                                     |
| amountRemaining   | 剩余金额                                     |
| volume            | 成交数量                                     |
| action            | 买卖类型 SELL-卖，BUY-买                        |
| orderType         | 委托单类型 MKT-市价，LMT-限价                      |
| fee               | 交易手续费（以detail交易明细中的手续费为准）                |
| feeCurrency       | 交易手续费币种                                  |
| state             | 委托单状态  UNDEAL:未成交，PARTDEAL:部分成交，DEAL:完全成交，CANCEL: 已撤销，PROCESSING：处理中 |
| matchType         | 匹配类型：MAKER-被动匹配，TAKER-主动匹配               |
| symbol            | 交易对                                      |
| detail            | 交易明细                                     |
| utcDeal           | 交易时间戳(毫秒级)                               |


## 挂单单号列表(个人未完全成交委托单列表)

> 请求示例:

```json
{
    "common":{
        "accesskey" : "1900000109",           
        "timestamp": "1500000000000",            
        "sign":"sdfsdfa1231231sdfsdfsd"        
    },
    "data":{
            "symbol":"ETC/BTC",                
            "num":"1000"		
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
            "result":[
                  "1603147161792831491",
                  "1603147093327106049",
                  "1603147072028430337"
             ]
    }
}
```

请求最新挂单单号列表(时间最近的委托单单号列表)

### 请求路径

`/api/v1/order/openList`

### 请求方式

`POST`

### 请求参数

| 字段名    | 填写类型 | 描述                |
| ------ | ---- | ----------------- |
| symbol | 非必填  | 交易对，若不填则查询所有交易对   |
| num    | 必填   | 请求的单号条数，最大不超过1000 |

### 响应参数

| 字段名       | 描述       |
| --------- | -------- |
| timestamp | 时间戳(毫秒数) |
| result    | 返回结果     |


## 查询个人历史委托单列表

> 请求示例:

```json
{
    "common":{
        "accesskey" : "1900000109",           
        "timestamp": "1500000000000",            
        "sign":"sdfsdfa1231231sdfsdfsd"        
    },
    "data":{
            "symbol":"ETC/BTC",                   
            "utcStart":"1536818874583",          
            "withTrade":"true"         
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
                            "orderNo":"123412313",             
                            "action":"SELL",                   
                            "orderType":"MKT",                  
                            "priceAverage": "0.00000000",
                            "priceLimit":"0.00000000",         
                            "symbol":"ETC/BTC",                  
                            "quantity" :"0.00010000",           
                            "quantityRemaining":"0.00010000",   
                            "amount":"0.00000000",               
                            "amountRemaining":"0.00000000",     
                            "fee":"0.00000000",                 
                            "utcUpdate":"1500000000000",        
                            "utcCreate":"1500000000000",          
                            "state":"CANCEL",                  
                            "detail": [
                                  {
                                      "dealNo": "234234114241",
                                      "matchType": "ACTIVE",
                                      "price": "0.00001000",
                                      "utcDeal": "1500000000000",
                                      "volume": "1.4266",
                                      "fee":"0.01",
                                      "feeCurrency":"BTC"
                                  },
                                  {
                                      "dealNo": "24234212312",
                                      "matchType": "PASSIVE",
                                      "price": "0.00001000",
                                      "utcDeal": "1500000000000",
                                      "volume": "0.00001000",
                                      "fee":"0.01",
                                      "feeCurrency":"BTC"
                                  }
                              ]
                        }
                    ]
            }
}
```

请求个人历史委托单列表

### 请求路径

`/api/v1/order/history`

### 请求方式

`POST`

### 请求参数

| 字段名          | 填写类型 | 描述                                      |
| ------------ | ---- | --------------------------------------- |
| symbol       | 必填   | 交易对                                     |
| utcStart     | 必填   | 查询开始时间(查询下单时间≥该时间的委托单，毫秒时间戳)            |
| utcEnd       | 非必填  | 查询结束时间(查询下单时间≤该时间的委托单，毫秒时间戳)            |
| startOrderNo | 非必填  | 起始委托单号（查询结果中不含该单号的数据，不传则表示查第一页数据）       |
| withTrade    | 非必填  | 是否需要返回对应委托单的交易明细（true-返回，false-不返回（小写）） |
| size         | 非必填  | 查询条数(取值范围暂定为1~100)                      |

### 响应参数

| 字段名               | 描述                                       |
| ----------------- | ---------------------------------------- |
| timestamp         | 时间戳(毫秒数)                                 |
| result            | 返回结果                                     |
| orderNo           | 委托单号                                     |
| action            | 买卖类型 SELL-卖，BUY-买                        |
| orderType         | 委托单类型 MKT-市价，LMT-限价                      |
| priceLimit        | 委托价格                                     |
| priceAverage      | 成交均价                                     |
| state             | 委托单状态  UNDEAL:未成交，PARTDEAL:部分成交，DEAL:完全成交，CANCEL: 已撤销 |
| quantity          | 委托数量                                     |
| quantityRemaining | 剩余数量                                     |
| amount            | 委托金额                                     |
| amountRemaining   | 剩余金额                                     |
| fee               | 手续费（以detail交易明细中的手续费为准）                  |
| feeCurrency       | 交易手续费币种                                  |
| symbol            | 交易对                                      |
| utcUpdate         | 最后成交时间戳(毫秒级)                             |
| utcCreate         | 委托时间戳(毫秒级)                               |
| detail            | 交易明细                                     |
| matchType         | 匹配类型：PASSIVE-被动匹配，ACTIVE-主动匹配            |
| dealNo            | 交易单号                                     |
| volume            | 成交数量                                     |
| price             | 交易价格                                     |
| utcDeal           | 交易时间戳(毫秒级)                               |


## 查询个人历史成交列表

> 请求示例:

```json
{
    "common":{
        "accesskey" : "1900000109",           
        "timestamp": "1500000000000",            
        "sign":"sdfsdfa1231231sdfsdfsd"        
    },
    "data":{
            "symbol":"ETC/BTC",                   
            "utcStart":"1536818874583",          
            "withTrade":"true"         
    }
}
```

> 返回示例:

```json
{
    "code":"1000",
    "msg":"success",
    "data":{
            "timestamp": "1500000000",
            "result":[ 
                        {
                          "dealNo":"12312312312312",   
                          "symbol": "BTC/USD",        
                          "matchType":"PASSIVE",       
                          "orderNo":"32423412412",      
                          "orderType":"MKT",           
                          "action":"SELL",              
                          "price": "0.00213640",      
                          "volume": "0.42220000",     
                          "utcDeal": "123123412",    
                          "fee":"0.01",				  
                          "feeCurrency":"USD"		   
                        }
                    ]
            }
}
```

请求个人历史成交列表

### 请求路径

`/api/v1/order/tradeHistory`

### 请求方式

`POST`

### 请求参数

| 字段名         | 填写类型 | 描述                                       |
| ----------- | ---- | ---------------------------------------- |
| utcStart    | 必填   | 查询开始时间(查询下单时间≥该时间的委托单，毫秒时间戳)             |
| symbol      | 非必填  | 交易对                                      |
| utcEnd      | 非必填  | 查询结束时间(查询下单时间≤该时间的委托单，毫秒时间戳)             |
| startDealNo | 非必填  | 起始交易单号（查询结果中不包含该单号的数据，不传则表示查第一页数据）       |
| size        | 非必填  | 查询条数（若最后一条数据和第size+1条数据的dealNo相同，则会将第size+1条数据一起返回，此时返回结果条数为size+1） |

### 响应参数

| 字段名         | 描述                            |
| ----------- | ----------------------------- |
| timestamp   | 时间戳(毫秒数)                      |
| result      | 返回结果                          |
| orderNo     | 委托单号                          |
| action      | 买卖类型 SELL-卖，BUY-买             |
| orderType   | 委托单类型 MKT-市价，LMT-限价           |
| symbol      | 交易对                           |
| matchType   | 匹配类型：PASSIVE-被动匹配，ACTIVE-主动匹配 |
| dealNo      | 交易单号                          |
| volume      | 成交数量                          |
| price       | 交易价格                          |
| fee         | 交易手续费                         |
| feeCurrency | 交易手续费币种                       |
| utcDeal     | 交易时间戳(毫秒级)                    |


#行情

## 深度行情(最新深度图数据)

> 请求示例:

```json
{
    "common":{
        "accesskey" : "1900000109",           
        "timestamp": "1500000000000",            
        "sign":"sdfsdfa1231231sdfsdfsd"        
    },
    "data":{
            "symbol":"ETC/BTC"          
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
                    "asks":[
                        {
                          "limitPrice":"123.000000000000000000000000000000",
                          "quantity":"15.990000000000000000000000000000"
                        }
                    ],
                    "bids":[
                        {
                          "limitPrice":"621.000000000000000000000000000000",
                          "quantity":"0.110000000000000000000000000000"
                        }
                    ]
                   }
    }
}
```

获取当前深度图行情数据，深度比例为百分之十

### 请求路径

`/api/v1/market/depth`

### 请求方式

`POST`

### 请求参数

| 字段名    | 填写类型 | 描述   |
| ------ | ---- | ---- |
| symbol | 必填   | 交易对  |

### 响应参数

| 字段名        | 描述       |
| ---------- | -------- |
| timestamp  | 时间戳(毫秒数) |
| result     | 返回结果     |
| asks       | 卖盘深度数据   |
| bids       | 买盘深度数据   |
| limitPrice | 委托价格     |
| quantity   | 数量       |


## 价格行情(最新订单簿数据)

> 请求示例:

```json
{
    "common":{
        "accesskey" : "1900000109",           
        "timestamp": "1500000000000",            
        "sign":"sdfsdfa1231231sdfsdfsd"        
    },
    "data":{
            "symbol":"ETC/BTC",                 
            "num":"50"            
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
                    "asks":[{"limitPrice":"123.00000000","quantity":"161.1300"}],
                    "bids":[{"limitPrice":"86.06000000","quantity":"0.7800"}]
                   }
    }
}
```

获取当前订单簿行情数据(最大买卖盘各请求1-50条记录)

### 请求路径

`/api/v1/market/orderBook`

### 请求方式

`POST`

### 请求参数

| 字段名    | 填写类型 | 描述   |
| ------ | ---- | ---- |
| num    | 必填   | 数量   |
| symbol | 必填   | 交易对  |

### 响应参数

| 字段名        | 描述                                   |
| ---------- | ------------------------------------ |
| timestamp  | 时间戳(毫秒数)                             |
| result     | 返回结果                                 |
| asks       | 卖盘深度数据                               |
| bids       | 买盘深度数据                               |
| limitPrice | 委托价格,注：币币交易对精度为小数点后8位，法币交易对精度为小数点后两位 |
| quantity   | 数量,注:精度为4位小数                         |


## 分时行情(最新K线数据)

> 请求示例:

```json
{
    "common":{
        "accesskey" : "1900000109",           
        "timestamp": "1500000000000",            
        "sign":"sdfsdfa1231231sdfsdfsd"        
    },
    "data":{
            "symbol":"ETC/BTC",                 
            "num":"50"            
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
            "result":[
                            {
                              "close": "0.00205900",
                              "high": "0.00206080",
                              "low": "0.00205340",
                              "open": "0.00205660",
                              "volume": "100.1218",
                              "timestamp": "1500000000000"
                            },
                            {
                              "close": "0.00205900",
                              "high": "0.00206080",
                              "low": "0.00205340",
                              "open": "0.00205660",
                              "volume": "100.1218",
                              "timestamp": "1500000000000"
                            }
                      ]
            }
}
```

获取5分钟K线行情数据(1-300条数据)

### 请求路径

`/api/v1/market/kline`

### 请求方式

`POST`

### 请求参数

| 字段名    | 填写类型 | 描述                                       |
| ------ | ---- | ---------------------------------------- |
| symbol | 必填   | 交易对                                      |
| num    | 必填   | 条数最大300条                                 |
| range  | 非必填  | K线类型(可选值:5min,15min,30min,1hour,6hour,12hour,1day),不传时默认值为5min |

### 响应参数

| 字段名       | 描述       |
| --------- | -------- |
| timestamp | 时间戳(毫秒数) |
| result    | 返回结果     |
| volume    | 交易量      |
| high      | 最高价      |
| low       | 最低价      |
| open      | 开盘价      |
| close     | 收盘价      |


## 实时成交行情（最新成交行情数据）

> 请求示例:

```json
{
    "common":{
        "accesskey" : "1900000109",           
        "timestamp": "1500000000000",            
        "sign":"sdfsdfa1231231sdfsdfsd"        
    },
    "data":{
            "symbol":"ETC/BTC"          
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
            "result":[
                  {
                  "price": "0.00212320",
                  "tradeType": "SELL",
                  "volume": "0.3499",
                  "timestamp": "1500000000000"
                  },
                  {
                  "price": "0.00212320",
                  "tradeType": "SELL",
                  "volume": "0.3499",
                  "timestamp": "1500000000000"
                  }
             ]
    }
}
```

请求最新成交行情数据(时间最近的50条成交数据)

### 请求路径

`/api/v1/market/tickers`

### 请求方式

`POST`

### 请求参数

| 字段名    | 填写类型 | 描述                                       |
| ------ | ---- | ---------------------------------------- |
| symbol | 必填   | 交易对                                      |

### 响应参数

| 字段名       | 描述                |
| --------- | ----------------- |
| timestamp | 时间戳(毫秒数)          |
| result    | 返回结果              |
| volume    | 交易数量              |
| dealTime  | 交易时间(时间戳)         |
| price     | 交易价格              |
| action    | 交易类型 BUY-买,SELL-卖 |



## 实时成交行情（最新成交行情数据）

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
            "result":[
                {
                  "symbol": "BTC/USD",						
                  "quantityMin": "0.00100000000",		
                  "quantityMax": "5000.00000000000",
                  "priceMin": "0.00100000000",			
                  "priceMax": "9999.00000000000",		
                  "deviationRatio": "0.3"				    
            	},
              	{
                  "symbol": "ETH/BTC",						
                  "quantityMin": "0.00001000000",		
                  "quantityMax": "8000.00000000000",
                  "priceMin": "0.00001000000",			
                  "priceMax": "8000.00000000000",		
                  "deviationRatio": "0.3"					
            	}
             ]
    }
}
```

请求目前交易所的可交易的交易对列表

### 请求路径

`/api/v1/market/symbolList`

### 请求方式

`POST`

### 请求参数

使用公有参数

### 响应参数

| 字段名            | 描述        |
| -------------- | --------- |
| timestamp      | 时间戳(毫秒数)  |
| result         | 返回结果      |
| symbol         | 交易对       |
| quantityMin    | 最小委托量     |
| quantityMax    | 最大委托量     |
| priceMin       | 最小委托价     |
| priceMax       | 最大委托价     |
| deviationRatio | 委托价最大偏差系数 |
