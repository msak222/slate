# **WEBSOCKET API**
# General Information

##API Information

- A websocket connection must be made beforehand in order to obtain data from the streams.
- This API consists of Websocket Push.
- All data will be returned in the form of JSON.
- If there is no data, nothing will be returned. (If the return type is an array, an empty array will be returned)
- If the Websocket link is broken, follow a disconnect/reconnect protocol to re-establish the link.
- The client needs to send the below specified parameters in order to establish the websocket connection.

##Websocket Connection Instructions

###Public Quote Topic Parameters

1.Base endpoint server name and port：
    host:mqtt.coinsuper.com
    port:1883
    (When you dev with Javascript,you should use host:api-mqtt-premium.coinsuper.com,port:443,isSSL:true)

2.Client Signature：
   Each message will have a unique signature. You must follow the rules below in order to obtain any data from this Websocket feed.
   Signature generation method：clientId = md5(access_key) + md5(email) + md5(topic)
   That is to say, the signature is the spliced combination of the md5 hashes of the user's access key, user's email address and query topic.

3.userName:
    api_subscriber

4.userCode:
    da65be18f44b33b69603c39fd03ddf49

5.Topic：
  topic=$env$subTopic
  Where the $env=exchange_ha_prod_hk，"$subTopic"s are described below.

###Private Data Topic Parameters

1.Base endpoint server name and port：
    host:mqtt.coinsuper.com
    port:1883
    (When you dev with Javascript,you should use host:api-mqtt-premium.coinsuper.com,port:443,isSSL:true)

2.Client Signature：
   clientId = $access_key
   (1. “$access_key” is the access_key a user get after enabling API access; 2. One clientId can only be used for one link, which in turn connects to multiple topics. )

3.userName:
   userName = api_private
   (“api_private” is a fixed value)
4.userCode:
   userCode=$email+","+$timeStamp+","+$sign
   ($sign=md5($access_key+","+$secret_key+","+$email+","+$timeStamp)，$email is the user account email address; $timeStamp is the timestamp in millisecond; $secret_key is the secret_key a user get after enabling API access.)
5.Topic：
  topic=$env$subTopic
  Where the $env=exchange_ha_prod_hk，"$subTopic"s are described below.

# Quotes

##  Latest Detailed Market Data

> Topic Sample:

```
exchange_ha_prod_hk/api/ws/v1/market/depth/BTC_USD
```

> Response Sample:

