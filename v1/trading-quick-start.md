---
title: Quick Start Trading | OANDA API
---

Trading quick start guide
---

To trade with OANDA, you first need to create a user.  A user owns accounts, and accounts are what hold the money you’re using to trade.  When you want to place a trade, you must specify the account containing the funds you wish to trade on.

What does this mean, you ask?  Simply [generate a user and account](http://oanda.github.com/gen-account.html).  You will be given a username, password and an account id.

* The account id is required as a parameter to any requests related to making trades or getting trade information

The username and password can, in most cases, be thrown away.  We wanted to make it easy for people to start trading as quickly as possible in our sandbox, and so the API is not authenticated.  **The account id is the only thing you need to trade.**

If you don't want to [automatically generate a test account](http://oanda.github.com/gen-account.html), you can follow the steps below using curl (or your own favourite HTTP client).

#### Step 1: Create a new user
	curl -X POST -H "Content-Type: application/x-www-form-urlencoded" "http://api-sandbox.oanda.com/v1/accounts"

Sample response:

	{
            "username" : "willymoth",
            "password" : "balvEdayg",
            "accountId" : 3563320
	}
#### Step 2: Get account information
	curl -X GET "http://api-sandbox.oanda.com/v1/accounts/3563320"

Sample response:

    	{
            "accountId" : 3563320,
            "accountName" : "Primary",
            "balance" : 100000,
            "unrealizedPl" : 0,
            "realizedPl" : 0,
            "marginUsed" : 0,
            "marginAvail" : 100000,
            "openTrades" : 0,
            "openOrders" : 0,
            "marginRate" : 0.05,
            "accountCurrency" : "USD"
    	}

#### Step 3: Start Trading
	curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "instrument=EUR_USD&units=1&side=buy" "http://api-sandbox.oanda.com/v1/accounts/3563320/trades"

Sample response:

        {
            "ids" : [
                178690627
            ],
            "instrument" : "EUR_USD",
            "units" : 1,
            "price" : 1.28485,
            "marginUsed" : 0.0642,
            "side" : "buy"
        }

