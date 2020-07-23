# API documentation for updating server data in GS4u monitoring

##### URL: https://api.gs4u.net/
Commands must be sent in the "cmd" variable, example: https://api.gs4u.net/?cmd=getVersion

#### The following commands are available:
* "**getVersion**" - gives the current version of the API. You can use it to check if the API is available.
  * No parameters are needed.
  * The GET request could look like this ```https://api.gs4u.net/?cmd=getVersion```
* "**getCoolDown**" - gives time in seconds, how often you can use the command sent in the parameter.
  * Parameters:
    * "**data**" - JSON object of the following structure: {"cmd": "updateServer"}. In this example it is requested, 
      how often you can use the "updateServer" command.
    * The GET request will look like this ```https://api.gs4u.net/?cmd=getCoolDown&data={"cmd":"updateServer"}```
* "**updateServer**" - update server information in monitoring. You need to send server information signed with a token. 
  * Parameters:
    * The "**data**" variable sends server data in JSON format. Below there is an example of such object.
    * In the variable "**s**", the signature/hash of data with token is sent. Below you can read about how to create a signature.
    * OPTIONAL "**hash**" to use another hashing method.
  * The GET request could look like this ``COPY8{"n": "My Server"...}&s=w3e5g...56``.
  * BUT DO NOT USE the GET request because the SERVER data will not fit in it! Use POST!

## How to create a hash (signature)
In the monitoring you can generate a token for your server. 
It is needed in order to create a signature / hash.
The signature is constructed as follows: ```md5(**data** + token)```
That is, an MD5 hash is created from a string that includes data and the token.
###### Example
if ```data = 'ABC'```, and ```token = '123'```, then the hash is ```s = md5('ABC123')```

#### Alternative for MD5
If MD5 doesn't suit you for some reason, you can also pass the variable **hash** and specify the hashing method.
The following hash methods are currently supported:
* md5
* sha1

### Server data JSON FORMAT:
```json
{
 "n":"Server name",
 "m":"Map",
 "pc":"Number of players currently playing",
 "pm":"Number of slots for players",
 "e":{
  "password":"1 if you need a password to enter the server, 0 if not",
  "game":"the code name of the game, for CS: GO it's 'csgo'",
  ... other additional server information is possible ...
 },
 "tabs":
 [
  {
   "name":"Name for Tab 1",
   "data":[
    {
     "name":"Name for Group 1",
     "data":[
      {
       "name":"Name for Item 1",
       "description":"Description for Item 1"
      },
      {
       "name":"Name for Item 2",
       "description":"Description for Item 2"
      },
     ]
    },
    {
     "name":"Name for Group 2",
     "data":[
      {
       "name":"Name for Item 3",
       "description":"Description for Item 3"
      }
     ]
    }
   ]
  },
  {
   "name":"Name for Tab 2",
   "data":[
    {
     "name":"Name for Group 1",
     "data":[
      {
       "name":"Name for Item 1",
       "description":"Description for Item 1"
      }
     ]
    }
   ]
  }
 ],
 "p":
 [
  {
   "name":"Player nickname",
   "kills":"number of frags",
   "deaths":"number of deaths",
   "time":"how long has the player been on the server",
   "team":"team number",
   "lvl": "player level",
   ... other data can be added ...
  },
  {... another player ...}
 ]
}
```

# Game codes for "game"
[The codes for all games are available here](README_GAMES.md)

# ERRORS:
0 - no errors

-1000 - Token for your server has not been created.

-2001 - Token created, but for some reason empty. Contact Support.

-2002 - Token not paid.

-2003 - Digital signature verification failed, are you using the wrong token?

-3000 - You are sending too many requests.

-3001 - You sent an unknown command to "cmd".

-7002 - you sent data in JSON format with the wrong syntax.

-1011 - ServerID was not found in the data

-1001 - not sent either "data" or "s"
