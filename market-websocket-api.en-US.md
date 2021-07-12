# Websocket Market push
The baseurl of all wss interfaces listed in this article is: wss://ws.bitrue.com/kline-api/ws
### Supported data
Currently, four types of data are provided: k-line, transaction, depth, and ticker.

### Data compression
The client sends messages to subscribe to the websocket without compression.
The websocket message received by the client is compressed by gzip, and the client needs to decompress it.

### Heartbeat
Every 3 minutes, the server will send a ping frame, and the client should reply with a pong frame within 10 minutes.
Otherwise, the server will actively disconnect the link. The client is allowed to send unpaired pong frames (that is, the client can send pong frames at a frequency higher than 10 minutes to maintain the connection).

### Update frequency
Update speed is real-time update

## Event
There are four types of events: heartbeat, request, subscription, and unsubscription.

### Heartbeat event

Heartbeat sent by the server to the client
``` json
{
  "ping":1621418174969
}
```
The heartbeat sent by the client to the server
``` json
{
  "pong":1621418174969
}
```

### Request event
The event of the requested data is req. After successfully requesting the data, you will receive the data structure set
For example, request transaction information on adausdt

``` json
{"event":"req","params":{"cb_id":"adausdt","channel":"market_adausdt_trade_ticker","top":20}}
```
The response data received is
``` json
{"event_rep":"rep","cb_id":"adausdt","status":"ok","top":20,"ds":"2021-05-19 17:53:00","ts":1621417980160,"channel":"market_adausdt_trade_ticker","data":[{"id":28187162,"price":1.743900,"amount":472.07373000000000000000000000000000,"side":"BUY","vol":270.70,"ts":1621412844000,"ds":"2021-05-19 16:27:24"},{"id":28187161,"price":1.742800,"amount":4.93212400000000000000000000000000,"side":"SELL","vol":2.83,"ts":1621412843000,"ds":"2021-05-19 16:27:23"},{"id":28187160,"price":1.742600,"amount":18.55869000000000000000000000000000,"side":"BUY","vol":10.65,"ts":1621412843000,"ds":"2021-05-19 16:27:23"},{"id":28187159,"price":1.743200,"amount":28.71050400000000000000000000000000,"side":"BUY","vol":16.47,"ts":1621412842000,"ds":"2021-05-19 16:27:22"},{"id":28187158,"price":1.744500,"amount":28.01667000000000000000000000000000,"side":"SELL","vol":16.06,"ts":1621412842000,"ds":"2021-05-19 16:27:22"},{"id":28187157,"price":1.744200,"amount":57.48883200000000000000000000000000,"side":"SELL","vol":32.96,"ts":1621412841000,"ds":"2021-05-19 16:27:21"},{"id":28187156,"price":1.742900,"amount":1.67318400000000000000000000000000,"side":"BUY","vol":0.96,"ts":1621412841000,"ds":"2021-05-19 16:27:21"},{"id":28187155,"price":1.743000,"amount":186.18726000000000000000000000000000,"side":"SELL","vol":106.82,"ts":1621412840000,"ds":"2021-05-19 16:27:20"},{"id":28187154,"price":1.743200,"amount":120.61200800000000000000000000000000,"side":"BUY","vol":69.19,"ts":1621412840000,"ds":"2021-05-19 16:27:20"},{"id":28187153,"price":1.743900,"amount":0.45341400000000000000000000000000,"side":"SELL","vol":0.26,"ts":1621412839000,"ds":"2021-05-19 16:27:19"},{"id":28187152,"price":1.743400,"amount":16.87611200000000000000000000000000,"side":"SELL","vol":9.68,"ts":1621412839000,"ds":"2021-05-19 16:27:19"},{"id":28187151,"price":1.743300,"amount":1.48180500000000000000000000000000,"side":"SELL","vol":0.85,"ts":1621412839000,"ds":"2021-05-19 16:27:19"},{"id":28187150,"price":1.743000,"amount":903.65835000000000000000000000000000,"side":"BUY","vol":518.45,"ts":1621412838000,"ds":"2021-05-19 16:27:18"},{"id":28187149,"price":1.743600,"amount":15.81445200000000000000000000000000,"side":"BUY","vol":9.07,"ts":1621412838000,"ds":"2021-05-19 16:27:18"},{"id":28187148,"price":1.743600,"amount":76.63122000000000000000000000000000,"side":"BUY","vol":43.95,"ts":1621412838000,"ds":"2021-05-19 16:27:18"},{"id":28187147,"price":1.743600,"amount":58.98598800000000000000000000000000,"side":"BUY","vol":33.83,"ts":1621412838000,"ds":"2021-05-19 16:27:18"},{"id":28187146,"price":1.743700,"amount":21.81368700000000000000000000000000,"side":"BUY","vol":12.51,"ts":1621412838000,"ds":"2021-05-19 16:27:18"},{"id":28187145,"price":1.743600,"amount":524.94565200000000000000000000000000,"side":"BUY","vol":301.07,"ts":1621412838000,"ds":"2021-05-19 16:27:18"},{"id":28187144,"price":1.743700,"amount":6.08551300000000000000000000000000,"side":"BUY","vol":3.49,"ts":1621412838000,"ds":"2021-05-19 16:27:18"},{"id":28187143,"price":1.743700,"amount":9.31135800000000000000000000000000,"side":"BUY","vol":5.34,"ts":1621412838000,"ds":"2021-05-19 16:27:18"}]}

```
### Subscribe to events
The event of the subscription data is sub. After successful subscription
For example, subscribe to the transaction information of the adausdt currency pair

