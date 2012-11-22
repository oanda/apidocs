# Transaction Endpoints

| Endpoint | Description |
| ---- | ---- |
| [GET /accounts/:account_id/transactions](#get-accountsaccount_id) | Retrieve transaction history for :account_id |
| [GET /accounts/:account_id/transactions/:id](transactions.md#get-accountsaccount_idtransactions-1) | Create a open trade |


## GET /accounts/:account_id/transactions

#### Request
    https://api.oanda.com/v1/accounts/12345/transactions?instrument=EUR/USD,maxCount=1

#### Respond
    {
        "transactions" : [
            {
                "id" : 177808963,
                "accountId" : 6531071,
                "type" : "SellMarket",
                "instrument" : "EUR\/USD",
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
        "nextPage" : "https:\/\/oanda-cs-dev:1342\/accounts\/6531071\/transactions?maxCount=1&maxTransId=177808962"
    }


####Query Parameters
**Optional**

* **maxTransId**: First transaction to get. The server will return trades with id less than or equal to this, in descending order (about pagination). 
* **minTransId**: Last transaction to get. The server will return trades with id greater or equal to this, in descending order (about pagination).
* **maxCount**: Maximum number of open trades to return. Default: 50 Max value: 500 
* **instrument**: Restrict open trade for a specific instrument. Default: all 
* **tradeIds**: A common separated list of trades to retrieve.

#### Required scope
read

## GET /accounts/:account_id/transactions
#### Request
    https://api.oanda.com/v1/accounts/12345/transactions/1170980

#### Respond
    {
        "id" : 177808963,
        "accountId" : 6531071,
        "type" : "SellMarket",
        "instrument" : "EUR\/USD",
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

#### Required scope
read


## Transaction Types

* **SellMarket**
* **BuyMarket**
* **ChangeTrade**
* **CloseTradeB**
* **CloseTradeS**
* **Interest**
* **ClosePositionB**
* **ClosePositionS**
* **Withhold**
* **BuyEntry**
* **SellEntry**
* **BuyLimit**
* **SellLimit**
* **BuyStop**
* **SellStop**
* **ChangeOrder**
* **CloseOrder**
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
* **PriceAlert**
* **FlaggedTrade**
