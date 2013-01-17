# User Endpoints

| Endpoint | Description |
| ---- | ---- |
| POST /v1/users | User registration |
<!--
| TODO: needed? GET /users/:username/profile/:profile_name | Set user profile for :username |
| TODO: needed? POST /users/:username/profile/:profile_name | Set user profile for :username |
-->

## POST /v1/users

Create a user account with random username and password.  One account will be created and funded.

#### Request
    curl -X POST -d 'currency=USD' http://api-sandbox.oanda.com/v1/users

#### Response
    {
    	"username" : "willymoth",
    	"password" : "balvEdayg"
	}
  
#### Query Parameters
**Required**

* **currency**: Home currency of user's accounts. Possible values: USD, CAD, EUR, CHF, AUD, GBP, JPY
