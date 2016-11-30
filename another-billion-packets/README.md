Another Day Another Billion Packets.
====================================

Our customers have datacenters and we have the cloud. We created the VPC out of EC2.

A problem that we face is that the IP space has collisions because of clear intersection in the IP address space. This is a *major* problem.

Why does the list of IPs

* 10.44.12.4,
* 10.44.12.5,
* 10.44.92.17,
* 10.108.6.4

You end up with a ton of W.X.Y.Z/32 route tables. We were coming from 168.192.0.0/16, but what happens if we move the datacenter into 10.44.0.0/16...*blarg*.

### Requirements

* Customer selected IP addresses
* Route aggregation for external connectivity. We don't want singular destination routes.
* Conformance with existing network designs...this is a bloody problem.

__Routing Table:__

* 192.168.0.0/16: Stay here
* 172.31.0.0/18: (our defined AWS VPC)
* 172.31.1.0/24: (Subnet 1)
* 172.31.2.0/24: (Subnet 2)

How do we integrate this? VPN, with VGW and CGW (whatever those are). This is called "direct connect."

Now we've set this up and each "Subnet" isn't connectable to anything other than your on-prem network.

### This is just "Virtual Networking"

* VLAN ~= Subnet
* VPC ~= VRF

### How do we manage 1,000,000 customers?

The VLAN ID space is constrained

* 12 bits => 4096 total VLANs

VRF support is constrained

* Large routers => 1 - 2 thousand VRFs ($200K+)

__We need a REALLY REALLY big router.__ We have a control plane and a data plane with a data pipe between them. Now you need a pair of these because you need HA. We need to scale linearly. How do we not buy large hardware off commercial banks so that the price is feasible.

### Example

* Average router config file: 50 char
* Config per VPC: 10 lines
* Subnets per VPC: 4
* Config per subnet: 5 lines
* Total VPCs: 2,000
* Config size: 3mb

this took 5 mins to parse at first.

__We need commodity hardware!__

### Silos of Capacity slide.

A virtual machine goes into a router pair. And we don't know how much capacity a person needs.

* Scale to millions,
* Host any instance in any VPC.

### Concepts

* Hypervisors with physical addresses.
* EC2 instances go to the "Virtual Network"
* Virtual machines from different customers on a hypervisor can get the same IP...*yikes*.
* Mapping Service is the important bit here. It acts as the "switch" in the network. Keeps a list of what boxes of a customer on which hypervisors. It double checks with mapping service on receipt of a request as a verification mechanism.
* We do packet abstraction by using the mapping service to mananage our "Virtual Packets"

__How do we make this fast enough?__ We need near 0 response time.

### Mapping Service Caching.

* The mapping service gets cached on each host.
* How does the cache get updated/invalidated??? This seems like a really really hard problem.

__How do we get home to the datacenter?__ We use the same mechanics in the edge nodes (which seem to be physical machines). The edge machines have the same mapping service caches.

### 3 Types Of Edges (translators)

* VPN tunnel (packet encapsulation, and do it with the correct information, IPSEC Stuff).
* Direct Connect (change to a VLAN tag on this).
* Internet Gateway (translator to a public IP, ephimeral or static).

### Brief Diversion

* They built this thing (huge router) known as a "blackfoot" based on a linux machine. Becaus it was built in cape-town, south africa.

### VPC Pricing

__$0.00__ - What the fuck...how do we do this??? Nov 10, 2010 (think black friday 2010). That's the date that they fully went all in on Amazon.com to EC2. VPC is built to handle everything now. They have to build their own "hypervisor routers." I guess that's the mapping service.

* We want firewals. (security groups)
* We want network ACLs

### Other public services as private now.

__We want s3 endpoints...__ the endpoints here are in the public space, how do we do this without going out to the internet?? We have to throw everything through NAT instances...is this wasted? Managed NAT is the way to do this? No, customers want s3 endpoints to be inside the VPC. Now the edge owns the bucket information. They just make it "look" local so that you don't have to run NATs or managed NATs. And we can do secure connection using bucket policys. You can even reference a specific VPC!!! This is nice.

### Simple Complex Balance.

* EC2 is the simple side...default VPC.
* Custom VPC is the complex side.
