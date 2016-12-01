Toyota Racing.
==============

How Toyota Racing development makes racing decisions in real time with AWS (Using DynamoDB). AWS says that this is "Real time data ingestion."

### Intro

* DynamoDB
* Streams
* AWS Firehose.

### Pre-race data.

* batching. We need it in minutes.
* models.
* give practice lap data back to drivers

### Live data.

* real time data streaming, need data in seconds.
* mods to the car based on changes to the race: when to pit, and other stuff.

### Stack

* Running node
* DynamoDB streams and Mongo
* S3
* ElastiCache
* JavaScript Google Polymer - Front End
* Get a track feed (sounds like the things I'm familar with).
* lamba pushes data from DynamoDB up to S3.
* They have this "arbiter"...I wonder what the hell this is.

### DynamoDB

* decouple collection and analysis. apparently DynamoDB makes this relatively easy
* DynamoDB has a stream that is an operation log. This gets persisted.
* Only settings: read iops, and write iops.
* Hash is location in the binary tree, range is the n-th number in the list.
* Talking about table and data model design. Great...don't care.
* Manual partitioning based upon because they've got a new table per "run" using the lable of the date.
* Now they're making one table per lap...ugh I suppose that this offers a nice parallelization mechanizm, but it feels dirty.

### Data Streaming

* They "tail" the stream.
* They call packets "shards". Maybe it's not a packet, but it's some segmentation mechanism of the stream.
* He said FAFF'ing NICE!!!.

### Sharding.

* You don't get notification on shard expiration/beginning.
* Or shards can split.
* This streaming mechanism fucking sucks.
* Use that java Kinesis client because it's the native.
* I don't know why the hell they're not using Kinesis.

### Firehose

* Fast data aggrigator.

### Example.

* They show a bunch of lap times and know how people are doing relative to last time they pitted, and if they're running efficiently or not and how soon they'll need gas and such.

