# **REST API**
## General Information

###API Request URI Prefix

- API Request URI Prefix：<aside class="notice">https://api-fix-premium.coinsuper.com</aside>

- API Request URI example(Personal Asset Information)：
  <aside class="notice">https://api-fix-premium.coinsuper.com/api/v1/asset/userAssetInfo</aside>

###API Specifications

- Access using HTTPS.
- All API requests are made as REST POST requests.
- Bodies of POST requests should be formatted as JSON.
- Return values will be formatted in JSON.
- Interfacing with our API requires encryption. This will be described in detail below.
- All POST headers must specify Content-Type=application/json

###POST Body Format

> Sample JSON POST Body

```
{
    "common":{
        "accesskey" : "1900000109",            // Personal Accesskey
        "sign":"sdfsdfa1231231sdfsdfsd"        // MD5 Encrypted Secret Key
        "timestamp":"1500000000000",        //UTC Time (Milliseconds)
    },
    "data":{
        ......                                //Data Specific to Request
    }
}

```

> Sample JSON Response

```
{
        "code":"1000",    // Status Code
        "msg":"success",    // Message Corresponding With Status Code
        "data":{
                    "timestamp":"1500000000000", //System UTC Time (Milliseconds)
                    "result":{
                                ....
                             }                  //Returned Data Specific to Request
                ..... 
               }    // Return Data
}
```

1.Parameter Descriptions：

    1.1 Request Parameters：
    
   Request parameters can be split into the "common" and "data" categories. Every request from 
   a user needs the same "common" parameters to be transmitted. These "common" parameters include 
   the "accesskey", "sign" and "timestamp". The "data" category will be specific to the type of 
   request being made. The "data" field must always be included in the JSON, even if it is left 
   empty for that type of request.
   
    1.2 Response Parameters：
    
   The response parameters "code" and "msg" will be returned every time. After a successful 
   request, "code" should be equal to "1000". If the code is not "1000", refer to the provided 
   table to find out what exception has occurred.

2.Quotation Accuracy：
   
    2.1 For crypto-crypto transactions, the price is accurate to 8 decimal places. quantity is 
        accuate to 4 decimal places.
   
    2.2 For crypto-fiat transactions, the price is accurate to 2 decimal places and the quantity 
        is accurate to 4 decimal places.
   
    2.3 The API servers will always use this degree of accuracy, regardless of decimal places of 
        request inputs in the "data" field.

3.Signature:
   
    All requests must contain a valid signature. The signature generation method is detailed 
    below.

4.Timestamp:
   
    All timestamps recieved or sent must use UTC time, specified down to atomic milliseconds.

5.To see which transaction pair symbols are supported, check the interface under section 3.5


## Get API Keys

###Signature Generation Method 

Our API requires a signature to be generated, to verify that information has not been tampered 
with or falsified by bad actors.

a. Sort all request parameters along with "accesskey" and "secretkey" according to the ASCII 
   code of the parameter key (small to large). Next, express these sorted parameters as a URL key-
   value pair collection (key1=value1&key2=value2…). This will be the pre-encryption signature string.

b. Apply MD5 encryption to the string. The final 32 bits will become the sign: XXXXXXXXXX...;

###Signature Generation Sample


>Complete Request Body  :

```json
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
```

Let us suppose there is a request with these parameters:

Request Parameters：
symbol : BTC/USD
num    : 50

Common Request Parameters:
accesskey : zhangsan
secretkey : zhangsan
timestamp : 1500000000000

i：The sorted pre-encryption signature string then is：

accesskey=zhangsan&num=50&secretkey=zhangsan&symbol=BTC/USD&timestamp=1500000000000

ii：Finally, we encrypt the raw signature string and assign it to signValue. This will be the 
   "sign" for the actual request body

signValue = md5(string);

signValue == acd1761b47fb5d65c1ef9e644adba4fc

###Status Code Table 

| Code Number | Code Message                      |
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

## User Asset Queries

### Personal Asset Information

> Request Sample:

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

> Response Sample:

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

Obtain your own personal asset information.

#### Endpoint Path

`/api/v1/asset/userAssetInfo`

#### Endpoint Details

`POST`

#### Request parameters

    Only common parameters must be included.

#### Response Parameters

| Parameter | Description           |
| --------- | --------------------- |
| userNo    | User's Account Number |
| email     | User Email            |
| timestamp | System Timestamp      |
| result    | Return Results        |
| total     | Total Balance         |
| available | Available Balance     |       

## Transactions

### Place a Buy Order

> Request Sample:

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

> Response Sample:

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

Place a buy order.

#### Endpoint Path

`/api/v1/order/buy`

#### Endpoint Details

`POST`

#### Request Parameters

