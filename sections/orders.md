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
    http://api-sandbox.oanda.com/v1/accounts/12345/orders?instrument=EUR_USD&count=4

#### Response
    {
      "orders" : [
          { "id" : 12345, "type": "entry", "side" : "buy", "instrument" : "EUR_USD", "units" : 100, "time" : "2013-01-09T22:02:46Z", "price" : 1.5, "stopLoss" : 1.2, "takeProfit" : 1.7, "expiry" : "2013-04-09T22:02:46Z", "upperBound" : 2.0, "lowerBound" : 1.0, "trailingStop" : 10, "ocaGroupId" : 0},
          { "id" : 12344, "type": "entry", "side" : "sell", "instrument" : "EUR_USD", "units" : 100, "time" : "2013-01-09T22:02:46Z", "price" : 1.5, "stopLoss" : 1.2, "takeProfit" : 1.7, "expiry" : "2013-04-09T22:02:46Z", "upperBound" : 2.0, "lowerBound" : 1.0, "trailingStop" : 10, "ocaGroupId" : 1},
          { "id" : 890, "type": "limit", "side" : "sell", "instrument" : "EUR_USD", "units" : 100, "time" : "2013-01-09T22:02:46Z", "price" : 1.5, "stopLoss" : 1.2, "takeProfit" : 1.7, "expiry" : "2013-04-09T22:02:46Z", "upperBound" : 2.0, "lowerBound" : 1.0, "trailingStop" : 10, "ocaGroupId" : 1},
          { "id" : 789, "type": "stop", "side" : "sell", "instrument" : "EUR_USD", "units" : 100, "time" : "2013-01-09T22:02:46Z", "price" : 1.5, "stopLoss" : 1.2, "takeProfit" : 1.7, "expiry" : "2013-04-09T22:02:46Z", "upperBound" : 2.0, "lowerBound" : 1.0, "trailingStop" : 10, "ocaGroupId" : 1}
      ],
      "nextPage" : "http:\/\/api-sandbox.oanda.com\/accounts\/12345\/orders?count=4&maxOrderId=788"
    }

#### Query Parameters
**Optional**

* **maxOrderId**: The server will return orders with id less than or equal to this, in descending order (for pagination)
* **count**: Maximum number of open orders to return. Default: 50 Max value: 500
* **instrument**: Retrieve open orders for a specific instrument only Default: all
* **orderIds**: A comma separated list of orders to retrieve


## POST /v1/accounts/:account_id/orders
#### Request
    curl -X POST -d 'instrument=EUR_USD&units=2&side=sell&price=1.2&expiry=2013-04-01T00:00:00Z' http://api-sandbox.oanda.com/v1/accounts/12345/orders

#### Response
    {
        "id" : 268167142,            // Order id
        "instrument" : "EUR_USD",    // Instrument of the order
        "price" : 1.2,               // Trigger price of the order
        "units" : 2,                 // Number of units
        "side" : "sell",             // Direction of the order
    }


#### Parameters
**Required**

* **instrument**: Instrument to open Order on
* **units**: Number of units to open Order for
* **expiry**: UTC Time (in RFC3339 Format) when order expires
* **price**: Price where order is set to trigger at
* **side**: 'buy' or 'sell'

**Optional**

<!--* **type**: entry (default), or limit (More about order types) -->
* **lowerBound**: Minimum execution price
* **upperBound**: Maximum execution price
* **stopLoss**: Stop Loss value
* **takeProfit**: Take Profit value
* **trailingStop**: Trailing Stop distance in pipettes

## GET /v1/accounts/:account_id/order/:order_id

#### Request
    http://api-sandbox.oanda.com/v1/accounts/1234/orders/43211

#### Response

    {
      "id" : 43211,                        // The ID of the Order
      "units" : 5,                         // The number of units in the Order
      "side" : "buy",                      // The direction of the Order
      "instrument" : "EUR_USD",            // The symbol of the instrument of the Order
      "time" : "2013-01-01T00:00:00Z",     // The time of the Order (in RFC3339 format)
      "price" : 1.45123,                   // The price the Order was executed at
      "expiry" : "2013-02-01T00:00:00Z",   // The time the Order expires (in RFC3339 format)
      "takeProfit" : 1.7,                  // The take-profit associated with the Order, if any
      "stopLoss" : 1.4,                    // The stop-loss associated with the Order, if any
      "trailingStop" : 10,                 // The trailing stop associated with the Order, if any
      "upperBound" : 0,
      "lowerBound" : 0
    }


## PUT /v1/accounts/:account_id/orders/:order_id

#### Request
    curl -X PUT -d 'stopLoss=1.3' http://api-sandbox.oanda.com/v1/accounts/12345/orders/43211

#### Response
    {
      "id" : 43211,                        // The ID of the Order
      "units" : 5,                         // The number of units in the Order
      "side" : "buy",                      // The direction of the Order
      "instrument" : "EUR_USD",            // The symbol of the instrument of the Order
      "units" : 5,                         // The number of units in the Order
      "time" : "2013-01-01T00:00:00Z",     // The time of the Order (in RFC3339 format)
      "price" : 1.45123,                   // The price the Order was executed at
      "takeProfit" : 1.7,                  // The take-profit associated with the Order, if any
      "stopLoss" : 1.3,                    // The stop-loss associated with the Order, if any
      "expiry" : "2013-02-01T00:00:00Z",   // The time the Order expires (in RFC3339 format)
      "upperBound" : 0,
      "lowerBound" : 0
    }

#### Parameters
**Optional**

* **units**: Number of units to open Order for |
* **price**: The price at which the order is set to trigger at
* **expiry**: UTC time (in RFC3339 format) when order expires
* **lowerBound**: Minimum execution price
* **upperBound**: Maximum execution price
* **stopLoss**: Stop Loss value
* **takeProfit**: Take Profit value
* **trailingStop**: Trailing Stop distance in pipettes




## DELETE /v1/accounts/:account_id/orders/:order_id

#### Request
    curl -X DELETE http://api-sandbox.oanda.com/v1/accounts/12345/order/43211

#### Response
    {
      "units" : 2,
      "instrument" : "EUR_USD",   // The symbol of the instrument of the Order
      "price" : 1.30601           // The price at which the Order executed
      "id" : 54332,               // The ID of the close Order transaction
      "side" : "sell"
    }