``` json
{"event":"sub","params":{"cb_id":"adausdt","channel":"market_adausdt_trade_ticker","top":20}}
```
After the subscription is successful, you will receive a response message that the subscription is successful

``` json
{
  "channel":"market_adausdt_trade_ticker",
  "cb_id":"adausdt",
  "event_rep":"subed",
  "status":"ok",
  "ts":1621418172662
}
```
After that, you will receive the response data of the user's specific subscription

``` json
{
  "channel":"market_adausdt_trade_ticker",
  "cb_id":"adausdt",
  "event_rep":"subed",
  "status":"ok",
  "ts":1621418172662
}
```


### Unsubscribe event
  The event to unsubscribe data is unsub. example:
 ``` json
{
    "event":"unsub",  #Cancel event
    "params":{
        "cb_id":"adausdt",  #Coin pair
        "channel":"market_adausdt_trade_ticker" #Canceled channel
    }
}
```

 There is no response message after cancellation.
 

# K line

The channel format of k-line is：market_%s_kline_%S
The placeholder %s is the name of the currency pair, such as adausdt, and %S is the accuracy supported by the bar, such as 1min.
Currently supported currency pairs can be obtained through the interface https://www.bitrue.com/kline-api/coin/symbolForShow.
The currently supported precisions are as follows:
1min
5min
15min
30min
1h
2h
4h
6h
8h
12h
1d
1w


For example: the channel of the k-line with 1min accuracy of adausdt is market_adausdt_kline_1min.

### Request historical bar
By default, historical bars return information about 1440 bars, and earlier historical bars currently do not support querying.

``` json
{
    "event":"req", #Event type: request
    "params":{
        "channel":"market_adausdt_kline_1min", #channel Represents the 1-minute k-line of the adausdt currency pair
        "cb_id":"adausdt"  #Currency pair name adausdt
    }
}

```
Response data
Part of the data is omitted from the sample response data
``` json
{
  "data":[
    {
      "close":2.0968, #Closing price
      "amount":28899.212766, #Transaction amount
      "high":2.0996, #Highest price
      "id":1621326480, k-line time seconds
      "low":2.096, # Lowest price
      "open":2.0989, #Starting price
      "vol":13775.04 #The number of transactions
    },
    {
      "close":2.0991,
      "amount":15431.118281,
      "high":2.1006,
      "id":1621326540,
      "low":2.0957,
      "open":2.0967,
      "vol":7358.66
    }
 ],
  "ts":1621418992094,
  "event_rep":"rep",
  "cb_id":"adausdt",
  "status":"ok",   #Status marked ok for success
  "channel":"market_adausdt_kline_1min"
}
```

### Subscribe to k-line