| Parameter        | Mandatory | Description                  |
| ---------- | ---- | ------------------- |
| symbol     | Yes   Transaction Pair Symbol                  |
| priceLimit | Yes   Price Limit(String type)                 |
| orderType  | Yes   Order Type MKT- "Market Price"，LMT- "Limit Price" |
| quantity   | Yes   Buy Quantity(String type)                |
| amount     | Yes   Amount(String type)                      |

Explanation：
1. If order type is LMT, the API will use "priceLimit" and "quantity". "amount" should be 
equal to "0" and will not be used.
2. If order type is MKT, the API will only use "amount". "priceLimit' and "quantity" should 
be "0" and will not be used.

####  Response Parameters

| Parameter | Description     |
| --------- | --------------- |
| timestamp | Timestamp       |
| result    | Results         |
| orderNo   | Order Number ID |     


### Place a Sell Order

> Request Sample:

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

> Response Sample:

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

Place a sell order.

#### Endpoint Path

`/api/v1/order/sell`

#### Endpoint Details

`POST`

#### Request Parameters

| Parameter  | Mandatory | Description                              |
| ---------- | --------- | ---------------------------------------- |
| symbol     | Yes       | Transaction Pair Symbol                  |
| priceLimit | Yes       | Price Limit(String type)                 |
| orderType  | Yes       | Order Type MKT- "Market Price"，LMT- "Limit Price" |
| quantity   | Yes       | Buy Quantity(String type)                |
| amount     | Yes       | Amount(String type)                      |


####  Response Parameters

| Parameter | Description     |
| --------- | --------------- |
| timestamp | Timestamp       |
| result    | Results         |
| orderNo   | Order Number ID |



### Cancel Order

> Request Sample:

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

> Response Sample:

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

Cancel a previously placed order.

#### Endpoint Path

`/api/v1/order/cancel`

#### Endpoint Details

`POST`

#### Request Parameters

| Parameter | Mandatory | Description     |
| --------- | --------- | --------------- |
| orderNo   | Yes       | Order Number ID |


####  Response Parameters

| Parameter | Description                              |
| --------- | ---------------------------------------- |
| timestamp | Timestamp                                |
| result    | Results                                  |
| operate   | Result Description : "success" or "failure" |


### Batch Cancellation of Orders

> Request Sample:

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

> Response Sample:

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

User Cancels Orders

#### Endpoint Path

`/api/v1/order/batchCancel`

#### Endpoint Details

`POST`

#### Request Parameters

| Parameter     | Mandatory | Description       |
| ------- | ---- | -------- |
| orderNoList | Yes   | Order Number to Cancel|

####  Response Parameters

| Parameter | Description                              |
| --------- | ---------------------------------------- |
| timestamp | System Timestamp (ms)                    |
| result    | Return Result                            |
| operate   | Result Description : "success" or "failure" |


### Return Order List

> Request Sample:

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

> Response Sample:

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

Obtain a list of order information (maximum 50 at one time). The user passes in the Order IDs for the orders they wish to check.

#### Endpoint Path

`/api/v1/order/list`

#### Endpoint Details

`POST`

#### Request Parameters

| Parameter   | Mandatory | Description                              |
| ----------- | --------- | ---------------------------------------- |
| orderNoList | Yes       | Pass in comma separated order numbers. Maximum 50 at one time. |

####  Response Parameters

| Parameter         | Description                              |
| ----------------- | ---------------------------------------- |
| timestamp         | Timestamp                                |
| result            | Results                                  |
| orderNo           | Order Number                             |
| action            | Order Type (Buy/Sell)                    |
| orderType         | Order Sub-type MKT-Market Price，LMT-Limit Price |
| priceLimit        | Price Limit                              |
| state             | Order Status  UNDEAL:Not Executed，PARTDEAL:Partially Executed，DEAL:Order Complete，CANCEL: Canceled |
| quantity          | Order Quantity                           |
| quantityRemaining | Remaining Quantity                       |
| amount            | Order Amount                             |
| amountRemaining   | Remaining Amount                         |
| fee               | Transaction Fee                          |
| symbol            | Trade Pair                               |
| utcUpdate         | Timestamp of Last Transaction            |
| utcCreate         | Timestamp on Order Creation              |


### Return Detailed Order List

> Request Sample:

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

> Response Sample:

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

Returns a list of information for up to 50 Orders. This information is more detailed than 2.4 
and includes individual transactions related to the order.

#### Endpoint Path

`/api/v1/order/details`

#### Endpoint Details

`POST`

#### Request Parameters

| Parameter   | Mandatory | Description                              |
| ----------- | --------- | ---------------------------------------- |
| orderNoList | Yes       | Pass in comma separated order numbers. Maximum 50 at one time. |

####  Response Parameters

