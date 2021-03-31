# Socket.io

**[Return to Main Page](https://annethor.github.io/reading-notes/)**

## Notes for Required Readings

[Video: Open System Interconnection Model Explained](#osi-explained)

[Video: TCP Three Way Handshake Details](#tcp-handshake)

[Wikipedia: WebSocket](#wikipedia-websocket)

[TutorialsPoint: SocketIO Tutorial](#socketio-tutorial);

[Educba: WebSocket Vs. Socket.io](#websocket-vs-socket)

## Review, Research, Discussion

### 1. What is the benefit of transforming data into packets?

Data is broken into packets so it can be sent more efficiently. Smaller data increases odds of successful delivery, and if the data is not all received, only missing parts need be requested again vs. the whole thing. This also allows data to be send via different pathways via the most efficient routes and avoid roadblocks.

### 2. UDP is often refereed to as a connectionless protocol. Why is this?

User Datagram Protocol is a one way transfer of data that does not monitor or give feedback on whether all of the information was successfully transmitted. It therefore more vulnerable to attacks and data loss in transfer. It does not utilize any "handshake" agreement between the client/server and it does not make any system of accounting how to assemble packets or check for completeness. UDP is used where speed is favored over completeness (so realtimes things like voice over internet, video, etc).

### 3. Can a socket server application have multiple socket connections?

Yes, a socket server utilizes an assigned port that has many socket connections that clients can connect to. 

### 4. Can a socket connection application be connected to multiple socket servers?

The socket connection/client application can connect to multiple sockets in the server (different namespaces), but the client application would not be connected to multiple different servers.

### 5. Can an application be both a socket server and a socket connection?

Yes, if they are configured to operate on different ports. I'm not sure I see the utility in that though, I guess it would depend on the functionality of the app. Socket connection apps can send signals as well as receive them so I'm not sure why one would also need to be the socket server within the same system ... ?

## Vocabulary Terms

Term | Definition
---- | ----------
Observer Pattern | Programming paradigm where the subject (which is an object) maintains a list of dependent objects (observers) and notifies them of state changes by triggering their methods; in event driven programming this would be the events manager object
Listener | Listeners are functions that await a signal and perform some action when this signal is emitted
Event Handler | An event handler is a function that waits for a certain action and responds to the action with some action
Event Driven Programming | This is a programming schema where control flow is determined by a series of objects interacting with one another by emitting signals and responding to signals when they are emitted in the environment
Event Loop | The event loop is a construct that awaits events or messages in a program and responds by making a request to some source (internal or external) to respond to the events
Event Queue | A data structure that organizes control flow based on a queue structure (first in, first out)
Call Stack | The call stack is a data strucutre that organizes control flow by responding to the most recent commands first (first in, last out manner)
Emit/Raise/Trigger | Signal sent to the environment that will trigger listener functions
Subscribe | Listener functions subscribe to certain signals, meaning they are listening for them and will respond if they are heard
Database | A database is a mechanism of storing information in a persistent way (relational or not)


### [OSI Explained](https://www.youtube.com/watch?v=vv4y_uOneC0)

In most simple setup, 2 computers are networked on a LAN cable, but because they can use different OS, there is an OSI Model. Basically data goes up and down the seven layers in the process of being sent from source to destination

1. Application Layer
2. Presentation
3. Session
4. Transport
5. Network
6. Data Link
7. Physical

**Application Layer**:
Used by network applications (computer apps that use internet)
Web browser is a network app operating in the PC that uses application layer protocols (HTTP/HTTPs) to communicate
All of these protocols collectively form the application layer (File Transfer (FTP), Web surfing (HTTP/s), Emails (SMTP), Virtual Terminals (TelNet))
Provides services for network applications using protocols

**Presentation Layer**:
Receives data from application layer in terms of connectors/numbers
Presentation layer converts into machine language (translation)
Before transmission, presentation reduces number of bits to present data (lossy or lossless data compression)
This reduces the space needed to store original file and speeds up data transmission
Before transmission data is encrypted to enhance security sender (encrypted)/receiver(decrypted)
3 basic functions: Translation/Compression/Encryption and Decryption

**Session Layer**:
Helps to set up/manage connections, enabling sending, receiving data and terminating/ending sessions
Guard APIs, NETBIOs (basic in/out system) that allows computers to communicate
Server preforms authentication before establishing session (username, password), session is started
Then server checks authorization that you can access the data you're requesting
Session layer preforms AUTHENTICATION and AUTHORIZATION
Keeps track of files being downloaded (text and images being accessed from web server, they're stored as separate files on server)
A separate session is initiated to send the data packets for each file (separate for image and for text files)
Session layer keeps track of which file data packets belong to and where they should be delivered

**YOUR WEB BROWSER PERFORMS APPLICATION, PRESENTATION, and SESSION LAYER FUNCTIONS**

**Transport Layer**:
Controls the reliability of communication through segmentation, flow control, and error control

- Segmentation: Data is segmented into small units (each includes seq number, port number, data number); port number directs to right app, seq numbers helps to reassemble
- Flow Control: imagine that server can send at 50mbps, but mobile can only accept 10mbps, so mobile with help fo transport layer communicates max input speed and server adjusts; each packet has a checksum and that should add up the expected total (using an algorithm, this is on a byte level)
- Error Control: if some data does not arrive at destination it uses automatic repeat request to get missing data or find corrupted data; protocols are TCP and UDP

Services | Protocols
-------- | ---------
Connection-oriented transmission | Transmission Control Protocol (TCP)
Connectionless transmission | User Datagram Protocol (UDP)

UDP is faster than TCP because it does not provide feedback about whether data is actually received (lost data can therefore be retrieved or tracked with TCP); UDP is used when lost data is immaterial (online streaming movies, songs, games, etc); TCP is used where full data delivery is a must (email, WWW, FTP, etc)

Main functions of transport layer are: Segmentation, Flow Control, Error Control, Connection and Connectionless Transmission
Transport Layer passes data segments to network layer

**Network layer**:
Accepts data segments from Transport Layer and passes to assigned computers within the same network
This is the layer where routers reside
Data units in network layer are called packet

- Logical Adressing: IP address assignment done in network layer, every computer has unique IP address, network layer assigns address to ensure data packets reach destination

- Routing: move data packet from source to destination; packets contain sender/receiver addresses (network and computer addresses); that is based on IP address + network mask

- Path Determination: a computer can be connected to internet server in multiple ways, choosing the best route from source to destination (under the hood it will choose this for you)

**Data Link Layer**:
Receives data from network layer in form of data packets

- Logical Addressing: done at network layer via IP addresses

- Physical Addressing: done at data link layer, MAC addresses are assigned and allow computers to transfer in a physical way between machines; these are added to the packets to make frames

Data link layer is embedded in your computer hardware

Two Main Functions:

- Provides access to media to higher layers by framing

- Controls how data is placed and received from media (Media Access Control and Error Detection); adds head and tail to frame, this is turned into packets, then back into frames to make frames that the destination can access

- Keeps an eye on when shared media are using the same transmission to avoid collisions

**Physical Layer**:
Up to now, data has been segmented by the transport layer, turned into packets at the network layer, and turned into frames at the data link layer resulting in a binary sequence; the physical layer converts to signal and communicates over local media (electric signal, light signal or radio signal); Signal is received as bits and passed to data link layer as frame

**Application Layer**:
Makes visible to user on computer screen


### [TCP Handshake](https://www.youtube.com/watch?v=xMtP5ZB3wSk)

TCP Three Way handshake
Transmission Control Protocol: reliable and connection oriented protocol allwoing successful/accurate data transmission
Before TCP transmits data, it uses 3 way handshake to establish connection
Example: client requests web page from server, before transmission TCP must be established:
1. Client sends syn segment to server asking for synchronization (i.e. asking for connection)

Let's say SYN: SEQ# 9001 ACK# 0 SYN #1

2. Server replies with (SYN-ACK), meaning it acknowledges the request and asks client to open connection too

SYN-ACK: SYN# 1 ACK# 9002 #SEQ 5001
*Server has sent it's ack as the sequence + 1 and send it's own sequence number*

3. Client replies with (ACK) which is like "yes" and the 2 way connection is established

ACK: SEQ:9002 ACK: 5002 SYN# 0
*Client has incremented sequence acknowledged server sequence and added 1 to it*

At this point both have agreed to establish connection


### [Wikipedia WebSocket](https://en.wikipedia.org/wiki/WebSocket)

WebSocket is a computer communications protocol providing full duplex communication channels over single TCP connection; it is distinct from HTTP. Both are located at the application layer of the OSI model and depend on TCP at level 4 (transport layer); enables interaction between client app (like web browser) and server with lower overhead than HTTP facilitating real time data transfer and 2 way communication (server can send to client without first being requested); WebSocket enables streams of messages on top of TCP

To estabish WebSocket connection, server/client exchange handshake starting with an HTTP req/res allowing servers to handle HTTP and WebSocket on the same port, once connected, communication switches to bidirectional binary protocol (not conforming to HTTP protocols)

In addition to ```Upgrade: websocket``` headers, the client sends ```Sec-WebSocket-Key``` header with base64 encoded random bytes and the server responds with a hash of the key in the ```Sec-WebSocket-Accept``` header; the point there is to prevent caching proxy, but it doe snot add any additional security/integrity

Once established, full duplex communication is a go, data is minimally framed with a small header followed by payload; they're sent as messages where they are split across multiple data frames (allows sending of partial messagse or allowing text to appear as it is written)

**Security**: unlike regular cors HTTP requests, WebSocket requests aren't subject to the same-origin policy, therefore they must validate ```Origin``` header against expected origin during connection establishment to avoid hijacking efforts; best to use tokens or similar to authenticate WebSockets

### [SocketIO Tutorial](https://www.tutorialspoint.com/socket.io/)

Socket.io enables real-time bidirectional event-based communication, works on all platforms/devices and built on top of websockets API (client side) and node.js

Socket.io has two parts:

- a client side library that runs in the browser
- a server side library for node.js

Client side reserved event in Socket.io
- Connect
- Connect_error
- Connect_timeout
- Reconnect, etc

Server side reserved events in Socket.io
- Connect
- Message
- Disconnect
- Reconnect
- Ping
- Join
- Leave

Client Side/Server Side:
Both use the ```socket.on()``` and ```socket.emit()``` language

Using ```io.sockets.emit()``` sends to ALL connected clients, often you will want to refine that to target only specific users; ```socket.broadcast.emit()``` can be used to send to everyone but the client that caused it

You can use namespaces to assign different paths or endpoints; these all use the same WebSocket, but they allow you to separate concerns

- created on server side
- joined by clients sending request to the server
- root namespace is '/', which is joined by clients if a namespace is not specificied by the client while connecting to the server
- connection using socket-object client side are made to default mamespace (meaning ```var socket = io()``` connects to default namespace)

Within each namespace you can define arbitrary channels that sockets can join and leave called **rooms**, these are further used to separate concenrs 

- **rooms can only be joined on the server side**

### [WebSocket Vs Socket](https://www.educba.com/websocket-vs-socket-io/)

#### **WebSocket** 
Communication protocol that provides persistent two way communication between Client/Server on TCP connection; major advantage over HTTP connection is full-duplex communication

WebSocket protocol schema ws://example.com:4000/chatroom.php

- ws = schema
- example.com = host
- 4000 = port
- chatroom.php = server

#### **Socket.io**
A library that allows real time, bi directional communication, helps in broadcasting to multiple sockets at a time and handling connection transparently

WebSocket | Socket.IO
--------- | ---------
protocol which is established over TCP connection | library to work with WebSocket
provides full duplex communication on TCP | provides event based communication between browser/server
proxy/load balance not supported | connection can be established in the presence of proxies/load balancers
no broadcasting | broadcasting
no fallbacks | supports fallbacks