``` json
{
    "event":"sub", #Event type: subscription
    "params":{
        "channel":"market_adausdt_kline_1min", #channel Represents the 1-minute k-line of the adausdt currency pair
        "cb_id":"adausdt"   #Currency pair name adausdt
    }
}
```
  Response data
  Successfully subscribed
 ``` json
{
  "channel":"market_adausdt_kline_1min",
  "cb_id":"adausdt",
  "event_rep":"subed",
  "status":"ok",
  "ts":1621419881168
}
 
 ```
 k-line data message
  ``` json
{
  "channel":"market_adausdt_kline_1min",
  "ts":1621413503504,
  "tick":{
    "amount":49855.950209,
    "close":1.7439,
    "vol":28616.74,
    "high":1.7445,
    "low":1.7406,
    "open":1.7423,
    "id":1621412820
  }
}

  ```

### Unsubscribe k-line

  ``` json
{
  "event":"unsub",
  "params":{
    "channel":"market_adausdt_kline_1min",
    "cb_id":"adausdt"
  }
}
```
  
# Deal
### Request transaction information

``` json
{"event":"req","params":{"cb_id":"adausdt","channel":"market_adausdt_trade_ticker","top":20}}
```
The response data received is
``` json
{"event_rep":"rep","cb_id":"adausdt","status":"ok","top":20,"ds":"2021-05-19 17:53:00","ts":1621417980160,"channel":"market_adausdt_trade_ticker","data":[{"id":28187162,"price":1.743900,"amount":472.07373000000000000000000000000000,"side":"BUY","vol":270.70,"ts":1621412844000,"ds":"2021-05-19 16:27:24"},{"id":28187161,"price":1.742800,"amount":4.93212400000000000000000000000000,"side":"SELL","vol":2.83,"ts":1621412843000,"ds":"2021-05-19 16:27:23"},{"id":28187160,"price":1.742600,"amount":18.55869000000000000000000000000000,"side":"BUY","vol":10.65,"ts":1621412843000,"ds":"2021-05-19 16:27:23"},{"id":28187159,"price":1.743200,"amount":28.71050400000000000000000000000000,"side":"BUY","vol":16.47,"ts":1621412842000,"ds":"2021-05-19 16:27:22"},{"id":28187158,"price":1.744500,"amount":28.01667000000000000000000000000000,"side":"SELL","vol":16.06,"ts":1621412842000,"ds":"2021-05-19 16:27:22"},{"id":28187157,"price":1.744200,"amount":57.48883200000000000000000000000000,"side":"SELL","vol":32.96,"ts":1621412841000,"ds":"2021-05-19 16:27:21"},{"id":28187156,"price":1.742900,"amount":1.67318400000000000000000000000000,"side":"BUY","vol":0.96,"ts":1621412841000,"ds":"2021-05-19 16:27:21"},{"id":28187155,"price":1.743000,"amount":186.18726000000000000000000000000000,"side":"SELL","vol":106.82,"ts":1621412840000,"ds":"2021-05-19 16:27:20"},{"id":28187154,"price":1.743200,"amount":120.61200800000000000000000000000000,"side":"BUY","vol":69.19,"ts":1621412840000,"ds":"2021-05-19 16:27:20"},{"id":28187153,"price":1.743900,"amount":0.45341400000000000000000000000000,"side":"SELL","vol":0.26,"ts":1621412839000,"ds":"2021-05-19 16:27:19"},{"id":28187152,"price":1.743400,"amount":16.87611200000000000000000000000000,"side":"SELL","vol":9.68,"ts":1621412839000,"ds":"2021-05-19 16:27:19"},{"id":28187151,"price":1.743300,"amount":1.48180500000000000000000000000000,"side":"SELL","vol":0.85,"ts":1621412839000,"ds":"2021-05-19 16:27:19"},{"id":28187150,"price":1.743000,"amount":903.65835000000000000000000000000000,"side":"BUY","vol":518.45,"ts":1621412838000,"ds":"2021-05-19 16:27:18"},{"id":28187149,"price":1.743600,"amount":15.81445200000000000000000000000000,"side":"BUY","vol":9.07,"ts":1621412838000,"ds":"2021-05-19 16:27:18"},{"id":28187148,"price":1.743600,"amount":76.63122000000000000000000000000000,"side":"BUY","vol":43.95,"ts":1621412838000,"ds":"2021-05-19 16:27:18"},{"id":28187147,"price":1.743600,"amount":58.98598800000000000000000000000000,"side":"BUY","vol":33.83,"ts":1621412838000,"ds":"2021-05-19 16:27:18"},{"id":28187146,"price":1.743700,"amount":21.81368700000000000000000000000000,"side":"BUY","vol":12.51,"ts":1621412838000,"ds":"2021-05-19 16:27:18"},{"id":28187145,"price":1.743600,"amount":524.94565200000000000000000000000000,"side":"BUY","vol":301.07,"ts":1621412838000,"ds":"2021-05-19 16:27:18"},{"id":28187144,"price":1.743700,"amount":6.08551300000000000000000000000000,"side":"BUY","vol":3.49,"ts":1621412838000,"ds":"2021-05-19 16:27:18"},{"id":28187143,"price":1.743700,"amount":9.31135800000000000000000000000000,"side":"BUY","vol":5.34,"ts":1621412838000,"ds":"2021-05-19 16:27:18"}]}

```
Data message received
``` json
{
  "event_rep":"rep",
  "cb_id":"adausdt",
  "status":"ok",
  "top":20,
  "ds":"2021-05-19 19:32:23",
  "ts":1621423943916,
  "channel":"market_adausdt_trade_ticker", 
  "data":[
    {
      "id":28187162,  #Transaction id
      "price":1.7439, #price
      "amount":472.07373, #Transaction amount
      "side":"BUY",  #direction
      "vol":270.7, #amount
      "ts":1621412844000, #Timestamp of the transaction
      "ds":"2021-05-19 16:27:24" #Utc+8 time of the transaction
    },
    {
      "id":28187161,
      "price":1.7428,
      "amount":4.932124,
      "side":"SELL",
      "vol":2.83,
      "ts":1621412843000,
      "ds":"2021-05-19 16:27:23"
    }
  ]
}
```