```json
{"symbol":"BTC/USD","asks":[{"price":"6681.39","quantity":"25.1470","stepValue":null},{"price":"6688.06","quantity":"50.7548","stepValue":null},{"price":"6694.74","quantity":"73.7040","stepValue":null},{"price":"6701.41","quantity":"99.5820","stepValue":null},{"price":"6708.09","quantity":"121.3416","stepValue":null},{"price":"6714.76","quantity":"146.3899","stepValue":null},{"price":"6721.44","quantity":"167.2699","stepValue":null},{"price":"6728.11","quantity":"191.5654","stepValue":null},{"price":"6734.79","quantity":"216.1048","stepValue":null},{"price":"6741.46","quantity":"241.8331","stepValue":null},{"price":"6748.14","quantity":"263.7908","stepValue":null},{"price":"6754.81","quantity":"289.2063","stepValue":null},{"price":"6761.49","quantity":"307.2237","stepValue":null},{"price":"6768.16","quantity":"327.0794","stepValue":null},{"price":"6774.84","quantity":"348.9058","stepValue":null},{"price":"6781.51","quantity":"362.0316","stepValue":null},{"price":"6788.19","quantity":"377.8478","stepValue":null},{"price":"6794.86","quantity":"390.7546","stepValue":null},{"price":"6801.53","quantity":"404.5182","stepValue":null},{"price":"6808.21","quantity":"418.8692","stepValue":null},{"price":"6814.88","quantity":"430.9434","stepValue":null},{"price":"6821.56","quantity":"444.9484","stepValue":null},{"price":"6828.23","quantity":"453.2668","stepValue":null},{"price":"6834.91","quantity":"464.4291","stepValue":null},{"price":"6841.58","quantity":"474.2563","stepValue":null},{"price":"6848.26","quantity":"483.8511","stepValue":null},{"price":"6854.93","quantity":"491.3878","stepValue":null},{"price":"6861.61","quantity":"497.8821","stepValue":null},{"price":"6868.28","quantity":"505.1597","stepValue":null},{"price":"6874.96","quantity":"509.4884","stepValue":null},{"price":"6881.63","quantity":"513.9862","stepValue":null},{"price":"6888.31","quantity":"517.1398","stepValue":null},{"price":"6894.98","quantity":"518.6871","stepValue":null},{"price":"6901.66","quantity":"520.9932","stepValue":null},{"price":"6908.33","quantity":"521.4774","stepValue":null},{"price":"6915.00","quantity":"522.2706","stepValue":null},{"price":"6921.68","quantity":"522.8107","stepValue":null},{"price":"6928.35","quantity":"522.8107","stepValue":null},{"price":"6935.03","quantity":"523.0449","stepValue":null},{"price":"6941.70","quantity":"523.5768","stepValue":null},{"price":"6948.38","quantity":"523.8180","stepValue":null},{"price":"6955.05","quantity":"524.0642","stepValue":null},{"price":"6961.73","quantity":"525.0642","stepValue":null},{"price":"6968.40","quantity":"525.0642","stepValue":null},{"price":"6975.08","quantity":"525.2080","stepValue":null},{"price":"6981.75","quantity":"525.2080","stepValue":null},{"price":"6988.43","quantity":"525.2080","stepValue":null},{"price":"6995.10","quantity":"525.2080","stepValue":null},{"price":"7001.78","quantity":"525.2080","stepValue":null},{"price":"7008.45","quantity":"525.2080","stepValue":null}],"bids":[{"price":"6674.72","quantity":"0.0000","stepValue":null},{"price":"6668.04","quantity":"0.8490","stepValue":null},{"price":"6661.37","quantity":"0.8490","stepValue":null},{"price":"6654.69","quantity":"0.8490","stepValue":null},{"price":"6648.02","quantity":"0.8490","stepValue":null},{"price":"6641.34","quantity":"0.8490","stepValue":null},{"price":"6634.67","quantity":"0.8490","stepValue":null},{"price":"6627.99","quantity":"0.8490","stepValue":null},{"price":"6621.32","quantity":"0.8490","stepValue":null},{"price":"6614.64","quantity":"2.1055","stepValue":null},{"price":"6607.97","quantity":"4.7561","stepValue":null},{"price":"6601.29","quantity":"8.1367","stepValue":null},{"price":"6594.62","quantity":"11.8820","stepValue":null},{"price":"6587.94","quantity":"14.1855","stepValue":null},{"price":"6581.27","quantity":"16.6696","stepValue":null},{"price":"6574.59","quantity":"17.3605","stepValue":null},{"price":"6567.92","quantity":"19.0313","stepValue":null},{"price":"6561.24","quantity":"19.9267","stepValue":null},{"price":"6554.57","quantity":"20.2487","stepValue":null},{"price":"6547.90","quantity":"21.0806","stepValue":null},{"price":"6541.22","quantity":"21.6186","stepValue":null},{"price":"6534.55","quantity":"21.6903","stepValue":null},{"price":"6527.87","quantity":"21.6903","stepValue":null},{"price":"6521.20","quantity":"21.7744","stepValue":null},{"price":"6514.52","quantity":"21.7744","stepValue":null},{"price":"6507.85","quantity":"21.8855","stepValue":null},{"price":"6501.17","quantity":"22.0919","stepValue":null},{"price":"6494.50","quantity":"27.4866","stepValue":null},{"price":"6487.82","quantity":"33.9069","stepValue":null},{"price":"6481.15","quantity":"41.4284","stepValue":null},{"price":"6474.47","quantity":"47.8954","stepValue":null},{"price":"6467.80","quantity":"53.0933","stepValue":null},{"price":"6461.12","quantity":"64.5647","stepValue":null},{"price":"6454.45","quantity":"74.4489","stepValue":null},{"price":"6447.77","quantity":"87.5970","stepValue":null},{"price":"6441.10","quantity":"98.2651","stepValue":null},{"price":"6434.43","quantity":"107.6162","stepValue":null},{"price":"6427.75","quantity":"116.6642","stepValue":null},{"price":"6421.08","quantity":"125.1862","stepValue":null},{"price":"6414.40","quantity":"135.8994","stepValue":null},{"price":"6407.73","quantity":"140.7876","stepValue":null},{"price":"6401.05","quantity":"148.7841","stepValue":null},{"price":"6394.38","quantity":"155.9597","stepValue":null},{"price":"6387.70","quantity":"161.8693","stepValue":null},{"price":"6381.03","quantity":"165.4503","stepValue":null},{"price":"6374.35","quantity":"169.8934","stepValue":null},{"price":"6367.68","quantity":"173.5712","stepValue":null},{"price":"6361.00","quantity":"176.1769","stepValue":null},{"price":"6354.33","quantity":"179.1493","stepValue":null},{"price":"6347.65","quantity":"181.3822","stepValue":null}],"depthPercent":"0.01","asksMinPrice":"6674.89","bidsMaxPrice":"6674.56"}
```

