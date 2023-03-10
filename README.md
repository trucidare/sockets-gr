## PR to grain StdLib: https://github.com/grain-lang/grain/pull/1590

# sockets-gr

A simple sock_* implementation for wasi runtimes using grain
  
## How to use

```javascript
import Net from "../server"

let content = "HTTP/1.1 200 OK
Date: Sun, 18 Oct 2012 10:36:20 GMT
Server: Grain Server
Content-Length: 77
Connection: Closed
Content-Type: text/html; charset=iso-8859-1

<html><head><title>Index</title></head><body><h3>It works!</h3></body></html>"

/**
 * Construct a server (internally the fd number (0) will be added by 3) 
 */
let server = Net.new(0, 5)

/**
 * Set the connected callback to the instance
 */
server.onConnected(
    (client) => {
        print("Client connected")
        void
    }
)

/**
 * Set the disconnected callback to the instance
 */

server.onDisconnected(
    (client) => {
        print("Client disconnected")
        void
    }
)

/**
 * Set the receive callback to the instance
 */
server.onReceive(
    (client, message) => {
        print("Client want: \r\n"++ message)
        server.send(client, content)
        void
    }
)

/**
 * Start the server 
 */
server.start()

while (true) {
    server.accecptClient();
}

```

Run the following commands to start the server

```bash
grain compile simpleServer.gr
wasmtime --tcplisten localhost:5000 --env a=b simpleServer.gr.wasm 
```

Open a browser and point it to http://localhost:5000/

