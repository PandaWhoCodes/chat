
# Simple chat
Creating a chat for the practice of new tools / languages ​​/ technologies.

## Architecture
The architecture of any project is described in 4 sections:

## Domain model
### Chat
The chat contains all the messages (sorted by time) sent to it. The main function of the chat (and the entire application at the moment) is the ability to take in a message, and see all the chat participants about this event by displaying the message.

### User

 - This user has only a name. 
 - In future: Different meta info (ava, age,
   gender, faith, etc.) 
  - access policy chat role (admin, moder, silent)

### Message
The message has its own text, the date of sending and a link to the user who sent it. In future:

 - may be edited 
 - can be removed 
 - can be forwarded 
 - reply to the message may be received

## Components
### Server API
A set of endpoints to interact with the main system. The usual api, which provides clients with access to the functionality of the application

### Comet server (messenger)
Must be able to push messages into the channels, without a user request, for real time left

### Clients to api
Any software capable of tugging server API methods. You also need to subscribe to channels for delivering realTime messages.

### Chat cursor
It is necessary to store the id of the last message read for each of the chats where there is a user in order to share the read and unreadable messages in the interface

### Persistent Storage
Database for permanent storage of chats, messages and users. Needed for prosotra history and potentially, search. The main problem (regardless of the selected database) is to provide a convenient scaling mechanism (coordinator of sharding), with a potentially huge number of users, chats and messages.

### Cache
The cache will be needed at least to store the user's chat list, and the last message for each of the chats (to get the left column in the format as VC or telegram) You can also store a cursor in the cache

## Component Interaction
### Client <=> Server
* Get chat list for user
* Get a certain number of messages from the chat, starting with a certain
* send a message
* Read all messages from chat to a specific
* I will not particularly bother with the API format, that is, I don’t set goals to achieve RestFull or the canonical compatibility of a specific jRpc version.

### Customer <=> Comet
Here you just need to subscribe to certain channels. There will be a channel for each of the chats.

## Specific Technologies
* persistenceDB - Mysql Here I will simply use what I know.
* messageDelivery - Centrifugo? firebase? With firebase there was an experience and centrifugo has long wanted to try + many positive reviews in terms of perfomance and high availibility
* server language - golang Just for a long time I wanted to try it with some kind of project with an existing TK, and not with exercises in interactive tutorials
* inMemoryDb - Redis Ideal for storing the chat list with the latest message (id and text)
* client - vue From the front, everything is sad for me, I heard that vue is pretty simple, and I will test it.
## Roadmap
This repo is very crude and curved. Many improvements are needed:

### Server part:

* Normal JWT and Authorization
* Googling good practices config applications Golang (ENV ??)
* Think about when to close connections to the database (after each request, or in the phase of completion of processing the http request)
* Scan to 1 command (docker compose? Cubernetes?)
* check everywhere that all errors are correctly processed, to think about how to replace err! = nil
* security audit
* write tests
* db resolver (shards chats and users)
* screw fashion collection logs (kibana)

### Client part:

* deal with normal asynchronous zaprosoami api for calling commands with async = false is sad
* to refuse jkveri (yuza as a matter of fact only for Ajax)
* do not load old messages 10 times in case they run out
it is normal to detect the read, now the read messages are those that were in the open chat, and you need to do those that came to the user's attention
* mark unread in the chat window (now it’s just the opposite of the chat name)
* make sure vue protects against xss
* author's name
* draw the sent message before it goes to the server, and handle success / error
* align date in the right column

## How to deploy
* Copy the repo 
* Restoring DB from chat.sql
* Run centrifugo on the server
* We configure nginx on the web folder ()
* Rename example-conf.json to conf.json and fill it in correctly
* go run main.go
* If you need to change hardcodes in js code

