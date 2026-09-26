
| Approach                     | Connection behavior                           | Direction         |
| ---------------------------- | --------------------------------------------- | ----------------- |
| **Polling**                  | Request → response → wait → repeat            | Server → client   |
| **Long polling**             | Request → server waits → response → reconnect | Server → client   |
| **SSE (Server-Sent Events)** | **One HTTP connection stays open**            | Server → client   |
| **WebSocket**                | One persistent connection                     | **Bidirectional** |

Long polling: The server holds the HTTP request open until it has something to return.
Once it responds, the client immediately opens another request.

Client ─── HTTP request ───────────────► Server
       waits...
       waits...
       waits...
Client ◄──────── response when data exists

Client ─── NEW HTTP request ──────────► Server
       waits...
       waits...
Client ◄──────── response when data exists

Client ─── NEW HTTP request ──────────► Server


NORMAL POLLING:

request ──► response
(wait 5 sec)
request ──► response
(wait 5 sec)
request ──► response

LONG POLLING:

request ─────────────►
        server waits...
        ◄── response

request ─────────────►
        server waits...
        ◄── response
        
HTTP STREAMING ← YOUR CODE:

request ─────────────►
        ◄── update
        ◄── update
        ◄── update
        ◄── update
        ◄── update
        ...

WEBSOCKET:

client ◄══════════════► server
       both directions
       whenever needed


Long polling = long-lived requests, plural.
Server eventually answers → connection closes → client makes another request.

HTTP streaming = long-lived response, singular.
Server keeps the response open → continuously sends more data.

WebSocket = long-lived bidirectional connection.
Both client and server can independently send messages.

And there's SSE, which is a standardized form of one-way HTTP streaming using text/event-stream.


Example: "We need live updates from the server, but the client doesn't need to send real-time messages back."
For something like a stock ticker using HTTP streaming/SSE rather than automatically jumping to WebSockets is an archiecture that makes sense as we don't need the communication back from the client like you would in a messenger app
