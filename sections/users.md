# User Endpoints

| Endpoint | Description |
| ---- | ---- |
| POST /users | User registration |
| TODO: needed? GET /users/:username/profile/:profile_name | Set user profile for :username |
| TODO: needed? POST /users/:username/profile/:profile_name | Set user profile for :username |

## POST /users

#### Request
    curl -X POST -d 'currency=USD' http://api.oanda.com/v1/users

#### Response
    {
    	"username" : "willymoth",
    	"password" : "balvEdayg"
	}
  
#### Query Parameters
**Required**

* **currency**: Home currency of user's accounts. Possible values: USD, CAD, EUR, CHF, AUD, GBP, JPY

#### Required scope
write