| Parameter         | Description                              |
| ----------------- | ---------------------------------------- |
| timestamp         | Timestamp                                |
| result            | Results                                  |
| orderNo           | Order Number                             |
| priceLimit        | Price Limit                              |
| price             | Transaction Price                        |
| quantity          | Order Quantity                           |
| quantityRemaining | Remaining Quantity                       |
| amount            | Order Amount                             |
| amountRemaining   | Remaining Amount                         |
| volume            | Volume of Transaction                    |
| action            | Order Type (Buy/Sell)                    |
| orderType         | Order Sub-type MKT-Market Price，LMT-Limit Price |
| fee               | Transaction Fee                          |
| feeCurrency       | Transaction Fee Currency                 |
| state             | Order Status  UNDEAL:Not Executed，PARTDEAL:Partially Executed，DEAL:Order Complete，CANCEL: Canceled |
| matchType         | Transaction Type：MAKER-Passive Match，TAKER-Active Match |
| symbol            | Trade Pair                               |
| detail            | Detailed Transaction Information         |
| utcDeal           | Timestamp of Transaction                 |


### Pending Order List

> Request Sample:

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

> Response Sample:

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

Obtain a list of pending orders (up to 1000) for a trade pair.

#### Endpoint Path

`/api/v1/order/openList`

#### Endpoint Details

`POST`

#### Request Parameters

| Parameter | Mandatory | Description                              |
| --------- | --------- | ---------------------------------------- |
| symbol    | No        | Trade Pair (Querries all Trade Pairs if unfilled) |
| num       | Yes       | Number of results to return (Max 1000)   |

####  Response Parameters

| Parameter | Description                |
| --------- | -------------------------- |
| timestamp | Timestamp                  |
| result    | Results, List of order IDs |


### Query Personal Order History

> Request Sample:

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

> Response Sample:

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

Request Personal Order History

#### Endpoint Path

`/api/v1/order/history`

#### Endpoint Details

`POST`

#### Request Parameters

| Parameter    | Mandatory | Description                              |
| ------------ | --------- | ---------------------------------------- |
| symbol       | Yes       | Trade Pair                               |
| utcStart     | Yes       | Query Start Time (≤ order creation time in the results, timestamps in ms) |
| utcEnd       | NO        | Query End Time ( ≥ order creation time in the results, timestamps in ms) |
| startOrderNo | NO        | Start Order Number (Its data are excluded from the query results. If it is not passed, the first page of data will be queried. ) |
| withTrade    | NO        | Return the Transaction Details of Corresponding Orders or Not (true-Return, false-Not Return (in lower case)) |
| size         | NO        | Number of Results (temporarily limited to 1-100) |

####  Response Parameters

| Parameter         | Description                              |
| ----------------- | ---------------------------------------- |
| timestamp         | Timestamp (ms)                           |
| result            | Return Result                            |
| orderNo           | Order No.                                |
| action            | Order Type SELL-Sell/BUY-Buy             |
| orderType         | Order Sub-type  MKT-Market Price, LMT-Limit Price |
| priceLimit        | Price Limit                              |
| priceAverage      | Average Transaction Price                |
| state             | Order State  UNDEAL: Not Executed，PARTDEAL: Partially Executed，DEAL: Order Complete，CANCEL: Cancelled |
| quantity          | Order Quantity                           |
| quantityRemaining | Remaining Quantity                       |
| amount            | Order Amount                             |
| amountRemaining   | Remaining Amount                         |
| fee               | Transaction Fee                          |
| feeCurrency       | Transaction Fee Currency                 |
| symbol            | Trade Pair                               |
| utcUpdate         | Timestamp of Last Transaction (ms)       |
| utcCreate         | Timestamp on Order Creation (ms)         |
| detail            | Transaction Details                      |
| matchType         | Transaction Type: PASSIVE-Passive Match，ACTIVE-Active Match |
| dealNo            | Transaction Number                       |
| volume            | Transaction Volume                       |
| price             | Transaction Price                        |
| utcDeal           | Transaction Timestamp (ms)               |


### Query Personal Trade History

> Request Sample:

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

> Response Sample:

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

Request Personal Trade History

#### Endpoint Path

`/api/v1/order/tradeHistory`

#### Endpoint Details

`POST`

#### Request Parameters

| FieldName   | Mandatory | Description                              |
| ----------- | --------- | ---------------------------------------- |
| utcStart    | Yes       | Query Start Time (≤ order creation time in the results, timestamps in ms) |
| symbol      | NO        | Trade Pair                               |
| utcEnd      | NO        | Query End Time ( ≥ order creation time in the results, timestamps in ms) |
| startDealNo | NO        | Start Transaction Number (Its data are excluded from the query results. If it is not passed, the first page of data will be queried.) |
| size        | NO        | Number of Results (When the last result has the same order number as the size+1 result, return the size+1 result and show size+1 results) |
####  Response Parameters

