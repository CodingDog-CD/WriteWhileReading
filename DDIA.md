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