### Function Definition

Connect to current market depth data for a specific currency (latest 50).

### Topic:

`$env/api/ws/v1/market/depth/$topicSymbol`

### Push frequency:

`1 per Second`

### Topic Parameters

| Name       | Mandatory | Description    |
| ---------- | ---- | ------------------- |
| topicSymbol| Yes  |Currency Pair        |

### Response Parameters

| Name         | Description                              |
| ------------ | ---------------------------------------- |
| symbol       | Currency Pair                            |
| depthPercent | Depth Percentage                         |
| asksMinPrice | Minimum Ask Price                        |
| bidsMaxPrice | Maximum Bid Price                        |
| asks         | Ask Data (JSON Array)                    |
| bids         | Bid Data (JSON Array)                    |
| price        | Price                                    |
| quantity     | Quantity                                 |
| stepValue    | This field will be removed. Please ignore it. |


## Latest Order Book Data

> Topic Sample:

```
exchange_ha_prod_hk/api/ws/v1/market/orderBook/BTC_USD/0.01
```

> Data Sample:

```json
{"asks":[{"amount":"349.76","limitPrice":"6674.89","quantity":"0.0524"},{"amount":"1601.32","limitPrice":"6674.96","quantity":"0.2399"},{"amount":"1744.18","limitPrice":"6675.02","quantity":"0.2613"},{"amount":"3495.73","limitPrice":"6675.08","quantity":"0.5237"},{"amount":"1987.85","limitPrice":"6675.13","quantity":"0.2978"},{"amount":"1614.04","limitPrice":"6675.14","quantity":"0.2418"},{"amount":"556.03","limitPrice":"6675.15","quantity":"0.0833"},{"amount":"3542.57","limitPrice":"6675.29","quantity":"0.5307"},{"amount":"1948.53","limitPrice":"6675.34","quantity":"0.2919"},{"amount":"1073.40","limitPrice":"6675.39","quantity":"0.1608"},{"amount":"1230.94","limitPrice":"6675.40","quantity":"0.1844"},{"amount":"1293.70","limitPrice":"6675.47","quantity":"0.1938"},{"amount":"1696.24","limitPrice":"6675.50","quantity":"0.2541"},{"amount":"1169.55","limitPrice":"6675.52","quantity":"0.1752"},{"amount":"658.87","limitPrice":"6675.55","quantity":"0.0987"}],"bids":[{"amount":"5666.70","limitPrice":"6674.56","quantity":"0.8490"},{"amount":"817.97","limitPrice":"6617.89","quantity":"0.1236"},{"amount":"336.18","limitPrice":"6617.84","quantity":"0.0508"},{"amount":"1715.64","limitPrice":"6616.43","quantity":"0.2593"},{"amount":"1926.69","limitPrice":"6616.40","quantity":"0.2912"},{"amount":"1663.90","limitPrice":"6615.92","quantity":"0.2515"},{"amount":"1404.30","limitPrice":"6614.73","quantity":"0.2123"},{"amount":"448.47","limitPrice":"6614.69","quantity":"0.0678"},{"amount":"1738.04","limitPrice":"6613.57","quantity":"0.2628"},{"amount":"945.71","limitPrice":"6613.39","quantity":"0.1430"},{"amount":"1605.04","limitPrice":"6613.28","quantity":"0.2427"},{"amount":"942.30","limitPrice":"6612.69","quantity":"0.1425"},{"amount":"1768.89","limitPrice":"6612.68","quantity":"0.2675"},{"amount":"840.35","limitPrice":"6611.76","quantity":"0.1271"},{"amount":"1350.11","limitPrice":"6611.72","quantity":"0.2042"}],"limitSize":15,"step":"0.01","symbol":"BTC/USD","timestamp":1532686075803}
```
### Function Definition:

