# Getting Started Guide

## Quick start with curl
---

#### Step 1: Create a new user
	$curl -X POST -d "currency=USD" "http://api-sandbox.oanda.com/users"

	{
    	"username" : "willymoth",
    	"password" : "balvEdayg"
	}
#### Step 2: Get account belongs to user
	$ curl "http://api-sandbox.oanda.com/users/willymoth/accounts"

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
	$ curl -X POST -d "instrument=EUR/USD&units=1&direction=long" "http://api-sandbox.oanda.com/accounts/6531071/trades

	{
    	"ids" : [177715575],
    	"instrument" : "EUR\/USD",
    	"units" : 2,
    	"price" : 1.30582,
    	"marginUsed" : 0.1306,
    	"direction" : "short"
	}

## Examples
-----

* [Simple Rate Panel](): Simple javascript page that displays rates for selected currency pair