Amazon EFS Deep Dive.
=====================

### Why and when to use EFS

* Types of storage
    * Block: ebs, ec2 instance
    * Object: S3, Glacier
    * File: EFS (file system interface, and such)
* How did we change the game here?
    * Simple, Elastic, Scalable
    * They keep bloody saying high availability.
    * Fast filesystem quickly.
    * Build on `NFS v4.1`, whatever that is.
    * Simple pricing -> easy forcasting. $0.30/GB
* You don't declare how large the file system is. The filesystem can grow to *Petabyte* scale
* Multiple AZ.
* Where can we use it?
    * us-east-1
    * us-east-2
    * us-west-2
    * ireland
* __When do I use this?__ Multiple instances running up agains a single filesystem. And an autogrowing filesystem. Less filesystem management.

### Understand key technical/security concepts

* __*Only 125 EFS instances per account*__ This fucking sucks.
* It's just a "mount target"
* Mount command: `mount -t nfs4 -0 nefvers=4.1 <file system DNS name>:/<user's local target directory>`
* *Claim.* Concurrency is completely managed. There are no bugs here?

### Lean how to leverage EFS's performacnce

* Paralell operation...
* Single operations that need to be super fast.
* Two different performance modes:
    * General Purpose (default): 7,000 ops/sec limit
    * Max I/O: need super fast ops/sec.
* Distributed across a bunch of different nodes. This seems loony, but ok.
* Generally it seems like this is a good move. We should probably use this.
* You get x00mB/s for xTb of data stored.
* Only use Ubuntu 15.04, 16.04, or Amazonlinux 2016.03.0
* __TEST, TEST, TEST!!!!!!__

### See EFS in action: copying data (hands-on)

* Two scenarios:
    * Transfer 1GB - 100GB files, move files from S3 to EBS
    * Files 4kb - 2mb lots of files, move files from S3 to EBS.
* Use `GNUparalell`. Seems like an interesting package...
* Some code showing GNUparalell as a command line mechanism.
* Repeatable outcomes, and optimize for costs.
* What's the best instance type for this? T2, C2, C4, M3, M4, R3, X?
* __1000 Big files.__
    * Tools:
        * Datadog
        * sumologic?
    * ~500mb/s
    * 50 instances -> 4.2GiB/s
* __Small files__
    * Plateau's out at 200 threads.
    * What's the best instance type for this..."c3.large"
    * 5000 files/min.
    * What happens with paralell
    * 300 instances -> 1,602,600 Files/min
* Less than $5 to do this test.
* __Paralellize everything__ This seems to be quite important here.
* __Wordpress.__
    * Content management/webserver stuff.
    * wikis, blogs, discussion boards.
    * 27% of websites run wordpress.
    * Easy content management system.
    * Store all the install crap on EFS.
    * Structured data (content) goes in RDS.
    * There will be a whitepaper on this.
    * Put elasticcache infront of RDS...curious...




### Review EFS's economics

### Upcoming feature plans.