Connect to the order book data stream. (Shows last 15 orders)

### Topic

`$env/api/ws/v1/market/orderBook/$topicSymbol/$step`

### Push Frequency

`1 per 1-2 Seconds`

### Topic Parameters

| Name        | Mandatory | Description                              |
| ----------- | --------- | ---------------------------------------- |
| topicSymbol | Yes       | Currency Pair                            |
| step        | Yes       | Aggregation Depth(For fiat, this value should be: 1.0，0.1，0.01；For crypto, this value should be:0.0001，0.000001，0.00000001) |


###  Data Parameters

| Name       | Description                              |
| ---------- | ---------------------------------------- |
| symbol     | Currency Pair                            |
| step       | Depth                                    |
| timestamp  | Timestamp (Milliseconds)                 |
| limitSize  | Maximum Push Orders                      |
| asks       | Ask Data (JSON Array)                    |
| bids       | Bid Data (JSON Array)                    |
| limitPrice | Price Limit. This number is accurate to 8 decimal points. For fiat, this is accurate to 2 decimal points. |
| quantity   | Quantity. This number is accurate to 4 decimal points |
| amount     | This field will be removed. Please ignore it. |


## Latest Kline Data

> Topic Sample:

```
exchange_ha_prod_hk/api/ws/v1/market/kline/BTC_USD/60000
```

> Data Sample:

```json
{"symbol":"BTC/USD","first":"6616.40","last":"6616.45","max":"6616.48","min":"6616.40","quantity":"31.76239814","timestamp":1532686140000}
```
### Function Definition:

Contains between 1 minute to 1 month of Kline data, returned as a single data point per push.

### Topic

`$env/api/ws/v1/market/orderBook/$topicSymbol/$step`

### Push Frequency

`1 per 1-2 Seconds`

### Topic Parameters

| Name        | Mandatory | Description                              |
| ----------- | --------- | ---------------------------------------- |
| topicSymbol | Yes       | Currency Pair                            |
| range       | Yes       | Kline Duration (1 Minute:60000，5 Minutes:300000，15 Minutes:900000，30 Minutes:1800000，1 Hour:3600000，2 Hours:7200000，4 Hours:14400000，6 Hours:21600000，12 Hours:43200000，1 Day:86400000，1 Month:604800000) |

###  Data Parameters

| Name      | Description              |
| --------- | ------------------------ |
| symbol    | Currency Pair            |
| range     | Kline Duration           |
| timestamp | Timestamp (Milliseconds) |
| quantity  | Trade Volume             |
| max       | Highest Price            |
| min       | Lowest Price             |
| first     | Opening Price            |
| last      | Closing Price            |


## Real Time Transaction Data

> Topic Sample:

