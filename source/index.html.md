---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

This documentation details the endpoints that will be exposed as part of the Workplace Arcade < > RTPos integration. For stage one this includes:

* Newsfeed
* Chat
* Profile
* User Creation

# Authentication

Workplace Arcade currently uses JWT tokens to authorize individual users. This token is attached to the header of requests requring authentication in the form of:

`X-USER-TOKEN: <token>`

We are going to be be building bespoke logic for the integration with RTPos so will look to them to dictate the authentication method.

# Newsfeed

## Load activities

```shell
curl "https://api.arcadehub.co/v2/activities.json"
  -H "X-USER-TOKEN: abcd"
  -d "page=1"
```

> The above command returns JSON structured like this:

```json
{
  "activities": [
    {
      "id": 1,
      "content": "This is my content",
      "display_content": "This is displayable content",
      "user": {
        "id": 1,
        "name": "John Doe",
        "image": "<url to image>",
      },
      "type": "Activity type",
      "user_liked": "< true | false >",
      "likes": 1,
      "comments": [
        {
          "user_name": "James McLaren",
          "user_image": "Link to image",
          "user_id": 1,
          "user_can_delete": "< true | false >",
          "content": "Comment Content",
          "data_data": "Date",
          "readable_date_data": "15 minutes ago",
        }
      ],
      "image": "Link to compressed image",
      "download_image": "Link to full-size image",
      "date_data": "Date",
      "readable_date_data": "15 minutes ago",
      "edited": "< true | false >",
      "file": {
        "link": "file.url",
        "name": "file name",
        "type": "file extension",
        "date": "seconds since epoch",
        "user": "User name",
        "size": "File size",
      },
      "additional_information": {

      },
      "created_at": "Date",
      "interesting_at": "Date",
      "views": [
        {
          "username": "user fullname",
          "user_id": "user id",
          "user_image": "user profile image",
          "date_data": "seconds since epoch",
          "created_at": "15 minutes ago".
        }
      ]
    }
  ],

  "summaries": [
    {
      "counts": { "SaleActivity": 10, "BadgeActivity": 5 },
      "oldest": "Date",
      "newest": "Date"
    }
  ]
}
```

This endpoint retrieves a paginated set of activities and summaries.

### HTTP Request

`GET https://stagingapi.arcadehub.co/v2/activities.json`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
page | 0 | Which set of activities to return. When loading the next set this value should increment by 1

<aside class="success">
The current batch size of activities is 15 but this can be changed
</aside>

### Summaries
The summary object is what we use to display our activity pills in the newsfeed. In the above response object, for example, it reflects the fact that between `oldest` and `newest` there have been 10 sale activities and 5 badge activities.
Each request to the activities endpoint will return this array and it will contain all summary activities over the date ranges of the returned objects

### Interesting at
The Workplace Arcade newsfeed has the capability to be either chronological or algorithmic. For a chronological newsfeed order activities by `created_at` as this value will not change. If you would like an algorithmic newsfeed, order activities by `interesting_at`. The Workplace Arcade system will adjust this value based on interactions with the activities to ensure that the most relevant content is shown to users.

## Post an activity

```shell
curl "https://stagingapi.arcadehub.co/v3/activities"
  -H "X-USER-TOKEN: abcd"
  -X POST
  -d "content=""&display_content=""&image=""&tagged_ids=[]&hash_tags=[]"
```

> The above command returns JSON structured like this:

```json
    {
      "id": 1,
      "content": "This is my content",
      "display_content": "This is displayable content",
      "user": {
        "id": 1,
        "name": "John Doe",
        "image": "<url to image>",
      },
      "type": "Activity type",
      "user_liked": "< true | false >",
      "likes": 1,
      "comments": [
        {
          "user_name": "James McLaren",
          "user_image": "Link to image",
          "user_id": 1,
          "user_can_delete": "< true | false >",
          "content": "Comment Content",
          "data_data": "Date",
          "readable_date_data": "15 minutes ago",
        }
      ],
      "image": "Link to compressed image",
      "download_image": "Link to full-size image",
      "date_data": "Date",
      "readable_date_data": "15 minutes ago",
      "edited": "< true | false >",
      "file": {
        "link": "file.url",
        "name": "file name",
        "type": "file extension",
        "date": "seconds since epoch",
        "user": "User name",
        "size": "File size",
      },
      "additional_information": {

      },
      "created_at": "Date",
      "interesting_at": "Date",
      "views": [
        {
          "username": "user fullname",
          "user_id": "user id",
          "user_image": "user profile image",
          "date_data": "seconds since epoch",
          "created_at": "15 minutes ago".
        }
      ]
    }
```

