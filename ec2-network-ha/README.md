EC2 Networking High Availability.
=================================

### Network.

* Same old problems: TTL and TTL not respected
* ELB - multiple IPs, max failover 7-12s
* Route 53 - multi-region, max failover 10-20s.
* Autoscailing - custom cloudwatch metrics, lifecycle hooks.
 
### Use Lambda as Glue!

* Adding interfaces in Auto Scaling groups
* Adding removing IPs form Route 53

### Networking Tips

* Bandwidth is based on VPG configs, and A NAT Gateway is one per region
* This is going to be heavy network diagrams.

### Agent based soluiton

* Using a network "agent" (i.e. a box for managing the network itself).
* Security issues because you're OS constrained.

### DNS (rte 53)

* Client must support DNS!

### ELB "Sandwich"

* Need a WAF layer infront of the webservers.

### Autoscaling group of 1.

* If you can handle reboot time this is kinda nice for if you just want a box up and running.

#### USE LAMBDA FOR HA MONITORING/REACTION!

* This is great.

### Floating Elastic Network Interface (or IP)

* Read on this...

### Route shifting.

* Standard failover using route tables so long as private IPs can change.

### Transit VPC

* Utilizes Cisco CSR (whatever this is)
* You use VPN tunnels from a hub VPC to spoke VPCs.
* Bandwidth botteneck at 1.5 to 2 GBPS.
* Failover time is 30s.

### Overlay Networks.

* This shit is heavy...you can abstract away from AWS.
* "Services VPC" -> a lot like the transit VPC. (Kinda like the transit VPC but using the ELB sandwich solution too.
    * __*Is this the "Shared Services VPC"? Damn it.*__ 
* You can wrap all the incoming and out going requests in a single vpc so that you can do packet inspection and such.

### VPN overlay mesh

* The problem here is that it's the complete graph on the number of VPC's