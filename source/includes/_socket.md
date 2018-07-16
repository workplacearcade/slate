# Chat Socket
Workplace Arcade utilises [socket.io](https://socket.io/) to handle real-time communication. This part of the documentation will brielfy outline which messaging related events have been implemented. Do note that some events will be broadcast from the server and some will need to be broadcast to the server.

## Chat Sessions
This event is sent from the Rails API server to the socket server when the `chat_sessions` create endpoint is hit. The socket server will then subscrive users to all relevant conversations

## Chat Join
This event is sent from the Rails API server to the socket server when someone joins a conversation. Users who are already in the joined conversation will receive a payload containing the conversation ID and the ID of the user who has just joined. Seperate to this workflow a message will also be sent to the conversation alerting the participants that a user has joined.

It is recommended that you use the `chat_join` event to update online counts for the users

## Chat Leave
Similar to the chat join event, this event is broadcast from the Rails API to the socket server. An identical payload will be sent to participants of the conversation the user has just left.

## Message
This event is sent from the Rails API when a new message is created. It will be broadcast to all users in the relevant conversation and has a payload containing the conversation ID and the JSON serialized message.

## Conversation Update
This event is sent from the Rails API when a conversation is updated. It will contain the conversation ID and information about what has changed

## Conversation Create
This event is sent from the Rails API when a conversation has been created. It will be broadcast to the users who are part of the new conversation and it is recommended that you use this event to update the conversations list to show the newly created conversation.

## Star Message
This event is sent from the Rails API when a message has been starred. Starring a message means announcing it to all users in the conversation. We currently use this event to trigger a UI update (highlighting the starred message).

## New
This event is sent from the front-end when a new socket connection is established. It tells the socket server that a particular user is online; information which is broadcast to all other users.

## User Reconnect
This event is fired from the front-end when an offline user successfully reconnects. We currently have logic in the Workplace Arcade frontend that attempts reconnection if the user is offline.

## Disconnect
This event is fired from the front-end when a user goes offline. We consider 'going offline' to be closing the application.

## Focus
This event is sent by the front-end when a user selects a particular conversation. The socket server will respond with a `status` event that contains information about which of the conversation users are online.

## Typing
This event is sent by the front-end when a user is typing in a conversation. We suggest sending this event every 50ms while a user is typing. This event is also received by the front-end and is used to construct the "James Mclaren is typing..." text that a user sees when viewing a conversation.

## Online users
The conversation workflow depends on knowing which users are online for notification purposes. We currently handle this by keeping track of online users on the front-end (read focus event section). That being said, we also have the capability to use the backend for this purpose. We're happy to go down whichever path you choose here.