### Subscribe to transaction information
``` json
{
  "event":"sub",
  "params":{
    "cb_id":"adabtc",
    "channel":"market_adabtc_trade_ticker"
  }
}
```

After the subscription is successful, you will receive a response message that the subscription is successful

``` json
{
  "channel":"market_adausdt_trade_ticker",
  "cb_id":"adausdt",
  "event_rep":"subed",
  "status":"ok",
  "ts":1621418172662
}
```
After that, you will receive the response data of the user's specific subscription

``` json
{
  "channel":"market_ltcbtc_trade_ticker",
  "tick":{
    "data":[
      {
        "id":20482764,
        "price":0.006277,
        "amount":0.00520991,
        "side":"BUY",
        "vol":0.83,
        "ts":1621426164263,
        "ds":"2021-05-19 20:09:24"
      },
      {
        "id":20482763,
        "price":0.006278,
        "amount":0.00018834,
        "side":"SELL",
        "vol":0.03,
        "ts":1621426160884,
        "ds":"2021-05-19 20:09:20"
      }
    ]
  },
  "ts":1621426164471
}
``` 

### Unsubscribe transaction information
``` json
{"event":"unsub","params":{"cb_id":"adausdt","channel":"market_adausdt_trade_ticker","top":20}}
```


# Complete Ticker by Symbol

### Subscribe to ticker
``` json
{"event":"sub","params":{"channel":"market_xlmeth_ticker","cb_id":"xlmeth"}}

```

Subscription successful response

``` json
{
  "channel":"market_xlmeth_ticker",
  "cb_id":"xlmeth",
  "event_rep":"subed",
  "status":"ok",
  "ts":1621422413684
}
```
Data response
``` json
{
  "tick":{
    "amount":0,  #Transaction amount
    "rose":0,  #24-hour rise and fall
    "close":0.000309,  #Closing price
    "vol":0,  #amount
    "high":0.000309, #Highest price
    "low":0.000309, #Lowest price 
    "open":0.000309 #Opening price
  },
  "channel":"market_xlmeth_ticker", #channelName 
  "ts":1621422413684  #Timestamp
}
```


