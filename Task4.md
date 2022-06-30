## AWS

### Pre requirements
You need to get access to TeamDev AWS account to be able to do below tasks. The accounts should be provided by your lectors.

### Sub Task1 (Basic AWS)
Create an infrastructure to have 2 EC2 instances: the key-value storage server and client. Establish communication between them.
As a result of the task, you will have to show you can **ping** server app from the client instance.
**PLS USE FREE TIER EC2 instance type**

Hint: Both EC2 instances have to be inside the same VPC, have a security group, which will allow only communication between them,
and custom network interfaces.

#### Something to read:
* [VPC](https://docs.aws.amazon.com/vpc/latest/userguide/how-it-works.html)
* [EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)
* [EKS](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html)
* [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html)

**Notes**: [Lecture Notes](https://docs.google.com/presentation/d/1wyWZEwgsRaWleMgPJEpw_l48swfnRYm4/edit?usp=sharing&ouid=104154946265606394823&rtpof=true&sd=true)

### Sub Task2 (CloudFormation templates)
Automate the infrastructure creation using Cloud Formation Templates (CFT). CloudFormation should deploy next resources:
1. VPC
2. Security Zones
3. EKS cluster in the VPC 
4. Deploy cert-manager on the EKS cluster
5. Deploy key-value server and key-value operator on the EKS cluster

#### Something to read:
* [CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)