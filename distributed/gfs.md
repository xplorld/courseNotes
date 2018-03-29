# Google File System

[paper](https://static.googleusercontent.com/media/research.google.com/zh-CN//archive/gfs-sosp2003.pdf).

## Overview

a distributed file system, can contain large files and have large overall size, with many many commodity (and mortal) devices.

## assumption

- commodity components, often fail
- million files * 100MB - 100GB per file
- large streaming read: MB of continous read with given file and offset
- small random read: KBs from given offset
- large sequential read: read whole file
- concurrent append (each append is atomic)
- bandwidth++

## interface

- create, delete, open, close, read, write
- snapshot, `record append`(concurrent append)

## architecture

- file
    - file = data + metadata
    - data splited to fix-size chunks on slave (may replicated)
    - metadata on master's memory
        - namespace (persistent to op log)
        - file-to-chunk mapping (persistent to op log)
        - location of each chunk replica (ask slaves on startup)
        - not important things: access control, etc (persistent to op log?)

- One master node
    - store metadata
    - heartbeat
    - chunk lease management, GC of chunk

- slaves
    - save chunks
    - 1 chunk = 1 linux file (???)
    - lease
        - for each chunk, 1 of all slaves are assigned 'primary' (are granted lease)
        - write to primary and propagate to other slaves
        - lease expires in 60 secs

- client
    - access master to get which chunk
    - access that slave to get chunk data (can read from any replica, maybe nearest or most idle one)
    - data no cache, metadata cached

## mutation 

- client ask master about all slaves and the primary
- client push data to one of the slave
- slaves forward data in a chained manner to ++throughput
- client -(request)-> primary
- primary serialize all requests for the same chunk
- primary propagate serialized order to all slaves
- all slaves reply ok to primary
- primary reply ok to client

| from | data | to |
|-|-|-|
| client | request | master |
| master | slaves and primary of this chunk | client |
| client | data | one of the slaves |
| that slave | data | all other slaves, in a chain |
| client | request | primary |
| primary | serialized requests | all other slaves |
| all other slaves | ok | primary |
| primary | ok | client |