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
                "type" : "marketIfTouched",
                "units" : 2,
                "side" : "sell",
                "action" : "open",
                "reason" : "user_submitted",
                "instrument" : "EUR_USD",
                "time" : "2012-07-03T14:30:38Z",
                "price" : 1.2736,
                "balance" : 100000.0815,
                "profitLoss" : 0,
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
* **ids**: A comma separated list of transactions ids to retrieve. Maximum number of ids: 50. No other parameter may be specified with the ids parameter.


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
                "type" : "marketIfTouched",
                "units" : 2,
                "side" : "sell",
                "action" : "open",
                "reason" : "user_submitted",
                "instrument" : "EUR_USD",
                "time" : "2012-07-03T14:30:38Z",
                "price" : 1.2736,
                "balance" : 100000.0815,
                "profitLoss" : 0,
                "marginUsed" : 0.1274
    }


####Transaction Types and Actions

Below is a table showing the possible actions associated with each transaction type

| Type | Action |
| ---- | ---- |
| market | open |
|        | close |
|        | update |
|        | correct |
|        | flag_to_close |
| marketIfTouched | open |
|       | update |
|       | close |
| limit | open |
|       | close |
| stop | open |
|      | close |
| position | close |
| balance | correct |
| interest | create |
|          | correct |
| profit_loss | correct |
|            | reset |
| margin | update |
| fund | deposit |
|      | withdraw |
|      | debit |
|      | credit |
| fund_fee | withdraw |
| api_fee | withdraw |
| api_license_fee | withdraw |
| wire_fee | withdraw |
| FOK_market | open |
| IOC_market | open |
|            | close |
| account | create |
| margin_alert | enter |
|            | resolve |
