Trading quick start guide
---

To trade with OANDA, you first need to create a user.  A user owns accounts, and accounts are what hold the money you’re using to trade.  When you want to place a trade, you must specify the account containing the funds you wish to trade on.

What does this mean, you ask?  Simply [generate a user and account](http://oanda.github.com/gen-account.html).  You will be given a username and an account id.

* The username can be used to retrieve the account id owned by the user
* The account id is required as a parameter to any requests related to making trades or getting trade information

The username and password can, in most cases, be thrown away.  We wanted to make it easy for people to trading as quickly as possible in our sandbox, and so the API is not authenticated.  **The account id is the only thing you need to trade.**

If you don't want to [automatically generate a test account](http://oanda.github.com/gen-account.html), you can follow the steps below using curl (or your own favourite HTTP client).

#### Step 1: Create a new user
	$curl -X POST -d "currency=USD" "http://api-sandbox.oanda.com/users"

Sample response:

	{
    	"username" : "willymoth",
    	"password" : "balvEdayg"
	}
#### Step 2: Get account belongs to user
	$ curl "http://api-sandbox.oanda.com/accounts?username=willymoth"

Sample response:

	[
    	{
        	"id" : 6531071,
        	"name" : "Primary",
        	"homecurr" : "USD",
        	"marginRate" : 0.05,
        	"accountPropertyName" : []
    	}
	]

#### Step 3: Start Trading
	$ curl -X POST -d "instrument=EUR/USD&units=1&direction=long" http://api-sandbox.oanda.com/accounts/6531071/trades

Sample response:

	{
    	"ids" : [177715575],
    	"instrument" : "EUR\/USD",
    	"units" : 2,
    	"price" : 1.30582,
    	"marginUsed" : 0.1306,
    	"direction" : "short"
	}
