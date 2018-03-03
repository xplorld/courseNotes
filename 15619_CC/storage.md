It was a wind and stormy night. Being tired and exausted, I wonder if it was right to choose this course. The AWS, the GCP, hadoop, and everything else - they all came up into my nightmare, constructing a big, big distrubuted spider's net, and I was the moth in the middle of the net. One inch away from my hand was the HBase tutorial, but, constrianed by this trap, I could not reach it.

## files

Files are the fundamental structure of data. It seems to be clumsy and errornous to directly use cleartext files as massive (>100M) data storage, but is still good to put in small files, e.g. configurations.

One good thing for files is that it is self-contained. If you want to transfer some data to another server, just `rsync`. You do not need to require your remote part to install a whole set of Database drivers, especially when the remote may not be able to do that: they may not have `sudo` permission, or are not allowed to open up more than one process, e.g. the remote is an AWS Lambda.

Bad things for files as small file structure is that it is "schemaless". No, REALLY schemaless. Not even has any structure, just plain bytes. In order to use that, the developer has to create their own schemas, or in other words, their own data protocol. In old days where space are expensive, experienced programmers create DSL parsed by handwritten parsers, later by generated parsers from YACC and other stuff. Now space is much cheaper, more and more programmers just use predefined protocols such as JSON, YAML and Protobuf. However, space is still not free. Considering transmission cost, in high efficiency senarios, one still has to make up a protocol.


## MySQL and JDBC

SQL is a complex world. MSSQL, MySQL, PostgreSQL, Orcale SQL, SQLite - all have different syntaxes, different performances, different pitfalls. JDBC tries to abstract out all these differencies, but the outcome is not that plausible. It uses `String`s to query, so that programmer can call each SQL's private dialect. Okay, now the backend is not replaceable. Then why do we just use the vendor's own library? It may be typed and performance tweaked.

If a startup wants to do an refactor, say from MySQL to PostgreSQL, they would have to examine all Query Strings. No, the Java Typesystem cannot come to help - invalid SQL strings are still valid `String`s. In conclusion, JDBC uses strings instead of function calls to make query, this design yields **abstraction leak**.

## Redis and Memcached

`HashMap` is a magnificient abstraction - though this very name is not that abstract, as some implementations may not use a hash. `Queue` is another great abstraction. It is wonderful to feel that almost all a decent programmer need to know lies in every Freshman's textbook.

| scale | HashMap | Queue | things to consider |
|-|-|-|-|
| small enough to put in memory | `std::unordered_map` | `std::queue` | cache miss, function call cost |
| have to put in disk ("persistent") | local key-value storage, e.g. RocksDB | ? | sequence/random IO, amplification|
| have to put in several servers ("distributed") | HDFS et.al. | RabbitMQ | voting, down recovery, CAP choice|
| I don't care, it's so big, let AWS do that | S3 | SQS | cross-region availbility, backup strategy, CDN usage | 

IMHO, Redis and Memcached lies in the "distributed" scale level.


## HBase

I'm schemaless! It's so cooooool!

Come on. Even if it is schemaless, one still have to set up a contract and a constraint to use it. what really makes Schemaless DBs strong is its flexibility, i.e. can easily change the data shape (contract) without having to notice the DB system or to fix a lot of broken code. In this view, HBase needs an ORM, just like MongoDB needs Mongoose. It is sad I did not see an HBase ORM tutorial in this project.

## Epilogue

Storage is complex. Its complexity lies in two directions: Model and Scale. Keep calm and be careful to choose the one you like.