This endpoint posts a new activity on the newsfeed.

<aside class="warning">This endpoint will post the created activity from the account of the currently authenticated user.</aside>

### HTTP Request

`POST https://stagingapi.arcadehub.co/v3/activities`

### URL Parameters

Parameter | Description
--------- | -----------
content | The HTML rich content of an activity
display_content | The displayable content of an activity. This will be used for notifications
image | A base64 string of the attached image
tagged_ids | An array of user ids tagged in the activity
hash_tags | An array of hash tags contained in the post

### Hash Tags and User Tags
See below for endpoints that return these specific values

## Post an activity with attached file

```shell
curl "https://stagingapi.arcadehub.co/v3/activities/file"
  -H "X-USER-TOKEN: abcd"
  -X POST
  -d "content=""&display_content=""&image=""&tagged_ids=[]&hash_tags=[]&file={}"
```

> The above command returns JSON structured like this:

```json
    {
      "id": 1,
      "content": "This is my content",
      "display_content": "This is displayable content",
      "user": {
        "id": 1,
        "name": "John Doe",
        "image": "<url to image>",
      },
      "type": "Activity type",
      "user_liked": "< true | false >",
      "likes": 1,
      "comments": [
        {
          "user_name": "James McLaren",
          "user_image": "Link to image",
          "user_id": 1,
          "user_can_delete": "< true | false >",
          "content": "Comment Content",
          "data_data": "Date",
          "readable_date_data": "15 minutes ago",
        }
      ],
      "image": "Link to compressed image",
      "download_image": "Link to full-size image",
      "date_data": "Date",
      "readable_date_data": "15 minutes ago",
      "edited": "< true | false >",
      "file": {
        "link": "file.url",
        "name": "file name",
        "type": "file extension",
        "date": "seconds since epoch",
        "user": "User name",
        "size": "File size",
      },
      "additional_information": {

      },
      "created_at": "Date",
      "interesting_at": "Date",
      "views": [
        {
          "username": "user fullname",
          "user_id": "user id",
          "user_image": "user profile image",
          "date_data": "seconds since epoch",
          "created_at": "15 minutes ago".
        }
      ]
    }
```

This endpoint posts a new activity on the newsfeed with an attached file.

<aside class="warning">This endpoint will post the created activity from the account of the currently authenticated user.</aside>

### HTTP Request

`POST https://stagingapi.arcadehub.co/v3/activities`

### URL Parameters

Parameter | Description
--------- | -----------
content | The HTML rich content of an activity
display_content | The displayable content of an activity. This will be used for notifications
image | A base64 string of the attached image
tagged_ids | An array of user ids tagged in the activity
hash_tags | An array of hash tags contained in the post
file | Multi-part form-data


## List hash tags
```shell
curl "https://stagingapi.arcadehub.co/v3/activities/list_tags"
  -H "X-USER-TOKEN: abcd"
```

> The above command returns JSON structured like this:

```json
{
  "tags": ["#sorrynotsorry", "#gameon"]
}
```

This endpoint returns an array of all hash tags that have previously been posted in the users current company

### HTTP Request

`GET https://stagingapi.arcadehub.co/v3/activities/list_tags`

## List taggable users
```shell
curl "https://stagingapi.arcadehub.co/v3/users/taggable"
  -H "X-USER-TOKEN: abcd"
```

> The above command returns JSON structured like this:

```json
{
  "users": [
    {
      "id": 1,
      "name": "James McLaren",
      "team_name": "The hackers",
      "image_url": "link to user image",
    }
  ]
}
```

This endpoint returns an array of users that can be tagged in an activity

### HTTP Request
`GET https://stagingapi.arcadehub.co/v3/users/taggable`

<aside class="warning">This list can be quite long for companies with no posting restriction. It is advised that you present this as a searchable/scrollable list</aside>

## Load activities by tag

