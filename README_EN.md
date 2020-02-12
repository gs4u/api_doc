# API documentation for updating server data in GS4u monitoring

##### URL: https://supd.gs4u.net/
Commands must be sent in the "cmd" variable, example: https://supd.gs4u.net/?cmd=getVersion

#### The following commands are available:
* "**getCoolDown**" - gives the time in seconds how often you can send information about the server. 
That is, exactly so many seconds should elapse between the sending of information.
* "**getVersion**" - gives the current version of the plugin, which is recommended for use.

Without a command, you need to send server information signed by a token.
* In the variable "**data**" you need to send server data in JSON format.
* In the variable "**s**" it is necessary to send a hash of data and token.
* OPTIONAL "**hash**" to use another hash method.

## How to create a hash (signature / signature)
In the monitoring you can generate a token for your server. 
It is needed in order to create a signature / hash.
The signature is constructed as follows: ```md5(**data** + token)```
That is, an MD5 hash is created from a string that includes data and the token.
###### Например
if ```data = 'ABC'```, and ```token = '123''```, then the hash is ```s = md5('ABC123')```

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
