### VPC 
* A Virtual Private Cloud (VPC) is a logically isolated section of the AWS Cloud where you can launch AWS resources in a virtual network that you define. It can span access multiple availability zones in a region. 
* Within a VPC, you can create subnets, which are ranges of IP addresses. Each subnet must reside entirely within one Availability Zone and cannot span zones. 
* By launching AWS resources in separate Availability Zones, you can protect your applications from the failure of a single Availability Zone.
* A public subnet is a subnet that is accessible from the internet, where as a private subnet is a subnet that is not. 
    - A public subnet is public because it has a route to an internet gateway. 
* To define access to the internet and between subnets, we use Route Tables.

### Internet Gateway
An Internet Gateway (IGW) is a horizontally scaled, redundant, and highly available VPC component that allows communication between instances in your VPC and the internet. It therefore imposes no availability risks or bandwidth constraints on your network traffic. 

### Network Address Translation
A NAT (Network Address Translation) gateway enables instances in a private subnet to connect to the internet or other AWS services, but prevents the internet from initiating connections with the instances. NAT gateways are designed to operate in one direction: Instances in a private subnet can connect to services outside your VPC but external services cannot initiate a connection with those instances.

![](https://i.imgur.com/qDM9vHc.png)

### A Network Access Control List (ACL) 
NACL is a set of rules that control traffic in and out of a subnet in a Virtual Private Cloud (VPC). It acts as a firewall for the subnet, allowing or denying traffic based on the rules defined by the administrator. Network ACLs are stateless, meaning that responses to allowed inbound traffic are subject to the rules for outbound traffic, and vice versa.

On the other hand, a Security Group acts as a virtual firewall for EC2 instances. It controls inbound and outbound traffic at the instance level. Security Groups are stateful, meaning that if you allow incoming traffic on a specific port, the outgoing traffic for that connection is automatically allowed.

#### Use cases
1. Network ACLs include adding an additional layer of security to your VPC by controlling traffic to subnets using rules that are similar to the rules for your security groups. Network ACLs can also be used to block IP addresses or ranges of IP addresses from accessing resources in your VPC.
2. Security Groups include controlling access to and from an ENI / EC2 instance. It can only have ALLOW rules which include IP addresses and other security groups.

You can find more information about Network ACLs and Security Groups in the official [AWS documentation](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html).