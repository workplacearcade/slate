# Profile

## Load User's Profile
```shell
curl "https://stagingapi.arcadehub.co/v2/profile"
  -H "X-USER-TOKEN: abcd"
```

> This will return a JSON object of the structure

```json
  {
    "profile" : {
      "user": {
        "id": 1,
        "name": "James McLaren",
        "email": "james@workplacearcade.com",
        "contact_number": "+61433348986",
        "birthday": "User birthday datetime",
        "about_me": "User provided about me",
        "badge": "Link to user's equipped badge image",
        "profile_image": "Link to user's profile image",
        "team": "Name of user's team",
      },
      "level": {
        "level": 12,
        "percentage_through_level": 25,
        "experience_to_level_up": 145,
        "next_level": 13,
      },
      "trait": "Current user's trait",
      "core_stats":       {
        "team_id": 12,
        "tokens_received": 4,
        "quests_completed": 7
      },
      "muted": "Has the current user muted all notifications? < true | false >",
      "is_self": "Does the profile belong to the current user? < true | false >"
    }
  }
```

This endpoint returns all of the initial information necessary to display a user's profile. A few points worth clarifying:

### Tokens received
Corresponds to the number of recognition stars that this particular user has received

### Trait
This is a user selected bonus that allows them to earn more chests for completing a particular action in Arcade. The return value will be a string corresponding to their selected trait.

### HTTP Request

`GET https://stagingapi.arcadehub.co/v2/profile`

### URL Parameters
Parameter | Description | Type
--------- | ----------- | ----
user_id | ID of the user whose profile you want to view | ID (defauls to current user)

## Updating Profile image
```shell
curl "https://stagingapi.arcadehub.co/users/image"
  -H "X-USER-TOKEN: abcd"
  -X POST
  -D "avatar="""
```

> This will return a JSON object of the structure:

```json
  {
    "image": "Link to newly uploaded image"
  }
```

This endpoint allows users to upload a profile image.

<aside class="success">
The uploaded image will be resized on the Workplace Arcade backend
</aside>

### HTTP Request

`POST https://stagingapi.arcadehub.co/users/image`

### URL Parameters
Parameter | Description | Type
--------- | ----------- | ----
avatar | The image to be uploaded | Base64 String

## Updating Profile
```shell
curl "https://stagingapi.arcadehub.co/v1/users"
  -H "X-USER-TOKEN: abcd"
  -X PUT
  -D "user[email]=""&user[firstname]=""&user[lastname]=""&user[contact_number]=""&user[birthday]=""&user[about_me]=""
```

> This will return a JSON object of the structure:

```json
  {
    "message": "Your account has been updated"
  }
```

This endpoint allows users to update their profile information. Do note that, with the exception of email address, all of these values are optional

### HTTP Request

`PUT https://stagingapi.arcadehub.co/v1/users`

### URL Parameters
Parameter | Description | Type
--------- | ----------- | ----
email | The user's email | String
firstname | The user's firstname | String
lastname | The user's lastname | String
contact_number | The user's contact number | String
birthday | The user's birthday | Datetime

## Load Available Badges
```shell
curl "https://stagingapi.arcadehub.co/v2/badges"
  -H "X-USER-TOKEN: abcd"
```

> This will return a JSON object of the structure

```json
  {
    "badges": [
      {
        "id": 1,
        "name": "Name of badge",
        "image_url": "Link to badge image url",
        "active": "Whether or not this badge is active for the current user",
        "unlocked": "Whether or not the current user has unlocked the badge",
        "modal_content": "Further information about how the badge can be earned",
        "unlcoked_at": "When the badge was unlocked"
      }
    ]
  }
```

This endpoint returns information about all the currently available badges in Arcade. Note that a user can only have one badge active at a time.

### HTTP Request

`GET https://stagingapi.arcadehub.co/v2/badges`

## Equip a badge
```shell
curl "https://stagingapi.arcadehub.co/v2/badges"
  -H "X-USER-TOKEN: abcd"
  -X PUT
  -D "id="""
```

> This will return a JSON object of the structure:

