# Order Endpoints

| Endpoint | Description |
| ---- | ---- |
| GET /accounts/:account_id/orders | Get a list of open orders |
| POST /accounts/:account_id/orders | Create a open Order |
| GET /accounts/:account_id/orders/:Order_id | Get information of an open Order |
| PUT /accounts/:account_id/orders/:Order_id | Modify an open Order |
| DELETE /accounts/:account_id/orders/:Order_id | Close an open Order |


## GET /accounts/:account_id/orders

#### Request
    https://api.oanda.com/v1/accounts/12345/orders?instrument=EUR/USD&maxCount=4

#### Respond
    {
      "orders" : [
          { "id" : 12345, "type": "entry", "direction" : "long", "instrument" : "EUR/USD", "units" : 100, "time" : 1234567891, "price" : 1.5, "stopLoss" : 1.2, "takeProfit" : 1.7, "expiry" : 1234567890, "highLimit" : 2.0, "lowLimit" : 1.0, "trailingStop" : 10, "ocaGroupId" : 0},
          { "id" : 12344, "type": "entry", "direction" : "short", "instrument" : "EUR/USD", "units" : 100, "time" : 1234567890, "price" : 1.5, "stopLoss" : 1.2, "takeProfit" : 1.7, "expiry" : 1234567890, "highLimit" : 2.0, "lowLimit" : 1.0, "trailingStop" : 10, "ocaGroupId" : 1},
          { "id" : 890, "type": "limit", "direction" : "short", "instrument" : "EUR/USD", "units" : 100, "time" : 1234567890, "price" : 1.5, "stopLoss" : 1.2, "takeProfit" : 1.7, "expiry" : 1234567890, "highLimit" : 2.0, "lowLimit" : 1.0, "trailingStop" : 10, "ocaGroupId" : 1},
          { "id" : 789, "type": "stop", "direction" : "short", "instrument" : "EUR/USD", "units" : 100, "time" : 1234567890, "price" : 1.5, "stopLoss" : 1.2, "takeProfit" : 1.7, "expiry" : 1234567890, "highLimit" : 2.0, "lowLimit" : 1.0, "trailingStop" : 10, "ocaGroupId" : 1}
      ],
      "nextPage" : "https:\/\/api.oanda.com\/accounts\/12345\/orders?maxCount=4&maxOrderId=788"
    }

#### Parameters
| Name | Description |
| ---- | ----------- |
| maxOrderId | The server will return orders with id less than or equal to this, in descending order (about pagination). |
| maxCount   | Maximum number of open orders to return. Default: 50 Max value: 500 |
| instrument | Restrict open Order for a specific instrument. Default: all |
| orderIds   | A common separated list of orders to retrieve. |

#### Required scope
read

## POST /accounts/:account_id/orders
#### Request
    curl -X POST -d 'instrument=EUR/USD&units=2&direction=short&price=1.2&expiry=1352939000' https://api.oanda.com/v1/accounts/12345/orders

#### Respond
    {
        "id" : 268167142,
        "instrument" : "EUR\/USD",
        "price" : 1.2,
        "units" : 2,
        "direction" : "short",
        "ocaGroupId" : 0
    }


#### Parameters
| Name | Description |
| ---- | ----------- |
| instrument | __required__ Instrument to open Order on |
| units | __required__ Number of units to open Order for |
| type | entry (default), or limit (More about order types) |
| direction | long (default) or short |
| price | __required__ The price at which the order will trigger at (TODO: re-word so it doesn't imply price is guaranteed |
| expiry | __required__ Time (seconds since epoch) when order expires |
| lowPrice | Minimum execution price |
| highPrice | Maximum execution price |
| stopLoss | Stop Loss value |
| takeProfit | Take Profit value |
| trailingStop | Trailing Stop distance in pipettes |
| ocaGroupId | OCA group id. 0 means not in a group |
| ocaLink | Any existing order id in the account. The fields ocaGroupId and ocaLink are mutually exculsive per request |

#### Required scope
trade

## GET /accounts/:account_id/order/:order_id

#### Request
    https://api.aonda.com/v1/accounts/1234/order/43211

#### Respond

    {
      "id" : 43211,             // The ID of the Order
      "units" : 5,                // The number of units in the Order
      "direction" : "long",       // The direction of the Order
      "instrument" : "EUR\/USD",   // The symbol of the instrument of the Order
      "time" : 1234567890,        // The time of the Order (seconds since Unix epoch)
      "price" : 1.45123,          // The price the Order was executed at
      "expiry" : 1352939000,
      "takeProfit" : 1.7,         // The take-profit associated with the Order, if any
      "stopLoss" : 1.4,           // The stop-loss associated with the Order, if any
      "trailingStop" : 10,         // The trailing stop associated with the Order, if any
      "highLimit" : 0,
      "lowLimit" : 0,
      "ocaGroupId" : 0
    }

#### Required scope
read





## PUT /accounts/:account_id/Order/:Order_id

#### Request
    curl -X PUT -d 'stopLoss=1.6' https://api.aonda.com/v1/Order/43211

#### Respond
    {
      "id" : 43211,             // The ID of the Order
      "units" : 5,                // The number of units in the Order
      "direction" : "long",       // The direction of the Order
      "instrument" : "EUR/USD",   // The symbol of the instrument of the Order
      "time" : 1234567890,        // The time of the Order (seconds since Unix epoch)
      "price" : 1.45123,          // The price the Order was executed at
      "takeProfit" : 1.7,         // The take-profit associated with the Order, if any
      "stopLoss" : 1.6,           // The stop-loss associated with the Order, if any
      "trailingStop" : 10         // The trailing stop associated with the Order, if any
    }

#### Parameters
| Name | Description |
| ---- | ----------- |
| units | Number of units to open Order for |
| price | The price at which the order will trigger at (TODO: re-word so it doesn't imply price is guaranteed |
| expiry | Time (seconds since epoch) when order expires |
| lowPrice | Minimum execution price |
| highPrice | Maximum execution price |
| stopLoss | Stop Loss value |
| takeProfit | Take Profit value |
| trailingStop | Trailing Stop distance in pipettes |
| ocaGroupId | OCA group id. 0 means not in a group |
| ocaLink | Any existing order id in the account. The fields ocaGroupId and ocaLink are mutually exculsive per request |

#### Required scope
trade



## DELETE /accounts/:account_id/Order/:Order_id

#### Request
    curl -X DELETE https://api.aonda.com/v1/order/43211

#### Respond
    {
      "id" : 54332,               // The ID of the close Order transaction
      "price" : 1.30601           // The pirce Order executed at
      "instrument" : "EUR/USD",   // The symbol of the instrument of the Order
      "unit" : 2,
      "direction" : "short",
      "ocaGroupId" : 0
    }

#### Parameters

#### Required scope
order

## GET template
#### Request
#### Respond
#### Parameters
#### Required scope
