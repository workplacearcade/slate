# Messaging

## Creating a chat session
```shell
curl "https://stagingapi.arcadehub.co/v2/chat_sessions"
  -H "X-USER-TOKEN: abcd"
  -X POST
```
> This will return a JSON object of the structure

```json
  {
    "token": "User chat token"
  }
```

This endpoint creates a new chat session for the current user. It returns a chat token which must be supplied for all subsequent chat request. It also notifies the socket server that the current user has joined

### HTTP Request

`POST https://stagingapi.arcadehub.co/v2/conversations/:id/messages`

## Loading Conversations
```shell
curl "https://stagingapi.arcadehub.co/v2/conversations"
  -H "X-USER-TOKEN: abcd"
```

> This will return a JSON object of the structure

```json
  {
    "conversations": [
      {
        "id": 1,
        "name": "Conversation name",
        "group": "Is conversation a group conversation? < true | false >",
        "image": "Link to conversation image",
        "created_at": "Date",
        "last_message": {
          "created_at": "Date",
        },
        "color": "Hex color code",
        "user_ids": [1, 2, 3],
        "unread_count": 1,
        "user_names": "James McLaren and John Doe",
        "raw_names": ["James McLaren", "John Doe"],
        "current_user_created": "< true | false >",
        "user_can_pin": "< true | false >",
        "current_user_in_conversation": "< true | false >",
        "users": [
          {
            "id": 1,
            "user_image": "Link to user image",
            "username": "James McLaren"
          }
        ],
        "muted": "< true | false >",
        "type": "< null | Channel | GroupConversation >",
        "sender_color": "Hex color code"

      }
    ]
  }
```

This endpoint returns all conversations in which the current user is involved.

### HTTP Request

`GET https://stagingapi.arcadehub.co/v2/conversations`

## Loading Channels
```shell
curl "https://stagingapi.arcadehub.co/v2/channels"
  -H "X-USER-TOKEN: abcd"
```

> This will return a JSON object of the structure

```json
  {
    "channels": [
      {
        "id": 1,
        "name": "Conversation name",
        "group": "Is conversation a group conversation? < true | false >",
        "image": "Link to conversation image",
        "created_at": "Date",
        "last_message": {
          "created_at": "Date",
        },
        "color": "Hex color code",
        "user_ids": [1, 2, 3],
        "unread_count": 1,
        "user_names": "James McLaren and John Doe",
        "raw_names": ["James McLaren", "John Doe"],
        "current_user_created": "< true | false >",
        "user_can_pin": "< true | false >",
        "current_user_in_conversation": "< true | false >",
        "users": [
          {
            "id": 1,
            "user_image": "Link to user image",
            "username": "James McLaren"
          }
        ],
        "muted": "< true | false >",
        "type": "< null | Channel | GroupConversation >",
        "sender_color": "Hex color code",
        "description": "This is a channel",
        "group_text": "TIAC"

      }
    ]
  }
```

This endpoint returns all channels that the current user has joined.

### HTTP Request

`GET https://stagingapi.arcadehub.co/v2/channels`

## Loading Available Channels
```shell
curl "https://stagingapi.arcadehub.co/v2/available_channels"
  -H "X-USER-TOKEN: abcd"
```

> This will return a JSON object of the structure

```json
  {
    "channels": [
      {
        "id": 1,
        "name": "Conversation name",
        "group": "Is conversation a group conversation? < true | false >",
        "image": "Link to conversation image",
        "created_at": "Date",
        "last_message": {
          "created_at": "Date",
        },
        "color": "Hex color code",
        "user_ids": [1, 2, 3],
        "unread_count": 1,
        "user_names": "James McLaren and John Doe",
        "raw_names": ["James McLaren", "John Doe"],
        "current_user_created": "< true | false >",
        "user_can_pin": "< true | false >",
        "current_user_in_conversation": "< true | false >",
        "users": [
          {
            "id": 1,
            "user_image": "Link to user image",
            "username": "James McLaren"
          }
        ],
        "muted": "< true | false >",
        "type": "< null | Channel | GroupConversation >",
        "sender_color": "Hex color code",
        "description": "This is a channel",
        "group_text": "TIAC"

      }
    ]
  }
```

This endpoint returns all channels that the current user can joined.

### HTTP Request

`POST https://stagingapi.arcadehub.co/v2/available_channels`

