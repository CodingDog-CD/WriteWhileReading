# Designing Data-Intensive Applications

## Foundations of Data Systems

### Reliable, Scalable and Maintainable Applications

### Data Models and Query Languages

### Storage and Retrieval
Log-structured storage is a simple storage strategy, where new data is going to be appended to the end of the file. <br/>
Read: poor performance, because the whole log file has to be traversed. time complexity: O(n) <br/>
Write: good performance, because it's very efficient to write to the end of an existing file. <br/>
The input data is written to a file, but we want to avoid overflow (super large file). The solution is: make storage segment. When a file gets bigger than some threshold, save it and start a new file to write.

SSTable - sorted string table <br/>
The difference between SSTable and Log-structured storage is that the data in SSTable is sorted by key. (We are in the scope of key-value storage) So that the compaction process and the merge process can be carried out efficiently. The approach is like the one used in mergesort algorithm.

Reference: Google's Bigtable paper <<Bigtable: A Distributed Storage System for Structured Data>>

### Encoding and Evolution

#### Dataflow through Databases

#### Dataflow through Service: REST, RPC

What is a service?
- When you have processes that need to communicate over a network, you can use the most common arrangement, where you have two roles: server and client
- Servers expose an API over the network
- Clients can connect to the servers to make requests to that API
- The API exposed by the server is known as a service.

What is a Web Service?
- When HTTP is used as the underlying protocol for talking to the service, it is called a web service.
- REST and SOAP

REST:
- it's not a protocol, but a design philosophy that builds upon the principles of HTTP
- simple data formats
- using URLs for identifying resources
- using HTTP features for cache control, authentication, and content type negotiation

SOAP:
- it's a XML-based protocol for making network API request

Remote Procedure Call (RPC)
- The RPC model tries to make a request to a remote network service look the same as calling a function or method in your programming language within the same process.

Difference between network request and local function call
- A local function call is predictable and either succeeds or fails, while a network request is unpredictable, the request or response may be lost due to a network issue, or the remote machine may be slow or unavailable, and such problems are entirely outside of your control.
- A local function call either returns a result, or throw an exception, or never returns (the process crashes or it goes into an infinite loop). While a network request has another possible outcome, it may return without a result, due to a timeout. (In this way, you are not able to know whether the request got through or not, because you don't get a response from the remote service)
- A local function call normally takes about the same time to execute, while a network request is much slower than a function call, and its latency is also wildly variable depending on the situation of network and remote service.
- When you call a local function, you can efficiently pass it references (pointers) to objects in local memory.
- When you make a network request, all those parameters need to be encoded into a sequence of bytes that can be sent over the network. It is problematic with larger objects.
- The client and the service may be implemented in different programming languages, so the RPC framework must translate datatypes from one programming language into another.

#### Message-Passing Dataflow
- REST and RPC, where one process sends a request over the network to another process and expects a response as quickly as possible
- databases, where one process writes encoded data, and another process reads it again sometime in the future
- A message broker is something in between (between REST/RPC and databases)
- A client's request (usually a message) is delivered to another process with low latency
- The message is not sent via a direct connection, but goes via an intermediary called a message broker (message queue or message-oriented middleware), which stores the message temporarily



## Distributed Data 

### Replication

### Partitioning

### Transactions

### The Trouble with Distributed Systems

### Consistency and Consensus

## Derived Data

### Batch Processing

### Stream Processing

### The Future of Data Systems