| Parameter   | Description                              |
| ----------- | ---------------------------------------- |
| timestamp   | Timestamp (ms)                           |
| result      | Return Result                            |
| orderNo     | Order Number                             |
| action      | Order Type SELL-Sell/BUY-Buy             |
| orderType   | Order Sub-type  MKT-Market Price, LMT-Limit Price |
| symbol      | Trade Pair                               |
| matchType   | Transaction Type: PASSIVE-Passive Match，ACTIVE-Active Match |
| dealNo      | Transaction Number                       |
| volume      | Transaction Volume                       |
| price       | Transaction Price                        |
| fee         | Transaction Fee                          |
| feeCurrency | Transaction Fee Currency                 |
| utcDeal     | Transaction Timestamp (ms)               |



##Quotes

### Detailed Market Quote (10%)

> Request Sample:

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

> Response Sample:

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

Obtain top 10% bids and asks for a trade pair.

#### Endpoint Path

`/api/v1/market/depth`

#### Endpoint Details

`POST`

#### Request Parameters

| Parameter | Mandatory | Description |
| --------- | --------- | ----------- |
| symbol    | Yes       | Trade Pair  |

####  Response Parameters

| Parameter  | Description       |
| ---------- | ----------------- |
| timestamp  | Timestamp         |
| result     | Results           |
| asks       | Ask Offers        |
| bids       | Bid Offers        |
| limitPrice | Price Limit       |
| quantity   | Quantity on Offer |


### Detailed Market Quote (1-50)

> Request Sample:

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

> Response Sample:

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

Return top 1-50 asks and bids for a trade pair.

#### Endpoint Path

`/api/v1/market/orderBook`

#### Endpoint Details

`POST`

#### Request Parameters

| Parameter | Mandatory | Description                 |
| --------- | --------- | --------------------------- |
| num       | Yes       | Number of results to return |
| symbol    | Yes       | Trade Pair                  |

####  Response Parameters

| Parameter  | Description       |
| ---------- | ----------------- |
| timestamp  | Timestamp         |
| result     | Results           |
| asks       | Ask Offers        |
| bids       | Bid Offers        |
| limitPrice | Price Limit       |
| quantity   | Quantity on Offer |


### Real Time Price Quotes

> Request Sample:

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

> Response Sample:

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

Returns 5 minutes of "kline" data for a trade pair (Up to 300 latest data points in 5 minute increments.)

#### Endpoint Path

`/api/v1/market/kline`

#### Endpoint Details

`POST`

#### Request Parameters

| Parameter | Mandatory | Description                              |
| --------- | --------- | ---------------------------------------- |
| symbol    | Yes       | Trade Pair                               |
| num       | Yes       | Data Point Count                         |
| range     | No        | Kline range(available values:5min,15min,30min,1hour,6hour,12hour,1day),default value:5min |

####  Response Parameters

| Parameter | Description   |
| --------- | ------------- |
| timestamp | Timestamp     |
| result    | Results       |
| volume    | Volume Traded |
| high      | Highest Price |
| low       | Lowest Price  |
| open      | Opening Price |
| close     | Closing Price |


### Real-Time Transaction Quotes

> Request Sample:

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

> Response Sample:

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

Return 50 of the latest transactions for a currency pair.

#### Endpoint Path

`/api/v1/market/tickers`

#### Endpoint Details

`POST`

#### Request Parameters

| Parameter | Mandatory | Description |
| --------- | --------- | ----------- |
| symbol    | Yes       | Trade Pair  |

####  Response Parameters

| Parameter | Description                         |
| --------- | ----------------------------------- |
| timestamp | Timestamp                           |
| result    | Results                             |
| volume    | Volume Traded                       |
| dealTime  | Transaction Timestamp               |
| price     | Transaction Price                   |
| action    | Order Type (BUY - Buy, SELL - Sell) |



### Supported Currency Pairs List

> Request Sample:

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

> Response Sample:

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

Returns a list of all supported currency pairs, as well as some currency pair specific trading information.

#### Endpoint Path

`/api/v1/market/symbolList`

#### Endpoint Details

`POST`

#### Request Parameters

Only uses common parameters.

####  Response Parameters

| Parameter      | Description                              |
| -------------- | ---------------------------------------- |
| timestamp      | Timestamp                                |
| result         | Results                                  |
| symbol         | Trade Pair                               |
| quantityMin    | Minimum Transaction Quantity             |
| quantityMax    | Maximum Transaction Quantity             |
| priceMin       | Minimum Transaction Price                |
| priceMax       | Maximum Transaction Price                |
| deviationRatio | Maximum Deviation Ratio in Transaction Price |