```
exchange_ha_prod_hk/api/ws/v1/market/tickers/BTC_USD
```

> Data Sample:

```json
[{"symbol":"BTC/USD","price":"6089.34","quantity":"0.4337","action":"BUY","timestamp":1532623321348},{"symbol":"BTC/USD","price":"6088.84","quantity":"0.0010","action":"BUY","timestamp":1532607467417},{"symbol":"BTC/USD","price":"6088.84","quantity":"0.2197","action":"SELL","timestamp":1532606712073},{"symbol":"BTC/USD","price":"6311.06","quantity":"0.1760","action":"BUY","timestamp":1532590562930},{"symbol":"BTC/USD","price":"6311.06","quantity":"0.0010","action":"SELL","timestamp":1532590547420},{"symbol":"BTC/USD","price":"6311.50","quantity":"0.8951","action":"SELL","timestamp":1532587958810},{"symbol":"BTC/USD","price":"6314.70","quantity":"0.0158","action":"SELL","timestamp":1532398791628},{"symbol":"BTC/USD","price":"6674.89","quantity":"0.0008","action":"SELL","timestamp":1532090050033},{"symbol":"BTC/USD","price":"6674.89","quantity":"0.0801","action":"SELL","timestamp":1531987659236},{"symbol":"BTC/USD","price":"6674.87","quantity":"0.2954","action":"SELL","timestamp":1531987655248},{"symbol":"BTC/USD","price":"6674.85","quantity":"0.1632","action":"SELL","timestamp":1531987653252},{"symbol":"BTC/USD","price":"6674.83","quantity":"0.1153","action":"SELL","timestamp":1531987651953},{"symbol":"BTC/USD","price":"6674.80","quantity":"0.2112","action":"SELL","timestamp":1531987651001},{"symbol":"BTC/USD","price":"6674.74","quantity":"0.2580","action":"SELL","timestamp":1531987646413},{"symbol":"BTC/USD","price":"6674.72","quantity":"0.1417","action":"SELL","timestamp":1531987643644},{"symbol":"BTC/USD","price":"6674.67","quantity":"0.2332","action":"SELL","timestamp":1531987642840},{"symbol":"BTC/USD","price":"6674.61","quantity":"0.1909","action":"BUY","timestamp":1531987599048},{"symbol":"BTC/USD","price":"6674.56","quantity":"0.1510","action":"BUY","timestamp":1531987571826},{"symbol":"BTC/USD","price":"6674.56","quantity":"0.0946","action":"BUY","timestamp":1531118165770},{"symbol":"BTC/USD","price":"6674.49","quantity":"0.1612","action":"BUY","timestamp":1531118165685}]
```
### Function Definition:

Request the latest transaction data (up to 50 entries)

### Topic

`$env/api/ws/v1/market/tickers/$topicSymbol`

### Push Frequency

`1 per 1-2 Seconds`

### Topic Parameters

| Name        | Mandatory | Description   |
| ----------- | --------- | ------------- |
| topicSymbol | Yes       | Currency Pair |

###  Data Parameters

| Name      | Description                             |
| --------- | --------------------------------------- |
| symbol    | Currency Pair                           |
| timestamp | Timestamp (Milliseconds)                |
| quantity  | Quantity                                |
| price     | Price                                   |
| action    | Transaction Direction BUY-Buy,SELL-Sell |


# Private Data

## Change of Asset in User Account

> Topic Sample:

```
exchange_ha_prod_hk/api/ws/personal/v1/asset/change/demouser@demo.domain
```

> Data Sample:

```json
{"asset":[{"available":0.738763122563248000000000000000,"currency":"BTC","total":1.306763122563248000000000000000,"utcUpdate":1542786801575},{"available":3226.058257880000000000000000000000,"currency":"USD","total":3226.058257880000000000000000000000,"utcUpdate":1542786801518}],"email":"demouser@demo.domain","timestamp":1542786802283,"userNo":"1600000000000000000"}
```
### Function Definition:

