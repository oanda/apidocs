# User Endpoints

| Endpoint | Description |
| ---- | ---- |
| POST /users | User registration |
| TODO: needed? GET /users/:username/profile/:profile_name | Set user profile for :username |
| TODO: needed? POST /users/:username/profile/:profile_name | Set user profile for :username |

## POST /users

#### Request
    curl -d 'first_name=John&last_name=Smith&email=john@smith.com&username=jsmith&password=mypassword&currency=USD' http://api.oanda.com/v1/users

#### Respond
    {}

#### Query Parameters
** Required **

* **firstName**: User's first name
* **lastName**: User's first name
* **email**: User's email address
* **username**: User's username
* **password**: User's password
* **currency**:Home currency. One of the following values: USD, CAD, EUR, CHF, AUD, GBP, JPY

**Optional**

* **emailOptIn**:Sign up for emailing list. 1 (yes) or 0 (no)


#### Required scope
write