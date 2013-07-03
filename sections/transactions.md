# Transaction Endpoints

| Endpoint | Description |
| ---- | ---- |
| [GET /v1/accounts/:account_id/transactions](#get-v1accountsaccount_id) | Retrieve transaction history for :account_id |
| [GET /v1/accounts/:account_id/transactions/:transaction_id](transactions.md#get-v1accountsaccount_idtransactions-1) | Retrieve details for :transaction_id  |


## GET /v1/accounts/:account_id/transactions

#### Request
    http://api-sandbox.oanda.com/v1/accounts/12345/transactions?instrument=EUR_USD&count=1

#### Response
    {
        "transactions" : [
            {
                "id" : 177808963,
                "accountId" : 6531071,
                "type" : "SellMarket",
                "instrument" : "EUR_USD",
                "units" : 2,
                "time" : "2012-07-03T14:30:38Z",
                "price" : 1.2736,
                "balance" : 100000.0815,
                "interest" : 0,
                "profitLoss" : 0,
                "upperBound" : 0,
                "lowerBound" : 0,
                "amount" : 2.5472,
                "stopLoss" : 0,
                "takeProfit" : 0,
                "expiry" : 0,
                "completionCode" : 100,
                "transactionLink" : 0,
                "orderLink" : 0,
                "diaspora" : 0,
                "trailingStop" : 0,
                "marginUsed" : 0.1274
            },
        ],
        "nextPage" : "http:\/\/api-sandbox.oanda.com\/v1\/accounts\/6531071\/transactions?count=1&maxId=177808962"
    }


####Query Parameters
**Optional**

* **maxId**: First transaction to get. The server will return transactions with id less than or equal to this, in descending order (for pagination). 
* **minId**: Last transaction to get. The server will return transactions with id greater or equal to this, in descending order (for pagination).
* **count**: Maximum number of transactions to return. The default and maximum value for count is 50.
* **instrument**: Retrieve transactions for a specific instrument only Default: all 
* **ids**: A comma separated list of transactions ids to retrieve

####Pagination

Transactions can be paginated with the count and maxId parameters.
At most, a maximum of 50 transactions can be returned in one query. 
If more transactions exist than specified by the given or default count, a url with maxId set to the next unreturned transaction will be constructed.

## GET /v1/accounts/:account_id/transactions/:transaction_id
#### Request
    http://api-sandbox.oanda.com/v1/accounts/12345/transactions/1170980

#### Response
    {
        "id" : 177808963,
        "accountId" : 6531071,
        "type" : "SellMarket",
        "instrument" : "EUR_USD",
        "units" : 2,
        "time" : "2012-07-03T14:30:38Z",
        "price" : 1.2736,
        "balance" : 100000.0815,
        "interest" : 0,
        "profitLoss" : 0,
        "upperBound" : 0,
        "lowerBound" : 0,
        "amount" : 2.5472,
        "stopLoss" : 0,
        "takeProfit" : 0,
        "expiry" : 0,
        "completionCode" : 100,
        "transactionLink" : 0,
        "orderLink" : 0,
        "diaspora" : 0,
        "trailingStop" : 0,
        "marginUsed" : 0.1274
    }



## Transaction Types

* **SellMarket**: Short market order
* **BuyMarket**: Long market order
* **ChangeTrade**: Modification to trailing stop, take profit, or stop loss of a trade
* **CloseTradeB**: Closing transaction of a Buy market trade
* **CloseTradeS**: Closing transaction of a Sell market trade
* **Interest**: Interest payment
* **ClosePositionB**: Closing transaction of a long position
* **ClosePositionS**: Closing transaction of a short position
* **Withhold**: 
* **BuyEntry**: Long entry limit order
* **SellEntry**: Short entry limit order
* **BuyLimit**: Long limit order
* **SellLimit**: Short limit order
* **BuyStop**: Buy stop order
* **SellStop**: Sell stop order
* **ChangeOrder**: Modification to an order
* **CloseOrder**: Cancelling of an order
* **AddFunds**
* **CrFunds**
* **RebateFunds**
* **PostInterestFunds**
* **AddFundsDivTransfer**
* **DelFunds**
* **DbFunds**
* **DelFundsDivTransfer**
* **Fee**
* **BuyCorrection**
* **SellCorrection**
* **PriceAlert**: Price alert notification. Used by OANDA mobile application.
* **FlaggedTrade**