```shell
curl "https://api.arcadehub.co/v2/activities.json"
  -H "X-USER-TOKEN: abcd"
  -d "tag=#sorrynotsorry"
```

> The above command returns JSON structured like this:

```json
{
  "activities": [
    {
      "id": 1,
      "content": "This is my content",
      "display_content": "This is displayable content",
      "user": {
        "id": 1,
        "name": "John Doe",
        "image": "<url to image>",
      },
      "type": "Activity type",
      "user_liked": "< true | false >",
      "likes": 1,
      "comments": [
        {
          "user_name": "James McLaren",
          "user_image": "Link to image",
          "user_id": 1,
          "user_can_delete": "< true | false >",
          "content": "Comment Content",
          "data_data": "Date",
          "readable_date_data": "15 minutes ago",
        }
      ],
      "image": "Link to compressed image",
      "download_image": "Link to full-size image",
      "date_data": "Date",
      "readable_date_data": "15 minutes ago",
      "edited": "< true | false >",
      "file": {
        "link": "file.url",
        "name": "file name",
        "type": "file extension",
        "date": "seconds since epoch",
        "user": "User name",
        "size": "File size",
      },
      "additional_information": {

      },
      "created_at": "Date",
      "interesting_at": "Date",
      "views": [
        {
          "username": "user fullname",
          "user_id": "user id",
          "user_image": "user profile image",
          "date_data": "seconds since epoch",
          "created_at": "15 minutes ago"
        }
      ]
    }
  ]
}
```

This endpoint returns an array of activities that contain the provided hash tag directly or have a comment on them that contains the hash tag

### HTTP Request

`GET https://stagingapi.arcadehub.co/v3/activities/by_tag`

### Query Parameters

Parameter | Description
--------- | -----------
tag | The tag you would like activities returned for

## Edit an activity

```shell
curl "https://stagingapi.arcadehub.co/v3/activities/:id"
  -H "X-USER-TOKEN: abcd"
  -X PUT
  -d "content=""&display_content=""&image=""&tagged_ids=[]&hash_tags=[]"
```

> The above command returns JSON structured like this:

```json
    {
      "id": 1,
      "content": "This is my content",
      "display_content": "This is displayable content",
      "user": {
        "id": 1,
        "name": "John Doe",
        "image": "<url to image>",
      },
      "type": "Activity type",
      "user_liked": "< true | false >",
      "likes": 1,
      "comments": [
        {
          "user_name": "James McLaren",
          "user_image": "Link to image",
          "user_id": 1,
          "user_can_delete": "< true | false >",
          "content": "Comment Content",
          "data_data": "Date",
          "readable_date_data": "15 minutes ago",
        }
      ],
      "image": "Link to compressed image",
      "download_image": "Link to full-size image",
      "date_data": "Date",
      "readable_date_data": "15 minutes ago",
      "edited": "< true | false >",
      "file": {
        "link": "file.url",
        "name": "file name",
        "type": "file extension",
        "date": "seconds since epoch",
        "user": "User name",
        "size": "File size",
      },
      "additional_information": {

      },
      "created_at": "Date",
      "interesting_at": "Date",
      "views": [
        {
          "username": "user fullname",
          "user_id": "user id",
          "user_image": "user profile image",
          "date_data": "seconds since epoch",
          "created_at": "15 minutes ago".
        }
      ]
    }
```

This endpoint updates an existing activity

<aside class="warning">This endpoint is only relevant for activities without files. If an activity with a file is edited using this endpoint then the file will be removed.</aside>

### HTTP Request

`PUT https://stagingapi.arcadehub.co/v3/activities/:id`

### URL Parameters

Parameter | Description
--------- | -----------
content | The HTML rich content of an activity
display_content | The displayable content of an activity. This will be used for notifications
image | A base64 string of the attached image
tagged_ids | An array of user ids tagged in the activity
hash_tags | An array of hash tags contained in the post

## Edit an activity with file

```shell
curl "https://stagingapi.arcadehub.co/v3/activities/file/:id"
  -H "X-USER-TOKEN: abcd"
  -X POST
  -d "content=""&display_content=""&image=""&tagged_ids=[]&hash_tags=[]&file=[]"
```

> The above command returns JSON structured like this:

