Cryptography and OpenSource at AWS.
===================================

###  What to expect

* Why s2n? OpenSSL and Heatbleed
* Opensource at AWS
* How to develop and test s2n

###  [OpenSSL](https://www.openssl.org)

* This library is impressively ubiquitous 

### [Heartbleed](https://en.wikipedia.org/wiki/Heartbleed)

* April 7, 2014. 
* Really large news event. Major issue.
* What is the heartbleed bug:
    * TLS is an absraction between TCP/IP and HTTP/SMTP,SQL
    * On the wire it is the record protocol. Here's the format:
        * (Message Type, Protocol Version, Record Length, Data)
    * There's an extention (heartbeat) in RFC6520 that has the following format:
        * (Message Type, Protocol Version, Recordlength, heartbeat message type, payload length, payload, padding)
        * This is a keep alive request
    * MTU = maximum tramsmission unit, is built in to TCP/IP, but not RDP (whatever that is).
    * What is the heartbleed bug? Both are allowed but should not be.
        * (24, 3.2, 22, 1, 3, 0xBADBAD, Padding)
        * (24, 3.2, 22, 1, 65536, 0xBADBAD, Padding)
    * Easy to exploit to ask for 64k of data of whatever data just happened in memory. It's the last payload length. That landed in memory that it returned
    * How is this possible? It was doing pointer arithmetic, and there isn't any check to match this.
    * The file in which the problem exists is a really really *TERSE* file for reading it. No one can actually read this file.
    * There were no good test cases. (Ugh)
    * The code is doing raw pointer arithmetic as opposed to something better.
    * Needless functionality included by default. 99.99% of the time the heartbeat isn't needed. It's included and enabled by default.
    * __Response:__ 80% fixed at amazon in 5 days and 98% fixed in a month.
        * AWS put in additional monitoring to pro-actively let customers know that this was a problem in their software. *THIS IS REALLY CRAZY.*

__The ubiquity of the usage of this code leads to this 2 line bug causing a news event larger than any new comment in the iraq war.__ Holy Fuck.

### The response. [s2n](https://github.com/awslabs/s2n).

* Apache 2.0 licensed. We need a better SSL/TLS implementation.
* It's an implementation of C99.
* He put it together in 40 hours of 5 weekends. It's super minimal to just to SSL/TLS.
* Ben Laurie: *"The really fun part of working on cryptography is spending hours staring a big opaque numbers trying to debud what's going on"*
* Proposal: make *s2n* an official AWS project. Start with a working backward document.
* Initial feedback: "Sounds great! Make sure it's open source."
* AWS Feedback: "What's the long-term ownership paradigm?"
* OpenSource at amazon is super mellow. WOA!
* Their processes are super rigorous. This is fairly impressive.
* How to actually get this fully running generally. That initial implementation was for one case, but there are a ton of cases on how to run this.

### Things to focus on.

* Team Culture
* Simple designs, readable code
* Quick, easy

### Team Culture

* agree and document goals priorities, and tenents
* agree and document our coding guidelines.
* make testing and safety easy and fast (this is super important).
* think hard about high-level design.

###