Connect to asset data after change of account info. In order to ensure the security of funds information, the asset change push interface needs to be approved after the official API technology group application is approved. (Please note: omission or duplication of "Push" data may occur under this topic)

### Topic

`$env/api/ws/personal/v1/asset/change/$email`

### Push Frequency

`1 per 1-2 Seconds`

### Topic Parameters

| Name  | Mandatory | Description                |
| ----- | --------- | -------------------------- |
| email | Yes       | User Account Email Address |

###  Data Parameters

| Name      | Description                |
| --------- | -------------------------- |
| userNo    | User Number ID             |
| email     | User Account Email Address |
| total     | Total Balance              |
| available | Available Balance          |
| currency  | Asset Type                 |
| utcUpdate | Time Of Update             |
| timestamp | Time Of Query              |


## Change of Order Status

> Topic Sample:

```
exchange_ha_prod_hk/ws/personal/v1/order/change/demouser@demo.domain
```

> Data Sample:

```json
{"asset":[{"available":0.738763122563248000000000000000,"currency":"BTC","total":1.306763122563248000000000000000,"utcUpdate":1542786801575},{"available":3226.058257880000000000000000000000,"currency":"USD","total":3226.058257880000000000000000000000,"utcUpdate":1542786801518}],"email":"demouser@demo.domain","timestamp":1542786802283,"userNo":"1600000000000000000"}
```
### Function Definition:

Connect to the data of order status changes (currently only connectable to pushes of order creation and order withdrawal. Please note: omission or duplication of "Push" data may occur under this topic)

### Topic

`$env/ws/personal/v1/order/change/$email`

### Push Frequency

`1 per 1-2 Seconds`

### Topic Parameters

| Name  | Mandatory | Description                |
| ----- | --------- | -------------------------- |
| email | Yes       | User Account Email Address |

###  Data Parameters

| Name              | Description                              |
| ----------------- | ---------------------------------------- |
| symbol            | Transaction Pair                         |
| orderNo           | Transaction Pair                         |
| quantity          | Order Quantity                           |
| quantityRemaining | Remaining Quantity                       |
| priceLimit        | Order Price                              |
| action            | Order Type SELL-Sell, BUY-Buy            |
| priceAverage      | Average Transaction Price                |
| amount            | Order Amount                             |
| amountRemaining   | Remaining Amount                         |
| fee               | Transaction Fee                          |
| state             | Order Status NEW: CreatedCANCEL: Cancelled |
| utcCreate         | Time Of Create                           |
| utcUpdate         | Time Of Update                           |


## Change of Order Transaction

> Topic Sample:

```
exchange_ha_prod_hk/ws/personal/v1/trade/change/demouser@demo.domain
```

> Data Sample:

```json
{"action":"BUY","dealNo":"1616392748072460289","matchType":"TAKER","orderNo":"1616392747273428993","orderType":"MKT","price":"1234.56000000","quantity":"0.00081000","symbol":"ETC/BTC","utcDeal":"1541512249094"}
```
### Function Definition:

Connect to transaction data(Please note: omission or duplication of "Push" data may occur under this topic)

### Topic

`$env/ws/personal/v1/order/change/$email`

### Push Frequency

`1 per 1-2 Seconds`

### Topic Parameters


| Name  | Mandatory | Description                |
| ----- | --------- | -------------------------- |
| email | Yes       | User Account Email Address |

###  Data Parameters

| Name      | Description                              |
| --------- | ---------------------------------------- |
| dealNo    | Transaction Number ID                    |
| orderNo   | Order Number ID                          |
| orderType | Order Sub-type  MKT-Market Price,LMT-Limit Price |
| symbol    | Transaction Pair                         |
| matchType | Transaction Type: MAKER-Passive Match，TAKER-Active Match |
| utcDeal   | Transaction Timestamp                    |
| quantity  | Transaction Volume                       |
| price     | Transaction Price                        |
| action    | Order Type SELL-Sell, BUY-Buy            |
