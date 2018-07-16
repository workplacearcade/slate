# Users

## Creating a new user

```shell
curl "https://stagingapi.arcadehub.co/v2/users"
  -H "X-USER-TOKEN: abcd"
  -X POST
  -d "user[firstname]=""&user[lastname]=""&user[email]=""&user[company_identifier]=""&user[entity_id]=""&user[team_id]=""&user[invite]="""
```

> The above command returns JSON structured like this:

```json
    { "message": "User has been created" }
```

This endpoint creates a new user in the Arcade system.

<aside class="warning">If the user account already exists a 400 error will be thrown</aside>

### HTTP Request

`POST https://stagingapi.arcadehub.co/v2/users`

### URL Parameters

Parameter | Description | Type
--------- | -----------   ----
fisrtname | The user's firstname | String
lastname | The user's lastname | String
email | The user's email address | String (required)
company_identifier | The user's integration identifier | String
entity_id | The user's company ID | ID
team_id | The user's team ID | ID
invite | Whether or not to send the user a welcome email on invite | boolean (default false)
