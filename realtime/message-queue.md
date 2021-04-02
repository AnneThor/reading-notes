# Message Queues

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

[Socket.io: Chat Example](#chat-example)

[Socket.io: Rooms](#rooms-namespaces)

[Socket Cheatsheet v3](#wikipedia-websocket)

## Review, Research & Discussion 

### What does it mean that web sockets are bidirectional? Why is this useful?

WebSockets are full duplex, meaning that they have a persisitent connection between client/server and that either side can send to the other at any time (vs. half duplex systems where traffic is one directional - i.e. client or server can send but traffic cannot flow from each simultaneously). It is useful in enabling "realtime" communication from either end, neither side has to wait before conveying information.

### Does socket.io use HTTP? Why?

Yes, socket.io does not use WebSocket, it mimics the behavior of WebSocket. It uses websockets as one means of transportatoin and provides others as a fallback or when websockets aren't desirable. The initial connection will be done by http regardless.

### What happens when a client emits an event?

Client apps emit events that they transmit to the server socket (with which they have a full duplex connection). The server app has a listener set for the event and the callback of this listener preforms some action (emitting another event to a directed audience or something else).

### What happens when a server emits an event?

When a server emits an event, it is able to transmit that to all, or a specified subset, of clients. If the clients who receive the event have a listener set for this event, they will respond somehow. If they do not, they will "ignore" it.

### What happens if a client “misses” an event?

The default is that they are ignored (same as if they are broadcast to a client that does not have a handler function). You store the events in a buffer if you want to access them later.

### How can we mitigate this?

You can push the unhandled events into a buffer and use a timer to check for a good connection (to handle them if the connection is reestablished). You have to be mindful about the timer, client memory resoures and not to buffer things that will never get handled.

## Vocabulary Terms 

Term | Definition
---- | ----------
Socket | A socket is one endpoint of a two way connection between two programs running on a network. The socket is bound to a port number, allowing TCP to correctly deliver information.
WebSocket | Communications protocol for full-duplex communication over a TCP connection
Socket.io | This is a library that is used to mimic WebSocket
Client | In terms of sockets, this would be an app that connects to the server app and sends it events as well as handles or ignores events it received
Server | In terms of sockets, this would be an app that handles the logic of the problem, it will accept all the client emitted events and route them accordingly to other clients (or back to the sender)
OSI Model | 7 layer framework that describes how data is processed in a networked system (essentially how the information is disassembled and reassembled at the different layers to securely and efficiently transfer information). Application -> Presentation -> Session -> Tranport -> Network -> Data Link -> Physical 
TCP Model | The "handshake" model between client and server that establishes open communication
TCP | Transmission Control Protocol is one of the protocols used by the OSI model which turns data into packets with a checksum that allows them to be trasmitted in smaller pieces and allows a checking ability on the receiving end to guarantee all information has been trasmitted and creates the ability to re-request missing portions
UDP | User Datagram Protocol is a protocol that is lossy and speeds transmission by not requiring information to be delivered in totality and does not have a checking mechanism to rerequest missing portions (this is used for streaming type services)
Packets | Packets are small pieces of data sent over a network that include the information and the source/destination and other information. When they reach the final destination they are reassembled into the content. Typically consist of header and payload.

## [Chat Example](https://socket.io/get-started/chat/)

[Created basic chat example](/chat-example)


## [Rooms Namespaces](https://socket.io/docs/v3/rooms/index.html)

Rooms are arbitrary channels that sockets can ```join``` and ```leave```: you can use them to direct messages. **Idea exists only on the server side.**

Join the room: 

```socket.join('some room')```

When emitting: 

```io.to('some room').emit('event', payload)```

or

```io.to('room1').to('room2').emit('event', payload)```

In the case of **unions** sockets that are in multiple rooms broadcast to will only get the message ONCE

To exclude the sender:

```JavaScript
io.on('connection', function(socket) {
    socket.to('some room').emit('some event');
})
```

Leaving a channel just call leave instead of join

**Other Points**

- Every socket has a unique ```socket.id```

- Use emitting to rooms in a synchronous manner

- After disconnecting, sockets leavea ll rooms they were in; you can catch the rooms they were in by listening on the disconnecting event (socket.rooms)

- you can use redis adapters to have multiple socket.io servers; ```rooms``` and ```sids``` objects are not shared between servers (they may exist on one server or the other)

