### VPC 
* A Virtual Private Cloud (VPC) is a logically isolated section of the AWS Cloud where you can launch AWS resources in a virtual network that you define. It can span access multiple availability zones in a region. 
* Within a VPC, you can create subnets, which are ranges of IP addresses. Each subnet must reside entirely within one Availability Zone and cannot span zones. 
* By launching AWS resources in separate Availability Zones, you can protect your applications from the failure of a single Availability Zone.
* A public subnet is a subnet that is accessible from the internet, where as a private subnet is a subnet that is not. 
    - A public subnet is public because it has a route to an internet gateway. 
* To define access to the internet and between subnets, we use Route Tables.

### Internet Gateway
An Internet Gateway (IGW) is a horizontally scaled, redundant, and highly available VPC component that allows communication between instances in your VPC and the internet. It therefore imposes no availability risks or bandwidth constraints on your network traffic. 

* An Internet Gateway (IGW) is a VPC component that allows communication between instances in your VPC and the internet. This means that if you have an IGW attached to your VPC, instances with public IP addresses can access the internet directly.
* A NAT Gateway, on the other hand, enables instances in a private subnet to connect to the internet or other AWS services, but prevents the internet from initiating a connection with those instances.
* So, if you have instances in a private subnet within a VPC that has an attached IGW, those instances would still need a NAT Gateway (or a NAT instance) to access the internet. Instances in public subnets with their own public IP addresses would not need a NAT Gateway as they can access the internet directly via the IGW.

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

### VPC Flow Logs
* VPC Flow Logs is a feature that enables you to capture information about the IP traffic going to and from network interfaces in your VPC. Flow log data can be published to Amazon CloudWatch Logs, Amazon S3, or Amazon Kinesis Data Firehose. After you create a flow log, you can retrieve and view the flow log records in the log group, bucket, or delivery stream that you configured.
* VPC Flow Logs can help you with a number of tasks, such as diagnosing overly restrictive security group rules, monitoring the traffic that is reaching your instance, and determining the direction of the traffic to and from the network interfaces. 
* Flow log data is collected outside of the path of your network traffic and therefore does not affect network throughput or latency. You can create or delete flow logs without any risk of impact to network performance.

### VPC Peering
* VPC Peering is a network connection between two or more VPC that enables you to route traffic between them using a private IP address. Instances in either VPC can communicate with each other as if they were in the same network. VPC Peering requires both VPC to be configured to be peered and and matched to work. 
* VPC Peering offers security and performance benefits for cloud resources. It lets you transfer data between VPCs, even if they are in a different AWS account or regions. Traffic always stays on the global AWS backbone and never traverses the public network, reducing security threats such as exploits and DDOS attacks.
* Note that you must not have overlapping CIDR (IP address ranges) between VPCs.
* VPC Peering connection is not transitive so connection between:
    - VPC A <-> VPC B
    - And VPC B <-> VPC C 
    - VPC A cannot communicate with VPC C

![](https://i.imgur.com/s3vRWlg.png)

#### VPC Endpoint
A VPC endpoint enables customers to privately connect to supported AWS services and VPC endpoint services powered by AWS PrivateLink. Amazon VPC instances do not require public IP addresses to communicate with resources of the service. Traffic between an Amazon VPC and a service does not leave the Amazon network. VPC endpoints are virtual devices.

* By default, VPC-attached compute resources such as EC2 instances, ECS containers, and Lambda functions typically access S3 and DynamoDB service endpoints via the Internet. This often requires a NAT gateway to provide a public IP address, which incurs data processing costs for each GB of traffic transferred.
* Gateway endpoints solve this problem by allowing resources with private IP addresses to communicate with DynamoDB/S3 without exposure to the public internet. They do not require any internet gateway, NAT device, or virtual private gateway in your VPC.

![](https://i.imgur.com/wjpWHwt.png)
![](https://i.imgur.com/yOk0MkJ.png)

There are two types of VPC endpoints: 
1. interface endpoints
2. gateway endpoints. 

##### Interface endpoints 
* Enable connectivity to services over AWS PrivateLink. These services include some AWS managed services, services hosted by other AWS customers and partners in their own Amazon VPCs (referred to as endpoint services), and supported AWS Marketplace partner services. 

##### Gateway endpoints
* Target specific IP routes in an Amazon VPC route table, in the form of a prefix-list, used for traffic destined to Amazon DynamoDB or Amazon Simple Storage Service (Amazon S3). Gateway endpoints do not enable AWS PrivateLink

#### Site-to-Site VPN and Direct Connect 
They are two ways to establish a secure connection between an organization’s on-premises infrastructure and its resources in the cloud.

* Site-to-Site VPN is a type of Virtual Private Network that is used to establish a secure connection among all the branches of an organization located worldwide. It creates an encrypted tunnel over the public internet to connect the organization’s on-premises infrastructure with its resources in the cloud.
* On the other hand, Direct Connect is a dedicated connection that connects an organization to its on-premises or data centers securely. It provides a high-speed, low-latency connection that bypasses the public internet and helps reduce network unpredictability and congestion. The speed of Direct Connect is usually higher than VPN connections.

In summary, Site-to-Site VPN uses the public internet to create an encrypted tunnel, while Direct Connect provides a dedicated connection that bypasses the public internet. The choice between Site-to-Site VPN and Direct Connect depends on your requirements.

### Three Tier Architecture 

A 3-tier architecture is a software architecture pattern where the application is broken down into three logical tiers: the presentation layer, the business logic layer, and the data storage layer. This architecture is used in a client-server application such as a web application that has the frontend, the backend, and the database. Each of these layers or tiers does a specific task and can be managed independently of each other.

In AWS, you can design and build a 3-tier cloud infrastructure using services such as Elastic Compute Cloud (EC2), Auto Scaling Group, Virtual Private Cloud (VPC), Elastic Load Balancer (ELB), Security Groups, and Internet Gateway. The infrastructure can be designed to be highly available and fault-tolerant.

![](https://i.imgur.com/prLIEd1.png)

### LAMP Stack on EC2
* Linux: OS for EC2 instance
* Apache: Web server that runs Linux (EC2)
* MySQL: RDS DB
* PHP: Application logic (running on EC2)
* Elasticache: Can add Redis/ Memcached for caching
* EBS: To store data locally & software

### Wordpress on AWS
* Wordpress is a free and open-source content management system (CMS) that is used to create websites and blogs.
* Simply put, there is two tiers: Application tier and Load balancer tier
* For example, users send images to the EC2 instances through the load balancers
* EFS can be used to access the files between different EC2 instances
![](https://i.imgur.com/6jQQ2Cx.png)