## Joining a channel
```shell
curl "https://stagingapi.arcadehub.co/v2/channels/:id/join"
  -H "X-USER-TOKEN: abcd"
  -X PUT
```

> This will return a JSON object of the structure

```json
  {
    "channels": [
      {
        "id": 1,
        "name": "Conversation name",
        "group": "Is conversation a group conversation? < true | false >",
        "image": "Link to conversation image",
        "created_at": "Date",
        "last_message": {
          "created_at": "Date",
        },
        "color": "Hex color code",
        "user_ids": [1, 2, 3],
        "unread_count": 1,
        "user_names": "James McLaren and John Doe",
        "raw_names": ["James McLaren", "John Doe"],
        "current_user_created": "< true | false >",
        "user_can_pin": "< true | false >",
        "current_user_in_conversation": "< true | false >",
        "users": [
          {
            "id": 1,
            "user_image": "Link to user image",
            "username": "James McLaren"
          }
        ],
        "muted": "< true | false >",
        "type": "< null | Channel | GroupConversation >",
        "sender_color": "Hex color code",
        "description": "This is a channel",
        "group_text": "TIAC"

      }
    ]
  }
```

This endpoint will join the current user to the specified channel. This will broadcast the joining event to all users in the conversation.

<aside class="warning">
The backend will ensure that the current user has permission to join the current channel (eg it is a channel created in the user's company). If the user does not have permission a 401 will be returned
</aside>

### HTTP Request

`PUT https://stagingapi.arcadehub.co/v2/channels/:id/join`

## Leaving a channel
```shell
curl "https://stagingapi.arcadehub.co/v2/channels/:id/leave"
  -H "X-USER-TOKEN: abcd"
  -X PUT
```

> This will return a JSON object of the structure

```json
  {
    "message": "You have left the conversation"
  }
```

This endpoint will remove the current user to the specified channel. This will broadcast the joining event to all users in the conversation.

### HTTP Request

`PUT https://stagingapi.arcadehub.co/v2/channels/:id/leave`

## Creating a conversation
```shell
curl "https://stagingapi.arcadehub.co/v2/conversations"
  -H "X-USER-TOKEN: abcd"
  -X POST
  -d "name=""&image=""&user_ids="""
```

> This will return a JSON object of the structure

```json
  {
    "conversation": {
      "id": 1,
      "name": "Conversation name",
      "group": "Is conversation a group conversation? < true | false >",
      "image": "Link to conversation image",
      "created_at": "Date",
      "last_message": {
        "created_at": "Date",
      },
      "color": "Hex color code",
      "user_ids": [1, 2, 3],
      "unread_count": 1,
      "user_names": "James McLaren and John Doe",
      "raw_names": ["James McLaren", "John Doe"],
      "current_user_created": "< true | false >",
      "user_can_pin": "< true | false >",
      "current_user_in_conversation": "< true | false >",
      "users": [
        {
          "id": 1,
          "user_image": "Link to user image",
          "username": "James McLaren"
        }
      ],
      "muted": "< true | false >",
      "type": "< null | Channel | GroupConversation >",
      "sender_color": "Hex color code"

    }
  }
```

This endpoint creates a new conversation in the Arcade system. This endpoint is used for creating standard conversations (eg not channels) and accepts an array of user IDs to add to the conversation. Subsequent users can be added at a later date

### HTTP Request

`POST https://stagingapi.arcadehub.co/v2/conversations`

### URL Parameters

Parameter | Description | Type
--------- | ----------- | ----
name | Conversation name | String
image | Conversation Image | Base 64 String
user_ids | IDs of users to add to conversation | array of IDs


## Creating a channel
```shell
curl "https://stagingapi.arcadehub.co/v2/channels"
  -H "X-USER-TOKEN: abcd"
  -X POST
  -d "name=""&image=""&user_ids=""&description="""
```

> This will return a JSON object of the structure

```json
  {
    "conversation": {
      "id": 1,
      "name": "Conversation name",
      "group": "Is conversation a group conversation? < true | false >",
      "image": "Link to conversation image",
      "created_at": "Date",
      "last_message": {
        "created_at": "Date",
      },
      "color": "Hex color code",
      "user_ids": [1, 2, 3],
      "unread_count": 1,
      "user_names": "James McLaren and John Doe",
      "raw_names": ["James McLaren", "John Doe"],
      "current_user_created": "< true | false >",
      "user_can_pin": "< true | false >",
      "current_user_in_conversation": "< true | false >",
      "users": [
        {
          "id": 1,
          "user_image": "Link to user image",
          "username": "James McLaren"
        }
      ],
      "muted": "< true | false >",
      "type": "< null | Channel | GroupConversation >",
      "sender_color": "Hex color code",
      "description": "This is a channel",
      "group_text": "TIAC"

    }
  }
```

This endpoint creates a new conversation in the Arcade system. This endpoint is used for creating standard conversations (eg not channels) and accepts an array of user IDs to add to the conversation. Subsequent users can be added at a later date

The Group Text value will return an acronym of the channel name

### HTTP Request

`POST https://stagingapi.arcadehub.co/v2/conversations`

### URL Parameters
Parameter | Description | Type
--------- | ----------- | ----
name | Conversation name | String
image | Conversation Image | Base 64 String
user_ids | IDs of users to add to conversation | array of IDs
description | Channel description to be show to prospective channel members | String

## Inviting users to a conversation
```shell
curl "https://stagingapi.arcadehub.co/v2/chat_sessions/invite"
  -H "X-USER-TOKEN: abcd"
  -X PUT
  -d "conversation_id=""&user_ids="""
```

> This will return a JSON object of the structure

```json
  {
    "invited": 12,
    "message": "James and Steve have been added to the chat"
  }
```

This endpoint can be used to invite new users to a conversation (channel or conversation). It will also broadcast the invite event to our socket server and immediately subscribe them to the corresponding conversation

### HTTP Request

`POST https://stagingapi.arcadehub.co/v2/chat_sessions/invite`

### URL Parameters
Parameter | Description | Type
--------- | ----------- | ----
conversation_id | ID of conversation to add users to | ID
user_ids | IDs of users to add to conversation | array of IDs

## Leaving a conversation
```shell
curl "https://stagingapi.arcadehub.co/v2/chat_sessions/leave"
  -H "X-USER-TOKEN: abcd"
  -X PUT
  -d "conversation_id="""
```

> This will return a JSON object of the structure

```json
  { }
```

This endpoint removes the current user from the specified conversation. It will broadcast the leave event to the chat server and alert all users in the chat that the current user has left.

### HTTP Request

`POST https://stagingapi.arcadehub.co/v2/chat_sessions/leave`

### URL Parameters
Parameter | Description | Type
--------- | ----------- | ----
conversation_id | ID of conversation to leave | ID

## Sending a Message
```shell
curl "https://stagingapi.arcadehub.co/v2/conversations/:id/messages"
  -H "X-USER-TOKEN: abcd"
  -X POST
  -d "message[content]=""&message[display_content]=""&message[image]=""&message[uuid]=""
  &message[system]=""&message[offline_users]=""&message[tagged_users]="""
```
> This will return a JSON object of the structure

```json
  {
    "message": {
      "id": 1,
      "content": "Content of the message",
      "display_content": "Display Content of the message",
      "is_reported": "Has someone reported the message? < true | false >",
      "reported_at": "Date message was reported",
      "sender": {
        "id": 1,
        "name": "James McLaren",
        "profile_image": "Link to user image",
        "color": "User's sender color",
      },
      "starred": "Whether or not a user has announced the message",
      "created_at": "Date",
      "uuid": "Unique identifier for the message",
      "group_number": "Which group of messages this message belongs to",
      "system": "Whether or not the message is a system message",
      "tagged_users": [
        {
          "id": 1,
          "name": "James McLaren"
        }
      ],
      "image": "Link to image",
      "file": {
        "link": "Link to file",
        "name": "File name",
        "type": "File extension",
        "date": "Seconds since epoch",
        "user": "James McLaren",
        "size": "File size"
      },
      "conversation_id": 1
    }
  }
```

This endpoint creates a new message in the specified conversation. It will broadcast this new message to our socket server and notify all online users in the conversation

### HTTP Request

`POST https://stagingapi.arcadehub.co/v2/conversations/:id/messages`

### URL Parameters
Parameter | Description | Type
--------- | ----------- | ----
content | HTML rich content of the message | String
display_content | Message content to be used in notifications | String
image | Attached message image | Base 64 String
uuid | Unique identifier of message | String
system | Whether or not the message is a system message | Boolean
offline_users | Users who are not online at time of sending. These users will be push notified | Array of IDS
tagged_users | Users who are tagged in the message. These users will be push notified | Array of IDs

## Sending a Message with attached file
```shell
curl "https://stagingapi.arcadehub.co/v2/conversations/:id/file"
  -H "X-USER-TOKEN: abcd"
  -X POST
  -d "message[content]=""&message[display_content]=""&message[file]=""&message[uuid]=""
  &message[system]=""&message[offline_users]=""&message[tagged_users]="""
```
> This will return a JSON object of the structure

```json
  {
    "message": {
      "id": 1,
      "content": "Content of the message",
      "display_content": "Display Content of the message",
      "is_reported": "Has someone reported the message? < true | false >",
      "reported_at": "Date message was reported",
      "sender": {
        "id": 1,
        "name": "James McLaren",
        "profile_image": "Link to user image",
        "color": "User's sender color",
      },
      "starred": "Whether or not a user has announced the message",
      "created_at": "Date",
      "uuid": "Unique identifier for the message",
      "group_number": "Which group of messages this message belongs to",
      "system": "Whether or not the message is a system message",
      "tagged_users": [
        {
          "id": 1,
          "name": "James McLaren"
        }
      ],
      "image": "Link to image",
      "file": {
        "link": "Link to file",
        "name": "File name",
        "type": "File extension",
        "date": "Seconds since epoch",
        "user": "James McLaren",
        "size": "File size"
      },
      "conversation_id": 1
    }
  }
```

This endpoint creates a new message in the specified conversation. It will broadcast this new message to our socket server and notify all online users in the conversation

### HTTP Request

`POST https://stagingapi.arcadehub.co/v2/conversations/:id/messages`

### URL Parameters
Parameter | Description | Type
--------- | ----------- | ----
content | HTML rich content of the message | String
display_content | Message content to be used in notifications | String
file | Attached file image | Multi-part form
uuid | Unique identifier of message | String
system | Whether or not the message is a system message | Boolean
offline_users | Users who are not online at time of sending. These users will be push notified | Array of IDS
tagged_users | Users who are tagged in the message. These users will be push notified | Array of IDs

## Loading Messages in a Conversation
```shell
curl "https://stagingapi.arcadehub.co/v2/conversations/:id/messages"
  -H "X-USER-TOKEN: abcd"
  -D "skip_count=""&return_count="""
```

> This will return a JSON object of the structure

```json
  {
    "messages": [
      {
        "id": 1,
        "content": "Content of the message",
        "display_content": "Display Content of the message",
        "is_reported": "Has someone reported the message? < true | false >",
        "reported_at": "Date message was reported",
        "sender": {
          "id": 1,
          "name": "James McLaren",
          "profile_image": "Link to user image",
          "color": "User's sender color",
        },
        "starred": "Whether or not a user has announced the message",
        "created_at": "Date",
        "uuid": "Unique identifier for the message",
        "group_number": "Which group of messages this message belongs to",
        "system": "Whether or not the message is a system message",
        "tagged_users": [
          {
            "id": 1,
            "name": "James McLaren"
          }
        ],
        "image": "Link to image",
        "file": {
          "link": "Link to file",
          "name": "File name",
          "type": "File extension",
          "date": "Seconds since epoch",
          "user": "James McLaren",
          "size": "File size"
        },
        "conversation_id": 1
      }
    ]
  }
```

This endpoint returns the specified range of messages for a particular conversation. It is designed to return the messages in a paginated fashion which can be controlled using the `skip_count` and `return_count`

<aside class="success">
This endpoint works for all types of conversations
</aside>



### HTTP Request

`GET https://stagingapi.arcadehub.co/v2/conversations/:id/messages`

### URL Parameters
Parameter | Description | Type
--------- | ----------- | ----
skip_count | The number of messages in the conversation to ignore | Integer (default 0)
return_count | The number of messages to return in the batch | Integer (default 30)

## Muting a conversation
```shell
curl "https://stagingapi.arcadehub.co/v2/notifications/mute"
  -H "X-USER-TOKEN: abcd"
  -X PUT
  -D "conversation_id="""
```

> This will return a JSON object of the structure

```json
  { "muted": "Current mute status" }
```

This endpoint allows users to toggle their mute status for a particular conversation. If a user mutes a conversation they will no longer receive notifications for it.

### URL Parameters
Parameter | Description | Type
--------- | ----------- | ----
conversation_id | ID of Conversation to mute | ID
