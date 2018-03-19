# Zoo of database storage

a zoo of all kinds of db storage structures. From simplest ones to complex ones.

## naive log

```
#!/bin/bash
    db_set () {
        echo "$1,$2" >> database
}
    db_get () {
        grep "^$1," database | sed -e "s/^$1,//" | tail -n 1
}
```

## kv log with in-memory hash index

- in memory: `Map: {String -> Offset}`
- in disk: `CSV`
- query
    - get offset from memory
    - read csv in disk, seek to offset

maintenance: regularly do compaction and merging, to remove redundant logs in disk

## SST: Sorted String Table

several SST files on disk, each holding a bunch of key-value logs. Within each SST file, keys are sorted. Each file is several KB. Each time there is an in memory table (memtable).

On write:

- is the key in mem? if true, just modify the memtable
- if memtable got too big, serialize and store to disk as a new SST table file.

On read:

- is the key in mem? if true, return value
- find out all SST files that may contain this key
    - naive method: all files need to be examined
    - in mem index: store ranges of each file - this index may be too big to fit in mem
    - bloom filter: less mem used, but emits some false positive ranges - pretty much acceptable 
- scan each file, stop at first occurance

On background:

- regularly do compaction and merging

## LSM tree

A multi-layer hierarchy of SST files.

## B-tree

Rule of thumb: LSM trees faster for writes, B trees faster for reads