### Unsubscribe tiker

``` json
{
  "event":"unsub",
  "params":{
    "channel":"market_xlmeth_ticker",
    "cb_id":"xlmeth"
  }
}
```

# Limited file depth information
The depth information provided by default is 40 files of depth information.
Provides three kinds of precision depth information.

### channel format market_%s_depth_step%d
Among them, %s represents the currency pair information, %d represents the accuracy type, and the accuracy has three values: 0, 1, 2 respectively.
The corresponding accuracy is the accuracy in the interface to obtain supported currency pair information.
For example, the response obtained by the currency pair adausdt through the supported currency pair information interface is

``` json
{
    "name":"ADA/USDT", #Coin pair
    "key":"adausdt",  #Coin pair
    "scale":{
        "price":6, #Accuracy of price display
        "volume":2 #Accuracy of volume display
    },
    "depth":[. #Accuracy information
         5, #The first precision, the price is accurate to 5 decimal places
         4, #The second level of precision, the price is accurate to 4 decimal places
         3 #The third level of precision, the price is accurate to 3 decimal places
    ]
}
```
Then
The depth information that the user requests for the first level of precision (accurate to the decimal point) is that the channel is market_adausdt_depth_step0.
The depth information requested by the user for the second level of precision is that the channel is market_adausdt_depth_step1.
The depth information requested by the user for the third level of precision is that the channel is market_adausdt_depth_step2.

### Subscribe to in-depth information

``` json
{"event":"sub","params":{"cb_id":"adausdt","channel":"market_adausdt_depth_step0"}}

```
Response first

``` json
{
  "channel":"market_adausdt_depth_step0", #Subscribed channel
  "cb_id":"adausdt",  #The name of the subscribed currency pair
  "event_rep":"subed", #Successfully subscribed
  "status":"ok",  #Subscribed successfully
  "ts":1621419669423  #Timestamp
}
```

After receiving data message
``` json
{
  "channel":"market_adausdt_depth_step0",
  "ts":1621426497365,
  "tick":{
    "buys":[
      [
        "0.15579",
        110
      ],
      [
        "0.12579",
        100
      ],
      [
        "0.05260",
        924402.01
      ]
    ],
    "asks":[
    ]
  }
}
```


### Unsubscribe from in-depth information
``` json
{"event":"unsub","params":{"cb_id":"adausdt","channel":"market_adausdt_depth_step0"}}

```

### Request in-depth information

``` json
{"event":"req","params":{"cb_id":"adausdt","channel":"market_adausdt_depth_step0"}}

```

response
``` json
{
  "ts":1621419599417,
  "tick":{
    "buys":[  #Pay the bill
    ],
    "asks":[  #Sell orders
      [
        "40199.75",  #price
        0.1  #amount
      ]
    ]
  }
}
```

## Get information about supported currency pairs
**Request URL：**
- `https://www.bitrue.com/kline-api/coin/symbolForShow`

**Request method：**
- GET

**parameter：**
Null

**Return example**

A total of four denominated currencies are supported

```json
{
    "btc":[           #btc The collection of currency pairs in the market
        {
            "name":"BTR/BTC",
            "key":"btrbtc",
            "scale":{
                "price":9,
                "volume":1
            },
            "depth":[
                9,
                8,
                7
            ]
        },
        {
            "name":"BTR/BTC",
            "key":"btrbtc", #Coin pair
            "scale":{
                "price":9, #Accuracy of price display
                "volume":1 #Accuracy of volume display
            },
            "depth":[ #Price accuracy of in-depth information
                 9, #The first precision, the price is accurate to 9 decimal places
                 8, #The second level of precision, the price is accurate to 8 decimal places
                 7 #The third level of precision, the price is accurate to 7 decimal places
            ]
        }
    ],
    "usdt":[ #usdt The set of currency pairs in the market (data has been omitted)

    ],
    "xrp":[ #xrp The set of currency pairs in the market (data has been omitted)

    ],
    "eth":[ #eth The set of currency pairs in the market (data has been omitted)

    ]
}
```