```json
    {
      "id": 1,
      "content": "This is my content",
      "display_content": "This is displayable content",
      "user": {
        "id": 1,
        "name": "John Doe",
        "image": "<url to image>",
      },
      "type": "Activity type",
      "user_liked": "< true | false >",
      "likes": 1,
      "comments": [
        {
          "user_name": "James McLaren",
          "user_image": "Link to image",
          "user_id": 1,
          "user_can_delete": "< true | false >",
          "content": "Comment Content",
          "data_data": "Date",
          "readable_date_data": "15 minutes ago",
        }
      ],
      "image": "Link to compressed image",
      "download_image": "Link to full-size image",
      "date_data": "Date",
      "readable_date_data": "15 minutes ago",
      "edited": "< true | false >",
      "file": {
        "link": "file.url",
        "name": "file name",
        "type": "file extension",
        "date": "seconds since epoch",
        "user": "User name",
        "size": "File size",
      },
      "additional_information": {

      },
      "created_at": "Date",
      "interesting_at": "Date",
      "views": [
        {
          "username": "user fullname",
          "user_id": "user id",
          "user_image": "user profile image",
          "date_data": "seconds since epoch",
          "created_at": "15 minutes ago".
        }
      ]
    }
```

This endpoint updates an existing activity

<aside class="warning">This endpoint is only relevant for activities with files. If an activity with an image is edited using this endpoint then the image will be removed. Also note that currently Workplace Arcade does not support activities with both files and images</aside>

### HTTP Request

`PUT https://stagingapi.arcadehub.co/v3/activities/:id`

### URL Parameters

Parameter | Description
--------- | -----------
content | The HTML rich content of an activity
display_content | The displayable content of an activity. This will be used for notifications
image | A base64 string of the attached image
tagged_ids | An array of user ids tagged in the activity
hash_tags | An array of hash tags contained in the post
file | multi-part form data

## Deleting an activity
```shell
curl "https://stagingapi.arcadehub.co/v1/activities/:id"
  -H "X-USER-TOKEN: abcd"
  -X DELETE
```

> The above command returns JSON structured like this:

```json
  { "message": "Your activity has been deleted" }
```

This endpoint deletes a posted activity.
<aside class="warning">Activities can only be deleted by users that posted them or account managers. Any other user attempting to do so will cause the system to return an error</aside>

### HTTP Request

`DELETE https://stagingapi.arcadehub.co/v1/activities/:id`

## Like Activity
```shell
curl "https://stagingapi.arcadehub.co/v2/activities/:id/like"
  -H "X-USER-TOKEN: abcd"
  -X PUT
```

> The above command returns JSON structured like this if the activity has been liked:

```json
  {
    "username": "James McLaren",
    "user_image": "Link to profile image",
    "user_id": 1
  }
```

> The above command returns JSON structured like this if the activity has been unliked:

```json
  {
    "message": "Activity unliked",
    "user_id": 1361
  }
```

This endpoint can be used to both like and unlike activities. As per the information above the response object will be different based on whether the activity has been liked or unliked

### HTTP Request
`PUT https://stagingapi.arcadehub.co/v2/activities/:id/like`

## Show Activity Likes
```shell
curl "https://stagingapi.arcadehub.co/v2/activities/:id/likes"
  -H "X-USER-TOKEN: abcd"
```

> The above command returns JSON structured like this if the activity has been liked:

```json
  {
    "likes": [
     {
        "user_id": 1,
        "name": "James McLaren",
        "image": "Link to user profile image",
        "badge": "Link to user badge image",
        "created_at": "15 minutes ago"
      }
    ]
  }
```

This endpoint returns an array of likes for a single activity

### HTTP Request
`GET https://stagingapi.arcadehub.co/v2/activities/:id/likes`

## Show Activity Views
```shell
curl "https://stagingapi.arcadehub.co/v2/activities/:id/views"
  -H "X-USER-TOKEN: abcd"
```

> The above command returns JSON structured like this if the activity has been liked:

```json
  {
    "likes": [
     {
      "username": "James McLaren",
      "user_id": 1,
      "user_image": "Link to user profile image",
      "date_data": "seconds since epoch",
      "created_at": "15 minutes ago",
     }
    ]
  }
```

This endpoint returns an array of everyone that has viewed an activity

### HTTP Request
`GET https://stagingapi.arcadehub.co/v2/activities/:id/views`
