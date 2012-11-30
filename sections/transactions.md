# Transaction Endpoints

| Endpoint | Description |
| ---- | ---- |
| [GET /v1/accounts/:account_id/transactions](#get-v1accountsaccount_id) | Retrieve transaction history for :account_id |
| [GET /v1/accounts/:account_id/transactions/:transaction_id](transactions.md#get-v1accountsaccount_idtransactions-1) | Retrieve details for :transaction_id  |


## GET /v1/accounts/:account_id/transactions

#### Request
    http://api-sandbox.oanda.com/v1/accounts/12345/transactions?instrument=EUR_USD&maxCount=1

#### Response
    {
        "transactions" : [
            {
                "id" : 177808963,
                "accountId" : 6531071,
                "type" : "SellMarket",
                "instrument" : "EUR_USD",
                "units" : 2,
                "time" : 1352934583,
                "price" : 1.2736,
                "balance" : 100000.0815,
                "interest" : 0,
                "profitLoss" : 0,
                "highOrderLimit" : 0,
                "lowOrderLimit" : 0,
                "amount" : 2.5472,
                "stopLoss" : 0,
                "takeProfit" : 0,
                "duration" : 0,
                "completionCode" : 100,
                "transactionLink" : 0,
                "orderLink" : 0,
                "diaspora" : 0,
                "trailingStop" : 0,
                "marginUsed" : 0.1274
            },
        ],
        "nextPage" : "http:\/\/oanda-cs-dev:1342\/accounts\/6531071\/transactions?maxCount=1&maxTransId=177808962"
    }


####Query Parameters
**Optional**

* **maxTransId**: First transaction to get. The server will return trades with id less than or equal to this, in descending order (about pagination). 
* **minTransId**: Last transaction to get. The server will return trades with id greater or equal to this, in descending order (about pagination).
* **maxCount**: Maximum number of open trades to return. Default: 50 Max value: 500 
* **instrument**: Restrict open trade for a specific instrument. Default: all 
* **tradeIds**: A common separated list of trades to retrieve.


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
        "time" : 1352934583,
        "price" : 1.2736,
        "balance" : 100000.0815,
        "interest" : 0,
        "profitLoss" : 0,
        "highOrderLimit" : 0,
        "lowOrderLimit" : 0,
        "amount" : 2.5472,
        "stopLoss" : 0,
        "takeProfit" : 0,
        "duration" : 0,
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
* **ChangeOrder**: Motification to a order
* **CloseOrder**: Cancelling of a order
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
