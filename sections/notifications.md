# Notifications Endpoints

| Endpoint | Description |
| -------- | ----------- |
| POST /user/:username/notifications | Register a mobile device for notification |

## POST /user/:username/notifications

#### Request
    curl -d 'type=apple+apns&app_id=A1234567.com.oanda.fxmobile&data=c9d4c07cfbbc26d6ef87a44d53e169831096a5d5fd82547556659dddf715defc' https://api.oanda.com/v1/users/testuser/notification

#### Response
    {}

#### Paramters
| Name | Description |
| ---- | ----------- |
| type | A string indicating the notification type. Currently 'apple apns' is supported. |
| appId | A string indicating the application's ID. |
| data | A string representing type specific data used by the backend to send notifications. For 'apple apns' types, this should be the device token assigned by Apple's APNS in hex format with no spaces. |

#### Required scope
??? TODO

#### Restriction
Each device can only be registered to one user.  However, a user an register with multiple devices.
