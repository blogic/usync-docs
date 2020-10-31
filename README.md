# uSync - manage your OpenWrt devices from your cloud

uSync is a cloud management stack for OpenWrt devices. The primary design concept is to keep it simple. Rather than building a monolithic behemoth, uSync is a collection of small tools providing the primitive building blocks for the stack.

uSync uses websockets for communication. The protocol is JSON based and only knows very few attributes.

| attr   | description                          |
|--------|--------------------------------------|
| serial | the serial number of the device      |
| uuid   | the UUID of the AP configuration     |
| state  | the UUID of the AP state             |
| active | the UUID of the active configuration |
| error  | indicate if an error happened        |

All UUIDs are linux timestamps.

## The Protocol

## Building Blocks

As previously mentioned uSync uses primitive building blocks. Lets meet them. We will leave out usyncd (the cloud server) and talk about it later on.

#### usync

This is the websocket client running on the AP. It will open up a connection to the server and start talking the uSync dialog by sending a heartbeat. The heartbeat is nothing more than a message including the serial and UUID (and active if we are running on an older configuration).

#### utpl

This is the OpenWrt template generator written using the lemon lexer. It allows us to render the config we received from the cloud into uci batch files using a JS like syntax and a Tomcat like pattern. Rendering the the whole wireless configuration is possible with <50 lines of tpl code.

#### usync-jsonschema

This is the json schema validator. We use this for validation of incoming cfg JSONs, prior to actually trying to apply them. 

#### usync-capability

This tool does an initial probe of /etc/board.json and nl80211 to figure out what the OpenWrt device is actually capable of. This information is used for two purposes.

* utpl uses the info to map various HW capabilites to datamodel properties (mapping wifi band to phy, ...)
* Send the capabilities up to your cloud. This allows your UI to know what it is presenting.

#### usync-schema

This is the JSON schema definition. It defines the possible properties and their validation currently supported by the datamodel.

## The Repos

* [usync-client](https://github.com/blogic/usync-client)
* [usync-schema](https://github.com/blogic/usync-schema)
* [usync-jsonschema](https://github.com/blogic/usync-jsonschema)
* [usync-py-ws](https://github.com/blogic/usync-py-ws)
