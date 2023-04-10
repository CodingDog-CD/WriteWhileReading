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

Why do we need replicate data?
- increase availability (e.g. failed node, network problem for a specific region, etc.)
- keep data geographically close to your users, distribured datacenters helps to decrease latency
- scale out the number of machines that can serve read queries (increase read throughput)

What kind of difficulties?
- data consistency, because data changes over time as users can write any time to the database

Approach to deal with the difficulty
- Leader-based replication 
- Leaders and Followers
- single-leader approach
  - a replica is designated as leader, which receives write request and distribute the data changes to all followers
  - other replicas are followers, which are normally read-only, serve for read operation

Synchronous replication VS Asynchronous replication
- synchronous replication makes sure that data in each replicas are always consistent
- synchronous replication waits until all replicas response to the leader
- synchronous replication will block further operations, when any follower in the whole replicationset encounters problems e.g. not reachable due to network problem, node failed, etc. Because no response can be given in such cases.
- Asynchronous replication doesn't wait for response of followers
- Asynchronous replication may cause problems like inconsistent data among different replicas
- Asynchronous replication may combine with synchronous replication -> semi-synchronous
- Semi-synchronous replication ensures one synchronous follower. Other followers use asynchronous approach

Setting up new follower
1. Take a consistent snapshot of the leader's database at some point in time - if possible, without taking a lock on the entire database
2. copy the snapshot to the new follower node
3. the follower connects to the leader and requests all the data changes that have happened since the snapshot was taken.
4. When the follower has processed the backlog of data chagnes since the snapshot, we say it caught up.


Handling Node Outages
- Follwer failure: catch-up recovery. On its local disk, each follower keeps a log of the data changes it has received from the leader. Therefore, a follower can recover quite easily. From its log, it knows the last transaction that was processed before the fault occurred. Thus, the follower can connect to the leader and request all the data changes that occurred during the time when the follower was disconnected. 
- Leader failure: Failover. A failover process means that one of the followers needs to be promoted to be the new leader, clients need to be reconfigured to send their writes to the new leader. 

Failover process:
1. **Determining that the leader has failed.** There are many things taht could potentially go wrong: crashes, power outage, network issues, etc. It's difficult to detect what has gone wrong, so most system simply use a timeout. 
2. **Choosing a new leader.** Election process. The best leader candidate is usually the replica with the most up-to-date data changes from the old leader (to minimize any data loss). Getting all the nodes to agree on a new leader is a consensus problem
3. **Reconfiguring the system to use the new leader.** Clients now need to send their write requests to the new leader. If the old leader comes back, it might still believe that it is the leader, not realizing that the other replicas have forced it to step down. The system needs to ensure that the old leader becomes a follower and recognizes the new leader.

### Partitioning

### Transactions

### The Trouble with Distributed Systems

### Consistency and Consensus

## Derived Data

### Batch Processing

### Stream Processing

### The Future of Data Systems


