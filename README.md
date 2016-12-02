reinvent-notes.
===============


### 11/29

* [Blockchain](./blockchain)
* [EC2 Network Ha](./ec2-network-ha)
* [Formal Logic](./formal-logic)

### 11/30

* [Another Day Another Billion Packets](./another-billion-packets)
* [Amazon EFS Deep Dive](./efs-deep-dive)
* [Toyota Racing](./toyota-racing)

### 12/1

* [Cryptography and OpenSource at AWS](./s2n)
* [Deploy a Scalable SAP Hybris Cluster with Docker on Amazon ECS](./docker-hybris)
* [Wild Rydes](./bots)

## Important findings.

[cloudformation parameter sections](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-cloudformation-interface.html). There is some flavour of mechanism that provides "parameter groupings"
 ```json
 "Metadata" : {
   "AWS::CloudFormation::Interface" : {
     "ParameterGroups" : [ ParameterGroup, ... ],
     "ParameterLabels" : ParameterLabel
   }
 }
 ```

The automated prover shit that Byron Cook did was really amazing. Look at formal logic.

The [s2n](./s2n) talk is really important about how to write code the right way!!

Elastic File System could be a faster way to do production releases because of nolonger neeidng to install anything.