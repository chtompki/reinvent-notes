reinvent-notes.
===============


### 11/29

* [Blockchain](./blockchain) - *Horrible*
* [EC2 Network Ha](./ec2-network-ha) - *Very Good*, outside my wheel-house.
* [Formal Logic](./formal-logic) - *Amazing*

### 11/30

* [Another Day Another Billion Packets](./another-billion-packets) - *Really interesting*
* [Amazon EFS Deep Dive](./efs-deep-dive) - *Good*
* [Toyota Racing](./toyota-racing) - *Ok*

### 12/1

* [Cryptography and OpenSource at AWS](./s2n) - *Very Good*
* [Deploy a Scalable SAP Hybris Cluster with Docker on Amazon ECS](./docker-hybris) - *Horrible*
* [Wild Rydes](./bots) - *Ok*

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