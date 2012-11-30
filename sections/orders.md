# Order Endpoints

| Endpoint | Description |
| ---- | ---- |
| [GET /v1/accounts/:account_id/orders](https://github.com/oanda/apidocs/blob/master/sections/orders.md#get-v1accountsaccount_idorders) | Get a list of open orders |
| [POST /v1/accounts/:account_id/orders](https://github.com/oanda/apidocs/blob/master/sections/orders.md#post-v1accountsaccount_idorders) | Create an open order |
| [GET /v1/accounts/:account_id/orders/:order_id](https://github.com/oanda/apidocs/blob/master/sections/orders.md#get-v1accountsaccount_idorderorder_id) | Get information about an open order |
| [PUT /v1/accounts/:account_id/orders/:order_id](https://github.com/oanda/apidocs/blob/master/sections/orders.md#put-v1accountsaccount_idordersorder_id) | Modify an open order |
| [DELETE /v1/accounts/:account_id/orders/:order_id](https://github.com/oanda/apidocs/blob/master/sections/orders.md#delete-v1accountsaccount_idordersorder_id) | Close an open order |


## GET /v1/accounts/:account_id/orders

#### Request
    http://api-sandbox.oanda.com/v1/accounts/12345/orders?instrument=EUR_USD&maxCount=4

#### Response
    {
      "orders" : [
          { "id" : 12345, "type": "entry", "direction" : "long", "instrument" : "EUR_USD", "units" : 100, "time" : 1234567891, "price" : 1.5, "stopLoss" : 1.2, "takeProfit" : 1.7, "expiry" : 1234567890, "highLimit" : 2.0, "lowLimit" : 1.0, "trailingStop" : 10, "ocaGroupId" : 0},
          { "id" : 12344, "type": "entry", "direction" : "short", "instrument" : "EUR_USD", "units" : 100, "time" : 1234567890, "price" : 1.5, "stopLoss" : 1.2, "takeProfit" : 1.7, "expiry" : 1234567890, "highLimit" : 2.0, "lowLimit" : 1.0, "trailingStop" : 10, "ocaGroupId" : 1},
          { "id" : 890, "type": "limit", "direction" : "short", "instrument" : "EUR_USD", "units" : 100, "time" : 1234567890, "price" : 1.5, "stopLoss" : 1.2, "takeProfit" : 1.7, "expiry" : 1234567890, "highLimit" : 2.0, "lowLimit" : 1.0, "trailingStop" : 10, "ocaGroupId" : 1},
          { "id" : 789, "type": "stop", "direction" : "short", "instrument" : "EUR_USD", "units" : 100, "time" : 1234567890, "price" : 1.5, "stopLoss" : 1.2, "takeProfit" : 1.7, "expiry" : 1234567890, "highLimit" : 2.0, "lowLimit" : 1.0, "trailingStop" : 10, "ocaGroupId" : 1}
      ],
      "nextPage" : "http:\/\/api-sandbox.oanda.com\/accounts\/12345\/orders?maxCount=4&maxOrderId=788"
    }

#### Query Parameters
**Optional**

* **maxOrderId**: The server will return orders with id less than or equal to this, in descending order (about pagination).
* **maxCount**: Maximum number of open orders to return. Default: 50 Max value: 500
* **instrument**: Restrict open Order for a specific instrument. Default: all
* **orderIds**: A common separated list of orders to retrieve.


## POST /v1/accounts/:account_id/orders
#### Request
    curl -X POST -d 'instrument=EUR_USD&units=2&direction=short&price=1.2&expiry=1352939000' http://api-sandbox.oanda.com/v1/accounts/12345/orders

#### Response
    {
        "id" : 268167142,            // Order id
        "instrument" : "EUR_USD",   // Instrument of the order
        "price" : 1.2,				  // Trigger price of the order
        "units" : 2,                 // Number of units
        "direction" : "short",       // Direction of th order
        "ocaGroupId" : 0
    }


#### Parameters
**Required**

* **instrument**: Instrument to open Order on
* **units**: Number of units to open Order for
* **expiry**: Time (seconds since epoch) when order expires
* **price**: Price where order is set to trigger at

**Optional**

* **type**: entry (default), or limit (More about order types)
* **direction**: long (default) or short
* **lowPrice**: Minimum execution price
* **highPrice**: Maximum execution price
* **stopLoss**: Stop Loss value
* **takeProfit**: Take Profit value
* **trailingStop**: Trailing Stop distance in pipettes
* **ocaGroupId**: OCA group id. 0 means not in a group
* **ocaLink**: Any existing order id in the account. The fields ocaGroupId and ocaLink are mutually exculsive per request

## GET /v1/accounts/:account_id/order/:order_id

#### Request
    http://api-sandbox.oanda.com/v1/accounts/1234/orders/43211

#### Response

    {
      "id" : 43211,             // The ID of the Order
      "units" : 5,                // The number of units in the Order
      "direction" : "long",       // The direction of the Order
      "instrument" : "EUR_USD",   // The symbol of the instrument of the Order
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






## PUT /v1/accounts/:account_id/orders/:order_id

#### Request
    curl -X PUT -d 'stopLoss=1.6' http://api-sandbox.aonda.com/v1/orders/43211

#### Response
    {
      "id" : 43211,             // The ID of the Order
      "units" : 5,                // The number of units in the Order
      "direction" : "long",       // The direction of the Order
      "instrument" : "EUR_USD",   // The symbol of the instrument of the Order
      "time" : 1234567890,        // The time of the Order (seconds since Unix epoch)
      "price" : 1.45123,          // The price the Order was executed at
      "takeProfit" : 1.7,         // The take-profit associated with the Order, if any
      "stopLoss" : 1.6,           // The stop-loss associated with the Order, if any
      "trailingStop" : 10         // The trailing stop associated with the Order, if any
    }

#### Parameters
**Optional**

* **units**: Number of units to open Order for |
* **price**: The price at which the order is set to trigger at
* **expiry**: Time (seconds since epoch) when order expires
* **lowPrice**: Minimum execution price
* **highPrice**: Maximum execution price
* **stopLoss**: Stop Loss value
* **takeProfit**: Take Profit value
* **trailingStop**: Trailing Stop distance in pipettes
* **ocaGroupId**: OCA group id. 0 means not in a group
* **ocaLink**: Any existing order id in the account. The fields ocaGroupId and ocaLink are mutually exculsive per request





## DELETE /v1/accounts/:account_id/orders/:order_id

#### Request
    curl -X DELETE http://api-sandbox.aonda.com/v1/order/43211

#### Response
    {
      "id" : 54332,               // The ID of the close Order transaction
      "price" : 1.30601           // The pirce Order executed at
      "instrument" : "EUR_USD",   // The symbol of the instrument of the Order
      "unit" : 2,
      "direction" : "short",
      "ocaGroupId" : 0
    }