```json
  {
    "status": "success",
    "message": "Your badge has been updated"
  }
```

This endpoint changes the equipped badge for the current user

### HTTP Request

`put https://stagingapi.arcadehub.co/v1/badges/active`

## Get Active User Quests
```shell
curl "https://stagingapi.arcadehub.co/v1/quests"
  -H "X-USER-TOKEN: abcd"
```

> This will return a JSON object of the structure:

```json
  {
    "quests": [
      {
        "reward": "Epic",
        "reward_type": "Chest",
        "reward_text": "Epic Chest",
        "objectives": [
          {
            "progress": 0.9,
            "goal": 1,
            "objective_content": "Like 1 Post"
          }
        ],
        "time_remaining": "1 hour",
        "time_remaining_exact": "End time in seconds since epoch",
        "progress": 0.9,
        "user_id": 1,
        "username": "James McLaren",
        "user_image": "Link to user image",
      }
    ]
  }
```

This endpoint returns an array of the quests to which a user is currently assigned. Do note that this endpoint is user for other quest types so the return object will contain some additional fields. Only the ones relevant for displaying the quest on the profile have been returned.

### HTTP Request
`get https://stagingapi.arcadehub.co/v1/quests`

## Load received tokens
```shell
curl "https://stagingapi.arcadehub.co/v2/tokens"
  -H "X-USER-TOKEN: abcd"
```

> This will return a JSON object of the structure

```json
  {
    "tokens": [
      {
        "id": 1,
        "awarding_user": "John Doe",
        "created_at": "Date token was awarded",
        "reason": "User provided reason for awarding",
        "category": "Type of token awarded",
        "image": "Link to token image",
        "awarding_user_id": 2,
        "location_information": {
          "location": "arcade.newsfeed",
          "location_id": 2
        }
      }
    ]
  }
```

This endpoint returns all tokens that have been awarded to the current user. The location corresponds to the activity that was created when the token was awarded.

### HTTP Request
`get https://stagingapi.arcadehub.co/v2.tokens`

## Awarding a Token
```shell
curl https://stagingapi.arcadehub.co/v1/tokens/award
  -H "X-USER-TOKEN: abcd"
  -X POST
  -D "user_id=""&category=""&message="""
```

> This will return a JSON object of the structure:

```json
  {
    "message": "Token has been awarded"
  }
```

This endpoint allows the current user to award their token to another user.

<aside class="warning">
Users are only able to award one token per week. If they attempt to award more this request will return an error message
</aside>

### Category
The token categories on offer are:
* Teaching
* MVP
* Performance
* Leadership
* Friendly


### HTTP Request

`POST https://stagingapi.arcadehub.co/v1/tokens/award`

### URL Parameters
Parameter | Description | Type
--------- | ----------- | ----
user_id | ID of the user who is receiving the token | ID
category | The type of token being awarded | String (see above for possible values)
reason | Message to be shown as part of awarded token | String (defaults to 'No Reason')

## List available traits
```shell
curl https://stagingapi.arcadehub.co/v2/traits
  -H "X-USER-TOKEN: abcd"
```

> This will return a JSON object of the structure:

```json
  {
    "traits": [
      {
        "name": "story teller",
        "modal_content": "Earn a rare chest every time you post on the newsfeed"
      }
    ]
  }
```

This end point returns a list of traits that a user can equip. In the current Workplace Arcade system users are only able to have one active trait and can only select their trait after level 10.

### HTTP Request

`GET https://stagingapi.arcadehub.co/v2/traits`

## Selecting new trait
```shell
curl https://stagingapi.arcadehub.co/v2/traits
  -H "X-USER-TOKEN: abcd"
  -X PUT
  -D "trait="""
```

> This will return a JSON object of the structure:

```json
  {
    "trait": "story teller"
  }
```

This endpoint allows a user to select their new trait. The provided `trait` param must be one of the available traits

### HTTP Request

`PUT https://stagingapi.arcadehub.co/v2/traits`

### URL Parameters
Parameter | Description | Type
--------- | ----------- | ----
trait | Value of new trait to select | String (see above)
